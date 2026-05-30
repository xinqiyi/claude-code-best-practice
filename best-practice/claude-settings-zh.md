# 设置最佳实践

![最后更新](https://img.shields.io/badge/最后更新-2026年5月21日%2000%3A27%20PKT-white?style=flat&labelColor=555) ![版本](https://img.shields.io/badge/Claude_Code-v2.1.145-blue?style=flat&labelColor=555)<br>
[![已实现](https://img.shields.io/badge/已实现-2ea44f?style=flat)](../.claude/settings.json)

Claude Code 的 `settings.json` 文件中所有可用配置选项的全面指南。截至 v2.1.145，Claude Code 提供 **80 多项设置**和 **180 多个环境变量**（使用 `settings.json` 中的 `"env"` 字段，无需包装脚本）。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [设置层级](#settings-hierarchy)
2. [核心配置](#core-configuration)
3. [权限](#permissions)
4. [Hooks](#hooks)
5. [MCP 服务器](#mcp-servers)
6. [沙箱](#sandbox)
7. [插件](#plugins)
8. [模型配置](#model-configuration)
9. [显示与用户体验](#display--ux)
10. [AWS 与云凭证](#aws--cloud-credentials)
11. [环境变量](#environment-variables-via-env)
12. [实用命令](#useful-commands)

---

## 设置层级

设置按优先级顺序应用（从高到低）：

| 优先级 | 位置 | 作用域 | 是否共享？ | 用途 |
|----------|----------|-------|---------|---------|
| 1 | 托管设置 | 组织 | 是（由 IT 部署） | 不可覆盖的安全策略 |
| 2 | 命令行参数 | 会话 | 不适用 | 临时单会话覆盖 |
| 3 | `.claude/settings.local.json` | 项目 | 否（git 忽略） | 个人项目特定设置 |
| 4 | `.claude/settings.json` | 项目 | 是（已提交） | 团队共享设置 |
| 5 | `~/.claude/settings.json` | 用户 | 不适用 | 全局个人默认设置 |

**托管设置**由组织实施，任何其他层级（包括命令行参数）都无法覆盖。交付方式：
- **服务端托管**设置（远程交付）
- **MDM 配置文件** — macOS plist 位于 `com.anthropic.claudecode`
- **注册表策略** — Windows `HKLM\SOFTWARE\Policies\ClaudeCode`（管理员）和 `HKCU\SOFTWARE\Policies\ClaudeCode`（用户级别，最低策略优先级）
- **文件** — `managed-settings.json` 和 `managed-mcp.json`（macOS：`/Library/Application Support/ClaudeCode/`，Linux/WSL：`/etc/claude-code/`，Windows：`C:\Program Files\ClaudeCode\`）
- **丢入目录** — `managed-settings.d/` 与 `managed-settings.json` 同级，用于独立的策略片段（v2.1.83）。遵循 systemd 约定，首先合并 `managed-settings.json` 作为基础，然后按字母顺序排序丢入目录中的所有 `*.json` 文件并合并覆盖。对于标量值，后文件覆盖前文件；数组进行拼接并去重；对象进行深度合并。以 `.` 开头的隐藏文件被忽略。使用数字前缀控制合并顺序（例如 `10-telemetry.json`、`20-security.json`）

在托管层级内，优先级为：服务端托管 > MDM/OS 级别策略 > 基于文件（`managed-settings.d/*.json` + `managed-settings.json`）> HKCU 注册表（仅 Windows）。仅使用一个托管来源；来源不会跨层级合并。在基于文件的层级内，丢入文件和基础文件一起合并。

> **注意：** 自 v2.1.75 起，已弃用的 Windows 回退路径 `C:\ProgramData\ClaudeCode\managed-settings.json` 已被移除。请使用 `C:\Program Files\ClaudeCode\managed-settings.json` 替代。

> **注意（v2.1.126）：** `/config` 现在将更改持久化到 `~/.claude/settings.json`，而非仅保留在内存中。通过交互式配置界面所做的编辑在重启后仍然有效。

**仅托管策略键：**

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `parentSettingsBehavior` | string | `"first-wins"` | 控制当同时存在管理员部署的托管层级时，由嵌入宿主进程（SDK 父进程）以编程方式提供的托管设置是否生效。`"first-wins"`：丢弃父进程提供的设置，仅应用管理员层级。`"merge"`：父进程提供的设置在管理员层级下应用，且经过过滤以确保它们能**收紧**策略但不能放宽策略。需要 v2.1.133+ |
| `policyHelper` | object | - | 管理员部署的可执行文件，在启动时动态计算托管设置。对象格式：`{path: string}`，指向辅助二进制文件。仅从 MDM 或系统 `managed-settings.json` 文件中生效（绝不从用户/项目设置中读取）。辅助输出在每次启动时合并到托管层级中。需要 v2.1.136+ |

**重要说明**：
- `deny` 规则具有最高安全优先级，不能被低优先级的 allow/ask 规则覆盖。
- 即使本地文件指定了不同的值，托管设置也可能锁定或覆盖本地行为。
- 数组设置（例如 `permissions.allow`）在各作用域之间进行**拼接和去重** — 所有层级的条目会被合并而非替换。

---

## 核心配置

### 通用设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `$schema` | string | - | 用于 IDE 验证和自动补全的 JSON Schema URL（例如 `"https://json.schemastore.org/claude-code-settings.json"`） |
| `model` | string | `"default"` | 覆盖默认模型。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID |
| `agent` | string | - | 设置主对话的默认 agent。值为 `.claude/agents/` 中的 agent 名称。也可通过 `--agent` CLI 标志使用 |
| `language` | string | `"english"` | Claude 的首选响应语言。同时设置语音听写语言和终端标签标题（v2.1.121） |
| `claudeMdExcludes` | array | - | 加载[记忆](https://code.claude.com/docs/en/memory)时要跳过的 `CLAUDE.md` 文件的 glob 模式或绝对路径。模式针对绝对文件路径进行匹配。仅适用于用户、项目和本地记忆；托管策略文件无法排除。示例：`["**/vendor/**/CLAUDE.md"]` |
| `cleanupPeriodDays` | number | `30` | 启动清理扫描的期限（最小 1）。不活跃的会话记录和孤立的 subagent worktree 将被删除；自 v2.1.117 起，扫描也覆盖 `~/.claude/tasks/`、`~/.claude/shell-snapshots/` 和 `~/.claude/backups/`。设置为 `0` 将被拒绝并显示验证错误。要在非交互模式（`-p`）中禁用会话记录写入，使用 `--no-session-persistence` 或 SDK 选项 `persistSession: false` |
| `autoUpdatesChannel` | string | `"latest"` | 发布渠道：`"stable"` 或 `"latest"` |
| `minimumVersion` | string | - | 阻止自动更新程序降级到低于特定版本。切换到稳定渠道并选择保持在当前版本直至稳定版追赶上来时自动设置。与 `autoUpdatesChannel` 一起使用 |
| `alwaysThinkingEnabled` | boolean | `false` | 默认为所有会话启用扩展思考 |
| `skipWebFetchPreflight` | boolean | `false` | 在获取 URL 之前跳过 WebFetch 黑名单检查 *(存在于 JSON schema 中，不在官方设置页面上)* |
| `availableModels` | array | - | 限制用户可通过 `/model`、`--model`、Config 工具或 `ANTHROPIC_MODEL` 选择的模型。不影响默认选项。示例：`["sonnet", "haiku"]` |
| `fastModePerSessionOptIn` | boolean | `false` | 要求用户每个会话选择加入快速模式 |
| `defaultShell` | string | `"bash"` | 输入框 `!` 命令的默认 shell。接受 `"bash"`（默认）或 `"powershell"`。设置为 `"powershell"` 会将交互式 `!` 命令通过 PowerShell 路由。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`（v2.1.84）。**v2.1.120：** 当 PowerShell 可用时，即使未安装 Git for Windows，也会在 Windows 上用作回退 shell。**v2.1.126：** 当 PowerShell 启用时，它被视为*主要* shell 而不是默认使用 Bash。PowerShell 7 检测现在也涵盖 Microsoft Store 安装、不在 PATH 中的 MSI 安装以及 `.NET` 全局工具安装 |
| `includeGitInstructions` | boolean | `true` | 在 Claude 的系统提示中包含内置的提交和 PR 工作流指令以及 git 状态快照。设置时，`CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` 环境变量优先于此设置 |
| `voice` | object | - | 语音听写配置。包含三个字段的对象：`enabled`（boolean — 按讲开关）、`mode`（string — `"hold"` 按住说话或 `"tap"` 点击切换）和 `autoSubmit`（boolean — 听写结束时立即提交转录文本）。运行 `/voice` 时自动写入。需要 Claude.ai 账户（v2.1.118 扩展了结构） |
| `voiceEnabled` | boolean | - | **已弃用** — 旧版别名，等同于 `voice.enabled`。请改用 `voice` 对象以获取 `mode` 和 `autoSubmit` 控制 |
| `showClearContextOnPlanAccept` | boolean | `false` | 在计划接受屏幕上显示"清除上下文"选项。设置为 `true` 可恢复此选项（自 v2.1.81 起默认隐藏） |
| `viewMode` | string | - | 启动时的默认转录查看模式：`"default"`、`"verbose"` 或 `"focus"`。设置后覆盖粘性 Ctrl+O 选择 |
| `disableDeepLinkRegistration` | string | - | 设置为 `"disable"` 可阻止 Claude Code 在启动时向操作系统注册 `claude-cli://` 协议处理程序。深度链接允许外部工具通过 `claude-cli://open?q=...` 打开带有预填提示的 Claude Code 会话。`q` 参数支持使用 URL 编码换行符（`%0A`）的多行提示。适用于协议处理程序注册受限或单独管理的环境 |
| `showThinkingSummaries` | boolean | `false` | 在交互式会话中显示扩展思考摘要。未设置或为 `false`（交互模式默认）时，思考块被 API 编辑并显示为折叠的存根。编辑只改变你所见的内容，不影响模型生成的内容 — 要减少思考消耗，请降低预算或禁用思考。非交互模式（`-p`）和 SDK 调用者无论此设置如何始终接收摘要 |
| `disableSkillShellExecution` | boolean | `false` | 禁用来自用户、项目、插件或附加目录来源的 skill 和自定义命令中的内联 shell 执行（`` !`...` `` 和 `` ```! `` 块）。命令被替换为 `[shell 命令执行已被策略禁用]` 而非执行。内置和托管 skill 不受影响（v2.1.91） |
| `skillOverrides` | string | - | 控制自动 skill 调用行为。值：`"off"`（完全不调用 skill）、`"user-invocable-only"`（仅运行用户通过 `/skill-name` 显式调用的 skill；通过 skill 描述进行的自动发现被禁用）、`"name-only"`（skill 仅通过精确名称匹配；基于描述的自动发现被禁用）。用于更严格地控制模型加载或运行哪些 skill（v2.1.129） |
| `forceRemoteSettingsRefresh` | boolean | `false` | **（仅托管）** 阻止 CLI 启动，直到远程托管设置被重新获取。如果获取失败，CLI 退出（故障关闭）。用于企业环境，其中策略执行必须在任何会话开始前保持最新（v2.1.92） |
| `wslInheritsWindowsSettings` | boolean | `false` | **（仅 Windows 托管设置）** 当为 `true` 时，WSL 上的 Claude Code 除了 `/etc/claude-code` 外，还从 Windows 策略链（HKLM 注册表 + `C:\Program Files\ClaudeCode\managed-settings.json`）读取托管设置，Windows 来源优先级更高。仅在 HKLM 注册表键或 `C:\Program Files\ClaudeCode\managed-settings.json` 中设置时生效，这两者都需要 Windows 管理员权限才能写入。要使 HKCU 策略也适用于 WSL，还需在 HKCU 自身中设置该标志。对原生 Windows 无影响（v2.1.118） |
| `tui` | string | `"default"` | 渲染模式：`"fullscreen"` 或 `"default"`。通过 `/tui fullscreen` 设置，可实现无闪烁的替代屏幕渲染（v2.1.110） |
| `awaySummaryEnabled` | boolean | `true` | 当用户离开后返回时生成"离开摘要"（空闲会话回顾）。设置为 `false` 可选择退出。与 `CLAUDE_CODE_ENABLE_AWAY_SUMMARY` 环境变量配合使用（v2.1.110） |
| `skillOverrides` | object | - | 按 skill 名称键控的每 skill 可见性覆盖。值为 `"on"`（完全可见）、`"name-only"`（可见但不会自动描述）、`"user-invocable-only"`（对模型发现隐藏但仍可通过斜杠命令调用）或 `"off"`（完全隐藏）。示例：`{"legacy-context": "name-only", "deploy": "off"}`（v2.1.129） |
| `disableRemoteControl` | boolean | `false` | 禁用[远程控制](https://code.claude.com/docs/en/remote-control)：阻止 `claude remote-control`、`--remote-control` 标志、自动启动和会话内切换。通常放在托管设置中以实现按设备 MDM 执行，但适用于任何作用域（v2.1.128） |
| `feedbackSurveyRate` | number | - | 符合条件的会话出现会话质量调查的概率（0–1）。企业管理员可以控制调查显示的频率。示例：`0.05` = 5% 的符合条件的会话 |

**示例：**
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

### 计划与记忆目录

将计划和自动记忆文件存储在自定义位置。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `plansDirectory` | string | `~/.claude/plans` | `/plan` 输出存储的目录 |
| `autoMemoryDirectory` | string | - | 自动记忆存储的自定义目录。接受 `~/` 展开路径。不接受项目设置（`.claude/settings.json`）中的值，以防止将记忆写入重定向到敏感位置；可从策略、本地和用户设置中接受 |
| `autoMemoryEnabled` | boolean | `true` | 启用[自动记忆](https://code.claude.com/docs/en/memory)。当为 `false` 时，Claude 不会读取或写入自动记忆目录。也可以在会话期间通过 `/memory` 切换，或通过 `CLAUDE_CODE_DISABLE_AUTO_MEMORY` 环境变量禁用 |

**示例：**
```json
{
  "plansDirectory": "./my-plans"
}
```

**使用场景：** 用于将计划工件与 Claude 的内部文件分开组织，或将计划保存在共享的团队位置。

### Worktree 设置

配置 `--worktree` 如何创建和管理 git worktree。有助于减少大型单仓库中的磁盘使用和启动时间。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `worktree.symlinkDirectories` | array | `[]` | 从主仓库符号链接到每个 worktree 的目录，以避免在磁盘上重复大型目录 |
| `worktree.sparsePaths` | array | `[]` | 通过 git sparse-checkout（cone 模式）在每个 worktree 中检出的目录。只有列出的路径会被写入磁盘 |
| `worktree.baseRef` | string | `"fresh"` | 新 worktree 从哪个 ref 分支。`"fresh"` 从 `origin/<默认分支>` 分支，以获得与远程匹配的干净树。`"head"` 从你当前的本地 `HEAD` 分支，包括未提交但已追踪的更改（v2.1.133） |
| `worktree.bgIsolation` | string | `"worktree"` | [后台会话](https://code.claude.com/docs/en/agent-view)的隔离模式。`"worktree"`（默认）阻止在主检出中的 `Edit`/`Write`，直到调用 `EnterWorktree`；`"none"` 让后台任务直接编辑工作副本（v2.1.143） |

**示例：**
```json
{
  "worktree": {
    "symlinkDirectories": ["node_modules", ".cache"],
    "sparsePaths": ["packages/my-app", "shared/utils"]
  }
}
```

### 归属设置

自定义 git 提交和拉取请求的归属消息。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `attribution.commit` | string | Co-authored-by | Git 提交归属（支持尾部标注） |
| `attribution.pr` | string | 生成的消息 | 拉取请求描述归属 |
| `prUrlTemplate` | string | - | URL 模板，控制提交归属中的"PR"徽章如何链接到拉取请求 UI。支持仓库主机、所有者、仓库和 PR 编号的占位符。适用于自托管的 GitLab/Bitbucket/GitHub Enterprise 实例，其中默认的 `https://github.com/...` URL 不适用（v2.1.119） |
| `includeCoAuthoredBy` | boolean | `true` | **已弃用** — 请使用 `attribution` 替代 |

**示例：**
```json
{
  "attribution": {
    "commit": "Generated with AI\n\nCo-Authored-By: Claude <noreply@anthropic.com>",
    "pr": "Generated with Claude Code"
  }
}
```

**注意：** 设置为空字符串（`""`）可完全隐藏归属。

### 认证辅助工具

用于动态认证令牌生成的脚本。

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `apiKeyHelper` | string | 输出认证令牌的 shell 脚本路径（作为 `X-Api-Key` 标头发送） |
| `forceLoginMethod` | string | 将登录限制为 `"claudeai"` 或 `"console"` 账户 |
| `forceLoginOrgUUID` | string \| array | 要求登录属于特定组织。接受单个 UUID 字符串（也会在登录时预选该组织）或 UUID 数组（任何列出的组织都可接受，无需预选）。在托管设置中设置时，如果认证账户不属于列出的组织，登录将失败；空数组将故障关闭并显示配置错误信息阻止登录 |
| `gcpAuthRefresh` | string | 自定义脚本，在 GCP 应用默认凭证过期或无法加载时刷新它们。由 Claude Code 在重试认证之前运行。当 ADC 生命周期短且需要组织特定的辅助工具来续期时很有用。示例：`"gcloud auth application-default login"` |

**示例：**
```json
{
  "apiKeyHelper": "/bin/generate_temp_api_key.sh",
  "forceLoginMethod": "console",
  "forceLoginOrgUUID": ["xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy"]
}
```

### 公司公告

在启动时向用户显示自定义公告（随机轮换）。

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `companyAnnouncements` | array | 启动时显示的字符串数组 |

**示例：**
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

## 权限

控制 Claude 可以执行哪些工具和操作。

### 权限结构

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

### 权限键

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `permissions.allow` | array | 允许不经提示使用工具的规则 |
| `permissions.ask` | array | 需要用户确认的规则 |
| `permissions.deny` | array | 阻止使用工具的规则（最高优先级） |
| `permissions.additionalDirectories` | array | Claude 可以访问的额外目录 |
| `permissions.defaultMode` | string | 默认权限模式。在远程环境中，仅 `acceptEdits` 和 `plan` 生效（v2.1.70+） |
| `permissions.disableBypassPermissionsMode` | string | 阻止绕过模式激活 |
| `permissions.skipDangerousModePermissionPrompt` | boolean | 跳过通过 `--dangerously-skip-permissions` 或 `defaultMode: "bypassPermissions"` 进入绕过权限模式前显示的确认提示。在项目设置（`.claude/settings.json`）中设置时被忽略，以防止不受信任的仓库自动绕过提示 |
| `allowManagedPermissionRulesOnly` | boolean | **（仅托管）** 仅托管权限规则适用；用户/项目的 `allow`、`ask`、`deny` 规则被忽略 |
| `autoMode` | object | 自定义[自动模式](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode)分类器阻止和允许的内容。包含 `environment`（受信任的基础设施描述）、`allow`（阻止规则的例外）、`soft_deny`（阻止规则）和 `hard_deny`（无条件阻止规则 — 不能被 `allow` 例外或 `$defaults` 哨兵覆盖，v2.1.136） — 都是 prose 字符串数组。**不会从共享项目设置**（`.claude/settings.json`）中读取，以防止仓库注入。可在用户、本地和托管设置中使用。设置 `allow` 或 `soft_deny` **会替换**该部分的整个默认列表，除非你在数组中包含文字字符串 `"$defaults"` — 哨兵会在该位置继承内置规则，从而将自定义条目添加到它们旁边（v2.1.118）。运行 `claude auto-mode defaults` 可在自定义前查看内置规则 |
| `disableAutoMode` | string | 设置为 `"disable"` 可防止[自动模式](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode)被激活。从 `Shift+Tab` 循环中移除 `auto`，并在启动时拒绝 `--permission-mode auto`。可在任何设置级别设置；在用户无法覆盖的托管设置中最有用 |
| `useAutoModeDuringPlan` | boolean | 当自动模式可用时，计划模式是否使用自动模式语义。默认值：`true`。不会从共享项目设置（`.claude/settings.json`）中读取。在 `/config` 中显示为"在计划期间使用自动模式" |

### 权限模式

| 模式 | 行为 |
|------|----------|
| `"default"` | 标准权限检查，带提示 |
| `"acceptEdits"` | 自动接受文件编辑**以及常见文件系统命令**（`mkdir`、`touch`、`mv`、`cp` 等）针对工作目录或 `additionalDirectories` 中的路径 |
| `"dontAsk"` | 自动拒绝工具，除非通过 `/permissions` 或 `permissions.allow` 规则预先批准 |
| `"bypassPermissions"` | 跳过所有权限检查（危险）。对受保护路径（`.git`、`.claude`、`.vscode`、`.idea`、`.husky`）的写入仍会提示。自 v2.1.121 起，对 `.claude/commands/`、`.claude/agents/`、`.claude/skills/` 和 `.claude/worktrees/` 的写入被明确豁免于受保护路径提示，因为 Claude 在创建 skill、subagent 和命令时经常写入这些位置。**v2.1.126** 进一步扩展了豁免：对 `.claude/`、`.git/`、`.vscode/` 和 shell 配置文件（例如 `.bashrc`、`.zshrc`）的写入在 `--dangerously-skip-permissions` 下不再提示。针对文件系统根目录或主目录的删除操作（`rm -rf /`、`rm -rf ~`）仍会提示，作为防止模型错误的熔断器 |
| `"auto"` | 自动批准工具调用，并进行后台安全检查，验证操作与你的请求一致。研究预览。分类器自动批准只读操作和文件编辑；其他所有内容都经过安全检查。在连续 3 次或累计 20 次阻止后回退到提示。自 v2.1.111 起位于默认的 `Shift+Tab` 权限模式循环中（`--enable-auto-mode` 标志已在 v2.1.111 中移除 — 使用 `--permission-mode auto` 以此模式启动）。通过 `autoMode` 设置进行配置 |
| `"plan"` | 只读探索模式。自 v2.1.136 起，即使存在匹配的 `Edit(...)` 允许规则，文件写入也会被阻止 — 计划模式现在会覆盖显式允许规则以保持其只读保证 |

### 工具权限语法

| 工具 | 语法 | 示例 |
|------|--------|----------|
| `Bash` | `Bash(命令模式)` | `Bash(npm run *)`、`Bash(* install)`、`Bash(git * main)` |
| `PowerShell` | `PowerShell(cmd *)` | `PowerShell(Get-ChildItem *)`、`PowerShell(git commit *)` — 与 Bash 相同的格式；常见别名被规范化（`gci`/`ls`/`dir` → `Get-ChildItem`），并且 PowerShell AST 被解析，因此 `|`/`;`/`&&`/`||` 链中的每个子命令都必须匹配 |
| `Read` | `Read(路径模式)` | `Read(.env)`、`Read(./secrets/**)` |
| `Edit` | `Edit(路径模式)` | `Edit(src/**)`、`Edit(*.ts)` |
| `Write` | `Write(路径模式)` | `Write(*.md)`、`Write(./docs/**)` |
| `NotebookEdit` | `NotebookEdit(模式)` | `NotebookEdit(*)` |
| `WebFetch` | `WebFetch(domain:模式)` | `WebFetch(domain:example.com)` |
| `WebSearch` | `WebSearch` | 全局网络搜索 |
| `Task` | `Task(agent-name)` | `Task(Explore)`、`Task(my-agent)` |
| `Agent` | `Agent(名称)` | `Agent(researcher)`、`Agent(*)` — 权限限定于 subagent 生成 |
| `Skill` | `Skill(skill-name)` 或 `Skill(前缀 *)` | `Skill(weather-fetcher)`、`Skill(weather *)` 匹配 `weather-fetcher`/`weather-svg-creator`（v2.1.139） |
| `MCP` | `mcp__server__tool` 或 `MCP(server:tool)` | `mcp__memory__*`、`MCP(github:*)` |

**评估顺序：** 规则按顺序评估：先 deny 规则，然后 ask，最后 allow。第一个匹配的规则获胜。

**Read/Edit 路径模式：** `Read`、`Edit` 和 `Write` 的权限规则支持四种前缀类型的 gitignore 风格模式：

| 前缀 | 含义 | 示例 |
|--------|---------|---------|
| `//` | 从文件系统根目录的绝对路径 | `Read(//Users/alice/file)` |
| `~/` | 相对于主目录 | `Read(~/.zshrc)` |
| `/` | 相对于项目根目录 | `Edit(/src/**)` |
| `./` 或 无 | 相对路径（当前目录） | `Read(.env)`、`Read(*.ts)` |

**符号链接解析：** 权限规则同时检查符号链接路径及其解析后的目标。**允许**规则仅在符号链接及其目标都匹配时才适用 — 指向允许目录外部的符号链接仍会提示。**拒绝**规则在符号链接或其目标之一匹配时即适用 — 指向被拒绝文件的符号链接本身也被拒绝。

**Bash 通配符说明：**
- `*` 可以出现在**任意位置**：前缀（`Bash(* install)`）、后缀（`Bash(npm *)`）或中间（`Bash(git * main)`）
- **单词边界：** `Bash(ls *)`（`*` 前有空格）匹配 `ls -la` 但不匹配 `lsof`；`Bash(ls*)`（无空格）两者都匹配
- `Bash(*)` 被视为等同于 `Bash`（匹配所有 bash 命令）
- 权限规则支持输出重定向：`Bash(python:*)` 匹配 `python script.py > output.txt`
- 旧版 `:*` 后缀语法（例如 `Bash(npm:*)`）等同于 ` *` 但已弃用
- **复合命令：** shell 运算符（`&&`、`||`、`;`、`|`、`|&`、`&` 和换行符）分割命令，每个子命令必须独立匹配 — `Bash(safe-cmd *)` **不**授权 `safe-cmd && other-cmd`
- **进程包装器：** `timeout`、`time`、`nice`、`nohup` 和 `stdbuf` 在匹配前被剥离（因此 `Bash(npm test *)` 也匹配 `timeout 30 npm test`）；裸 `xargs`（无标志）也被剥离。执行包装器 `watch`、`setsid`、`ionice`、`flock` 和带 `-exec`/`-delete` 的 `find` 始终提示，不能通过前缀规则批准

**示例：**
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

Hook 配置（事件、属性、匹配器、退出码、环境变量和 HTTP hooks）维护在专用仓库中：

> **[claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks)** — 完整的 hook 参考，包含声音通知系统、所有 25 个 hook 事件、HTTP hooks、匹配器模式、退出码和环境变量。

Hook 相关的设置键（`hooks`、`disableAllHooks`（同时禁用任何自定义状态行）、`allowManagedHooksOnly`、`allowedHttpHookUrls`、`httpHookAllowedEnvVars`）在此处有文档。

有关官方 hooks 参考，请参阅 [Claude Code Hooks 文档](https://code.claude.com/docs/en/hooks)。

---

## MCP 服务器

配置模型上下文协议服务器以扩展功能。

> **OAuth（v2.1.111）：** 通过 OAuth 进行认证的 MCP 服务器遵循 [RFC 9728](https://datatracker.ietf.org/doc/rfc9728/) 进行受保护资源元数据发现。符合规范的服务器在 `/.well-known/oauth-protected-resource` 下暴露授权端点，Claude Code 会自动完成 OAuth 流程 — 符合规范的服务器无需手动的 `apiKeyHelper` 或 `headersHelper` 脚本。

> **保留的服务器名称（v2.1.128）：** `workspace` 是保留的 MCP 服务器名称。用户定义的此名称的服务器在加载时被跳过，并在会话日志中记录警告。重命名任何预先存在的 `workspace` 服务器以避免冲突。

> **`.mcp.json` 热重载（v2.1.139）：** `/mcp` 重新连接操作现在在重新连接前重新从磁盘读取 `.mcp.json`，因此添加或编辑服务器不再需要会话重启。Claude Code 还将 `CLAUDE_PROJECT_DIR` 注入到 stdio 启动的 MCP 服务器环境中（v2.1.139），以便服务器可以解析相对于项目根目录的路径。

### MCP 设置

| 键 | 类型 | 作用域 | 描述 |
|-----|------|-------|-------------|
| `enableAllProjectMcpServers` | boolean | 任意 | 自动批准所有 `.mcp.json` 服务器 |
| `enabledMcpjsonServers` | array | 任意 | 允许列表特定服务器名称 |
| `disabledMcpjsonServers` | array | 任意 | 阻止列表特定服务器名称 |
| `allowedMcpServers` | array | 仅托管 | 带名称/命令/URL 匹配的允许列表 |
| `deniedMcpServers` | array | 仅托管 | 带匹配的阻止列表 |
| `allowManagedMcpServersOnly` | boolean | 仅托管 | 仅允许明确列在托管允许列表中的 MCP 服务器 |
| `channelsEnabled` | boolean | 仅托管 | 允许团队和企业用户使用[频道](https://code.claude.com/docs/en/channels)。未设置或为 `false` 时，无论 `--channels` 标志如何，频道消息传递都被阻止 |
| `allowedChannelPlugins` | array | 仅托管 | 可以推送消息的频道插件允许列表。设置后替换默认的 Anthropic 允许列表。未定义 = 回退到默认值，空数组 = 阻止所有频道插件。需要 `channelsEnabled: true`。每个条目是一个包含 `marketplace` 和 `plugin` 字段的对象（v2.1.84） |

### MCP 服务器匹配（托管设置）

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

### 按服务器工具加载（`alwaysLoad`，v2.1.121）

默认情况下，MCP 工具定义是延迟的（在需要时通过工具搜索加载到上下文中）。在 `.mcp.json`（或内联 `mcpServers`）中的单个 MCP 服务器条目上设置 `alwaysLoad: true`，可将该服务器从延迟中豁免 — 该服务器的每个工具都会在会话开始时提前加载，无论 `ENABLE_TOOL_SEARCH` 如何设置。适用于所有服务器类型；需要 Claude Code v2.1.121+。仅用于每轮都需要的一小组工具 — 每个提前加载的工具都会消耗原本可用于对话的上下文。

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

MCP 服务器也可以通过在该工具的 `_meta` 对象中包含 `"anthropic/alwaysLoad": true` 来将单个工具标记为始终加载 — 当只有服务器的一部分工具应绕过延迟时很有用。

**示例：**
```json
{
  "enableAllProjectMcpServers": true,
  "enabledMcpjsonServers": ["memory", "github", "filesystem"],
  "disabledMcpjsonServers": ["experimental-server"]
}
```

---

## 沙箱

配置 bash 命令沙箱以确保安全。

### 沙箱设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `sandbox.enabled` | boolean | `false` | 启用 bash 沙箱 |
| `sandbox.failIfUnavailable` | boolean | `false` | 当沙箱已启用但无法启动时，退出并显示错误，而不是在无沙箱状态下运行。对于需要严格沙箱的企业策略很有用（v2.1.83） |
| `sandbox.autoAllowBashIfSandboxed` | boolean | `true` | 沙箱启用时自动批准 bash。自 v2.1.139 起，shell 扩展形式（`$VAR`、`$(cmd)`）被正确识别，因此含有变量替换的命令在沙箱自动批准启用时不再回退到提示 |
| `sandbox.excludedCommands` | array | `[]` | 在沙箱外运行的命令 |
| `sandbox.allowUnsandboxedCommands` | boolean | `true` | 允许 `dangerouslyDisableSandbox`。当设置为 `false` 时，逃生舱被完全禁用，所有命令必须在沙箱内运行（或在 `excludedCommands` 中）。对于需要严格沙箱的企业策略很有用 |
| `sandbox.ignoreViolations` | object | `{}` | 命令模式到路径数组的映射 — 抑制违规警告 *(存在于 JSON schema 中，不在官方设置页面上)* |
| `sandbox.enableWeakerNestedSandbox` | boolean | `false` | **（仅 Linux 和 WSL2）** 为非特权 Docker 环境启用较弱的沙箱（降低安全性） |
| `sandbox.network.allowUnixSockets` | array | `[]` | **（仅 macOS）** 沙箱中可访问的特定 Unix 套接字路径。在 Linux 和 WSL2 上被忽略，因为 seccomp 过滤器无法检查套接字路径；请改用 `allowAllUnixSockets` |
| `sandbox.network.allowAllUnixSockets` | boolean | `false` | 允许所有 Unix 套接字（覆盖 `allowUnixSockets`）。在 Linux 和 WSL2 上，这是允许 Unix 套接字的唯一方式，因为它跳过了 otherwise 会阻止 `socket(AF_UNIX, ...)` 调用的 seccomp 过滤器 |
| `sandbox.network.allowLocalBinding` | boolean | `false` | 允许绑定到 localhost 端口（macOS） |
| `sandbox.network.allowedDomains` | array | `[]` | 沙箱的网络域名允许列表 |
| `sandbox.network.deniedDomains` | array | `[]` | bash 沙箱的网络域名阻止列表。优先于 `allowedDomains` 中的通配符。支持 glob 模式（例如 `"*.example.com"`）（v2.1.113） |
| `sandbox.network.httpProxyPort` | number | - | HTTP 代理端口 1-65535（自定义代理） |
| `sandbox.network.socksProxyPort` | number | - | SOCKS5 代理端口 1-65535（自定义代理） |
| `sandbox.network.allowManagedDomainsOnly` | boolean | `false` | 仅允许托管允许列表中的域名（托管设置） |
| `sandbox.network.allowMachLookup` | array | `[]` | （仅 macOS）沙箱可以查找的额外 XPC/Mach 服务名称。支持单个尾部 `*` 进行前缀匹配。需要通过 XPC 通信的工具需要，例如 iOS 模拟器或 Playwright。示例：`["com.apple.coresimulator.*"]` |
| `sandbox.filesystem.allowWrite` | array | `[]` | 沙箱命令可以写入的额外路径。数组在所有设置作用域中合并。也与 `Edit(...)` 允许权限规则中的路径合并。前缀：`/`（绝对路径）、`~/`（主目录）、`./` 或无（项目设置中相对于项目，用户设置中相对于 `~/.claude`）。旧版 `//` 绝对路径前缀仍然有效。**注意：** 这与 [Read/Edit 权限规则](#tool-permission-syntax)不同，后者使用 `//` 表示绝对路径，`/` 表示相对于项目 |
| `sandbox.filesystem.denyWrite` | array | `[]` | 沙箱命令不能写入的路径。数组在所有设置作用域中合并。也与 `Edit(...)` 拒绝权限规则中的路径合并。与 `allowWrite` 相同的路径前缀约定 |
| `sandbox.filesystem.denyRead` | array | `[]` | 沙箱命令不能读取的路径。数组在所有设置作用域中合并。也与 `Read(...)` 拒绝权限规则中的路径合并。与 `allowWrite` 相同的路径前缀约定 |
| `sandbox.filesystem.allowRead` | array | `[]` | 在 `denyRead` 区域内重新允许读取访问的路径。优先于 `denyRead`。数组在所有设置作用域中合并。与 `allowWrite` 相同的路径前缀约定 |
| `sandbox.filesystem.allowManagedReadPathsOnly` | boolean | `false` | **（仅托管）** 仅托管设置中的 `allowRead` 路径被尊重。用户、项目和本地设置中的 `allowRead` 条目被忽略 |
| `sandbox.enableWeakerNetworkIsolation` | boolean | `false` | （仅 macOS）允许访问系统 TLS 信任（`com.apple.trustd.agent`）；降低安全性 |
| `sandbox.bwrapPath` | string | - | **（仅托管，Linux/WSL2）** bubblewrap（`bwrap`）二进制文件的绝对路径。覆盖自动 `PATH` 检测。仅从托管设置中生效，而非用户或项目设置。示例：`/opt/admin/bwrap`（v2.1.133） |
| `sandbox.socatPath` | string | - | **（仅托管，Linux/WSL2）** 用于沙箱网络代理的 `socat` 二进制文件的绝对路径。覆盖自动 `PATH` 检测。仅从托管设置中生效。示例：`/opt/admin/socat`（v2.1.133） |

**示例：**
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

## 插件

配置 Claude Code 插件和市场。

### 插件设置

| 键 | 类型 | 作用域 | 描述 |
|-----|------|-------|-------------|
| `enabledPlugins` | object | 任意 | 启用/禁用特定插件 |
| `extraKnownMarketplaces` | object | 项目 | 添加自定义插件市场（通过 `.claude/settings.json` 进行团队共享） |
| `strictKnownMarketplaces` | array | 仅托管 | 允许的市场列表 |
| `skippedMarketplaces` | array | 任意 | 用户拒绝安装的市场 *(存在于 JSON schema 中，不在官方设置页面上)* |
| `skippedPlugins` | array | 任意 | 用户拒绝安装的插件 *(存在于 JSON schema 中，不在官方设置页面上)* |
| `pluginConfigs` | object | 任意 | 按插件的 MCP 服务器配置（以 `plugin@marketplace` 为键）*(存在于 JSON schema 中，不在官方设置页面上)* |
| `blockedMarketplaces` | array | 仅托管 | 阻止特定插件市场。每个条目可以按源字符串、`hostPattern` 或 `pathPattern` 匹配 — 自 v2.1.119 起，`hostPattern` 和 `pathPattern` 匹配器在任何下载触及文件系统之前被正确执行，因此被阻止的市场永远不会到达磁盘 |
| `pluginTrustMessage` | string | 仅托管 | 提示用户信任插件时显示的自定义消息 |

**市场来源类型：** `github`、`git`、`directory`、`hostPattern`、`settings`、`url`、`npm`、`file`。使用 `source: 'settings'` 可内联声明少量插件，而无需设置托管的市场仓库。

**示例：**
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

## 模型配置

### 模型别名

| 别名 | 描述 |
|-------|-------------|
| `"default"` | 针对你的账户类型推荐的模型 |
| `"sonnet"` | 最新的 Sonnet 模型（Claude Sonnet 4.6） |
| `"opus"` | 最新的 Opus 模型（Claude Opus 4.6） |
| `"haiku"` | 快速的 Haiku 模型 |
| `"sonnet[1m]"` | 具有 1M token 上下文的 Sonnet |
| `"opus[1m]"` | 具有 1M token 上下文的 Opus（自 v2.1.75 起，Max、Team 和 Enterprise 版默认） |
| `"opusplan"` | Opus 用于规划，Sonnet 用于执行 |

**示例：**
```json
{
  "model": "opus"
}
```

> **注意（v2.1.144）：** `/model` 更改**当前会话**的模型。在 `/model` 选择器中按 `d` 也可将选择设置为默认值。`model` 设置和 `ANTHROPIC_MODEL` 继续控制持久默认值。

### 模型覆盖

将 Anthropic 模型 ID 映射到 Bedrock、Vertex 或 Foundry 部署的特定提供商模型 ID。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `effortLevel` | string | - | 跨会话持久化 effort level。接受 `"low"`、`"medium"`、`"high"` 或 `"xhigh"`（仅 Opus 4.7，v2.1.111）。运行 `/effort low`、`/effort medium`、`/effort high` 或 `/effort xhigh` 时自动写入。Opus 4.6、Sonnet 4.6 和 Opus 4.7 支持。不受支持的级别回退到活跃模型上支持的最高级别 |
| `modelOverrides` | object | - | 将模型选择器条目映射到提供商特定 ID（例如 Bedrock 推理配置文件 ARN）。每个键是一个模型选择器条目名称，每个值是提供商模型 ID |

**示例：**
```json
{
  "modelOverrides": {
    "claude-opus-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-opus-4-6-v1:0",
    "claude-sonnet-4-6": "arn:aws:bedrock:us-east-1:123456789:inference-profile/anthropic.claude-sonnet-4-6-v1:0"
  }
}
```

### Effort Level

`/model` 命令暴露了一个 **effort level** 控制，用于调整模型在每个响应中应用的推理量。在 `/model` UI 中使用 ← → 箭头键循环切换 effort level。

| Effort Level | 描述 |
|-------------|-------------|
| Max | 最大推理深度，仅 Opus 4.6 |
| XHigh | 扩展高推理深度，仅 Opus 4.7（所有计划中 Opus 4.7 的默认值，v2.1.111） |
| High（Opus 4.6/Sonnet 4.6 默认） | 完整推理深度，最适合复杂任务 |
| Medium | 均衡推理，适合日常任务 |
| Low | 最小推理，最快响应 |

**如何使用：**
1. 运行 `/effort low`、`/effort medium` 或 `/effort high` 直接设置（v2.1.76+）
2. 或运行 `/model` → 选择模型 → 使用 **← →** 箭头键调节
3. 该设置通过 `settings.json` 中的 `effortLevel` 键持久化

**注意：** Effort level 在 Max 和 Team 计划上适用于 Opus 4.6、Sonnet 4.6 和 Opus 4.7。默认值在 v2.1.68 中从 High 改为 Medium，然后在 v2.1.94 中为 API 密钥、Bedrock/Vertex/Foundry、Team 和企业用户改回 **High**。在 v2.1.117 中，Pro/Max 订阅者在 Opus 4.6 和 Sonnet 4.6 上的默认值也从 `medium` 提升到 `high`，使所有层级在 `high` 上保持一致。v2.1.111 引入了 **`xhigh`**（仅 Opus 4.7）并使其成为所有计划中 Opus 4.7 的默认 effort level。自 v2.1.75 起，Opus 4.6 的 1M 上下文窗口在 Max、Team 和企业计划上默认可用。

**Effort env 传播：** 在 skill 文件中，使用 `${CLAUDE_EFFORT}` 引用当前的 effort level（v2.1.120）。自 v2.1.133 起，相同的 `$CLAUDE_EFFORT` 变量也被注入到 Bash 工具子进程和 hook 处理程序的环境中，因此 shell 脚本和 hook 命令可以基于活跃的 effort 层级调整其行为，而无需读取单独的配置文件。

### 模型环境变量

通过 `env` 键配置：

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

## 显示与用户体验

### 显示设置

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `statusLine` | object | - | 自定义状态行配置 |
| `outputStyle` | string | `"default"` | 输出样式（例如 `"Explanatory"`） |
| `spinnerTipsEnabled` | boolean | `true` | 等待时显示提示 |
| `spinnerVerbs` | object | - | 自定义 spinner 动词，包含 `mode`（"append" 或 "replace"）和 `verbs` 数组 |
| `spinnerTipsOverride` | object | - | 自定义 spinner 提示，包含 `tips`（字符串数组）和可选的 `excludeDefault`（boolean）。当 `excludeDefault` 为 `true` 时，仅显示自定义提示；当为 `false` 或不存在时，自定义提示与内置提示合并。自 v2.1.121 起，`excludeDefault: true` 也会抑制基于时间的 spinner 提示 |
| `respectGitignore` | boolean | `true` | 在文件选择器中尊重 .gitignore |
| `prefersReducedMotion` | boolean | `false` | 减少 UI 中的动画和动作效果 |
| `fileSuggestion` | object | - | 自定义文件建议命令（见下方文件建议配置） |
| `autoScrollEnabled` | boolean | `true` | 在全屏模式下自动滚动对话。设置为 `false` 可禁用自动滚动（v2.1.110）。v2.1.119 之前的版本将此存储在 `~/.claude.json` 中 |
| `editorMode` | string | `"normal"` | 输入提示的按键绑定模式：`"normal"` 或 `"vim"`。在 `/config` 中显示为**编辑器模式**。v2.1.119 之前的版本将此存储在 `~/.claude.json` 中 |
| `showTurnDuration` | boolean | `true` | 在响应后显示轮次持续时间（例如"处理了 1 分 6 秒"）。v2.1.119 之前的版本将此存储在 `~/.claude.json` 中 |
| `teammateMode` | string | `"auto"` | [agent team](https://code.claude.com/docs/en/agent-teams) 队友的显示方式：`"auto"`（在 tmux 或 iTerm2 中选择分屏，否则在进程内显示）、`"in-process"` 或 `"tmux"`。参见[选择显示模式](https://code.claude.com/docs/en/agent-teams#choose-a-display-mode)。v2.1.119 之前的版本将此存储在 `~/.claude.json` 中 |
| `terminalProgressBarEnabled` | boolean | `true` | 在支持的终端中显示终端进度条（ConEmu、Ghostty 1.2.0+ 和 iTerm2 3.6.6+）。在 `/config` 中显示为**终端进度条**。v2.1.119 之前的版本将此存储在 `~/.claude.json` 中 |
| `preferredNotifChannel` | string | `"auto"` | 任务完成和权限提示通知的方式。值：`"auto"`、`"terminal_bell"`、`"iterm2"`、`"iterm2_with_bell"`、`"kitty"`、`"ghostty"`、`"notifications_disabled"`。默认 `"auto"` 在 iTerm2、Ghostty 和 Kitty 中发送桌面通知，在其他终端中不执行任何操作。设置为 `"terminal_bell"` 可在任何终端中响铃。在 `/config` 中显示为**通知**。参见[获取终端铃声或通知](https://code.claude.com/docs/en/terminal-config#get-a-terminal-bell-or-notification) |

### 全局配置设置（`~/.claude.json`）

这些与 IDE 相关的首选项存储在 `~/.claude.json` 中，**而非** `settings.json`。

> **v2.1.119 迁移说明：** 自 v2.1.119 起，`autoScrollEnabled`、`editorMode`、`showTurnDuration`、`teammateMode` 和 `terminalProgressBarEnabled` 已移至 `settings.json`，并在上面的显示设置表中记录。旧版本将它们存储在此处。

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| `autoConnectIde` | boolean | `false` | 当 Claude Code 从外部终端启动时，自动连接到正在运行的 IDE。在 VS Code 或 JetBrains 终端之外运行时，在 `/config` 中显示为**自动连接到 IDE（外部终端）** |
| `autoInstallIdeExtension` | boolean | `true` | 当从 VS Code 终端运行时，自动安装 Claude Code IDE 扩展。在 `/config` 中显示为**自动安装 IDE 扩展**。也可以通过 `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` 环境变量禁用 |
| `externalEditorContext` | boolean | `true` | 在可用时包含关于外部编辑器的额外上下文。设置为 `false` 可禁用 |

### 工作区与团队

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `sshConfigs` | object[] | SSH 连接定义，以桌面端下拉菜单形式呈现。每个条目必须包含 `id`、`name` 和 `sshHost`；可选包含 `sshPort`、`sshIdentityFile` 和 `startDirectory` |

**字段参考：**

| 字段 | 是否必需 | 描述 |
|-------|----------|-------------|
| `id` | 是 | SSH 连接条目的唯一标识符 |
| `name` | 是 | 桌面端下拉菜单中显示的显示名称 |
| `sshHost` | 是 | SSH 主机（例如 `user@dev.example.com` 或 `dev.example.com`） |
| `sshPort` | 否 | SSH 端口号 |
| `sshIdentityFile` | 否 | SSH 身份文件（私钥）的路径 |
| `startDirectory` | 否 | 连接后的初始工作目录 |

**示例：**
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

### 状态行配置

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

| 字段 | 描述 |
|-------|-------------|
| `type` | 设置为 `"command"` 以运行 shell 脚本 |
| `command` | 生成状态行输出的 shell 命令或脚本路径 |
| `padding` | 添加到状态行内容的额外水平间距（以字符为单位）。默认为 `0`。控制超出界面内置间距的相对缩进 |
| `refreshInterval` | 除事件驱动更新外，每 N 秒重新运行一次命令。最小值为 `1`。当状态行显示基于时间的数据（例如时钟）或后台 subagent 在主会话空闲时更改 git 状态时很有用。留空则仅在事件时运行（v2.1.97） |

**状态行输入字段：**

状态行命令在 stdin 上接收一个 JSON 对象。完整的 JSON schema 和示例，请参见[状态行文档](https://code.claude.com/docs/en/statusline)。

| 字段 | 描述 |
|-------|-------------|
| `model.id`、`model.display_name` | 当前模型标识符和显示名称 |
| `cwd`、`workspace.current_dir` | 当前工作目录（两者包含相同值；建议使用 `workspace.current_dir`） |
| `workspace.project_dir` | 启动 Claude Code 的目录（如果工作目录更改，可能与 `cwd` 不同） |
| `workspace.added_dirs` | 通过 `/add-dir` 或 `--add-dir` 添加的额外目录 |
| `workspace.git_worktree` | 在使用 `git worktree add` 创建的链接 worktree 内时的 git worktree 名称。在主工作树中不存在（v2.1.97） |
| `cost.total_cost_usd` | 以美元计的总会话费用 |
| `cost.total_duration_ms` | 自会话开始以来的总挂钟时间，以毫秒计 |
| `cost.total_api_duration_ms` | 等待 API 响应的总时间，以毫秒计 |
| `cost.total_lines_added`、`cost.total_lines_removed` | 会话期间更改的代码行数 |
| `context_window.total_input_tokens`、`context_window.total_output_tokens` | 跨会话的累计 token 计数 |
| `context_window.context_window_size` | 最大上下文窗口大小，以 token 计（默认 200000，扩展上下文为 1000000） |
| `context_window.used_percentage` | 已用上下文窗口的预计算百分比 |
| `context_window.remaining_percentage` | 剩余上下文窗口的预计算百分比 |
| `context_window.current_usage` | 最近一次 API 调用的 token 计数（输入、输出、缓存 token） |
| `exceeds_200k_tokens` | 最近 API 响应的总 token 数是否超过 200k（固定阈值） |
| `rate_limits.five_hour.used_percentage` | 五小时速率限制使用百分比（v2.1.80+） |
| `rate_limits.five_hour.resets_at` | 五小时速率限制重置时间戳（Unix 纪元秒） |
| `rate_limits.seven_day.used_percentage` | 七天速率限制使用百分比 |
| `rate_limits.seven_day.resets_at` | 七天速率限制重置时间戳（Unix 纪元秒） |
| `session_id` | 唯一会话标识符 |
| `session_name` | 使用 `--name` 或 `/rename` 设置的自定义会话名称。如果未设置自定义名称则不存在 |
| `transcript_path` | 对话记录文件路径 |
| `version` | Claude Code 版本 |
| `output_style.name` | 当前输出样式的名称 |
| `vim.mode` | 启用 vim 模式时的当前 vim 模式（`NORMAL` 或 `INSERT`） |
| `agent.name` | 使用 `--agent` 标志或 agent 设置运行时的 agent 名称 |
| `effort.level` | 当前推理 effort（`low`、`medium`、`high`、`xhigh` 或 `max`）。反映实时会话值，包括会话中的 `/effort` 更改。当当前模型不支持 effort 参数时不存在（v2.1.121） |
| `thinking.enabled` | 会话是否启用了扩展思考（v2.1.121） |
| `worktree.name` | 活跃 worktree 的名称（仅在 `--worktree` 会话期间存在） |
| `worktree.path` | worktree 目录的绝对路径 |
| `worktree.branch` | worktree 的 git 分支名称。对于基于 hook 的 worktree 不存在 |
| `worktree.original_cwd` | 进入 worktree 前的目录 |
| `worktree.original_branch` | 进入 worktree 前检出的 git 分支。对于基于 hook 的 worktree 不存在 |
| `github` | 检测到的当前分支的 GitHub 仓库和拉取请求信息 — 仓库身份和关联的 PR（v2.1.145） |

### 文件建议配置

文件建议脚本在 stdin 上接收一个 JSON 对象（例如 `{"query": "src/comp"}`），并且必须输出最多 15 个文件路径（每行一个）。

```json
{
  "fileSuggestion": {
    "type": "command",
    "command": "~/.claude/file-suggestion.sh"
  },
  "respectGitignore": true
}
```

**示例：**
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

## AWS 与云凭证

### AWS 设置

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `awsAuthRefresh` | string | 刷新 AWS 认证的脚本（修改 `.aws` 目录） |
| `awsCredentialExport` | string | 输出带有 AWS 凭证的 JSON 的脚本 |

**示例：**
```json
{
  "awsAuthRefresh": "aws sso login --profile myprofile",
  "awsCredentialExport": "/bin/generate_aws_grant.sh"
}
```

### OpenTelemetry

| 键 | 类型 | 描述 |
|-----|------|-------------|
| `otelHeadersHelper` | string | 生成动态 OpenTelemetry 标头的脚本 |

**示例：**
```json
{
  "otelHeadersHelper": "/bin/generate_otel_headers.sh"
}
```

---

## 环境变量（通过 `env`）

为所有 Claude Code 会话设置环境变量。

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "...",
    "NODE_ENV": "development",
    "DEBUG": "true"
  }
}
```

### 常见环境变量

| 变量 | 描述 |
|----------|-------------|
| `ANTHROPIC_API_KEY` | 用于认证的 API 密钥 |
| `ANTHROPIC_AUTH_TOKEN` | OAuth 令牌 |
| `CLAUDE_CODE_OAUTH_TOKEN` | 用于 Claude.ai 认证的 OAuth 访问令牌。用于 SDK 和自动化环境，替代 `/login`。优先级高于钥匙串中存储的凭证 |
| `CLAUDE_CODE_OAUTH_REFRESH_TOKEN` | 用于 Claude.ai 认证的 OAuth 刷新令牌。设置后，`claude auth login` 直接交换此令牌，而无需打开浏览器。需要 `CLAUDE_CODE_OAUTH_SCOPES` |
| `CLAUDE_CODE_OAUTH_SCOPES` | 空格分隔的 OAuth 作用域，刷新令牌是用这些作用域颁发的（例如 `"user:profile user:inference user:sessions:claude_code"`）。当设置了 `CLAUDE_CODE_OAUTH_REFRESH_TOKEN` 时需要 |
| `ANTHROPIC_WORKSPACE_ID` | [工作负载身份联合](https://platform.claude.com/docs/en/manage-claude/workload-identity-federation)的工作区 ID。当联合规则限定到多个工作区时设置，以便令牌交换知道要针对哪个工作区（v2.1.141） |
| `ANTHROPIC_BASE_URL` | 自定义 API 端点 |
| `ANTHROPIC_BEDROCK_BASE_URL` | 覆盖 Bedrock 端点 URL |
| `ANTHROPIC_BEDROCK_MANTLE_BASE_URL` | 覆盖 Bedrock Mantle 端点 URL。参见 [Mantle 端点](https://code.claude.com/docs/en/amazon-bedrock#use-the-mantle-endpoint) |
| `ANTHROPIC_BEDROCK_SERVICE_TIER` | Bedrock 服务层级：`default`、`flex` 或 `priority`。作为 `X-Amzn-Bedrock-Service-Tier` 标头在每次请求中发送。参见 [Amazon Bedrock 服务层级](https://code.claude.com/docs/en/amazon-bedrock#service-tiers)（v2.1.122） |
| `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` | 由嵌入 Claude Code 并代表用户管理模型提供商路由的宿主平台设置。设置后，`settings.json` 中的提供商选择/端点/认证环境变量（例如 `CLAUDE_CODE_USE_BEDROCK`、`ANTHROPIC_BASE_URL`、`ANTHROPIC_API_KEY`）被忽略，因此用户设置无法覆盖宿主的路由。Bedrock/Vertex/Foundry 的自动遥测退出也被跳过，因此遥测遵循标准的 `DISABLE_TELEMETRY` 退出（v2.1.126） |
| `ANTHROPIC_VERTEX_BASE_URL` | 覆盖 Vertex AI 端点 URL |
| `ANTHROPIC_BETAS` | 逗号分隔的 Anthropic beta 标头值 |
| `ANTHROPIC_VERTEX_PROJECT_ID` | Vertex AI 的 GCP 项目 ID |
| `ANTHROPIC_CUSTOM_MODEL_OPTION` | 添加为 `/model` 选择器中自定义条目的模型 ID。用于使非标准或特定网关的模型可选，而不替换内置别名 |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_NAME` | `/model` 选择器中自定义模型条目的显示名称。未设置时默认为模型 ID |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` | `/model` 选择器中自定义模型条目的显示描述。未设置时默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_CUSTOM_MODEL_OPTION_SUPPORTED_CAPABILITIES` | 覆盖自定义模型条目的能力检测。逗号分隔的值（例如 `effort,thinking`）。当自定义模型支持自动检测无法确认的功能时需要。参见[模型配置](https://code.claude.com/docs/en/model-config#customize-pinned-model-display-and-capabilities) |
| `ANTHROPIC_MODEL` | 要使用的模型名称。接受别名（`sonnet`、`opus`、`haiku`）或完整模型 ID。覆盖 `model` 设置 |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 使用自定义模型 ID 覆盖 Haiku 模型别名（例如用于第三方部署） |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时，自定义 `/model` 选择器中 Haiku 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中 Haiku 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL_SUPPORTED_CAPABILITIES` | 覆盖固定 Haiku 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `CLAUDECODE` | 在 Claude Code 生成的 shell 环境（Bash 工具、tmux 会话）中设置为 `1`。不在 hooks 或状态行命令中设置。用于检测脚本是否在 Claude Code shell 内运行 |
| `CLAUDE_CODE_SESSION_ID` | 只读。在 Bash 和 PowerShell 工具子进程中自动设置为当前会话 ID。与传递给 hooks 的 `session_id` 字段匹配。在 `/clear` 时更新。用于将脚本和外部工具与启动它们的 Claude Code 会话关联（v2.1.132） |
| `AI_AGENT` | 由 Claude Code 在子进程环境（Bash 工具、hooks、MCP stdio 服务器）中自动设置。通用标志，标识父进程为 AI agent — 用于在从任何 AI agent 调用时调整行为的工具，而不是检查每个 agent 特定的变量如 `CLAUDECODE` *(位于 v2.1.120 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` | 设置为 `1` 以在组织状态检查因网络错误而失败时允许快速模式。当企业代理阻止状态端点时有用 |
| `CLAUDE_CODE_USE_BEDROCK` | 使用 AWS Bedrock（`1` 启用） |
| `CLAUDE_CODE_USE_VERTEX` | 使用 Google Vertex AI（`1` 启用） |
| `CLAUDE_CODE_USE_FOUNDRY` | 使用 Microsoft Foundry（`1` 启用） |
| `CLAUDE_CODE_USE_MANTLE` | 使用 Bedrock [Mantle 端点](https://code.claude.com/docs/en/amazon-bedrock#use-the-mantle-endpoint)（`1` 启用） |
| `CLAUDE_CODE_USE_POWERSHELL_TOOL` | 设置为 `1` 以在 Windows 上启用 PowerShell 工具（选择加入预览）。启用后，Claude 可以原生运行 PowerShell 命令，而无需通过 Git Bash 路由。仅原生 Windows 支持，不支持 WSL（v2.1.84） |
| `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY` | 设置为 `1` 可阻止 Claude Code 在生成 PowerShell 进行工具调用、hooks 和状态行命令时传递 `-ExecutionPolicy Bypass`，而是尊重机器的有效执行策略。默认情况下，Claude Code 在进程范围内绕过执行策略，以便 `.ps1` 脚本和模块导入在默认受限的 Windows 上工作。永远不会覆盖组策略 `MachinePolicy`/`UserPolicy`（v2.1.143） |
| `CLAUDE_CODE_REMOTE` | 只读。当 Claude Code 作为云会话运行时自动设置为 `true`。从 hook 或设置脚本中读取此项以检测是否在云环境中 |
| `CLAUDE_CODE_REMOTE_SESSION_ID` | 只读。在云会话中自动设置为当前会话 ID。读取此项以构造回链到会话记录的链接 |
| `CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX` | 自动生成的远程控制会话名称的前缀。默认为机器主机名 |
| `CLAUDE_CODE_ENABLE_TELEMETRY` | 启用/禁用遥测（`0` 或 `1`） |
| `DISABLE_ERROR_REPORTING` | 禁用错误报告（`1` 禁用） |
| `DISABLE_AUTOUPDATER` | 设置为 `1` 以禁用针对 npm 注册表的自动更新检查。也可配置为仅启动时变量 — 参见 [CLI 启动标志](./claude-cli-startup-flags.md#environment-variables) |
| `DISABLE_UPDATES` | 设置为 `1` 以完全阻止所有更新路径 — 自动检查、通知和手动 `claude update`。比 `DISABLE_AUTOUPDATER` 更严格，后者仅禁用后台检查。用于必须阻止所有更新直到明确重新启用的环境 *(位于 v2.1.118 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` | 设置为 `1` 以让 Claude Code 在有新版本可用时在后台运行包管理器的升级命令。适用于 Homebrew 和 WinGet 安装。其他包管理器继续显示升级命令而不执行。参见[自动更新](https://code.claude.com/docs/en/setup#auto-updates)（v2.1.129） |
| `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` | 设置为 `1` 以在 `ANTHROPIC_BASE_URL` 指向 Anthropic 兼容的网关（如 LiteLLM、Kong 或内部代理）时，从网关的 `/v1/models` 端点填充 `/model` 选择器。默认关闭，因为由共享 API 密钥支持的网关会暴露密钥可以访问的每个模型。发现的模型仍由 `availableModels` 允许列表过滤（v2.1.129，从先前的自动发现中选择了加入） |
| `DISABLE_TELEMETRY` | 禁用遥测（`1` 禁用） |
| `MCP_TIMEOUT` | MCP 启动超时时间（毫秒） |
| `MAX_MCP_OUTPUT_TOKENS` | 最大 MCP 输出 token（默认：25000）。当输出超过 10000 token 时显示警告 |
| `API_TIMEOUT_MS` | API 请求超时时间（毫秒）（默认：600000） |
| `BASH_MAX_TIMEOUT_MS` | Bash 命令超时时间 |
| `BASH_MAX_OUTPUT_LENGTH` | 最大 bash 输出长度 |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 自动压缩阈值百分比（1-100）。默认约为 95%。设置更低（例如 `50`）可更早触发压缩。高于 95% 的值无效。使用 `/context` 监控当前使用情况。示例：`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50 claude` |
| `CLAUDE_CODE_MAX_CONTEXT_TOKENS` | 覆盖 Claude Code 为活跃模型假定的上下文窗口大小。仅当同时设置 `DISABLE_COMPACT` 时生效。当通过 `ANTHROPIC_BASE_URL` 路由到其上下文窗口与其名称的内置大小不匹配的模型时使用 |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | 在 bash 调用之间保持 cwd（`1` 启用） |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | 禁用后台任务（`1` 禁用） |
| `ENABLE_TOOL_SEARCH` | MCP 工具搜索阈值（例如 `auto:5`） |
| `ENABLE_PROMPT_CACHING_1H` | 选择加入 1 小时提示缓存 TTL。替换已弃用的 `ENABLE_PROMPT_CACHING_1H_BEDROCK` *(位于 v2.1.108 更新日志中，尚未在官方环境变量页面上)* |
| `FORCE_PROMPT_CACHING_5M` | 强制 5 分钟提示缓存 TTL *(位于 v2.1.108 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_ENABLE_AWAY_SUMMARY` | 退出离开摘要/空闲会话回顾。设置为 `0` 可禁用。与 `awaySummaryEnabled` 设置配合使用（v2.1.110） |
| `DISABLE_PROMPT_CACHING` | 禁用所有提示缓存（`1` 禁用） |
| `DISABLE_PROMPT_CACHING_HAIKU` | 禁用 Haiku 提示缓存 |
| `DISABLE_PROMPT_CACHING_SONNET` | 禁用 Sonnet 提示缓存 |
| `DISABLE_PROMPT_CACHING_OPUS` | 禁用 Opus 提示缓存 |
| `ENABLE_PROMPT_CACHING_1H_BEDROCK` | 在 Bedrock 上请求 1 小时缓存 TTL（`1` 启用）*(不在官方文档中 — 未验证；v2.1.108 更新日志称已弃用，由 `ENABLE_PROMPT_CACHING_1H` 替代)* |
| `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` | 禁用实验性 beta 功能（`1` 禁用） |
| `CLAUDE_CODE_SHELL` | 覆盖自动 shell 检测 |
| `CLAUDE_CODE_FILE_READ_MAX_OUTPUT_TOKENS` | 覆盖默认文件读取 token 限制 |
| `CLAUDE_CODE_GLOB_HIDDEN` | 设置为 `false` 以在 Claude 调用 Glob 工具时从结果中排除点文件。默认包含。不影响 `@` 文件自动补全、`ls`、Grep 或 Read |
| `CLAUDE_CODE_GLOB_NO_IGNORE` | 设置为 `false` 以使 Glob 工具尊重 `.gitignore` 模式。默认情况下，Glob 返回所有匹配文件，包括 git 忽略的文件。不影响 `@` 文件自动补全，后者有自己的 `respectGitignore` 设置 |
| `CLAUDE_CODE_GLOB_TIMEOUT_SECONDS` | Glob 文件发现的超时时间（秒） |
| `CLAUDE_CODE_ENABLE_TASKS` | 设置为 `true` 以在非交互模式（`-p` 标志）中启用任务跟踪。任务在交互模式下默认开启 |
| `CLAUDE_CODE_SIMPLE` | 设置为 `1` 以使用最小的系统提示以及仅 Bash、文件读取和文件编辑工具运行。也可配置为仅启动时变量 — 参见 [CLI 启动标志](./claude-cli-startup-flags.md#environment-variables) |
| `CLAUDE_CODE_EXIT_AFTER_STOP_DELAY` | 空闲后自动退出 SDK 模式（毫秒） |
| `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING` | 禁用自适应思考（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_THINKING` | 强制禁用扩展思考（`1` 禁用） |
| `DISABLE_INTERLEAVED_THINKING` | 阻止发送交错思考 beta 标头（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_1M_CONTEXT` | 禁用 1M token 上下文窗口（`1` 禁用） |
| `CLAUDE_CODE_ACCOUNT_UUID` | 覆盖用于认证的账户 UUID |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | 禁用与 git 相关的系统提示指令 |
| `CLAUDE_CODE_NEW_INIT` | 设置为 `true` 以使 `/init` 运行交互式设置流程。在探索代码库之前询问要生成哪些文件（CLAUDE.md、skills、hooks）。不设置此项时，`/init` 自动生成 CLAUDE.md |
| `CLAUDE_CODE_PLUGIN_SEED_DIR` | 一个或多个只读插件种子目录的路径，在 Unix 上以 `:` 分隔，在 Windows 上以 `;` 分隔。将预填充的插件捆绑到容器镜像中。Claude Code 在启动时从这些目录注册市场，并使用预缓存的插件而无需重新克隆 |
| `ENABLE_CLAUDEAI_MCP_SERVERS` | 启用 Claude.ai MCP 服务器 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 设置 effort level：`low`、`medium`、`high`、`xhigh`（仅 Opus 4.7，v2.1.111）、`max`（仅 Opus 4.6）或 `auto`（使用模型默认）。优先于 `/effort` 和 `effortLevel` 设置。也可配置为仅启动时变量 — 参见 [CLI 启动标志](./claude-cli-startup-flags.md#environment-variables) |
| `CLAUDE_EFFORT` | 只读。注入到 Bash 工具子进程和 hook 处理程序中，带有活跃的 effort level，以便 shell 脚本和 hooks 可以适应当前层级（与 `CLAUDE_CODE_EFFORT_LEVEL` 配套；v2.1.133）。在 skill 文件中使用 `${CLAUDE_EFFORT}` *(位于更新日志中，不在官方环境变量页面上 — 只读，用户不可配置)* |
| `CLAUDE_CODE_MAX_TURNS` | 停止前的最大 agentic 轮次 *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 等同于同时设置 `DISABLE_AUTOUPDATER`、`DISABLE_FEEDBACK_COMMAND`、`DISABLE_ERROR_REPORTING` 和 `DISABLE_TELEMETRY` |
| `CLAUDE_CODE_SKIP_SETTINGS_SETUP` | 跳过首次运行的设置流程 *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_PROMPT_CACHING_ENABLED` | 覆盖提示缓存行为 *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_DISABLE_TOOLS` | 逗号分隔的禁用工具列表 *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_DISABLE_MCP` | 禁用所有 MCP 服务器（`1` 禁用） *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | 每个响应的最大输出 token。默认值：32000（自 v2.1.77 起 Opus 4.6 为 64000）。上限：64000（自 v2.1.77 起 Opus 4.6 和 Sonnet 4.6 为 128000） |
| `CLAUDE_CODE_DISABLE_FAST_MODE` | 完全禁用快速模式（`1` 禁用） |
| `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` | 设置为 `1` 以将[快速模式](https://code.claude.com/docs/en/fast-mode)固定到 Claude Opus 4.6 而不是默认的 Opus 4.7。设置后，`/fast` 在 Opus 4.6 上运行；不设置时，`/fast` 在 Opus 4.7 上运行（v2.1.142） |
| `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` | 设置为 `1` 以在流式请求中途失败时禁用非流式回退。流式错误传播到重试层。当代理或网关导致回退产生重复工具执行时很有用（v2.1.83） |
| `CLAUDE_ENABLE_STREAM_WATCHDOG` | 中止停滞的流（`1` 启用） |
| `CLAUDE_CODE_ENABLE_FINE_GRAINED_TOOL_STREAMING` | 启用细粒度工具流式传输（`1` 启用） |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | 禁用自动记忆（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING` | 禁用 `/rewind` 的文件检查点（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_ATTACHMENTS` | 禁用附件处理（`1` 禁用） |
| `CLAUDE_CODE_DISABLE_CLAUDE_MDS` | 阻止加载 CLAUDE.md 文件（`1` 禁用） |
| `CLAUDE_CODE_RESUME_INTERRUPTED_TURN` | 如果上一个会话在轮次中间结束，自动恢复（`1` 启用） |
| `CLAUDE_CODE_SKIP_PROMPT_HISTORY` | 设置为 `1` 以跳过将提示历史和会话记录写入磁盘。使用此变量启动的会话不会出现在 `--resume`、`--continue` 或上箭头历史中。适用于临时的脚本化会话 |
| `CLAUDE_CODE_USER_EMAIL` | 同步提供用户电子邮件用于认证 |
| `CLAUDE_CODE_ORGANIZATION_UUID` | 同步提供组织 UUID 用于认证 |
| `CLAUDE_CONFIG_DIR` | 自定义配置目录（覆盖默认的 `~/.claude`） |
| `CLAUDE_CODE_TMPDIR` | 覆盖用于内部临时文件的临时目录。Claude Code 会在此路径后附加 `/claude/`。默认值：Unix/macOS 上为 `/tmp`，Windows 上为 `os.tmpdir()` |
| `ANTHROPIC_CUSTOM_HEADERS` | API 请求的自定义标头（`Name: Value` 格式，多个标头用换行符分隔） |
| `ANTHROPIC_FOUNDRY_API_KEY` | Microsoft Foundry 认证的 API 密钥 |
| `ANTHROPIC_FOUNDRY_BASE_URL` | Foundry 资源的基础 URL |
| `ANTHROPIC_FOUNDRY_RESOURCE` | Foundry 资源名称 |
| `AWS_BEARER_TOKEN_BEDROCK` | Bedrock 认证的 API 密钥 |
| `ANTHROPIC_SMALL_FAST_MODEL` | **已弃用** — 请改用 `ANTHROPIC_DEFAULT_HAIKU_MODEL` |
| `ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION` | 已弃用的 Haiku 类模型覆盖的 AWS 区域 |
| `CLAUDE_CODE_SHELL_PREFIX` | 在 bash 命令前添加的命令前缀 |
| `BASH_DEFAULT_TIMEOUT_MS` | 默认 bash 命令超时时间（毫秒） |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH` | 跳过 Bedrock 的 AWS 认证（`1` 跳过） |
| `CLAUDE_CODE_SKIP_FOUNDRY_AUTH` | 跳过 Foundry 的 Azure 认证（`1` 跳过） |
| `CLAUDE_CODE_SKIP_MANTLE_AUTH` | 跳过 Bedrock Mantle 的 AWS 认证（例如使用 LLM 网关时） |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH` | 跳过 Vertex 的 Google 认证（`1` 跳过） |
| `CLAUDE_CODE_PROXY_RESOLVES_HOSTS` | 允许代理执行 DNS 解析 |
| `CLAUDE_CODE_API_KEY_HELPER_TTL_MS` | `apiKeyHelper` 的凭证刷新间隔（毫秒） |
| `CLAUDE_CODE_CLIENT_CERT` | mTLS 的客户端证书路径 |
| `CLAUDE_CODE_CLIENT_KEY` | mTLS 的客户端私钥路径 |
| `CLAUDE_CODE_CLIENT_KEY_PASSPHRASE` | 加密的 mTLS 密钥的密码短语 |
| `CLAUDE_CODE_CERT_STORE` | TLS 连接的 CA 证书来源的逗号分隔列表：`bundled`（随 Claude Code 附带的 Mozilla CA 集）和/或 `system`（操作系统信任存储）。默认值：`bundled,system`。系统存储集成需要原生二进制分发；在 Node.js 运行时上，无论此值如何，仅使用捆绑集（v2.1.101） |
| `CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS` | 插件市场 git 克隆超时时间（毫秒）（默认：120000） |
| `CLAUDE_CODE_PLUGIN_PREFER_HTTPS` | 设置为 `1` 以通过 HTTPS 而非 SSH 克隆 GitHub `owner/repo` 简写插件来源。适用于插件安装/更新和 `/plugin marketplace add`/`update`。在没有为 `github.com` 配置 SSH 密钥的 CI 运行器或容器中有用（v2.1.141） |
| `CLAUDE_CODE_PLUGIN_CACHE_DIR` | 覆盖插件根目录 |
| `CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL` | 跳过自动添加官方市场（`1` 禁用） |
| `CLAUDE_CODE_SYNC_PLUGIN_INSTALL` | 等待插件安装完成后再执行首次查询（`1` 启用） |
| `CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS` | 同步插件安装的超时时间（毫秒） |
| `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` | 设置为 `1` 以在 `git pull` 失败时保留现有的市场缓存，而不是擦除并重新克隆。在离线或气隙环境中有用，因为重新克隆会以相同方式失败 |
| `CLAUDE_CODE_HIDE_ACCOUNT_INFO` | 从 UI 隐藏电子邮件/组织信息 *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_DISABLE_CRON` | 禁用计划/cron 任务（`1` 禁用） |
| `DISABLE_INSTALLATION_CHECKS` | 禁用安装警告 |
| `DISABLE_FEEDBACK_COMMAND` | 禁用 `/feedback` 命令。旧名称 `DISABLE_BUG_COMMAND` 也被接受 |
| `DISABLE_DOCTOR_COMMAND` | 隐藏 `/doctor` 命令（`1` 禁用） |
| `DISABLE_LOGIN_COMMAND` | 隐藏 `/login` 命令（`1` 禁用） |
| `DISABLE_LOGOUT_COMMAND` | 隐藏 `/logout` 命令（`1` 禁用） |
| `DISABLE_UPGRADE_COMMAND` | 隐藏 `/upgrade` 命令（`1` 禁用） |
| `DISABLE_EXTRA_USAGE_COMMAND` | 隐藏 `/extra-usage` 命令 — 在 v2.1.144 中重命名为 `/usage-credits`，尽管此环境变量名称未更改（`1` 禁用） |
| `DISABLE_INSTALL_GITHUB_APP_COMMAND` | 隐藏 `/install-github-app` 命令（`1` 禁用） |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS` | 禁用风味文本和非必要模型调用 *(不在官方文档中 — 未验证)* |
| `CLAUDE_CODE_DEBUG_LOGS_DIR` | 覆盖调试日志文件目录路径 |
| `CLAUDE_CODE_DEBUG_LOG_LEVEL` | 最小调试日志级别 |
| `CLAUDE_AUTO_BACKGROUND_TASKS` | 强制长任务自动后台运行（`1` 启用） |
| `CLAUDE_CODE_DISABLE_LEGACY_MODEL_REMAP` | 阻止将 Opus 4.0/4.1 重新映射到较新模型（`1` 禁用） |
| `FALLBACK_FOR_ALL_PRIMARY_MODELS` | 触发所有主要模型的回退模型，而不仅仅是默认模型（`1` 启用） |
| `CCR_FORCE_BUNDLE` | 设置为 `1` 以强制 `claude --remote` 捆绑并上传本地仓库，即使 GitHub 访问可用。也可配置为仅启动时变量 — 参见 [CLI 启动标志](./claude-cli-startup-flags.md#environment-variables) |
| `CLAUDE_CODE_GIT_BASH_PATH` | 仅 Windows：Git Bash 可执行文件（`bash.exe`）的路径。当 Git Bash 已安装但不在 PATH 中时使用 |
| `DISABLE_COST_WARNINGS` | 禁用费用警告消息 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 覆盖 subagent 的模型（例如 `haiku`、`sonnet`） |
| `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` | 设置为 `1` 以从子进程环境（Bash 工具、hooks、MCP stdio 服务器）中剥离 Anthropic 和云提供商凭证。用于深度防御，当子进程不应继承 API 密钥时（v2.1.83） |
| `CLAUDE_CODE_SCRIPT_CAPS` | JSON 对象，限制在设置了 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` 时，特定脚本每个会话可被调用的次数。键是与命令文本匹配的子字符串；值是整数调用限制。例如 `{"deploy.sh": 2}` 允许 `deploy.sh` 最多调用两次。匹配基于子字符串；通过 `xargs` 或 `find -exec` 的运行时扇出不会被检测到 — 这是深度防御控制 |
| `CLAUDE_CODE_PERFORCE_MODE` | 设置为 `1` 以启用 Perforce 感知的写入保护。设置后，如果目标文件缺少所有者写入位（Perforce 在同步文件上清除该位，直到 `p4 edit` 打开它们），Edit、Write 和 NotebookEdit 会失败并显示 `p4 edit <file>` 提示。防止 Claude Code 绕过 Perforce 变更跟踪（v2.1.98） |
| `CLAUDE_CODE_MAX_RETRIES` | 覆盖 API 请求重试次数（默认：10） |
| `CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY` | 最大并行只读工具数（默认：10） |
| `CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS` | 在 SDK 模式下禁用内置 subagent 类型（`1` 禁用） |
| `CLAUDE_AGENT_SDK_MCP_NO_PREFIX` | 在 SDK 模式下跳过 MCP 工具的 `mcp__<server>__` 前缀（`1` 启用） |
| `MCP_CONNECTION_NONBLOCKING` | 在 `-p` 模式下设置为 `true` 以完全跳过 MCP 连接等待。将 `--mcp-config` 服务器连接限制在 5 秒，而不是阻塞在最慢的服务器上 *(位于 v2.1.89 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hook 超时时间（毫秒）（替代硬编码的 1.5 秒限制） |
| `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY` | 禁用反馈调查提示（`1` 禁用） |
| `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` | 设置为 `1` 以将会话质量调查路由到自己的 OpenTelemetry 收集器，当 Anthropic 定向的非必要流量被阻止时。调查评分仅作为 OTEL 事件发送到配置的收集器 — 没有调查数据发送给 Anthropic。当设置了 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`、`DISABLE_TELEMETRY` 或 `DO_NOT_TRACK` 时生效；否则无效。`CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY` 和组织产品反馈策略优先级更高（v2.1.136） |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | 禁用终端标题更新（`1` 禁用） |
| `CLAUDE_CODE_TMUX_TRUECOLOR` | 设置为 `1` 以允许在 tmux 内输出 24 位真彩色。默认情况下，Claude Code 在设置了 `$TMUX` 时将颜色限制为 256 色，因为 tmux 不会传递真彩色转义序列，除非配置为这样做。在向 `~/.tmux.conf` 添加 `set -ga terminal-overrides ',*:Tc'` 后设置此项 |
| `CLAUDE_CODE_NO_FLICKER` | 设置为 `1` 以启用无闪烁的替代屏幕渲染。消除全屏重绘期间的视觉闪烁（v2.1.88） |
| `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` | 设置为 `1` 以禁用全屏渲染并使用经典主屏幕渲染器。对话保留在终端的原生回滚中，因此 `Cmd+f` 和 tmux 复制模式正常工作。优先于 `CLAUDE_CODE_NO_FLICKER` 和 `tui` 设置。你也可以使用 `/tui default` 切换（v2.1.132） |
| `CLAUDE_CODE_FORCE_SYNC_OUTPUT` | 设置为 `1` 以在终端支持但未自动检测到时强制启用 DEC 私有模式 2026 同步输出。适用于如 Emacs `eat` 等实现 BSU/ESU 但不响应能力探测的模拟器。在 tmux 下无效（v2.1.129） |
| `CLAUDE_CODE_SCROLL_SPEED` | 全屏渲染的鼠标滚轮滚动倍率。增大可加快滚动速度，减小可获得更精细的控制 |
| `CLAUDE_CODE_DISABLE_VIRTUAL_SCROLL` | 设置为 `1` 以禁用全屏渲染中的虚拟滚动，并渲染记录中的每条消息。如果全屏模式下的滚动显示消息应出现但为空白区域，则使用此项 |
| `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` | 设置为 `1` 以完全退出替代屏幕（全屏）渲染器并使用经典的回滚渲染器。当终端多路复用器、录制工具或无障碍工具无法干净地处理替代屏幕缓冲区时有用（v2.1.132） |
| `CLAUDE_CODE_DISABLE_MOUSE` | 设置为 `1` 以禁用全屏渲染中的鼠标跟踪。当鼠标事件干扰终端多路复用器或无障碍工具时有用 |
| `CLAUDE_CODE_HIDE_CWD` | 设置为 `1` 以在 Claude Code 启动徽标横幅中隐藏当前工作目录。在屏幕录制、演示或共享会话中有用，其中 CWD 路径会泄露主机或项目布局信息（v2.1.119） |
| `CLAUDE_CODE_FORCE_SYNC_OUTPUT` | 设置为 `1` 以强制 Claude Code 向终端的写入进行同步输出刷新。默认为异步/缓冲输出以获得性能。当终端输出与子进程输出交错或乱序时，用作调试辅助手段（v2.1.129） |
| `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` | 控制 Claude Code 基于包管理器的后台自动更新检查。设置为 `0` 以禁用后台检查（Claude Code 不会轮询包管理器以获取更新版本）；设置为 `1`（默认）以保持后台检查启用。独立于 `DISABLE_AUTOUPDATER`，后者控制 npm 注册表自动更新程序（v2.1.129） |
| `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` | 设置为 `1` 以选择从配置的 LLM 网关（例如 Bedrock/Vertex 前面的企业代理）获取可用模型列表。启用后，`/model` 选择器从网关的发现端点填充，而不是从内置别名列表。当你的网关暴露用户应从中选择的精选模型子集时使用（v2.1.129） |
| `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` | 设置为 `1` 以重新启用面向 OpenTelemetry 企业的会话中质量调查。当配置了 `OTEL_*` 环境变量或 `feedbackSurveyRate` 时，调查默认被抑制，以防止调查流量泄漏到企业遥测管道中。当管理员尽管有 OTel 部署但仍希望获得抽样调查数据时使用（v2.1.136） |
| `CLAUDE_CODE_ACCESSIBILITY` | 设置为 `1` 以保持原生终端光标对屏幕阅读器和无障碍工具可见 |
| `CLAUDE_CODE_SYNTAX_HIGHLIGHT` | 设置为 `0` 以禁用 diff 输出中的语法高亮 |
| `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL` | 跳过自动 IDE 扩展安装（`1` 跳过） |
| `CLAUDE_CODE_AUTO_CONNECT_IDE` | 覆盖自动 IDE 连接行为 |
| `CLAUDE_CODE_IDE_HOST_OVERRIDE` | 覆盖 IDE 主机地址以进行连接 |
| `CLAUDE_CODE_IDE_SKIP_VALID_CHECK` | 跳过 IDE 锁文件验证（`1` 跳过） |
| `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | OTel 标头辅助脚本的去抖间隔（毫秒） |
| `CLAUDE_CODE_OTEL_FLUSH_TIMEOUT_MS` | OpenTelemetry 刷新的超时时间（毫秒） |
| `CLAUDE_CODE_OTEL_SHUTDOWN_TIMEOUT_MS` | OpenTelemetry 关闭的超时时间（毫秒） |
| `CLAUDE_ENABLE_BYTE_WATCHDOG` | 设置为 `1` 以强制启用字节级流式空闲看门狗，或 `0` 以强制禁用它。未设置时，对于 Anthropic API 连接，看门狗默认启用。字节看门狗在 `CLAUDE_STREAM_IDLE_TIMEOUT_MS`（最小 5 分钟）持续时间内没有字节到达线路时中止连接，独立于事件级看门狗 |
| `CLAUDE_STREAM_IDLE_TIMEOUT_MS` | 流式空闲看门狗的超时时间（毫秒）。两个看门狗适用：**字节级**（默认和最小值 `300000` / 5 分钟，当没有字节到达线路时中止）和**事件级**（默认 `90000` / 90 秒，无最小值，当没有 SSE 事件到达时中止）。字节看门狗默认对 Anthropic API 连接启用；通过 `CLAUDE_ENABLE_BYTE_WATCHDOG` 控制。如果长时间运行的工具或慢速网络导致过早超时错误，请增加事件超时 |
| `OTEL_LOG_TOOL_DETAILS` | 设置为 `1` 以在 OpenTelemetry 事件中包含 `tool_parameters`。默认出于隐私原因省略 *(位于 v2.1.85 更新日志中，尚未在官方环境变量页面上)* |
| `OTEL_LOG_RAW_API_BODIES` | 设置为 `1` 以将完整的 API 请求和响应体作为 OpenTelemetry 日志事件发出。默认出于隐私和负载大小原因省略。在网关或代理处调试时有用 *(位于 v2.1.111 更新日志中，尚未在官方环境变量页面上)* |
| `OTEL_LOG_USER_PROMPTS` | 设置为 `1` 以在 OpenTelemetry LLM 请求跨度中包含 `user_system_prompt` 字段。默认出于隐私原因省略 — 用户提示可能包含敏感数据，因此仅在你控制 OTel 收集器并有相应策略时选择加入 *(位于 v2.1.121 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_FORK_SUBAGENT` | 设置为 `1` 以在外部构建（非 Anthropic 签名分发）上启用分叉 subagent。分叉 subagent 在隔离的子进程中运行，而不是共享主 agent 的上下文 *(位于 v2.1.117 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_MCP_SERVER_NAME` | MCP 服务器的名称，作为环境变量传递给 `headersHelper` 脚本，以便它们可以生成特定于服务器的认证标头 *(位于 v2.1.85 更新日志中，尚未在官方环境变量页面上)* |
| `CLAUDE_CODE_MCP_SERVER_URL` | MCP 服务器的 URL，与 `CLAUDE_CODE_MCP_SERVER_NAME` 一起作为环境变量传递给 `headersHelper` 脚本 *(位于 v2.1.85 更新日志中，尚未在官方环境变量页面上)* |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 覆盖 Opus 模型别名（例如 `claude-opus-4-6[1m]`） |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时，自定义 `/model` 选择器中 Opus 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中 Opus 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_OPUS_MODEL_SUPPORTED_CAPABILITIES` | 覆盖固定 Opus 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 覆盖 Sonnet 模型别名（例如 `claude-sonnet-4-6`） |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_NAME` | 在 Bedrock/Vertex/Foundry 上使用固定模型时，自定义 `/model` 选择器中 Sonnet 条目标签。默认为模型 ID |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_DESCRIPTION` | 自定义 `/model` 选择器中 Sonnet 条目描述。默认为 `Custom model (<model-id>)` |
| `ANTHROPIC_DEFAULT_SONNET_MODEL_SUPPORTED_CAPABILITIES` | 覆盖固定 Sonnet 模型的能力检测。逗号分隔的值（例如 `effort,thinking`）。当固定模型支持自动检测无法确认的功能时需要 |
| `MAX_THINKING_TOKENS` | 每个响应的最大扩展思考 token 数 |
| `CLAUDE_CODE_AUTO_COMPACT_WINDOW` | 设置用于自动压缩计算的上下文容量（以 token 计）。默认为模型的上下文窗口（标准 200K，扩展上下文模型 1M）。在 1M 模型上使用较低的值（例如 `500000`）可将其视为 500K 用于压缩。上限为实际上下文窗口。`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 作为此值的百分比应用。设置此项可将压缩阈值与状态行的 `used_percentage` 解耦 |
| `DISABLE_AUTO_COMPACT` | 禁用自动上下文压缩（`1` 禁用）。手动 `/compact` 仍然有效 |
| `DISABLE_COMPACT` | 禁用所有压缩 — 包括自动和手动（`1` 禁用） |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | 启用提示建议 |
| `CLAUDE_CODE_PLAN_MODE_REQUIRED` | 要求会话使用计划模式 |
| `CLAUDE_CODE_TEAM_NAME` | agent team 的团队名称 |
| `CLAUDE_CODE_TASK_LIST_ID` | 任务集成的任务列表 ID |
| `CLAUDE_ENV_FILE` | 自定义环境文件路径 |
| `FORCE_AUTOUPDATE_PLUGINS` | 强制插件自动更新（`1` 启用） |
| `HTTP_PROXY` | 网络请求的 HTTP 代理 URL |
| `HTTPS_PROXY` | 网络请求的 HTTPS 代理 URL |
| `NO_PROXY` | 逗号分隔的绕过代理的主机列表 |
| `MCP_TOOL_TIMEOUT` | MCP 工具执行超时时间（毫秒） |
| `MCP_CLIENT_SECRET` | MCP OAuth 客户端密钥 |
| `MCP_OAUTH_CALLBACK_PORT` | MCP OAuth 回调端口 |
| `IS_DEMO` | 启用演示模式 |
| `SLASH_COMMAND_TOOL_CHAR_BUDGET` | 斜杠命令工具输出的字符预算 |
| `VERTEX_REGION_CLAUDE_3_5_HAIKU` | Vertex AI 区域覆盖（Claude 3.5 Haiku） |
| `VERTEX_REGION_CLAUDE_3_7_SONNET` | Vertex AI 区域覆盖（Claude 3.7 Sonnet） |
| `VERTEX_REGION_CLAUDE_4_0_OPUS` | Vertex AI 区域覆盖（Claude 4.0 Opus） |
| `VERTEX_REGION_CLAUDE_4_0_SONNET` | Vertex AI 区域覆盖（Claude 4.0 Sonnet） |
| `VERTEX_REGION_CLAUDE_4_1_OPUS` | Vertex AI 区域覆盖（Claude 4.1 Opus） |

---

## 实用命令

| 命令 | 描述 |
|---------|-------------|
| `/model` | 切换模型并调整 Opus 4.6 effort level |
| `/effort` | 直接设置 effort level：`low`、`medium`、`high`、`xhigh`（仅 Opus 4.7，v2.1.111）或 `max`（仅 Opus 4.6）（v2.1.76+） |
| `/config` | 交互式配置 UI |
| `/memory` | 查看/编辑所有记忆文件 |
| `/agents` | 管理 subagent |
| `/mcp` | 管理 MCP 服务器 |
| `/hooks` | 查看已配置的 hooks |
| `/plugin` | 管理插件 |
| `claude plugin tag` | 在市场中对插件版本打标签以供分发。在市场仓库中运行，提供插件名称和版本（v2.1.118） |
| `claude plugin prune` | 移除其市场来源不再存在的插件（例如市场已删除或 `extraKnownMarketplaces` 条目已移除）。清理本地缓存并禁用孤立插件（v2.1.121） |
| `claude plugin details <plugin>` | 显示插件的组件清单（命令、agent、skill、hook）以及它增加的每个会话上下文 token 成本。在托管环境中启用插件前，用于审计 token 花费（v2.1.139） |
| `/keybindings` | 配置自定义键盘快捷键 |
| `/skills` | 查看和管理 skill |
| `/permissions` | 查看和管理权限规则 |
| `/usage-credits` | 查看剩余使用额度及限制。在 v2.1.144 中从 `/extra-usage` 重命名（旧名称仍有效） |
| `--doctor` | 诊断配置问题 |
| `--debug` | 调试模式，显示 hook 执行详情 |

---

## 快速参考：完整示例

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "model": "sonnet",
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
  "effortLevel": "xhigh",

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
    "ANTHROPIC_BEDROCK_SERVICE_TIER": "priority"
  }
}
```

---

## 来源

- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
- [Claude Code 设置 JSON Schema](https://json.schemastore.org/claude-code-settings.json)
- [Claude Code 更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code GitHub 设置示例](https://github.com/feiskyer/claude-code-settings)
- [Shipyard - Claude Code CLI 速查表](https://shipyard.build/blog/claude-code-cheat-sheet/)
- [Claude Code 环境变量参考](https://code.claude.com/docs/en/env-vars)
- [Claude Code 权限参考](https://code.claude.com/docs/en/permissions)
