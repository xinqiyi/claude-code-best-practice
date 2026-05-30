# Sub-agents 最佳实践

![最后更新](https://img.shields.io/badge/Last_Updated-May%2021%2C%202026%2012%3A05%20AM%20PKT-white?style=flat&labelColor=555) ![版本](https://img.shields.io/badge/Claude_Code-v2.1.145-blue?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-subagents-implementation.md)

Claude Code subagents — frontmatter 字段和官方内置 agent 类型。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段（16 个）

| 字段 | 类型 | 必需 | 描述 |
|-------|------|----------|-------------|
| `name` | string | 是 | 使用小写字母和连字符的唯一标识符 |
| `description` | string | 是 | 何时调用。使用 `"PROACTIVELY"` 由 Claude 自动调用 |
| `tools` | string/list | 否 | 工具的白名单，逗号分隔（例如 `Read, Write, Edit, Bash`）。省略时继承所有工具。支持 `Agent(agent_type)` 语法以限制可生成的 subagent；旧的 `Task(agent_type)` 别名仍然有效 |
| `disallowedTools` | string/list | 否 | 禁止使用的工具，从继承或指定的列表中移除 |
| `model` | string | 否 | 使用的模型：`sonnet`、`opus`、`haiku`、完整模型 ID（例如 `claude-opus-4-6`）或 `inherit`（默认值：`inherit`） |
| `permissionMode` | string | 否 | 权限模式：`default`、`acceptEdits`、`auto`、`dontAsk`、`bypassPermissions` 或 `plan` |
| `maxTurns` | integer | 否 | Subagent 停止前的最大 agentic 轮次数量 |
| `skills` | list | 否 | 启动时预加载到 agent 上下文中的 skill 名称（注入完整内容，而不仅仅是使其可用） |
| `mcpServers` | list | 否 | 此 subagent 的 MCP server — server 名称字符串或内联的 `{name: config}` 对象 |
| `hooks` | object | 否 | 作用域为此 subagent 的生命周期 hooks。支持所有 hook 事件；`PreToolUse`、`PostToolUse` 和 `Stop` 是最常用的 |
| `memory` | string | 否 | 持久化记忆作用域：`user`、`project` 或 `local` |
| `background` | boolean | 否 | 设为 `true` 以始终作为后台任务运行（默认值：`false`） |
| `effort` | string | 否 | 此 subagent 激活时的 effort 级别覆盖：`low`、`medium`、`high`、`xhigh`、`max`（仅 Opus 4.6）。默认值：继承自 session |
| `isolation` | string | 否 | 设为 `"worktree"` 以在临时 git worktree 中运行（无更改时自动清理） |
| `initialPrompt` | string | 否 | 当此 agent 作为主 session agent 运行时（通过 `--agent` 或 `agent` 设置），自动作为第一个用户轮次提交。会处理 commands 和 skills。会附加到用户提供的 prompt 之前 |
| `color` | string | 否 | Subagent 在任务列表和记录中的显示颜色：`red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink` 或 `cyan` |

---

## ![官方](../!/tags/official.svg)**（5 个）**

| # | Agent | Model | Tools | 描述 |
|---|-------|-------|-------|-------------|
| 1 | `general-purpose` | inherit | 全部 | 复杂的多步骤任务 — 用于研究、代码搜索和自主工作的默认 agent 类型 |
| 2 | `Explore` | haiku | 只读（无 Write、Edit） | 快速的代码库搜索和探索 — 优化用于查找文件、搜索代码和回答代码库相关问题 |
| 3 | `Plan` | inherit | 只读（无 Write、Edit） | Plan 模式下的预先规划研究 — 在编写代码之前探索代码库并设计方案实现路径 |
| 4 | `statusline-setup` | sonnet | Read, Edit | 配置用户的 Claude Code 状态栏设置 |
| 5 | `claude-code-guide` | haiku | Glob, Grep, Read, WebFetch, WebSearch | 回答关于 Claude Code 功能、Agent SDK 和 Claude API 的问题 |

---

## 来源

- [创建自定义 subagents — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [CLI 参考 — Claude Code 文档](https://code.claude.com/docs/en/cli-reference)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
