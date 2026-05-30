# 子代理报告 — 变更日志历史

## 状态图例

| Status | 含义 |
|--------|------|
| ✅ `COMPLETE (reason)` | 已采取行动并成功解决 |
| ❌ `INVALID (reason)` | 发现项不正确、不适用或有意为之 |
| ✋ `ON HOLD (reason)` | 行动推迟 — 等待外部依赖或用户决定 |

---

## [2026-02-28 03:22 PM PKT] Claude Code v2.1.63

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 代理表格 | 将 `workflow-changelog-claude-agents-frontmatter-agent` 添加到"本仓库中的代理"表格 | ✅ COMPLETE（已添加，model：opus，继承所有 tools，无 skills/memory） |
| 2 | HIGH | 代理表格 | 修复 presentation-curator 的 skills 列 — 为技能名称添加 `presentation/` 前缀 | ✅ COMPLETE（已更新为 presentation/vibe-to-agentic-framework 等） |
| 3 | MED | 字段文档 | 在 `color` 字段添加说明，指出该字段可用但不在官方 frontmatter 表格中 | ✅ COMPLETE（已在 description 列添加关于非官方状态的说明） |
| 4 | MED | 调用部分 | 扩展调用部分，增加 --agents CLI 标志、/agents 命令、claude agents CLI、代理恢复 | ✅ COMPLETE（已添加包含 5 种方法的调用方式表格） |

---

## [2026-03-07 08:35 AM PKT] Claude Code v2.1.71

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 损坏链接 | 修复指向 `reports/claude-agent-memory.md` 的代理 memory 链接 | ✅ COMPLETE |
| 2 | HIGH | 行为变更 | 更新 `tools` 字段描述：`Task(agent_type)` → `Agent(agent_type)`（v2.1.63 重命名） | ✅ COMPLETE |
| 3 | HIGH | 行为变更 | 更新调用部分：Task tool → Agent tool（v2.1.63 重命名） | ✅ COMPLETE（已更新标题、代码示例并添加重命名说明） |
| 4 | HIGH | 示例更新 | 更新完整示例：`Task(monitor, rollback)` → `Agent(monitor, rollback)` | ✅ COMPLETE |
| 5 | HIGH | 内置代理 | 将 `Bash` 代理添加到官方 Claude 代理表格（model：inherit，用途：在独立上下文中执行终端命令） | ✅ COMPLETE（已添加到表格） |
| 6 | HIGH | 代理表格 | 将 `workflow-concepts-agent` 添加到"本仓库中的代理"表格（model：opus，color：green） | ✅ COMPLETE |
| 7 | HIGH | 代理表格 | 将 `workflow-claude-settings-agent` 添加到"本仓库中的代理"表格（model：opus，color：yellow） | ✅ COMPLETE |
| 8 | MED | 内置代理 | 修复 `statusline-setup` 的 model：`inherit` → `Sonnet` | ✅ COMPLETE |
| 9 | MED | 内置代理 | 修复 `claude-code-guide` 的 model：`inherit` → `Haiku` | ❌ NOT APPLICABLE（已从表格中移除） |
| 10 | MED | 代理表格 | 修复 `weather-agent` 的 color：`teal` → `green` | ✅ COMPLETE |
| 11 | MED | 调用 | 将 `--agent <name>` CLI 标志添加到调用方式表格 | ✅ COMPLETE（已添加为调用方式表格的第一行） |
| 12 | MED | 行为变更 | 更新第 147 行文本：官方 Claude 代理表格标题中的 "Task tool" → "Agent tool" | ✅ COMPLETE（用户已重写标题文本） |
| 13 | MED | 跨文件 | 更新 CLAUDE.md：将 `Task(...)` 引用替换为 `Agent(...)`（第 50-53、61 行） | ✅ COMPLETE（已更新编排部分和 tools 字段描述） |

---

## [2026-03-12 12:17 PM PKT] Claude Code v2.1.74

未检测到偏差 — 报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-13 04:21 PM PKT] Claude Code v2.1.74

未检测到偏差 — 报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-15 12:50 PM PKT] Claude Code v2.1.76

未检测到偏差 — 报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-17 12:42 PM PKT] Claude Code v2.1.77

未检测到偏差 — 报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-18 11:41 PM PKT] Claude Code v2.1.78

未检测到偏差 — 报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-19 11:56 AM PKT] Claude Code v2.1.79

未检测到偏差 — 报告与官方文档完全同步。所有 13 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-20 08:35 AM PKT] Claude Code v2.1.80

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新增字段 | 将 `effort` 字段添加到 Frontmatter Fields 表格（string，可选 — 努力级别覆盖：`low`、`medium`、`high`、`max`） | ✅ COMPLETE（已添加在 `background` 和 `isolation` 之间，计数更新 14→15） |

---

## [2026-03-21 09:07 PM PKT] Claude Code v2.1.81

未检测到偏差 — 报告与官方文档完全同步。所有 15 个 frontmatter 字段和 6 个内置代理均匹配。

---

## [2026-03-23 09:49 PM PKT] Claude Code v2.1.81

