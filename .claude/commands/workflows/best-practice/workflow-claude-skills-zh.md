---
description: 跟踪 Claude Code skills 报告变更并查找需要更新的内容
argument-hint: [要检查的版本数，默认 10]
---

# Workflow Changelog — Skills Report

你是 claude-code-best-practice 项目的协调员。你的任务是启动一个研究 agent，等待其结果，并呈报 **Skills Reference** 报告（`best-practice/claude-skills.md`）的漂移情况。

此 workflow 检查的正好是**两种漂移类型**：
1. **Frontmatter 字段** — 官方文档中新增或移除的任何字段
2. **官方 bundled skills** — 新增或移除的任何 bundled skill

**要检查的版本数：** `$ARGUMENTS`（如果为空或非数字则默认 10）

这是一个**先读取后报告**的 workflow。启动 agent，合并发现，生成报告。仅在用户批准后才采取行动。

---

## Phase 1: 启动研究 Agent

使用以下 prompt 启动 `workflow-claude-skills-agent`：

> 研究 claude-code-best-practice 项目中 skills 报告的漂移情况。检查最近 $ARGUMENTS 个版本（默认 10）。
>
> 获取以下 2 个外部来源：
> 1. Skills Reference: https://code.claude.com/docs/en/skills
> 2. Changelog: https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-skills.md`）。
>
> 检查两件事：
> 1. **Frontmatter 字段**：将官方文档支持的 skill frontmatter 字段与报告的 Frontmatter Fields 表进行比较。标记任何新增或移除的字段。
> 2. **Official bundled skills**：将官方文档的 bundled skills 列表（以及 changelog 中提到的任何新增 bundled skills）与报告的 official skills 表进行比较。标记任何新增或移除的 skills。

---

## Phase 2: 读取之前的 Changelog 条目

**在 agent 运行期间**，读取 `changelog/best-practice/claude-skills/changelog.md` 获取最近 25 条条目。解析 priority actions 以识别：
- **Recurring items** — 曾经出现过且仍未解决的问题
- **New items** — 首次出现的问题
- **Resolved items** — 之前标记的问题现已修复

---

## Phase 3: 生成报告

**等待 agent 完成。** 生成包含以下部分的报告：

1. **Frontmatter Field Changes** — 官方文档与报告相比新增或移除的字段
2. **Official Bundled Skill Changes** — 与表格相比新增或移除的 bundled skills

以带优先级的 **Action Items** 汇总表结束。每个条目必须包含一个 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type              | Action                                | Status
1  | New Field         | Add <field> to frontmatter table      | NEW
2  | Removed Field     | Remove <field> from table             | RECURRING (first seen: <date>)
3  | New Skill         | Add <skill> to official skills table   | NEW
4  | Removed Skill     | Remove <skill> from table             | NEW
```

同时包含一个 **Resolved Since Last Run** 部分，列出之前运行中出现但已不再存在的问题。

---

## Phase 3.5: 将摘要追加到 Changelog

**此阶段是强制的 — 始终在向用户呈报报告之前执行。**

读取现有的 `changelog/best-practice/claude-skills/changelog.md` 文件，然后**追加**（不要覆盖）一个新条目在末尾。条目格式必须严格为：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**Status 格式 — 必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 已采取行动并成功解决
- `INVALID (reason)` — 发现不正确、不适用或属有意为之
- `ON HOLD (reason)` — 推迟行动，等待外部依赖或用户决定

`(reason)` 是强制的，必须简要说明做了什么或原因。

**追加规则：**
- 始终追加 — 绝不覆盖或替换之前的条目
- 日期和时间为命令执行时的 Pakistan Standard Time（PKT，UTC+5）；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本号来自 agent 的发现
- 如果 `changelog/best-practice/claude-skills/changelog.md` 不存在或为空，先创建带有 Status Legend 表（参见文件顶部），然后是第一个条目
- 每个条目之间用 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的条目** — 省略 NONE 优先级的条目

---

## Phase 3.6: 更新 Last Updated Badge

**此阶段是强制的 — 始终在 Phase 3.5 之后、呈报报告之前立即执行。**

更新 `best-practice/claude-skills.md` 顶部的 "Last Updated" badge。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，对其进行 URL 编码（空格转 `%20`，逗号转 `%2C`），然后替换 badge 中的日期部分。如果版本号有变化，同时更新 badge 中的 Claude Code 版本。

**不要将 badge 更新记录为 changelog 或报告中的 action items。** Badge 同步是每次运行的常规操作，不是发现项。

---

## Phase 4: 提供行动选项

在呈报报告后（并确认 changelog 和 badge 均已更新），询问用户：

1. **Execute all actions** — 应用所有变更
2. **Execute specific actions** — 用户选择要执行的编号
3. **Just save the report** — 不做任何变更

执行时：
- **New fields**：添加到 Frontmatter Fields 表，包含来自官方文档的正确类型、是否必填和描述
- **Removed fields**：移除前与用户确认
- **New skills**：添加到 official skills 表，包含正确的 #、skill name 和 description
- **Removed skills**：移除前与用户确认
- 在新增或移除后，更新 `## Frontmatter Fields (N)` 和 `## ![Official](...) **(N)**` 标题中的计数

---

## 关键规则

1. **绝不猜测**版本或日期 — 使用 agent 的数据
2. **交叉验证字段计数** — 报告的字段数必须与官方文档一致
3. **交叉验证 skill 计数** — 报告的 skill 数必须与官方文档一致
4. **不要自动执行** — 始终先呈报报告
5. **始终追加到 changelog** — Phase 3.5 是强制的。绝不跳过。绝不覆盖之前的条目。
6. **始终更新 Last Updated badge** — Phase 3.6 是强制的。绝不跳过。
7. **与之前运行进行比较** — 从 changelog 读取最近 25 条条目，将每个 action item 标记为 NEW、RECURRING 或 RESOLVED。
8. **区分 bundled 与 installable** — 只跟踪随 Claude Code 发布（bundled）的 skills。不跟踪来自 Official Skills Repository（github.com/anthropics/skills）的 skills — 那些是 installable 而非 bundled。
