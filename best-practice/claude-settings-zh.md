# Settings 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Jun%2026%2C%202026%2010%3A46%20AM%20PKT-white?style=flat&labelColor=555) ![Version](https://img.shields.io/badge/Claude_Code-v2.1.193-blue?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../.claude/settings.json)

Claude Code 的 `settings.json` 文件中所有可用配置选项的全面指南。截至 v2.1.193，Claude Code 暴露了 **80+ 项 setting** 和 **200+ 个环境变量**（使用 `settings.json` 中的 `"env"` 字段以避免 wrapper script）。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [Settings Hierarchy](#settings-hierarchy)
2. [Core Configuration](#core-configuration)
3. [Permissions](#permissions)
4. [Hooks](#hooks)
5. [MCP Servers](#mcp-servers)
6. [Sandbox](#sandbox)
7. [Plugins](#plugins)
8. [Model Configuration](#model-configuration)
9. [Display & UX](#display--ux)
10. [AWS & Cloud Credentials](#aws--cloud-credentials)
11. [Environment Variables](#environment-variables-via-env)
12. [Useful Commands](#useful-commands)

---

## Settings Hierarchy

Setting 按照优先级顺序应用（从高到低）：

| Priority | Location | Scope | Shared? | Purpose |
|----------|----------|-------|---------|---------|
| 1 | Managed settings | Organization | Yes（由 IT 部署） | 不可被覆盖的安全策略 |
| 2 | Command line arguments | Session | N/A | 临时的单 session 覆盖 |
| 3 | `.claude/settings.local.json` | Project | No（git-ignored） | 个人项目专用 |
| 4 | `.claude/settings.json` | Project | Yes（已提交） | 团队共享 settings |
| 5 | `~/.claude/settings.json` | User | N/A | 全局个人默认值 |

**Managed settings** 是组织强制执行的，不能被任何其他级别覆盖，包括命令行参数。交付方式：
- **Server-managed** settings（远程交付）
- **MDM profiles** — macOS plist 位于 `com.anthropic.claudecode`
- **Registry policies** — Windows `HKLM\SOFTWARE\Policies\ClaudeCode`（管理员）和 `HKCU\SOFTWARE\Policies\ClaudeCode`（用户级，最低策略优先级）
- **File** — `managed-settings.json` 和 `managed-mcp.json`（macOS：`/Library/Application Support/ClaudeCode/`，Linux/WSL：`/etc/claude-code/`，Windows：`C:\Program Files\ClaudeCode\`）
- **Drop-in directory** — 与 `managed-settings.json` 并列的 `managed-settings.d/`，用于独立的策略片段（v2.1.83）。遵循 systemd 约定，`managed-settings.json` 首先作为基础合并，然后 `*.json` 文件按字母排序并合并到顶部。较晚的文件覆盖较早的标量值；数组被连接和去重；对象被深度合并。以 `.` 开头的隐藏文件会被忽略。使用数字前缀控制合并顺序（例如 `10-telemetry.json`、`20-security.json`）

在 managed 层级内，优先级为：server-managed > MDM/OS-level policies > file-based（`managed-settings.d/*.json` + `managed-settings.json`）> HKCU registry（仅 Windows）。只使用一个 managed source；不同层级的 source 不会合并。在 file-based 层级内，drop-in 文件和基础文件会合并在一起。

> **Note:** 截至 v2.1.75，已弃用的 Windows 回退路径 `C:\ProgramData\ClaudeCode\managed-settings.json` 已被移除。请改用 `C:\Program Files\ClaudeCode\managed-settings.json`。

> **Note (v2.1.126):** `/config` 现在将更改持久化到 `~/.claude/settings.json`，而不是仅保留在内存中。通过交互式 Config UI 所做的编辑在重启后仍然存在。

**仅限 managed 的策略 key：**

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `parentSettingsBehavior` | string | `"first-wins"` | 控制当嵌入宿主进程（SDK parent）以编程方式提供的 managed settings 与管理员部署的 managed 层级同时存在时的行为。`"first-wins"`：丢弃 parent 提供的 settings，仅应用管理员层级。`"merge"`：parent 提供的 settings 在管理员层级下应用，并经过过滤，使其可以**收紧**策略但不能放松。需要 v2.1.133+ |
| `policyHelper` | object | - | 管理员部署的可执行文件，在每次启动时动态计算 managed settings。对象形状：`{path: string}` 指向 helper 二进制文件。仅从 MDM 或系统 `managed-settings.json` 文件生效（绝不从 user/project settings 生效）。Helper 输出在每次启动时合并到 managed 层级。需要 v2.1.136+ |
| `requiredMinimumVersion` | string | - | **（仅限 Managed）** 如果安装的版本低于此最低版本，阻止 Claude Code 启动。CLI 以错误退出，提示用户升级。与 `minimumVersion`（控制自动更新下限）互补——此 key 在启动时强制执行。示例：`"2.1.163"` |
| `requiredMaximumVersion` | string | - | **（仅限 Managed）** 如果安装的版本超过此上限，阻止 Claude Code 启动。如果版本太新，CLI 以错误退出。与 `requiredMinimumVersion` 一起使用以在 managed 环境中锁定特定版本范围。示例：`"2.1.165"` |

**Important**：
- `deny` 规则具有最高的安全优先级，不能被较低优先级的 allow/ask 规则覆盖。
- Managed settings 可能锁定或覆盖本地行为，即使本地文件指定了不同的值。
- 数组 settings（例如 `permissions.allow`）跨 scope **连接和去重**——来自所有层级的条目被合并，而不是替换。

---

## Core Configuration

### General Settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `$schema` | string | - | 用于 IDE 验证和自动补全的 JSON Schema URL（例如 `"https://json.schemastore.org/claude-code-settings.json"`） |
| `model` | string | `"default"` | 覆盖默认模型。接受别名（`sonnet`、`opus`、`haiku`）或完整 model ID |
| `agent` | string | - | 设置主对话的默认 agent。值是来自 `.claude/agents/` 的 agent 名称。也可通过 `--agent` CLI flag 设置 |
| `language` | string | `"english"` | Claude 的首选响应语言。也设置语音听写语言和终端选项卡标题（v2.1.121） |
| `claudeMdExcludes` | array | - | 加载 [memory](https://code.claude.com/docs/en/memory) 时要跳过的 `CLAUDE.md` 文件的 glob 模式或绝对路径。模式与绝对文件路径匹配。仅适用于 user、project 和 local memory；managed policy 文件不可排除。示例：`["**/vendor/**/CLAUDE.md"]` |
| `claudeMd` | string | - | **（仅限 Managed）** 作为组织管理的 [memory](https://code.claude.com/docs/en/memory) 注入的 CLAUDE.md 风格指令。仅在 managed 或 policy settings 中设置时生效；在 user、project、local settings 中被忽略。示例：`"Always run make lint before committing."` |
| `cleanupPeriodDays` | number | `30` | 启动清理扫描的年龄截止（最小 1）。删除不活跃的 session transcript 和孤立的 subagent worktree；截至 v2.1.117，扫描还涵盖 `~/.claude/tasks/`、`~/.claude/shell-snapshots/` 和 `~/.claude/backups/`。设置为 `0` 会被拒绝并产生验证错误。要在非交互模式（`-p`）中禁用 transcript 写入，使用 `--no-session-persistence` 或 `persistSession: false` SDK 选项 |
| `autoUpdatesChannel` | string | `"latest"` | 发布 channel：`"stable"` 或 `"latest"` |
| `minimumVersion` | string | - | 阻止 auto-updater 降级到低于特定版本。在切换到 stable channel 并选择保持当前版本直到 stable 追上来时自动设置。与 `autoUpdatesChannel` 一起使用 |
| `alwaysThinkingEnabled` | boolean | `false` | 默认对所有 session 启用 extended thinking |
| `skipWebFetchPreflight` | boolean | `false` | 跳过 WebFetch 域名安全检查，该检查在获取前将每个请求的主机名发送到 `api.anthropic.com`。在阻止到 Anthropic 的出站流量的环境中设置为 `true`，例如具有限制性出口的 Bedrock、Vertex AI 或 Foundry 部署 |
| `availableModels` | array | - | 限制用户可以通过 `/model`、`--model`、Config 工具或 `ANTHROPIC_MODEL` 选择的模型。不影响 Default 选项。截至 v2.1.172，还约束 subagent dispatching 和 `advisorModel` 选择器的模型选择器。使用 `enforceAvailableModels: true` 进一步约束 Default 模型选项。示例：`["sonnet", "haiku"]` |
| `enforceAvailableModels` | boolean | `false` | **（仅限 Managed）** 当为 `true` 时，`availableModels` allowlist 也约束 Default 模型选项——用户即使通过 Default 槽位也不能选择 allowlist 之外的模型。没有此 flag 时，`availableModels` 不对 Default 选项施加限制。与 `availableModels` 配合使用以实现完整模型锁定（v2.1.175） |
| `fastModePerSessionOptIn` | boolean | `false` | 要求用户每个 session 主动选择加入 fast mode |
| `defaultShell` | string | `"bash"` | 输入框 `!` 命令的默认 shell。接受 `"bash"`（默认）或 `"powershell"`。设置 `"powershell"` 将在 Windows 上通过 PowerShell 路由交互式 `!` 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`（v2.1.84）。**v2.1.120:** 当 PowerShell 可用时，即使没有安装 Git for Windows，它也会在 Windows 上用作回退 shell。**v2.1.126:** 当启用 PowerShell 时，它被视为*主要* shell 而非默认使用 Bash。PowerShell 7 检测现在还涵盖 Microsoft Store 安装、不在 PATH 上的 MSI 安装和 `.NET` global tool 安装 |
| `includeGitInstructions` | boolean | `true` | 在 Claude 的 system prompt 中包含内置的 commit 和 PR workflow 指令以及 git status 快照。设置时，`CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` 环境变量优先于此 setting |
| `voice` | object | - | 语音听写配置。包含三个字段的对象：`enabled`（boolean——push-to-talk 开/关）、`mode`（string——`"hold"` 按住说话或 `"tap"` 点击切换）和 `autoSubmit`（boolean——听写结束时立即提交 transcript）。运行 `/voice` 时自动写入。需要 Claude.ai 账号（v2.1.118 扩展了结构） |
| `voiceEnabled` | boolean | - | **DEPRECATED** — `voice.enabled` 的旧版别名。改用 `voice` 对象以获取 `mode` 和 `autoSubmit` 控件 |
| `showClearContextOnPlanAccept` | boolean | `false` | 在 plan accept 屏幕上显示 "clear context" 选项。设置为 `true` 以恢复该选项（自 v2.1.81 起默认隐藏） |
| `viewMode` | string | - | 启动时的默认 transcript view mode：`"default"`、`"verbose"` 或 `"focus"`。设置时覆盖 sticky Ctrl+O 选择 |
| `disableDeepLinkRegistration` | string | - | 设置为 `"disable"` 以阻止 Claude Code 在启动时向操作系统注册 `claude-cli://` 协议处理程序。Deep link 允许外部工具通过 `claude-cli://open?q=...` 使用预填充的 prompt 打开 Claude Code session。`q` 参数支持使用 URL 编码换行符（`%0A`）的多行 prompt。在协议处理程序注册受限或单独管理的环境中很有用 |
| `showThinkingSummaries` | boolean | `false` | 在交互 session 中显示 extended thinking summary。不设置或 `false`（交互模式默认）时，thinking block 会被 API 编辑并显示为折叠存根。编辑只改变您看到的内容，而不改变模型生成的内容——要减少 thinking 开销，请降低 budget 或禁用 thinking。非交互模式（`-p`）和 SDK 调用者始终接收 summary，无论此 setting 如何 |
| `disableSkillShellExecution` | boolean | `false` | 禁用来自 user、project、plugin 或 additional-directory 源的 skill 和自定义 command 中的 inline shell 执行（`` !`...` `` 和 `` ```! `` block）。命令被替换为 `[shell command execution disabled by policy]` 而不是运行。内置和 managed skill 不受影响（v2.1.91） |
| `maxSkillDescriptionChars` | number | `1536` | 每个 skill 的 `description` 和 `when_to_use` 文本在 Claude 每轮看到的 [skill listing](https://code.claude.com/docs/en/skills) 中的组合字符上限。超过此长度的文本将被截断（v2.1.105） |
| `skillListingBudgetFraction` | number | `0.01` | 为 Claude 每轮看到的 [skill listing](https://code.claude.com/docs/en/skills) 保留的模型 context window 比例（`0.01` = 1%）。当 listing 超过 budget 时，使用最少的 skill 的描述会折叠为仅名称，以便 Claude 仍能调用它们但看不到原因（v2.1.105） |
| `forceRemoteSettingsRefresh` | boolean | `false` | **（仅限 Managed）** 在远程 managed settings 被新鲜获取之前阻止 CLI 启动。如果获取失败，CLI 退出（fail-closed）。在企业环境中使用，其中策略执行必须在任何 session 开始之前是最新的（v2.1.92） |
| `wslInheritsWindowsSettings` | boolean | `false` | **（仅限 Windows managed settings）** 当为 `true` 时，WSL 上的 Claude Code 除了 `/etc/claude-code` 之外还从 Windows 策略链（HKLM registry + `C:\Program Files\ClaudeCode\managed-settings.json`）读取 managed settings，Windows 源优先。仅在 HKLM registry key 或 `C:\Program Files\ClaudeCode\managed-settings.json` 中设置时生效，这两者都需要 Windows 管理员权限才能写入。要使 HKCU 策略也适用于 WSL，该 flag 还必须另外在 HKCU 本身中设置。对原生 Windows 没有影响（v2.1.118） |
| `tui` | string | `"default"` | 渲染模式：`"fullscreen"` 或 `"default"`。通过 `/tui fullscreen` 设置以实现无闪烁 alt-screen 渲染（v2.1.110） |
| `awaySummaryEnabled` | boolean | `true` | 当用户离开后返回时生成 "away summary"（空闲 session 回顾）。设置为 `false` 可退出。与 `CLAUDE_CODE_ENABLE_AWAY_SUMMARY` env var 配合使用（v2.1.110） |
| `autoCompactEnabled` | boolean | `true` | 当 context 接近模型限制时自动压缩对话。设置为 `false` 禁用自动压缩并手动管理 context。也可通过 `DISABLE_AUTO_COMPACT` env var 禁用 |
| `skillOverrides` | object | - | 按 skill 名称的每 skill 可见性覆盖。值为 `"on"`（完整）、`"name-only"`（可见但不自动描述）、`"user-invocable-only"`（对模型发现隐藏但仍可通过 slash 调用）或 `"off"`（完全隐藏）。示例：`{"legacy-context": "name-only", "deploy": "off"}`（v2.1.129） |
| `disableRemoteControl` | boolean | `false` | 禁用 [Remote Control](https://code.claude.com/docs/en/remote-control)：阻止 `claude remote-control`、`--remote-control` flag、自动启动和 session 内切换。通常放在 managed settings 中用于按设备 MDM 执行，但可以从任何 scope 生效（v2.1.128） |
| `agentPushNotifEnabled` | boolean | `false` | 当 Claude 决定推送时（例如任务完成），向 [Remote Control](https://code.claude.com/docs/en/remote-control) 发送主动推送通知。在 `/config` 中显示为 **Push when Claude decides** |
| `inputNeededNotifEnabled` | boolean | `false` | 当权限提示或问题等待用户输入时，向 [Remote Control](https://code.claude.com/docs/en/remote-control) 发送推送通知。在 `/config` 中显示为 **Push when actions required** |
| `remoteControlAtStartup` | boolean/null | - | 在启动时自动连接 [Remote Control](https://code.claude.com/docs/en/remote-control)。`true` 始终自动连接，`false` 永不自动连接，未设置则使用组织默认值。可以在任何 scope 设置（v2.1.119+） |
| `disableAgentView` | boolean | `false` | 设置为 `true` 以关闭 [后台 agent 和 agent view](https://code.claude.com/docs/en/agent-view)：`claude agents`、`--bg`、`/background` 和按需 supervisor。可以在任何 scope 设置，但通常放在 managed settings 中。等效于将 `CLAUDE_CODE_DISABLE_AGENT_VIEW` env var 设置为 `1` |
| `disableWorkflows` | boolean | `false` | 设置为 `true` 以禁用 [dynamic workflows](https://code.claude.com/docs/en/workflows)（`/workflows`）和内置的 workflow slash command。可以在任何 scope 设置。等效于 `CLAUDE_CODE_DISABLE_WORKFLOWS` env var。Workflow 在 v2.1.154 中引入 |
| `workflowKeywordTriggerEnabled` | boolean | `true` | 在 prompt 中输入单词 "ultracode" 是否触发 [dynamic workflow](https://code.claude.com/docs/en/workflows)。设置为 `false` 需要明确的 `/workflows` 调用。Ultracode、`/workflows` 和已保存的 workflow command 不受此 setting 影响。在 `/config` 中显示为 **Workflow keyword trigger**（v2.1.157；trigger 关键词从 workflow 更名为 ultracode 在 v2.1.160） |
| `ultracode` | boolean | - | **（仅限 Session——不持久化）** 当为 `true` 时，harness 默认对每个实质性任务编写并运行 workflow，最大化彻底性而不考虑 token 成本。出现在官方 "Available settings" 列表中，但作用域限定于 session：通过 `/effort ultracode`、`--settings` 或 SDK 设置，而非写入 `settings.json`（v2.1.154） |
| `disableBundledSkills` | boolean | `false` | 对模型隐藏 Claude Code 的内置能力（bundled skill）。当为 `true` 时，模型不能调用内置 skill。与 `CLAUDE_CODE_DISABLE_BUNDLED_SKILLS` env var 配合使用。当需要严格的 plugin-only 定制时很有用（v2.1.169） |
| `disableArtifact` | boolean | `false` | 禁用 Artifact web publishing 工具。当为 `true` 时，Claude 不能创建或发布 web artifact。可以在任何 scope 设置 |
| `feedbackSurveyRate` | number | - | 当符合条件时 session 质量调查出现的概率（0–1）。企业管理员可以控制调查显示的频率。示例：`0.05` = 5% 的符合条件 session |
| `advisorModel` | string | - | 服务端 advisor 工具的模型。接受模型别名（`opus`、`sonnet`、`fable`）或完整 model ID。未设置时，advisor 使用 session 模型。需要 v2.1.98+ |
| `respondToBashCommands` | boolean | `true` | 在 `!` shell 命令完成后 Claude 是否自动响应。设置为 `false` 以禁用 `!` bash 命令完成时的自动跟进响应（v2.1.186） |

**Example:**
```json
{
  "model": "opus",
  "agent": "code-reviewer",
  "language": "japanese",
  "cleanupPeriodDays": 60,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true
}
```

### Plans & Memory Directories

将 plan 和 auto-memory 文件存储在自定义位置。

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `plansDirectory` | string | `~/.claude/plans` | `/plan` 输出存储的目录 |
| `autoMemoryDirectory` | string | - | 用于 auto-memory 存储的自定义目录。接受 `~/` 扩展路径。不接受在 project settings（`.claude/settings.json`）中设置，以防止重定向 memory 写入到敏感位置；从 policy、local 和 user settings 接受 |
| `autoMemoryEnabled` | boolean | `true` | 启用 [auto memory](https://code.claude.com/docs/en/memory)。当为 `false` 时，Claude 不从 auto-memory 目录读取或写入。也可以在 session 期间通过 `/memory` 切换，或通过 `CLAUDE_CODE_DISABLE_AUTO_MEMORY` env var 禁用 |
| `fileCheckpointingEnabled` | boolean | `true` | 在每次编辑前快照文件，以便您可以使用 `/rewind` 恢复。设置为 `false` 跳过 checkpointing 并节省磁盘空间。也可通过 `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING` env var 禁用 |

**Example:**
```json
{
  "plansDirectory": "./my-plans"
}
```

**Use Case:** 用于将规划 artifacts 与 Claude 的内部文件分开组织，或将 plan 保留在共享团队位置。

### Worktree Settings

配置 `--worktree` 如何创建和管理 git worktree。用于减少大型 monorepo 中的磁盘使用和启动时间。

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `worktree.symlinkDirectories` | array | `[]` | 从主 repository symlink 到每个 worktree 的目录，以避免在磁盘上复制大型目录 |
| `worktree.sparsePaths` | array | `[]` | 通过 git sparse-checkout（cone mode）在每个 worktree 中检出的目录。只有列出的路径被写入磁盘 |
| `worktree.baseRef` | string | `"fresh"` | 新 worktree 从哪个 ref 分支。`"fresh"` 从 `origin/<default-branch>` 分支以获得匹配远程的干净树。`"head"` 从当前本地 `HEAD` 分支，包括未提交但已跟踪的更改（v2.1.133） |
| `worktree.bgIsolation` | string | `"worktree"` | [后台 session](https://code.claude.com/docs/en/agent-view) 的隔离模式。`"worktree"`（默认）阻止在主检出中 `Edit`/`Write` 直到调用 `EnterWorktree`；`"none"` 允许后台作业直接编辑工作副本（v2.1.143） |

**Example:**
```json
{
  "worktree": {
    "symlinkDirectories": ["node_modules", ".cache"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  }
}
```

### Attribution Settings

自定义 git commit 和 pull request 的 attribution 消息。

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `attribution.commit` | string | Co-authored-by | Git commit attribution（支持 trailers） |
| `attribution.pr` | string | Generated message | Pull request 描述 attribution |
| `attribution.sessionUrl` | boolean | `true` | 在 web session 或 Remote Control session 生成的 commit 和 PR 中包含指向 claude.ai session URL 的链接。设置为 `false` 以省略链接。在本地 CLI session 中无效（v2.1.183） |
| `prUrlTemplate` | string | - | URL template，控制 commit attribution 中的 "PR" badge 如何链接到 pull request UI。支持 repo host、owner、repo 和 PR number 的占位符。用于自托管 GitLab/Bitbucket/GitHub Enterprise 实例，其中默认 `https://github.com/...` URL 不适用（v2.1.119） |
| `includeCoAuthoredBy` | boolean | `true` | **DEPRECATED** - 请改用 `attribution` |

**Example:**
```json
{
  "attribution": {
    "commit": "Generated with AI\n\nCo-Authored-By: Claude <noreply@anthropic.com>",
    "pr": "Generated with Claude Code"
  }
}
```

**Note:** 设置为空字符串（`""`）以完全隐藏 attribution。

### Authentication Helpers

用于动态身份验证 token 生成的脚本。

| Key | Type | Description |
|-----|------|-------------|
| `apiKeyHelper` | string | 输出 auth token（作为 `X-Api-Key` header 发送）的 shell script 路径 |
| `forceLoginMethod` | string | 限制登录到 `"claudeai"` 或 `"console"` 账号 |
| `forceLoginOrgUUID` | string \| array | 要求登录属于特定组织。接受单个 UUID 字符串（也会在登录期间预选该组织）或 UUID 数组，其中任何列出的组织都被接受而不预选。在 managed settings 中设置时，如果认证账号不属于列出的组织，登录将失败；空数组 fail closed 并以配置错误消息阻止登录 |
| `gcpAuthRefresh` | string | 在 GCP Application Default Credentials 过期或无法加载时刷新它们的自定义脚本。Claude Code 在重试认证之前运行。当 ADC 是短期的且需要 org 特定的 helper 来续期时很有用。示例：`"gcloud auth application-default login"` |

**Example:**
```json
{
  "apiKeyHelper": "/bin/generate_temp_api_key.sh",
  "forceLoginMethod": "console",
  "forceLoginOrgUUID": ["xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy"]
}
```

### Company Announcements

在启动时向用户显示自定义公告（随机循环）。

| Key | Type | Description |
|-----|------|-------------|
| `companyAnnouncements` | array | 在启动时显示的字符串数组 |

**Example:**
```json
{
  "companyAnnouncements": [
    "Welcome to Acme Corp!",
    "Remember to run tests before committing!",
    "Check the wiki for coding standards"
  ]
}
```

---

## Permissions

控制 Claude 可以执行哪些工具和操作。

### Permission Structure

```json
{
  "permissions": {
    "allow": [],
    "ask": [],
    "deny": [],
    "additionalDirectories": [],
    "defaultMode": "acceptEdits",
    "disableBypassPermissionsMode": "disable"
  }
}
```

### Permission Keys

| Key | Type | Description |
|-----|------|-------------|
| `permissions.allow` | array | 允许无需提示即可使用工具的规则 |
| `permissions.ask` | array | 需要用户确认的规则 |
| `permissions.deny` | array | 阻止工具使用的规则（最高优先级） |
| `permissions.additionalDirectories` | array | Claude 可以访问的额外目录 |
| `permissions.defaultMode` | string | 默认 permission mode。在 Remote 环境中，仅 `acceptEdits` 和 `plan` 被接受（v2.1.70+） |
| `permissions.disableBypassPermissionsMode` | string | 阻止 bypass mode 激活 |
| `permissions.skipDangerousModePermissionPrompt` | boolean | 在通过 `--dangerously-skip-permissions` 或 `defaultMode: "bypassPermissions"` 进入 bypass permissions mode 之前跳过显示的确认 prompt。在 project settings（`.claude/settings.json`）中设置时被忽略，以防止不受信任的 repository 自动绕过提示 |
| `allowManagedPermissionRulesOnly` | boolean | **（仅限 Managed）** 仅 managed permission rules 适用；user/project 的 `allow`、`ask`、`deny` 规则被忽略 |
| `autoMode` | object | 自定义 [auto mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) 分类器阻止和允许的内容。包含 `environment`（受信任的基础设施描述）、`allow`（block 规则的例外）、`soft_deny`（block 规则）和 `hard_deny`（无条件 block 规则——不能被 `allow` 例外或 `$defaults` sentinel 覆盖，v2.1.136）——所有数组都是纯文本字符串。还接受 `classifyAllShell`（boolean，默认 `false`）：当为 `true` 时，将所有 Bash/PowerShell 命令路由通过 auto-mode 分类器，而非仅任意代码执行模式，以更多的分类器调用成本提供更严格的覆盖（v2.1.193）。**不从共享 project settings**（`.claude/settings.json`）读取以防止 repo injection。可在 user、local 和 managed settings 中使用。设置 `allow` 或 `soft_deny` **替换**该部分的整个默认列表，除非您在数组中包含字面字符串 `"$defaults"`——sentinel 在该位置继承内置规则，以便自定义条目与它们一起添加（v2.1.118）。运行 `claude auto-mode defaults` 在自定义之前查看内置规则 |
| `disableAutoMode` | string | 设置为 `"disable"` 以阻止 [auto mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) 被激活。从 `Shift+Tab` 循环中移除 `auto`，并在启动时拒绝 `--permission-mode auto`。可以在任何 settings 级别设置；在 managed settings 中最有用，用户无法覆盖 |
| `useAutoModeDuringPlan` | boolean | plan mode 在 auto mode 可用时是否使用 auto mode 语义。默认：`true`。不从共享 project settings（`.claude/settings.json`）读取。在 `/config` 中显示为 "Use auto mode during plan" |

### Permission Modes

| Mode | Behavior |
|------|----------|
| `"default"` | 标准权限检查，带提示 |
| `"acceptEdits"` | 自动接受工作目录或 `additionalDirectories` 中路径的文件编辑**和常见文件系统命令**（`mkdir`、`touch`、`mv`、`cp` 等）。**v2.1.160:** 在写入授予代码执行权限的 build-tool 配置文件（`.npmrc`、`.yarnrc*`、`bunfig.toml`、`.bazelrc`、`.pre-commit-config.yaml`、`.devcontainer/` 等）和写入 shell startup 文件（`.zshenv`、`.zlogin`、`.bash_login`）以及 `~/.config/git/` 之前始终提示 |
| `"dontAsk"` | 自动拒绝工具，除非通过 `/permissions` 或 `permissions.allow` 规则预批准 |
| `"bypassPermissions"` | 跳过所有权限检查（危险）。所有基于路径的提示都被跳过——对 `.git`、`.config/git`、`.claude`、`.vscode`、`.idea`、`.husky`、`.cargo`、`.devcontainer`、`.yarn` 和 `.mvn` 的写入不再提示（**v2.1.121** 豁免了 `.claude/commands/`、`.claude/agents/`、`.claude/skills/` 和 `.claude/worktrees/`；**v2.1.126** 移除了所有剩余的基于路径的提示）。只有针对文件系统根目录或主目录的删除（`rm -rf /`、`rm -rf ~`）仍然提示，作为模型错误的 circuit breaker |
| `"auto"` | 通过后台安全检查自动批准工具调用，验证操作是否与您的请求一致。研究预览。分类器自动批准只读和文件编辑；将其他所有内容通过安全检查。在 3 次连续或 20 次总阻止后回退到提示。自 v2.1.111 起在默认 `Shift+Tab` permission-mode 循环中（`--enable-auto-mode` flag 在 v2.1.111 中移除——使用 `--permission-mode auto` 以此模式启动）。使用 `autoMode` setting 进行配置 |
| `"plan"` | 只读探索模式。截至 v2.1.136，即使存在匹配的 `Edit(...)` allow rule，文件写入也被阻止——plan mode 现在覆盖明确的 allow rules 以维持其只读保证 |

### Tool Permission Syntax

| Tool | Syntax | Examples |
|------|--------|----------|
| `Bash` | `Bash(command pattern)` | `Bash(npm run *)`、`Bash(* install)`、`Bash(git * main)` |
| `PowerShell` | `PowerShell(cmd *)` | `PowerShell(Get-ChildItem *)`、`PowerShell(git commit *)`——与 Bash 相同的形状；常见别名被规范化（`gci`/`ls`/`dir` → `Get-ChildItem`）并且 PowerShell AST 被解析，所以 `|`/`;`/`&&`/`||` 链的每个子命令都必须匹配 |
| `Read` | `Read(path pattern)` | `Read(.env)`、`Read(./secrets/**)` |
| `Edit` | `Edit(path pattern)` | `Edit(src/**)`、`Edit(*.ts)` |
| `Write` | `Write(path pattern)` | `Write(*.md)`、`Write(./docs/**)` |
| `NotebookEdit` | `NotebookEdit(pattern)` | `NotebookEdit(*)` |
| `WebFetch` | `WebFetch(domain:pattern)` | `WebFetch(domain:example.com)` |
| `WebSearch` | `WebSearch` | 全局 web search |
| `Task` | `Task(agent-name)` | `Task(Explore)`、`Task(my-agent)` |
| `Agent` | `Agent(name)` | `Agent(researcher)`、`Agent(*)`——权限作用域限定于 subagent spawning |
| `Skill` | `Skill(skill-name)` 或 `Skill(prefix *)` | `Skill(weather-fetcher)`、`Skill(weather *)` 匹配 `weather-fetcher`/`weather-svg-creator`（v2.1.139） |
| `MCP` | `mcp__server__tool` 或 `MCP(server:tool)` | `mcp__memory__*`、`MCP(github:*)` |
| `Tool` | `Tool(param:value)` | `Agent(model:opus)`、`Bash(cmd:npm run *)`——将 permission rules 针对工具的输入参数进行匹配；在 value 位置支持 `*` 通配符（v2.1.178） |
| `Cd` | `Cd(path pattern)` | `Cd(/home/*)`、`Cd(~/projects/*)`——控制 `/cd` 命令可以导航到哪些目录 |

**Evaluation order:** 规则按顺序评估：首先是 deny rules，然后是 ask，然后是 allow。第一个匹配的规则胜出。

**Deny rule glob patterns (v2.1.166):** 在 `deny` 规则中，在 tool-name 位置使用 `"*"` 匹配所有工具——等效于全局 deny。例如，deny 数组中的 `"*"` 阻止每个工具调用。这使得完全锁定访问权限并划分特定的 allow/ask 例外成为可能。

**Read/Edit path patterns:** `Read`、`Edit` 和 `Write` 的 permission rules 支持 gitignore 风格的模式，有四种前缀类型：

| Prefix | Meaning | Example |
|--------|---------|---------|
| `//` | 从文件系统根目录的绝对路径 | `Read(//Users/alice/file)` |
| `~/` | 相对于主目录 | `Read(~/.zshrc)` |
| `/` | 相对于项目根目录 | `Edit(/src/**)` |
| `./` 或无 | 相对路径（当前目录） | `Read(.env)`、`Read(*.ts)` |

**Symlink resolution:** Permission rules 同时检查 symlink 路径和其解析目标。**Allow** 规则仅在 symlink *和*其目标都匹配时适用——允许目录内的指向外部的 symlink 仍然提示。**Deny** 规则在 symlink *或*其目标匹配时适用——指向被拒绝文件的 symlink 本身也被拒绝。

**Bash wildcard notes:**
- `*` 可以出现在**任何位置**：前缀（`Bash(* install)`）、后缀（`Bash(npm *)`）或中间（`Bash(git * main)`）
- **Word boundary:** `Bash(ls *)`（`*` 前有空格）匹配 `ls -la` 但**不**匹配 `lsof`；`Bash(ls*)`（无空格）匹配两者
- `Bash(*)` 被视为等效于 `Bash`（匹配所有 bash 命令）
- Permission rules 支持输出重定向：`Bash(python:*)` 匹配 `python script.py > output.txt`
- 旧版 `:*` 后缀语法（例如 `Bash(npm:*)`）等效于 ` *` 但已弃用
- **Compound commands:** shell 操作符（`&&`、`||`、`;`、`|`、`|&`、`&` 和换行符）拆分命令，每个子命令必须独立匹配——`Bash(safe-cmd *)` **不**授权 `safe-cmd && other-cmd`
- **Process wrappers:** `timeout`、`time`、`nice`、`nohup` 和 `stdbuf` 在匹配前被剥离（因此 `Bash(npm test *)` 也匹配 `timeout 30 npm test`）；裸 `xargs`（无 flags）也会被剥离。Exec wrapper `watch`、`setsid`、`ionice`、`flock` 和带 `-exec`/`-delete` 的 `find` 始终提示，不能由前缀规则批准

**Example:**
```json
{
  "permissions": {
    "allow": [
      "Edit(*)",
      "Write(*)",
      "Bash(npm run *)",
      "Bash(git *)",
      "WebFetch(domain:*)",
      "mcp__*"
    ],
    "ask": [
      "Bash(rm *)",
      "Bash(git push *)"
    ],
    "deny": [
      "Read(.env)",
      "Read(./secrets/**)",
      "Bash(curl *)"
    ],
    "additionalDirectories": ["../shared-libs/"]
  }
}
```

---

## Hooks

Hook configuration（events、properties、matchers、exit codes、environment variables 和 HTTP hooks）在专门的 repository 中维护：

> **[claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks)** — 完整的 hook 参考，包含声音通知系统、全部 25 个 hook event、HTTP hooks、matcher 模式、exit codes 和 environment variables。

Hook 相关的 settings keys（`hooks`、`disableAllHooks`（也会禁用任何自定义 status line）、`allowManagedHooksOnly`、`allowedHttpHookUrls`、`httpHookAllowedEnvVars`）记录在那里。

有关官方 hooks 参考，请参见 [Claude Code Hooks Documentation](https://code.claude.com/docs/en/hooks)。

---

## MCP Servers

配置 Model Context Protocol server 以扩展能力。

> **OAuth (v2.1.111):** 通过 OAuth 进行身份验证的 MCP server 遵循 [RFC 9728](https://datatracker.ietf.org/doc/rfc9728/) 的 protected-resource metadata discovery。合规的 server 在 `/.well-known/oauth-protected-resource` 下暴露授权 endpoint，Claude Code 自动完成 OAuth 流程——无需为符合规范的 server 手动编写 `apiKeyHelper` 或 `headersHelper` script。

> **Reserved server name (v2.1.128):** `workspace` 是预留的 MCP server 名称。具有此名称的用户定义 server 在加载时被跳过，并在 session log 中记录警告。重命名任何预先存在的 `workspace` server 以避免冲突。

> **`.mcp.json` hot-reload (v2.1.139):** `/mcp` Reconnect 操作现在在重新连接之前从磁盘重新读取 `.mcp.json`，因此添加或编辑 server 不再需要 restart session。Claude Code 还会将 `CLAUDE_PROJECT_DIR` 注入到通过 stdio 启动的 MCP server 环境中（v2.1.139），以便 server 可以解析相对于项目根目录的路径。

> **Per-server timeout floor (v2.1.162):** 每个 server 的 `timeout` 值小于 1000ms 的将被忽略，改为应用全局 `MCP_TOOL_TIMEOUT` 默认值。大于等于 1000ms 的值按之前的方式接受。

### MCP Settings

| Key | Type | Scope | Description |
|-----|------|-------|-------------|
| `enableAllProjectMcpServers` | boolean | Any | 自动批准所有 `.mcp.json` server |
| `enabledMcpjsonServers` | array | Any | 特定 server 名称的 allowlist |
| `disabledMcpjsonServers` | array | Any | 特定 server 名称的 blocklist |
| `allowedMcpServers` | array | Managed only | 带 name/command/URL 匹配的 allowlist |
| `deniedMcpServers` | array | Managed only | 带匹配的 blocklist |
| `allowManagedMcpServersOnly` | boolean | Managed only | 仅允许在 managed allowlist 中明确列出的 MCP server |
| `channelsEnabled` | boolean | Managed only | 允许 Team 和 Enterprise 用户使用 [channels](https://code.claude.com/docs/en/channels)。未设置或 `false` 时，channel 消息传递被阻止，无论 `--channels` flag 如何 |
| `allowedChannelPlugins` | array | Managed only | 可以推送消息的 channel plugin 的 allowlist。设置时替换默认 Anthropic allowlist。未定义 = 回退到默认值，空数组 = 阻止所有 channel plugin。需要 `channelsEnabled: true`。每个条目是一个具有 `marketplace` 和 `plugin` 字段的对象（v2.1.84） |
| `allowAllClaudeAiMcps` | boolean | Managed only | 在 `managed-mcp.json` 旁边加载 claude.ai cloud MCP connector。启用时，claude.ai 托管的 MCP connector 在管理员部署的 managed MCP server 之外也可用 |
| `disableClaudeAiConnectors` | boolean | Any | 禁用 claude.ai MCP connector 的自动获取。当为 `true` 时，无论任何 `allowAllClaudeAiMcps` 策略如何，claude.ai cloud connector 都不会被加载（v2.1.182） |

### MCP Server Matching (Managed Settings)

```json
{
  "allowedMcpServers": [
    { "serverName": "github" },
    { "serverCommand": "npx @modelcontextprotocol/*" },
    { "serverUrl": "https://mcp.company.com/*" }
  ],
  "deniedMcpServers": [
    { "serverName": "dangerous-server" }
  ]
}
```

### Per-Server Tool Loading (`alwaysLoad`, v2.1.121)

默认情况下，MCP 工具定义是延迟的（按需通过 tool search 加载到 context）。在 `.mcp.json`（或 inline `mcpServers`）中的单个 MCP server 条目上设置 `alwaysLoad: true` 以将该 server 从延迟中豁免——该 server 的每个工具然后在 session 开始时预先加载，无论 `ENABLE_TOOL_SEARCH`。适用于所有 server 类型；需要 Claude Code v2.1.121+。仅对每轮都需要的一小组工具使用此选项——每个预先加载的工具消耗本可用于对话的 context。

```json
{
  "mcpServers": {
    "always-on-server": {
      "type": "http",
      "url": "https://mcp.example.com",
      "alwaysLoad": true
    }
  }
}
```

MCP server 还可以通过在工具的 `_meta` 对象中包含 `"anthropic/alwaysLoad": true` 将单个工具标记为始终加载——当仅 server 的部分工具应绕过延迟时很有用。

**Example:**
```json
{
  "enableAllProjectMcpServers": true,
  "enabledMcpjsonServers": ["memory", "github", "filesystem"],
  "disabledMcpjsonServers": ["experimental-server"]
}
```

---

## Sandbox

配置 bash 命令 sandboxing 以确保安全。

### Sandbox Settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `sandbox.enabled` | boolean | `false` | 启用 bash sandboxing |
| `sandbox.failIfUnavailable` | boolean | `false` | 当 sandbox 启用但无法启动时以错误退出，而不是在无 sandbox 的情况下运行。用于需要严格 sandboxing 的企业策略（v2.1.83） |
| `sandbox.autoAllowBashIfSandboxed` | boolean | `true` | 在 sandboxed 时自动批准 bash。截至 v2.1.139，shell expansion 形式（`$VAR`、`$(cmd)`）被正确识别，因此包含变量替换的命令在启用 sandbox 自动批准时不再回退到提示 |
| `sandbox.excludedCommands` | array | `[]` | 在 sandbox 外运行的命令 |
| `sandbox.allowUnsandboxedCommands` | boolean | `true` | 允许 `dangerouslyDisableSandbox`。设置为 `false` 时，逃生舱口完全禁用，所有命令必须 sandboxed（或在 `excludedCommands` 中）。用于需要严格 sandboxing 的企业策略 |
| `sandbox.ignoreViolations` | object | `{}` | 命令模式到路径数组的映射——抑制违规警告 *(在 JSON schema 中，不在官方 settings 页面上)* |
| `sandbox.enableWeakerNestedSandbox` | boolean | `false` | **（仅限 Linux 和 WSL2）** 为无特权 Docker 环境启用较弱的 sandbox（降低安全性） |
| `sandbox.network.allowUnixSockets` | array | `[]` | **（仅限 macOS）** sandbox 中可访问的特定 Unix socket 路径。在 Linux 和 WSL2 上被忽略，因为 seccomp filter 无法检查 socket 路径；改用 `allowAllUnixSockets` |
| `sandbox.network.allowAllUnixSockets` | boolean | `false` | 允许所有 Unix socket（覆盖 `allowUnixSockets`）。在 Linux 和 WSL2 上，这是允许 Unix socket 的唯一方式，因为它跳过了 otherwise 会阻止 `socket(AF_UNIX, ...)` 调用的 seccomp filter |
| `sandbox.network.allowLocalBinding` | boolean | `false` | 允许绑定到 localhost 端口（macOS） |
| `sandbox.network.allowedDomains` | array | `[]` | sandbox 的网络域 allowlist |
| `sandbox.network.deniedDomains` | array | `[]` | bash sandbox 的网络域 denylist。优先于 `allowedDomains` 中的通配符。支持 glob 模式（例如 `"*.example.com"`）（v2.1.113） |
| `sandbox.network.httpProxyPort` | number | - | HTTP proxy 端口 1-65535（自定义 proxy） |
| `sandbox.network.socksProxyPort` | number | - | SOCKS5 proxy 端口 1-65535（自定义 proxy） |
| `sandbox.network.allowManagedDomainsOnly` | boolean | `false` | 仅允许 managed allowlist 中的域（managed settings） |
| `sandbox.network.allowMachLookup` | array | `[]` | （仅限 macOS）sandbox 可以查找的额外 XPC/Mach 服务名称。支持单个尾随 `*` 进行前缀匹配。需要用于通过 XPC 通信的工具，如 iOS Simulator 或 Playwright。示例：`["com.apple.coresimulator.*"]` |
| `sandbox.filesystem.allowWrite` | array | `[]` | sandboxed 命令可以写入的额外路径。数组跨所有 settings scope 合并。也与来自 `Edit(...)` allow permission rules 的路径合并。前缀：`/`（绝对路径）、`~/`（home）、`./` 或无（project settings 中为项目相对，user settings 中为 `~/.claude` 相对）。旧的 `//` 绝对路径前缀仍然有效。**Note:** 这与 [Read/Edit permission rules](#tool-permission-syntax) 不同，后者使用 `//` 表示绝对路径，`/` 表示项目相对 |
| `sandbox.filesystem.denyWrite` | array | `[]` | sandboxed 命令不能写入的路径。数组跨所有 settings scope 合并。也与来自 `Edit(...)` deny permission rules 的路径合并。路径前缀约定与 `allowWrite` 相同 |
| `sandbox.filesystem.denyRead` | array | `[]` | sandboxed 命令不能读取的路径。数组跨所有 settings scope 合并。也与来自 `Read(...)` deny permission rules 的路径合并。路径前缀约定与 `allowWrite` 相同 |
| `sandbox.filesystem.allowRead` | array | `[]` | 在 `denyRead` 区域内重新允许读取访问的路径。优先于 `denyRead`。数组跨所有 settings scope 合并。路径前缀约定与 `allowWrite` 相同 |
| `sandbox.filesystem.allowManagedReadPathsOnly` | boolean | `false` | **（仅限 Managed）** 仅来自 managed settings 的 `allowRead` 路径被接受。来自 user、project 和 local settings 的 `allowRead` 条目被忽略 |
| `sandbox.enableWeakerNetworkIsolation` | boolean | `false` | （仅限 macOS）允许访问系统 TLS trust（`com.apple.trustd.agent`）；降低安全性 |
| `sandbox.bwrapPath` | string | - | **（仅限 Managed，Linux/WSL2）** 指向 bubblewrap（`bwrap`）二进制文件的绝对路径。覆盖自动 `PATH` 检测。仅从 managed settings 生效，不从 user 或 project settings 生效。示例：`/opt/admin/bwrap`（v2.1.133） |
| `sandbox.socatPath` | string | - | **（仅限 Managed，Linux/WSL2）** 指向用于 sandbox network proxy 的 `socat` 二进制文件的绝对路径。覆盖自动 `PATH` 检测。仅从 managed settings 生效。示例：`/opt/admin/socat`（v2.1.133） |
| `sandbox.allowAppleEvents` | boolean | `false` | **（仅限 macOS）** 选择加入允许 sandboxed 命令发送 Apple Events。需要用于依赖 Apple Events IPC 的工具，如 `open`、`osascript` 或浏览器认证流程（v2.1.181） |
| `sandbox.credentials` | object | — | 精细控制哪些 credential 文件和 environment variables 被阻止在 sandboxed subprocess 环境中。包含 `files`（阻止 sandboxed 读取的文件路径数组）和 `envVars`（从 subprocess 环境中剥离的 env var 名称数组）。`files` 或 `envVars` 中的单个无效条目被剥离并发出警告，有效子集被强制执行（v2.1.187；在 v2.1.191+ 中扩展为具有子数组的对象） |

**Example:**
```json
{
  "sandbox": {
    "enabled": true,
    "autoAllowBashIfSandboxed": true,
    "excludedCommands": ["git", "docker", "gh"],
    "allowUnsandboxedCommands": false,
    "network": {
      "allowUnixSockets": ["/var/run/docker.sock"],
      "allowLocalBinding": true
    }
  }
}
```

---

## Plugins

配置 Claude Code plugins 和 marketplaces。

### Plugin Settings

| Key | Type | Scope | Description |
|-----|------|-------|-------------|
| `enabledPlugins` | object | Any | 启用/禁用特定 plugins |
| `extraKnownMarketplaces` | object | Project | 添加自定义 plugin marketplaces（通过 `.claude/settings.json` 团队共享） |
| `strictKnownMarketplaces` | array | Managed only | 允许的 marketplace 的 allowlist |
| `strictPluginOnlyCustomization` | boolean \| array | Managed only | 阻止来自 user 和 project 源的 skills、agents、hooks 和 MCP servers，因此它们只能来自 plugins 或 managed settings。`true` 锁定全部四个表面；数组如 `["skills", "hooks"]` 仅锁定指定的 |
| `pluginSuggestionMarketplaces` | array | Managed only | marketplace 名称的 allowlist，其 plugins 可以在 session 期间作为上下文安装建议出现。限制哪些 marketplace 可以显示 "you might want this plugin" 提示（v2.1.152） |
| `skippedMarketplaces` | array | Any | 用户拒绝安装的 marketplace *(在 JSON schema 中，不在官方 settings 页面上)* |
| `skippedPlugins` | array | Any | 用户拒绝安装的 plugins *(在 JSON schema 中，不在官方 settings 页面上)* |
| `pluginConfigs` | object | Any | 每个 plugin 的 MCP server configs（按 `plugin@marketplace` 索引）*(在 JSON schema 中，不在官方 settings 页面上)* |
| `blockedMarketplaces` | array | Managed only | 阻止特定的 plugin marketplaces。每个条目可以按 source 字符串、`hostPattern` 或 `pathPattern` 匹配——截至 v2.1.119，`hostPattern` 和 `pathPattern` 匹配器在任何下载触及文件系统之前被正确执行，因此被阻止的 marketplace 永远不会到达磁盘 |
| `pluginTrustMessage` | string | Managed only | 在提示用户信任 plugins 时显示的自定义消息 |

**Marketplace source types:** `github`、`git`、`directory`、`hostPattern`、`settings`、`url`、`npm`、`file`。使用 `source: 'settings'` inline 声明一小组 plugins 而无需设置托管 marketplace repository。

**Example:**
```json
{
  "enabledPlugins": {
    "formatter@acme-tools": true,
    "deployer@acme-tools": true,
    "experimental@acme-tools": false
  },
  "extraKnownMarketplaces": {
    "acme-tools": {
      "source": {
        "source": "github",
        "repo": "acme-corp/claude-plugins"
      }
    },
    "inline-tools": {
      "source": {
        "source": "settings",
        "name": "inline-tools",
        "plugins": [
          {
            "name": "code-formatter",
            "source": { "source": "github", "repo": "acme-corp/code-formatter" }
          }
        ]
      }
    }
  }
}
```

---

## Model Configuration

### Model Aliases

| Alias | Description |
|-------|-------------|
| `"default"` | 为您的账号类型推荐 |
| `"sonnet"` | 最新的 Sonnet 模型（Anthropic API 上为 Claude Sonnet 4.6；第三方 provider 上为 4.5） |
| `"opus"` | 最新的 Opus 模型（截至 v2.1.154，Anthropic API 上为 Claude Opus 4.8；Bedrock/Vertex/Foundry 上为 4.6）。自 v2.1.142 起也是 fast-mode 默认值。Opus 4.8 默认为 `high` effort 并支持 `/effort xhigh` |
| `"haiku"` | 快速的 Haiku 模型 |
| `"sonnet[1m]"` | 带 1M token context 的 Sonnet |
| `"opus[1m]"` | 带 1M token context 的 Opus（自 v2.1.75 起在 Max、Team 和 Enterprise 上为默认值） |
| `"opusplan"` | Opus 用于规划，Sonnet 用于执行 |
| `"fable"` | Claude Fable 5——长周期 reasoning 模型。仅限 Anthropic API（v2.1.170+）。Fable 5 默认包含 1M context；`[1m]` 后缀被自动剥离，因此 `fable[1m]` 是冗余的（v2.1.173） |

**Example:**
```json
{
  "model": "opus"
}
```

> **Note (v2.1.144):** `/model` 仅更改**当前 session** 的模型。在 `/model` 选择器中按 `d` 也将选择设置为默认值。`model` setting 和 `ANTHROPIC_MODEL` 继续控制持久化默认值。

### Model Overrides

将 Anthropic model ID 映射到 Bedrock、Vertex 或 Foundry 部署的 provider 特定 model ID。

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `effortLevel` | string | - | 跨 session 持久化 effort level。接受 `"low"`、`"medium"`、`"high"` 或 `"xhigh"`（Opus 4.7 和 4.8，v2.1.111）。运行 `/effort low`、`/effort medium`、`/effort high` 或 `/effort xhigh` 时自动写入。支持 Opus 4.6、Sonnet 4.6、Opus 4.7 和 Opus 4.8（默认为 `high`）。不支持的级别回退到 active 模型上的最高支持级别 |
| `fallbackModel` | array | - | 当主模型不可用时（例如 rate-limited 或容量问题），最多 3 个按顺序尝试的 fallback model ID。每个条目是一个 model ID 或别名。Claude Code 首先尝试主模型；如果失败，按顺序尝试每个 fallback。在第一个成功响应时停止（v2.1.166） |
| `modelOverrides` | object | - | 将模型选择器条目映射到 provider 特定的 ID（例如 Bedrock inference profile ARN）。每个 key 是模型选择器条目名称，每个值是 provider 的 model ID |

**Example:**
```json
{
  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0",
    "claude-sonnet-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-sonnet-4-6-v1:0"
  }
}
```

### Effort Level

`/model` 命令暴露了一个 **effort level** 控件，用于调整模型对每个响应应用的 reasoning 量。在 `/model` UI 中使用 ← → 箭头键循环切换 effort level。

| Effort Level | Description |
|-------------|-------------|
| Max | 最大 reasoning 深度，仅 Opus 4.6 |
| XHigh | 扩展高 reasoning 深度，Opus 4.7 和 4.8（所有 plan 上 Opus 4.7 的默认值，v2.1.111；在 Opus 4.8 上可用但默认为 `high`，v2.1.154） |
| High（Opus 4.6/Sonnet 4.6 默认） | 完整 reasoning 深度，最适合复杂任务 |
| Medium | 平衡 reasoning，适合日常任务 |
| Low | 最小 reasoning，最快响应 |

**How to use:**
1. 运行 `/effort low`、`/effort medium` 或 `/effort high` 直接设置（v2.1.76+）
2. 或运行 `/model` → 选择模型 → 使用 **← →** 箭头键调整
3. setting 通过 `settings.json` 中的 `effortLevel` key 持久化

**Note:** Effort level 适用于 Max 和 Team plan 上的 Opus 4.6、Sonnet 4.6、Opus 4.7 和 Opus 4.8。默认值在 v2.1.68 中从 High 更改为 Medium，然后在 v2.1.94 中对 API-key、Bedrock/Vertex/Foundry、Team 和 Enterprise 用户改回 **High**。在 v2.1.117 中，Pro/Max 订阅者在 Opus 4.6 和 Sonnet 4.6 上的默认值也从 `medium` 提高到 `high`，使所有层级在 `high` 上对齐。v2.1.111 引入了 **`xhigh`**（当时仅 Opus 4.7）并将其设为所有 plan 上 Opus 4.7 的默认 effort level。**v2.1.154** 将 **Opus 4.8** 添加为 Anthropic API 上的最新 Opus；它支持 `xhigh` 但默认为 `high`。截至 v2.1.75，Opus 4.6 的 1M context window 在 Max、Team 和 Enterprise plan 上默认可用。

**Effort env propagation:** 在 skill 文件内部，使用 `${CLAUDE_EFFORT}` 引用当前 effort level（v2.1.120）。截至 v2.1.133，相同的 `$CLAUDE_EFFORT` 变量也被注入到 Bash 工具 subprocess 和 hook handler 的环境中，因此 shell scripts 和 hook commands 可以根据 active effort tier 调整行为，而无需读取单独的配置文件。

### Model Environment Variables

通过 `env` key 配置：

```json
{
  "env": {
    "ANTHROPIC_MODEL": "sonnet",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "custom-haiku-model",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "custom-sonnet-model",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "custom-opus-model",
    "CLAUDE_CODE_SUBAGENT_MODEL": "haiku",
    "MAX_THINKING_TOKENS": "10000"
  }
}
```

---

## Display & UX

### Display Settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `statusLine` | object | - | 自定义 status line 配置 |
| `outputStyle` | string | `"default"` | 输出风格（例如 `"Explanatory"`） |
| `spinnerTipsEnabled` | boolean | `true` | 等待时显示提示 |
| `spinnerVerbs` | object | - | 自定义 spinner verbs，具有 `mode`（"append" 或 "replace"）和 `verbs` 数组 |
| `spinnerTipsOverride` | object | - | 自定义 spinner tips，具有 `tips`（字符串数组）和可选的 `excludeDefault`（boolean）。当 `excludeDefault` 为 `true` 时，仅显示自定义 tips；当 `false` 或缺失时，自定义 tips 与内置 tips 合并。截至 v2.1.121，`excludeDefault: true` 也抑制基于时间的 spinner tips |
| `respectGitignore` | boolean | `true` | 在 file picker 中尊重 .gitignore |
| `prefersReducedMotion` | boolean | `false` | 减少 UI 中的动画和运动效果 |
| `axScreenReader` | boolean | `false` | 启用 screen-reader 友好的输出模式。当为 `true` 时，Claude 输出平面文本，没有装饰性的 box-drawing 字符、颜色和其他终端 UI 元素。在 `/config` 中显示为 **Screen reader mode**。仅适用于 User scope——不从 project 或 managed settings 读取。也可通过 `--ax-screen-reader` CLI flag 使用（v2.1.181） |
| `syntaxHighlightingDisabled` | boolean | `false` | 禁用 diff、code block 和 file preview 中的语法高亮。与 `CLAUDE_CODE_SYNTAX_HIGHLIGHT` env var 不同，后者仅控制 diff 输出 |
| `fileSuggestion` | object | - | 自定义文件建议命令（参见下方的 File Suggestion Configuration） |
| `autoScrollEnabled` | boolean | `true` | 在 fullscreen mode 中自动滚动对话。设置为 `false` 以禁用自动滚动（v2.1.110）。v2.1.119 之前的版本将此存储在 `~/.claude.json` |
| `editorMode` | string | `"normal"` | 输入 prompt 的 key binding mode：`"normal"` 或 `"vim"`。在 `/config` 中显示为 **Editor mode**。v2.1.119 之前的版本将此存储在 `~/.claude.json` |
| `showTurnDuration` | boolean | `true` | 在响应后显示 turn duration 消息（例如 "Cooked for 1m 6s"）。v2.1.119 之前的版本将此存储在 `~/.claude.json` |
| `teammateMode` | string | `"auto"` | [agent team](https://code.claude.com/docs/en/agent-teams) teammate 的显示方式：`"auto"`（在 tmux 或 iTerm2 中选择分割窗格，否则 in-process）、`"in-process"`、`"tmux"` 或 `"iterm2"`（强制 iTerm2 分割窗格，无论自动检测如何，v2.1.186）。参见 [choose a display mode](https://code.claude.com/docs/en/agent-teams#choose-a-display-mode)。v2.1.119 之前的版本将此存储在 `~/.claude.json` |
| `terminalProgressBarEnabled` | boolean | `true` | 在支持的终端中显示终端进度条（ConEmu、Ghostty 1.2.0+ 和 iTerm2 3.6.6+）。在 `/config` 中显示为 **Terminal progress bar**。v2.1.119 之前的版本将此存储在 `~/.claude.json` |
| `preferredNotifChannel` | string | `"auto"` | 任务完成和权限提示通知的方法。值：`"auto"`、`"terminal_bell"`、`"iterm2"`、`"iterm2_with_bell"`、`"kitty"`、`"ghostty"`、`"notifications_disabled"`。默认 `"auto"` 在 iTerm2、Ghostty 和 Kitty 中发送桌面通知，在其他终端中不执行任何操作。设置 `"terminal_bell"` 在任何终端中响铃。在 `/config` 中显示为 **Notifications**。参见 [Get a terminal bell or notification](https://code.claude.com/docs/en/terminal-config#get-a-terminal-bell-or-notification) |
| `wheelScrollAccelerationEnabled` | boolean | `true` | 在 fullscreen mode 中禁用鼠标滚轮滚动加速。设置为 `false` 以使用固定的每次 tick 滚动步长而非 OS 级加速曲线（v2.1.174） |
| `footerLinksRegexes` | array | - | 用于匹配 URL 的 regex 模式，匹配的 URL 在 footer row 中显示为 link badge。每个匹配的 URL 在聊天 UI 底部生成一个可点击的 badge（v2.1.176） |

### Global Config Settings (`~/.claude.json`)

这些 IDE 相关偏好存储在 `~/.claude.json`，**而非** `settings.json`。

> **v2.1.119 migration note:** 截至 v2.1.119，`autoScrollEnabled`、`editorMode`、`showTurnDuration`、`teammateMode` 和 `terminalProgressBarEnabled` 移入 `settings.json`，并在上方的 Display Settings 表中记录。较早的版本将它们存储在这里。

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `autoConnectIde` | boolean | `false` | 当 Claude Code 从外部终端启动时自动连接到运行中的 IDE。在 `/config` 中显示为 **Auto-connect to IDE (external terminal)**，当在 VS Code 或 JetBrains 终端之外运行时 |
| `autoInstallIdeExtension` | boolean | `true` | 当从 VS Code 终端运行时自动安装 Claude Code IDE extension。在 `/config` 中显示为 **Auto-install IDE extension**。也可通过 `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` env var 禁用 |
| `externalEditorContext` | boolean | `false` | 当您使用 `Ctrl+G` 打开外部编辑器时，将 Claude 之前的响应作为 `#` 注释的 context 前置。设置为 `true` 启用 |
| `teammateDefaultModel` | string | `null` | 当 lead 分发时 [agent-team](https://code.claude.com/docs/en/agent-teams) teammate 的默认模型。`null` 继承 lead 的模型。在官方 settings 页面上列为 "Global config settings" |

### Workspace & Teams

| Key | Type | Description |
|-----|------|-------------|
| `sshConfigs` | object[] | 在 Desktop 中显示为下拉菜单的 SSH 连接定义。每个条目必须包含 `id`、`name` 和 `sshHost`；可选 `sshPort`、`sshIdentityFile` 和 `startDirectory` |

**Field reference:**

| Field | Required | Description |
|-------|----------|-------------|
| `id` | yes | SSH 连接条目的唯一标识符 |
| `name` | yes | 在 Desktop 下拉菜单中显示的显示名称 |
| `sshHost` | yes | SSH host（例如 `user@dev.example.com` 或 `dev.example.com`） |
| `sshPort` | no | SSH 端口号 |
| `sshIdentityFile` | no | SSH identity 文件（私钥）的路径 |
| `startDirectory` | no | 连接后的初始工作目录 |

**Example:**
```json
{
  "sshConfigs": [
    {
      "id": "dev-vm",
      "name": "Dev VM",
      "sshHost": "user@dev.example.com",
      "sshPort": 22,
      "sshIdentityFile": "~/.ssh/id_ed25519",
      "startDirectory": "/home/user/project"
    }
  ]
}
```

### Status Line Configuration

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh",
    "padding": 2,
    "refreshInterval": 5
  }
}
```

| Field | Description |
|-------|-------------|
| `type` | 设置为 `"command"` 以运行 shell script |
| `command` | 生成 status line 输出的 shell 命令或 script 路径 |
| `padding` | 添加到 status line 内容的额外水平间距（以字符为单位）。默认为 `0`。控制超出界面内置间距的相对缩进 |
| `refreshInterval` | 除了事件驱动更新之外，每 N 秒重新运行命令。最小值为 `1`。当 status line 显示基于时间的数据（例如时钟）或当后台 subagent 在主 session 空闲时更改 git 状态时很有用。不设置则仅在事件发生时运行（v2.1.97） |

**Status Line Input Fields:**

status line 命令通过 stdin 接收一个 JSON 对象。有关完整的 JSON schema 和示例，请参见 [Status Line Documentation](https://code.claude.com/docs/en/statusline)。

| Field | Description |
|-------|-------------|
| `model.id`、`model.display_name` | 当前模型标识符和显示名称 |
| `cwd`、`workspace.current_dir` | 当前工作目录（两者包含相同的值；推荐使用 `workspace.current_dir`） |
| `workspace.project_dir` | Claude Code 启动的目录（如果工作目录更改，可能与 `cwd` 不同） |
| `workspace.added_dirs` | 通过 `/add-dir` 或 `--add-dir` 添加的额外目录 |
| `workspace.git_worktree` | 当在通过 `git worktree add` 创建的链接 worktree 中时的 git worktree 名称。在主工作树中缺失（v2.1.97） |
| `cost.total_cost_usd` | 总 session 成本（美元） |
| `cost.total_duration_ms` | session 开始以来的总 wall-clock 时间（毫秒） |
| `cost.total_api_duration_ms` | 等待 API 响应的总时间（毫秒） |
| `cost.total_lines_added`、`cost.total_lines_removed` | session 期间更改的代码行数 |
| `context_window.total_input_tokens`、`context_window.total_output_tokens` | session 期间累计的 token 计数 |
| `context_window.context_window_size` | 最大 context window 大小（token）（默认 200000，extended context 为 1000000） |
| `context_window.used_percentage` | 预计算的 context window 使用百分比 |
| `context_window.remaining_percentage` | 预计算的 context window 剩余百分比 |
| `context_window.current_usage` | 最后一次 API 调用的 token 计数（input、output、cache tokens） |
| `exceeds_200k_tokens` | 最近的 API 响应的总 token 数是否超过 200k（固定阈值） |
| `rate_limits.five_hour.used_percentage` | 五小时 rate limit 使用百分比（v2.1.80+） |
| `rate_limits.five_hour.resets_at` | 五小时 rate limit 重置时间戳（Unix epoch seconds） |
| `rate_limits.seven_day.used_percentage` | 七天 rate limit 使用百分比 |
| `rate_limits.seven_day.resets_at` | 七天 rate limit 重置时间戳（Unix epoch seconds） |
| `session_id` | 唯一 session 标识符 |
| `session_name` | 使用 `--name` 或 `/rename` 设置的自定义 session 名称。未设置自定义名称时缺失 |
| `transcript_path` | 对话 transcript 文件路径 |
| `version` | Claude Code 版本 |
| `output_style.name` | 当前 output style 的名称 |
| `vim.mode` | 启用 vim mode 时的当前 vim mode（`NORMAL` 或 `INSERT`） |
| `agent.name` | 使用 `--agent` flag 或 agent settings 运行时的 agent 名称 |
| `effort.level` | 当前 reasoning effort（`low`、`medium`、`high`、`xhigh` 或 `max`）。反映实时 session 值，包括 session 中的 `/effort` 更改。当前模型不支持 effort 参数时缺失（v2.1.121） |
| `thinking.enabled` | session 是否启用 extended thinking（v2.1.121） |
| `worktree.name` | active worktree 名称（仅在 `--worktree` session 期间存在） |
| `worktree.path` | worktree 目录的绝对路径 |
| `worktree.branch` | worktree 的 git 分支名称。对于基于 hook 的 worktree 缺失 |
| `worktree.original_cwd` | 进入 worktree 之前的目录 |
| `worktree.original_branch` | 进入 worktree 之前检出的 git 分支。对于基于 hook 的 worktree 缺失 |
| `github` | 检测到当前分支时，GitHub repository 和 pull-request 信息——repo 身份和关联的 PR（v2.1.145） |

### File Suggestion Configuration

文件建议 script 通过 stdin 接收一个 JSON 对象（例如 `{"query": "src/comp"}`），并且必须输出最多 15 个文件路径（每行一个）。

```json
{
  "fileSuggestion": {
    "type": "command",
    "command": "~/.claude/file-suggestion.sh"
  },
  "respectGitignore": true
}
```

**Example:**
```json
{
  "statusLine": {
    "type": "command",
    "command": "git branch --show-current 2>/dev/null || echo 'no-branch'"
  },
  "spinnerTipsEnabled": true,
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": ["Cooking", "Brewing", "Crafting", "Conjuring"]
  },
  "spinnerTipsOverride": {
    "tips": ["Use /compact at ~50% context", "Start with plan mode for complex tasks"],
    "excludeDefault": true
  }
}
```

---

## AWS & Cloud Credentials

### AWS Settings

| Key | Type | Description |
|-----|------|-------------|
| `awsAuthRefresh` | string | 刷新 AWS auth（修改 `.aws` 目录）的 script |
| `awsCredentialExport` | string | 输出包含 AWS credentials 的 JSON 的 script |

**Example:**
```json
{
  "awsAuthRefresh": "aws sso login --profile myprofile",
  "awsCredentialExport": "/bin/generate_aws_grant.sh"
}
```

### OpenTelemetry

| Key | Type | Description |
|-----|------|-------------|
| `otelHeadersHelper` | string | 生成动态 OpenTelemetry headers 的 script |

**Example:**
```json
{
  "otelHeadersHelper": "/bin/generate_otel_headers.sh"
}
```

---

## Environment Variables (via `env`)

为所有 Claude Code session 设置环境变量。

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "...",
    "NODE_ENV": "development",
    "DEBUG": "true"
  }
}
```

### Common Environment Variables

| Variable | Description |
|----------|-------------|
| `ANTHROPIC_API_KEY` | 用于认证的 API key |
| `ANTHROPIC_AUTH_TOKEN` | OAuth token |
| `CLAUDE_CODE_OAUTH_TOKEN` | 用于 Claude.ai 认证的 OAuth access token。SDK 和自动化环境中替代 `/login`。优先于 keychain 存储的凭据 |
| `CLAUDE_CODE_OAUTH_REFRESH_TOKEN` | 用于 Claude.ai 认证的 OAuth refresh token。设置后，`claude auth login` 直接交换此 token 而无需打开浏览器。需要 `CLAUDE_CODE_OAUTH_SCOPES` |
| `CLAUDE_CODE_OAUTH_SCOPES` | refresh token 被授予的空格分隔 OAuth scope（例如 `"user:profile user:inference user:sessions:claude_code"`）。当 `CLAUDE_CODE_OAUTH_REFRESH_TOKEN` 设置时必需 |
| `ANTHROPIC_WORKSPACE_ID` | 用于 [workload identity federation](https://platform.claude.com/docs/en/manage-claude/workload-identity-federation) 的 Workspace ID。当您的 federation rule 作用域包含多于一个 workspace 时设置，以便 token 交换知道目标 workspace（v2.1.141） |
| `ANTHROPIC_BASE_URL` | 自定义 API endpoint |
| `ANTHROPIC_BEDROCK_BASE_URL` | 覆盖 Bedrock endpoint URL |
| `ANTHROPIC_BEDROCK_MANTLE_BASE_URL` | 覆盖 Bedrock Mantle endpoint URL。参见 [Mantle endpoint](https://code.claude.com/docs/en/amazon-bedrock#use-the-mantle-endpoint) |
| `ANTHROPIC_BEDROCK_SERVICE_TIER` | Bedrock service tier：`default`、`flex` 或 `priority`。作为 `X-Amzn-Bedrock-Service-Tier` header 在每个请求上发送。参见 [Amazon Bedrock service tiers](https://code.claude.com/docs/en/amazon-bedrock#service-tiers)（v2.1.122） |
| `ANTHROPIC_AWS_API_KEY` | 用于 AWS 上 Claude Platform 的 Workspace API key |
| `ANTHROPIC_AWS_BASE_URL` | 覆盖 AWS 上 Claude Platform endpoint URL |
| `ANTHROPIC_AWS_WORKSPACE_ID` | AWS 上 Claude Platform 必需的 workspace ID |
| `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` | 由嵌入 Claude Code 并代表用户管理模型 provider 路由的宿主平台设置。设置后，`settings.json` 中的 provider 选择/endpoint/认证 env var（例如 `CLAUDE_CODE_USE_BEDROCK`、`ANTHROPIC_BASE_URL`、`ANTHROPIC_API_KEY`）被忽略，因此用户 settings 无法覆盖宿主的路由。Bedrock/Vertex/Foundry 的自动遥测退出也被跳过，因此遥测遵循标准的 `DISABLE_TELEMETRY` 退出（v2.1.126） |
| `ANTHROPIC_VERTEX_BASE_URL` | 覆盖 Vertex AI endpoint URL |
| `ANTHROPIC_BETAS` | 逗号分隔的 Anthropic beta header 值 |
| `ANTHROPIC_VERTEX_PROJECT_ID` | Vertex AI 的 GCP project ID |
| `GCLOUD_PROJECT` | Vertex AI 请求的 GCP project ID（覆盖 `ANTHROPIC_VERTEX_PROJECT_ID`） |
| `GOOGLE_APPLICATION_CREDENTIALS` | 用于 Vertex AI 认证的 GCP service account credential 文件路径 |
| `GOOGLE_CLOUD_PROJECT` | Vertex AI 请求的 GCP project ID（覆盖 `ANTHROPIC_VERTEX_PROJECT_ID`） |
| `ANTHROPIC_CUSTOM_MODEL_OPTION` | 要作为自定义条目添加到 `/model` 选择器中的 Model ID。用于使非标准或 gateway 特定模型可选择而无需替换内置别名 |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_NAME` | `/model` 选择器中自定义模型条目的显示名称。未设置时默认为 model ID |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` | `/model` 选择器中自定义模型条目的显示描述。未设置时默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_SUPPORTED_CAPABILITIES` | 覆盖自定义模型条目的能力检测。逗号分隔的值（例如 `effort,thinking`）。当自定义模型支持自动检测无法确认的功能时必需。参见 [model configuration](https://code.claude.com/docs/en/model-config#customize-pinned-model-display-and-capabilities) |
| `ANTHROPIC_MODEL` | 要使用的模型名称。接受别名（`sonnet`、`opus`、`haiku`）或完整 model ID。覆盖 `model` setting |
| `INIT_PROMPT` | 在 session 初始化时注入的自定义 system prompt |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 使用自定义 model ID 覆盖 Haiku 模型别名（例如用于第三方部署） |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用 pinned model 时自定义 `/model` 选择器中的 Haiku 条目标签。默认为 model ID |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Haiku 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_SUPPORTED_CAPABILITIES` | 覆盖 pinned Haiku 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当 pinned 模型支持自动检测无法确认的功能时必需 |
| `CLAUDECODE` | 在 Claude Code 生成的 shell 环境（Bash 工具、tmux session）中设置为 `1`。不在 hook 或 status line 命令中设置。用于检测 script 是否在 Claude Code shell 中运行 |
| `CLAUDE_CODE_CHILD_SESSION` | 在 Claude Code 通过 Bash、PowerShell 和 Monitor 工具、hook 命令和 status line 命令生成的子进程中设置为 `1`。不在 stdio MCP server 子进程中设置。与 `CLAUDECODE` 不同，这仅由 Claude Code 自己的生成路径设置（而非 IDE extension），因此它可靠地区分嵌套 `claude` session 与在 IDE 集成终端中启动的顶级 `claude`。嵌套交互式 TUI session 会自动从 `--resume`、`--continue`、上箭头历史和 `claude agents` 中排除。非交互式 `claude -p` session 仍然持久化。设置 `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE=1` 覆盖此排除（v2.1.172） |
| `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE` | 设置为 `1` 以覆盖嵌套交互式 TUI session 从 `--resume`、`--continue`、上箭头历史和 `claude agents` 中的自动排除。默认情况下，嵌套 session（其中 `CLAUDE_CODE_CHILD_SESSION=1`）被排除以防止其污染历史。当您希望跟踪嵌套 session 时设置此项以强制持久化 |
| `CLAUDE_CODE_SESSION_ID` | 只读。在 Bash 和 PowerShell 工具子进程中自动设置为当前 session ID。匹配传递给 hook 的 `session_id` 字段。在 `/clear` 时更新。用于将 script 和外部工具与启动它们的 Claude Code session 相关联（v2.1.132）。在 `--resume` 时也注入到 stdio MCP server 环境中（v2.1.163 changelog）*(在 v2.1.163 changelog 中；尚未在官方 env-vars 页面上——只读)* |
| `AI_AGENT` | 由 Claude Code 在子进程环境（Bash 工具、hook、MCP stdio server）中自动设置。标识父进程为 AI agent 的通用 flag——对于从任何 AI agent 调用时调整行为的工具很有用，而不是检查每个 agent 特定的变量如 `CLAUDECODE` *(在 v2.1.120 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` | 设置为 `1` 以在组织状态检查因网络错误失败时允许 fast mode。当企业 proxy 阻止状态 endpoint 时很有用 |
| `CLAUDE_CODE_USE_BEDROCK` | 使用 AWS Bedrock（`1` 启用） |
| `CLAUDE_CODE_USE_VERTEX` | 使用 Google Vertex AI（`1` 启用） |
| `CLAUDE_CODE_USE_FOUNDRY` | 使用 Microsoft Foundry（`1` 启用） |
| `CLAUDE_CODE_USE_MANTLE` | 使用 Bedrock [Mantle endpoint](https://code.claude.com/docs/en/amazon-bedrock#use-the-mantle-endpoint)（`1` 启用） |
| `CLAUDE_CODE_USE_POWERSHELL_TOOL` | 设置为 `1` 以在 Windows 上启用 PowerShell 工具（opt-in preview）。启用时，Claude 可以原生运行 PowerShell 命令而不是通过 Git Bash 路由。仅在原生 Windows 上支持，不支持 WSL（v2.1.84） |
| `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY` | 设置为 `1` 以阻止 Claude Code 在生成 PowerShell 用于工具调用、hook 和 status line 命令时传递 `-ExecutionPolicy Bypass`，改为尊重机器的有效执行策略。默认情况下，Claude Code 在进程 scope 绕过执行策略，以便 `.ps1` 脚本和模块导入在默认 Restricted 的 Windows 上工作。永不会覆盖 Group Policy `MachinePolicy`/`UserPolicy`（v2.1.143） |
| `CLAUDE_CODE_REMOTE` | 只读。当 Claude Code 作为 cloud session 运行时自动设置为 `true`。从 hook 或 setup script 读取此项以检测您是否在 cloud 环境中 |
| `CLAUDE_CODE_REMOTE_SESSION_ID` | 只读。在 cloud session 中自动设置为当前 session 的 ID。读取此项以构建返回 session transcript 的链接 |
| `CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX` | 自动生成的 Remote Control session 名称的前缀。默认为机器 hostname |
| `CLAUDE_CLIENT_PRESENCE_FILE` | 指向一个文件的路径，当该文件存在时，表示有活跃的客户端并抑制来自 Remote Control 的移动推送通知。在桌面客户端始终运行且不需要移动 ping 的环境中很有用 |
| `CLAUDE_CODE_ENABLE_TELEMETRY` | 启用/禁用遥测（`0` 或 `1`） |
| `DISABLE_ERROR_REPORTING` | 禁用错误报告（`1` 禁用） |
| `DISABLE_AUTOUPDATER` | 设置为 `1` 以禁用对 npm registry 的自动更新检查。也可作为仅启动变量配置——参见 [CLI 启动参数](./claude-cli-startup-flags-zh.md#environment-variables) |
| `DISABLE_UPDATES` | 设置为 `1` 以完全阻止所有更新路径——自动检查、通知和手动 `claude update`。比 `DISABLE_AUTOUPDATER` 更严格，后者仅禁用后台检查。在必须阻止所有更新直到明确重新启用的环境中使用 *(在 v2.1.118 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` | 设置为 `1` 以让 Claude Code 在新版本可用时在后台运行您的 package manager 的升级命令。适用于 Homebrew 和 WinGet 安装。其他 package manager 继续显示升级命令而不运行它。参见 [Auto updates](https://code.claude.com/docs/en/setup#auto-updates)（v2.1.129） |
| `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` | 设置为 `1` 以当 `ANTHROPIC_BASE_URL` 指向与 Anthropic 兼容的 gateway（如 LiteLLM、Kong 或内部 proxy）时，从您的 gateway 的 `/v1/models` endpoint 填充 `/model` 选择器。默认关闭，因为由共享 API key 支持的 gateway 会暴露该 key 可访问的每个模型。发现的模型仍受 `availableModels` allowlist 过滤（v2.1.129，从先前的自动发现更改） |
| `DISABLE_TELEMETRY` | 禁用遥测（`1` 禁用） |
| `DO_NOT_TRACK` | 标准 opt-out 变量；设置为 `1` 以退出遥测收集。被 `DISABLE_TELEMETRY` 尊重 |
| `MCP_TIMEOUT` | MCP 启动超时时间（ms） |
| `CLAUDE_CODE_MCP_ALLOWLIST_ENV` | 使用仅安全基线环境生成 stdio MCP server，剥离大多数继承的 env var 以防止凭据泄漏到不受信任的 server 进程中 |
| `MAX_MCP_OUTPUT_TOKENS` | 最大 MCP 输出 token（默认：25000）。当输出超过 10,000 token 时显示警告 |
| `API_TIMEOUT_MS` | API 请求超时时间（ms）（默认：600000） |
| `API_FORCE_IDLE_TIMEOUT` | 覆盖 streaming 连接的 5 分钟空闲超时。设置为 `0` 完全禁用空闲超时，`1` 在所有连接上强制执行，或不设置使用默认（在经常停滞的缓慢或不可靠 gateway 上自动启用）。用于缓慢的 API gateway（v2.1.169） |
| `CLAUDE_CODE_CONNECT_TIMEOUT_MS` | streaming API 请求的 connect、TLS 和 response-header 阶段的超时时间（ms）（默认：`60000` / 60 秒）。如果在此窗口内没有 response header 到达，请求被中止并重试。设置为 `0` 禁用并仅依赖 `API_TIMEOUT_MS` |
| `BASH_MAX_TIMEOUT_MS` | Bash 命令超时时间 |
| `BASH_MAX_OUTPUT_LENGTH` | 最大 bash 输出长度 |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 自动压缩阈值百分比（1-100）。默认约 95%。设置较低（例如 `50`）以更早触发压缩。高于 95% 的值无效。使用 `/context` 监控当前使用情况。示例：`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50 claude` |
| `CLAUDE_CODE_MAX_CONTEXT_TOKENS` | 覆盖 Claude Code 为 active model 假设的 context window 大小。仅在也设置了 `DISABLE_COMPACT` 时生效。当通过 `ANTHROPIC_BASE_URL` 路由到其 context window 与其名称的内置大小不匹配的模型时使用 |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | 在 bash 调用之间保持 cwd（`1` 启用） |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | 禁用后台任务（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_BG_SHELL_PRESSURE_REAP` | 设置为 `1` 以禁用在内存压力下自动回收空闲的后台 shell 命令。未设置时，Claude Code 在内存压力下自动回收空闲的后台 shell 以释放资源（v2.1.193 changelog，尚未在官方 env-vars 页面上） |
| `CLAUDE_CODE_DISABLE_ADVISOR_TOOL` | 设置为 `1` 以禁用 advisor 工具和 `/advisor` 命令。省略 advisor 使用的 env-var 等效方式。与 `advisorModel` 配合用于 advisor 配置（最低 v2.1.98） |
| `CLAUDE_CODE_DISABLE_AGENT_VIEW` | 设置为 `1` 以关闭后台 agent 和 agent view（`claude agents`、`--bg`、`/background`、按需 supervisor）。`disableAgentView` setting 的 env-var 等效方式 *(在官方 settings 页面上引用；未在 env-vars 页面上列出)* |
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` | 启用实验性 agent teams 功能（`1` 启用）。允许在 session 内生成协调的 subagent team。也可作为仅启动变量配置——参见 [CLI 启动参数](./claude-cli-startup-flags-zh.md#environment-variables) |
| `CLAUDE_CODE_DISABLE_WORKFLOWS` | 设置为 `1` 以禁用 [dynamic workflows](https://code.claude.com/docs/en/workflows)（`/workflows`）和内置的 workflow slash command。`disableWorkflows` setting 的 env-var 等效方式 |
| `CLAUDE_CODE_ENABLE_AUTO_MODE` | 设置为 `1` 以使 [auto mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) 在 Amazon Bedrock、Google Cloud Vertex AI 和 Microsoft Foundry 上可用。对 Anthropic API 没有影响，其中 auto mode 默认可用（v2.1.158） |
| `CLAUDE_CODE_DISABLE_BUNDLED_SKILLS` | 设置为 `1` 以对模型隐藏 Claude Code 的内置能力（bundled skill）。`disableBundledSkills` setting 的 env-var 等效方式（v2.1.169） |
| `CLAUDE_CODE_DISABLE_ARTIFACT` | 设置为 `1` 以禁用 [Artifact](https://code.claude.com/docs/en/artifacts) 工具，该工具将 session 输出发布为 claude.ai 上的私有 web 页面。等效于 `disableArtifact` setting |
| `CLAUDE_CODE_ARTIFACT_AUTO_OPEN` | 设置为 `0` 以阻止 Claude Code 在新 artifact 发布时自动打开浏览器。重新发布现有 artifact 不会打开浏览器，无论此 setting 如何 |
| `ENABLE_TOOL_SEARCH` | MCP 工具搜索阈值（例如 `auto:5`） |
| `ENABLE_PROMPT_CACHING_1H` | 选择使用 1 小时 prompt cache TTL。替换已弃用的 `ENABLE_PROMPT_CACHING_1H_BEDROCK` *(在 v2.1.108 changelog 中，尚未在官方 env-vars 页面上)* |
| `FORCE_PROMPT_CACHING_5M` | 强制 5 分钟 prompt cache TTL *(在 v2.1.108 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_ENABLE_AWAY_SUMMARY` | 退出 away summary / 空闲 session 回顾。设置为 `0` 禁用。与 `awaySummaryEnabled` setting 配合使用（v2.1.110） |
| `DISABLE_PROMPT_CACHING` | 禁用所有 prompt caching（`1` 禁用） |
| `DISABLE_PROMPT_CACHING_HAIKU` | 禁用 Haiku prompt caching |
| `DISABLE_PROMPT_CACHING_SONNET` | 禁用 Sonnet prompt caching |
| `DISABLE_PROMPT_CACHING_OPUS` | 禁用 Opus prompt caching |
| `ENABLE_PROMPT_CACHING_1H_BEDROCK` | 在 Bedrock 上请求 1 小时 cache TTL（`1` 启用） *(不在官方文档中——未验证；v2.1.108 changelog 说已弃用，由 `ENABLE_PROMPT_CACHING_1H` 替代)* |
| `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` | 禁用实验性 beta 功能（`1` 禁用） |
| `CLAUDE_CODE_SHELL` | 覆盖自动 shell 检测 |
| `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` | 覆盖默认文件读取 token 限制 |
| `CLAUDE_CODE_GLOB_HIDDEN` | 设置为 `false` 以在 Claude 调用 Glob 工具时从结果中排除 dotfiles。默认包含。不影响 `@` 文件自动补全、`ls`、Grep 或 Read |
| `CLAUDE_CODE_GLOB_NO_IGNORE` | 设置为 `false` 以使 Glob 工具尊重 `.gitignore` 模式。默认情况下，Glob 返回所有匹配的文件，包括被 gitignore 的。不影响 `@` 文件自动补全，后者有自己的 `respectGitignore` setting |
| `CLAUDE_CODE_GLOB_TIMEOUT_SECONDS` | Glob 文件发现的超时时间（秒） |
| `CLAUDE_CODE_ENABLE_TASKS` | 控制 session 是否使用结构化的 Task 工具（`TaskCreate`、`TaskUpdate`、`TaskGet`、`TaskList`）还是旧版 `TodoWrite` 工具。截至 v2.1.142，Task 工具在所有模式下是默认的。设置为 `0` 回退到 `TodoWrite` |
| `CLAUDE_CODE_SIMPLE` | 设置为 `1` 以使用最小 system prompt 和仅 Bash、文件读取和文件编辑工具运行。也可作为仅启动变量配置——参见 [CLI 启动参数](./claude-cli-startup-flags-zh.md#environment-variables) |
| `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` | 在空闲持续时间后自动退出 SDK 模式（ms） |
| `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING` | 禁用 adaptive thinking（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_THINKING` | 强制禁用 extended thinking（`1` 禁用） |
| `DISABLE_INTERLEAVED_THINKING` | 阻止发送 interleaved-thinking beta header（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_1M_CONTEXT` | 禁用 1M token context window（`1` 禁用） |
| `CLAUDE_CODE_ACCOUNT_UUID` | 覆盖用于认证的账号 UUID |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | 禁用 git 相关的 system prompt 指令 |
| `CLAUDE_CODE_ATTRIBUTION_HEADER` | 设置为 `0` 以从 system prompt 中省略 Claude Code attribution 块 |
| `CLAUDE_CODE_NEW_INIT` | 设置为 `true` 以使 `/init` 运行交互式 setup 流程。在探索代码库之前询问要生成哪些文件（CLAUDE.md、skills、hooks）。没有此项时，`/init` 自动生成 CLAUDE.md |
| `CLAUDE_CODE_PLUGIN_SEED_DIR` | 指向一个或多个只读 plugin seed 目录的路径，在 Unix 上由 `:` 分隔，在 Windows 上由 `;` 分隔。将预填充的 plugins 捆绑到容器镜像中。Claude Code 在启动时从这些目录注册 marketplace，并使用预先缓存的 plugins 而无需重新克隆 |
| `ENABLE_CLAUDEAI_MCP_SERVERS` | 启用 Claude.ai MCP server |
| `CLAUDE_CODE_EFFORT_LEVEL` | 设置 effort level：`low`、`medium`、`high`、`xhigh`（Opus 4.7 和 4.8，v2.1.111）、`max`（仅 Opus 4.6）或 `auto`（使用模型默认值）。优先于 `/effort` 和 `effortLevel` setting。也可作为仅启动变量配置——参见 [CLI 启动参数](./claude-cli-startup-flags-zh.md#environment-variables) |
| `CLAUDE_EFFORT` | 只读。注入到 Bash 工具子进程和 hook handler 中，带有 active effort level，以便 shell scripts 和 hook 可以适应当前层级（`CLAUDE_CODE_EFFORT_LEVEL` 的配套变量；v2.1.133）。在 skill 文件内部使用 `${CLAUDE_EFFORT}` *(在 changelog 中，不在官方 env-vars 页面上——只读，不可用户配置)* |
| `CLAUDE_CODE_ALWAYS_ENABLE_EFFORT` | 设置为 `1` 以在所有模型上强制启用 effort 参数，即使是那些通常不支持 effort level 选择的模型。允许 `/effort` 和 `effortLevel` setting 对标准 effort 能力集之外的模型生效（v2.1.154） *(在 v2.1.154 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_MAX_TURNS` | 停止前的最大 agentic turn 数 *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 等效于设置 `DISABLE_AUTOUPDATER`、`DISABLE_FEEDBACK_COMMAND`、`DISABLE_ERROR_REPORTING` 和 `DISABLE_TELEMETRY` |
| `CLAUDE_CODE_SKIP_SETTINGS_SETUP` | 跳过首次运行 settings setup 流程 *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_PROMPT_CACHING_ENABLED` | 覆盖 prompt caching 行为 *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_DISABLE_TOOLS` | 要禁用的逗号分隔工具列表 *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_DISABLE_MCP` | 禁用所有 MCP server（`1` 禁用） *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | 每个响应的最大输出 token。默认：32,000（截至 v2.1.77，Opus 4.6 为 64,000）。上限：64,000（截至 v2.1.77，Opus 4.6 和 Sonnet 4.6 为 128,000） |
| `CLAUDE_CODE_DISABLE_FAST_MODE` | 完全禁用 fast mode（`1` 禁用） |
| `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` | **在 v2.1.160 中移除** —— 环境变量现在是空操作。Fast mode 在默认模型上运行，无论此变量如何。之前将 [fast mode](https://code.claude.com/docs/en/fast-mode) 固定到 Claude Opus 4.6 而非默认值（v2.1.142–v2.1.159） |
| `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` | 设置为 `1` 以在 streaming 请求中途失败时禁用非 streaming 回退。Streaming 错误传播到重试层。当 proxy 或 gateway 导致回退产生重复的工具执行时很有用（v2.1.83） |
| `CLAUDE_ENABLE_STREAM_WATCHDOG` | 中止停滞的 stream（`1` 启用） |
| `CLAUDE_CODE_ENABLE_FINE_GRAINED_TOOL_STREAMING` | 在 Anthropic API 上默认启用（v2.1.139+）；设置为 `0` 退出 |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | 禁用 auto memory（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING` | 禁用 `/rewind` 的文件 checkpointing（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_ATTACHMENTS` | 禁用 attachment 处理（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_CLAUDE_MDS` | 阻止加载 CLAUDE.md 文件（`1` 禁用） |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` | 在启动时从通过 `--add-dir` 指定的额外目录加载 CLAUDE.md memory 文件（`1` 启用）。也可作为仅启动变量配置——参见 [CLI 启动参数](./claude-cli-startup-flags-zh.md#environment-variables) |
| `CLAUDE_CODE_DISABLE_POLICY_SKILLS` | 跳过从系统级 managed skills 目录加载 skills（`1` 禁用） |
| `CLAUDE_CODE_RESUME_INTERRUPTED_TURN` | 如果上一个 session 在中途结束，自动恢复（`1` 启用） |
| `CLAUDE_CODE_SKIP_PROMPT_HISTORY` | 设置为 `1` 以跳过将 prompt history 和 session transcript 写入磁盘。使用此变量启动的 session 不会出现在 `--resume`、`--continue` 或上箭头历史中。用于短暂的脚本化 session |
| `CLAUDE_CODE_USER_EMAIL` | 同步提供用于认证的用户 email |
| `CLAUDE_CODE_ORGANIZATION_UUID` | 同步提供用于认证的组织 UUID |
| `CLAUDE_CONFIG_DIR` | 自定义配置目录（覆盖默认的 `~/.claude`） |
| `CLAUDE_CODE_TMPDIR` | 覆盖用于内部临时文件的临时目录。Claude Code 将 `/claude/` 追加到此路径。默认：Unix/macOS 上为 `/tmp`，Windows 上为 `os.tmpdir()` |
| `ANTHROPIC_CUSTOM_HEADERS` | API 请求的自定义 headers（`Name: Value` 格式，多个 headers 用换行分隔） |
| `CLAUDE_CODE_EXTRA_BODY` | 要合并到每个 API 请求体顶层的 JSON 对象。用于注入供应商特定字段（例如自定义 gateway 的路由提示） |
| `CLAUDE_CODE_PROPAGATE_TRACEPARENT` | 设置为 `1` 以在通过自定义 proxy 路由时通过请求传播 W3C `traceparent` header，将 Claude Code trace 链接到上游遥测 |
| `ANTHROPIC_FOUNDRY_API_KEY` | 用于 Microsoft Foundry 认证的 API key |
| `ANTHROPIC_FOUNDRY_BASE_URL` | Foundry 资源的 Base URL |
| `ANTHROPIC_FOUNDRY_RESOURCE` | Foundry 资源名称 |
| `AWS_BEARER_TOKEN_BEDROCK` | 用于认证的 Bedrock API key |
| `ANTHROPIC_SMALL_FAST_MODEL` | **DEPRECATED** — 请改用 `ANTHROPIC_DEFAULT_HAIKU_MODEL` |
| `ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION` | 已弃用的 Haiku 类模型覆盖的 AWS region |
| `CLAUDE_CODE_SHELL_PREFIX` | 前置到 bash 命令的命令前缀 |
| `BASH_DEFAULT_TIMEOUT_MS` | 默认 bash 命令超时时间（ms） |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH` | 跳过 Bedrock 的 AWS auth（`1` 跳过） |
| `CLAUDE_CODE_SKIP_FOUNDRY_AUTH` | 跳过 Foundry 的 Azure auth（`1` 跳过） |
| `CLAUDE_CODE_SKIP_MANTLE_AUTH` | 跳过 Bedrock Mantle 的 AWS 认证（例如使用 LLM gateway 时） |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH` | 跳过 Vertex 的 Google auth（`1` 跳过） |
| `CLAUDE_CODE_PROXY_RESOLVES_HOSTS` | 允许 proxy 执行 DNS 解析 |
| `CLAUDE_CODE_API_KEY_HELPER_TTL_MS` | `apiKeyHelper` 的凭据刷新间隔（ms） |
| `CLAUDE_CODE_CLIENT_CERT` | 用于 mTLS 的客户端证书路径 |
| `CLAUDE_CODE_CLIENT_KEY` | 用于 mTLS 的客户端私钥路径 |
| `CLAUDE_CODE_CLIENT_KEY_PASSPHRASE` | 加密 mTLS 密钥的密码短语 |
| `CLAUDE_CODE_CERT_STORE` | TLS 连接的逗号分隔 CA 证书源列表：`bundled`（Claude Code 附带的 Mozilla CA 集）和/或 `system`（OS trust store）。默认：`bundled,system`。系统存储集成需要原生二进制发行版；在 Node.js runtime 上，无论此值如何都仅使用 bundled 集（v2.1.101） |
| `CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS` | Plugin marketplace git clone 超时时间（ms）（默认：120000） |
| `CLAUDE_CODE_PLUGIN_PREFER_HTTPS` | 设置为 `1` 以通过 HTTPS 而不是 SSH 克隆 GitHub `owner/repo` 简写 plugin 源。适用于 plugin install/update 和 `/plugin marketplace add`/`update`。在没有为 `github.com` 配置 SSH key 的 CI runner 或容器中很有用（v2.1.141） |
| `CLAUDE_CODE_PLUGIN_CACHE_DIR` | 覆盖 plugins 根目录 |
| `CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL` | 跳过自动添加官方 marketplace（`1` 禁用） |
| `CLAUDE_CODE_SYNC_PLUGIN_INSTALL` | 在首次查询之前等待 plugin 安装完成（`1` 启用） |
| `CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS` | 同步 plugin 安装的超时时间（ms） |
| `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` | 设置为 `1` 以在 `git pull` 失败时保留现有 marketplace cache，而不是擦除并重新克隆。在离线或 airgapped 环境中很有用，其中重新克隆会以相同方式失败 |
| `CLAUDE_CODE_ENABLE_BACKGROUND_PLUGIN_REFRESH` | 在后台安装完成后在 session turn 边界刷新 plugin 状态（`1` 启用）。没有此项时，新安装的 plugins 在下一次 session 中生效 |
| `CLAUDE_CODE_HIDE_ACCOUNT_INFO` | 从 UI 中隐藏 email/org 信息 *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_DISABLE_CRON` | 禁用 scheduled/cron 任务（`1` 禁用） |
| `DISABLE_INSTALLATION_CHECKS` | 禁用安装警告 |
| `DISABLE_FEEDBACK_COMMAND` | 禁用 `/feedback` 命令。旧名称 `DISABLE_BUG_COMMAND` 也被接受 |
| `DISABLE_DOCTOR_COMMAND` | 隐藏 `/doctor` 命令（`1` 禁用） |
| `DISABLE_LOGIN_COMMAND` | 隐藏 `/login` 命令（`1` 禁用） |
| `DISABLE_LOGOUT_COMMAND` | 隐藏 `/logout` 命令（`1` 禁用） |
| `DISABLE_UPGRADE_COMMAND` | 隐藏 `/upgrade` 命令（`1` 禁用） |
| `DISABLE_EXTRA_USAGE_COMMAND` | 隐藏 `/extra-usage` 命令——在 v2.1.144 中重命名为 `/usage-credits`，但此 env var 名称未更改（`1` 禁用） |
| `DISABLE_INSTALL_GITHUB_APP_COMMAND` | 隐藏 `/install-github-app` 命令（`1` 禁用） |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS` | 禁用 flavor text 和非必要的模型调用 *(不在官方文档中——未验证)* |
| `CLAUDE_CODE_DEBUG_LOGS_DIR` | 覆盖 debug log 文件路径。尽管名称如此，这是一个文件路径而不是目录。需要单独通过 `--debug`、`/debug` 或 `DEBUG` env var 启用 debug mode；仅设置此变量不会启用日志记录。使用 `--debug-file` 同时启用 debug mode 并设置路径。默认：`~/.claude/debug/<session-id>.txt` |
| `CLAUDE_CODE_DEBUG_LOG_LEVEL` | 最低 debug log 级别 |
| `CLAUDE_AUTO_BACKGROUND_TASKS` | 强制自动后台化长时间任务（`1` 启用） |
| `CLAUDE_CODE_DISABLE_LEGACY_MODEL_REMAP` | 阻止将 Opus 4.0/4.1 重新映射到较新模型（`1` 禁用） |
| `FALLBACK_FOR_ALL_PRIMARY_MODELS` | 为所有主模型触发 fallback model，而不仅仅是默认模型（`1` 启用） |
| `CCR_FORCE_BUNDLE` | 设置为 `1` 以强制 `claude --remote` 捆绑并上传您的本地 repository，即使 GitHub 访问可用。也可作为仅启动变量配置——参见 [CLI 启动参数](./claude-cli-startup-flags-zh.md#environment-variables) |
| `CLAUDE_CODE_GIT_BASH_PATH` | 仅限 Windows：指向 Git Bash 可执行文件（`bash.exe`）的路径。在安装了 Git Bash 但不在 PATH 中时使用 |
| `DISABLE_COST_WARNINGS` | 禁用成本警告消息 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 覆盖 subagent 的模型（例如 `haiku`、`sonnet`） |
| `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` | 设置为 `1` 以从子进程环境（Bash 工具、hook、MCP stdio server）中剥离 Anthropic 和 cloud provider 凭据。当子进程不应继承 API key 时用于深度防御（v2.1.83） |
| `CLAUDE_CODE_SCRIPT_CAPS` | 当设置了 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` 时，限制特定 scripts 每个 session 可以被调用的次数的 JSON 对象。Keys 是与命令文本匹配的子字符串；values 是整数调用限制。例如，`{"deploy.sh": 2}` 允许 `deploy.sh` 最多被调用两次。匹配是基于子字符串的；不检测通过 `xargs` 或 `find -exec` 的 runtime fan-out——这是一个深度防御控制 |
| `CLAUDE_CODE_PERFORCE_MODE` | 设置为 `1` 以启用 Perforce 感知的写保护。启用时，如果目标文件缺少 owner-write 位（Perforce 在 synced 文件上清除直到 `p4 edit` 打开它们），Edit、Write 和 NotebookEdit 会以 `p4 edit <file>` 提示失败。防止 Claude Code 绕过 Perforce 更改跟踪（v2.1.98） |
| `CLAUDE_CODE_MAX_RETRIES` | 覆盖 API 请求重试次数（默认：10） |
| `CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY` | 最大并行只读工具数（默认：10） |
| `CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS` | 在 SDK mode 中禁用内置 subagent 类型（`1` 禁用） |
| `CLAUDE_AGENT_SDK_MCP_NO_PREFIX` | 在 SDK mode 中跳过 MCP 工具的 `mcp__<server>__` 前缀（`1` 启用） |
| `CLAUDE_ASYNC_AGENT_STALL_TIMEOUT_MS` | 后台 subagent 的停滞超时时间（ms）（默认：600000 / 10 分钟）。如果 subagent 在此持续时间内没有产生输出，则被终止 |
| `MCP_CONNECTION_NONBLOCKING` | 在 `-p` mode 中设置为 `true` 以完全跳过 MCP 连接等待。将 `--mcp-config` server 连接限制在 5 秒而不是阻塞在最慢的 server 上 *(在 v2.1.89 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hook 超时时间（ms）（替换固定的 1.5s 限制） |
| `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY` | 禁用反馈调查提示（`1` 禁用） |
| `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` | 设置为 `1` 以在 Anthropic 绑定的非必要流量被阻止时将 session 质量调查路由到您自己的 OpenTelemetry collector。调查评分仅作为 OTEL events 发送到您配置的 collector——不会向 Anthropic 发送调查数据。当 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`、`DISABLE_TELEMETRY` 或 `DO_NOT_TRACK` 设置时适用；否则无效。`CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY` 和组织产品反馈策略优先（v2.1.136） |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | 禁用终端标题更新（`1` 禁用） |
| `CLAUDE_CODE_TMUX_TRUECOLOR` | 设置为 `1` 以允许 tmux 内部的 24-bit truecolor 输出。默认情况下，Claude Code 在设置了 `$TMUX` 时限制为 256 色，因为 tmux 除非配置否则不会传递 truecolor 转义序列。在将 `set -ga terminal-overrides ',*:Tc'` 添加到 `~/.tmux.conf` 后设置此项 |
| `CLAUDE_CODE_NO_FLICKER` | 设置为 `1` 以启用无闪烁 alt-screen 渲染。消除 fullscreen 重绘期间的视觉闪烁（v2.1.88） |
| `CLAUDE_CODE_ALT_SCREEN_FULL_REPAINT` | 设置为 `1` 以在 fullscreen 渲染中每帧重绘整个屏幕。当部分重绘在不寻常的终端模拟器中产生视觉 artifact 时使用 |
| `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` | 设置为 `1` 以禁用 fullscreen 渲染并使用经典的主屏幕渲染器。对话保留在终端的原生 scrollback 中，因此 `Cmd+f` 和 tmux copy mode 照常工作。优先于 `CLAUDE_CODE_NO_FLICKER` 和 `tui` setting。您也可以使用 `/tui default` 切换（v2.1.132） |
| `CLAUDE_CODE_FORCE_SYNC_OUTPUT` | 设置为 `1` 以在您的终端支持但未被自动检测到时强制启用 DEC private mode 2026 synchronized output。用于实现 BSU/ESU 但不响应能力探测的模拟器，如 Emacs `eat`。在 tmux 下无效（v2.1.129） |
| `CLAUDE_CODE_SCROLL_SPEED` | fullscreen 渲染的鼠标滚轮滚动乘数。增加以获得更快的滚动，减少以获得更精细的控制 |
| `CLAUDE_CODE_DISABLE_VIRTUAL_SCROLL` | 设置为 `1` 以在 fullscreen 渲染中禁用虚拟滚动并渲染 transcript 中的每条消息。当 fullscreen mode 中的滚动显示应出现消息的空白区域时使用 |
| `CLAUDE_CODE_DISABLE_MOUSE` | 设置为 `1` 以在 fullscreen 渲染中禁用鼠标跟踪。当鼠标事件干扰终端复用器或可访问性工具时有用 |
| `CLAUDE_CODE_HIDE_CWD` | 设置为 `1` 以在 Claude Code 启动 logo banner 中隐藏当前工作目录。在屏幕录制、演示或共享 session 中很有用，其中 CWD 路径会泄露主机或项目布局信息（v2.1.119） |
| `CLAUDE_CODE_ACCESSIBILITY` | 设置为 `1` 以保持原生终端光标对屏幕阅读器和可访问性工具可见 |
| `CLAUDE_AX_SCREEN_READER` | 设置为 `1` 以渲染屏幕阅读器友好的输出：平面文本，没有装饰性边框或动画。设置为 `0` 以强制关闭 screen-reader mode，即使 `axScreenReader` setting 为 `true`。`--ax-screen-reader` CLI flag 优先（v2.1.181+） |
| `CLAUDE_CODE_NATIVE_CURSOR` | 设置为 `1` 以在输入插入位置显示终端自己的光标，而不是 Claude Code 的自定义光标字符 |
| `CLAUDE_CODE_SYNTAX_HIGHLIGHT` | 设置为 `0` 以禁用 diff 输出中的语法高亮 |
| `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` | 跳过自动 IDE extension 安装（`1` 跳过） |
| `CLAUDE_CODE_AUTO_CONNECT_IDE` | 覆盖自动 IDE 连接行为 |
| `CLAUDE_CODE_IDE_HOST_OVERRIDE` | 覆盖用于连接的 IDE host 地址 |
| `CLAUDE_CODE_IDE_SKIP_VALID_CHECK` | 跳过 IDE lockfile 验证（`1` 跳过） |
| `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | OTel headers helper script 的去抖间隔（ms） |
| `CLAUDE_CODE_OTEL_FLUSH_TIMEOUT_MS` | OpenTelemetry flush 的超时时间（ms） |
| `CLAUDE_CODE_OTEL_SHUTDOWN_TIMEOUT_MS` | OpenTelemetry shutdown 的超时时间（ms） |
| `CLAUDE_ENABLE_BYTE_WATCHDOG` | 设置为 `1` 强制启用字节级 streaming idle watchdog，或 `0` 强制禁用它。未设置时，watchdog 默认对 Anthropic API 连接启用。字节 watchdog 在 `CLAUDE_STREAM_IDLE_TIMEOUT_MS`（最低 5 分钟）设置的持续时间内没有字节在线路上到达时中止连接，独立于事件级 watchdog |
| `CLAUDE_STREAM_IDLE_TIMEOUT_MS` | streaming idle watchdog 的超时时间（ms）。适用两个 watchdog：**字节级**（默认和最小 `300000` / 5 分钟，当没有字节在线路上到达时中止）和**事件级**（默认 `90000` / 90 秒，无最小值，当没有 SSE 事件到达时中止）。字节 watchdog 默认对 Anthropic API 连接启用；通过 `CLAUDE_ENABLE_BYTE_WATCHDOG` 控制。如果长时间运行的工具或缓慢的网络导致过早超时错误，请增加事件超时 |
| `OTEL_LOG_TOOL_DETAILS` | 设置为 `1` 以在 OpenTelemetry events 中包含 `tool_parameters`。默认出于隐私原因省略 *(在 v2.1.85 changelog 中，尚未在官方 env-vars 页面上)* |
| `OTEL_LOG_RAW_API_BODIES` | 设置为 `1` 以将完整的 API request 和 response body 作为 OpenTelemetry log events 发送。默认出于隐私和 payload 大小原因省略。用于在 gateway 或 proxy 处调试 *(在 v2.1.111 changelog 中，尚未在官方 env-vars 页面上)* |
| `OTEL_RESOURCE_ATTRIBUTES` | 逗号分隔的 `key=value` 对，作为资源属性添加到 Claude Code 发出的所有 OpenTelemetry metric data points。用于附加在每个 metric 上出现的环境或部署标签（例如 `environment=production,team=platform`），以便在 collector 中进行过滤（v2.1.162） |
| `OTEL_LOG_USER_PROMPTS` | 设置为 `1` 以在 OpenTelemetry LLM request span 中包含 `user_system_prompt` 字段。默认出于隐私原因省略——用户 prompt 可能包含敏感数据，因此仅在您控制 OTel collector 并有相关政策时选择加入 *(在 v2.1.121 changelog 中，尚未在官方 env-vars 页面上)* |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | 用于 metrics 和 logs 的 OpenTelemetry collector endpoint URL。参见 [Monitoring](https://code.claude.com/docs/en/monitoring-usage) |
| `OTEL_EXPORTER_OTLP_HEADERS` | OpenTelemetry exporter headers（`Name=Value` 格式，逗号分隔），用于与 collector 进行认证 |
| `OTEL_LOG_TOOL_CONTENT` | 设置为 `1` 以将完整工具输入和输出作为 OpenTelemetry log events 发送。默认出于隐私原因省略 |
| `OTEL_METRICS_EXPORTER` | OpenTelemetry metrics exporter 类型（例如 `otlp`）。参见 [Monitoring](https://code.claude.com/docs/en/monitoring-usage) |
| `OTEL_TRACES_EXPORTER` | OpenTelemetry traces exporter 类型（例如 `otlp`）。参见 [Monitoring](https://code.claude.com/docs/en/monitoring-usage) |
| `OTEL_METRICS_INCLUDE_ENTRYPOINT` | 设置为 `1` 以在所有 OpenTelemetry metric data points 上包含 session entry-point（例如 interactive vs `-p` vs SDK）作为标签。用于按 Claude Code 调用方式细分 metrics（v2.1.161 changelog） *(在 v2.1.161 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_FORK_SUBAGENT` | 设置为 `1` 以在外部构建（非 Anthropic 签名发行版）上启用 forked subagent。Forked subagent 在隔离的子进程中运行，而不是共享主 agent 的 context *(在 v2.1.117 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_MCP_SERVER_NAME` | MCP server 的名称，作为环境变量传递给 `headersHelper` scripts，以便它们可以生成 server 特定的认证 headers *(在 v2.1.85 changelog 中，尚未在官方 env-vars 页面上)* |
| `CLAUDE_CODE_MCP_SERVER_URL` | MCP server 的 URL，作为环境变量与 `CLAUDE_CODE_MCP_SERVER_NAME` 一起传递给 `headersHelper` scripts *(在 v2.1.85 changelog 中，尚未在官方 env-vars 页面上)* |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 覆盖 Opus 模型别名（例如 `claude-opus-4-6[1m]`） |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用 pinned model 时自定义 `/model` 选择器中的 Opus 条目标签。默认为 model ID |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Opus 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_SUPPORTED_CAPABILITIES` | 覆盖 pinned Opus 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当 pinned 模型支持自动检测无法确认的功能时必需 |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 覆盖 Sonnet 模型别名（例如 `claude-sonnet-4-6`） |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用 pinned model 时自定义 `/model` 选择器中的 Sonnet 条目标签。默认为 model ID |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Sonnet 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_SUPPORTED_CAPABILITIES` | 覆盖 pinned Sonnet 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当 pinned 模型支持自动检测无法确认的功能时必需 |
| `ANTHROPIC_DEFAULT_FABLE_MODEL` | 覆盖 Fable 模型别名（例如 `claude-fable-5`） |
| `ANTHROPIC_DEFAULT_FABLE_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用 pinned model 时自定义 `/model` 选择器中的 Fable 条目标签。默认为 model ID |
| `ANTHROPIC_DEFAULT_FABLE_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中的 Fable 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_FABLE_MODEL_SUPPORTED_CAPABILITIES` | 覆盖 pinned Fable 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当 pinned 模型支持自动检测无法确认的功能时必需 |
| `MAX_THINKING_TOKENS` | 每个响应的最大 extended thinking token。设置为 `0` 以在 Anthropic API 上完全禁用 extended thinking（等效于 `--thinking disabled`）。仅在使用固定 thinking budget 时适用——在 adaptive thinking 模型（Opus 4.7+）上，effort level 控制 thinking depth |
| `CLAUDE_CODE_AUTO_COMPACT_WINDOW` | 设置用于自动压缩计算的 context 容量（token）。默认为模型的 context window（标准 200K，extended context 模型为 1M）。对 1M 模型使用较低值（例如 `500000`），在压缩时将其视为 500K。上限为实际 context window。`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 作为此值的百分比应用。设置此项将压缩阈值与 status line 的 `used_percentage` 解耦 |
| `DISABLE_AUTO_COMPACT` | 禁用自动 context compaction（`1` 禁用）。手动 `/compact` 仍然有效 *(不在官方文档中——未验证)* |
| `DISABLE_COMPACT` | 禁用所有 compaction——自动和手动（`1` 禁用） |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | 启用 prompt 建议 |
| `CLAUDE_CODE_PLAN_MODE_REQUIRED` | 要求 session 使用 plan mode |
| `CLAUDE_CODE_TEAM_NAME` | agent team 的 team 名称 |
| `CLAUDE_CODE_TASK_LIST_ID` | 任务集成的 task list ID |
| `CLAUDE_ENV_FILE` | 自定义环境文件路径 |
| `FORCE_AUTOUPDATE_PLUGINS` | 强制 plugin 自动更新（`1` 启用） |
| `HTTP_PROXY` | 用于网络请求的 HTTP proxy URL |
| `HTTPS_PROXY` | 用于网络请求的 HTTPS proxy URL |
| `NO_PROXY` | 绕过 proxy 的逗号分隔主机列表 |
| `MCP_TOOL_TIMEOUT` | MCP 工具执行超时时间（ms） |
| `MCP_CLIENT_SECRET` | MCP OAuth 客户端 secret |
| `MCP_OAUTH_CALLBACK_PORT` | MCP OAuth 回调端口 |
| `IS_DEMO` | 启用 demo mode |
| `SLASH_COMMAND_TOOL_CHAR_BUDGET` | slash command 工具输出的字符 budget |
| `VERTEX_REGION_CLAUDE_3_5_HAIKU` | Claude 3.5 Haiku 的 Vertex AI region 覆盖 |
| `VERTEX_REGION_CLAUDE_3_7_SONNET` | Claude 3.7 Sonnet 的 Vertex AI region 覆盖 |
| `VERTEX_REGION_CLAUDE_4_0_OPUS` | Claude 4.0 Opus 的 Vertex AI region 覆盖 |
| `VERTEX_REGION_CLAUDE_4_0_SONNET` | Claude 4.0 Sonnet 的 Vertex AI region 覆盖 |
| `VERTEX_REGION_CLAUDE_4_1_OPUS` | Claude 4.1 Opus 的 Vertex AI region 覆盖 |

---

## Useful Commands

| Command | Description |
|---------|-------------|
| `/model` | 切换模型并调整 effort level（Opus 4.7 和 4.8） |
| `/effort` | 直接设置 effort level：`low`、`medium`、`high`、`xhigh`（Opus 4.7 和 4.8，v2.1.111）或 `max`（仅 Opus 4.6）（v2.1.76+） |
| `/config` | 交互式配置 UI；也接受 `key=value` 语法用于基于 prompt 的设置：`/config model=sonnet`（v2.1.181） |
| `/memory` | 查看/编辑所有 memory 文件 |
| `/agents` | 管理 subagent |
| `/mcp` | 管理 MCP server |
| `/hooks` | 查看已配置的 hook |
| `/plugin` | 管理 plugin |
| `claude plugin tag` | 在 marketplace 中标记 plugin 版本以进行分发。使用 plugin 名称和版本从 marketplace repo 运行（v2.1.118） |
| `claude plugin prune` | 移除其 marketplace 源不再存在的 plugin（例如 marketplace 被删除或 `extraKnownMarketplaces` 条目被移除）。清理本地 cache 并禁用孤立的 plugin（v2.1.121） |
| `claude plugin details <plugin>` | 显示 plugin 的组件清单（commands、agents、skills、hooks）及其添加的每个 session context-token 成本。用于在 managed 环境中启用 plugin 之前审计 token 开销（v2.1.139） |
| `/keybindings` | 配置自定义键盘快捷键 |
| `/skills` | 查看和管理 skill |
| `/permissions` | 查看和管理 permission rules |
| `/usage-credits` | 查看剩余的 usage credits 和限制。在 v2.1.144 中从 `/extra-usage` 重命名（旧名称仍然有效） |
| `--doctor` | 诊断配置问题 |
| `--debug` | Debug mode，带有 hook 执行详细信息 |

---

## Quick Reference: Complete Example

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "sonnet",
  "advisorModel": "fable",
  "agent": "code-reviewer",
  "language": "english",
  "cleanupPeriodDays": 30,
  "autoUpdatesChannel": "stable",
  "alwaysThinkingEnabled": true,
  "showThinkingSummaries": true,
  "viewMode": "default",
  "tui": "fullscreen",
  "awaySummaryEnabled": false,
  "includeGitInstructions": true,
  "defaultShell": "bash",
  "plansDirectory": "./plans",
  "claudeMdExcludes": ["**/vendor/**/CLAUDE.md"],
  "effortLevel": "high",
  "maxSkillDescriptionChars": 1536,
  "skillListingBudgetFraction": 0.01,
  "disableAgentView": false,
  "disableWorkflows": false,
  "workflowKeywordTriggerEnabled": true,
  "syntaxHighlightingDisabled": false,

  "worktree": {
    "symlinkDirectories": ["node_modules"],
    "sparsePaths": ["packages/my-app", "shared/utils"],
    "baseRef": "fresh",
    "bgIsolation": "worktree"
  },

  "skillOverrides": {
    "legacy-context": "name-only",
    "deploy": "off"
  },

  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0"
  },

  "autoMode": {
    "classifyAllShell": false,
    "environment": [
      "Source control: github.example.com/acme-corp and all repos under it",
      "Trusted internal domains: *.internal.example.com"
    ],
    "soft_deny": ["$defaults", "Never run terraform apply"],
    "hard_deny": ["Never run rm -rf on directories outside the project"]
  },

  "permissions": {
    "allow": [
      "Edit(*)",
      "Write(*)",
      "Bash(npm run *)",
      "Bash(git *)",
      "WebFetch(domain:*)",
      "mcp__*",
      "Agent(*)"
    ],
    "deny": [
      "Read(.env)",
      "Read(./secrets/**)"
    ],
    "additionalDirectories": ["../shared/"],
    "defaultMode": "acceptEdits"
  },

  "enableAllProjectMcpServers": true,

  "mcpServers": {
    "always-on-server": {
      "type": "http",
      "url": "https://mcp.example.com",
      "alwaysLoad": true
    }
  },

  "sshConfigs": [
    {
      "id": "dev-vm",
      "name": "Dev VM",
      "sshHost": "user@dev.example.com"
    }
  ],

  "sandbox": {
    "enabled": true,
    "excludedCommands": ["git", "docker"],
    "filesystem": {
      "denyRead": ["./secrets/"],
      "denyWrite": ["./.env"]
    }
  },

  "attribution": {
    "commit": "Generated with Claude Code",
    "pr": ""
  },
  "prUrlTemplate": "https://gitlab.example.com/{owner}/{repo}/-/merge_requests/{number}",

  "statusLine": {
    "type": "command",
    "command": "git branch --show-current"
  },

  "spinnerTipsEnabled": true,
  "spinnerTipsOverride": {
    "tips": ["Custom tip 1", "Custom tip 2"],
    "excludeDefault": false
  },
  "prefersReducedMotion": false,
  "preferredNotifChannel": "terminal_bell",

  "env": {
    "NODE_ENV": "development",
    "CLAUDE_CODE_EFFORT_LEVEL": "medium",
    "ANTHROPIC_BEDROCK_SERVICE_TIER": "priority",
    "CLAUDE_CODE_ENABLE_AUTO_MODE": "1"
  }
}
```

---

## Sources

- [Claude Code Settings Documentation](https://code.claude.com/docs/en/settings)
- [Claude Code Settings JSON Schema](https://json.schemastore.org/claude-code-settings.json)
- [Claude Code Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code GitHub Settings Examples](https://github.com/feiskyer/claude-code-settings)
- [Claude Code Environment Variables Reference](https://code.claude.com/docs/en/env-vars)
- [Claude Code Permissions Reference](https://code.claude.com/docs/en/permissions)
