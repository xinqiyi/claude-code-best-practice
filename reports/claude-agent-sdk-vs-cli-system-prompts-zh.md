# Claude Agent SDK 与 Claude CLI：系统提示与输出一致性

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![SDK vs CLI 系统提示对比图](assets/sdk-vs-cli-diagram.svg)

---

## 执行摘要

当通过 **Claude Agent SDK** 与 **Claude CLI (Claude Code)** 发送相同消息（例如，"挪威的首都是什么？"）时，伴随这些消息的系统提示存在根本性差异。CLI 使用**模块化系统提示架构**（约 269 基础 token，并根据功能有条件地加载额外上下文），而 SDK 默认使用最简提示。**即便配置匹配，两者之间也无法保证输出一致**，这是因为 Claude 架构中缺乏种子参数（seed parameter）且存在固有的非确定性。

---

## 1. 系统提示对比

### Claude CLI (Claude Code)

Claude CLI 使用**模块化系统提示架构**，包含约 269 token 的基础提示，并有条件地加载额外上下文：

| 组件 | 描述 | 加载方式 |
|-----------|-------------|---------|
| **基础系统提示** | 核心指令与行为 | 始终加载（约 269 tokens） |
| **工具指令** | 18+ 个内置工具（Write、Read、Edit、Bash、TodoWrite 等） | 始终加载 |
| **编码规范** | 代码风格、格式化规则、安全实践 | 始终加载 |
| **安全规则** | 拒绝规则、注入防御、危害防范 | 始终加载 |
| **回复风格** | 语气、详细程度、解释深度、emoji 使用 | 始终加载 |
| **环境上下文** | 工作目录、git 状态、平台信息 | 始终加载 |
| **项目上下文** | CLAUDE.md 内容、设置、hooks 配置 | 条件加载 |
| **子代理提示** | Plan 模式、Explore agent、Task agent | 条件加载 |
| **安全审查** | 扩展安全指令（约 2,610 tokens） | 条件加载 |

**主要特点：**
- **模块化架构**，包含 110+ 个按条件加载的系统提示字符串
- 基础提示较小（约 269 tokens），总量根据激活的功能而变化
- 包含广泛的安全和注入防御层
- 自动加载工作目录中的 CLAUDE.md 文件
- 交互模式下保持会话持久上下文

### Claude Agent SDK

Agent SDK 默认使用**最简系统提示**，包含：

| 组件 | 描述 | Token 影响 |
|-----------|-------------|--------------|
| **必要的工具指令** | 仅包含显式提供的工具 | 极小 |
| **基础安全** | 最少的安全指令 | 极小 |

**主要特点：**
- 默认不含编码规范或风格偏好
- 除非显式配置，否则不包含项目上下文
- 不包含大量的工具描述
- 需要显式配置才能匹配 CLI 行为

---

## 2. 各接口发送的内容

### 示例："挪威的首都是什么？"

#### 通过 Claude CLI

```
System Prompt: [模块化, 约 269+ 基础 tokens]
├── 基础系统提示（约 269 tokens）
├── 工具指令（Write、Read、Edit、Bash、Grep、Glob 等）
├── Git 安全协议
├── 代码引用规范
├── 专业客观性指令
├── 安全与注入防御规则
├── 环境上下文（操作系统、目录、日期）
├── CLAUDE.md 内容（若存在）[条件加载]
├── MCP 工具描述（若已配置）[条件加载]
├── Plan/Explore 模式提示 [条件加载]
└── 会话/对话上下文

User Message: "挪威的首都是什么？"
```

#### 通过 Claude Agent SDK（默认）

```
System Prompt: [最简]
├── 必要的工具指令（如果提供了任何工具）
└── 基本操作上下文

User Message: "挪威的首都是什么？"
```

#### 通过 Agent SDK（使用 `claude_code` 预设）

```typescript
const response = await query({
  prompt: "挪威的首都是什么？",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    }
  }
});
```

```
System Prompt: [模块化，与 CLI 一致]
├── 完整的 Claude Code 系统提示
├── 工具指令
├── 编码规范
└── 安全规则

// 注意：除非配置了 settingSources，否则仍不包含 CLAUDE.md
```

---

## 3. 自定义方法

### Claude CLI 自定义

