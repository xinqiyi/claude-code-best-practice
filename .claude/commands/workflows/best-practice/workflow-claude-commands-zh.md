---
description: 跟踪 Claude Code commands 报告的变更，发现需要更新的内容
argument-hint: [要检查的版本数，默认 10]
---

# Workflow Changelog — Commands 报告

你是 claude-code-best-practice 项目的协调者。你的工作是启动一个研究 agent，等待其结果，并呈现关于 **Commands 参考报告**（`best-practice/claude-commands.md`）漂移情况的报告。

此工作流检查恰好**两种类型的漂移**：
1. **Frontmatter 字段** — 官方文档中任何新增或移除的字段
2. **官方 commands** — 任何新增或移除的内置 slash command

**要检查的版本数：** `$ARGUMENTS`（默认为空或非数字时为 10）

这是一个**先读后报**工作流。启动 agent，合并发现结果，生成报告。仅在用户批准后才执行操作。

---

## 阶段 1：启动研究 Agent

使用以下 prompt 启动 `workflow-claude-commands-agent`：

> 研究 claude-code-best-practice 项目，检测 commands 报告的漂移情况。检查最后 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 2 个外部数据源：
> 1. Slash Commands 参考文档：https://code.claude.com/docs/en/slash-commands
> 2. Changelog：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告（`best-practice/claude-commands.md`）。
>
> 检查恰好两项内容：
> 1. **Frontmatter 字段**：将官方文档支持的 command frontmatter 字段与报告的 Frontmatter Fields 表格进行比较。标记任何新增或移除的字段。
> 2. **官方 commands**：将官方文档的内置 slash commands 列表与报告的 official commands 表格进行比较。标记任何新增或移除的 commands。同时检查是否有 command 的 tag 或描述发生了变化。

---

## 阶段 2：读取之前的 Changelog 条目

**在 agent 运行期间**，读取 `changelog/best-practice/claude-commands/changelog.md` 以获取最后 25 个条目。解析 priority actions 以识别：
- **重复出现的项目** — 之前出现过且仍未解决的问题
- **新出现的项目** — 首次出现的问题
- **已解决的项目** — 之前标记的问题现已修复

---

## 阶段 3：生成报告

**等待 agent 完成。** 生成包含以下部分的报告：

1. **Frontmatter 字段变更** — 官方文档中新增或移除的字段，与我们的报告对比
2. **官方 Command 变更** — 内置 slash commands 的新增或移除，与我们的表格对比

最后附上带优先级的 **Action Items** 摘要表格。每个项目必须包含 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type              | Action                                | Status
1  | New Field         | Add <field> to frontmatter table      | NEW
2  | Removed Field     | Remove <field> from table             | RECURRING (first seen: <date>)
3  | New Command       | Add <command> to official table        | NEW
4  | Removed Command   | Remove <command> from table           | NEW
5  | Changed Tag       | Update <command> tag from X to Y      | NEW
```

同时包含一个 **Resolved Since Last Run** 部分，列出之前运行中已不再存在的问题。

---

## 阶段 3.5：将摘要追加到 Changelog

**此阶段是强制性的——始终在向用户呈现报告之前执行。**

读取现有的 `changelog/best-practice/claude-commands/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一个新条目。条目格式必须严格如下：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**Status 格式——必须使用以下三种格式之一：**
- `COMPLETE (reason)` — 已采取行动并成功解决
- `INVALID (reason)` — 发现不正确、不适用或是有意为之
- `ON HOLD (reason)` — 行动推迟，等待外部依赖或用户决定

`(reason)` 是必填项，必须简要说明已完成的操作或原因。

**追加规则：**
- 始终追加——绝不覆盖或替换之前的条目
- 日期和时间为命令执行时的巴基斯坦标准时间（PKT, UTC+5）；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本号来自 agent 的发现结果
- 如果 `changelog/best-practice/claude-commands/changelog.md` 不存在或为空，创建一个带有 Status Legend 表格的文件（参见文件顶部），然后添加第一个条目
- 每个条目之间用 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的项目**——省略 NONE 优先级的项目

---

## 阶段 3.6：更新 Last Updated Badge

**此阶段是强制性的——始终在阶段 3.5 之后、向用户呈现报告之前立即执行。**

更新 `best-practice/claude-commands.md` 顶部的 "Last Updated" badge。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，对其进行 URL 编码（空格转 `%20`，逗号转 `%2C`），替换 badge 中的日期部分。如果 Claude Code 版本有变化，同时更新 badge 中的版本号。

**不要将 badge 更新记录为 changelog 或报告中的 action item。** Badge 同步是每次运行的例行操作，不是发现结果。

---

## 阶段 4：提供执行选项

在呈现报告之后（并确认 changelog 和 badge 均已更新），询问用户：

1. **Execute all actions** — 应用所有变更
2. **Execute specific actions** — 用户选择要执行的编号
3. **Just save the report** — 不做变更

执行时：
- **新增字段**：添加到 Frontmatter Fields 表格，包含来自官方文档的正确类型、是否必填和描述
- **移除字段**：在移除前与用户确认
- **新增 commands**：添加到 official commands 表格，包含正确的 #、command、tag 和描述。插入到正确的 tag 分组中（表格按 tag 排序）
- **移除 commands**：在移除前与用户确认
- **变更的 tags**：更新 command 的 tag，并在需要时重新排序
- 在任何新增或移除之后，更新 `## Frontmatter Fields (N)` 和 `## ![Official](...) **(N)**` 标题中的计数

---

## 关键规则

1. **绝不猜测**版本或日期——使用 agent 的数据
2. **交叉验证字段计数**——报告中的字段数必须与官方文档一致
3. **交叉验证 command 计数**——报告中的 command 数必须与官方文档一致
4. **不要自动执行**——始终先呈现报告
5. **始终追加到 changelog**——阶段 3.5 是强制性的。绝不可跳过。绝不可覆盖之前的条目。
6. **始终更新 Last Updated badge**——阶段 3.6 是强制性的。绝不可跳过。
7. **与之前的运行结果比较**——读取 changelog 的最后 25 个条目，将每个 action item 标记为 NEW、RECURRING 或 RESOLVED。
8. **保持 tag 排序规则**——official commands 表格按 tag（字母序）排序，然后在每个 tag 分组内按 command 名称排序。在新增或移除 commands 时保持此排序规则。
