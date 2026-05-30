# 技能最佳实践

![最后更新](https://img.shields.io/badge/Last_Updated-May%2021%2C%202026%2012%3A04%20AM%20PKT-white?style=flat&labelColor=555) ![版本](https://img.shields.io/badge/Claude_Code-v2.1.145-blue?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-skills-implementation.md)

Claude Code skills——frontmatter 字段和官方内置 skills。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Frontmatter 字段（15）

| 字段 | 类型 | 必需 | 描述 |
|-------|------|----------|-------------|
| `name` | string | 否 | 显示名称和 `/slash-command` 标识符。如果省略，默认为目录名称 |
| `description` | string | 推荐 | 描述该 skill 的功能。显示在自动补全中，供 Claude 用于自动发现 |
| `when_to_use` | string | 否 | Claude 应在何时调用该 skill 的额外上下文——触发短语和示例请求。追加到 skill 列表中的 `description` 字段，计入 1,536 字符上限 |
| `argument-hint` | string | 否 | 自动补全时显示的提示（例如 `[issue-number]`、`[filename]`） |
| `arguments` | string/list | 否 | 用于在 skill 内容中进行 `$name` 替换的命名位置参数。接受空格分隔的字符串或 YAML 列表——名称按顺序映射到参数位置 |
| `disable-model-invocation` | boolean | 否 | 设置为 `true` 可阻止 Claude 自动调用该 skill |
| `user-invocable` | boolean | 否 | 设置为 `false` 可从 `/` 菜单中隐藏——该 skill 仅作为背景知识，用于 agent 预加载 |
| `allowed-tools` | string | 否 | 该 skill 激活时允许无需权限提示即可使用的工具 |
| `model` | string | 否 | 运行该 skill 时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | 否 | 调用时覆盖模型的 effort 级别（`low`、`medium`、`high`、`xhigh`、`max`） |
| `context` | string | 否 | 设置为 `fork` 可在隔离的 subagent 上下文中运行该 skill |
| `agent` | string | 否 | 设置 `context: fork` 时的 subagent 类型（默认：`general-purpose`） |
| `hooks` | object | 否 | 作用于该 skill 的生命周期 hooks |
| `paths` | string/list | 否 | 限制 skill 自动激活条件的 Glob 模式。接受逗号分隔的字符串或 YAML 列表——Claude 仅在处理匹配文件时加载该 skill |
| `shell` | string | 否 | `` !`command` `` 代码块使用的 Shell——`bash`（默认）或 `powershell`。需要设置 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |

---

## ![官方](../!/tags/official.svg) **（9）**

| # | Skill | 描述 |
|---|-------|-------------|
| 1 | `simplify` | 审查已更改的代码，优化复用性、质量和效率——重构以消除重复 |
| 2 | `batch` | 批量对多个文件运行命令 |
| 3 | `debug` | 调试失败的命令或代码问题 |
| 4 | `loop` | 按固定间隔重复运行提示词或斜杠命令（最长 3 天） |
| 5 | `claude-api` | 使用 Claude API 或 Anthropic SDK 构建应用——在导入 `anthropic` / `@anthropic-ai/sdk` 时触发 |
| 6 | `fewer-permission-prompts` | 扫描对话记录中的常见只读 Bash/MCP 调用，并向 `.claude/settings.json` 添加优先级白名单以减少权限提示 |
| 7 | `run` | 启动并驱动项目应用，查看更改在真实应用中的运行效果（不仅仅是测试）。需要 v2.1.145 |
| 8 | `verify` | 构建并运行应用以确认代码更改达到了预期效果，无需依赖测试或类型检查。需要 v2.1.145 |
| 9 | `run-skill-generator` | 教导 `/run` 和 `/verify` 如何构建和启动项目——在 `.claude/skills/run-<name>/` 记录每个项目的启动配方。需要 v2.1.145 |

另见：社区维护的可安装 skills 请参考[官方 Skills 仓库](https://github.com/anthropics/skills/tree/main/skills)。

---

## 来源

- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [Monorepo 中的 Skills 发现](../reports/claude-skills-for-larger-mono-repos.md)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
