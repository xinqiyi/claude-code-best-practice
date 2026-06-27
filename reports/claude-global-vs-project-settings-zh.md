# Claude Code: Global 与 Project-Level 功能

全面对比哪些 Claude Code 功能是仅 global（`~/.claude/`）的，哪些在 global 和 project-level（`.claude/`）都有对等实现。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Table of Contents

1. [Overview](#overview)
2. [Global-Only Features](#global-only-features)
3. [Dual-Scope Features](#dual-scope-features)
4. [Settings Precedence](#settings-precedence)
5. [Directory Structure Comparison](#directory-structure-comparison)
6. [Tasks System](#tasks-system)
7. [Agent Teams](#agent-teams)
8. [Design Principles](#design-principles)
9. [Sources](#sources)

---

## Overview

Claude Code 使用**scope 层次结构**，其中一些功能在 global（`~/.claude/`）和 project（`.claude/`）级别都存在，而另一些则仅限 global。设计原则：属于*个人状态*或*跨项目协调*的内容存储在 global；属于*团队可共享的项目 config* 的内容可以存在 project 级别。

- `~/.claude/` 是你的 **user-level 主目录**（global，所有项目）
- `.claude/` 在仓库中是你的 **project-level 主目录**（仅限于该项目）

---

## Global-Only Features

这些**仅**存在于 `~/.claude/` 下，无法限定到某个项目：

| Feature | Location | Purpose |
|---------|----------|---------|
| **Tasks** | `~/.claude/tasks/` | 跨 session 和 agents 的持久化任务列表 |
| **Agent Teams** | `~/.claude/teams/` | 多 agent 协调配置（实验性，2026 年 2 月） |
| **Auto Memory** | `~/.claude/projects/<hash>/memory/` | Claude 自我编写的每个项目的学习记录（个人，永不共享） |
| **Credentials & OAuth** | 系统 keychain + `~/.claude.json` | API keys、OAuth tokens（永远不在项目文件中） |
| **Keybindings** | `~/.claude/keybindings.json` | 自定义键盘快捷键 |
| **MCP User Servers** | `~/.claude.json`（`mcpServers` key） | 跨所有项目的个人 MCP servers |
| **Preferences/Cache** | `~/.claude.json` | 主题、模型、output style、session 状态 |

---

## Dual-Scope Features

这些在两个级别都存在，**project-level 优先于** global：

| Feature | Global（`~/.claude/`） | Project（`.claude/`） | 优先级 |
|---------|----------------------|---------------------|------------|
| **CLAUDE.md** | `~/.claude/CLAUDE.md` | `./CLAUDE.md` 或 `.claude/CLAUDE.md` | Project 覆盖 global |
| **Settings** | `~/.claude/settings.json` | `.claude/settings.json` + `.claude/settings.local.json` | Project > Global |
| **Rules** | `~/.claude/rules/*.md` | `.claude/rules/*.md` | Project 覆盖 |
| **Agents/Subagents** | `~/.claude/agents/*.md` | `.claude/agents/*.md` | Project 覆盖 |
| **Commands** | `~/.claude/commands/*.md` | `.claude/commands/*.md` | 两者均可用 |
| **Skills** | `~/.claude/skills/` | `.claude/skills/` | 两者均可用 |
| **Hooks** | `~/.claude/hooks/` | `.claude/hooks/` | 两者均执行 |
| **MCP Servers** | `~/.claude.json`（user scope） | `.mcp.json`（project scope） | 三个 scope：local > project > user |

---

## Settings Precedence

用户可写的 settings 按此覆盖顺序应用（从最高到最低）：

| Priority | Location | Scope | Version Control | Purpose |
|----------|----------|-------|-----------------|---------|
| 1 | 命令行 flags | Session | 不适用 | 单次 session 覆盖 |
| 2 | `.claude/settings.local.json` | Project | No（git-ignored） | 个人项目特定 |
| 3 | `.claude/settings.json` | Project | Yes（已提交） | 团队共享 settings |
| 4 | `~/.claude/settings.local.json` | User | 不适用 | 个人 global 覆盖 |
| 5 | `~/.claude/settings.json` | User | 不适用 | Global 个人 settings |

策略层：`managed-settings.json` 是组织强制的，不能被本地文件覆盖。

**重要**：`deny` 规则具有最高的安全优先级，不能被低优先级的 allow/ask 规则覆盖。

---

## Directory Structure Comparison

### Global Scope（`~/.claude/`）

```
~/.claude/
├── settings.json              # User-level settings（所有项目）
├── settings.local.json        # 个人覆盖
├── CLAUDE.md                  # User memory（所有项目）
├── agents/                    # User subagents（对所有项目可用）
│   └── *.md
├── rules/                     # User-level 模块化 rules
│   └── *.md
├── commands/                  # User-level commands
│   └── *.md
├── skills/                    # User-level skills
│   └── */SKILL.md
├── tasks/                     # GLOBAL-ONLY：任务列表
│   └── {task-list-id}/
├── teams/                     # GLOBAL-ONLY：Agent team 配置
│   └── {team-name}/
│       └── config.json
├── projects/                  # GLOBAL-ONLY：每个项目的 auto-memory
│   └── {project-hash}/
│       └── memory/
│           ├── MEMORY.md
│           └── *.md
├── keybindings.json           # GLOBAL-ONLY：键盘快捷键
└── hooks/                     # User-level hooks
    ├── scripts/
    └── config/

~/.claude.json                 # GLOBAL-ONLY：MCP servers、OAuth、preferences、caches
```

### Project Scope（`.claude/`）

```
.claude/
├── settings.json              # 团队共享 settings
├── settings.local.json        # 个人项目覆盖（git-ignored）
├── CLAUDE.md                  # Project memory（替代 ./CLAUDE.md）
├── agents/                    # Project subagents
│   └── *.md
├── rules/                     # Project-level 模块化 rules
│   └── *.md
├── commands/                  # 自定义 slash commands
│   └── *.md
├── skills/                    # 自定义 skills
│   └── {skill-name}/
│       ├── SKILL.md
│       └── supporting-files/
├── hooks/                     # Project-level hooks
│   ├── scripts/
│   └── config/
└── plugins/                   # 已安装的 plugins

.mcp.json                      # Project-scoped MCP servers（仓库根目录）
```

---

## Tasks System

在 **Claude Code v2.1.16**（2026 年 1 月 22 日）中引入，取代已弃用的 TodoWrite 系统。

### 存储

Tasks 存储在本地文件系统的 `~/.claude/tasks/` 中（不在云数据库中）。这使得 task 状态可审计、可版本控制且可崩溃恢复。

### Tools

| Tool | Purpose |
|------|---------|
| **TaskCreate** | 创建新 task，包含 `subject`、`description` 和 `activeForm` |
| **TaskGet** | 按 ID 获取特定 task 的完整详情 |
| **TaskUpdate** | 更改状态、设置 owner、添加依赖关系或删除 |
| **TaskList** | 列出所有 tasks 及其当前状态 |

### Task Lifecycle

```
pending  →  in_progress  →  completed
```

### Dependency Management

Tasks 可以通过 `addBlockedBy`/`addBlocks` 阻止其他 tasks，创建防止过早执行的依赖关系图。

### Multi-Session Collaboration

```bash
CLAUDE_CODE_TASK_LIST_ID=my-project-tasks claude
```

共享相同 ID 的所有 session 实时看到 task 更新，支持并行工作流和 session 恢复。

### 与旧 Todos 的关键区别

| Feature | Old Todos | New Tasks |
|---------|-----------|-----------|
| Scope | 单 session | 跨 session、跨 agent |
| Dependencies | 无 | 完整的依赖关系图 |
| Storage | 仅内存中 | 文件系统（`~/.claude/tasks/`） |
| Persistence | Session 结束时丢失 | 在重启和崩溃后仍然存在 |
| Multi-session | 不可能 | 通过 `CLAUDE_CODE_TASK_LIST_ID` |

---

## Agent Teams

于 **2026 年 2 月 5 日** 作为实验性功能宣布。Agent Teams 允许多个 Claude Code session 协调共享工作。

### 启用

```json
// 在 ~/.claude/settings.json 中
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### 配置

Team 配置位于 `~/.claude/teams/{team-name}/` 并支持以下模式：

| Mode | Description | Requirements |
|------|-------------|--------------|
| **In-process**（默认） | 所有 teammates 在你的终端内运行 | 无 |
| **Split panes** | 每个 teammate 获得自己的 pane | tmux 或 iTerm2（不可用 VS Code terminal） |

---

## Design Principles

global-only 与 dual-scope 的划分遵循清晰的模式：

| Category | Scope | Rationale |
|----------|-------|-----------|
| **Coordination state**（tasks, teams） | Global-only | 需要超越任何单个项目的持久性 |
| **Security state**（credentials, OAuth） | Global-only | 防止意外提交到版本控制 |
| **Personal learning**（auto-memory） | Global-only | 用户特定，不可团队共享 |
| **Input preferences**（keybindings） | Global-only | 用户肌肉记忆，非项目特定 |
| **Configuration**（settings, rules, agents） | 两种级别 | 团队需要共享项目特定行为 |
| **Workflow definitions**（commands, skills） | 两种级别 | 可以是个人的或团队共享的 |

Auto-memory（`~/.claude/projects/<hash>/memory/`）是一个值得注意的混合体：它*关于*特定项目，但存储在*global*，因为它代表个人学习而非团队可共享的配置。

---

## Sources

- [Claude Code Settings Documentation](https://code.claude.com/docs/en/settings)
- [Orchestrate Teams of Claude Code Sessions](https://code.claude.com/docs/en/agent-teams)
- [What are Tasks in Claude Code - ClaudeLog](https://claudelog.com/faqs/what-are-tasks-in-claude-code/)
- [Claude Code Task Management - ClaudeFast](https://claudefa.st/blog/guide/development/task-management)
- [Claude Code Tasks Update - VentureBeat](https://venturebeat.com/orchestration/claude-codes-tasks-update-lets-agents-work-longer-and-coordinate-across)
- [Where Are Claude Code Global Settings - ClaudeLog](https://claudelog.com/faqs/where-are-claude-code-global-settings/)
- [Claude Opus 4.6 Agent Teams - VentureBeat](https://venturebeat.com/technology/anthropics-claude-opus-4-6-brings-1m-token-context-and-agent-teams-to-take)
- [How to Set Up Claude Code Agent Teams (Full Walkthrough) - r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1qz8tyy/how_to_set_up_claude_code_agent_teams_full/)
- [Anthropic replaced Claude Code's old 'Todos' with Tasks - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1qkjznp/anthropic_replaced_claude_codes_old_todos_with/)
