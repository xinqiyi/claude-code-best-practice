# HOOKS-README
包含 hooks 的所有详情、脚本和说明

## Hook Events 概览 - [官方 30 个 Hooks](https://code.claude.com/docs/en/hooks)
Claude Code 提供了多个 hook 事件，在 workflow 的不同时间点运行：

| # | Hook | Description | Options |
|:-:|------|-------------|---------|
| 1 | `PreToolUse` | 在工具调用之前运行（可以阻止它们） | `async`、`timeout: 5000`、`tool_use_id` |
| 2 | `PermissionRequest` | 在 Claude Code 向用户请求权限时运行 | `async`、`timeout: 5000`、`permission_suggestions` |
| 3 | `PostToolUse` | 在工具调用成功完成后运行 | `async`、`timeout: 5000`、`tool_response`、`tool_use_id` |
| 4 | `PostToolUseFailure` | 在工具调用失败后运行 | `async`、`timeout: 5000`、`error`、`is_interrupt`、`tool_use_id` |
| 5 | `UserPromptSubmit` | 在用户提交 prompt 后、Claude 处理之前运行 | `async`、`timeout: 5000`、`prompt` |
| 6 | `Notification` | 在 Claude Code 发送通知时运行 | `async`、`timeout: 5000`、`notification_type`、`message`、`title` |
| 7 | `Stop` | 在 Claude Code 完成响应时运行 | `async`、`timeout: 5000`、`stop_reason`、`last_assistant_message`、`stop_hook_active` |
| 8 | `SubagentStart` | 在 subagent 任务启动时运行 | `async`、`timeout: 5000`、`agent_id`、`agent_type` |
| 9 | `SubagentStop` | 在 subagent 任务完成时运行 | `async`、`timeout: 5000`、`agent_id`、`agent_type`、`last_assistant_message`、`agent_transcript_path`、`stop_hook_active` |
| 10 | `PreCompact` | 在 Claude Code 即将运行 compact 操作之前运行 | `async`、`timeout: 5000`、`once`、`compact_trigger` |
| 11 | `PostCompact` | 在 Claude Code 完成 compact 操作后运行 | `async`、`timeout: 5000`、`compact_trigger` |
| 12 | `SessionStart` | 在 Claude Code 启动新会话或恢复已有会话时运行 | `async`、`timeout: 5000`、`once`、`agent_type`、`model`、`source` |
| 13 | `SessionEnd` | 在 Claude Code 会话结束时运行 | `async`、`timeout: 5000`、`once`、`reason` |
| 14 | `Setup` | 在 Claude Code 运行 /setup 命令进行项目初始化时运行 | `async`、`timeout: 30000` |
| 15 | `TeammateIdle` | 在 teammate agent 变为空闲时运行（实验性 agent teams） | `async`、`timeout: 5000`、`teammate_name`、`team_name` |
| 16 | `TaskCreated` | 在通过 TaskCreate 工具创建任务时运行（实验性 agent teams） | `async`、`timeout: 5000`、`task_id`、`task_subject`、`task_description`、`teammate_name`、`team_name` |
| 17 | `TaskCompleted` | 在后台任务完成时运行（实验性 agent teams） | `async`、`timeout: 5000`、`task_id`、`task_subject`、`task_description`、`teammate_name`、`team_name` |
| 18 | `ConfigChange` | 在会话期间配置文件发生变更时运行 | `async`、`timeout: 5000`、`file_path`、`config_source` |
| 19 | `WorktreeCreate` | 在 agent worktree isolation 为自定义 VCS setup 创建工作树时运行 | `async`、`timeout: 5000`、`worktree_path`、`isolation_reason` |
| 20 | `WorktreeRemove` | 在 agent worktree isolation 为自定义 VCS teardown 移除工作树时运行 | `async`、`timeout: 5000`、`worktree_path`、`removal_reason` |
| 21 | `InstructionsLoaded` | 在 CLAUDE.md 或 `.claude/rules/*.md` 文件被加载到 context 时运行 | `async`、`timeout: 5000`、`file_path`、`memory_type`、`load_reason`、`globs`、`trigger_file_path`、`parent_file_path` |
| 22 | `Elicitation` | 在 MCP server 在工具调用期间请求用户输入时运行 | `async`、`timeout: 5000`、`server_name`、`tool_name`、`elicitation_schema` |
| 23 | `ElicitationResult` | 在用户响应 MCP elicitation 之后、响应发送回 server 之前运行 | `async`、`timeout: 5000`、`server_name`、`tool_name`、`user_response` |
| 24 | `StopFailure` | 在 turn 因 API 错误（rate limit、auth failure 等）而结束时运行 | `async`、`timeout: 5000`、`error_type`、`error_message`、`last_assistant_message` |
| 25 | `CwdChanged` | 在会话期间工作目录发生变更时运行（响应式环境管理，例如 direnv） | `async`、`timeout: 5000`、`old_cwd`、`new_cwd` |
| 26 | `FileChanged` | 在会话期间被监听的文件发生变更时运行（响应式环境管理，例如 direnv）。**需要 `matcher` 带管道分隔的 basenames**（例如 `.envrc\|.env`）来指定要监听的文件 | `async`、`timeout: 5000`、`file_path`、`changed_reason` |
| 27 | `PermissionDenied` | 在 auto mode classifier 拒绝工具调用后运行。返回 `{retry: true}` 告诉模型可以重试 | `async`、`timeout: 5000`、`tool_name`、`tool_input`、`tool_use_id`、`reason` |
| 28 | `UserPromptExpansion` | 在用户输入的 slash command 或 MCP prompt 展开为 prompt 时运行，在到达 Claude 之前（可以阻止）。**支持对 `command_name` 的 `matcher`**（例如 `deploy\|review`） | `async`、`timeout: 5000`、`expansion_type`、`command_name`、`command_args`、`command_source`、`prompt` |
| 29 | `PostToolBatch` | 在一整批并行工具调用完成后、下一次模型调用之前运行（可以阻止）。不支持 matcher | `async`、`timeout: 5000`、`tool_calls`（`tool_name`、`tool_input`、`result`、`succeeded` 的数组） |
| 30 | `MessageDisplay` | 在 assistant 消息显示给用户期间运行。仅显示 — **不能阻止**；可以通过 `displayContent` 重写屏幕文本而不更改 transcript。不支持 matcher | `async`、`timeout: 5000`、`message`、`message_index` |

