# Commands 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Jun%2026%2C%202026%2011%3A06%20AM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.193-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../implementation/claude-commands-implementation.md)

Claude Code commands — frontmatter 字段和官方内置 slash command。

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
| `description` | string | Recommended | command 的功能。显示在自动补全中，并由 Claude 用于自动发现 |
| `when_to_use` | string | No | Claude 何时应调用该 skill 的附加上下文——触发短语和示例请求。追加到 listing 中的 `description`，计入 1,536 字符上限 |
| `argument-hint` | string | No | 自动补全期间显示的提示（例如 `[issue-number]`、`[filename]`） |
| `arguments` | string/list | No | 命名的位置参数，用于 command 内容中的 `$name` 替换。接受空格分隔的字符串或 YAML 列表——名称按顺序映射到参数位置 |
| `disable-model-invocation` | boolean | No | 设置为 `true` 可阻止 Claude 自动调用此 command |
| `user-invocable` | boolean | No | 设置为 `false` 可从 `/` 菜单中隐藏——command 变为仅 background knowledge |
| `paths` | string/list | No | 限制此 skill 何时激活的 glob 模式。接受逗号分隔的字符串或 YAML 列表。设置后，Claude 仅在处理匹配模式的文件时自动加载该 skill |
| `allowed-tools` | string | No | 此 command 处于 active 状态时无需权限提示即可使用的工具 |
| `disallowed-tools` | string/list | No | 在此 command 处于 active 状态时从 Claude 可用工具池中移除的工具。发送下一条消息时清除。`allowed-tools` 的反向 |
| `model` | string | No | 此 command 运行时使用的模型（例如 `haiku`、`sonnet`、`opus`） |
| `effort` | string | No | 调用时覆盖模型 effort level（`low`、`medium`、`high`、`xhigh`、`max`） |
| `context` | string | No | 设置为 `fork` 可在隔离的 subagent context 中运行 command |
| `agent` | string | No | 设置 `context: fork` 时的 subagent 类型（默认：`general-purpose`） |
| `shell` | string | No | `` !`command` `` 块的 shell —— 接受 `bash`（默认）或 `powershell`。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1` |
| `hooks` | object | No | 作用于此 command 的生命周期 hook |

---

## ![Official](../!/tags/official.svg) **(85)**

| # | Command | Tag | Description |
|---|---------|-----|-------------|
| 1 | `/login` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 登录您的 Anthropic 账号 |
| 2 | `/logout` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 退出您的 Anthropic 账号 |
| 3 | `/setup-bedrock` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 通过交互式向导配置 Amazon Bedrock 认证、region 和 model pins。仅在设置 `CLAUDE_CODE_USE_BEDROCK=1` 时可见。首次使用的 Bedrock 用户也可以从登录界面访问此向导 |
| 4 | `/setup-vertex` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 通过交互式向导配置 Google Vertex AI 认证、project、region 和 model pins。仅在设置 `CLAUDE_CODE_USE_VERTEX=1` 时可见。首次使用的 Vertex AI 用户也可以从登录界面访问此向导 |
| 5 | `/upgrade` | ![Auth](https://img.shields.io/badge/Auth-2980B9?style=flat) | 打开升级页面切换到更高级别的 plan tier |
| 6 | `/color [color\|default]` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 为当前 session 设置 prompt bar 颜色。可用颜色：`red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink`、`cyan`。使用 `default` 重置。不带参数运行以选择随机颜色 |
| 7 | `/config [key=value ...]` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 打开 Settings 界面以调整 theme、model、output style 和其他偏好。从 v2.1.181 开始，传递一个或多个 `key=value` 对可直接设置 setting 而无需打开界面，例如 `/config thinking=false`。`key=value` 形式也适用于非交互（`-p`）和 Remote Control 模式。运行 `/config help` 列出可设置的 key。别名：`/settings` |
| 8 | `/focus` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 切换 focus view，仅显示您上次的 prompt、一行工具调用摘要（含编辑 diffstats）和最终响应。选择跨 session 持久化；在 settings 中设置 `viewMode` 可覆盖。仅在 fullscreen 渲染中可用 |
| 9 | `/keybindings` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 打开或创建您的 keybindings 配置文件 |
| 10 | `/permissions` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 管理工具的 allow、ask 和 deny 规则。打开交互式对话框，您可以按 scope 查看规则、添加或移除规则、管理工作目录以及查看最近的 auto mode 拒绝记录。别名：`/allowed-tools` |
| 11 | `/privacy-settings` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 查看和更新您的隐私设置。仅适用于 Pro 和 Max plan 订阅者 |
| 12 | `/radio` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 在浏览器中打开 Claude FM lo-fi 电台。当没有浏览器可用时打印流 URL。在 Bedrock、Vertex 或 Foundry 上不可用 |
| 13 | `/sandbox` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 切换 sandbox 模式。仅在受支持的平台上可用 |
| 14 | `/scroll-speed` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 交互式调整鼠标滚轮滚动速度 |
| 15 | `/statusline` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 配置 Claude Code 的 status line。描述您想要的内容，或不带参数运行以从 shell prompt 自动配置 |
| 16 | `/stickers` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 订购 Claude Code 贴纸 |
| 17 | `/terminal-setup` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 配置终端的 Shift+Enter 和其他快捷键的 keybindings。仅在需要它的终端中可见，如 VS Code、Cursor、Devin Desktop、Alacritty 或 Zed |
| 18 | `/theme` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 更改颜色 theme。包括浅色和深色变体、色盲友好（daltonized）主题、使用终端调色板的 ANSI 主题、"Auto (match terminal)" 选项（跟随终端的浅色/深色模式），以及从 `~/.claude/themes/` 或 plugin 加载的自定义主题。选择 "New custom theme…" 创建您自己的 |
| 19 | `/tui [default\|fullscreen]` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 设置终端 UI 渲染器并重新启动进入其中，保持对话完整。`fullscreen` 启用无闪烁 alt-screen 渲染器。不带参数时打印活动的渲染器 |
| 20 | `/voice [hold\|tap\|off]` | ![Config](https://img.shields.io/badge/Config-F39C12?style=flat) | 切换语音听写，或在特定模式下启用它。需要 Claude.ai 账号 |
| 21 | `/context [all]` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 将当前 context 使用情况可视化为彩色网格。显示 context 密集型工具的优化建议、memory 膨胀和容量警告。传递 `all` 展开完整细目 |
| 22 | `/cost` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | `/usage` 的别名 |
| 23 | `/insights` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 生成分析您的 Claude Code session 的报告，包括项目领域、交互模式和摩擦点 |
| 24 | `/stats` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | `/usage` 的别名。在 Stats 选项卡上打开 |
| 25 | `/status` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 打开 Settings 界面（Status 选项卡），显示 version、model、account 和连接状态。在 Claude 响应期间也可工作，无需等待当前响应完成 |
| 26 | `/usage` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 显示 session 成本、plan 使用限制和活动统计。在 Pro、Max、Team 或 Enterprise plan 上，包括按 skill、subagent、plugin 和 MCP server 的使用细目。`/cost` 和 `/stats` 是别名 |
| 27 | `/usage-credits` | ![Context](https://img.shields.io/badge/Context-8E44AD?style=flat) | 配置 usage credits 以在达到限制时继续工作。之前称为 `/extra-usage` |
| 28 | `/doctor` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 诊断并验证您的 Claude Code 安装和 settings。结果显示带有状态图标。按 `f` 让 Claude 修复任何报告的问题 |
| 29 | `/feedback [report]` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 提交反馈、报告 bug 或分享您的对话。别名：`/bug`、`/share` |
| 30 | `/heapdump` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 将 JavaScript heap snapshot 和内存细目写入 `~/Desktop`，用于诊断高内存使用。在提交关于内存增长的 bug 报告时有用 |
| 31 | `/help` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 显示帮助和可用命令 |
| 32 | `/powerup` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 通过带有动画演示的快速交互式课程发现 Claude Code 功能 |
| 33 | `/release-notes` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 在交互式版本选择器中查看 changelog。选择特定版本查看其 release notes，或选择显示所有版本 |
| 34 | `/tasks` | ![Debug](https://img.shields.io/badge/Debug-E74C3C?style=flat) | 查看和管理所有在后台运行的内容。也可用 `/bashes` |
| 35 | `/copy [N]` | ![Export](https://img.shields.io/badge/Export-7F8C8D?style=flat) | 复制最后一条助手响应到剪贴板。传递数字 `N` 复制第 N 条最新响应：`/copy 2` 复制倒数第二条。当存在 code block 时，显示交互式选择器以选择单个块或完整响应。在选择器中按 `w` 将选择写入文件而不是剪贴板，这在 SSH 中很有用 |
| 36 | `/export [filename]` | ![Export](https://img.shields.io/badge/Export-7F8C8D?style=flat) | 将当前对话导出为纯文本。带文件名时直接写入该文件。不带时打开对话框以复制到剪贴板或保存到文件 |
| 37 | `/agents` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 agent 配置 |
| 38 | `/chrome` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 配置 Claude in Chrome 设置 |
| 39 | `/hooks` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 查看工具事件的 hook 配置 |
| 40 | `/ide` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 IDE 集成并显示状态 |
| 41 | `/mcp [reconnect <server>\|enable\|disable [<server>\|all]]` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 MCP server 连接和 OAuth 认证。不带参数运行以打开交互式列表，传递 `reconnect <server>` 重新连接一个断开的 server，或传递 `enable`/`disable` 带 server 名称或 `all` 来更改连接状态而不打开对话框 |
| 42 | `/plugin [subcommand]` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 管理 Claude Code plugin。不带参数运行以打开 plugin 菜单，或传递子命令如 `list`、`install`、`enable` 或 `disable` 直接操作 |
| 43 | `/reload-plugins [--force]` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 重新加载所有活跃 plugin 以应用待处理的更改而无需重启。报告每个重新加载组件的计数并标记任何加载错误。当重新加载会更改加载的 MCP 工具并使 prompt cache 失效时，该命令会警告并跳过，除非您传递 `--force` |
| 44 | `/reload-skills` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 重新扫描 skill 和 command 目录，使 session 期间在磁盘上添加或更改的 skill 可用而无需重启。报告可用的 skill 数量以及添加或移除了多少 |
| 45 | `/skills` | ![Extensions](https://img.shields.io/badge/Extensions-16A085?style=flat) | 列出可用的 skill。按 `t` 按 token 数量排序。按 `Space` 对 Claude 或 `/` 菜单隐藏 skill，然后按 `Enter` 保存 |
| 46 | `/memory` | ![Memory](https://img.shields.io/badge/Memory-3498DB?style=flat) | 编辑 `CLAUDE.md` memory 文件，启用或禁用 auto-memory，并查看 auto-memory 条目 |
| 47 | `/advisor [model\|off]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 启用或禁用 advisor 工具，该工具在任务的关键时刻咨询第二个模型以获取指导。接受模型名称（`opus`、`sonnet`、`fable`）或完整 model ID；不带参数打开选择器。使用 `off` 禁用 |
| 48 | `/effort [low\|medium\|high\|xhigh\|max\|ultracode]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 设置模型 effort level。可用级别取决于模型，包括 `low`、`medium`、`high`、`xhigh`、`max`（仅 session）和 `ultracode`（结合 `xhigh` reasoning 与自动 workflow orchestration；仅 session）。不带参数时打开交互式滑块选择级别。`auto` 重置为模型默认值。立即生效，无需等待当前响应完成 |
| 49 | `/fast [on\|off]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 打开或关闭 fast mode |
| 50 | `/model [model]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 切换 AI 模型并将其保存为新 session 的默认值。在某一行上按 `s` 仅对当前 session 切换。对于支持 effort level 的模型，使用左右箭头调整 effort level。在之前有输出后中途切换模型时，Claude 会在应用更改前警告 |
| 51 | `/passes` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 与朋友分享一周免费的 Claude Code。仅在您的账号符合条件时可见 |
| 52 | `/plan [description]` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 直接从 prompt 进入 plan mode。传递可选描述以进入 plan mode 并立即开始该任务，例如 `/plan fix the auth bug` |
| 53 | `/ultraplan <prompt>` | ![Model](https://img.shields.io/badge/Model-E67E22?style=flat) | 在 ultraplan session 中起草计划，在浏览器中审查，然后远程执行或发送回您的终端 |
| 54 | `/add-dir <path>` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 为当前 session 添加文件访问的工作目录。大多数 `.claude/` 配置不会从添加的目录中发现 |
| 55 | `/diff` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 打开交互式 diff viewer，显示未提交的更改和逐轮 diff。使用左右箭头在当前 git diff 和各个 Claude turn 之间切换，使用上下键浏览文件 |
| 56 | `/init` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 使用 `CLAUDE.md` 指南初始化项目。设置 `CLAUDE_CODE_NEW_INIT=1` 以获得交互式流程，该流程还逐步指导 skill、hook 和个人 memory 文件 |
| 57 | `/review [PR]` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 按编号审查 GitHub pull request，使用与 `/code-review` 相同的审查引擎。不带参数时列出开放的 PR |
| 58 | `/security-review` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 分析当前分支上的待处理更改的安全漏洞。审查 git diff 并识别 injection、auth 问题和数据暴露等风险 |
| 59 | `/team-onboarding` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 从您的 Claude Code 使用历史生成 team onboarding 指南。Claude 分析您过去 30 天的 session、command 和 MCP server 使用情况，并生成 markdown 指南。对于 Pro、Max、Team 和 Enterprise plan 的 claude.ai 订阅者，还会返回一个分享链接，队友可以直接在 Claude Code 中打开 |
| 60 | `/ultrareview [PR]` | ![Project](https://img.shields.io/badge/Project-27AE60?style=flat) | 在云 sandbox 中运行深度、multi-agent code review。首选调用方式是 `/code-review ultra`；`/ultrareview` 保留为别名。在 Pro 和 Max 上包含 3 次免费运行，之后需要 usage credits |
| 61 | `/autofix-pr [prompt]` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 生成一个 Claude Code on the web session，监视当前分支的 PR，并在 CI 失败或审查者留下评论时推送修复。从您检出的分支通过 `gh pr view` 检测开放的 PR；要监视不同的 PR，请先检出其分支。需要 `gh` CLI 和 Claude Code on the web 的访问权限 |
| 62 | `/desktop` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 在 Claude Code Desktop 应用中继续当前 session。仅 macOS 和 Windows。别名：`/app` |
| 63 | `/install-github-app` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 为 repository 安装 Claude GitHub App，可选择设置 GitHub Actions workflow 和 secret |
| 64 | `/install-slack-app` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 安装 Claude Slack app。打开浏览器完成 OAuth 流程 |
| 65 | `/mobile` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 显示下载 Claude mobile app 的二维码。别名：`/ios`、`/android` |
| 66 | `/remote-control` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 使此 session 可从 claude.ai 进行 remote control。别名：`/rc` |
| 67 | `/remote-env` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 选择 cloud agent 的默认环境 |
| 68 | `/schedule [description]` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 创建、更新、列出或运行 routine。Claude 以对话方式引导您完成设置。别名：`/routines` |
| 69 | `/teleport` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 将 Claude Code on the web session 拉入此终端：打开选择器，然后获取分支和对话。也可用 `/tp`。需要 claude.ai 订阅 |
| 70 | `/web-setup` | ![Remote](https://img.shields.io/badge/Remote-5D6D7E?style=flat) | 使用您本地的 `gh` CLI 凭据将您的 GitHub 账号连接到 Claude Code on the web。`/schedule` 如果 GitHub 未连接会自动提示此操作 |
| 71 | `/background [prompt]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 将当前 session 分离为后台 agent 运行并释放此终端。传递 prompt 以在分离前发送一条指令。使用 `claude agents` 监视 session。别名：`/bg` |
| 72 | `/branch [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 在此时点创建当前对话的分支 |
| 73 | `/btw <question>` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 快速提出旁侧问题而不添加到对话中 |
| 74 | `/cd <path>` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 将 session 移动到新的工作目录而不破坏 prompt cache |
| 75 | `/clear [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 以空 context 开始新对话。传递可选的 `name` 为之前的对话标记标签以便通过 `/resume` 轻松检索。要在继续同一对话的同时释放 context，改用 `/compact`。别名：`/reset`、`/new` |
| 76 | `/compact [instructions]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 压缩对话，可选焦点指令 |
| 77 | `/exit` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 退出 CLI。在附加的后台 session 中，此操作分离且 session 继续运行。别名：`/quit` |
| 78 | `/fork <directive>` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 生成一个 forked subagent：一个继承完整对话的后台 subagent，在执行指令的同时您可以继续工作 |
| 79 | `/goal [condition\|clear]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 设置目标——Claude 在条件满足之前跨 turn 持续工作。不带参数时显示当前或最近达成的目标。`clear`、`stop`、`off`、`reset`、`none` 或 `cancel` 可提前移除活跃目标 |
| 80 | `/recap` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 按需生成当前 session 的一行摘要，不影响进行中的对话 |
| 81 | `/rename [name]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 重命名当前 session 并在 prompt bar 上显示名称。不带名称时从对话历史自动生成 |
| 82 | `/resume [session]` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 通过 ID 或名称恢复对话，或打开 session 选择器。从 v2.1.144 开始，后台 session 在选择器中以 `bg` 标记。别名：`/continue` |
| 83 | `/rewind` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 将对话和/或代码回退到之前的点，或从选定的消息摘要。参见 checkpointing。别名：`/checkpoint`、`/undo` |
| 84 | `/stop` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 停止当前后台 session。仅在附加到后台 session 时可用；transcript 和任何 worktree 会保留。要分离而不停止，使用 `/exit` 或按 `←` |
| 85 | `/workflows` | ![Session](https://img.shields.io/badge/Session-4A90D9?style=flat) | 打开 workflow 进度视图以监视、暂停、恢复或保存运行中和已完成的 workflow |

像 `/debug` 这样的内置 skill 也可以出现在 slash-command 菜单中，但它们不是内置 command。

---

## Sources

- [Claude Code Slash Commands](https://code.claude.com/docs/en/slash-commands)
- [Claude Code Interactive Mode](https://code.claude.com/docs/en/interactive-mode)
- [Claude Code CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
