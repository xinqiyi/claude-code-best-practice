# CLI 启动参数最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2002%2C%202026-white?style=flat&labelColor=555)

Claude Code 从终端启动时的启动参数、顶级子命令和启动环境变量参考。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 目录

1. [Session Management](#session-management)
2. [Model & Configuration](#model--configuration)
3. [Permissions & Security](#permissions--security)
4. [Output & Format](#output--format)
5. [System Prompt](#system-prompt)
6. [Agent & Subagent](#agent--subagent)
7. [MCP & Plugins](#mcp--plugins)
8. [Directory & Workspace](#directory--workspace)
9. [Budget & Limits](#budget--limits)
10. [Integration](#integration)
11. [Initialization & Maintenance](#initialization--maintenance)
12. [Debug & Diagnostics](#debug--diagnostics)
13. [Settings Override](#settings-override)
14. [Version & Help](#version--help)
15. [Subcommands](#subcommands)
16. [Environment Variables](#environment-variables)

---

## Session Management

| Flag | Short | Description |
|------|-------|-------------|
| `--continue` | `-c` | 继续当前目录中最近的对话 |
| `--resume` | `-r` | 通过 ID 或名称恢复特定 session，或显示交互式选择器 |
| `--from-pr <NUMBER\|URL>` | | 恢复链接到特定 GitHub PR 的 session |
| `--fork-session` | | 恢复时创建新的 session ID（与 `--resume` 或 `--continue` 配合使用） |
| `--session-id <UUID>` | | 使用特定的 session ID（必须是有效的 UUID） |
| `--no-session-persistence` | | 禁用 session 持久化（仅 print mode） |
| `--remote` | | 在 claude.ai 上创建新的 web session |
| `--teleport` | | 在本地终端中恢复 web session |

---

## Model & Configuration

| Flag | Short | Description |
|------|-------|-------------|
| `--model <NAME>` | | 使用别名（`sonnet`、`opus`、`haiku`）或完整 model ID 设置模型 |
| `--fallback-model <NAME>` | | 当默认模型过载时自动 fallback 的模型（仅 print mode） |
| `--betas <LIST>` | | 包含在 API 请求中的 Beta header（仅限 API key 用户） |

---

## Permissions & Security

| Flag | Short | Description |
|------|-------|-------------|
| `--dangerously-skip-permissions` | | 跳过所有权限提示。请极其谨慎地使用 |
| `--allow-dangerously-skip-permissions` | | 允许权限绕过作为选项而不激活它 |
| `--permission-mode <MODE>` | | 以指定的 permission mode 启动：`default`、`plan`、`acceptEdits`、`bypassPermissions` |
| `--allowedTools <TOOLS>` | | 无需提示即可执行的工具（permission rule 语法） |
| `--disallowedTools <TOOLS>` | | 从模型 context 中完全移除的工具 |
| `--tools <TOOLS>` | | 限制 Claude 可以使用的内置工具（使用 `""` 禁用所有） |
| `--permission-prompt-tool <TOOL>` | | 指定 MCP 工具在非交互模式下处理权限提示 |

---

## Output & Format

| Flag | Short | Description |
|------|-------|-------------|
| `--print` | `-p` | 打印响应而不进入交互模式（headless/SDK 模式） |
| `--output-format <FORMAT>` | | 输出格式：`text`、`json`、`stream-json` |
| `--input-format <FORMAT>` | | 输入格式：`text`、`stream-json` |
| `--json-schema <SCHEMA>` | | 获取匹配 schema 的验证 JSON（仅 print mode） |
| `--include-partial-messages` | | 包含部分 streaming 事件（需要 `--print` 和 `--output-format=stream-json`） |
| `--verbose` | | 启用详细日志记录，带有完整的逐轮输出 |

---

## System Prompt

| Flag | Short | Description |
|------|-------|-------------|
| `--system-prompt <TEXT>` | | 用自定义文本替换整个 system prompt |
| `--system-prompt-file <PATH>` | | 从文件加载 system prompt，替换默认值（仅 print mode） |
| `--append-system-prompt <TEXT>` | | 将自定义文本追加到默认 system prompt |
| `--append-system-prompt-file <PATH>` | | 将文件内容追加到默认 prompt（仅 print mode） |

---

## Agent & Subagent

| Flag | Short | Description |
|------|-------|-------------|
| `--agent <NAME>` | | 为当前 session 指定一个 agent |
| `--agents <JSON>` | | 通过 JSON 动态定义自定义 subagent |
| `--teammate-mode <MODE>` | | 设置 agent team 显示方式：`auto`、`in-process`、`tmux` |

---

## MCP & Plugins

| Flag | Short | Description |
|------|-------|-------------|
| `--mcp-config <PATH\|JSON>` | | 从 JSON 文件或字符串加载 MCP server |
| `--strict-mcp-config` | | 仅使用 `--mcp-config` 中的 MCP server，忽略所有其他 |
| `--plugin-dir <PATH>` | | 从目录加载 plugin 用于当前 session（可重复） |

---

## Directory & Workspace

| Flag | Short | Description |
|------|-------|-------------|
| `--add-dir <PATH>` | | 添加 Claude 可访问的额外工作目录 |
| `--worktree` | `-w` | 在隔离的 git worktree 中启动 Claude（从 HEAD 分支） |

---

## Budget & Limits

| Flag | Short | Description |
|------|-------|-------------|
| `--max-budget-usd <AMOUNT>` | | API 调用在停止前的最大美元金额（仅 print mode） |
| `--max-turns <NUMBER>` | | 限制 agentic turn 的数量（仅 print mode） |

---

## Integration

| Flag | Short | Description |
|------|-------|-------------|
| `--chrome` | | 启用 Chrome 浏览器集成以进行 web 自动化 |
| `--no-chrome` | | 为此 session 禁用 Chrome 浏览器集成 |
| `--ide` | | 如果恰好有一个有效的 IDE 可用，启动时自动连接到 IDE |

---

## Initialization & Maintenance

| Flag | Short | Description |
|------|-------|-------------|
| `--init` | | 运行初始化 hook 并启动交互模式 |
| `--init-only` | | 运行初始化 hook 并退出（不启动交互 session） |
| `--maintenance` | | 运行维护 hook 并退出 |

---

## Debug & Diagnostics

| Flag | Short | Description |
|------|-------|-------------|
| `--debug <CATEGORIES>` | | 启用 debug 模式，可选类别过滤（例如 `"api,hooks"`） |

---

## Settings Override

| Flag | Short | Description |
|------|-------|-------------|
| `--settings <PATH\|JSON>` | | 要加载的 settings JSON 文件路径或 JSON 字符串 |
| `--setting-sources <LIST>` | | 要加载的逗号分隔来源列表：`user`、`project`、`local` |
| `--disable-slash-commands` | | 为此 session 禁用所有 skill 和 slash command |

---

## Version & Help

| Flag | Short | Description |
|------|-------|-------------|
| `--version` | `-v` | 输出版本号 |
| `--help` | `-h` | 显示帮助信息 |

---

## Subcommands

这些是作为 `claude <subcommand>` 运行的顶级命令：

| Subcommand | Description |
|------------|-------------|
| `claude` | 启动交互式 REPL |
| `claude "query"` | 以初始 prompt 启动 REPL |
| `claude agents` | 列出已配置的 agent |
| `claude auth` | 管理 Claude Code 认证 |
| `claude doctor` | 从命令行运行诊断 |
| `claude install` | 安装或切换 Claude Code 原生构建 |
| `claude mcp` | 配置 MCP server（`add`、`remove`、`list`、`get`、`enable`） |
| `claude plugin` | 管理 Claude Code plugin |
| `claude remote-control` | 管理 remote control session |
| `claude setup-token` | 创建用于订阅使用的长期 token |
| `claude update` / `claude upgrade` | 更新到最新版本 |

---

## Environment Variables

这些仅限启动时的环境变量在启动 Claude Code 之前在 shell 中设置（它们不能通过 `settings.json` 配置）：

| Variable | Description |
|----------|-------------|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | 启用实验性 agent team。也可通过 env 配置——参见 [Settings 参考](./claude-settings-zh.md#environment-variables) |
| `CLAUDE_CODE_TMPDIR` | 覆盖内部文件的临时目录。也可通过 `env` key 配置——参见 [Settings 参考](./claude-settings-zh.md#environment-variables-via-env) |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | 启用额外目录 CLAUDE.md 加载。也可通过 `env` key 配置——参见 [Settings 参考](./claude-settings-zh.md#environment-variables-via-env) |
| `DISABLE_AUTOUPDATER=1` | 禁用自动更新。也可通过 `env` key 配置——参见 [Settings 参考](./claude-settings-zh.md#environment-variables-via-env) |
| `CLAUDE_CODE_EFFORT_LEVEL` | 控制 thinking 深度——参见 [Settings 参考](./claude-settings-zh.md#environment-variables-via-env) |
| `USE_BUILTIN_RIPGREP=0` | 使用系统 ripgrep 代替内置版本（Alpine Linux） |
| `CLAUDE_CODE_SIMPLE` | 启用 simple mode（仅 Bash + Edit 工具）。也可通过 `env` key 配置——参见 [Settings 参考](./claude-settings-zh.md#environment-variables-via-env) |
| `CLAUDE_BASH_NO_LOGIN=1` | 跳过 BashTool 的 login shell |
| `CCR_FORCE_BUNDLE=1` | 使用 `claude --remote` 时强制 bundling/上传本地 repository。也可通过 `env` key 配置——参见 [Settings 参考](./claude-settings-zh.md#environment-variables-via-env) |

关于可通过 `settings.json` 中的 `"env"` key 配置的环境变量（包括 `MAX_THINKING_TOKENS`、`CLAUDE_CODE_SHELL`、`CLAUDE_CODE_ENABLE_TASKS`、`CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`、`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` 等），请参见 [Claude Settings 参考](./claude-settings-zh.md#environment-variables-via-env)。

---

## Sources

- [Claude Code CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [Claude Code Headless Mode](https://code.claude.com/docs/en/headless)
- [Claude Code Setup](https://code.claude.com/docs/en/setup)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code Common Workflows](https://code.claude.com/docs/en/common-workflows)
