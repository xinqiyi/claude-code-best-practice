# Claude Agent SDK vs Claude CLI: System Prompts 与输出一致性

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![SDK vs CLI System Prompts 图示](assets/sdk-vs-cli-diagram.svg)

---

## Executive Summary

当通过 **Claude Agent SDK** 与 **Claude CLI（Claude Code）** 发送相同的消息（例如 "What is the capital of Norway?"）时，伴随这些消息的 system prompt 是根本不同的。CLI 使用**模块化 system prompt 架构**（约 269 token 基础 prompt，附加按功能条件加载的 context），而 SDK 默认使用最小化 prompt。**两者之间不保证输出相同**，即使配置匹配，因为缺少 seed 参数且 Claude 架构存在固有的非确定性。

---

## 1. System Prompt 对比

### Claude CLI（Claude Code）

Claude CLI 使用**模块化 system prompt 架构**，基础 prompt 约 269 token，附加条件加载的 context：

| Component | Description | Loading |
|-----------|-------------|---------|
| **Base System Prompt** | 核心指令和行为 | 始终（~269 tokens） |
| **Tool Instructions** | 18+ 内置 tools（Write, Read, Edit, Bash, TodoWrite 等） | 始终 |
| **Coding Guidelines** | 代码风格、格式化规则、安全实践 | 始终 |
| **Safety Rules** | 拒绝规则、注入防御、危害预防 | 始终 |
| **Response Style** | 语气、详细程度、解释深度、emoji 使用 | 始终 |
| **Environment Context** | 工作目录、git status、平台信息 | 始终 |
| **Project Context** | CLAUDE.md 内容、settings、hooks 配置 | 条件加载 |
| **Subagent Prompts** | Plan mode、Explore agent、Task agent | 条件加载 |
| **Security Review** | 扩展安全指令（~2,610 tokens） | 条件加载 |

**关键特征：**
- **模块化架构**，110+ 个 system prompt 字符串条件加载
- 基础 prompt 适中（~269 tokens），总大小随激活的功能变化
- 包含广泛的安全和注入防御层
- 自动加载工作目录中的 CLAUDE.md 文件
- 交互模式下会话持久化 context

### Claude Agent SDK

Agent SDK 默认使用**最小化 system prompt**，包含：

| Component | Description | Token Impact |
|-----------|-------------|--------------|
| **Essential Tool Instructions** | 仅显式提供的 tools | 最小 |
| **Basic Safety** | 最小化安全指令 | 最小 |

**关键特征：**
- 默认没有编码指南或风格偏好
- 没有项目 context，除非显式配置
- 没有扩展的 tool description
- 需要显式配置才能匹配 CLI 行为

---

## 2. 每种接口发送了什么

### 示例："What is the capital of Norway?"

#### 通过 Claude CLI

```
System Prompt: [模块化，~269+ base tokens]
├── Base system prompt (~269 tokens)
├── Tool instructions (Write, Read, Edit, Bash, Grep, Glob, etc.)
├── Git 安全协议
├── 代码引用指南
├── 专业性客观性指令
├── Security 和 injection defense 规则
├── Environment context (OS, directory, date)
├── CLAUDE.md content（如果存在）[条件加载]
├── MCP tool descriptions（如果配置）[条件加载]
├── Plan/Explore mode prompts [条件加载]
└── Session/conversation context

User Message: "What is the capital of Norway?"
```

#### 通过 Claude Agent SDK（默认）

```
System Prompt: [最小化]
├── Essential tool instructions（如果提供了任何 tools）
└── Basic operational context

User Message: "What is the capital of Norway?"
```

#### 通过 Agent SDK（使用 `claude_code` preset）

```typescript
const response = await query({
  prompt: "What is the capital of Norway?",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    }
  }
});
```

```
System Prompt: [模块化，匹配 CLI]
├── Full Claude Code system prompt
├── Tool instructions
├── Coding guidelines
└── Safety rules

// 注意：仍不包含 CLAUDE.md，除非配置了 settingSources
```

---

## 3. 自定义方法

### Claude CLI 自定义

