# Claude Code：Agent Memory Frontmatter

为 subagent 提供持久化记忆——使 agent 能够跨会话学习、记忆和积累知识。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

**Claude Code v2.1.33**（2026 年 2 月）引入了 `memory` frontmatter 字段，为每个 subagent 提供独立的持久化 markdown 知识存储。在此之前，每次 agent 调用都从零开始。

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

## 记忆作用域

| 作用域 | 存储位置 | 版本控制 | 共享 | 适用场景 |
|-------|---------|---------|------|---------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | 否 | 否 | 跨项目知识（推荐默认值） |
| `project` | `.claude/agent-memory/<agent-name>/` | 是 | 是 | 团队共享的项目特定知识 |
| `local` | `.claude/agent-memory-local/<agent-name>/` | 否（已 git-ignore） | 否 | 个人专属的项目特定知识 |

这些作用域与设置层级（`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`）相对应。

---

## 工作原理

1. **启动时**：`MEMORY.md` 的前 200 行被注入到 agent 的系统提示中
2. **工具访问**：自动启用 `Read`、`Write`、`Edit`，使 agent 能够管理其记忆
3. **执行期间**：agent 可自由读写其记忆目录
4. **整理优化**：如果 `MEMORY.md` 超过 200 行，agent 会将细节移至主题特定文件中

```
~/.claude/agent-memory/code-reviewer/     # user 作用域示例
├── MEMORY.md                              # 主文件（加载前 200 行）
├── react-patterns.md                      # 主题特定文件
└── security-checklist.md                  # 主题特定文件
```

---

## Agent 记忆与其他记忆系统

| 系统 | 写入者 | 读取者 | 作用域 |
|------|-------|-------|--------|
| **CLAUDE.md** | 你（手动） | 主 Claude + 所有 agent | 项目 |
| **Auto-memory** | 主 Claude（自动） | 仅主 Claude | 按项目按用户 |
| **`/memory` 命令** | 你（通过编辑器） | 仅主 Claude | 按项目按用户 |
| **Agent memory** | Agent 自身 | 仅该特定 agent | 可配置（user/project/local） |

这些系统是**互补的**——agent 同时读取 CLAUDE.md（项目上下文）和自身记忆（agent 特定知识）。

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

这结合了 **skills**（启动时的静态知识）与 **memory**（随时间积累的动态知识）。

---

## 使用技巧

- **提示记忆使用**——包含明确的指令：`"Before starting, review your memory. After completing, update your memory with what you learned."`
- **请求记忆检查**——在调用 agent 时：`"Review this PR, and check your memory for patterns you've seen before."`
- **选择正确的作用域**——`user` 用于跨项目，`project` 用于团队共享，`local` 用于个人

---

## 来源

- [创建自定义 subagent — Claude Code 文档](https://code.claude.com/docs/en/sub-agents)
- [管理 Claude 的记忆 — Claude Code 文档](https://code.claude.com/docs/en/memory)
- [Claude Code v2.1.33 发布说明](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
