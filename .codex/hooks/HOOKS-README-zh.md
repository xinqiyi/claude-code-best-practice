# HOOKS-README
包含 Codex CLI hooks 的全部细节、脚本和说明。

## Hook 事件概览

Codex CLI 通过 hooks.json 提供 **8 个 hook**：

| # | Hook | Event Type | Config File | Description |
|:-:|------|------------|-------------|-------------|
| 1 | `SessionStart` | `SessionStart` | `hooks.json` | 会话启动时运行一次——注入 context 并播放声音 |
| 2 | `PreToolUse` | `PreToolUse` | `hooks.json` | 工具执行前运行——播放声音 |
| 3 | `PermissionRequest` | `PermissionRequest` | `hooks.json` | Codex 请求敏感操作审批时运行——播放声音 |
| 4 | `PostToolUse` | `PostToolUse` | `hooks.json` | 工具完成后运行——播放声音 |
| 5 | `Stop` | `stop` | `hooks.json` | 会话结束时运行——播放声音 |
| 6 | `UserPromptSubmit` | `UserPromptSubmit` | `hooks.json` | 用户提交 prompt 时运行——播放声音 |
| 7 | `PreCompact` | `PreCompact` | `hooks.json` | 上下文压缩前运行——播放声音 |
| 8 | `PostCompact` | `PostCompact` | `hooks.json` | 上下文压缩后运行——播放声音 |

### Hook 调用方式

所有 hook（hooks.json）均使用 `--hook` 标志调用：
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

SessionStart hook 将上下文输出到 **stdout**，直接送入模型的 context window。内容包括：
- 当前日期/时间
- Git 分支名称
- Working tree 状态（clean 或有未提交的更改）
- Working directory 路径

## 前置条件

使用 hook 之前，请确保系统已安装 **Python 3**。

### 所需软件

#### 所有平台（Windows、macOS、Linux）
- **Python 3**：运行 hook 脚本所必需
- 验证安装：`python3 --version`

**安装说明：**
- **Windows**：从 [python.org](https://www.python.org/downloads/) 下载或通过 `winget install Python.Python.3` 安装
- **macOS**：通过 `brew install python3` 安装（需要 [Homebrew](https://brew.sh/)）
- **Linux**：通过 `sudo apt install python3`（Ubuntu/Debian）或 `sudo yum install python3`（RHEL/CentOS）安装

### 音频播放器（自动检测）

hook 脚本会自动检测并使用适用于你平台的音频播放器：

- **macOS**：使用 `afplay`（系统内置，无需安装）
- **Linux**：使用 `paplay`（来自 `pulseaudio-utils`）——通过 `sudo apt install pulseaudio-utils` 安装
- **Windows**：使用内置的 `winsound` 模块（Python 自带）

### 配置文件

共有 **两个** 配置文件：

1. **`.codex/hooks.json`** — 注册 `SessionStart`、`PreToolUse`、`PermissionRequest`、`PostToolUse`、`Stop`、`UserPromptSubmit`、`PreCompact` 和 `PostCompact` hook
2. **`.codex/hooks/config/hooks-config.json`** — 启用/禁用各个 hook 以及日志记录

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

## 配置 Hook（启用/禁用）

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
- `disableSessionStartHook`：设为 `true` 以禁用会话启动时的 context 注入和声音
- `disablePreToolUseHook`：设为 `true` 以禁用工具执行前的声音
- `disablePermissionRequestHook`：设为 `true` 以禁用权限请求时的声音
- `disablePostToolUseHook`：设为 `true` 以禁用工具完成后的声音
- `disableStopHook`：设为 `true` 以禁用会话停止时的声音
- `disableUserPromptSubmitHook`：设为 `true` 以禁用用户提交 prompt 时的声音
- `disablePreCompactHook`：设为 `true` 以禁用上下文压缩前的声音
- `disablePostCompactHook`：设为 `true` 以禁用上下文压缩后的声音
- `disableLogging`：设为 `true` 以禁用将 hook 事件记录到 `.codex/hooks/logs/hooks-log.jsonl`

### 配置回退

有两个配置文件：

1. **`.codex/hooks/config/hooks-config.json`** — 共享/默认配置，提交到 git
2. **`.codex/hooks/config/hooks-config.local.json`** — 你的个人覆盖配置（git-ignored）

本地配置文件（`.local.json`）优先于共享配置，允许每位开发者自定义自己的 hook 行为而不影响团队。

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

当日志启用时（`"disableLogging": false`），hook 事件会以 JSON Lines 格式记录到 `.codex/hooks/logs/hooks-log.jsonl`。每个条目包含从 Codex CLI 接收到的完整 JSON payload。

## 测试

运行测试套件：
```bash
python3 -m unittest tests.test_hooks -v
```

## 语音

用于生成声音的网站：https://elevenlabs.io/
使用的语音：Adam - American, Dark and Tough

## 未来可扩展性

本项目可通过以下方式扩展：

1. 在 `hooks.py` 的 `HOOK_SOUND_MAP` 中添加新条目
2. 在 `.codex/hooks/sounds/` 中添加对应的声音文件
3. 在 `hooks-config.json` 中添加开关
4. 在 `hooks.json` 中添加新的 hook 条目