| Method | Command | Effect |
|--------|---------|--------|
| **追加到 prompt** | `claude -p "..." --append-system-prompt "..."` | 添加指令同时保留默认值 |
| **替换 prompt** | `claude -p "..." --system-prompt "..."` | 完全替换 system prompt |
| **Project context** | CLAUDE.md 文件 | 自动加载，持久化 |
| **Output styles** | `/output-style [name]` | 应用预定义的响应风格 |

### Agent SDK 自定义

| Method | Configuration | Effect |
|--------|---------------|--------|
| **Custom prompt** | `systemPrompt: "..."` | 完全替换默认值（丢失 tools） |
| **Preset with append** | `systemPrompt: { type: "preset", preset: "claude_code", append: "..." }` | 保留 CLI 功能 + 自定义指令 |
| **CLAUDE.md loading** | `settingSources: ["project"]` | 加载项目级指令 |
| **Output styles** | `settingSources: ["user"]` 或 `settingSources: ["project"]` | 加载保存的输出风格 |

### 配置对比表

| Feature | CLI Default | SDK Default | SDK with Preset |
|---------|-------------|-------------|-----------------|
| Tool instructions | ✅ Full | ❌ Minimal | ✅ Full |
| Coding guidelines | ✅ Yes | ❌ No | ✅ Yes |
| Safety rules | ✅ Yes | ❌ Basic | ✅ Yes |
| CLAUDE.md auto-load | ✅ Yes | ❌ No | ❌ No* |
| Project context | ✅ Automatic | ❌ No | ❌ No* |

*需要显式 `settingSources: ["project"]` 配置

---

## 4. 输出一致性保证

### 关键发现：不保证确定性

**Claude Messages API 不提供用于可重复性的 seed 参数。** 这是一个根本的架构限制。

### 阻碍相同输出的因素

| Factor | Description | 可控？ |
|--------|-------------|---------------|
| **Different system prompts** | CLI vs SDK 有不同的默认值 | ✅ Yes（通过配置） |
| **Floating-point arithmetic** | 并行硬件特性 | ❌ No |
| **MoE routing** | Mixture-of-Experts 架构变化 | ❌ No |
| **Batching/scheduling** | 云基础设施差异 | ❌ No |
| **Numeric precision** | Inference engine 变化 | ❌ No |
| **Model snapshots** | 版本更新/更改 | ❌ No |

### Temperature 和 Sampling

