# CLI 启动参数最佳实践

![最后更新](https://img.shields.io/badge/Last_Updated-Mar%2002%2C%202026-white?style=flat&labelColor=555)

从终端启动 Claude Code 时使用的 Claude Code 启动参数、顶级子命令和启动环境变量参考。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 目录

1. [会话管理](#session-management)
2. [模型与配置](#model--configuration)
3. [权限与安全](#permissions--security)
4. [输出与格式](#output--format)
5. [系统提示词](#system-prompt)
6. [Agent 与 Subagent](#agent--subagent)
7. [MCP 与插件](#mcp--plugins)
8. [目录与工作区](#directory--workspace)
9. [预算与限制](#budget--limits)
10. [集成](#integration)
11. [初始化与维护](#initialization--maintenance)
12. [调试与诊断](#debug--diagnostics)
13. [设置覆盖](#settings-override)
14. [版本与帮助](#version--help)
15. [子命令](#subcommands)
16. [环境变量](#environment-variables)

---

## 会话管理

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--continue` | `-c` | 继续当前目录中最近的对话 |
| `--resume` | `-r` | 按 ID 或名称恢复特定会话，或显示交互式选择器 |
| `--from-pr <NUMBER\|URL>` | | 恢复链接到特定 GitHub PR 的会话 |
| `--fork-session` | | 在恢复时为会话创建新 ID（与 `--resume` 或 `--continue` 配合使用） |
| `--session-id <UUID>` | | 使用特定的会话 ID（必须是有效的 UUID） |
| `--no-session-persistence` | | 禁用会话持久化（仅 print 模式） |
| `--remote` | | 在 claude.ai 上创建新的 web 会话 |
| `--teleport` | | 在本地终端中恢复 web 会话 |

---

## 模型与配置

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--model <NAME>` | | 使用别名（`sonnet`、`opus`、`haiku`）或完整模型 ID 设置模型 |
| `--fallback-model <NAME>` | | 默认模型过载时自动回退的模型（仅 print 模式） |
| `--betas <LIST>` | | 包含在 API 请求中的 beta 标头（仅 API 密钥用户） |

---

## 权限与安全

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--dangerously-skip-permissions` | | 跳过所有权限提示。请极其谨慎使用 |
| `--allow-dangerously-skip-permissions` | | 启用权限绕过作为选项（但不激活它） |
| `--permission-mode <MODE>` | | 以指定的权限模式启动：`default`、`plan`、`acceptEdits`、`bypassPermissions` |
| `--allowedTools <TOOLS>` | | 无需提示即可执行的 tools（权限规则语法） |
| `--disallowedTools <TOOLS>` | | 从模型上下文中完全移除的 tools |
| `--tools <TOOLS>` | | 限制 Claude 可以使用的内置 tools（使用 `""` 禁用所有内置 tools） |
| `--permission-prompt-tool <TOOL>` | | 指定用于在非交互模式下处理权限提示的 MCP tool |

---

## 输出与格式

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--print` | `-p` | 打印响应，不进入交互模式（headless/SDK 模式） |
| `--output-format <FORMAT>` | | 输出格式：`text`、`json`、`stream-json` |
| `--input-format <FORMAT>` | | 输入格式：`text`、`stream-json` |
| `--json-schema <SCHEMA>` | | 获取符合 JSON schema 的经过验证的 JSON 输出（仅 print 模式） |
| `--include-partial-messages` | | 包含部分流式事件（需要 `--print` 和 `--output-format=stream-json`） |
| `--verbose` | | 启用详细日志输出，显示逐轮完整信息 |

---

## 系统提示词

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--system-prompt <TEXT>` | | 用自定义文本替换整个系统提示词 |
| `--system-prompt-file <PATH>` | | 从文件加载系统提示词，替换默认值（仅 print 模式） |
| `--append-system-prompt <TEXT>` | | 将自定义文本追加到默认系统提示词之后 |
| `--append-system-prompt-file <PATH>` | | 将文件内容追加到默认系统提示词之后（仅 print 模式） |

---

## Agent 与 Subagent

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--agent <NAME>` | | 为当前会话指定一个 agent |
| `--agents <JSON>` | | 通过 JSON 动态定义自定义 subagent |
| `--teammate-mode <MODE>` | | 设置 agent 团队显示模式：`auto`、`in-process`、`tmux` |

---

## MCP 与插件

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--mcp-config <PATH\|JSON>` | | 从 JSON 文件或字符串加载 MCP 服务器 |
| `--strict-mcp-config` | | 仅使用来自 `--mcp-config` 的 MCP 服务器，忽略所有其他配置 |
| `--plugin-dir <PATH>` | | 从目录加载插件（仅限当前会话，可重复使用） |

---

## 目录与工作区

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--add-dir <PATH>` | | 添加额外的供 Claude 访问的工作目录 |
| `--worktree` | `-w` | 在隔离的 git worktree 中启动 Claude（从 HEAD 分支） |

---

## 预算与限制

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--max-budget-usd <AMOUNT>` | | API 调用在达到指定金额前停止的最大美元预算（仅 print 模式） |
| `--max-turns <NUMBER>` | | 限制 agentic 轮次数量（仅 print 模式） |

---

## 集成

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--chrome` | | 启用 Chrome 浏览器集成以进行 web 自动化 |
| `--no-chrome` | | 禁用当前会话的 Chrome 浏览器集成 |
| `--ide` | | 启动时自动连接到 IDE（如果恰好有一个有效的 IDE 可用） |

---

## 初始化与维护

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--init` | | 运行初始化 hooks 并启动交互模式 |
| `--init-only` | | 运行初始化 hooks 后退出（不进入交互会话） |
| `--maintenance` | | 运行维护 hooks 后退出 |

---

## 调试与诊断

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--debug <CATEGORIES>` | | 启用调试模式，可选的类别过滤（例如 `"api,hooks"`） |

---

## 设置覆盖

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--settings <PATH\|JSON>` | | 要加载的 settings JSON 文件路径或 JSON 字符串 |
| `--setting-sources <LIST>` | | 以逗号分隔的要加载的 sources 列表：`user`、`project`、`local` |
| `--disable-slash-commands` | | 禁用当前会话的所有 skills 和 slash 命令 |

---

## 版本与帮助

| 标志 | 缩写 | 描述 |
|------|-------|-------------|
| `--version` | `-v` | 输出版本号 |
| `--help` | `-h` | 显示帮助信息 |

---

## 子命令

以下是通过 `claude <subcommand>` 运行的顶级命令：

| 子命令 | 描述 |
|------------|-------------|
| `claude` | 启动交互式 REPL |
| `claude "query"` | 使用初始提示词启动 REPL |
| `claude agents` | 列出已配置的 agents |
| `claude auth` | 管理 Claude Code 身份验证 |
| `claude doctor` | 从命令行运行诊断 |
| `claude install` | 安装或切换 Claude Code 原生构建版本 |
| `claude mcp` | 配置 MCP 服务器（`add`、`remove`、`list`、`get`、`enable`） |
| `claude plugin` | 管理 Claude Code 插件 |
| `claude remote-control` | 管理远程控制会话 |
| `claude setup-token` | 创建用于订阅使用的长期令牌 |
| `claude update` / `claude upgrade` | 更新到最新版本 |

---

## 环境变量

以下启动时环境变量需在启动 Claude Code 之前在 shell 中设置（它们无法通过 `settings.json` 配置）：

| 变量 | 描述 |
|----------|-------------|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | 启用实验性 agent 团队功能 |
| `CLAUDE_CODE_TMPDIR` | 覆盖内部文件的临时目录。也可通过 `env` 键配置——参见[设置参考](./claude-settings.md#environment-variables-via-env) |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | 启用额外目录的 CLAUDE.md 加载 |
| `DISABLE_AUTOUPDATER=1` | 禁用自动更新。也可通过 `env` 键配置——参见[设置参考](./claude-settings.md#environment-variables-via-env) |
| `CLAUDE_CODE_EFFORT_LEVEL` | 控制思考深度——参见[设置参考](./claude-settings.md#environment-variables-via-env) |
| `USE_BUILTIN_RIPGREP=0` | 使用系统 ripgrep 而非内置版本（Alpine Linux） |
| `CLAUDE_CODE_SIMPLE` | 启用简单模式（仅 Bash + Edit tools）。也可通过 `env` 键配置——参见[设置参考](./claude-settings.md#environment-variables-via-env) |
| `CLAUDE_BASH_NO_LOGIN=1` | 跳过 BashTool 的登录 shell |
| `CCR_FORCE_BUNDLE=1` | 在使用 `claude --remote` 时强制打包/上传本地仓库。也可通过 `env` 键配置——参见[设置参考](./claude-settings.md#environment-variables-via-env) |

有关可通过 `settings.json` 中的 `"env"` 键配置的环境变量（包括 `MAX_THINKING_TOKENS`、`CLAUDE_CODE_SHELL`、`CLAUDE_CODE_ENABLE_TASKS`、`CLAUDE_CODE_DISABLE_BACKGROUND_TASKS`、`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` 等），请参阅 [Claude 设置参考](./claude-settings.md#environment-variables-via-env)。

---

## 来源

- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
- [Claude Code 无头模式](https://code.claude.com/docs/en/headless)
- [Claude Code 设置](https://code.claude.com/docs/en/setup)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code 常见工作流](https://code.claude.com/docs/en/common-workflows)
