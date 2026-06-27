# 理解大型 Monorepo 中的 Claude Skills 发现机制

在 monorepo 中使用 Claude Code 时，理解 skills 如何被发现并加载到 context 中，对于有效组织项目特定能力至关重要。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 与 CLAUDE.md 的重要区别

**Skills 的加载行为与 CLAUDE.md 文件不同。** CLAUDE.md 文件沿目录树向上遍历（祖先加载），而 skills 使用不同的发现机制，专注于项目内的嵌套目录。

## Skills 如何被发现

### 1. 标准 Skill 位置

Skills 从以下固定位置加载，基于 scope：

| Location | Path | Applies to |
|----------|------|------------|
| Enterprise | Managed settings | 组织中的所有用户 |
| Personal | `~/.claude/skills/<skill-name>/SKILL.md` | 你的所有项目 |
| Project | `.claude/skills/<skill-name>/SKILL.md` | 仅此项目 |
| Plugin | `<plugin>/skills/<skill-name>/SKILL.md` | 启用 plugin 的位置 |

### 2. 从嵌套目录自动发现

当你在子目录中处理文件时，Claude Code 会自动从嵌套的 `.claude/skills/` 目录中发现 skills。例如，如果你正在编辑 `packages/frontend/` 中的文件，Claude Code 也会在 `packages/frontend/.claude/skills/` 中查找 skills。

这支持了各 package 拥有自身 skills 的 monorepo 设置。

## Monorepo 结构示例

考虑一个包含多个 package 的典型 monorepo：

```
/mymonorepo/
├── .claude/
│   └── skills/
│       └── shared-conventions/SKILL.md    # 项目级 skill
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── react-patterns/SKILL.md  # 前端专用 skill
│   │   └── src/
│   │       └── App.tsx
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── api-design/SKILL.md      # 后端专用 skill
│   │   └── src/
│   └── shared/
│       ├── .claude/
│       │   └── skills/
│       │       └── utils-patterns/SKILL.md  # 共享工具 skill
│       └── src/
```

## 场景 1：刚在根目录启动 Claude（尚未编辑任何文件）

当你在 `/mymonorepo/` 运行 Claude Code 且尚未编辑任何文件：

```bash
cd /mymonorepo
claude
# 刚启动 - 尚未编辑任何文件
```

| Skill | 在 Context 中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 根目录 `.claude/skills/` 中的项目级 skill |
| `react-patterns` | **否** | 未发现 - 尚未处理 `packages/frontend/` 中的文件 |
| `api-design` | **否** | 未发现 - 尚未处理 `packages/backend/` 中的文件 |
| `utils-patterns` | **否** | 未发现 - 尚未处理 `packages/shared/` 中的文件 |

## 场景 2：编辑某个 Package 中的文件后

在你要求 Claude 编辑 `packages/frontend/src/App.tsx` 之后：

| Skill | 在 Context 中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 根目录 `.claude/skills/` 中的项目级 skill |
| `react-patterns` | **是** | 编辑 `packages/frontend/` 中的文件时被发现 |
| `api-design` | **否** | 仍未发现 - 尚未处理 `packages/backend/` 中的文件 |
| `utils-patterns` | **否** | 仍未发现 - 尚未处理 `packages/shared/` 中的文件 |

**关键洞察**：嵌套 skills 是**按需发现**的——当你在这些目录中处理文件时才被发现，而不是在 session 启动时预加载。

## 关键行为：Description vs 完整内容

Skill 的 description 被加载到 context 中，这样 Claude 知道有哪些可用的 skills，但**完整 skill 内容仅在调用时加载**。这是一项重要的优化：

- **Description**：始终在 context 中（受字符预算限制）
- **完整内容**：按需加载，在 skill 被调用时

> 注意：带有预加载 skills 的 subagent 工作方式不同——完整 skill 内容在启动时注入。

## 优先级顺序（当 Skills 同名时）

当 skills 在不同层级上共享相同名称时，更高优先级的位置胜出：

| Priority | Location | Scope |
|----------|----------|-------|
| 1（最高） | Enterprise | 组织级 |
| 2 | Personal（`~/.claude/skills/`） | 你的所有项目 |
| 3（最低） | Project（`.claude/skills/`） | 仅此项目 |

Plugin skills 使用 `plugin-name:skill-name` 命名空间，因此不会与其他层级冲突。

## 为什么这种设计适合 Monorepo

- **Package 专用 skills 保持隔离** — 在 `packages/frontend/` 中工作的前端开发者获得前端专用 skills，而后端 skills 不会占用 context。

- **自动发现减少配置** — 无需显式注册 package 级别的 skills，当你在那些目录中工作时它们会自动被发现。

- **Context 得到优化** — 最初只加载 skill description，嵌套 skills 按需发现。

- **团队可维护自己的 skills** — 每个 package 团队可以定义其领域专用的 skills，无需与其他团队协调。

## 字符预算考量

Skill description 加载到 context 中最多占用一个字符预算（默认 15,000 字符）。在拥有许多 package 和 skills 的大型 monorepo 中，可能会达到此限制。

- 运行 `/context` 检查是否有关于被排除 skills 的警告
- 设置 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量来提高限制

## 最佳实践

1. **将共享工作流放在根 `.claude/skills/` 中** — 仓库级约定、commit 工作流和共享模式。

2. **将 package 专用 skills 放在 package 的 `.claude/skills/` 中** — 框架特定模式、组件约定、该 package 特有的测试工具。

3. **对危险 skills 使用 `disable-model-invocation: true`** — 部署或破坏性 skills 应要求用户显式调用。

4. **保持 skill description 简洁** — Description 始终在 context 中（直到字符预算），冗长的 description 浪费 context 空间。

5. **在 skill 名称中使用命名空间** — 考虑用 package 名称作为前缀（如 `frontend-review`、`backend-deploy`），以避免混淆。

## 对比：Skills vs CLAUDE.md 加载

| Behavior | CLAUDE.md | Skills |
|----------|-----------|--------|
| 祖先加载（沿目录树向上） | 是 | 否 |
| 嵌套/后代发现（沿目录树向下） | 是（lazy） | 是（自动发现） |
| 全局位置 | `~/.claude/CLAUDE.md` | `~/.claude/skills/` |
| 项目位置 | `.claude/` 或仓库根目录 | `.claude/skills/` |
| 内容加载 | 完整内容 | 仅 description（调用时加载完整内容） |

---

## Sources

- [Claude Code Documentation - Extend Claude with Skills](https://code.claude.com/docs/en/skills)
- [Claude Code Documentation - Automatic Discovery from Nested Directories](https://code.claude.com/docs/en/skills#automatic-discovery-from-nested-directories)