| 方法 | 命令 | 效果 |
|--------|---------|--------|
| **追加提示** | `claude -p "..." --append-system-prompt "..."` | 在保留默认设置的同时添加指令 |
| **替换提示** | `claude -p "..." --system-prompt "..."` | 完全替换系统提示 |
| **项目上下文** | CLAUDE.md 文件 | 自动加载，持久生效 |
| **输出风格** | `/output-style [name]` | 应用预定义的回复风格 |

### Agent SDK 自定义

| 方法 | 配置 | 效果 |
|--------|---------------|--------|
| **自定义提示** | `systemPrompt: "..."` | 完全替换默认提示（会丢失工具配置） |
| **预设 + 追加** | `systemPrompt: { type: "preset", preset: "claude_code", append: "..." }` | 保留 CLI 功能 + 自定义指令 |
| **加载 CLAUDE.md** | `settingSources: ["project"]` | 加载项目级指令 |
| **输出风格** | `settingSources: ["user"]` 或 `settingSources: ["project"]` | 加载已保存的输出风格 |

### 配置对比表

| 功能 | CLI 默认 | SDK 默认 | SDK 带预设 |
|---------|-------------|-------------|-----------------|
| 工具指令 | ✅ 完整 | ❌ 最简 | ✅ 完整 |
| 编码规范 | ✅ 有 | ❌ 无 | ✅ 有 |
| 安全规则 | ✅ 完整 | ❌ 基础 | ✅ 完整 |
| CLAUDE.md 自动加载 | ✅ 是 | ❌ 否 | ❌ 否* |
| 项目上下文 | ✅ 自动 | ❌ 否 | ❌ 否* |

*需要显式配置 `settingSources: ["project"]`

---

## 4. 输出一致性保证

### 关键发现：不保证确定性

**Claude Messages API 不提供用于可重现性的种子参数（seed parameter）。** 这是一个根本性的架构限制。

### 导致输出无法完全一致的因素

| 因素 | 描述 | 是否可控？ |
|--------|-------------|---------------|
| **系统提示不同** | CLI 与 SDK 有不同的默认设置 | ✅ 是（通过配置） |
| **浮点数运算** | 并行硬件的微差异 | ❌ 否 |
| **MoE 路由** | 混合专家（Mixture-of-Experts）架构差异 | ❌ 否 |
| **批处理/调度** | 云基础设施差异 | ❌ 否 |
| **数值精度** | 推理引擎差异 | ❌ 否 |
| **模型快照** | 版本更新/变更 | ❌ 否 |

### 温度与采样