> **注意：** Hooks 15-17（`TeammateIdle`、`TaskCreated` 和 `TaskCompleted`）需要实验性 agent teams 功能。在启动 Claude Code 时设置 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` 以启用它们。

### 未出现在官方文档中

以下条目存在于 [Claude Code Changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) 中，但**未列出**在[官方 Hooks Reference](https://code.claude.com/docs/en/hooks) 中：

| Item | Added In | Changelog Reference | Notes |
|------|----------|-------------------|-------|
| `Setup` hook | [v2.1.10](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#2110) | "Added new Setup hook event that can be triggered via `--init`, `--init-only`, or `--maintenance` CLI flags for repository setup and maintenance operations" | 未在官方 hooks reference 页面列出（列出了 26 个 hooks，Setup 被排除） |
| Agent frontmatter hooks | [v2.1.0](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) | "Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent's lifecycle" | Changelog 仅提到 3 个 hooks，但测试确认在 agent 会话中实际触发了 **6 个 hooks**：PreToolUse、PostToolUse、PermissionRequest、PostToolUseFailure、Stop、SubagentStop。并非所有 30 个 hooks 都受支持。 |

## 前置条件

在使用 hooks 之前，请确保系统已安装 **Python 3**。

### 所需软件

#### 所有平台（Windows、macOS、Linux）
- **Python 3**：运行 hook 脚本所必需
- 验证安装：`python3 --version`

**安装说明：**
- **Windows**：从 [python.org](https://www.python.org/downloads/) 下载或通过 `winget install Python.Python.3` 安装
- **macOS**：通过 `brew install python3` 安装（需要 [Homebrew](https://brew.sh/)）
- **Linux**：通过 `sudo apt install python3`（Ubuntu/Debian）或 `sudo yum install python3`（RHEL/CentOS）安装

### 音频播放器（可选 - 自动检测）

hook 脚本会自动检测并使用适合平台的音频播放器：

- **macOS**：使用 `afplay`（系统内置，无需安装）
- **Linux**：使用 `paplay`（来自 `pulseaudio-utils`）— 通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**：使用内置的 `winsound` 模块（Python 自带）

### Hooks 如何执行

Hooks 在 `.claude/settings.json` 中配置为直接用 Python 3 运行：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py"
}
```

