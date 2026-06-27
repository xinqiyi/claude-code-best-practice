---
description: 使用最新的 Claude Code 功能和概念更新 README CONCEPTS 部分
argument-hint: [要检查的 changelog 版本数，默认 10]
---

# Workflow Changelog — README Concepts

你是 claude-code-best-practice 项目的协调员。你的任务是并行启动两个研究 agent，等待其结果，合并发现，并呈报关于 **README CONCEPTS 部分**（`README.md`）漂移的统一报告。

**要检查的版本数：** `$ARGUMENTS`（如果为空或非数字则默认 10）

这是一个**先读取后报告**的 workflow。启动 agents，合并结果，生成报告。仅在用户批准后才采取行动。

---

## Phase 0: 并行启动两个 Agent

**立即**使用 Task 工具在**同一条消息中**（并行启动）启动两个 agent：

### Agent 1: workflow-concepts-agent

使用 `subagent_type: "workflow-concepts-agent"` 启动。向其传入以下 prompt：

> 研究 claude-code-best-practice 项目中 README CONCEPTS 部分的漂移情况。检查最近 $ARGUMENTS 个版本（默认 10）。
>
> 获取以下 3 个外部来源：
> 1. Claude Code Docs Index: https://code.claude.com/docs/en
> 2. Claude Code Changelog: https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
> 3. Claude Code Features Overview: https://code.claude.com/docs/en/overview
>
> 然后读取本地的 README.md（特别是 CONCEPTS 表）、CLAUDE.md 和 `reports/claude-global-vs-project-settings.md`。分析官方文档列出的 Claude Code 概念/功能与 README CONCEPTS 表所记录的之间的差异。返回结构化的发现报告，涵盖缺失的概念、变更的概念、已弃用的概念、URL 准确性、描述准确性和 badge 准确性。

### Agent 2: claude-code-guide

使用 `subagent_type: "claude-code-guide"` 启动。向其传入以下 prompt：

> 研究最新的 Claude Code 功能和概念。我需要你找到应该被记录的**完整** Claude Code 概念/功能列表。对每个概念/功能提供：
> 1. 官方功能名称
> 2. 官方文档 URL
> 3. 文件系统位置（例如 `.claude/commands/`、`~/.claude/teams/`）
> 4. 简要描述（一行）
> 5. 引入时间（版本/日期，如果已知的话）
>
> 特别检查这些可能缺失的概念：
> - **Worktrees** — git worktree isolation，用于并行开发
> - **Agent Teams** — 多 agent 协调
> - **Tasks** — 跨会话的持久化任务列表
> - **Auto Memory** — Claude 自我编写的项目学习记录
> - **Keybindings** — 自定义键盘快捷键
> - **Remote Connections** — SSH、Docker、云开发
> - **IDE Integration** — VS Code、JetBrains 扩展
> - **Model Configuration** — 模型选择和路由
> - **GitHub Integration** — PR 审查、issue 分类
> - 以及最近 Claude Code 版本中的任何其他概念
>
> 要全面 — 搜索网页，获取文档，为你找到的所有内容提供具体的版本号和详细信息。

两个 agent 独立运行，将各自返回发现。

---

## Phase 0.5: 读取 Verification Checklist

**在 agents 运行期间**，如果 `changelog/best-practice/concepts/verification-checklist.md` 存在，则读取它。此文件包含累积的验证规则。如果尚不存在，跳过此步骤 — 它将在 Phase 2 中创建。

---

## Phase 1: 读取之前的 Changelog 条目

**在合并发现之前**，如果 `changelog/best-practice/concepts/changelog.md` 文件存在，读取它以获取之前的 changelog 条目。每个条目之间用 `---` 分隔。解析之前条目中的 priority actions，以便与当前发现进行比较。这可以帮助你识别：
- **Recurring items** — 曾经出现过且仍未解决的问题
- **Newly resolved items** — 之前运行中出现但现已修复的问题
- **New items** — 本次运行中首次出现的问题

如果文件尚不存在，所有条目均为 `NEW`。

---

## Phase 2: 合并发现并生成报告