即便设置 `temperature=0.0`（贪心解码）：
- **不保证**完全确定性
- 由于基础设施因素，仍可能出现微小差异
- 已知缺陷：[Claude CLI 对相同输入产生非确定性输出](https://github.com/anthropics/claude-code/issues/3370)

---

## 5. 实现最大一致性

要使 SDK 和 CLI 之间的输出**尽可能接近**一致：

### Agent SDK 配置

```typescript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

// 选项 1：使用 claude_code 预设
const response = await client.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 1024,
  // 尽可能匹配 CLI 系统提示
  system: "Your exact system prompt matching CLI",
  messages: [
    { role: "user", content: "挪威的首都是什么？" }
  ],
  // 使用贪心解码以获得最大一致性
  temperature: 0
});

// 选项 2：使用 Agent SDK query 函数
import { query } from "@anthropic-ai/agent-sdk";

for await (const message of query({
  prompt: "挪威的首都是什么？",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code"
    },
    temperature: 0,
    model: "claude-sonnet-4-20250514",
    // 像 CLI 一样加载项目上下文
    settingSources: ["project"]
  }
})) {
  // 处理回复
}
```

### CLI 配置

```bash
# 尽可能匹配 SDK 配置
claude -p "挪威的首都是什么？" \
  --model claude-sonnet-4-20250514 \
  --temperature 0
```

### 仍然无法保证

即使配置完全匹配：
- 不同运行之间输出可能不同
- SDK 与 CLI 之间输出可能不同
- 不存在用于强制可重现性的种子参数

---

## 6. 实际应用建议

### 何时使用各接口

| 使用场景 | 推荐接口 | 原因 |
|----------|----------------------|--------|
| 交互式开发 | Claude CLI | 完整的工具套件、项目上下文 |
| 程序化集成 | Agent SDK | 细粒度控制、可嵌入 |
| 一致性 API 响应 | Agent SDK + 自定义提示 | 对系统提示有更多控制 |
| 批量处理 | Agent SDK | 更适合自动化管道 |
| 一次性任务 | Claude CLI | 设置更快、可获取即时上下文 |

### 设计建议

1. **不要依赖逐比特可重现性**
   - 构建能容忍微小输出变化的应用程序
   - 使用结构化输出和验证

2. **对于需要一致性的生产管道：**
   - 尽可能缓存结果
   - 使用带 JSON schema 验证的结构化输出
   - 结合确定性逻辑和验证
   - 考虑多轮生成并取共识

3. **在 SDK 中匹配 CLI 行为：**
   ```typescript
   systemPrompt: {
     type: "preset",
     preset: "claude_code",
     append: "你的额外指令"
   },
   settingSources: ["project", "user"]
   ```

---

## 7. 系统提示 Token 影响

| 配置 | 架构 | 备注 |
|---------------|-------------|-------|
| SDK（最简模式） | 最小化默认设置 | 仅包含必要的工具指令 |
| SDK（claude_code 预设） | 模块化（约 269+ 基础） | 与 CLI 一致，因功能而异 |
| CLI（默认） | 模块化（约 269+ 基础） | 有条件地加载额外上下文 |
| CLI（带 MCP 工具） | 模块化 + MCP | MCP 工具描述会显著增加 token |

**注意：** Claude Code 使用模块化架构，包含 110+ 个系统提示字符串。基础提示约 269 tokens，各个组件从 18 tokens 到 2,610 tokens 不等，具体取决于激活的功能。

**影响：** SDK 的最小化默认设置为实际任务留出了更多上下文空间，但代价是失去了 Claude Code 的全部能力。

---

## 8. 汇总表

| 维度 | Claude CLI | Agent SDK（默认） | Agent SDK（预设） |
|--------|------------|--------------------|--------------------|
| **系统提示** | 模块化（约 269+ 基础） | 最简 | 模块化（与 CLI 一致） |
| **包含的工具** | 18+ 内置 | 仅当提供时 | 18+ 内置 |
| **CLAUDE.md 自动加载** | 是 | 否 | 否（需要配置） |
| **编码规范** | 有 | 无 | 有 |
| **安全规则** | 完整 | 基础 | 完整 |
| **温度控制** | 是 | 是 | 是 |
| **确定性保证** | 否 | 否 | 否 |
| **输出是否一致？** | 不适用 | 否（对比 CLI） | 更接近，但仍否 |

---

## 9. 结论

**问：SDK 与 CLI 中，相同消息伴随的系统提示有何不同？**

CLI 使用**模块化系统提示架构**，包含约 269 token 的基础提示和 110+ 个按条件加载的组件（工具指令、编码规范、安全规则、项目上下文）。SDK 使用**最小化默认设置**，仅包含必要的工具指令，但可以通过 `claude_code` 预设配置以匹配 CLI 行为。

**问：是否能保证输出一致？**

**不能。** 即便系统提示匹配、输入相同且 `temperature=0`，也无法保证输出一致，原因如下：
- Claude API 中缺乏种子参数
- 浮点数运算差异
- 基础设施层面的非确定性
- 模型架构（混合专家/Mixture-of-Experts）路由差异

**建议：** 设计系统时应能容忍输出变化，而不是依赖确定性行为。对于一致性要求高的应用，请使用结构化输出、缓存和验证层。

---

## 来源

- [Modifying System Prompts - Agent SDK](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/sdk#modifying-system-prompts)
- [Claude Code CLI Reference](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/cli)
- [Claude Code Headless Mode](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/headless)
- [Claude Code Best Practices - Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Claude Messages API Reference](https://docs.anthropic.com/en/api/messages)
- [GitHub Issue #3370: Non-deterministic output](https://github.com/anthropics/claude-code/issues/3370)
- [Claude Code System Prompts Repository](https://github.com/Piebald-AI/claude-code-system-prompts) - 模块化提示架构分析
- [Why Deterministic Output from LLMs is Nearly Impossible](https://unstract.com/blog/understanding-why-deterministic-output-from-llms-is-nearly-impossible/)

---

*本报告由 Claude Code 使用 Opus 4.5 模型于 2026 年 2 月 3 日生成。*
