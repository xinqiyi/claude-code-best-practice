# Claude Code: Agent Memory Frontmatter

为 subagents 提供持久 memory — 让 agents 能够跨会话学习、记忆和积累知识。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Overview

在 **Claude Code v2.1.33**（2026 年 2 月）中引入，`memory` frontmatter 字段为每个 subagent 提供了其自己的持久化 markdown 知识存储。在此之前，每次 agent 调用都从零开始。

```yaml
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

You are a code reviewer. As you review code, update your agent memory with
patterns, conventions, and recurring issues you discover.
```

---

## Memory Scopes

| Scope | 存储位置 | 版本控制 | 共享 | 最适合 |
|-------|-----------------|-------------------|--------|----------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | No | No | 跨项目知识（推荐的默认值） |
| `project` | `.claude/agent-memory/<agent-name>/` | Yes | Yes | 团队应共享的项目特定知识 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | No（git-ignored） | No | 个人的项目特定知识 |

这些 scope 映射了 settings 层次结构（`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`）。

---

## 工作原理

1. **启动时**：`MEMORY.md` 的前 200 行被注入到 agent 的 system prompt 中
2. **工具访问**：`Read`、`Write`、`Edit` 自动启用，让 agent 可以管理其 memory
3. **执行期间**：agent 自由读取/写入其 memory 目录
4. **管理**：如果 `MEMORY.md` 超过 200 行，agent 将详细信息移入主题特定文件中

```
~/.claude/agent-memory/code-reviewer/     # user scope 示例
├── MEMORY.md                              # 主文件（加载前 200 行）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

---

## Agent Memory 与其他 Memory 系统

| System | 谁写入 | 谁读取 | Scope |
|--------|-----------|-----------|-------|
| **CLAUDE.md** | 你（手动） | 主 Claude + 所有 agents | Project |
| **Auto-memory** | 主 Claude（自动） | 仅主 Claude | 每个项目每个用户 |
| **`/memory` command** | 你（通过编辑器） | 仅主 Claude | 每个项目每个用户 |
| **Agent memory** | Agent 自己 | 仅该特定 agent | 可配置（user/project/local） |

这些系统是**互补的** — agent 既读取 CLAUDE.md（项目 context）也读取自己的 memory（agent 特定知识）。

---

## 实际示例

```yaml
---
name: api-developer
description: Implement API endpoints following team conventions
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---

Implement API endpoints. Follow the conventions from your preloaded skills.
As you work, save architectural decisions and patterns to your memory.
```

这结合了 **skills**（启动时的静态知识）和 **memory**（随时间积累的动态知识）。

---

## 提示

- **提示 memory 使用** — 包含明确的指令：`"Before starting, review your memory. After completing, update your memory with what you learned."`
- **调用 agents 时请求 memory 检查**：`"Review this PR, and check your memory for patterns you've seen before."`
- **选择正确的 scope** — 跨项目使用 `user`，团队共享使用 `project`，个人使用 `local`

---

## Sources

- [Create custom subagents — Claude Code Docs](https://code.claude.com/docs/en/sub-agents)
- [Manage Claude's memory — Claude Code Docs](https://code.claude.com/docs/en/memory)
- [Claude Code v2.1.33 Release Notes](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
