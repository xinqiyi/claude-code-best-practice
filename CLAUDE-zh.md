# CLAUDE.md

当在本仓库中编写代码时，本文件为 Claude Code (claude.ai/code) 提供指导。

## 仓库概览

这是一个 Claude Code 配置的最佳实践仓库，展示了 skills、subagents、hooks 和 commands 的使用模式。它作为参考实现而非应用代码库存在。

## 核心组件

### 天气系统（示例 Workflow）
通过 **Command → Agent → Skill** 架构演示两种不同的 skill 模式：
- `/weather-orchestrator` command (`.claude/commands/weather-orchestrator.md`): 入口 — 询问用户选择 C/F，invoke agent，然后 invoke SVG skill
- `weather-agent` agent (`.claude/agents/weather-agent.md`): 使用其 preloaded 的 `weather-fetcher` skill 获取温度（agent skill 模式）
- `weather-fetcher` skill (`.claude/skills/weather-fetcher/SKILL.md`): Preloaded 到 agent 中 — 从 Open-Meteo 获取温度的指令
- `weather-svg-creator` skill (`.claude/skills/weather-svg-creator/SKILL.md`): Skill — 创建 SVG 天气卡片，写入 `orchestration-workflow/weather.svg` 和 `orchestration-workflow/output.md`

两种 skill 模式：agent skills（通过 `skills:` 字段 preloaded）与 skills（通过 `Skill` tool invoke）。完整的流程 diagram 见 `orchestration-workflow/orchestration-workflow.md`。

### Skill 定义结构
`.claude/skills/<name>/SKILL.md` 中的 skills 使用 YAML frontmatter：
- `name`: 显示名称和 `/slash-command`（默认为目录名）
- `description`: 何时 invoke（推荐用于自动发现）
- `argument-hint`: 自动补全提示（例如 `[issue-number]`）
- `disable-model-invocation`: 设为 `true` 以阻止自动 invocation
- `user-invocable`: 设为 `false` 以在 `/` 菜单中隐藏（仅作为背景知识）
- `allowed-tools`: skill 处于 active 状态时无需 permission prompt 即可使用的 tools
- `model`: skill 处于 active 状态时使用的 model
- `context`: 设为 `fork` 以在隔离的 subagent context 中运行
- `agent`: `context: fork` 所使用的 subagent 类型（默认: `general-purpose`）
- `hooks`: 限定于此 skill 的 lifecycle hooks

### 演示文稿系统
参见 `.claude/rules/presentation.md` — 演示文稿工作按单个演示委托给 `presentation-vibe-coding`（用于 `presentation/vibe-coding-to-agentic-engineering/`）或 `presentation-claude-gemini`（用于 `presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini/`）。

### Hooks 系统
`.claude/hooks/` 中的跨平台声音通知系统：
- `scripts/hooks.py`: Claude Code hook 事件的主 handler
- `config/hooks-config.json`: 团队共享配置
- `config/hooks-config.local.json`: 个人覆盖（git-ignored）
- `sounds/`: 按 hook 事件组织的音频文件（通过 ElevenLabs TTS 生成）

`.claude/settings.json` 中配置的 hook 事件: PreToolUse, PostToolUse, UserPromptSubmit, Notification, Stop, SubagentStart, SubagentStop, PreCompact, SessionStart, SessionEnd, Setup, PermissionRequest, TeammateIdle, TaskCompleted, ConfigChange。

特殊处理: git commits 触发 `pretooluse-git-committing` 声音。

## 关键模式

### Subagent 编排
Subagents **不能** 通过 bash 命令 invoke 其他 subagents。应使用 Agent tool（在 v2.1.63 中从 Task 重命名；`Task(...)` 仍然可作为别名使用）：
```
Agent(subagent_type="agent-name", description="...", prompt="...", model="haiku")
```

在 subagent 定义中明确说明 tool 使用方式。避免使用可能被误解为 bash 命令的模糊术语，如 "launch"。

