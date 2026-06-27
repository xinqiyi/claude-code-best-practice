# Claude 高级 Tool Use 模式

API 层面的功能（现已 GA），可减少 token 消耗、降低延迟并提高工具准确性。随 Opus/Sonnet 4.6 发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Table of Contents

1. [Overview](#overview)
2. [Programmatic Tool Calling (PTC)](#programmatic-tool-calling-ptc)
3. [Dynamic Filtering for Web Search/Fetch](#dynamic-filtering-for-web-searchfetch)
4. [Tool Search Tool](#tool-search-tool)
5. [Tool Use Examples](#tool-use-examples)
6. [Claude Code Relevance](#claude-code-relevance)

---

## Overview

| Feature | Problem Solved | Token Savings | Availability |
|---------|---------------|---------------|--------------|
| Programmatic Tool Calling | 多步 agent 循环在往返中消耗 token | ~37% 减少 | API, Foundry (GA) |
| Dynamic Filtering | Web search/fetch 结果将不相关内容膨胀到 context 中 | ~24% 输入 token 减少 | API, Foundry (GA) |
| Tool Search Tool | 过多的 tool definition 膨胀 context | ~85% 减少 | API, Foundry (GA) |
| Tool Use Examples | 仅靠 schema 无法表达使用模式 | 72% → 90% 准确率 | API, Foundry (GA) |

所有功能自 2026 年 2 月 18 日起 **正式可用（generally available）**。

**策略分层** — 从最大的瓶颈开始：
- Context 因 tool definition 膨胀 → Tool Search Tool
- 中间结果过大 → Programmatic Tool Calling
- Web search 噪声 → Dynamic Filtering
- 参数错误 → Tool Use Examples

---

## Programmatic Tool Calling (PTC)

<img src="assets/programmatic-tool-calling-diagram.svg" alt="PTC Diagram — 传统 vs 编程式 Tool Calling" width="100%" />

### 范式转变

**之前（传统 Tool Calling）：**
```
User prompt → Claude → Tool call 1 → Response 1 → Claude → Tool call 2 → Response 2 → Claude → Tool call 3 → Response 3 → Claude → Final answer
```
每次 tool call 都需要一次完整的模型往返。3 个 tools = 3 次 inference 传递。

**之后（Programmatic Tool Calling）：**
```
User prompt → Claude → 编写 Python script → Script 内部调用 Tool 1, Tool 2, Tool 3 → stdout → Claude → Final answer
```
Claude 编写代码来编排所有 tools。只有最终的 `stdout` 进入 context window。3 个 tools = 1 次 inference 传递。

### 工作原理

1. 你使用 `allowed_callers: ["code_execution_20250825"]` 定义 tools
2. Claude 编写 Python 代码，在 sandbox 内以 async function 方式调用这些 tools
3. 当 tool function 被调用时，sandbox 暂停，API 返回一个 `tool_use` block
4. 你提供 tool result — 它进入**正在运行的代码**，而非 Claude 的 context
5. 代码恢复执行，处理结果，根据需要调用更多 tools
6. 只有最终执行的 `stdout` 到达 Claude

### 关键配置

```json
{
  "tools": [
    {
      "type": "code_execution_20250825",
      "name": "code_execution"
    },
    {
      "name": "query_database",
      "description": "Execute a SQL query. Returns rows as JSON objects with fields: id (str), name (str), revenue (float).",
      "input_schema": {
        "type": "object",
        "properties": {
          "sql": { "type": "string", "description": "SQL query to execute" }
        },
        "required": ["sql"]
      },
      "allowed_callers": ["code_execution_20250825"]
    }
  ]
}
```

### `allowed_callers` 字段

| Value | Behavior |
|-------|----------|
| `["direct"]` | 仅传统 tool calling（省略时的默认值） |
| `["code_execution_20250825"]` | 仅可从 Python sandbox 调用 |
| `["direct", "code_execution_20250825"]` | 两种模式均可 |

**建议：** 每个 tool 选择一种模式，而非两者兼用。这为 Claude 提供更清晰的指导。

### 响应中的 `caller` 字段

每个 tool use block 都包含一个 `caller` 字段，以便了解其调用方式：

```json
// Direct（传统方式）
{ "caller": { "type": "direct" } }

// Programmatic（来自 code execution）
{ "caller": { "type": "code_execution_20250825", "tool_id": "srvtoolu_abc123" } }
```

### 高级模式

**Batch processing** — 在 1 次 inference 传递中处理 N 个项目：
```python
regions = ["West", "East", "Central", "North", "South"]
results = {}
for region in regions:
    data = await query_database(f"SELECT SUM(revenue) FROM sales WHERE region='{region}'")
    results[region] = data[0]["revenue"]

top = max(results.items(), key=lambda x: x[1])
print(f"Top region: {top[0]} with ${top[1]:,}")
```

**Early termination** — 一旦满足成功条件即停止：
```python
endpoints = ["us-east", "eu-west", "apac"]
for endpoint in endpoints:
    status = await check_health(endpoint)
    if status == "healthy":
        print(f"Found healthy endpoint: {endpoint}")
        break
```

**Conditional tool selection：**
```python
file_info = await get_file_info(path)
if file_info["size"] < 10000:
    content = await read_full_file(path)
else:
    content = await read_file_summary(path)
print(content)
```

**Data filtering** — 减少 Claude 看到的内容：
```python
logs = await fetch_logs(server_id)
errors = [log for log in logs if "ERROR" in log]
print(f"Found {len(errors)} errors")
for error in errors[-10:]:
    print(error)
```

### 模型兼容性

| Model | Supported |
|-------|-----------|
| Claude Opus 4.6 | Yes |
| Claude Sonnet 4.6 | Yes |
| Claude Sonnet 4.5 | Yes |
| Claude Opus 4.5 | Yes |

### 约束

| Constraint | Detail |
|-----------|--------|
| **不支持 Bedrock/Vertex** | 仅限 API 和 Foundry |
| **不支持 MCP tools** | MCP connector tools 不能被编程式调用 |
| **不支持 web search/fetch** | Web tools 不支持 PTC |
| **不支持 structured outputs** | `strict: true` tools 不兼容 |
| **不支持 forced tool choice** | `tool_choice` 无法强制 PTC |
| **Container lifetime** | 约 4.5 分钟后过期 |
| **ZDR** | 不在 Zero Data Retention 覆盖范围内 |
| **Tool results 为 strings** | 验证外部结果以防止代码注入风险 |

### 何时使用 PTC

| Good Use Cases | Less Ideal |
|----------------|------------|
| 处理需要聚合的大数据集 | 响应简单的单次 tool call |
| 3+ 个顺序依赖的 tool call | 需要即时用户反馈的 tools |
| 在 Claude 看到结果前过滤/转换 | 极快操作（开销 > 收益） |
| 跨多个项目的并行操作 | |
| 基于中间结果的条件逻辑 | |

### Token 效率

- 编程式调用的 tool results **不会添加到 Claude 的 context** 中 — 只有最终 `stdout`
- 中间处理发生在代码中，而非模型 token 中
- 10 个编程式 tools ≈ 10 次直接调用的 1/10 的 token

---

## Dynamic Filtering for Web Search/Fetch

### 问题

Web search 和 fetch tools 会将完整的 HTML 页面转储到 Claude 的 context window 中。其中大部分内容无关 — 导航、广告、样板文本。Claude 随后对其进行推理，浪费 token 并降低准确性。

### 解决方案

Claude 现在**编写并执行 Python 代码来过滤 web 结果**，在它们进入 context window 之前进行处理。Claude 不再直接推理原始 HTML，而是在 sandbox 中过滤、解析并仅提取相关内容。

### 工作原理

**之前：**
```
Query → Search results → Fetch full HTML × N pages → 所有内容进入 context → Claude 对所有内容推理
```

**之后：**
```
Query → Search results → Claude 编写过滤代码 → 代码仅提取相关内容 → 过滤后的结果进入 context
```

### API 配置

使用带有 beta header 的更新版 tool type 版本：

```json
{
  "model": "claude-opus-4-6",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20260209",
      "name": "web_search"
    },
    {
      "type": "web_fetch_20260209",
      "name": "web_fetch"
    }
  ]
}
```

**需要 header：** `anthropic-beta: code-execution-web-tools-2026-02-09`

**默认启用**，当与 Sonnet 4.6 和 Opus 4.6 一起使用新版 tool type 版本时。

### Benchmark 结果

**BrowseComp**（在网站上查找特定信息）：

| Model | Without Filtering | With Filtering | Improvement |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 33.3% | **46.6%** | +13.3 pp |
| Opus 4.6 | 45.3% | **61.6%** | +16.3 pp |

**DeepsearchQA**（多步研究，F1 score）：

| Model | Without Filtering | With Filtering | Improvement |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 52.6% | **59.4%** | +6.8 pp |
| Opus 4.6 | 69.8% | **77.3%** | +7.5 pp |

**Token 效率：** 平均减少 24% 的输入 token。Sonnet 4.6 成本降低；Opus 4.6 可能因更复杂的过滤代码而略有增加。

### Use Cases

- 筛选技术文档
- 跨多个来源验证引用
- 交叉引用搜索结果
- 多步研究查询
- 在大页面中找到特定的数据点

---

## Tool Search Tool

### 问题

在 context 中加载所有 tool definition 会浪费 context。如果你有 50 个 MCP tools，每个约 1.5K token，那就是 75K token，在用户提问之前就已消耗。

### 解决方案

将不常用的 tools 标记为 `defer_loading: true`。它们在初始 context 中被排除。Claude 通过 Tool Search Tool 按需发现它们。

### 配置

```json
{
  "tools": [
    {
      "type": "mcp_toolset",
      "mcp_server_name": "google-drive",
      "default_config": { "defer_loading": true },
      "configs": {
        "search_files": { "defer_loading": false }
      }
    }
  ]
}
```

### 最佳实践

- 保持 3-5 个最常用的 tools 始终加载，其余延迟加载
- 编写清晰、描述性的 tool name 和 description（搜索依赖于它们）
- 在 system prompt 中记录可用的 capabilities

### 何时使用

- Tool definition 消耗超过 10K token
- 10+ 个 tools 可用
- 多个 MCP servers
- 因选项过多导致的 tool 选择准确性问题

### Token 节省

在 Anthropic 的 benchmarks 中，tool definition token 约减少 85%（77K → 8.7K）。

### Claude Code 对等实现

Claude Code 具有 **MCP tool search auto mode**（自 v2.1.7 起默认启用）。当 MCP tool description 超过 context 的 10% 时，它们会被延迟并通过 `MCPSearch` 发现。使用 `ENABLE_TOOL_SEARCH=auto:N` 配置阈值，其中 N 是 context 百分比（0-100）。

---

## Tool Use Examples

### 问题

JSON schemas 定义结构，但无法表达：
- 何时包含可选参数
- 哪些参数组合有意义
- 格式约定（日期格式、ID 模式）
- 嵌套结构使用

### 解决方案

在 tool definition 中添加 `input_examples` — schema 之外的具象使用模式。

### 配置

```json
{
  "name": "create_ticket",
  "description": "Create a support ticket",
  "input_schema": {
    "type": "object",
    "properties": {
      "title": { "type": "string" },
      "priority": { "type": "string", "enum": ["low", "medium", "high", "critical"] },
      "assignee": { "type": "string" },
      "labels": { "type": "array", "items": { "type": "string" } }
    },
    "required": ["title"]
  },
  "input_examples": [
    {
      "title": "Login page returns 500 error",
      "priority": "critical",
      "assignee": "oncall-team",
      "labels": ["bug", "auth", "production"]
    },
    {
      "title": "Add dark mode support",
      "priority": "low",
      "labels": ["feature-request", "ui"]
    },
    {
      "title": "Update API docs for v2 endpoints"
    }
  ]
}
```

### 最佳实践

- 使用**真实数据**，而非像 "example_value" 这样的占位字符串
- 展示**多样性**：最少、部分和完整规范的示例
- 保持简洁：**每个 tool 1-5 个示例**
- 聚焦于解决歧义 — 以行为清晰度为目标，而非 schema 完备性
- 展示参数关联（例如 `priority: "critical"` 往往会有 `assignee`）

### 结果

在 Anthropic 的 benchmarks 中，复杂参数处理的准确率从 72% 提升到 90%。

---

## Claude Code 相关性

### 直接适用于 Claude Code 用户的内容

| Feature | Claude Code Status | Action |
|---------|-------------------|--------|
| Tool Search | 自 v2.1.7 起内置，作为 MCPSearch auto mode | 如有大量 MCP tools，调整 `ENABLE_TOOL_SEARCH=auto:N` |
| Dynamic Filtering | CLI 中不可用（API 级别的 web tools） | 与做 web 研究的 Agent SDK 用户相关 |
| PTC | CLI 中不可用 | 与构建自定义 agents 的 Agent SDK 用户相关 |
| Tool Use Examples | CLI 中不可配置 | 与自定义 MCP server 作者相关 |

### 对 Agent SDK 开发者

如果你正在使用 `@anthropic-ai/claude-agent-sdk` 构建 agents，PTC 可立即使用：

1. 将 `code_execution_20250825` 添加到你的 tools 数组中
2. 在适合批处理/过滤的 tools 上设置 `allowed_callers`
3. 实现 tool result loop（暂停 → 提供 result → 恢复）
4. 从 tools 返回结构化数据（JSON），以便于编程式解析

### 对 MCP server 作者

如果你正在构建自定义 MCP servers，Tool Use Examples 可以改善 Claude 使用你的 tools 的方式：
- 在 tool schemas 中添加 `input_examples`
- 在 description 中清晰地记录返回格式（PTC 需要解析它们）

---

## Sources

- [Anthropic Engineering: Advanced Tool Use](https://www.anthropic.com/engineering/advanced-tool-use)
- [Programmatic Tool Calling Documentation](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)
- [Code Execution Tool Documentation](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)
- [Improved Web Search with Dynamic Filtering](https://claude.com/blog/improved-web-search-with-dynamic-filtering)