即使 `temperature=0.0`（greedy decoding）：
- 不保证完全确定性
- 由于基础设施因素，仍可能出现微小变化
- 已知 bug：[Claude CLI produces non-deterministic output for identical inputs](https://github.com/anthropics/claude-code/issues/3370)

---

## 5. 实现最大一致性

要使 SDK 和 CLI 之间的输出**尽可能接近**相同：

### Agent SDK 配置

```typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

// 选项 1：使用 claude_code preset
const response = await client.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1024,
  // 尽可能匹配 CLI system prompt
  system: "Your exact system prompt matching CLI",
  messages: [
    { role: "user", content: "What is the capital of Norway?" }
  ],
  // 使用 greedy decoding 以获得最大一致性
  temperature: 0
});

// 选项 2：使用 Agent SDK query function
import { query } from "@anthropic-ai/agent-sdk";

for await (const message of query({
  prompt: "What is the capital of Norway?",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    },
    temperature: 0,
    model: "claude-sonnet-4-20250514",
    // 像 CLI 一样加载项目 context
    settingSources: ["project"]
  }
})) {
  // 处理响应
}
```

### CLI 配置

```bash
# 尽可能匹配 SDK 配置
claude -p "What is the capital of Norway?" \
  --model claude-sonnet-4-20250514 \
  --temperature 0
```

### 仍然不保证

即使配置完全匹配：
- 输出可能在运行之间不同
- 输出可能在 SDK 和 CLI 之间不同
- 不存在 seed 参数来强制可重复性

---

## 6. 实际影响

### 何时使用每种接口

| Use Case | 推荐接口 | 原因 |
|----------|----------------------|--------|
| 交互式开发 | Claude CLI | 完整工具套件、项目 context |
| 编程式集成 | Agent SDK | 细粒度控制、嵌入 |
| 一致的 API 响应 | Agent SDK + custom prompt | 对 system prompt 更多控制 |
| Batch processing | Agent SDK | 更适合自动化 pipeline |
| 一次性任务 | Claude CLI | 更快的设置、即时 context |

### 设计建议

1. **不要依赖 bit-perfect 可重复性**
   - 构建对微小输出变化具有鲁棒性的应用程序
   - 使用 structured outputs 和验证

2. **对于需要一致性的生产 pipeline：**
   - 在可能时缓存结果
   - 使用带有 JSON schema validation 的 structured outputs
   - 结合确定性逻辑和验证
   - 考虑使用共识的多轮生成

3. **在 SDK 中匹配 CLI 行为：**
   ```typescript
   systemPrompt: {
     type: "preset",
     preset: "claude_code",
     append: "Your additional instructions"
   },
   settingSources: ["project", "user"]
   ```

---

## 7. System Prompt Token 影响

| Configuration | Architecture | Notes |
|---------------|-------------|-------|
| SDK (minimal) | 最小化默认值 | 仅基本 tool instructions |
| SDK (claude_code preset) | 模块化（~269+ base） | 匹配 CLI，按功能变化 |
| CLI (default) | 模块化（~269+ base） | 按条件加载附加 context |
| CLI (with MCP tools) | 模块化 + MCP | MCP tool descriptions 增加大量 token |

**注意：** Claude Code 使用模块化架构，包含 110+ 个 system prompt 字符串。基础 prompt 约 269 tokens，单个组件根据激活的功能从 18 到 2,610 tokens 不等。

**影响：** SDK 的最小化默认为你的实际任务提供了更多 context，但代价是失去 Claude Code 的全部能力。

---

## 8. 总结表

| Aspect | Claude CLI | Agent SDK (Default) | Agent SDK (Preset) |
|--------|------------|--------------------|--------------------|
| **System prompt** | 模块化（~269+ base） | 最小化 | 模块化（匹配 CLI） |
| **Tools included** | 18+ 内置 | 仅在提供时 | 18+ 内置 |
| **CLAUDE.md auto-load** | Yes | No | No（需要配置） |
| **Coding guidelines** | Yes | No | Yes |
| **Safety rules** | Full | Basic | Full |
| **Temperature control** | Yes | Yes | Yes |
| **Determinism guarantee** | No | No | No |
| **Identical outputs?** | 不适用 | No（相对于 CLI） | 更接近，但仍 No |

---

## 9. 结论

**问：SDK 与 CLI 中相同消息伴随什么 system prompt？**

CLI 使用**模块化 system prompt 架构**，包含约 269 token 的基础 prompt 和 110+ 个条件加载的组件（tool instructions、coding guidelines、safety rules、project context）。SDK 使用**最小化默认值**，仅包含基本 tool instructions，但可以通过 `claude_code` preset 配置为匹配 CLI 行为。

**问：是否保证输出相同？**

**不保证。** 即使 system prompt 匹配、输入相同且 `temperature=0`，由于以下原因也不保证输出相同：
- Claude API 中缺少 seed 参数
- Floating-point arithmetic 变化
- Infrastructure 级别的非确定性
- 模型架构（Mixture-of-Experts）routing 变化

**建议：** 将系统设计为对输出变化具有鲁棒性，而非依赖确定性行为。对于一致性关键的应用，使用 structured outputs、缓存和验证层。

---

## Sources

- [Modifying System Prompts - Agent SDK](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/sdk#modifying-system-prompts)
- [Claude Code CLI Reference](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/cli)
- [Claude Code Headless Mode](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/headless)
- [Claude Code Best Practices - Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Claude Messages API Reference](https://docs.anthropic.com/en/api/messages)
- [GitHub Issue #3370: Non-deterministic output](https://github.com/anthropics/claude-code/issues/3370)
- [Claude Code System Prompts Repository](https://github.com/Piebald-AI/claude-code-system-prompts) - 模块化 prompt 架构分析
- [Why Deterministic Output from LLMs is Nearly Impossible](https://unstract.com/blog/understanding-why-deterministic-output-from-llms-is-nearly-impossible/)

---

*本报告由 Claude Code 使用 Opus 4.5 模型于 2026 年 2 月 3 日生成。*