### Subagent 定义结构
`.claude/agents/*.md` 中的 subagents 使用 YAML frontmatter：
- `name`: Subagent 标识符
- `description`: 何时 invoke（使用 "PROACTIVELY" 以自动 invocation）
- `tools`: 逗号分隔的 tools 允许列表（省略时继承所有）。支持 `Agent(agent_type)` 语法
- `disallowedTools`: 要拒绝的 tools，从继承或指定列表中移除
- `model`: Model 别名: `haiku`, `sonnet`, `opus` 或 `inherit`（默认: `inherit`）
- `permissionMode`: Permission 模式（例如 `"acceptEdits"`, `"plan"`, `"bypassPermissions"`）
- `maxTurns`: Subagent 停止前的最大 agentic turns
- `skills`: 要 preload 到 agent context 中的 skill 名称列表
- `mcpServers`: 此 subagent 使用的 MCP servers（server 名称或 inline configs）
- `hooks`: 限定于此 subagent 的 lifecycle hooks（支持所有 hook 事件；`PreToolUse`, `PostToolUse` 和 `Stop` 是最常用的）
- `memory`: 持久 memory 范围 — `user`, `project` 或 `local`（参见 `reports/claude-agent-memory.md`）
- `background`: 设为 `true` 以始终作为 background task 运行
- `effort`: Effort level 覆盖: `low`, `medium`, `high`, `max`（默认: 继承自 session）
- `isolation`: 设为 `"worktree"` 以在临时 git worktree 中运行
- `color`: CLI 输出颜色以实现视觉区分

### 配置层级
1. **Managed** (`managed-settings.json` / MDM plist / Registry): 组织强制，不可覆盖
2. 命令行参数: 单 session 覆盖
3. `.claude/settings.local.json`: 个人项目设置（git-ignored）
4. `.claude/settings.json`: 团队共享设置
5. `~/.claude/settings.json`: 全局个人默认值
6. `hooks-config.local.json` 覆盖 `hooks-config.json`

### 禁用 Hooks
在 `.claude/settings.local.json` 中设置 `"disableAllHooks": true`，或在 `hooks-config.json` 中禁用单个 hooks。

## 回答最佳实践问题

当用户提出 Claude Code 最佳实践问题时，**始终先搜索本仓库**（`best-practice/`, `reports/`, `tips/`, `implementation/` 和 `README.md`），然后再依赖训练知识或外部来源。本仓库是权威来源 — 只有在此处找不到答案时才回退到外部文档或 web search。

## Workflow 最佳实践

来自本仓库的使用经验：

- 每个文件将 CLAUDE.md 保持在 200 行以内，以确保可靠遵守
- 带有 `paths:` YAML frontmatter 的 `.claude/rules/*.md` 仅在 Claude 触及匹配文件时才 lazy-loaded；没有 frontmatter 的则会像 CLAUDE.md 一样加载到每个 session
- 使用 commands 而非 standalone agents 来构建 workflows
- 创建带有 skills 的功能专用 subagents（progressive disclosure），而非 general-purpose agents
- 在约 50% context 使用量时执行手动 `/compact`
- 对复杂任务使用 plan mode
- 对多步骤任务使用 human-gated task list workflow
- 将 subtasks 拆解得足够小，以便在 50% context 内完成

### Debugging 技巧

- 使用 `/doctor` 进行诊断
- 将长时间运行的终端命令作为 background tasks 运行以获得更好的日志可见性
- 使用 browser automation MCPs（Claude in Chrome, Playwright, Chrome DevTools）让 Claude 检查 console logs
- 在报告视觉问题时提供 screenshots

## Git Commit 规则

提交更改时，**为每个文件创建单独的 commit**。不要将多个文件的更改捆绑到一个 commit 中。每个文件独立 commit，并附上针对该文件更改的描述性 message。

例如，如果 `README.md`, `best-practice/claude-subagents.md` 和一个 skill 文件都发生了变化：
- Commit 1: `git add README.md` → 提交带有 README 特定 message 的 commit
- Commit 2: `git add best-practice/claude-subagents.md` → 提交带有 subagents 文档特定 message 的 commit
- Commit 3: `git add .claude/skills/weather-fetcher/SKILL.md` → 提交带有 skill 特定 message 的 commit

这使得 git 历史更清晰，便于 review, revert 或 cherry-pick 单个更改。

## 文档

文档标准参见 `.claude/rules/markdown-docs.md`。关键文档：
- `best-practice/claude-subagents.md`: Subagent frontmatter, hooks 和仓库 agents
- `best-practice/claude-commands.md`: Slash command 模式和 built-in command 参考
- `orchestration-workflow/orchestration-workflow.md`: 天气系统流程 diagram
