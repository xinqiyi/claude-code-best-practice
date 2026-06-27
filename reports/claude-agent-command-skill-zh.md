# Agents vs Commands vs Skills — 何时使用什么

Claude Code 中三种扩展机制的对比：subagents、commands 和 skills。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

![Slash menu 显示 time-skill、time-command 和 time-agent](assets/agent-command-skill-1.jpg)

---

## 一览

| | Agent | Command | Skill |
|---|---|---|---|
| **位置** | `.claude/agents/<name>.md` | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` |
| **Context** | 独立 subagent 进程 | 内联（主对话） | 内联（主对话） |
| **用户可调用** | 无 `/` menu — 由 Claude 或通过 Agent tool 调用 | Yes — `/command-name` | Yes — `/skill-name`（除非 `user-invocable: false`） |
| **由 Claude 自动调用** | Yes — 通过 `description` 字段 | No | Yes — 通过 `description` 字段（除非 `disable-model-invocation: true`） |
| **接受参数** | 通过 `prompt` 参数 | `$ARGUMENTS`、`$0`、`$1` | `$ARGUMENTS`、`$0`、`$1` |
| **动态 context 注入** | No | Yes — `` !`command` `` | Yes — `` !`command` `` |
| **自己的 context window** | Yes — 隔离 | No — 共享主窗口 | No — 共享主窗口（除非 `context: fork`） |
| **模型覆盖** | `model:` frontmatter | `model:` frontmatter | `model:` frontmatter |
| **工具限制** | `tools:` / `disallowedTools:` | `allowed-tools:` | `allowed-tools:` |
| **Hooks** | `hooks:` frontmatter | — | `hooks:` frontmatter |
| **Memory** | `memory:` frontmatter（user/project/local） | — | — |
| **可预加载 skills** | Yes — `skills:` frontmatter | — | — |
| **MCP servers** | `mcpServers:` frontmatter | — | — |

---

## 何时使用每种

### 在以下情况下使用 Agent：

- 任务是**自主且多步骤的** — agent 需要探索、决策和行动，无需持续指导
- 你需要**context 隔离** — 工作不应污染主对话窗口
- agent 需要跨会话的**持久 memory**（例如，学习模式的 code reviewer）
- 你想要在启动时**预加载领域知识**通过 skills，而不使主 context 混乱
- 任务适合**在后台运行**或在 **git worktree** 中运行
- 你需要**工具限制**或**不同的 permission mode**（例如 `acceptEdits`、`plan`）

**示例**：`weather-agent` — 使用其预加载的 `weather-fetcher` skill 自主获取天气数据，在独立的 context 中以受限工具运行。

### 在以下情况下使用 Command：

- 你需要一个**用户启动的入口点** — 用户明确触发的工作流
- 工作流涉及**编排**其他 agents 或 skills
- 你想要**保持 context 精简** — command 内容在用户触发之前不会注入到会话 context 中

**示例**：`weather-orchestrator` — 用户触发它，它询问 C/F 偏好，调用 agent，然后调用 SVG skill。

### 在以下情况下使用 Skill：

- 你希望 **Claude 根据用户意图自动调用** — skill description 被注入到会话 context 中进行语义匹配
- 任务是一个**可复用过程**，可以从多个地方调用（commands、agents 或 Claude 本身）
- 你需要 **agent 预加载** — 在启动时将领域知识注入特定 agent

**示例**：`weather-svg-creator` — 当用户请求天气卡片时 Claude 自动调用；也可从 commands 调用。

---

## Command → Agent → Skill 架构

本仓库演示了一种分层编排模式：

```
User 触发 /command
    ↓
Command 编排工作流
    ↓
Command 调用 Agent（独立 context，自主执行）
    ↓
Agent 使用预加载的 Skill（领域知识）
    ↓
Command 调用 Skill（内联，用于输出生成）
```

**具体示例** — 天气系统：

```
/weather-orchestrator（command — 入口点，询问 C/F）
    ↓
weather-agent（agent — 自主获取温度）
    ├── weather-fetcher（agent skill — 预加载的 API 指令）
    ↓
