# CLAUDE.md

本文档为 Claude Code（claude.ai/code）在此仓库中处理代码时提供指导。

## 仓库概述

这是一个 Claude Code 配置的最佳实践仓库，演示了 skills、subagents、hooks 和 commands 的模式。它作为一个参考实现，而非应用代码库。

## 关键组件

### 天气系统（示例工作流）
通过 **Command → Agent → Skill** 架构展示两种不同的 skill 模式：
- `/weather-orchestrator` 命令（`.claude/commands/weather-orchestrator.md`）：入口点——询问用户选择 C/F，调用 agent，然后调用 SVG skill
- `weather-agent` agent（`.claude/agents/weather-agent.md`）：使用预加载的 `weather-fetcher` skill 获取温度（agent skill 模式）
- `weather-fetcher` skill（`.claude/skills/weather-fetcher/SKILL.md`）：预加载到 agent 中——从 Open-Meteo 获取温度的指令
- `weather-svg-creator` skill（`.claude/skills/weather-svg-creator/SKILL.md`）：Skill——创建 SVG 天气卡片，写入 `orchestration-workflow/weather.svg` 和 `orchestration-workflow/output.md`

两种 skill 模式：agent skills（通过 `skills:` 字段预加载）与 skills（通过 `Skill` 工具调用）。完整流程图请参阅 `orchestration-workflow/orchestration-workflow.md`。

### Skill 定义结构
`.claude/skills/<name>/SKILL.md` 中的 skills 使用 YAML frontmatter：
- `name`：显示名称和 `/slash-command`（默认使用目录名）
- `description`：何时调用（推荐用于自动发现）
- `argument-hint`：自动补全提示（例如 `[issue-number]`）
- `disable-model-invocation`：设置为 `true` 以阻止自动调用
- `user-invocable`：设置为 `false` 以从 `/` 菜单中隐藏（仅作为背景知识）
- `allowed-tools`：skill 激活时无需权限提示即可使用的工具
- `model`：skill 激活时使用的模型
- `context`：设置为 `fork` 以在隔离的 subagent 上下文中运行
- `agent`：`context: fork` 时的 subagent 类型（默认：`general-purpose`）
- `hooks`：限定到此 skill 的生命周期 hooks

### 演示系统
参阅 `.claude/rules/presentation.md`——演示工作按每次演示委托给 `presentation-vibe-coding`（用于 `presentation/vibe-coding-to-agentic-engineering/`）或 `presentation-claude-gemini`（用于 `presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini/`）。

### Hooks 系统
`.claude/hooks/` 中的跨平台声音通知系统：
- `scripts/hooks.py`：Claude Code hook 事件的主处理器
- `config/hooks-config.json`：共享团队配置
- `config/hooks-config.local.json`：个人覆盖配置（git-ignored）
- `sounds/`：按 hook 事件组织的音频文件（通过 ElevenLabs TTS 生成）

在 `.claude/settings.json` 中配置的 Hook 事件：PreToolUse, PostToolUse, UserPromptSubmit, Notification, Stop, SubagentStart, SubagentStop, PreCompact, SessionStart, SessionEnd, Setup, PermissionRequest, TeammateIdle, TaskCompleted, ConfigChange。

特殊处理：git 提交触发 `pretooluse-git-committing` 声音。

## 关键模式

### Subagent 编排
Subagents **不能**通过 bash 命令调用其他 subagents。请使用 Agent 工具（从 v2.1.63 起由 Task 重命名而来；`Task(...)` 仍可作为别名使用）：
```
Agent(subagent_type="agent-name", description="...", prompt="...", model="haiku")
```

在 subagent 定义中要明确工具的用途。避免使用像 "launch" 这样可能被误解为 bash 命令的模糊术语。