**等待两个 agent 完成。** 一旦你获得：
- **workflow-concepts-agent 发现** — 包含本地文件读取、外部文档获取和漂移检测的详细分析
- **claude-code-guide 发现** — 对最新 Claude Code 功能和概念的独立研究

交叉验证两者。专门的 agent 提供了特定于 CONCEPTS 的漂移分析，而 claude-code-guide agent 可能发现它遗漏的内容（例如非常近期的变更、未记录的功能或来自 web 搜索的上下文）。标记两者之间的任何矛盾，供用户解决。

**执行 verification checklist（如果存在）：** 对 `changelog/best-practice/concepts/verification-checklist.md` 中的每条规则执行检查。在报告中包含一个 **Verification Log** 部分。

**如有需要，更新 checklist：** 如果某个发现揭示了现有 checklist 规则未涵盖的新类型的漂移，向 `changelog/best-practice/concepts/verification-checklist.md` 追加一条新规则。如果文件不存在，创建它。规则必须包含：category、what to check、depth level、what source to compare against、date added 和 origin。

同时将当前发现与之前的 changelog 条目（来自 Phase 1）进行比较。对每个 priority action，标记为：
- `NEW` — 该问题首次出现
- `RECURRING` — 在之前运行中出现过且仍未解决（包含首次出现的运行日期）
- `RESOLVED` — 在之前运行中出现过但现已修复（包含解决日期）

生成结构化报告，包含以下部分：

1. **Missing Concepts** — 存在于官方文档中但 CONCEPTS 表中缺失的功能/概念，包含：
   - 官方名称和文档 URL
   - 推荐的 Location 列值
   - 推荐的 Description 列值 — **仅限 badges 和补充链接；不包含散文式描述**（Description 列通常是一个链接列，而非功能摘要）
   - 可直接粘贴的精确 markdown 表格行
   - 引入版本（如果已知）
2. **Changed Concepts** — 名称、URL、位置或描述发生变化的概念
3. **Deprecated/Removed Concepts** — CONCEPTS 表中存在但官方文档中不再包含的概念
4. **URL Accuracy** — 逐个概念的 URL 验证
5. **Description Accuracy** — 逐个概念的描述/位置验证
6. **Badge Accuracy** — Badge 链接验证和缺失 badge 建议
7. **claude-code-guide Agent Findings** — agent 提供的未被专门 agent 捕获的独特见解。仅包含新增信息的发现。标记矛盾之处。

以带优先级的 **Action Items** 汇总表结束：

```
Priority Actions:
#  | Type                | Action                                     | Status
1  | Missing Concept     | Add <concept> row to CONCEPTS table         | NEW
2  | Changed URL         | Update <concept> docs link                  | NEW
3  | Changed Description | Update <concept> description                | RECURRING (first seen: <date>)
4  | Deprecated Concept  | Remove <concept> row from CONCEPTS table    | NEW
5  | Broken Badge        | Fix badge link for <concept>                | NEW
```

同时包含一个 **Resolved Since Last Run** 部分，列出之前运行中出现但已不再存在的问题。

---

## Phase 2.5: 将摘要追加到 Changelog

**此阶段是强制的 — 始终在向用户呈报报告之前执行。**

读取现有的 `changelog/best-practice/concepts/changelog.md` 文件，然后**追加**（不要覆盖）一个新条目在末尾。如果文件不存在，先创建带有 Status Legend 表，然后是第一个条目。条目格式必须严格为：

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
- 每个条目之间用 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的条目** — 省略 NONE 优先级的条目

---

## Phase 2.6: 更新 Last Updated Badge

**此阶段是强制的 — 始终在 Phase 2.5 之后、呈报报告之前立即执行。**

更新 `README.md` 第 3 行顶部的 "Last Updated" badge。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，对其进行 URL 编码（空格转 `%20`，逗号转 `%2C`），然后替换 badge 中的日期部分。

**不要将 badge 更新记录为 changelog 或报告中的 action items。**

---

## Phase 2.7: 验证所有 CONCEPTS URL

