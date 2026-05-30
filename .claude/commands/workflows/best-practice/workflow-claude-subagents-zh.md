---
description: 跟踪 Claude Code subagents 报告的变更，发现需要更新的内容
argument-hint: [要检查的版本数量，默认为 10]
---

# 工作流变更日志 — Subagents 报告

你是一个 claude-code-best-practice 项目的协调者。你的工作是启动一个 research agent，等待其返回结果，并提交一份关于 **Subagents 参考** 报告（`best-practice/claude-subagents.md`）中差异的报告。

此工作流只检查**两种类型的差异**：
1. **Frontmatter 字段** — 官方文档中新增或移除的任何字段
2. **官方 sub-agents** — 新增或移除的任何内置 agent

**要检查的版本数：** `$ARGUMENTS`（如果为空或非数字，默认为 10）

这是一个**先读取再报告**的工作流。启动 agent，合并发现结果，生成报告。只有在用户批准时才执行操作。

---

## 阶段 1：启动 Research Agent

使用以下提示生成 `workflow-claude-subagents-agent`：

> 研究 claude-code-best-practice 项目中的 subagents 报告差异。检查最近 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 2 个外部来源：
> 1. Sub-agents 参考：https://code.claude.com/docs/en/sub-agents
> 2. 更新日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-subagents.md`）。
>
> 只检查以下两件事：
> 1. **Frontmatter 字段**：将官方文档支持的 frontmatter 字段表与报告的 Frontmatter 字段表进行比较。标记任何新增或移除的字段。
> 2. **官方 sub-agents**：将官方文档的内置 subagents 列表与报告的官方 agents 表进行比较。标记任何新增或移除的 agent。

---

## 阶段 2：读取之前的变更日志条目

**在 agent 运行的同时**，读取 `changelog/best-practice/claude-subagents/changelog.md` 以获取最近 25 条条目。解析优先操作以识别：
- **重复项** — 之前出现过且仍未解决的问题
- **新项** — 首次出现的问题
- **已解决项** — 之前标记的问题现已修复

---

## 阶段 3：生成报告

**等待 agent 完成。** 生成包含以下部分的报告：

1. **Frontmatter 字段变更** — 官方文档中相对于我们报告新增或移除的字段
2. **官方 Sub-agent 变更** — 相对于我们表格新增或移除的内置 agents

最后附上一个按优先级排序的**操作项**汇总表。每个项必须包含一个 `Status` 列，显示 `NEW`、`RECURRING（首次出现：<日期>）` 或 `RESOLVED`：

```
Priority Actions:
#  | Type           | Action                              | Status
1  | New Field      | 在 frontmatter 表中添加 <field>      | NEW
2  | Removed Field  | 从表中移除 <field>                   | RECURRING（首次出现：<日期>）
3  | New Agent      | 在官方 agents 表中添加 <agent>        | NEW
4  | Removed Agent  | 从表中移除 <agent>                    | NEW
```

同时包含一个**自上次运行以来已解决**部分，列出之前运行中已不再存在的问题项。

---

## 阶段 3.5：将摘要附加到变更日志

**此阶段是强制性的 — 在向用户展示报告之前必须执行。**

读取现有的 `changelog/best-practice/claude-subagents/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一条新条目。条目格式必须精确如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<版本号>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <类型> | <操作描述> | <状态> |
| ... | ... | ... | ... | ... |
```

**状态格式 — 必须使用以下三种格式之一：**
- `COMPLETE（原因）` — 已执行操作并成功解决
- `INVALID（原因）` — 发现结果不正确、不适用或是有意为之
- `ON HOLD（原因）` — 操作已推迟，等待外部依赖或用户决策

`（原因）` 是必须的，需简要说明做了什么或为什么。

**追加规则：**
- 始终追加 — 绝不覆盖或替换之前的条目
- 日期和时间是命令执行时的巴基斯坦标准时间（PKT，UTC+5）；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本号来自 agent 的发现结果
- 如果 `changelog/best-practice/claude-subagents/changelog.md` 不存在或为空，则先创建包含状态图例表（见文件顶部）的内容，再添加第一条条目
- 每条条目之间用 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的项** — 省略 NONE 优先级的项

---

## 阶段 3.6：更新最后更新徽章

**此阶段是强制性的 — 必须在阶段 3.5 之后、展示报告之前立即执行。**

更新 `best-practice/claude-subagents.md` 顶部的"最后更新"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格转 `%20`，逗号转 `%2C`），并替换徽章中的日期部分。如果 Claude Code 版本已变更，也同时更新徽章中的版本号。

**不要将徽章更新记录为变更日志或报告中的操作项。** 徽章同步是每次运行的常规部分，不是发现结果。

---

## 阶段 4：提供执行操作选项

在展示报告（并确认变更日志和徽章均已更新）之后，询问用户：

1. **执行所有操作** — 应用所有变更
2. **执行特定操作** — 用户选择要执行的编号
3. **仅保存报告** — 不做变更

执行时：
- **新增字段**：添加到 Frontmatter 字段表，包含正确的类型、必需状态和来自官方文档的描述
- **移除的字段**：在移除前与用户确认
- **新增 agents**：添加到官方 agents 表，包含正确的编号、名称、model、tools 和描述
- **移除的 agents**：在移除前与用户确认

---

## 关键规则

1. **从不猜测**版本或日期 — 使用来自 agent 的数据
2. **交叉核对字段数量** — 报告中的字段数量必须与官方文档一致
3. **交叉核对 agent 数量** — 报告中的 agent 数量必须与官方文档一致
4. **不自动执行** — 始终先展示报告
5. **始终追加到变更日志** — 阶段 3.5 是强制性的。绝不能跳过。绝不能覆盖之前的条目。
6. **始终更新最后更新徽章** — 阶段 3.6 是强制性的。绝不能跳过。
7. **与之前的运行结果进行比较** — 从变更日志中读取最近 25 条条目，将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