### Subagent 定义结构
`.claude/agents/*.md` 中的 Subagents 使用 YAML frontmatter：
- `name`：Subagent 标识符
- `description`：何时调用（使用 "PROACTIVELY" 表示自动调用）
- `tools`：工具的白名单，逗号分隔（省略则继承所有工具）。支持 `Agent(agent_type)` 语法
- `disallowedTools`：拒绝使用的工具，从继承或指定的列表中移除
- `model`：模型别名：`haiku`、`sonnet`、`opus` 或 `inherit`（默认：`inherit`）
- `permissionMode`：权限模式（例如 `"acceptEdits"`、`"plan"`、`"bypassPermissions"`）
- `maxTurns`：subagent 停止前的最大 agentic 轮次
- `skills`：要预加载到 agent 上下文中的 skill 名称列表
- `mcpServers`：此 subagent 的 MCP 服务器（服务器名称或内联配置）
- `hooks`：限定到此 subagent 的生命周期 hooks（支持所有 hook 事件；`PreToolUse`、`PostToolUse` 和 `Stop` 最为常用）
- `memory`：持久化内存范围——`user`、`project` 或 `local`（参见 `reports/claude-agent-memory.md`）
- `background`：设置为 `true` 以始终作为后台任务运行
- `effort`：努力级别覆盖：`low`、`medium`、`high`、`max`（默认：继承自会话）
- `isolation`：设置为 `"worktree"` 以在临时 git worktree 中运行
- `color`：CLI 输出颜色，便于视觉区分

### 配置层级
1. **Managed**（`managed-settings.json` / MDM plist / Registry）：组织强制，不可覆盖
2. 命令行参数：单会话覆盖
3. `.claude/settings.local.json`：个人项目设置（git-ignored）
4. `.claude/settings.json`：团队共享设置
5. `~/.claude/settings.json`：全局个人默认设置
6. `hooks-config.local.json` 覆盖 `hooks-config.json`

### 禁用 Hooks
在 `.claude/settings.local.json` 中设置 `"disableAllHooks": true`，或在 `hooks-config.json` 中禁用单个 hooks。

## 回答最佳实践问题

当用户提出 Claude Code 最佳实践问题时，**始终优先在此仓库中搜索**（`best-practice/`、`reports/`、`tips/`、`implementation/` 和 `README.md`），然后再依赖训练知识或外部来源。此仓库是权威来源——仅在找不到答案时才回退到外部文档或网络搜索。

## 工作流最佳实践

来自此仓库的经验：

- 将 CLAUDE.md 保持在每个文件 200 行以内以确保可靠的遵循
- 带有 `paths:` YAML frontmatter 的 `.claude/rules/*.md` 仅在 Claude 触及匹配文件时懒加载；没有 frontmatter 则像 CLAUDE.md 一样加载到每个会话中
- 使用 commands 进行工作流，而非独立的 agents
- 创建带有 skills 的特定功能 subagents（渐进式信息呈现），而不是通用 agents
- 在上下文使用约 50% 时手动执行 `/compact`
- 复杂任务从 plan 模式开始
- 对多步骤任务使用人工控制的 task list workflow
- 将子任务拆解到足够小，以便在 50% 上下文内完成

### 调试技巧

- 使用 `/doctor` 进行诊断
- 将长时间运行的终端命令作为后台任务运行以获得更好的日志可见性
- 使用浏览器自动化 MCPs（Claude in Chrome、Playwright、Chrome DevTools）让 Claude 检查控制台日志
- 报告视觉问题时提供截图

## Git 提交规则

提交更改时，**每个文件创建独立的提交**。不要将多个文件的更改捆绑到单个提交中。每个文件应有其自己的提交，并附带特定于该文件更改的描述性消息。

例如，如果 `README.md`、`best-practice/claude-subagents.md` 和一个 skill 文件都发生了更改：
- 提交 1：`git add README.md` → 附带 README 特定消息的提交
- 提交 2：`git add best-practice/claude-subagents.md` → 附带 subagents-doc 特定消息的提交
- 提交 3：`git add .claude/skills/weather-fetcher/SKILL.md` → 附带 skill 特定消息的提交

这使得 git 历史更清晰，更易于审查、回滚或挑选单个更改。

## 文档

文档标准请参阅 `.claude/rules/markdown-docs.md`。关键文档：
- `best-practice/claude-subagents.md`：Subagent frontmatter、hooks 和 repository agents
- `best-practice/claude-commands.md`：Slash command 模式和内置命令参考
- `orchestration-workflow/orchestration-workflow.md`：天气系统流程图
