# Skills 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Jun%2026%2C%202026%2010%3A06%20AM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.193-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-skills-implementation.md)

Claude Code skills — frontmatter 字段和官方内置 skill。

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
| `name` | string | No | 显示名称和 `/slash-command` 标识符。如果省略，则默认为目录名 |
| `description` | string | Recommended | skill 的功能。显示在自动补全中，并由 Claude 用于自动发现 |
| `when_to_use` | string | No | Claude 何时应调用该 skill 的附加上下文——触发短语和示例请求。追加到 skill listing 中的 `description`，计入 1,536 字符上限 |
| `argument-hint` | string | No | 自动补全期间显示的提示（例如 `[issue-number]`、`[filename]`） |
| `arguments` | string/list | No | 命名的位置参数，用于 skill 内容中的 `$name` 替换。接受空格分隔的字符串或 YAML 列表——名称按顺序映射到参数位置 |
| `disable-model-invocation` | boolean | No | 设置为 `true` 可阻止 Claude 自动调用此 skill |
| `user-invocable` | boolean | No | 设置为 `false` 可从 `/` 菜单中隐藏——skill 变为仅 background knowledge，用于 agent preloading |
| `allowed-tools` | string | No | 此 skill 处于 active 状态时无需权限提示即可使用的工具 |
| `disallowed-tools` | string/list | No | skill active 时从 Claude 可用工具池中移除的工具（例如阻止 `AskUserQuestion` 用于后台循环）。接受空格/逗号分隔的字符串或 YAML 列表——限制在下一条消息时清除 |
| `model` | string | No | 此 skill 运行时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | No | 调用时覆盖模型 effort level（`low`、`medium`、`high`、`xhigh`、`max`） |
| `context` | string | No | 设置为 `fork` 可在隔离的 subagent context 中运行 skill |
| `agent` | string | No | 设置 `context: fork` 时的 subagent 类型（默认：`general-purpose`） |
| `hooks` | object | No | 作用于此 skill 的生命周期 hook |
| `paths` | string/list | No | 限制 skill 自动激活的 glob 模式。接受逗号分隔的字符串或 YAML 列表——Claude 仅在处理匹配文件时才加载该 skill |
| `shell` | string | No | `` !`command` `` 块的 shell —— `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |

---

## ![Official](../!/tags/official.svg) **(10)**

| # | Skill | Description |
|---|-------|-------------|
| 1 | `code-review` | 以选定的 effort level 审查当前 diff 的正确性 bug（low/medium：较少但高置信度的发现；high→max：更广泛覆盖）—— `--comment` 将发现作为 inline PR comment 发布 |
| 2 | `batch` | 批量对多个文件运行命令 |
| 3 | `debug` | 调试失败的命令或代码问题 |
| 4 | `loop` | 以循环间隔（最长 3 天）运行 prompt 或 slash command |
| 5 | `claude-api` | 使用 Claude API 或 Anthropic SDK 构建应用——在 `anthropic` / `@anthropic-ai/sdk` import 时触发 |
| 6 | `fewer-permission-prompts` | 扫描 transcript 中常见的只读 Bash/MCP 调用，并向 `.claude/settings.json` 添加优先 allowlist 以减少权限提示 |
| 7 | `run` | 启动并驱动项目应用到实际应用中查看更改的效果（不仅是测试）。需要 v2.1.145 |
| 8 | `verify` | 构建并运行应用以确认代码更改是否达到预期效果，而不依赖于测试或类型检查。需要 v2.1.145 |
| 9 | `run-skill-generator` | 教会 `/run` 和 `/verify` 如何构建和启动项目——在 `.claude/skills/run-<name>/` 记录每个项目的启动配方。需要 v2.1.145 |
| 10 | `simplify` | 审查更改的代码以寻找清理机会（reuse、simplification、efficiency、abstraction level），四个审查 agent 并行工作。从 v2.1.154 开始**不**寻找正确性 bug——请使用 `/code-review` |

另请参见：[Official Skills Repository](https://github.com/anthropics/skills/tree/main/skills) 获取社区维护的可安装 skill。

---

## Sources

- [Claude Code Skills — Docs](https://code.claude.com/docs/en/skills)
- [Skills Discovery in Monorepos](../reports/claude-skills-for-larger-mono-repos.md)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