## 配置 Hooks（启用/禁用）

Hooks 可以在全局和单独层面轻松启用或禁用。

### 一次性禁用所有 Hooks

编辑 `.claude/settings.local.json` 并设置：
```json
{
  "disableAllHooks": true
}
```

**注意：** `.claude/settings.local.json` 文件是 git-ignored 的，因此每个用户可以配置自己的 hook 偏好，而不影响团队在 `.claude/settings.json` 中的共享设置。

> **Managed Settings：** 如果管理员通过 managed policy settings 配置了 hooks，在用户、项目或本地设置中设置的 `disableAllHooks` 无法禁用这些 managed hooks（在 v2.1.49 中修复）。

### 禁用单个 Hooks

如需精细控制，可以通过编辑 hooks 配置文件来禁用特定的 hooks。

#### 配置文件

有两个配置文件用于管理单个 hooks：

1. **`.claude/hooks/config/hooks-config.json`** - 提交到 git 的共享/默认配置
2. **`.claude/hooks/config/hooks-config.local.json`** - 你的个人覆盖（git-ignored）

本地配置文件（`.local.json`）优先级高于共享配置，允许每个开发者自定义 hook 行为而不影响团队。

#### 共享配置

编辑 `.claude/hooks/config/hooks-config.json` 设置团队默认值：

