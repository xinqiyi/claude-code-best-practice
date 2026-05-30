# HOOKS-README

包含 hooks 相关的所有详细信息、脚本和使用说明。

## Hook 事件概览 - [官方 27 个 Hook](https://code.claude.com/docs/en/hooks)

Claude Code 提供了多个 hook 事件，在工作流的不同阶段触发：

| # | Hook | 描述 | 选项 |
|:-:|------|------|---------|
| 1 | `PreToolUse` | 在工具调用之前运行（可阻止调用） | `async`, `timeout: 5000`, `tool_use_id` |
| 2 | `PermissionRequest` | 当 Claude Code 向用户请求权限时运行 | `async`, `timeout: 5000`, `permission_suggestions` |
| 3 | `PostToolUse` | 在工具调用成功完成后运行 | `async`, `timeout: 5000`, `tool_response`, `tool_use_id` |
| 4 | `PostToolUseFailure` | 在工具调用失败后运行 | `async`, `timeout: 5000`, `error`, `is_interrupt`, `tool_use_id` |
| 5 | `UserPromptSubmit` | 当用户提交 prompt、Claude 处理之前运行 | `async`, `timeout: 5000`, `prompt` |
| 6 | `Notification` | 当 Claude Code 发送通知时运行 | `async`, `timeout: 5000`, `notification_type`, `message`, `title` |
| 7 | `Stop` | 当 Claude Code 完成响应时运行 | `async`, `timeout: 5000`, `stop_reason`, `last_assistant_message`, `stop_hook_active` |
| 8 | `SubagentStart` | 当 subagent 任务开始时运行 | `async`, `timeout: 5000`, `agent_id`, `agent_type` |
| 9 | `SubagentStop` | 当 subagent 任务完成时运行 | `async`, `timeout: 5000`, `agent_id`, `agent_type`, `last_assistant_message`, `agent_transcript_path`, `stop_hook_active` |
| 10 | `PreCompact` | 在 Claude Code 即将执行 compact 操作前运行 | `async`, `timeout: 5000`, `once`, `compact_trigger` |
| 11 | `PostCompact` | 在 Claude Code 完成 compact 操作后运行 | `async`, `timeout: 5000`, `compact_trigger` |
| 12 | `SessionStart` | 当 Claude Code 启动新会话或恢复已有会话时运行 | `async`, `timeout: 5000`, `once`, `agent_type`, `model`, `source` |
| 13 | `SessionEnd` | 当 Claude Code 会话结束时运行 | `async`, `timeout: 5000`, `once`, `reason` |
| 14 | `Setup` | 当 Claude Code 运行 /setup 命令进行项目初始化时运行 | `async`, `timeout: 30000` |
| 15 | `TeammateIdle` | 当 teammate agent 进入空闲状态时运行（实验性 agent teams） | `async`, `timeout: 5000`, `teammate_name`, `team_name` |
| 16 | `TaskCreated` | 当通过 TaskCreate 工具创建任务时运行（实验性 agent teams） | `async`, `timeout: 5000`, `task_id`, `task_subject`, `task_description`, `teammate_name`, `team_name` |
| 17 | `TaskCompleted` | 当后台任务完成时运行（实验性 agent teams） | `async`, `timeout: 5000`, `task_id`, `task_subject`, `task_description`, `teammate_name`, `team_name` |
| 18 | `ConfigChange` | 当会话期间配置文件发生变更时运行 | `async`, `timeout: 5000`, `file_path`, `config_source` |
| 19 | `WorktreeCreate` | 当 agent worktree 隔离机制创建 worktree 用于自定义 VCS 设置时运行 | `async`, `timeout: 5000`, `worktree_path`, `isolation_reason` |
| 20 | `WorktreeRemove` | 当 agent worktree 隔离机制移除 worktree 用于自定义 VCS 清理时运行 | `async`, `timeout: 5000`, `worktree_path`, `removal_reason` |
| 21 | `InstructionsLoaded` | 当 CLAUDE.md 或 `.claude/rules/*.md` 文件被加载到上下文中时运行 | `async`, `timeout: 5000`, `file_path`, `memory_type`, `load_reason`, `globs`, `trigger_file_path`, `parent_file_path` |
| 22 | `Elicitation` | 当 MCP 服务器在工具调用期间请求用户输入时运行 | `async`, `timeout: 5000`, `server_name`, `tool_name`, `elicitation_schema` |
| 23 | `ElicitationResult` | 在用户响应 MCP elicitation 后、响应返回给服务器之前运行 | `async`, `timeout: 5000`, `server_name`, `tool_name`, `user_response` |
| 24 | `StopFailure` | 当因 API 错误（速率限制、认证失败等）导致回合结束时运行 | `async`, `timeout: 5000`, `error_type`, `error_message`, `last_assistant_message` |
| 25 | `CwdChanged` | 当会话期间工作目录发生变化时运行（响应式环境管理，例如 direnv） | `async`, `timeout: 5000`, `old_cwd`, `new_cwd` |
| 26 | `FileChanged` | 当会话期间被监视的文件发生变更时运行（响应式环境管理，例如 direnv）。**需要 `matcher` 指定要监视的文件，使用管道符分隔 basename**（例如 `.envrc\|.env`） | `async`, `timeout: 5000`, `file_path`, `changed_reason` |
| 27 | `PermissionDenied` | 在 auto mode 分类器拒绝工具调用后运行。返回 `{retry: true}` 告知模型可重试 | `async`, `timeout: 5000`, `tool_name`, `tool_input`, `tool_use_id`, `reason` |

