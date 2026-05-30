# Claude Memory（Claude 记忆）

通过 CLAUDE.md 文件实现持久化上下文 — 如何编写以及如何在单体仓库中加载。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1. 编写一份好的 CLAUDE.md

一份结构良好的 CLAUDE.md 是改善 Claude Code 在您项目中输出质量的最有效方式。Humanlayer 有一份优秀的指南，涵盖了应包含的内容、如何组织结构以及常见陷阱。

- [Humanlayer - 编写一份好的 Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)

---

## 2. 大型单体仓库中的 CLAUDE.md

在使用 Claude Code 处理单体仓库时，理解 CLAUDE.md 文件如何加载到上下文中，对于有效组织项目指令至关重要。

<p align="center">
  <a href="https://x.com/bcherny/status/2016339448863355206"><img src="assets/claude-memory/claude-memory-monorepo.jpg" alt="单体仓库中的 CLAUDE.md 加载" width="600"></a>
</p>

### 两种加载机制

Claude Code 使用两种不同的机制来加载 CLAUDE.md 文件：

#### 祖先加载（向目录树上方）

当您启动 Claude Code 时，它会从当前工作目录**向上**向文件系统根目录遍历，沿途加载找到的每一个 CLAUDE.md。这些文件在**启动时立即加载**。

#### 后代加载（向目录树下方）

位于当前工作目录下级子目录中的 CLAUDE.md 文件在**启动时不会加载**。它们仅在 Claude 在会话期间读取这些子目录中的文件时才会被包含。这被称为**懒加载**。

### 示例单体仓库结构

考虑一个典型的单体仓库，其中包含不同组件的独立目录：

```
/mymonorepo/
├── CLAUDE.md          # 根级别指令（所有组件共享）
├── frontend/
│   └── CLAUDE.md      # 前端专用指令
├── backend/
│   └── CLAUDE.md      # 后端专用指令
└── api/
    └── CLAUDE.md      # API 专用指令
```

### 场景 1：从根目录运行 Claude Code

当您从 `/mymonorepo/` 运行 Claude Code 时：

```bash
cd /mymonorepo
claude
```

| 文件 | 启动时加载？ | 原因 |
|------|-------------------|--------|
| `/mymonorepo/CLAUDE.md` | 是 | 这是您当前的工作目录 |
| `/mymonorepo/frontend/CLAUDE.md` | 否 | 仅在您读取/编辑 `frontend/` 中的文件时加载 |
| `/mymonorepo/backend/CLAUDE.md` | 否 | 仅在您读取/编辑 `backend/` 中的文件时加载 |
| `/mymonorepo/api/CLAUDE.md` | 否 | 仅在您读取/编辑 `api/` 中的文件时加载 |

### 场景 2：从组件目录运行 Claude Code

当您从 `/mymonorepo/frontend/` 运行 Claude Code 时：

```bash
cd /mymonorepo/frontend
claude
```

| 文件 | 启动时加载？ | 原因 |
|------|-------------------|--------|
| `/mymonorepo/CLAUDE.md` | 是 | 这是祖先目录 |
| `/mymonorepo/frontend/CLAUDE.md` | 是 | 这是您当前的工作目录 |
| `/mymonorepo/backend/CLAUDE.md` | 否 | 目录树的不同分支 |
| `/mymonorepo/api/CLAUDE.md` | 否 | 目录树的不同分支 |

### 关键要点

1. **祖先始终在启动时加载** — Claude 向目录树**上方**遍历，加载找到的所有 CLAUDE.md 文件。这确保您始终可以访问根级别、仓库范围的指令。

2. **后代懒加载** — 子目录 CLAUDE.md 文件仅在您与这些子目录中的文件交互时加载。这防止了不相关的上下文使您的会话变得臃肿。

3. **同级目录永不加载** — 如果您在 `frontend/` 中工作，`backend/CLAUDE.md` 或 `api/CLAUDE.md` 不会被加载到上下文中。

4. **全局 CLAUDE.md** — 您还可以在主目录的 `~/.claude/CLAUDE.md` 放置一个 CLAUDE.md，它将适用于所有 Claude Code 会话，无论项目如何。

### 为什么这种设计适合单体仓库

- **共享指令向下传播** — 根级别的 CLAUDE.md 包含适用于任何地方的仓库级约定、编码标准和通用模式。

- **组件专用指令保持隔离** — 前端开发者不需要后端专用指令来混乱他们的上下文，反之亦然。

- **上下文得到优化** — 通过懒加载后代 CLAUDE.md 文件，Claude Code 避免了在启动时加载可能数百 KB 的不相关指令。

### 最佳实践

1. **将共享约定放在根 CLAUDE.md 中** — 编码标准、提交信息格式、PR 模板以及其他仓库级指南。

2. **将组件专用指令放在组件的 CLAUDE.md 中** — 特定框架的模式、组件架构、该组件特有的测试约定。

3. **使用 CLAUDE.local.md 存放个人偏好** — 将其添加到 `.gitignore` 中，用于不应与团队共享的指令。

---

## 来源

- [Claude Code 文档 - Claude 如何查找记忆](https://code.claude.com/docs/en/memory#how-claude-looks-up-memories)
- [Boris Cherny 在 X 上的推文 - 关于 CLAUDE.md 加载的说明](https://x.com/bcherny/status/2016339448863355206)
- [Humanlayer - 编写一份好的 Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
