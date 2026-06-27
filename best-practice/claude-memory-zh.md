# Claude Memory

通过 CLAUDE.md 文件实现持久化 context —— 如何编写它们以及它们在 monorepo 中如何加载。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1. 编写良好的 CLAUDE.md

结构良好的 CLAUDE.md 是提高 Claude Code 对您项目输出质量的最有效方式。Humanlayer 有一份优秀的指南，涵盖应包含的内容、如何组织以及常见陷阱。

- [Humanlayer - Writing a good Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)

---

## 2. 大型 Monorepo 中的 CLAUDE.md

在 monorepo 中使用 Claude Code 时，了解 CLAUDE.md 文件如何加载到 context 中对于有效组织项目指令至关重要。

<p align="center">
  <a href="https://x.com/bcherny/status/2016339448863355206"><img src="assets/claude-memory/claude-memory-monorepo.jpg" alt="CLAUDE.md 在 monorepo 中的加载" width="600"></a>
</p>

### 两种加载机制

Claude Code 使用两种不同的机制来加载 CLAUDE.md 文件：

#### 向上加载（沿目录树向上）

当您启动 Claude Code 时，它会从当前工作目录**向上**遍历到文件系统根目录，并加载沿途找到的每个 CLAUDE.md。这些文件在**启动时立即加载**。

#### 向下加载（沿目录树向下）

当前工作目录下的子目录中的 CLAUDE.md 文件**不会在启动时加载**。它们仅在 session 期间 Claude 读取这些子目录中的文件时才被包含。这被称为**lazy loading**。

### Monorepo 结构示例

考虑一个典型的 monorepo，其中不同组件有各自的目录：

```
/mymonorepo/
├── CLAUDE.md          # 根级指令（跨所有组件共享）
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

| File | 启动时加载？ | 原因 |
|------|-------------------|--------|
| `/mymonorepo/CLAUDE.md` | Yes | 它是当前工作目录 |
| `/mymonorepo/frontend/CLAUDE.md` | No | 仅在您读取/编辑 `frontend/` 中的文件时加载 |
| `/mymonorepo/backend/CLAUDE.md` | No | 仅在您读取/编辑 `backend/` 中的文件时加载 |
| `/mymonorepo/api/CLAUDE.md` | No | 仅在您读取/编辑 `api/` 中的文件时加载 |

### 场景 2：从组件目录运行 Claude Code

当您从 `/mymonorepo/frontend/` 运行 Claude Code 时：

```bash
cd /mymonorepo/frontend
claude
```

| File | 启动时加载？ | 原因 |
|------|-------------------|--------|
| `/mymonorepo/CLAUDE.md` | Yes | 它是祖先目录 |
| `/mymonorepo/frontend/CLAUDE.md` | Yes | 它是当前工作目录 |
| `/mymonorepo/backend/CLAUDE.md` | No | 目录树的不同分支 |
| `/mymonorepo/api/CLAUDE.md` | No | 目录树的不同分支 |

### 关键要点

1. **祖先始终在启动时加载** — Claude 沿目录树向上遍历并加载找到的所有 CLAUDE.md 文件。这确保您始终可以访问根级的、repository 范围的指令。

2. **后代延迟加载** — 子目录中的 CLAUDE.md 文件仅在您与这些子目录中的文件交互时才加载。这防止无关的 context 膨胀您的 session。

3. **同级永不加载** — 如果您在 `frontend/` 中工作，您不会在 context 中获得 `backend/CLAUDE.md` 或 `api/CLAUDE.md`。

4. **全局 CLAUDE.md** — 您也可以在主文件夹的 `~/.claude/CLAUDE.md` 中放置一个 CLAUDE.md，它适用于**所有** Claude Code session，无论项目如何。

### 此设计为何适用于 Monorepo

- **共享指令向下传播** — 根级 CLAUDE.md 包含到处适用的 repository 级约定、编码标准和通用模式。

- **组件特定指令保持隔离** — 前端开发者不需要后端专用指令充斥其 context，反之亦然。

- **Context 已优化** — 通过延迟加载后代 CLAUDE.md 文件，Claude Code 避免在启动时加载可能数百 KB 的无关指令。

### 最佳实践

1. **将共享约定放在根 CLAUDE.md 中** — 编码标准、commit message 格式、PR 模板和其他 repository 级指南。

2. **将组件特定指令放在组件 CLAUDE.md 中** — Framework 特定模式、组件架构、该组件独有的 testing 约定。

3. **使用 CLAUDE.local.md 存放个人偏好** — 将其添加到 `.gitignore`，用于不应与团队共享的指令。

---

## Sources

- [Claude Code Documentation - How Claude Looks Up Memories](https://code.claude.com/docs/en/memory#how-claude-looks-up-memories)
- [Boris Cherny on X - Clarification on CLAUDE.md Loading](https://x.com/bcherny/status/2016339448863355206)
- [Humanlayer - Writing a good Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
