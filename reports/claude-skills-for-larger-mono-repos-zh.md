# 理解大型 Monorepo 中的 Claude Skills 发现机制

在使用 Claude Code 处理 monorepo 时，理解 skills 如何被发现并加载到上下文中对于有效组织项目特定能力至关重要。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 与 CLAUDE.md 的重要区别

**Skills 的加载行为与 CLAUDE.md 文件不同。** CLAUDE.md 文件会沿目录树向上遍历（祖先加载），而 skills 使用不同的发现机制，专注于项目内的嵌套目录。

## Skills 的发现机制

### 1. 标准 Skill 位置

Skills 根据作用域从以下固定位置加载：

| 位置 | 路径 | 适用范围 |
|----------|------|------------|
| Enterprise（企业） | Managed settings（托管设置） | 组织内所有用户 |
| Personal（个人） | `~/.claude/skills/<skill-name>/SKILL.md` | 你的所有项目 |
| Project（项目） | `.claude/skills/<skill-name>/SKILL.md` | 仅限当前项目 |
| Plugin（插件） | `<plugin>/skills/<skill-name>/SKILL.md` | 启用的插件范围内 |

### 2. 从嵌套目录自动发现

当你处理子目录中的文件时，Claude Code 会自动从嵌套的 `.claude/skills/` 目录中发现 skills。例如，如果你正在编辑 `packages/frontend/` 中的文件，Claude Code 也会在 `packages/frontend/.claude/skills/` 中查找 skills。

这支持了各 package 拥有自己 skills 的 monorepo 设置。

## 示例 Monorepo 结构

考虑一个包含多个独立 package 的典型 monorepo：

```
/mymonorepo/
├── .claude/
│   └── skills/
│       └── shared-conventions/SKILL.md    # 项目级 skill
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── react-patterns/SKILL.md  # 前端特定 skill
│   │   └── src/
│   │       └── App.tsx
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── api-design/SKILL.md      # 后端特定 skill
│   │   └── src/
│   └── shared/
│       ├── .claude/
│       │   └── skills/
│       │       └── utils-patterns/SKILL.md  # 共享工具 skill
│       └── src/
```

## 场景 1：在根目录刚启动 Claude（尚未编辑任何文件）

当你从 `/mymonorepo/` 运行 Claude Code 且尚未编辑任何文件时：

```bash
cd /mymonorepo
claude
# 刚启动 - 尚未编辑任何文件
```

| Skill | 在上下文中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 根目录 `.claude/skills/` 中的项目级 skill |
| `react-patterns` | **否** | 未发现 — 尚未处理 `packages/frontend/` 中的文件 |
| `api-design` | **否** | 未发现 — 尚未处理 `packages/backend/` 中的文件 |
| `utils-patterns` | **否** | 未发现 — 尚未处理 `packages/shared/` 中的文件 |

## 场景 2：编辑 Package 中的文件后

在你让 Claude 编辑 `packages/frontend/src/App.tsx` 之后：

| Skill | 在上下文中？ | 原因 |
|-------|-------------|--------|
| `shared-conventions` | **是** | 根目录 `.claude/skills/` 中的项目级 skill |
| `react-patterns` | **是** | 编辑 `packages/frontend/` 中的文件时被发现 |
| `api-design` | **否** | 仍未发现 — 尚未处理 `packages/backend/` 中的文件 |
| `utils-patterns` | **否** | 仍未发现 — 尚未处理 `packages/shared/` 中的文件 |

**关键洞察**：嵌套 skills 是**按需发现**的——当你处理这些目录中的文件时才会被发现。它们不会在会话启动时预加载。

## 关键行为：描述 vs 完整内容

Skill 的描述会被加载到上下文中，以便 Claude 知道哪些可用，但**完整的 skill 内容仅在调用时加载**。这是一项重要的优化：

- **描述**：始终在上下文中（在字符预算范围内）
- **完整内容**：在调用 skill 时按需加载

> 注意：预加载了 skills 的 subagents 行为不同——完整 skill 内容会在启动时注入。

## 优先级顺序（当 Skills 名称相同时）

当不同层级的 skills 名称相同时，优先级更高的位置获胜：

| 优先级 | 位置 | 作用域 |
|----------|----------|-------|
| 1（最高） | Enterprise（企业） | 组织范围 |
| 2 | Personal（个人）`~/.claude/skills/` | 你的所有项目 |
| 3（最低） | Project（项目）`.claude/skills/` | 仅限当前项目 |

Plugin skills 使用 `plugin-name:skill-name` 命名空间，因此不会与其他层级冲突。

## 这种设计为何适合 Monorepo

- **Package 特定 skills 保持隔离** — 在 `packages/frontend/` 中工作的前端开发者获得前端特定 skills，而不会让后端 skills 杂乱地占用上下文。

- **自动发现减少配置** — 无需显式注册 package 级别的 skills；当你在这些目录中工作时，它们会被自动发现。

- **上下文得到优化** — 仅初始加载 skill 描述，嵌套 skills 按需发现。

- **团队可以维护自己的 skills** — 每个 package 团队可以定义其领域特定的 skills，而无需与其他团队协调。

## 字符预算考量

Skill 描述会在字符预算（默认 15,000 字符）内加载到上下文中。在包含大量 package 和 skills 的大型 monorepo 中，你可能会达到此限制。

- 运行 `/context` 检查关于被排除 skills 的警告
- 设置 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量以增加限制

## 最佳实践

1. **将共享 workflow 放在根目录 `.claude/skills/` 中** — 仓库范围的约定、提交 workflow 和共享模式。

2. **将 package 特定 skills 放在 package 的 `.claude/skills/` 中** — 该 package 特有的框架特定模式、组件约定、测试工具。

3. **对危险 skills 使用 `disable-model-invocation: true`** — 部署或破坏性 skills 应要求用户显式调用。

4. **保持 skill 描述简洁** — 描述始终在上下文中（在字符预算范围内），因此冗长的描述会浪费上下文空间。

5. **在 skill 名称中使用命名空间** — 考虑使用 package 名称作为前缀（例如 `frontend-review`、`backend-deploy`）以避免混淆。

## 对比：Skills 与 CLAUDE.md 加载

| 行为 | CLAUDE.md | Skills |
|----------|-----------|--------|
| 祖先加载（沿目录树向上） | 是 | 否 |
| 嵌套/子目录发现（沿目录树向下） | 是（惰性） | 是（自动发现） |
| 全局位置 | `~/.claude/CLAUDE.md` | `~/.claude/skills/` |
| 项目位置 | `.claude/` 或仓库根目录 | `.claude/skills/` |
| 内容加载 | 完整内容 | 仅描述（调用时加载完整内容） |

---

## 来源

- [Claude Code Documentation - Extend Claude with Skills](https://code.claude.com/docs/en/skills)
- [Claude Code Documentation - Automatic Discovery from Nested Directories](https://code.claude.com/docs/en/skills#automatic-discovery-from-nested-directories)
