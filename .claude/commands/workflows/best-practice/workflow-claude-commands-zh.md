---
description: 跟踪 Claude Code 命令报告变更并查找需要更新的内容
argument-hint: [要检查的版本数量，默认为 10]
---

# 工作流变更日志 — 命令报告

你是 claude-code-best-practice 项目的协调员。你的工作是启动一个 research agent，等待其结果，并生成一份关于**命令参考**报告（`best-practice/claude-commands.md`）中差异的报告。

此工作流只检查**两种类型的差异**：
1. **Frontmatter 字段** — 官方文档中新增或移除的任何字段
2. **官方命令** — 新增或移除的任何内置斜杠命令

**要检查的版本数：** `$ARGUMENTS`（如果为空或非数字，默认为 10）

这是一个**先读取后报告**的工作流。启动 agent，合并发现，生成报告。仅在用户批准后才执行操作。

---

## 阶段 1：启动 Research Agent

使用以下 prompt 生成 `workflow-claude-commands-agent`：

> 研究 claude-code-best-practice 项目中的命令报告差异。检查最近 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 2 个外部来源：
> 1. Slash Commands 参考：https://code.claude.com/docs/en/slash-commands
> 2. 变更日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-commands.md`）。
>
> 只检查以下两项：
> 1. **Frontmatter 字段**：将官方文档支持的命令 frontmatter 字段与报告的 Frontmatter Fields 表格进行对比。标记任何新增或移除的字段。
> 2. **官方命令**：将官方文档的内置斜杠命令列表与报告的官方命令表格进行对比。标记任何新增或移除的命令。同时检查是否有命令的 tag 或 description 发生了变化。

---

## 阶段 2：读取之前的变更日志条目

**在 agent 运行期间**，读取 `changelog/best-practice/claude-commands/changelog.md` 以获取最近 25 条条目。解析优先级操作以识别：
- **重复项** — 之前出现过且仍未解决的问题
- **新增项** — 首次出现的问题
- **已解决项** — 之前标记的问题现已修复

---

## 阶段 3：生成报告

**等待 agent 完成。** 生成包含以下部分的报告：

1. **Frontmatter 字段变更** — 官方文档中新增或移除的字段，与我们的报告对比
2. **官方命令变更** — 新增或移除的内置斜杠命令，与我们的表格对比

最后附带一个按优先级排序的**操作项**汇总表。每个项必须包含一个 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type              | Action                                | Status
1  | New Field         | Add <field> to frontmatter table      | NEW
2  | Removed Field     | Remove <field> from table             | RECURRING (first seen: <date>)
3  | New Command       | Add <command> to official table        | NEW
4  | Removed Command   | Remove <command> from table           | NEW
5  | Changed Tag       | Update <command> tag from X to Y      | NEW
```

同时包含一个**自上次运行以来已解决**部分，列出之前运行中已不再是问题的项。

---

## 阶段 3.5：将摘要追加到变更日志

**此阶段为强制阶段 — 在向用户呈现报告之前务必执行。**

读取现有的 `changelog/best-practice/claude-commands/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一条新条目。条目格式必须完全如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**状态格式 — 必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 操作已执行并成功解决
- `INVALID (reason)` — 发现结果不正确、不适用或是有意为之
- `ON HOLD (reason)` — 操作推迟，等待外部依赖或用户决定

`(reason)` 为必填，必须简要说明已完成的操作或原因。

**追加规则：**
- 始终追加 — 切勿覆盖或替换之前的条目
- 日期和时间是命令在巴基斯坦标准时间（PKT，UTC+5）中执行的时间；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本来自 agent 的发现
- 如果 `changelog/best-practice/claude-commands/changelog.md` 不存在或为空，则先创建 Status Legend 表格（参见文件顶部），然后创建第一条条目
- 每条条目之间用 `---` 分隔
- **只包含 HIGH、MEDIUM 或 LOW 优先级的项** — 省略 NONE 优先级的项

---

## 阶段 3.6：更新最后更新徽章

**此阶段为强制阶段 — 必须在阶段 3.5 之后、呈现报告之前立即执行。**

更新 `best-practice/claude-commands.md` 顶部的"最后更新"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格转 `%20`，逗号转 `%2C`），然后替换徽章中的日期部分。如果 Claude Code 版本有变化，也要更新徽章中的版本。

**不要将徽章更新记录为变更日志或报告中的操作项。** 徽章同步是每次运行的常规操作，不是发现结果。

---

## 阶段 4：提供执行选项

在呈现报告（并确认变更日志和徽章均已更新）后，询问用户：

1. **执行所有操作** — 应用所有变更
2. **执行特定操作** — 用户选择要执行的编号
3. **仅保存报告** — 不做任何变更

执行时：
- **新增字段**：将新字段添加到 Frontmatter Fields 表格，包含正确的类型、必需状态和官方文档中的描述
- **移除字段**：在移除前与用户确认
- **新增命令**：将新命令添加到官方命令表格，包含正确的 #、command、tag 和描述。插入到正确的 tag 组中（表格按 tag 排序）
- **移除命令**：在移除前与用户确认
- **更改 tag**：更新命令的 tag，必要时重新排序
- 执行任何新增或移除后，更新 `## Frontmatter Fields (N)` 和 `## ![Official](...) **(N)**` 标题中的计数

---

## 关键规则

1. **切勿猜测**版本或日期 — 使用 agent 提供的数据
2. **交叉验证字段数量** — 报告中的字段数量必须与官方文档一致
3. **交叉验证命令数量** — 报告中的命令数量必须与官方文档一致
4. **不要自动执行** — 始终先呈现报告
5. **始终追加到变更日志** — 阶段 3.5 为强制阶段。切勿跳过。切勿覆盖之前的条目。
6. **始终更新最后更新徽章** — 阶段 3.6 为强制阶段。切勿跳过。
7. **与之前的运行进行比较** — 从变更日志中读取最近 25 条条目，并将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
8. **维护 tag 排序顺序** — 官方命令表格按 tag（字母顺序）排序，在每个 tag 组内按命令名称排序。在添加或移除命令时保持此顺序。
