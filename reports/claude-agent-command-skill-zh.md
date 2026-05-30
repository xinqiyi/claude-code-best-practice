# Agents vs Commands vs Skills — 何时使用哪种

Claude Code 三种扩展机制的对比：subagent、commands 和 skills。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![显示 time-skill、time-command 和 time-agent 的斜杠菜单](assets/agent-command-skill-1.jpg)

---

## 概览

| | Agent | Command | Skill |
|---|---|---|---|
| **位置** | `.claude/agents/<name>.md` | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` |
| **运行上下文** | 独立的 subagent 进程 | 内联（主对话） | 内联（主对话） |
| **用户可调用** | 不在 `/` 菜单中 — 由 Claude 或通过 Agent 工具调用 | 是 — `/command-name` | 是 — `/skill-name`（除非设 `user-invocable: false`） |
| **Claude 自动调用** | 是 — 通过 `description` 字段 | 否 | 是 — 通过 `description` 字段（除非设 `disable-model-invocation: true`） |
| **接受参数** | 通过 `prompt` 参数 | `$ARGUMENTS`、`$0`、`$1` | `$ARGUMENTS`、`$0`、`$1` |
| **动态上下文注入** | 否 | 是 — `` !`command` `` | 是 — `` !`command` `` |
| **独立的上下文窗口** | 是 — 隔离的 | 否 — 共享主上下文 | 否 — 共享主上下文（除非设 `context: fork`） |
| **模型覆盖** | `model:` 前置元数据 | `model:` 前置元数据 | `model:` 前置元数据 |
| **工具限制** | `tools:` / `disallowedTools:` | `allowed-tools:` | `allowed-tools:` |
| **生命周期钩子** | `hooks:` 前置元数据 | — | `hooks:` 前置元数据 |
| **记忆** | `memory:` 前置元数据（user/project/local） | — | — |
| **可预加载 skills** | 是 — `skills:` 前置元数据 | — | — |
| **MCP 服务器** | `mcpServers:` 前置元数据 | — | — |

---

## 何时使用

### 使用 Agent 的场景：

- 任务是**自主且多步骤的**——agent 需要探索、决策并自主行动，无需持续指导
- 你需要**上下文隔离**——工作不应污染主对话窗口
- agent 需要跨会话的**持久记忆**（例如，学习模式的代码审查员）
- 你想通过 skills **预加载领域知识**，而不占用主上下文空间
- 任务适合**在后台运行**或**在 git worktree 中执行**
- 你需要**工具限制**或**不同的权限模式**（例如 `acceptEdits`、`plan`）

**示例**：`weather-agent`——使用预加载的 `weather-fetcher` skill 自主获取天气数据，在独立的上下文中运行，工具受限。

### 使用 Command 的场景：

- 你需要一个**用户触发的入口点**——用户显式调用的工作流
- 工作流涉及**编排**其他 agents 或 skills
- 你想**保持上下文精简**——命令内容在用户触发前不会注入到会话上下文中

**示例**：`weather-orchestrator`——用户触发它，它询问摄氏/华氏偏好，调用 agent，然后调用 SVG skill。

### 使用 Skill 的场景：

- 你想让**Claude 根据用户意图自动调用**——skill 的描述被注入到会话上下文中用于语义匹配
- 任务是一个**可复用的流程**，可以从多个地方调用（commands、agents 或 Claude 自身）
- 你需要**agent 预加载**——在启动时将领域知识注入特定 agent

**示例**：`weather-svg-creator`——当用户要求天气卡片时，Claude 自动调用它；也可以从 commands 中调用。

---

## Command → Agent → Skill 架构

本仓库演示了一种分层编排模式：

```
用户触发 /command
    ↓
Command 编排工作流
    ↓
Command 调用 Agent（独立上下文，自主运行）
    ↓
Agent 使用预加载的 Skill（领域知识）
    ↓
Command 调用 Skill（内联，用于生成输出）
```

**具体示例**——天气系统：

```
/weather-orchestrator（command — 入口点，询问摄氏/华氏）
    ↓
weather-agent（agent — 自主获取温度）
    ├── weather-fetcher（agent skill — 预加载的 API 指令）
    ↓
weather-svg-creator（skill — 内联创建 SVG）
```

---

## 前置元数据对比

### Agent 前置元数据

```yaml
---
name: my-agent
description: Use this agent PROACTIVELY when...
tools: Read, Write, Edit, Bash
model: sonnet
maxTurns: 10
permissionMode: acceptEdits
memory: user
skills:
  - my-skill