```json
{
  "disableLogging": false,
  "disablePreToolUseHook": false,
  "disablePermissionRequestHook": false,
  "disablePostToolUseHook": false,
  "disablePostToolUseFailureHook": false,
  "disablePostToolBatchHook": false,
  "disableUserPromptSubmitHook": false,
  "disableUserPromptExpansionHook": false,
  "disableNotificationHook": false,
  "disableMessageDisplayHook": false,
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
- `disableLogging`：设置为 `true` 以禁用将 hook 事件记录到 `.claude/hooks/logs/hooks-log.jsonl`（有助于防止日志文件增长）

#### 本地配置（个人覆盖）

创建或编辑 `.claude/hooks/config/hooks-config.local.json` 设置个人偏好：

```json
{
  "disableLogging": true,
  "disablePostToolUseHook": true,
  "disableSessionStartHook": true
}
```

在此示例中，日志记录被禁用，PostToolUse 和 SessionStart hooks 在本地被覆盖。所有其他 hooks 将使用共享配置的值。

**注意：** 单个 hook 的开关由 hook 脚本（`.claude/hooks/scripts/hooks.py`）检查。本地设置覆盖共享设置，如果 hook 被禁用，脚本静默退出，不播放任何声音或执行 hook 逻辑。

### Text to Speech（TTS）
用于生成声音的网站：https://elevenlabs.io/
使用的声音：Samara X

## Agent Frontmatter Hooks

Claude Code 2.1.0 引入了在 agent frontmatter 文件中定义的 agent 专属 hooks。这些 hooks 仅在 agent 的生命周期内运行，并支持一部分 hook 事件。

### 受支持的 Agent Hooks

Agent frontmatter hooks 支持 **6 个 hooks**（并非全部 30 个）。changelog 最初只提到 3 个，但测试确认在 agent 会话中实际触发了 6 个 hooks：
- `PreToolUse`：在 agent 使用工具之前运行
- `PostToolUse`：在 agent 完成工具使用后运行
- `PermissionRequest`：在工具需要用户权限时运行
- `PostToolUseFailure`：在工具调用失败后运行
- `Stop`：在 agent 完成时运行
- `SubagentStop`：在 subagent 完成时运行

> **注意：** [v2.1.0 changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#210) 仅提到 3 个 hooks：*"Added hooks support to agent frontmatter, allowing agents to define PreToolUse, PostToolUse, and Stop hooks scoped to the agent's lifecycle"*。然而，使用 `claude-code-hook-agent` 的测试确认在 agent 会话中实际触发了 6 个 hooks。剩余的 24 个 hooks（例如 Notification、SessionStart、SessionEnd 等）不会在 agent context 中触发。
>
> **更新（2026 年 2 月）：** [官方 hooks reference](https://code.claude.com/docs/en/hooks) 现在声明对于 skill/agent frontmatter hooks，"所有 hook 事件均受支持"。这可能意味着支持范围已超出最初测试的 6 个 hooks。建议重新测试以验证现在是否有更多 hooks 在 agent 会话中触发。

### Agent 声音文件夹

Agent 专属声音存储在单独的文件夹中：
- `.claude/hooks/sounds/agent_pretooluse/`
- `.claude/hooks/sounds/agent_posttooluse/`
- `.claude/hooks/sounds/agent_permissionrequest/`
- `.claude/hooks/sounds/agent_posttoolusefailure/`
- `.claude/hooks/sounds/agent_stop/`
- `.claude/hooks/sounds/agent_subagentstop/`

### 创建带有 Hooks 的 Agent

1. 在 `.claude/agents/` 中创建 agent 定义文件：

```markdown
---
name: my-agent
description: 描述这个 agent 做什么
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

你的 agent 指令在此...
```

2. 将声音文件添加到 agent 声音文件夹：
   - `agent_pretooluse/agent_pretooluse.wav`
   - `agent_posttooluse/agent_posttooluse.wav`
   - `agent_permissionrequest/agent_permissionrequest.wav`
   - `agent_posttoolusefailure/agent_posttoolusefailure.wav`
   - `agent_stop/agent_stop.wav`
   - `agent_subagentstop/agent_subagentstop.wav`

### 示例：Weather Fetcher Agent

参见 `.claude/agents/claude-code-hook-agent.md` 了解配置了 hooks 的 agent 的完整示例。

### Hook 选项：`once: true`

`once: true` 选项确保 hook 每次会话只运行一次：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "once": true
}
```

这对于像 `SessionStart`、`SessionEnd` 和 `PreCompact` 这样只应触发一次的 hooks 很有用。

> **注意：** `once` 选项**仅适用于 skills，不适用于 agents**。它在基于 settings 的 hooks 和 skill frontmatter 中有效，但在 agent frontmatter hooks 中不受支持。

### Hook 选项：`async: true`

Hooks 可以通过添加 `"async": true` 在后台运行，不阻塞 Claude Code 的执行：

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
- 通知和声音效果
- 任何不应拖慢 Claude Code 的副作用

本项目对所有 hooks 使用 `async: true`，因为声音通知是不需要阻塞执行的副作用。`timeout` 指定 async hook 在终止前可以运行多长时间。

### Hook 选项：`asyncRewake`（自 v2.1.72，未记载）

