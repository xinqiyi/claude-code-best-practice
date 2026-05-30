# Claude Code: 全局与项目级功能对比

全局专属（`~/.claude/`）与同时拥有全局和项目级（`.claude/`）配置的 Claude Code 功能的全面对比。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 目录

1. [概述](#overview)
2. [全局专属功能](#global-only-features)
3. [双作用域功能](#dual-scope-features)
4. [设置优先级](#settings-precedence)
5. [目录结构对比](#directory-structure-comparison)
6. [任务系统](#tasks-system)
7. [Agent 团队](#agent-teams)
8. [设计原则](#design-principles)
9. [来源](#sources)

---

## 概述

Claude Code 使用**作用域层级**，某些功能同时存在于全局（`~/.claude/`）和项目（`.claude/`）层级，而其他功能则仅限全局。设计原则是：*个人状态*或*跨项目协调*相关内容放在全局；*团队可共享的项目配置*则放在项目层级。

- `~/.claude/` 是你的**用户级根目录**（全局，适用于所有项目）
- 仓库内的 `.claude/` 是你的**项目级根目录**（仅限于该项目）

---

## 全局专属功能

以下功能**仅**位于 `~/.claude/` 下，无法限定在项目范围：

| 功能 | 位置 | 用途 |
|---------|----------|---------|
| **任务 (Tasks)** | `~/.claude/tasks/` | 跨会话和跨 agent 的持久化任务列表 |
| **Agent 团队** | `~/.claude/teams/` | 多 agent 协调配置（实验性，2026 年 2 月） |
| **自动记忆 (Auto Memory)** | `~/.claude/projects/<hash>/memory/` | Claude 为每个项目自写的学习内容（个人专属，从不共享） |
| **凭据与 OAuth** | 系统钥匙串 + `~/.claude.json` | API 密钥、OAuth 令牌（绝不会放在项目文件中） |
| **按键绑定 (Keybindings)** | `~/.claude/keybindings.json` | 自定义键盘快捷键 |
| **MCP 用户服务器** | `~/.claude.json`（`mcpServers` 键） | 跨所有项目的个人 MCP 服务器 |
| **偏好设置/缓存** | `~/.claude.json` | 主题、模型、输出风格、会话状态 |

---

## 双作用域功能

以下功能同时存在于两个层级，**项目层级优先于全局层级**：

| 功能 | 全局（`~/.claude/`） | 项目（`.claude/`） | 优先级 |
|---------|----------------------|---------------------|------------|
| **CLAUDE.md** | `~/.claude/CLAUDE.md` | `./CLAUDE.md` 或 `.claude/CLAUDE.md` | 项目覆盖全局 |
| **设置 (Settings)** | `~/.claude/settings.json` | `.claude/settings.json` + `.claude/settings.local.json` | 项目 > 全局 |
| **规则 (Rules)** | `~/.claude/rules/*.md` | `.claude/rules/*.md` | 项目覆盖 |
| **Agents/Subagents** | `~/.claude/agents/*.md` | `.claude/agents/*.md` | 项目覆盖 |
| **命令 (Commands)** | `~/.claude/commands/*.md` | `.claude/commands/*.md` | 两者均可用 |
| **技能 (Skills)** | `~/.claude/skills/` | `.claude/skills/` | 两者均可用 |
| **钩子 (Hooks)** | `~/.claude/hooks/` | `.claude/hooks/` | 两者均执行 |
| **MCP 服务器** | `~/.claude.json`（用户作用域） | `.mcp.json`（项目作用域） | 三个作用域：local > project > user |

---

## 设置优先级

用户可写的设置按以下覆盖顺序应用（从高到低）：

| 优先级 | 位置 | 作用域 | 版本控制 | 用途 |
|----------|----------|-------|-----------------|---------|
| 1 | 命令行标志 | 会话 | 不适用 | 单会话覆盖 |
| 2 | `.claude/settings.local.json` | 项目 | 否（git-ignored） | 个人项目专属 |
| 3 | `.claude/settings.json` | 项目 | 是（已提交） | 团队共享设置 |
| 4 | `~/.claude/settings.local.json` | 用户 | 不适用 | 个人全局覆盖 |
| 5 | `~/.claude/settings.json` | 用户 | 不适用 | 全局个人设置 |

策略层：`managed-settings.json` 由组织强制实施，本地文件无法覆盖。

**重要**：`deny` 规则具有最高的安全优先级，低优先级的 allow/ask 规则无法覆盖。

---

## 目录结构对比

### 全局作用域（`~/.claude/`）

```
~/.claude/
├── settings.json              # 用户级设置（所有项目）
├── settings.local.json        # 个人覆盖
├── CLAUDE.md                  # 用户记忆（所有项目）
├── agents/                    # 用户 subagent（适用于所有项目）
│   └── *.md
├── rules/                     # 用户级模块化规则
│   └── *.md
├── commands/                  # 用户级命令
│   └── *.md
├── skills/                    # 用户级技能
│   └── */SKILL.md
├── tasks/                     # 全局专用：任务列表
│   └── {task-list-id}/
├── teams/                     # 全局专用：Agent 团队配置
│   └── {team-name}/
│       └── config.json
├── projects/                  # 全局专用：按项目的自动记忆
│   └── {project-hash}/
│       └── memory/
│           ├── MEMORY.md
│           └── *.md
├── keybindings.json           # 全局专用：键盘快捷键
└── hooks/                     # 用户级钩子
    ├── scripts/
    └── config/

~/.claude.json                 # 全局专用：MCP 服务器、OAuth、偏好设置、缓存
```

### 项目作用域（`.claude/`）

```
.claude/
├── settings.json              # 团队共享设置
├── settings.local.json        # 个人项目覆盖（git-ignored）
├── CLAUDE.md                  # 项目记忆（替代 ./CLAUDE.md）
├── agents/                    # 项目 subagent
│   └── *.md
├── rules/                     # 项目级模块化规则
│   └── *.md
├── commands/                  # 自定义斜杠命令
│   └── *.md
├── skills/                    # 自定义技能
│   └── {skill-name}/
│       ├── SKILL.md
│       └── supporting-files/
├── hooks/                     # 项目级钩子
│   ├── scripts/
│   └── config/
└── plugins/                   # 已安装的插件

.mcp.json                      # 项目作用域的 MCP 服务器（仓库根目录）
```

---

## 任务系统

于 **Claude Code v2.1.16**（2026 年 1 月 22 日）引入，取代了已废弃的 TodoWrite 系统。

### 存储

任务存储在本地文件系统的 `~/.claude/tasks/` 中（非云端数据库）。这使得任务状态可审计、可版本控制、可从崩溃中恢复。

### 工具

| 工具 | 用途 |
|------|---------|
| **TaskCreate** | 使用 `subject`、`description` 和 `activeForm` 创建新任务 |
| **TaskGet** | 按 ID 检索特定任务的完整详情 |
| **TaskUpdate** | 更改状态、设置所有者、添加依赖项或删除 |
| **TaskList** | 列出所有任务及其当前状态 |

### 任务生命周期

```
pending  →  in_progress  →  completed
```

### 依赖管理

任务可通过 `addBlockedBy`/`addBlocks` 阻塞其他任务，形成依赖关系图，防止过早执行。

### 多会话协作

```bash
CLAUDE_CODE_TASK_LIST_ID=my-project-tasks claude
```

所有共享同一 ID 的会话都能实时看到任务更新，从而支持并行工作流和会话恢复。

### 与旧版 Todos 的关键区别

| 功能 | 旧版 Todos | 新版 Tasks |
|---------|-----------|-----------|
| 作用域 | 单一会话 | 跨会话、跨 agent |
| 依赖关系 | 无 | 完整的依赖关系图 |
| 存储 | 仅内存 | 文件系统（`~/.claude/tasks/`） |
| 持久性 | 会话结束即丢失 | 重启和崩溃后仍保留 |
| 多会话 | 不支持 | 通过 `CLAUDE_CODE_TASK_LIST_ID` 支持 |

---

## Agent 团队

于 **2026 年 2 月 5 日**宣布为实验性功能。Agent 团队允许多个 Claude Code 会话协调处理共享工作。

### 启用方式

```json
// 在 ~/.claude/settings.json 中
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### 配置

团队配置位于 `~/.claude/teams/{team-name}/`，支持以下模式：

| 模式 | 描述 | 要求 |
|------|-------------|--------------|
| **进程内 (In-process)**（默认） | 所有团队成员在你的终端内运行 | 无 |
| **分屏 (Split panes)** | 每个团队成员拥有自己的窗格 | tmux 或 iTerm2（非 VS Code 终端） |

---

## 设计原则

全局专属与双作用域的划分遵循清晰的模式：

| 类别 | 作用域 | 理由 |
|----------|-------|-----------|
| **协调状态**（tasks、teams） | 全局专属 | 需要超越单个项目持久存在 |
| **安全状态**（凭据、OAuth） | 全局专属 | 防止意外提交到版本控制 |
| **个人学习**（自动记忆） | 全局专属 | 用户专属，不可团队共享 |
| **输入偏好**（按键绑定） | 全局专属 | 用户肌肉记忆，非项目专属 |
| **配置**（settings、rules、agents） | 双层级 | 团队需要共享项目专属行为 |
| **工作流定义**（commands、skills） | 双层级 | 可以是个人或团队共享 |

自动记忆（`~/.claude/projects/<hash>/memory/`）是一个值得注意的混合体：它*关于*特定项目，但存储*在全局*，因为它代表个人学习内容而非团队可共享的配置。

---

## 来源

- [Claude Code Settings Documentation](https://code.claude.com/docs/en/settings)
- [Orchestrate Teams of Claude Code Sessions](https://code.claude.com/docs/en/agent-teams)
- [What are Tasks in Claude Code - ClaudeLog](https://claudelog.com/faqs/what-are-tasks-in-claude-code/)
- [Claude Code Task Management - ClaudeFast](https://claudefa.st/blog/guide/development/task-management)
- [Claude Code Tasks Update - VentureBeat](https://venturebeat.com/orchestration/claude-codes-tasks-update-lets-agents-work-longer-and-coordinate-across)
- [Where Are Claude Code Global Settings - ClaudeLog](https://claudelog.com/faqs/where-are-claude-code-global-settings/)
- [Claude Opus 4.6 Agent Teams - VentureBeat](https://venturebeat.com/technology/anthropics-claude-opus-4-6-brings-1m-token-context-and-agent-teams-to-take)
- [How to Set Up Claude Code Agent Teams (Full Walkthrough) - r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1qz8tyy/how_to_set_up_claude_code_agent_teams_full/)
- [Anthropic replaced Claude Code's old 'Todos' with Tasks - r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1qkjznp/anthropic_replaced_claude_codes_old_todos_with/)
