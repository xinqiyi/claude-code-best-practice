# HOOKS-README

包含 Codex CLI hooks 的所有详细信息、脚本和说明。

## Hook 事件概览

Codex CLI 通过 `hooks.json` 提供 **8 个 hooks**：

| # | Hook | 事件类型 | 配置文件 | 描述 |
|:-:|------|------------|-------------|-------------|
| 1 | `SessionStart` | `SessionStart` | `hooks.json` | 在会话启动时运行一次 — 注入上下文并播放声音 |
| 2 | `PreToolUse` | `PreToolUse` | `hooks.json` | 在工具执行前运行 — 播放声音 |
| 3 | `PermissionRequest` | `PermissionRequest` | `hooks.json` | 当 Codex 请求批准敏感操作时运行 — 播放声音 |
| 4 | `PostToolUse` | `PostToolUse` | `hooks.json` | 在工具完成后运行 — 播放声音 |
| 5 | `Stop` | `stop` | `hooks.json` | 在会话结束时运行 — 播放声音 |
| 6 | `UserPromptSubmit` | `UserPromptSubmit` | `hooks.json` | 当用户提交提示时运行 — 播放声音 |
| 7 | `PreCompact` | `PreCompact` | `hooks.json` | 在上下文压缩前运行 — 播放声音 |
| 8 | `PostCompact` | `PostCompact` | `hooks.json` | 在上下文压缩后运行 — 播放声音 |

### Hook 调用方式

所有 hooks（hooks.json）都通过 `--hook` 标志调用：
```
python3 .codex/hooks/scripts/hooks.py --hook SessionStart
python3 .codex/hooks/scripts/hooks.py --hook PreToolUse
python3 .codex/hooks/scripts/hooks.py --hook PermissionRequest
python3 .codex/hooks/scripts/hooks.py --hook PostToolUse
python3 .codex/hooks/scripts/hooks.py --hook Stop
python3 .codex/hooks/scripts/hooks.py --hook UserPromptSubmit
python3 .codex/hooks/scripts/hooks.py --hook PreCompact
python3 .codex/hooks/scripts/hooks.py --hook PostCompact
```

### SessionStart 上下文注入

SessionStart hook 将上下文输出到 **stdout**，该内容直接注入到模型的上下文窗口中。包括：
- 当前日期/时间
- Git 分支名称
- 工作树状态（干净或未提交的变更）
- 工作目录路径

## 前置条件

在使用 hooks 之前，请确保系统已安装 **Python 3**。

### 所需软件

#### 所有平台（Windows、macOS、Linux）
- **Python 3**：运行 hook 脚本所需
- 验证安装：`python3 --version`

