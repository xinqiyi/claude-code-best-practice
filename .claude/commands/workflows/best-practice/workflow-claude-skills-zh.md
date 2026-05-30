---
description: 追踪 Claude Code skills 报告变更并发现需要更新的内容
argument-hint: [要检查的版本数量，默认 10]
---

# 工作流变更日志 — Skills 报告

你是 claude-code-best-practice 项目的协调员。你的工作是启动一个 research agent，等待其结果，并就 **Skills 参考**报告（`best-practice/claude-skills.md`）中的漂移情况生成报告。

此工作流仅检查**两种漂移**：
1. **Frontmatter 字段** — 官方文档中新增或移除的任何字段
2. **官方内置 skills** — 新增或移除的任何内置 skill

**要检查的版本数：** `$ARGUMENTS`（如果为空或不是数字，默认为 10）

这是一个**先读取再报告**的工作流。启动 agent，合并发现结果，生成报告。仅在用户批准时才采取行动。

---

## 阶段 1：启动 Research Agent

使用以下 prompt 生成 `workflow-claude-skills-agent`：

> 研究 claude-code-best-practice 项目的 skills 报告漂移。检查最近 `$ARGUMENTS` 个版本（默认：10）。
>
> 获取以下 2 个外部来源：
> 1. Skills 参考：https://code.claude.com/docs/en/skills
> 2. 变更日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-skills.md`）。
>
> 仅检查两件事：
> 1. **Frontmatter 字段**：将官方文档支持的 skill frontmatter 字段与报告的 Frontmatter Fields 表格进行比较。标记任何新增或移除的字段。
> 2. **官方内置 skills**：将官方文档的内置 skills 列表（以及变更日志中提到的任何新内置 skills）与报告的官方 skills 表格进行比较。标记任何新增或移除的 skills。

---

## 阶段 2：读取之前的变更日志条目

**在 agent 运行期间**，读取 `changelog/best-practice/claude-skills/changelog.md` 以获取最近 25 个条目。解析优先级操作以识别：
- **重复项** — 之前出现过且仍未解决的问题
- **新项** — 首次出现的问题
- **已解决项** — 之前标记的问题现已修复

---

## 阶段 3：生成报告

**等待 agent 完成。** 生成包含以下部分的报告：

1. **Frontmatter 字段变更** — 官方文档中新增或移除的字段，与我们的报告对比
2. **官方内置 Skills 变更** — 新增或移除的内置 skills，与我们的表格对比

以优先级排序的 **Action Items** 汇总表结尾。每个项目必须包含 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type              | Action                                | Status
1  | New Field         | Add <field> to frontmatter table      | NEW
2  | Removed Field     | Remove <field> from table             | RECURRING (first seen: <date>)
3  | New Skill         | Add <skill> to official skills table   | NEW
4  | Removed Skill     | Remove <skill> from table             | NEW
```

还需包含一个 **Resolved Since Last Run** 部分，列出之前运行中已不再存在的问题。

---

## 阶段 3.5：将汇总追加到变更日志

**此阶段为强制阶段 — 在向用户呈现报告之前始终执行。**

读取现有的 `changelog/best-practice/claude-skills/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一个新条目。条目格式必须精确如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**状态格式 — 必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 已采取行动并成功解决
- `INVALID (reason)` — 发现结果不正确、不适用或是有意为之
- `ON HOLD (reason)` — 行动已推迟，等待外部依赖或用户决策

`(reason)` 为必填项，必须简要说明已完成的操作或原因。

**追加规则：**
- 始终追加 — 绝不覆盖或替换之前的条目
- 日期和时间是命令执行时的巴基斯坦标准时间（PKT, UTC+5）；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本号来自 agent 的发现结果
- 如果 `changelog/best-practice/claude-skills/changelog.md` 不存在或为空，先创建状态图例表（参见文件顶部）再创建第一个条目
- 每个条目之间用 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的项目** — 省略 NONE 优先级的项目

---

## 阶段 3.6：更新最后更新徽章

**此阶段为强制阶段 — 始终在阶段 3.5 之后立即执行，在呈现报告之前。**

更新 `best-practice/claude-skills.md` 顶部的"Last Updated"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格转 `%20`，逗号转 `%2C`），并替换徽章中的日期部分。如果 Claude Code 版本已变更，也更新徽章中的版本号。

**不要将徽章更新记录为变更日志或报告中的操作项。** 徽章同步是每次运行的常规部分，不是发现结果。

---

## 阶段 4：提供执行选项

在呈现报告（并确认变更日志和徽章均已更新）后，询问用户：

1. **执行所有操作** — 应用所有变更
2. **执行特定操作** — 用户选择要执行的编号
3. **仅保存报告** — 不做任何更改

执行时：
- **新字段**：使用官方文档中的正确类型、必填状态和描述添加到 Frontmatter Fields 表格
- **已移除的字段**：移除前与用户确认
- **新 skills**：使用正确的编号、skill 名称和描述添加到官方 skills 表格
- **已移除的 skills**：移除前与用户确认
- 在任何新增或移除后，更新 `## Frontmatter Fields (N)` 和 `## ![Official](...) **(N)**` 标题中的计数

---

## 关键规则

1. **从不猜测**版本或日期 — 使用来自 agent 的数据
2. **交叉核对字段计数** — 报告中的字段计数必须与官方文档一致
3. **交叉核对 skill 计数** — 报告中的 skill 计数必须与官方文档一致
4. **不要自动执行** — 始终先呈现报告
5. **始终追加到变更日志** — 阶段 3.5 是强制性的。绝不能跳过。绝不能覆盖之前的条目。
6. **始终更新"Last Updated"徽章** — 阶段 3.6 是强制性的。绝不能跳过。
7. **与之前的运行结果比较** — 从变更日志中读取最近 25 个条目，并将每个操作项标记为 NEW、RECURRING 或 RESOLVED
8. **区分内置和可安装的 skills** — 仅追踪随 Claude Code 一起发布的 skills（内置）。不要追踪来自官方 Skills 仓库（github.com/anthropics/skills）的 skills — 那些是可安装的，不是内置的。