> **注意：** Hook 15-17（`TeammateIdle`、`TaskCreated` 和 `TaskCompleted`）需要实验性 agent teams 功能。启动 Claude Code 时设置 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 以启用。

### 官方文档中未列出

以下条目存在于 [Claude Code Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) 中，但**未列入** [官方 Hooks 参考文档](https://code.claude.com/docs/en/hooks)：

| 条目 | 添加版本 | Changelog 参考 | 备注 |
|------|----------|-------------------|-------|
| `Setup` hook | [v2.1.10](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#2110) | "Added new Setup hook event that can be triggered via `--init`, `--init-only`, or `--maintenance` CLI flags for repository setup and maintenance operations" | 官方 hooks 参考页面未列出（列出 26 个 hook，Setup 被排除） |
| Agent frontmatter hooks | [v2.1.0](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) | "Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent's lifecycle" | Changelog 仅提及 3 个 hook，但测试确认 agent 会话中实际触发 **6 个 hook**：PreToolUse、PostToolUse、PermissionRequest、PostToolUseFailure、Stop、SubagentStop。并非全部 27 个 hook 都受支持。 |

## 前置条件

使用 hooks 之前，请确保系统已安装 **Python 3**。

### 所需软件

#### 所有平台（Windows、macOS、Linux）
- **Python 3**：运行 hook 脚本必需
- 验证安装：`python3 --version`

**安装说明：**
- **Windows**：从 [python.org](https://www.python.org/downloads/) 下载，或通过 `winget install Python.Python.3` 安装
- **macOS**：通过 `brew install python3` 安装（需要 [Homebrew](https://brew.sh/)）
- **Linux**：通过 `sudo apt install python3`（Ubuntu/Debian）或 `sudo yum install python3`（RHEL/CentOS）安装

### 音频播放器（可选 - 自动检测）

Hook 脚本会自动检测并使用适合您平台的音频播放器：

- **macOS**：使用 `afplay`（内置，无需安装）
- **Linux**：使用 `paplay`（来自 `pulseaudio-utils`）- 通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**：使用内置 `winsound` 模块（Python 自带）

### Hook 执行方式

Hooks 配置在 `.claude/settings.json` 中，直接使用 Python 3 运行：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py"
}
```

## 配置 Hooks（启用/禁用）

Hooks 可以方便地在全局和单个级别启用或禁用。

### 一次性禁用所有 Hooks

编辑 `.claude/settings.local.json` 并设置：
```json
{
  "disableAllHooks": true
}
```

**注意：** `.claude/settings.local.json` 文件被 git 忽略，因此每个用户可以配置自己的 hook 偏好，而不会影响团队在 `.claude/settings.json` 中的共享设置。

> **托管设置：** 如果管理员通过托管策略配置了 hooks，则在用户、项目或本地设置中设置的 `disableAllHooks` 无法禁用这些托管的 hooks（v2.1.49 修复）。

### 禁用单个 Hooks

如需精细控制，可以通过编辑 hook 配置文件来禁用特定 hooks。

#### 配置文件

有两个用于管理单个 hooks 的配置文件：

1. **`.claude/hooks/config/hooks-config.json`** - 提交到 git 的共享/默认配置
2. **`.claude/hooks/config/hooks-config.local.json`** - 个人覆盖配置（被 git 忽略）

本地配置文件（`.local.json`）优先于共享配置，允许每个开发者自定义自己的 hook 行为，而不影响团队。

#### 共享配置

编辑 `.claude/hooks/config/hooks-config.json` 设置团队范围的默认值：

```json
{
  "disableLogging": false,
  "disablePreToolUseHook": false,
  "disablePermissionRequestHook": false,
  "disablePostToolUseHook": false,
  "disablePostToolUseFailureHook": false,
  "disableUserPromptSubmitHook": false,
  "disableNotificationHook": false,
  "disableStopHook": false,
  "disableSubagentStartHook": false,
  "disableSubagentStopHook": false,
  "disablePreCompactHook": false,
  "disablePostCompactHook": false,
  "disableElicitationHook": false,
  "disableElicitationResultHook": false,
  "disableStopFailureHook": false,
  "disableSessionStartHook": false,
  "disableSessionEndHook": false,
  "disableSetupHook": false,
  "disableTeammateIdleHook": false,
  "disableTaskCompletedHook": false,
  "disableConfigChangeHook": false,
  "disableWorktreeCreateHook": false,
  "disableWorktreeRemoveHook": false,
  "disableInstructionsLoadedHook": false,
  "disableCwdChangedHook": false,
  "disableFileChangedHook": false,
  "disablePermissionDeniedHook": false
}
```

**配置选项：**
- `disableLogging`：设置为 `true` 可禁用将 hook 事件记录到 `.claude/hooks/logs/hooks-log.jsonl`（有助于防止日志文件增长）

#### 本地配置（个人覆盖）

创建或编辑 `.claude/hooks/config/hooks-config.local.json` 设置个人偏好：

```json
{
  "disableLogging": true,
  "disablePostToolUseHook": true,
  "disableSessionStartHook": true
}
```

在此示例中，日志记录被禁用，PostToolUse 和 SessionStart hooks 被本地覆盖。所有其他 hooks 将使用共享配置的值。

**注意：** 单个 hook 的开关由 hook 脚本（`.claude/hooks/scripts/hooks.py`）检查。本地设置覆盖共享设置，如果某个 hook 被禁用，脚本将静默退出，不播放任何声音或执行 hook 逻辑。

### 文本转语音（TTS）

用于生成声音的网站：https://elevenlabs.io/
使用的声音：Samara X

## Agent Frontmatter Hooks

Claude Code 2.1.0 引入了对 agent frontmatter 文件中定义的 agent 专属 hooks 的支持。这些 hooks 仅在 agent 生命周期内运行，并支持部分 hook 事件。

### 支持的 Agent Hooks

Agent frontmatter hooks 支持 **6 个 hooks**（并非全部 27 个）。Changelog 最初仅提到 3 个，但测试确认 agent 会话中实际触发 6 个 hooks：
- `PreToolUse`：在 agent 使用工具之前运行
- `PostToolUse`：在 agent 完成工具使用后运行
- `PermissionRequest`：当工具需要用户权限时运行
- `PostToolUseFailure`：在工具调用失败后运行
- `Stop`：当 agent 完成时运行
- `SubagentStop`：当 subagent 完成时运行

> **注意：** [v2.1.0 changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) 仅提及 3 个 hook：*"Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent's lifecycle"*。然而，使用 `claude-code-hook-agent` 进行的测试确认 agent 会话中实际触发 6 个 hooks。其余 21 个 hooks（例如 Notification、SessionStart、SessionEnd 等）在 agent 上下文中不会触发。
>
> **更新（2026 年 2 月）：** [官方 hooks 参考](https://code.claude.com/docs/en/hooks) 现在声明 skill/agent frontmatter hooks *"支持所有 hook 事件"*。这可能意味着支持范围已扩大到最初测试的 6 个 hooks 之外。建议重新测试以验证是否有更多 hooks 现在能够在 agent 会话中触发。

### Agent 声音文件夹

Agent 专属声音存储在不同的文件夹中：
- `.claude/hooks/sounds/agent_pretooluse/`
- `.claude/hooks/sounds/agent_posttooluse/`
- `.claude/hooks/sounds/agent_permissionrequest/`
- `.claude/hooks/sounds/agent_posttoolusefailure/`
- `.claude/hooks/sounds/agent_stop/`
- `.claude/hooks/sounds/agent_subagentstop/`

### 创建带 Hooks 的 Agent

1. 在 `.claude/agents/` 中创建 agent 定义文件：

```markdown
---
name: my-agent
description: 描述此 agent 的功能
hooks:
  PreToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PreToolUse
  PostToolUse:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PostToolUse
  PermissionRequest:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PermissionRequest
  PostToolUseFailure:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: PostToolUseFailure
  Stop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: Stop
  SubagentStop:
    - type: command
      command: python3 ${CLAUDE_PROJECT_DIR}/.claude/hooks/scripts/hooks.py --agent=my-agent
      timeout: 5000
      async: true
      statusMessage: SubagentStop
---

你的 agent 使用说明在此处...
```

2. 添加声音文件到 agent 声音文件夹：
   - `agent_pretooluse/agent_pretooluse.wav`
   - `agent_posttooluse/agent_posttooluse.wav`
   - `agent_permissionrequest/agent_permissionrequest.wav`
   - `agent_posttoolusefailure/agent_posttoolusefailure.wav`
   - `agent_stop/agent_stop.wav`
   - `agent_subagentstop/agent_subagentstop.wav`

### 示例：天气获取 Agent

请参阅 `.claude/agents/claude-code-hook-agent.md` 获取一个包含完整 hooks 配置的 agent 示例。

### Hook 选项：`once: true`

`once: true` 选项确保 hook 每个会话仅运行一次：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "once": true
}
```

这对于 `SessionStart`、`SessionEnd` 和 `PreCompact` 等应仅触发一次的 hooks 非常有用。

> **注意：** `once` 选项**仅适用于 skills，不适用于 agents**。它在基于 settings 的 hooks 和 skill frontmatter 中有效，但在 agent frontmatter hooks 中不受支持。

### Hook 选项：`async: true`

Hooks 可以在后台运行而不阻塞 Claude Code 的执行，只需添加 `"async": true`：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
```

**何时使用 async hooks：**
- 日志记录和分析
- 通知和音效
- 任何不应拖慢 Claude Code 的副作用

本项目的所有 hooks 均使用 `async: true`，因为声音通知是副作用，不需要阻塞执行。`timeout` 指定异步 hook 在被终止前可运行的最长时间。

### Hook 选项：`asyncRewake`（v2.1.72 起，未记录在官方文档）

`asyncRewake` 选项将异步执行与在失败时唤醒模型的能力相结合：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "asyncRewake": true
}
```

当 `asyncRewake` 为 `true` 时，hook 在后台运行（隐含 `async`），但如果它以退出码 2（阻塞错误）退出，则会唤醒模型处理该错误。这对于通常非阻塞但需要将关键故障上报的 hooks 很有用。在 settings schema 的 `propertyNames` 中发现 — 尚未出现在官方文档中。

### Hook 选项：`statusMessage`

`statusMessage` 字段设置一个自定义的 spinner 消息，在 hook 运行时显示给用户：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true,
  "statusMessage": "PreToolUse"
}
```

本项目在所有 hooks 上将 `statusMessage` 设置为 hook 事件名称，因此 spinner 会短暂显示正在触发的 hook（例如"PreToolUse"、"SessionStart"、"Stop"）。对于同步 hooks 最为明显；对于异步 hooks，消息在 hook 进入后台运行前会短暂闪现。

### Hook 选项：`if`（v2.1.85 起）

`if` 字段使用权限规则语法为 hooks 添加条件执行。设置后，仅当条件匹配时才会启动 hook 进程 — 减少不必要的进程启动：

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "./validate-git.sh",
        "if": "Bash(git *)"
      }]
    }]
  }
}
```

**关键细节：**
- 使用权限规则语法：`Bash(git *)`、`Edit(*.ts)`、`mcp__.*`
- 仅适用于工具事件 hooks：`PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionRequest`
- 在 handler 级别设置（每个 handler 的粒度），而非 matcher 级别
- 没有 `if` 时，hook 进程在每次 matcher 匹配时都会启动 — 使用 `if` 后，仅当条件也匹配时才启动
- 本项目未使用 `if`，因为所有 hooks 都会触发声音播放，与工具参数无关

## Hook 类型

Claude Code 支持四种 hook handler 类型。本项目使用 `command` hooks 进行所有声音播放。

### `type: "command"`（本项目使用的方式）

运行 shell 命令。通过 stdin 接收 JSON 输入，通过退出码和 stdout 传达结果。

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true
}
```

### `type: "prompt"`

向 Claude 模型发送 prompt 进行单轮评估。模型返回 yes/no 决策，格式为 JSON（`{"ok": true/false, "reason": "..."}`）。适用于需要判断而非确定性规则的决策场景。

```json
{
  "type": "prompt",
  "prompt": "Check if all tasks are complete. $ARGUMENTS",
  "timeout": 30
}
```

**支持的事件：** PreToolUse、PostToolUse、PostToolUseFailure、PermissionRequest、UserPromptSubmit、Stop、SubagentStop、TaskCreated、TaskCompleted。**仅 command 事件（prompt/agent 类型不支持）：** ConfigChange、CwdChanged、Elicitation、ElicitationResult、FileChanged、InstructionsLoaded、Notification、PermissionDenied、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、StopFailure、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove。

### `type: "agent"`

生成一个 subagent，具有多轮工具访问权限（Read、Grep、Glob），用于在返回决策前验证条件。响应格式与 prompt hooks 相同。适用于需要检查实际文件或测试输出的验证场景。

```json
{
  "type": "agent",
  "prompt": "Verify that all unit tests pass. $ARGUMENTS",
  "timeout": 120
}
```

### `type: "http"`（v2.1.63 起）

将 JSON POST 到 URL 并接收 JSON 响应，而不是运行 shell 命令。适用于与外部服务或 webhooks 集成。启用沙箱时，HTTP hooks 通过沙箱网络代理路由。

```json
{
  "type": "http",
  "url": "http://localhost:8080/hooks/pre-tool-use",
  "timeout": 30,
  "headers": {
    "Authorization": "Bearer $MY_TOKEN"
  },
  "allowedEnvVars": ["MY_TOKEN"]
}
```

**不支持以下事件：** ConfigChange、CwdChanged、Elicitation、ElicitationResult、FileChanged、InstructionsLoaded、Notification、PermissionDenied、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、StopFailure、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove（command-only 事件）。Headers 支持使用 `$VAR_NAME` 进行环境变量插值，但仅限于在 `allowedEnvVars` 中明确列出的变量。

## 环境变量

Claude Code 向 hook 脚本提供以下环境变量：

| 变量 | 可用性 | 描述 |
|----------|------|---------|
| `$CLAUDE_PROJECT_DIR` | 所有 hooks | 项目根目录。路径包含空格时请使用引号包裹 |
| `$CLAUDE_ENV_FILE` | SessionStart、CwdChanged、FileChanged | 用于持久化后续 Bash 命令的环境变量的文件路径。使用追加模式（`>>`）以保留来自其他 hooks 的变量。**Windows 修复（v2.1.111）：** `CLAUDE_ENV_FILE` 和 SessionStart hook 环境文件现在在 Windows 上生效（v2.1.111 之前，这在 Windows 上是静默无操作） |
| `${CLAUDE_PLUGIN_ROOT}` | Plugin hooks | 插件的根目录，用于随插件捆绑的脚本 |
| `$CLAUDE_CODE_REMOTE` | 所有 hooks | 在远程 Web 环境中设置为 `"true"`，在本地 CLI 中不设置 |
| `${CLAUDE_SKILL_DIR}` | Skill hooks | Skill 自身的目录，用于随 skill 捆绑的脚本（v2.1.69 起） |
| `${CLAUDE_PLUGIN_DATA}` | Plugin hooks | 插件的持久化数据目录，在插件更新后仍保留（v2.1.78 起） |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hooks | 覆盖 SessionEnd hook 的超时时间（毫秒）。v2.1.74 之前，SessionEnd hooks 在 1.5 秒后被杀死，无论配置的 `timeout` 是多少。现在会遵循 hook 的 `timeout` 值，或者设置此环境变量（v2.1.74 起） |
| `session_id`（通过 stdin JSON） | 所有 hooks | 当前会话 ID，作为 stdin 上 JSON 输入的一部分接收（不是环境变量） |

### 通用输入字段（stdin JSON）

每个 hook 通过 stdin 接收一个包含以下通用字段的 JSON 对象，此外还有上面"选项"列中列出的任何 hook 特有字段：

| 字段 | 类型 | 描述 |
|-------|------|------|
| `hook_event_name` | string | 触发的事件名称（例如 `"PreToolUse"`、`"Stop"`） |
| `session_id` | string | 当前会话标识符 |
| `transcript_path` | string | 对话记录 JSON 文件的路径 |
| `cwd` | string | 当前工作目录 |
| `permission_mode` | string | 当前权限模式：`default`、`plan`、`acceptEdits`、`dontAsk` 或 `bypassPermissions` |
| `agent_id` | string | 唯一的 subagent 标识符。当 hook 在 subagent 上下文中触发时存在（v2.1.69 起） |
| `agent_type` | string | Agent 类型名称（例如 `Bash`、`Explore`、`Plan` 或自定义）。使用 `--agent <name>` 标志时或在 subagent 内部时存在（v2.1.69 起） |

> **注意：** Hook 特有字段（例如 PreToolUse 的 `tool_name`、Stop 的 `last_assistant_message`）在以上 [Hook 事件概览](#hook-事件概览---官方-27-个-hook) 表的"选项"列中列出。

## Hooks 管理命令

Claude Code 提供了用于管理 hooks 的内置命令：

- **`/hooks`** — 交互式 hook 管理 UI。无需编辑 JSON 文件即可查看、添加和删除 hooks。Hooks 按来源标记：`[User]`、`[Project]`、`[Local]`、`[Plugin]`。您也可以在此菜单中切换 `disableAllHooks`。
- **`claude hooks reload`** — 无需重启会话即可重新加载 hooks 配置。在编辑 settings 文件后使用（v2.0.47 起）。

## MCP 工具匹配器

对于 `PreToolUse`、`PostToolUse` 和 `PermissionRequest` hooks，可以使用模式 `mcp__<server>__<tool>` 匹配 MCP（Model Context Protocol）工具：

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "mcp__memory__.*",
      "hooks": [{ "type": "command", "command": "echo 'MCP memory tool used'" }]
    }]
  }
}
```

支持完整正则表达式：`mcp__memory__.*`（来自 memory 服务器的所有工具）、`mcp__.*__write.*`（来自任何服务器的任何写入工具）。

### 每个 Hook 的 Matcher 参考

Matchers 过滤哪些事件触发 hook。并非所有 hooks 都支持 matcher — 不支持 matcher 的 hooks 始终触发。

| Hook | Matcher 字段 | 可能的值 | 示例 |
|------|--------------|------|---------|
| `PreToolUse` | `tool_name` | 任何工具名称：`Bash`、`Read`、`Edit`、`Write`、`Glob`、`Grep`、`Agent`、`WebFetch`、`WebSearch`、`AskUserQuestion`、`ExitPlanMode`、`mcp__*` | `"matcher": "Bash"` |
| `PermissionRequest` | `tool_name` | 与 PreToolUse 相同 | `"matcher": "mcp__memory__.*"` |
| `PostToolUse` | `tool_name` | 与 PreToolUse 相同 | `"matcher": "Write"` |
| `PostToolUseFailure` | `tool_name` | 与 PreToolUse 相同 | `"matcher": "Bash"` |
| `Notification` | `notification_type` | `permission_prompt`、`idle_prompt`、`auth_success`、`elicitation_dialog` | `"matcher": "permission_prompt"` |
| `SubagentStart` | `agent_type` | `Bash`、`Explore`、`Plan` 或自定义 agent 名称 | `"matcher": "Bash"` |
| `SubagentStop` | `agent_type` | `Bash`、`Explore`、`Plan` 或自定义 agent 名称 | `"matcher": "Bash"` |
| `SessionStart` | `source` | `startup`、`resume`、`clear`、`compact` | `"matcher": "startup"` |
| `SessionEnd` | `reason` | `clear`、`resume`、`logout`、`prompt_input_exit`、`bypass_permissions_disabled`、`other` | `"matcher": "logout"` |
| `PreCompact` | `compact_trigger` | `manual`、`auto` | `"matcher": "auto"` |
| `PostCompact` | `compact_trigger` | `manual`、`auto` | `"matcher": "manual"` |
| `Elicitation` | `server_name` | MCP 服务器名称 | `"matcher": "my-mcp-server"` |
| `ElicitationResult` | `server_name` | MCP 服务器名称 | `"matcher": "my-mcp-server"` |
| `ConfigChange` | `config_source` | `user_settings`、`project_settings`、`local_settings`、`policy_settings`、`skills` | `"matcher": "project_settings"` |
| `UserPromptSubmit` | — | 不支持 matcher | 始终触发 |
| `Stop` | — | 不支持 matcher | 始终触发 |
| `TeammateIdle` | — | 不支持 matcher | 始终触发 |
| `TaskCreated` | — | 不支持 matcher | 始终触发 |
| `TaskCompleted` | — | 不支持 matcher | 始终触发 |
| `WorktreeCreate` | — | 不支持 matcher | 始终触发 |
| `WorktreeRemove` | — | 不支持 matcher | 始终触发 |
| `InstructionsLoaded` | `load_reason` | `session_start`、`nested_traversal`、`path_glob_match`、`include`、`compact` | `"matcher": "session_start"` |
| `StopFailure` | `error` | `rate_limit`、`authentication_failed`、`billing_error`、`invalid_request`、`server_error`、`max_output_tokens`、`unknown` | `"matcher": "rate_limit"` |
| `CwdChanged` | — | 不支持 matcher | 始终触发 |
| `FileChanged` | `filename`（basename） | 管道符分隔的 basenames：`.envrc`、`.env`、`.env.local` | `"matcher": ".envrc\|.env"` |
| `Setup` | — | 不支持 matcher | 始终触发 |
| `PermissionDenied` | `tool_name` | 工具名称（与 PreToolUse 相同） | `"matcher": "Bash"` |

## 已知问题与解决方法

### Agent Stop Hook 错误（SubagentStop 与 Stop）

**错误报告：** [GitHub Issue #19220](https://github.com/anthropics/claude-code/issues/19220)

**问题：** 在 agent 的 frontmatter 中定义 `Stop` hook 时，传递给 hook 脚本的 `hook_event_name` 是 `"SubagentStop"` 而非 `"Stop"`。这与官方文档矛盾，并破坏了与其他 agent hooks（`PreToolUse` 和 `PostToolUse`）的一致性，后者会正确传递配置的名称。

| Hook | 定义名称 | 实际接收名称 | 状态 |
|------|------------|-------------|--------|
| PreToolUse | `PreToolUse:` | `"PreToolUse"` | ✅ 正确 |
| PostToolUse | `PostToolUse:` | `"PostToolUse"` | ✅ 正确 |
| Stop | `Stop:` | `"SubagentStop"` | ❌ 不一致 |

**状态：** [官方 hooks 参考](https://code.claude.com/docs/en/hooks#hooks-in-skills-and-agents) 现在将此记录为预期行为：*"对于 subagents，Stop hooks 会自动转换为 SubagentStop，因为这是 subagent 完成时触发的事件。"* 本项目通过在 `hooks.py` 中使用 `AGENT_HOOK_SOUND_MAP` 处理此问题，该映射包含一个单独的 `SubagentStop` 条目，映射到 `agent_subagentstop` 声音文件夹。

### PreToolUse 的 `updatedInput` 用于 AskUserQuestion（v2.1.85 起）

当 `PreToolUse` hook 匹配 `AskUserQuestion` 时，它可以返回 `updatedInput` 来自动回答问题 — 使无头集成能够以编程方式回答用户问题，无需手动输入：

```json
{
  "hookSpecificOutput": {
    "updatedInput": {
      "question": "Do you want to proceed?",
      "answer": "yes"
    }
  }
}
```

这对于 CI/CD 流水线、自动化测试或任何 Claude Code 在没有人类操作终端的环境中运行的场景非常有用。尚未出现在官方文档中 — 来源于 GitHub changelog v2.1.85。

### PreToolUse 的 `additionalContext` 现在在工具失败时保留（v2.1.110）

在 v2.1.110 之前，如果 `PreToolUse` hook 返回的 `additionalContext`，随后工具调用失败，该上下文会被**静默丢弃**。自 v2.1.110 起，来自 `PreToolUse` 的 `additionalContext` 即使在匹配的工具调用失败时也会被保留并呈现给模型 — 因此 hook 提供的上下文无论工具执行结果如何都能一致地到达模型。

这不影响本项目，因为 `hooks.py` 不返回 `additionalContext`。

### PermissionRequest `updatedInput` 会重新检查拒绝规则（v2.1.102、v2.1.110）

当 `PermissionRequest` hook 返回 `hookSpecificOutput.updatedInput` 来重写工具输入时，该重写后的输入现在会重新根据权限拒绝规则进行评估。在 v2.1.102 / v2.1.110（两个相关修复）之前，hook 可以返回绕过已配置拒绝规则的 `updatedInput` — 这是一个安全相关的边缘情况。

这不影响本项目，因为 `hooks.py` 不使用决策控制或 `updatedInput`。

### PreToolUse 决策控制弃用

`PreToolUse` hook 之前使用顶层 `decision` 和 `reason` 字段来阻止工具调用。这些现在已被**弃用**。请改用 `hookSpecificOutput.permissionDecision` 和 `hookSpecificOutput.permissionDecisionReason`：

| 已弃用 | 当前用法 |
|-----------|---------|
| `"decision": "approve"` | `"hookSpecificOutput": { "permissionDecision": "allow" }` |
| `"decision": "block"` | `"hookSpecificOutput": { "permissionDecision": "deny" }` |

这不影响本项目，因为 `hooks.py` 使用异步声音播放，不使用决策控制。

## 决策控制模式

不同的 hooks 使用不同的输出 schema 来阻止或控制执行。本项目不使用决策控制（所有 hooks 都是异步声音播放），但供参考：

| Hook(s) | 控制方法 | 值 |
|---------|---------|------|
| PreToolUse | `hookSpecificOutput.permissionDecision` | `allow`、`deny`、`ask`、`defer`（仅限 headless `-p` 模式，v2.1.89+） |
| PreToolUse | `hookSpecificOutput.autoAllow` | `true` — 自动批准此工具的后续使用（v2.0.76 起） |
| PermissionRequest | `hookSpecificOutput.decision.behavior` | `allow`、`deny` |
| Stop、SubagentStop、ConfigChange | 顶层 `decision` | `block` |
| PreCompact | `decision` + 退出码 2 | `{"decision": "block"}` 或退出码 2 — 阻止压缩操作（v2.1.105 起） |
| TeammateIdle、TaskCreated、TaskCompleted | `continue` + 退出码 2 | `{"continue": false, "stopReason": "..."}` — JSON 决策控制于 v2.1.70 添加。TaskCreated 也使用退出码 2 阻止任务创建（stderr 反馈给模型） |
| UserPromptSubmit | 可修改 `prompt` 字段 | 通过 stdout 返回修改后的 prompt |
| WorktreeCreate | 非零退出 + stdout 路径 | 非零退出表示创建失败；stdout 提供 worktree 路径 |
| Elicitation | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 控制 MCP elicitation 响应 |
| ElicitationResult | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 在发送到服务器之前覆盖用户响应 |
| PermissionDenied | `hookSpecificOutput.retry` | `true` — 表示模型可以重试被拒绝的工具调用（v2.1.89+） |

### 通用 JSON 输出字段

所有 hooks 可以通过 stdout JSON 返回以下字段：

| 字段 | 类型 | 描述 |
|-------|------|------|
| `continue` | bool | 如果为 `false`，完全停止 Claude |
| `stopReason` | string | 当 `continue` 为 `false` 时显示的消息 |
| `suppressOutput` | bool | 在 verbose 模式下隐藏 stdout |
| `systemMessage` | string | 向用户显示的警告消息 |
| `additionalContext` | bool | 添加到 Claude 对话中的上下文 |

## Hook 去重与外部变更

- **Hook 去重：** 在多个 settings 位置定义的相同 hook handler 只会并行运行一次，防止重复执行。
- **外部变更检测：** 当 hooks 在活动会话期间被外部修改时（例如，另一个进程编辑 settings 文件），Claude Code 会发出警告。