---
```

### Command 前置元数据

```yaml
---
description: Do something useful
argument-hint: [issue-number]
allowed-tools: Read, Edit, Bash(gh *)
model: sonnet
---
```

### Skill 前置元数据

```yaml
---
name: my-skill
description: Do something when the user asks for...
argument-hint: [file-path]
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Glob
model: sonnet
context: fork
agent: general-purpose
---
```

---

## 关键区别

### 自动调用

| 机制 | Claude 能自动调用吗？ | 如何阻止 |
|-----------|------------------------|----------------|
| Agent | 是 — 通过 `description`（使用"PROACTIVELY"来鼓励自动调用） | 移除或弱化描述 |
| Command | 否 — 始终由用户通过 `/` 触发 | 不适用 |
| Skill | 是 — 通过 `description` | 设置 `disable-model-invocation: true` |

### 在 `/` 菜单中的可见性

| 机制 | 显示在 `/` 菜单中吗？ | 如何隐藏 |
|-----------|---------------------|-------------|
| Agent | 否 | 不适用 |
| Command | 是 — 始终显示 | 无法隐藏 |
| Skill | 是 — 默认显示 | 设置 `user-invocable: false` |

### 上下文隔离

| 机制 | 在独立上下文中运行？ | 如何配置 |
|-----------|---------------------|-----------------|
| Agent | 始终 | 内置行为 |
| Command | 从不 | 不适用 |
| Skill | 可选 | 设置 `context: fork` |

---

## 实例分析："当前时间是多少？"

本仓库为同一任务定义了所有三种机制——显示 PKT 当前时间。以下是用户输入 **"当前时间是多少？"** 而未显式调用任何 `/` 命令时发生的情况：

| 机制 | 会触发吗？ | 原因 |
|-----------|--------------|---------------|
| `time-command` | 否 | Commands **永远不会自动调用**。用户需要显式输入 `/time-command` 才能运行。Commands 没有自动发现路径——它们严格由用户触发。 |
| `time-agent` | **可能** | agent 的 `description` 写道 *"使用此 agent 显示巴基斯坦标准时间"*。Claude 将其与用户意图匹配，并可能通过 Agent 工具生成它。但 agents 运行在**独立的上下文窗口**中，对于这个简单任务来说过于臃肿。 |
| `time-skill` | **最有可能** | skill 的 `description` 写道 *"显示巴基斯坦标准时间 (PKT, UTC+5)。当用户询问当前时间、巴基斯坦时间或 PKT 时使用。"* Claude 匹配此描述并通过 Skill 工具调用它。由于它**内联运行**，没有上下文开销，是最高效的匹配。 |

### 解析顺序

当多个机制匹配同一意图时，Claude 优先选择满足请求的**最轻量级选项**：

```
1. Skill（内联，无上下文开销）     ← 首选
2. Agent（独立上下文，自主运行）    ← 当 skill 不可用或任务复杂时使用
3. Command（永不 — 需要显式 /）   ← 仅当用户输入 /time-command 时
```

### 如果技能上设置了 `disable-model-invocation: true` 会怎样？

那么 Claude **无法**自动调用该 skill。agent 成为唯一可自动调用的选项，因此 Claude 会改为生成 `time-agent`——代价是对一个简单的 bash 命令使用独立的上下文窗口。

### 如果 skill 和 agent 都禁用了自动调用会怎样？

那么**不会自动触发任何内容**。Claude 会回退到自身的通用知识，可能直接运行 `TZ='Asia/Karachi' date`——不涉及任何扩展机制。用户需要显式输入 `/time-command` 或 `/time-skill` 来使用它们。

![Claude 在用户询问"当前时间是多少？"时自动调用 time-skill](assets/agent-command-skill-2.png)

---

## 来源

- [Claude Code Skills — 官方文档](https://code.claude.com/docs/en/skills)
- [Claude Code Sub-agents — 官方文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code Slash Commands — 官方文档](https://code.claude.com/docs/en/slash-commands)
- [Skills 最佳实践](../best-practice/claude-skills.md)
- [Commands 最佳实践](../best-practice/claude-commands.md)
- [Sub-agents 最佳实践](../best-practice/claude-subagents.md)