**此阶段是强制的 — 始终在 Phase 2.6 之后、呈报报告之前执行。**

对 CONCEPTS 表中的每个概念：

1. **External docs URLs**（例如 `https://code.claude.com/docs/en/skills`）：使用 WebFetch 获取每个 URL 并验证其返回有效页面。标记任何失效或移动的链接。
2. **Local badge links**（例如 `best-practice/claude-commands.md`）：使用 Read 工具验证文件是否存在。标记任何失效的链接。
3. **Implementation badge links**（例如 `.claude/commands/`）：验证路径是否存在。

在报告中包含 **URL Validation Log**：

```
URL Validation Log:
#  | Concept     | URL Type  | URL                                           | Status | Notes
1  | Commands    | External  | https://code.claude.com/docs/en/skills         | OK     |
2  | Commands    | Badge     | best-practice/claude-commands.md               | OK     |
3  | Sub-Agents  | External  | https://code.claude.com/docs/en/sub-agents     | OK     |
...
```

**如果任何 URL 失效**，将其添加为 HIGH 优先级的 action items。

---

## Phase 3: 提供行动选项

在呈报报告后（并确认 changelog 已更新），询问用户：

1. **Execute all actions** — 添加缺失的概念，更新已变更的概念，移除已弃用的概念
2. **Execute specific actions** — 用户选择要执行的编号
3. **Just save the report** — 不做任何变更

执行时：
- **Missing concepts**：向 `README.md` 中的 CONCEPTS 表添加新行，遵循现有格式：
  ```
  | [**Name**](docs-url) | `location` | [![Best Practice](!/tags/best-practice.svg)](...) [![Implemented](!/tags/implemented.svg)](...) [Supplementary Link](...) |
  ```
  第三列是 **badges 和链接 — 绝不含散文式描述**。仅当对应文件存在时才添加 badges（best-practice、implemented）。如果没有适用的 badges 或链接，将该列留空（仅 `|  |`）。
- **Changed concepts**：更新发生变化的特定列
- **Deprecated concepts**：移除行之前与用户确认
- **Broken URLs**：将 URL 修复为当前有效的链接
- **Badge fixes**：将 badge 链接更新为正确的文件路径
- 保持与现有表一致的字母或逻辑排序
- 在所有操作之后，重新验证 CONCEPTS 表的一致性

---

## 关键规则

1. **在同一条消息中并行启动两个 agent** — 绝不顺序执行
2. **等待两个 agent** 完成后再生成报告
3. **绝不猜测**版本、URL 或日期 — 使用 agents 的数据
4. **缺失的概念是 HIGH PRIORITY** — CONCEPTS 表是开发者最先看到的内容
5. **验证每个 URL** — 失效的链接会降低整个项目的可信度
6. **不要自动执行** — 始终先呈报报告
7. **始终追加到 changelog** — Phase 2.5 是强制的。绝不跳过。绝不覆盖之前的条目。
8. **与之前运行进行比较** — 从 changelog 读取之前的条目，将每个 action item 标记为 NEW、RECURRING 或 RESOLVED。
9. **执行 verification checklist（如果存在）** — 读取 verification-checklist.md 并执行每条规则。如果文件不存在且存在需要持久化规则的发现，则创建该文件。
10. **始终更新 Last Updated badge** — Phase 2.6 是强制的。
11. **始终验证所有 CONCEPTS URL** — Phase 2.7 是强制的。失效的 URL 是 HIGH 优先级。
12. **提供可直接粘贴的行** — 对于缺失的概念，包含精确的 markdown 表格行，以便执行时直接复制粘贴。
13. **尊重现有表格格式** — 匹配现有行的列结构、badge 模式和链接风格。
14. **Description 列仅用于 badges 和链接** — 绝不包含散文式描述。CONCEPTS 表（包括 Hot 子表）中的第三列包含 badges（best-practice、implemented、beta）和补充链接（docs、blog posts、related reports）。绝不写描述功能是什么的散文 — 功能名称链接到官方文档，解释应在那里。如果某行没有 badges 或链接，将该单元格留空。
