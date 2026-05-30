# Claude 高级工具使用模式

API 级别功能（现已正式发布），旨在减少 Token 消耗、降低延迟并提高工具使用准确性。随 Opus/Sonnet 4.6 发布。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#overview)
2. [编程式工具调用 (PTC)](#programmatic-tool-calling-ptc)
3. [Web 搜索/抓取的动态过滤](#dynamic-filtering-for-web-searchfetch)
4. [工具搜索工具](#tool-search-tool)
5. [工具使用示例](#tool-use-examples)
6. [与 Claude Code 的关联](#claude-code-relevance)

---

## 概述

| 功能 | 解决的问题 | Token 节省 | 可用性 |
|---------|---------------|---------------|--------------|
| 编程式工具调用 | 多步骤 agent 循环在往返通信中消耗大量 Token | ~37% 减少 | API, Foundry（正式发布） |
| 动态过滤 | Web 搜索/抓取结果用无关内容膨胀上下文 | ~24% 更少的输入 Token | API, Foundry（正式发布） |
| 工具搜索工具 | 过多工具定义导致上下文膨胀 | ~85% 减少 | API, Foundry（正式发布） |
| 工具使用示例 | 仅靠 Schema 无法表达使用模式 | 72% → 90% 准确率 | API, Foundry（正式发布） |

所有功能自 **2026 年 2 月 18 日**起**全面可用**。

**策略性分层** — 从最大的瓶颈开始：
- 工具定义导致的上下文膨胀 → 工具搜索工具
- 大量中间结果 → 编程式工具调用
- Web 搜索噪音 → 动态过滤
- 参数错误 → 工具使用示例

---

## 编程式工具调用 (PTC)

<img src="assets/programmatic-tool-calling-diagram.svg" alt="PTC 示意图 — 传统方式 vs 编程式工具调用" width="100%" />

### 范式转变

**之前（传统工具调用）：**
```
用户提示 → Claude → 工具调用 1 → 响应 1 → Claude → 工具调用 2 → 响应 2 → Claude → 工具调用 3 → 响应 3 → Claude → 最终答案
```
每次工具调用都需要一次完整的模型往返。3 个工具 = 3 次推理。

**之后（编程式工具调用）：**
```
用户提示 → Claude → 编写 Python 脚本 → 脚本内部调用工具 1、工具 2、工具 3 → stdout → Claude → 最终答案
```
Claude 编写代码来编排所有工具。只有最终的 `stdout` 进入上下文窗口。3 个工具 = 1 次推理。

### 工作原理

1. 你使用 `allowed_callers: ["code_execution_20250825"]` 定义工具
2. Claude 编写 Python 代码，在沙箱中将这些工具作为异步函数调用
3. 当工具函数被调用时，沙箱暂停并返回一个 `tool_use` 块
4. 你提供工具结果 — 结果会发送给**正在运行的代码**，而非 Claude 的上下文
5. 代码继续执行，处理结果，必要时调用更多工具
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
      "description": "执行 SQL 查询。返回以 JSON 对象形式表示的行，字段包括：id (str)、name (str)、revenue (float)。",
      "input_schema": {
        "type": "object",
        "properties": {
          "sql": { "type": "string", "description": "要执行的 SQL 查询" }
        },
        "required": ["sql"]
      },
      "allowed_callers": ["code_execution_20250825"]
    }
  ]
}
```

### `allowed_callers` 字段

| 值 | 行为 |
|-------|----------|
| `["direct"]` | 仅传统工具调用（省略时的默认值） |
| `["code_execution_20250825"]` | 仅能从 Python 沙箱调用 |
| `["direct", "code_execution_20250825"]` | 两种模式均可 |

**建议：** 每个工具选择一种模式，不要同时选择两种。这能为 Claude 提供更清晰的指导。

### 响应中的 `caller` 字段

每个 tool use 块都包含一个 `caller` 字段，用于说明调用方式：

```json
// 直接调用（传统方式）
{ "caller": { "type": "direct" } }

// 编程式调用（从代码执行中）
{ "caller": { "type": "code_execution_20250825", "tool_id": "srvtoolu_abc123" } }
```

### 高级模式

**批量处理** — 在 1 次推理中处理 N 个项目：
```python
regions = ["West", "East", "Central", "North", "South"]
results = {}
for region in regions:
    data = await query_database(f"SELECT SUM(revenue) FROM sales WHERE region='{region}'")
    results[region] = data[0]["revenue"]

top = max(results.items(), key=lambda x: x[1])
print(f"Top region: {top[0]} with ${top[1]:,}")
```

**提前终止** — 一旦满足成功条件即停止：
```python
endpoints = ["us-east", "eu-west", "apac"]
for endpoint in endpoints:
    status = await check_health(endpoint)
    if status == "healthy":
        print(f"Found healthy endpoint: {endpoint}")
        break
```

**条件性工具选择：**
```python
file_info = await get_file_info(path)
if file_info["size"] < 10000:
    content = await read_full_file(path)
else:
    content = await read_file_summary(path)
print(content)
```

**数据过滤** — 减少 Claude 看到的内容：
```python
logs = await fetch_logs(server_id)
errors = [log for log in logs if "ERROR" in log]
print(f"Found {len(errors)} errors")
for error in errors[-10:]:
    print(error)
```

### 模型兼容性

| 模型 | 是否支持 |
|-------|-----------|
| Claude Opus 4.6 | 是 |
| Claude Sonnet 4.6 | 是 |
| Claude Sonnet 4.5 | 是 |
| Claude Opus 4.5 | 是 |

### 限制条件

| 约束条件 | 详情 |
|-----------|--------|
| **不支持 Bedrock/Vertex** | 仅 API 和 Foundry |
| **不支持 MCP 工具** | MCP 连接器工具不能以编程方式调用 |
| **不支持 Web 搜索/抓取** | Web 工具在 PTC 中不受支持 |
| **不支持结构化输出** | 与 `strict: true` 工具不兼容 |
| **不支持强制工具选择** | `tool_choice` 不能强制使用 PTC |
| **容器生命周期** | 约 4.5 分钟过期 |
| **ZDR** | 不包含在零数据保留范围内 |
| **工具结果为字符串** | 验证外部结果以防止代码注入风险 |

### 何时使用 PTC

| 适合的使用场景 | 不太理想的场景 |
|----------------|------------|
| 处理需要聚合计算的大型数据集 | 返回简单响应的单次工具调用 |
| 3 个以上依赖关系的连续工具调用 | 需要即时用户反馈的工具 |
| 在 Claude 看到结果之前进行过滤/转换 | 非常快速的操作（开销大于收益） |
| 跨多个项目的并行操作 | |
| 基于中间结果的条件逻辑 | |

### Token 效率

- 编程式调用的工具结果**不会添加到 Claude 的上下文中** — 只有最终 `stdout`
- 中间处理在代码中完成，而非模型 Token
- 以编程方式调用 10 个工具 ≈ 10 次直接调用的 1/10 Token 消耗

---

## Web 搜索/抓取的动态过滤

### 问题

Web 搜索和抓取工具将完整的 HTML 页面倾倒进 Claude 的上下文窗口。其中大部分内容都是无关的 — 导航、广告、模板内容。Claude 随后需要对所有这些内容进行推理，浪费 Token 并降低准确性。

### 解决方案

Claude 现在**编写并执行 Python 代码来过滤 Web 结果**，然后再将其送入上下文窗口。Claude 不再对原始 HTML 进行推理，而是在沙箱中过滤、解析并仅提取相关内容。

### 工作原理

**之前：**
```
查询 → 搜索结果 → 抓取完整 HTML × N 页 → 所有内容进入上下文 → Claude 对所有内容进行推理
```

**之后：**
```
查询 → 搜索结果 → Claude 编写过滤代码 → 代码仅提取相关内容 → 过滤后的结果进入上下文
```

### API 配置

使用更新后的工具类型版本，并附带 beta 请求头：

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

**所需请求头：** `anthropic-beta: code-execution-web-tools-2026-02-09`

**默认启用** — 在使用新工具类型版本且搭配 Sonnet 4.6 和 Opus 4.6 时。

### 基准测试结果

**BrowseComp**（在网站上查找特定信息）：

| 模型 | 无过滤 | 有过滤 | 提升幅度 |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 33.3% | **46.6%** | +13.3 个百分点 |
| Opus 4.6 | 45.3% | **61.6%** | +16.3 个百分点 |

**DeepsearchQA**（多步骤研究，F1 分数）：

| 模型 | 无过滤 | 有过滤 | 提升幅度 |
|-------|-------------------|----------------|-------------|
| Sonnet 4.6 | 52.6% | **59.4%** | +6.8 个百分点 |
| Opus 4.6 | 69.8% | **77.3%** | +7.5 个百分点 |

**Token 效率：** 平均减少 24% 的输入 Token。Sonnet 4.6 成本降低；Opus 4.6 由于更复杂的过滤代码可能略有增加。

### 使用场景

- 筛选技术文档
- 跨多个来源验证引用
- 交叉引用搜索结果
- 多步骤研究查询
- 在大型页面中查找埋藏较深的特定数据点

---

## 工具搜索工具

### 问题

预先加载所有工具定义会浪费上下文。如果你有 50 个 MCP 工具，每个约 1.5K Token，那么在用户提问之前就已经消耗了 75K Token。

### 解决方案

将不常用的工具标记为 `defer_loading: true`。它们被排除在初始上下文之外。Claude 通过工具搜索工具按需发现它们。

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

- 保持 3-5 个最常用的工具始终加载，其余延迟加载
- 编写清晰、描述性的工具名称和描述（搜索依赖它们）
- 在系统提示中记录可用的能力

### 何时使用

- 工具定义消耗超过 10K Token
- 有 10 个以上工具可用
- 多个 MCP 服务器
- 因选项过多导致工具选择准确性下降

### Token 节省

工具定义 Token 减少约 85%（在 Anthropic 的基准测试中，从 77K 降至 8.7K）。

### Claude Code 等效实现

Claude Code 内置了 **MCP 工具搜索自动模式**（自 v2.1.7 起默认启用）。当 MCP 工具描述超过上下文的 10% 时，它们会被延迟加载并通过 `MCPSearch` 发现。使用 `ENABLE_TOOL_SEARCH=auto:N` 配置阈值，其中 N 是上下文百分比（0-100）。

---

## 工具使用示例

### 问题

JSON schema 定义了结构，但无法表达：
- 何时包含可选参数
- 哪些参数组合有意义
- 格式约定（日期格式、ID 模式）
- 嵌套结构的使用方式

### 解决方案

在工具定义中添加 `input_examples` — 超出 schema 范围的具体使用模式。

### 配置

```json
{
  "name": "create_ticket",
  "description": "创建支持工单",
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

- 使用**真实数据**，而非像 "example_value" 这样的占位符字符串
- 展示**多样性**：最小化、部分和完整规格
- 保持简洁：**每个工具 1-5 个示例**
- 专注于消除歧义 — 以行为清晰为目标，而非 schema 完整性
- 展示参数关联（例如，`priority: "critical"` 通常带有 `assignee`）

### 结果

在 Anthropic 的基准测试中，复杂参数处理的准确率从 72% 提升至 90%。

---

## 与 Claude Code 的关联

### 直接适用于 Claude Code 用户的功能

| 功能 | Claude Code 状态 | 操作建议 |
|---------|-------------------|--------|
| 工具搜索 | 自 v2.1.7 起内置为 MCPSearch 自动模式 | 如果你有大量 MCP 工具，调整 `ENABLE_TOOL_SEARCH=auto:N` |
| 动态过滤 | CLI 中不可用（API 级别的 Web 工具） | 适用于进行 Web 研究的 Agent SDK 用户 |
| PTC | CLI 中不可用 | 适用于构建自定义 agent 的 Agent SDK 用户 |
| 工具使用示例 | CLI 中不可配置 | 适用于自定义 MCP 服务器开发者 |

### 面向 Agent SDK 开发者

如果你正在使用 `@anthropic-ai/claude-agent-sdk` 构建 agent，PTC 可以立即投入使用：

1. 将 `code_execution_20250825` 添加到你的 tools 数组中
2. 在受益于批处理/过滤的工具上设置 `allowed_callers`
3. 实现工具结果循环（暂停 → 提供结果 → 继续）
4. 从工具返回结构化数据（JSON）以便于编程式解析

### 面向 MCP 服务器开发者

如果你正在构建自定义 MCP 服务器，工具使用示例可以改善 Claude 使用你的工具的方式：
- 在工具 schema 中添加 `input_examples`
- 在描述中清楚地记录返回格式（PTC 需要解析它们）

---

## 来源

- [Anthropic Engineering: Advanced Tool Use](https://www.anthropic.com/engineering/advanced-tool-use)
- [Programmatic Tool Calling Documentation](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)
- [Code Execution Tool Documentation](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)
- [Improved Web Search with Dynamic Filtering](https://claude.com/blog/improved-web-search-with-dynamic-filtering)
