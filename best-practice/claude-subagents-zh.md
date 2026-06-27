# Sub-agents 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Jun%2026%2C%202026%2011%3A35%20AM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.193-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-subagents-implementation.md)

Claude Code subagents — frontmatter 字段和官方内置 agent 类型。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段 (16)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | 唯一标识符，使用小写字母和连字符 |
| `description` | string | Yes | 何时调用。使用 `"PROACTIVELY"` 让 Claude 自动调用 |
| `tools` | string/list | No | 逗号分隔的工具 allowlist（例如 `Read, Write, Edit, Bash`）。如果省略，则继承所有工具。支持 `Agent(agent_type)` 语法来限制可生成的 subagent；较旧的 `Task(agent_type)` 别名仍然有效 |
| `disallowedTools` | string/list | No | 要拒绝的工具，从继承或指定的列表中移除 |
| `model` | string | No | 要使用的模型：`sonnet`、`opus`、`haiku`、完整 model ID（例如 `claude-opus-4-6`）或 `inherit`（默认：`inherit`） |
| `permissionMode` | string | No | Permission mode：`default`、`acceptEdits`、`auto`、`dontAsk`、`bypassPermissions` 或 `plan` |
| `maxTurns` | integer | No | subagent 停止前的最大 agentic turn 数 |
| `skills` | list | No | 在 agent context 启动时预加载的 skill 名称（完整内容注入，而不仅仅是可用） |
| `mcpServers` | list | No | 此 subagent 的 MCP server —— server 名称字符串或 inline `{name: config}` 对象 |
| `hooks` | object | No | 作用于此 subagent 的生命周期 hook。所有 hook 事件均受支持；`PreToolUse`、`PostToolUse` 和 `Stop` 是最常用的 |
| `memory` | string | No | 持久化 memory scope：`user`、`project` 或 `local` |
| `background` | boolean | No | 设置为 `true` 始终作为后台任务运行（默认：`false`） |
| `effort` | string | No | 此 subagent active 时的 effort level 覆盖：`low`、`medium`、`high`、`xhigh`、`max`（仅 Opus 4.6）。默认：从 session 继承 |
| `isolation` | string | No | 设置为 `"worktree"` 以在临时 git worktree 中运行（如果没有更改则自动清理） |
| `initialPrompt` | string | No | 当此 agent 作为主 session agent（通过 `--agent` 或 `agent` setting）运行时，自动作为第一个用户 turn 提交。command 和 skill 会被处理。添加到用户提供的任何 prompt 之前 |
| `color` | string | No | subagent 在 task list 和 transcript 中的显示颜色：`red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink` 或 `cyan` |

---

## ![Official](../!/tags/official.svg) **(5)**

| # | Agent | Model | Tools | Description |
|---|-------|-------|-------|-------------|
| 1 | `general-purpose` | inherit | All | 复杂的多步骤任务——用于 research、code search 和自主工作的默认 agent 类型 |
| 2 | `Explore` | haiku | Read-only（无 Write、Edit） | 快速代码库搜索和探索——优化用于查找文件、搜索代码和回答代码库问题 |
| 3 | `Plan` | inherit | Read-only（无 Write、Edit） | Plan mode 中的预规划 research——在编写代码之前探索代码库并设计实现方法 |
| 4 | `statusline-setup` | sonnet | Read, Edit | 配置用户的 Claude Code status line 设置 |
| 5 | `claude-code-guide` | haiku | Glob, Grep, Read, WebFetch, WebSearch | 回答关于 Claude Code 功能、Agent SDK 和 Claude API 的问题 |

---

## Sources

- [Create custom subagents — Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [CLI reference — Claude Code Docs](https://code.claude.com/docs/en/cli-reference)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