**安装说明：**
- **Windows**：从 [python.org](https://www.python.org/downloads/) 下载，或通过 `winget install Python.Python.3` 安装
- **macOS**：通过 `brew install python3` 安装（需要 [Homebrew](https://brew.sh/)）
- **Linux**：通过 `sudo apt install python3`（Ubuntu/Debian）或 `sudo yum install python3`（RHEL/CentOS）安装

### 音频播放器（自动检测）

hook 脚本会自动检测并使用适合您平台的音频播放器：

- **macOS**：使用 `afplay`（内置，无需安装）
- **Linux**：使用来自 `pulseaudio-utils` 的 `paplay` — 通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**：使用内置的 `winsound` 模块（包含在 Python 中）

### 配置文件

共有 **两个** 配置文件：

1. **`.codex/hooks.json`** — 注册 `SessionStart`、`PreToolUse`、`PermissionRequest`、`PostToolUse`、`Stop`、`UserPromptSubmit`、`PreCompact` 和 `PostCompact` hooks
2. **`.codex/hooks/config/hooks-config.json`** — 启用/禁用单个 hook 和日志记录

#### hooks.json

```json
{
  "hooks": {
    "SessionStart": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook SessionStart",
        "statusMessage": "Initializing session hooks...",
        "timeout": 10
      }
    ],
    "PreToolUse": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook PreToolUse",
        "statusMessage": "Running pre-tool-use hook...",
        "timeout": 10
      }
    ],
    "PermissionRequest": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook PermissionRequest",
        "statusMessage": "Running permission request hook...",
        "timeout": 10
      }
    ],
    "PostToolUse": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook PostToolUse",
        "statusMessage": "Running post-tool-use hook...",
        "timeout": 10
      }
    ],
    "Stop": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook Stop",
        "statusMessage": "Running session stop hook...",
        "timeout": 10
      }
    ],
    "UserPromptSubmit": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook UserPromptSubmit",
        "statusMessage": "Running user prompt submit hook...",
        "timeout": 10
      }
    ],
    "PreCompact": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook PreCompact",
        "statusMessage": "Running pre-compact hook...",
        "timeout": 10
      }
    ],
    "PostCompact": [
      {
        "type": "shell",
        "command": "python3 .codex/hooks/scripts/hooks.py --hook PostCompact",
        "statusMessage": "Running post-compact hook...",
        "timeout": 10
      }
    ]
  }
}
```

## 配置 Hooks（启用/禁用）

### 禁用单个 Hook

编辑 `.codex/hooks/config/hooks-config.json`：
```json
{
  "disableSessionStartHook": false,
  "disablePreToolUseHook": false,
  "disablePermissionRequestHook": false,
  "disablePostToolUseHook": false,
  "disableStopHook": false,
  "disableUserPromptSubmitHook": false,
  "disablePreCompactHook": false,
  "disablePostCompactHook": false,
  "disableLogging": true
}
```

**配置选项：**
- `disableSessionStartHook`：设为 `true` 以禁用会话启动的上下文注入和声音
- `disablePreToolUseHook`：设为 `true` 以禁用工具使用前的声音
- `disablePermissionRequestHook`：设为 `true` 以禁用权限请求的声音
- `disablePostToolUseHook`：设为 `true` 以禁用工具使用后的声音
- `disableStopHook`：设为 `true` 以禁用会话停止的声音
- `disableUserPromptSubmitHook`：设为 `true` 以禁用用户提交提示时的声音
- `disablePreCompactHook`：设为 `true` 以禁用压缩前的声音
- `disablePostCompactHook`：设为 `true` 以禁用压缩后的声音
- `disableLogging`：设为 `true` 以禁用将 hook 事件记录到 `.codex/hooks/logs/hooks-log.jsonl`

### 配置回退

共有两个配置文件：

1. **`.codex/hooks/config/hooks-config.json`** — 提交到 git 的共享/默认配置
2. **`.codex/hooks/config/hooks-config.local.json`** — 个人覆盖配置（被 git 忽略）

本地配置文件（`.local.json`）优先级高于共享配置，允许每个开发者自定义其 hook 行为而不影响团队。

#### 本地配置（个人覆盖）

创建或编辑 `.codex/hooks/config/hooks-config.local.json` 以设置个人偏好：

```json
{
  "disableSessionStartHook": false,
  "disablePreToolUseHook": false,
  "disablePermissionRequestHook": false,
  "disablePostToolUseHook": false,
  "disableStopHook": true,
  "disableUserPromptSubmitHook": false,
  "disablePreCompactHook": false,
  "disablePostCompactHook": false,
  "disableLogging": true
}
```

### 日志记录

当日志记录启用时（`"disableLogging": false`），hook 事件会以 JSON Lines 格式记录到 `.codex/hooks/logs/hooks-log.jsonl`。每条记录包含从 Codex CLI 接收的完整 JSON 负载。

## 测试

运行测试套件：
```bash
python3 -m unittest tests.test_hooks -v
```

## 语音

用于生成声音的网站：https://elevenlabs.io/
使用的语音：Adam - American, Dark and Tough

## 未来扩展

该项目可以通过以下方式扩展：

1. 在 `hooks.py` 中添加新的 `HOOK_SOUND_MAP` 条目
2. 在 `.codex/hooks/sounds/` 中添加对应的声音文件
3. 在 `hooks-config.json` 中添加切换开关
4. 在 `hooks.json` 中添加新的 hook 条目