`asyncRewake` 选项结合了 async 执行和在失败时唤醒模型的能力：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "asyncRewake": true
}
```

当 `asyncRewake` 为 `true` 时，hook 在后台运行（隐含 `async`），但如果它以退出码 2（阻塞错误）退出，它会唤醒模型以处理错误。这对于通常非阻塞但需要报告关键失败的 hooks 很有用。在 settings schema `propertyNames` 中发现 — 尚未出现在官方文档中。

### Hook 选项：`statusMessage`

`statusMessage` 字段设置在 hook 运行时显示给用户的自定义 spinner 消息：

```json
{
  "type": "command",
  "command": "python3 .claude/hooks/scripts/hooks.py",
  "timeout": 5000,
  "async": true,
  "statusMessage": "PreToolUse"
}
```

本项目在所有 hooks 上将 `statusMessage` 设置为 hook 事件名称，使 spinner 短暂显示正在触发的 hook（例如 "PreToolUse"、"SessionStart"、"Stop"）。这对同步 hooks 最明显；对于 async hooks，消息会在 hook 在后台运行前短暂闪烁。

### Hook 选项：`if`（自 v2.1.85）

`if` 字段使用权限规则语法为 hooks 添加条件执行。设置后，仅当条件匹配时才启动 hook 进程 — 减少不必要的进程启动：

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
- 设置在 handler 级别（每个 handler 的粒度），而非 matcher 级别
- 没有 `if` 时，hook 进程在每次 matcher 匹配时启动 — 有 `if` 时，仅在条件同时匹配时才启动
- 本项目不使用 `if`，因为所有 hooks 无论工具参数如何都会为声音播放而触发的

## Hook Types

Claude Code 支持四种 hook handler 类型。本项目对所有声音播放使用 `command` hooks。

### `type: "command"`（本项目使用）

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

向 Claude 模型发送 prompt 进行单轮评估。模型以 yes/no 决定作为 JSON（`{"ok": true/false, "reason": "..."}`）返回。适用于需要判断而非确定性规则的决策。

```json
{
  "type": "prompt",
  "prompt": "Check if all tasks are complete. $ARGUMENTS",
  "timeout": 30
}
```

**支持的事件：** PreToolUse、PostToolUse、PostToolUseFailure、PermissionRequest、UserPromptSubmit、Stop、SubagentStop、TaskCreated、TaskCompleted。**仅 command 的事件（不支持 prompt/agent 类型）：** ConfigChange、CwdChanged、Elicitation、ElicitationResult、FileChanged、InstructionsLoaded、MessageDisplay、Notification、PermissionDenied、PostCompact、PostToolBatch、PreCompact、SessionEnd、SessionStart、Setup、StopFailure、SubagentStart、TeammateIdle、UserPromptExpansion、WorktreeCreate、WorktreeRemove。

### `type: "agent"`

启动一个 subagent，具有多轮工具访问权限（Read、Grep、Glob），以在返回决定前验证条件。响应格式与 prompt hooks 相同。适用于验证需要检查实际文件或测试输出的情况。

```json
{
  "type": "agent",
  "prompt": "Verify that all unit tests pass. $ARGUMENTS",
  "timeout": 120
}
```

### `type: "http"`（自 v2.1.63）

向 URL POST JSON 并接收 JSON 响应，而非运行 shell 命令。适用于与外部服务或 webhooks 集成。启用 sandbox 时，HTTP hooks 通过 sandbox network proxy 路由。

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

**不支持以下事件：** ConfigChange、CwdChanged、Elicitation、ElicitationResult、FileChanged、InstructionsLoaded、Notification、PermissionDenied、PostCompact、PreCompact、SessionEnd、SessionStart、Setup、StopFailure、SubagentStart、TeammateIdle、WorktreeCreate、WorktreeRemove（仅 command 的事件）。Headers 支持使用 `$VAR_NAME` 的环境变量插值，但仅适用于在 `allowedEnvVars` 中明确列出的变量。

## 环境变量

Claude Code 向 hook 脚本提供以下环境变量：

| Variable | Availability | Description |
|----------|-------------|-------------|
| `$CLAUDE_PROJECT_DIR` | 所有 hooks | 项目根目录。对于包含空格的路径使用引号包裹 |
| `$CLAUDE_ENV_FILE` | SessionStart、CwdChanged、FileChanged | 用于持久化后续 Bash 命令的环境变量的文件路径。使用追加（`>>`）以保留来自其他 hooks 的变量。**Windows 修复（v2.1.111）：** `CLAUDE_ENV_FILE` 和 SessionStart hook 环境文件现在在 Windows 上生效（v2.1.111 之前，这在 Windows 上是静默空操作） |
| `${CLAUDE_PLUGIN_ROOT}` | Plugin hooks | Plugin 的根目录，用于与 plugin 捆绑的脚本 |
| `$CLAUDE_CODE_REMOTE` | 所有 hooks | 在远程 web 环境中设置为 `"true"`，在本地 CLI 中不设置 |
| `${CLAUDE_SKILL_DIR}` | Skill hooks | Skill 自己的目录，用于与 skill 捆绑的脚本（自 v2.1.69） |
| `${CLAUDE_PLUGIN_DATA}` | Plugin hooks | Plugin 的持久化数据目录，在 plugin 更新后仍然存在（自 v2.1.78） |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hooks | 覆盖 SessionEnd hook 的超时时间（毫秒）。v2.1.74 之前，无论配置的 `timeout` 如何，SessionEnd hooks 在 1.5 秒后被杀死。现在遵守 hook 的 `timeout` 值或此环境变量（如果设置）（自 v2.1.74） |
| `session_id`（通过 stdin JSON） | 所有 hooks | 当前会话 ID，作为 stdin 上 JSON 输入的一部分接收（不是环境变量） |

### 通用输入字段（stdin JSON）

每个 hook 在 stdin 上接收一个包含以下通用字段的 JSON 对象，以及上面 Options 列中列出的任何 hook 专属字段：

| Field | Type | Description |
|-------|------|-------------|
| `hook_event_name` | string | 触发的 hook 事件名称（例如 `"PreToolUse"`、`"Stop"`） |
| `session_id` | string | 当前会话标识符 |
| `transcript_path` | string | 会话 transcript JSON 文件的路径 |
| `cwd` | string | 当前工作目录 |
| `permission_mode` | string | 当前权限模式：`default`、`plan`、`acceptEdits`、`dontAsk` 或 `bypassPermissions` |
| `agent_id` | string | 唯一的 subagent 标识符。当 hook 在 subagent context 中触发时存在（自 v2.1.69） |
| `agent_type` | string | Agent 类型名称（例如 `Bash`、`Explore`、`Plan` 或自定义）。当使用 `--agent <name>` 标志或在 subagent 内部时存在（自 v2.1.69） |

> **注意：** Hook 专属字段（例如 PreToolUse 的 `tool_name`、Stop 的 `last_assistant_message`）在上方 [Hook Events 概览](#hook-events-overview---official-30-hooks) 表格的 Options 列中列出。

## Hooks 管理命令

Claude Code 提供了用于管理 hooks 的内置命令：

- **`/hooks`** — 交互式 hook 管理 UI。无需编辑 JSON 文件即可查看、添加和删除 hooks。Hooks 按来源标记：`[User]`、`[Project]`、`[Local]`、`[Plugin]`。你还可以从此菜单切换 `disableAllHooks`。
- **`claude hooks reload`** — 重新加载 hooks 配置而不重新启动会话。在编辑 settings 文件后很有用（自 v2.0.47）。

## MCP Tool Matchers

对于 `PreToolUse`、`PostToolUse` 和 `PermissionRequest` hooks，你可以使用模式 `mcp__<server>__<tool>` 来匹配 MCP（Model Context Protocol）工具：

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

支持完整 regex：`mcp__memory__.*`（memory server 的所有工具）、`mcp__.*__write.*`（任何 server 的任何 write 工具）。

### Per-Hook Matcher Reference

Matchers 过滤哪些事件触发 hook。并非所有 hooks 都支持 matchers — 不支持 matcher 的 hooks 始终触发。

| Hook | Matcher Field | Possible Values | Example |
|------|--------------|-----------------|---------|
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
| `Elicitation` | `server_name` | MCP server 名称 | `"matcher": "my-mcp-server"` |
| `ElicitationResult` | `server_name` | MCP server 名称 | `"matcher": "my-mcp-server"` |
| `ConfigChange` | `config_source` | `user_settings`、`project_settings`、`local_settings`、`policy_settings`、`skills` | `"matcher": "project_settings"` |
| `UserPromptExpansion` | `command_name` | slash command 或 MCP prompt 名称（例如 `deploy`） | `"matcher": "deploy\|review"` |
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
| `FileChanged` | `filename`（basename） | 管道分隔的 basenames：`.envrc`、`.env`、`.env.local` | `"matcher": ".envrc\|.env"` |
| `Setup` | — | 不支持 matcher | 始终触发 |
| `PermissionDenied` | `tool_name` | 工具名称（与 PreToolUse 相同） | `"matcher": "Bash"` |

## 已知问题与变通方法

### Agent Stop Hook Bug（SubagentStop vs Stop）

**Bug Report：** [GitHub Issue #19220](https://github.com/anthropics/claude-code/issues/19220)

**问题：** 在 agent 的 frontmatter 中定义 `Stop` hook 时，传递给 hook 脚本的 `hook_event_name` 是 `"SubagentStop"` 而非 `"Stop"`。这与官方文档矛盾，并破坏了与其他 agent hooks（`PreToolUse` 和 `PostToolUse`）的一致性，后者正确传递其配置的名称。

| Hook | Defined As | Received As | Status |
|------|------------|-------------|--------|
| PreToolUse | `PreToolUse:` | `"PreToolUse"` | ✅ 正确 |
| PostToolUse | `PostToolUse:` | `"PostToolUse"` | ✅ 正确 |
| Stop | `Stop:` | `"SubagentStop"` | ❌ 不一致 |

**状态：** [官方 hooks reference](https://code.claude.com/docs/en/hooks#hooks-in-skills-and-agents) 现在将此文档化为预期行为：*"For subagents, Stop hooks are automatically converted to SubagentStop since that is the event that fires when a subagent completes."* 本项目通过 `hooks.py` 中的 `AGENT_HOOK_SOUND_MAP` 处理，该映射有一个单独的 `SubagentStop` 条目，指向 `agent_subagentstop` 声音文件夹。

### PreToolUse `updatedInput` for AskUserQuestion（自 v2.1.85）

当 `PreToolUse` hook 匹配 `AskUserQuestion` 时，它可以返回 `updatedInput` 来自动回答问题 — 使 headless integrations 能够以编程方式回答用户问题而无需手动输入：

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

这对于 CI/CD pipelines、自动化测试或任何 Claude Code 在没有人工终端的情况下运行的 context 很有用。尚未出现在官方 docs 页面中 — 来源于 GitHub changelog v2.1.85。

### PreToolUse `additionalContext` 现在在工具失败时被保留（v2.1.110）

v2.1.110 之前，如果工具本身随后失败，`PreToolUse` hook 返回的任何 `additionalContext` 会被**静默丢弃**。自 v2.1.110 起，即使匹配的工具调用失败，`PreToolUse` 的 `additionalContext` 也会被保留并传递给模型 — 所以无论工具结果如何，hook 提供的 context 都会一致地到达模型。

这不会影响本项目，因为 `hooks.py` 不返回 `additionalContext`。

### PermissionRequest `updatedInput` 重新检查 Deny Rules（v2.1.102、v2.1.110）

当 `PermissionRequest` hook 返回 `hookSpecificOutput.updatedInput` 重写工具输入时，重写的输入现在会重新通过权限 deny rules 评估。v2.1.102 / v2.1.110 之前（两个相关修复），hook 可能返回 `updatedInput` 绕过了配置的 deny rules — 这是一个与安全相关的边缘情况。

这不会影响本项目，因为 `hooks.py` 不使用 decision control 或 `updatedInput`。

### PreToolUse Decision Control 已弃用

`PreToolUse` hook 之前使用顶层的 `decision` 和 `reason` 字段来阻止工具调用。这些现在**已弃用**。请改用 `hookSpecificOutput.permissionDecision` 和 `hookSpecificOutput.permissionDecisionReason`：

| 已弃用 | 当前 |
|-----------|---------|
| `"decision": "approve"` | `"hookSpecificOutput": { "permissionDecision": "allow" }` |
| `"decision": "block"` | `"hookSpecificOutput": { "permissionDecision": "deny" }` |

这不会影响本项目，因为 `hooks.py` 使用 async 声音播放且不使用 decision control。

## Decision Control Patterns

不同的 hooks 使用不同的输出 schema 来阻止或控制执行。本项目不使用 decision control（所有 hooks 都是 async 声音播放），但作为参考：

| Hook(s) | Control Method | Values |
|---------|---------------|--------|
| PreToolUse | `hookSpecificOutput.permissionDecision` | `allow`、`deny`、`ask`、`defer`（仅 headless `-p` 模式，v2.1.89+） |
| PreToolUse | `hookSpecificOutput.autoAllow` | `true` — 自动批准该工具的未来使用（自 v2.0.76） |
| PermissionRequest | `hookSpecificOutput.decision.behavior` | `allow`、`deny` |
| Stop、SubagentStop、ConfigChange | 顶层 `decision` | `block` |
| PostToolBatch | 顶层 `decision` + 退出码 2 | `block` — 在一批并行工具调用完成后、下一次模型调用前停止 agentic loop |
| PreCompact | `decision` + 退出码 2 | `{"decision": "block"}` 或退出码 2 — 阻止 compaction（自 v2.1.105） |
| TeammateIdle、TaskCreated、TaskCompleted | `continue` + 退出码 2 | `{"continue": false, "stopReason": "..."}` — JSON decision control 在 v2.1.70 中添加。TaskCreated 也使用退出码 2 阻止任务创建（stderr 反馈给模型） |
| UserPromptSubmit | 可以修改 `prompt` 字段 | 通过 stdout 返回修改后的 prompt |
| UserPromptExpansion | 顶层 `decision` + `hookSpecificOutput.additionalContext` | `block` 阻止 slash command / MCP prompt 展开；`additionalContext` 在展开的 prompt 旁边注入 context |
| WorktreeCreate | 非零退出码 + stdout 路径 | 非零退出码使创建失败；stdout 提供 worktree 路径 |
| Elicitation | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 控制 MCP elicitation 响应 |
| ElicitationResult | `hookSpecificOutput.action` + `hookSpecificOutput.content` | `accept`、`decline`、`cancel` — 在发送到 server 之前覆盖用户响应 |
| PermissionDenied | `hookSpecificOutput.retry` | `true` — 表示模型可以重试被拒绝的工具调用（v2.1.89+） |
| MessageDisplay | `hookSpecificOutput.displayContent` | 仅替换屏幕文本 — 不会更改 transcript 或 Claude 看到的内容（仅显示，不能阻止） |

### 通用 JSON 输出字段

所有 hooks 可以通过 stdout JSON 返回以下字段：

| Field | Type | Description |
|-------|------|-------------|
| `continue` | bool | 如果为 `false`，完全停止 Claude |
| `stopReason` | string | 当 `continue` 为 false 时显示的消息 |
| `suppressOutput` | bool | 从 verbose 模式中隐藏 stdout |
| `systemMessage` | string | 显示给用户的警告消息 |
| `additionalContext` | string | 添加到 Claude 会话的 context |

## Hook Deduplication 和外部变更

- **Hook deduplication：** 在多个 settings 位置定义的相同 hook handlers 仅并行运行一次，防止重复执行。
- **外部变更检测：** 在活跃会话期间，当 hooks 被外部修改（例如另一个进程编辑 settings 文件）时，Claude Code 会发出警告。