未检测到偏差 — 报告与官方文档完全同步。所有 15 个 frontmatter 字段（14 个官方 + 1 个非官方 `color`）和 6 个内置代理均匹配。

---

## [2026-03-25 08:07 PM PKT] Claude Code v2.1.83

未检测到偏差 — 报告与官方文档完全同步。所有 15 个 frontmatter 字段（14 个官方 + 1 个非官方 `color`）和 6 个内置代理均匹配。

**关注项：** `initialPrompt` 已在 v2.1.83 变更日志中添加，但尚未出现在官方文档支持的 frontmatter 字段表格中。当它出现时，报告将需要更新。

---

## [2026-03-26 01:01 PM PKT] Claude Code v2.1.84

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新增字段 | 将 `initialPrompt` 添加到 Frontmatter Fields 表格（string，可选 — 当代理通过 `--agent` 或 `agent` 设置作为主会话代理运行时，自动作为用户第一轮输入提交） | ✅ COMPLETE（已添加在 `isolation` 和 `color` 之间，计数更新 15→16） |

---

## [2026-03-27 06:28 PM PKT] Claude Code v2.1.85

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段（15 个官方 + 1 个非官方 `color`）和 6 个内置代理均匹配。

---

## [2026-03-28 06:00 PM PKT] Claude Code v2.1.86

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段（15 个官方 + 1 个非官方 `color`）和 6 个内置代理均匹配。

---

## [2026-04-01 12:26 PM PKT] Claude Code v2.1.89

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段（15 个官方 + 1 个非官方 `color`）和 6 个内置代理均匹配。

---

## [2026-04-02 09:11 PM PKT] Claude Code v2.1.90

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 移除代理 | 从官方 Claude 代理表格中移除 `Bash` — 官方文档列出 5 个内置代理，`Bash` 不在其中 | ✅ COMPLETE（已移除 Bash 行，重新编号 6→5 个代理） |
| 2 | LOW | 字段文档 | 更新 `color` 字段描述 — 移除"不在官方 frontmatter 表格中"的注记；`color` 现在出现在官方支持的 frontmatter 字段表格中 | ✅ COMPLETE（已从 color 字段描述中移除非官方注记） |

---

## [2026-04-03 08:30 PM PKT] Claude Code v2.1.91

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | LOW | 字段文档 | 更新 `permissionMode` 字段描述 — 添加 `auto` 作为有效值（官方文档现在列出：`default`、`acceptEdits`、`auto`、`dontAsk`、`bypassPermissions`、`plan`） | ✅ COMPLETE（在 permissionMode 描述的 `acceptEdits` 和 `dontAsk` 之间添加了 `auto`） |

---

## [2026-04-04 10:43 PM PKT] Claude Code v2.1.92

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-08 09:34 PM PKT] Claude Code v2.1.96

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | LOW | 字段文档 | 更新 `model` 字段描述 — 添加完整模型 ID 支持（例如 `claude-opus-4-6`）以及别名 | ✅ COMPLETE（已更新描述以匹配官方文档措辞） |
| 2 | LOW | 字段文档 | 更新 `effort` 字段描述 — 添加 `max（仅 Opus 4.6）` 限定符 | ✅ COMPLETE（已为 max 选项添加仅 Opus 4.6 的说明） |
| 3 | LOW | 字段文档 | 更新 `color` 字段描述 — 将 `（例如 green、magenta）` 替换为明确的有效值：`red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink`、`cyan` | ✅ COMPLETE（已将基于示例的描述替换为详尽的可用值列表） |

---

## [2026-04-09 11:34 PM PKT] Claude Code v2.1.97

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-11 06:10 PM PKT] Claude Code v2.1.101

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-13 08:02 PM PKT] Claude Code v2.1.101

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-14 11:14 PM PKT] Claude Code v2.1.107

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-16 08:16 PM PKT] Claude Code v2.1.110

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-18 07:53 PM PKT] Claude Code v2.1.114

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-24 12:27 AM PKT] Claude Code v2.1.118

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-26 01:10 PM PKT] Claude Code v2.1.119

未检测到偏差 — 报告与官方文档完全同步。所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-04-29 12:49 AM PKT] Claude Code v2.1.121

在两个跟踪维度上未检测到偏差 — 所有 16 个 frontmatter 字段和 5 个内置代理均匹配。应用户请求应用了一项范围外的枚举值更新。

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | LOW | 字段文档 | 更新 `effort` 字段描述 — 在 `high` 和 `max` 之间添加 `xhigh` 以匹配官方文档的枚举值列表 | ✅ COMPLETE（已在第 33 行插入 `xhigh`；反映了 v2.1.91 中将 `auto` 添加到 `permissionMode` 时的模式） |

---

## [2026-05-01 03:30 PM PKT] Claude Code v2.1.126

在两个跟踪维度上未检测到偏差 — 所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-05-12 11:38 PM PKT] Claude Code v2.1.139

在两个跟踪维度上未检测到偏差 — 所有 16 个 frontmatter 字段和 5 个内置代理均匹配。

---

## [2026-05-21 12:05 AM PKT] Claude Code v2.1.145

在两个跟踪维度上未检测到偏差 — 所有 16 个 frontmatter 字段和 5 个内置代理均匹配。