weather-svg-creator（skill — 内联创建 SVG）
```

---

## Frontmatter 对比

### Agent Frontmatter

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

### Command Frontmatter

```yaml
---
description: Do something useful
argument-hint: [issue-number]
allowed-tools: Read, Edit, Bash(gh *)
model: sonnet
---
```

### Skill Frontmatter

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

| 机制 | Claude 能否自动调用？ | 如何阻止 |
|-----------|------------------------|----------------|
| Agent | Yes — 通过 `description`（使用 "PROACTIVELY" 鼓励） | 移除或弱化 description |
| Command | No — 始终由用户通过 `/` 启动 | 不适用 |
| Skill | Yes — 通过 `description` | 设置 `disable-model-invocation: true` |

### `/` menu 中的可见性

| 机制 | 出现在 `/` menu 中？ | 如何隐藏 |
|-----------|---------------------|-------------|
| Agent | No | 不适用 |
| Command | Yes — 始终 | 无法隐藏 |
| Skill | Yes — 默认显示 | 设置 `user-invocable: false` |

### Context 隔离

| 机制 | 在自己的 context 中运行？ | 如何配置 |
|-----------|---------------------|-----------------|
| Agent | 始终 | 内置行为 |
| Command | 从不 | 不适用 |
| Skill | 可选 | 设置 `context: fork` |

---

## 实践示例："What is the current time?"

本仓库为同一个任务定义了所有三种机制 — 显示 PKT 当前时间。以下是用户键入 **"What is the current time?"** 而未显式调用任何 `/` command 时发生的情况：

| 机制 | 是否会触发？ | 为什么/为什么不 |
|-----------|--------------|---------------|
| `time-command` | No | Commands **从不自动调用**。用户需要显式键入 `/time-command` 才能运行。Commands 没有自动发现路径 — 它们严格由用户启动。 |
| `time-agent` | **Yes**（可能） | agent 的 `description` 说 *"Use this agent to display the current time in Pakistan Standard Time"*。Claude 将其与用户意图匹配，并可能通过 Agent tool 生成它。但是，agents 在**独立的 context window** 中运行，对于这个简单任务来说重量过重。 |
| `time-skill` | **Yes**（最有可能） | skill 的 `description` 说 *"Display the current time in Pakistan Standard Time (PKT, UTC+5). Use when the user asks for the current time, Pakistan time, or PKT."* Claude 匹配这一点并通过 Skill tool 调用它。由于它是**内联**运行且没有 context 开销，因此是最有效的匹配。 |

### 解析顺序

当多种机制匹配相同意图时，Claude 优先选择满足请求的**最轻量选项**：

```
1. Skill（内联，无 context 开销）     ← 首选
2. Agent（独立 context，自主）    ← 在 skill 不可用或任务复杂时使用
3. Command（从不 — 需要显式 `/`）   ← 仅在用户键入 /time-command 时
```

### 如果 skill 设置了 `disable-model-invocation: true` 会怎样？

那么 Claude **不能**自动调用该 skill。agent 成为唯一可自动调用的选项，因此 Claude 将转而生成 `time-agent` — 代价是为一行 bash 命令单独使用一个独立的 context window。

### 如果 skill 和 agent 都禁用了自动调用会怎样？

那么**没有东西会自动触发**。Claude 将回退到自己的通用知识，很可能直接运行 `TZ='Asia/Karachi' date` — 不涉及任何扩展机制。用户需要显式键入 `/time-command` 或 `/time-skill` 才能使用。

![当用户问 "What is the current time?" 时 Claude 自动调用 time-skill](assets/agent-command-skill-2.png)

---

## Sources

- [Claude Code Skills — Docs](https://code.claude.com/docs/en/skills)
- [Claude Code Sub-agents — Docs](https://code.claude.com/docs/en/sub-agents)
- [Claude Code Slash Commands — Docs](https://code.claude.com/docs/en/slash-commands)
- [Skills Best Practice](../best-practice/claude-skills.md)
- [Commands Best Practice](../best-practice/claude-commands.md)
- [Sub-agents Best Practice](../best-practice/claude-subagents.md)
