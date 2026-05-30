---
description: 使用最新的 Claude Code 特性和概念更新 README CONCEPTS 部分
argument-hint: [number of changelog versions to check, default 10]
---

# Workflow Changelog — README Concepts

你是 claude-code-best-practice 项目的协调员。你的工作是并行启动两个 research agent，等待它们的结果，合并发现，并提交一份关于 **README CONCEPTS 部分** (`README.md`) 差异的统一报告。

**要检查的版本数:** `$ARGUMENTS`（默认：如果为空或非数字则为 10）

这是一个 **先读取再报告** 的 workflow。启动 agents，合并结果，并生成报告。仅在用户批准时才采取行动。

---

## Phase 0: 并行启动两个 Agents

**立即**使用 Task 工具在 **同一条消息** 中（并行启动）同时生成两个 agents：

### Agent 1: workflow-concepts-agent

使用 `subagent_type: "workflow-concepts-agent"` 生成。提供如下 prompt：

> Research the claude-code-best-practice project for README CONCEPTS section drift. Check the last $ARGUMENTS versions (default: 10).
>
> 获取以下 3 个外部来源：
> 1. Claude Code 文档索引：https://code.claude.com/docs/en
> 2. Claude Code Changelog：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
> 3. Claude Code 功能概览：https://code.claude.com/docs/en/overview
>
> 然后读取本地 README.md（特别是 CONCEPTS 表格）、CLAUDE.md 和 `reports/claude-global-vs-project-settings.md`。分析官方文档列出的 Claude Code 概念/功能与我们的 README CONCEPTS 表格记录的内容之间的差异。返回一份结构化发现报告，涵盖缺失的概念、变化的概念、弃用的概念、URL 准确性、描述准确性和徽章准确性。

### Agent 2: claude-code-guide

使用 `subagent_type: "claude-code-guide"` 生成。提供如下 prompt：

> Research the latest Claude Code features and concepts. I need you to find the COMPLETE list of all Claude Code concepts/features that should be documented. For each, provide:
> 1. 官方功能名称
> 2. 官方文档 URL
> 3. 文件系统位置（例如，`.claude/commands/`、`~/.claude/teams/`）
> 4. 简要描述（一行）
> 5. 引入时间（如果知道版本/日期）
>
> 特别检查这些可能缺失的概念：
> - **Worktrees** — 用于并行开发的 git worktree 隔离
> - **Agent Teams** — 多 agent 协调
> - **Tasks** — 跨会话的持久化任务列表
> - **Auto Memory** — Claude 自动写入的项目学习记录
> - **Keybindings** — 自定义键盘快捷键
> - **Remote Connections** — SSH、Docker、云端开发
> - **IDE Integration** — VS Code、JetBrains 扩展
> - **Model Configuration** — 模型选择和路由
> - **GitHub Integration** — PR 审查、问题分类
> - 以及来自近期 Claude Code 版本的任何其他概念
>
> 要全面——搜索网络、获取文档，并提供具体版本号和所有发现的详细信息。

两个 agents 独立运行并返回它们的结果。

---

## Phase 0.5: 读取验证检查清单

**在 agents 运行时**，如果存在，读取 `changelog/best-practice/concepts/verification-checklist.md`。该文件包含累积的验证规则。如果该文件还不存在，则跳过此步骤——它将在 Phase 2 中创建。

---

## Phase 1: 读取之前的 Changelog 条目

**在合并结果之前**，如果存在，读取文件 `changelog/best-practice/concepts/changelog.md` 以获取之前的 changelog 条目。每个条目由 `---` 分隔。解析先前条目中的优先级操作，以便与当前发现进行比较。这可以让你识别：
- **Recurring items** — 之前出现过且仍未解决的问题
- **Newly resolved items** — 之前运行中存在的但现在已修复的问题
- **New items** — 在此次运行中首次出现的问题

如果该文件尚不存在，则所有项均为 `NEW`。

---

## Phase 2: 合并结果并生成报告

**等待两个 agents 完成。** 一旦你拿到：
- **workflow-concepts-agent 的发现** — 包含本地文件读取、外部文档获取和差异检测的详细分析
- **claude-code-guide 的发现** — 关于最新 Claude Code 特性和概念的独立研究

交叉引用两者。专用 agent 提供特定于 CONCEPTS 的差异分析，而 claude-code-guide agent 可能会发现它遗漏的内容（例如，非常近期的变更、未记录的功能或来自网络搜索的上下文）。标记两者之间的任何矛盾之处供用户解决。

**执行验证检查清单（如果存在）：** 对于 `changelog/best-practice/concepts/verification-checklist.md` 中的每一条规则，执行检查。在报告中包含 **Verification Log** 部分。

**必要时更新检查清单：** 如果发现揭示了一种现有检查清单规则未涵盖的新差异类型，则将新规则追加到 `changelog/best-practice/concepts/verification-checklist.md`。如果文件不存在，则创建它。规则必须包括：分类、检查内容、深度级别、比较来源、添加日期和来源。

同时将当前发现与之前的 changelog 条目（来自 Phase 1）进行比较。对于每个优先级操作，标记为：
- `NEW` — 该问题首次出现
- `RECURRING` — 在之前的运行中出现过且仍未解决（包括首次出现的运行日期）
- `RESOLVED` — 在之前的运行中出现过但现在已修复（包括解决日期）

生成包含以下部分的结构化报告：

1. **缺失的概念** — 官方文档中存在但 CONCEPTS 表格中缺失的功能/概念，包含：
   - 官方名称和文档 URL
   - 推荐位置列的值
   - 推荐描述列的值 — **仅徽章和补充链接；无散文描述**（描述列通常是链接列，而非功能摘要）
   - 准备粘贴的确切 markdown 表格行
   - 引入版本（如果知道）
2. **变化的概念** — 名称、URL、位置或描述已更改的概念
3. **已弃用/已移除的概念** — CONCEPTS 表格中存在但官方文档中不再存在的概念
4. **URL 准确性** — 按概念进行 URL 验证
5. **描述准确性** — 按概念进行描述/位置验证
6. **徽章准确性** — 徽章链接验证和缺失徽章建议
7. **claude-code-guide agent 的发现** — 来自该 agent 但未被专用 agent 捕获的独特见解。仅包含增加新信息的发现。标记矛盾之处。

以优先化的 **Action Items** 摘要表格结尾：

```
Priority Actions:
#  | Type                | Action                                     | Status
1  | Missing Concept     | Add <concept> row to CONCEPTS table         | NEW
2  | Changed URL         | Update <concept> docs link                  | NEW
3  | Changed Description | Update <concept> description                | RECURRING (first seen: <date>)
4  | Deprecated Concept  | Remove <concept> row from CONCEPTS table    | NEW
5  | Broken Badge        | Fix badge link for <concept>                | NEW
```

同时包含 **自上次运行以来已解决** 部分，列出上次运行中不再存在的问题。

---

## Phase 2.5: 将摘要追加到 Changelog

**此阶段是强制性的——在向用户呈现报告之前始终执行。**

读取现有的 `changelog/best-practice/concepts/changelog.md` 文件，然后在末尾 **追加**（不要覆盖）一个新条目。如果文件不存在，则先创建一个状态图例表格，然后添加第一个条目。条目格式必须完全如下：

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
- `INVALID (reason)` — 发现不正确、不适用或有意为之
- `ON HOLD (reason)` — 操作推迟，等待外部依赖或用户决策

`(reason)` 是必需的，必须简要说明做了什么或原因。

**追加规则：**
- 始终追加——从不覆盖或替换之前的条目
- 日期和时间是命令执行时的巴基斯坦标准时间（PKT，UTC+5）；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本来自 agent 发现结果
- 每个条目由 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的项**——省略 NONE 优先级的项

---

## Phase 2.6: 更新最后更新徽章

**此阶段是强制性的——必须在 Phase 2.5 之后、呈现报告之前立即执行。**

更新 `README.md` 顶部（第 3 行）的"最后更新"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格编码为 `%20`，逗号编码为 `%2C`），并替换徽章中的日期部分。

**不要在 changelog 或报告中将徽章更新记录为操作项。**

---

## Phase 2.7: 验证所有 CONCEPTS URL

**此阶段是强制性的——必须在 Phase 2.6 之后、呈现报告之前立即执行。**

对于 CONCEPTS 表格中的每个概念：

1. **外部文档 URL**（例如，`https://code.claude.com/docs/en/skills`）：使用 WebFetch 获取每个 URL 并验证它返回有效页面。标记任何失效或已移动的链接。
2. **本地徽章链接**（例如，`best-practice/claude-commands.md`）：使用 Read 工具验证文件是否存在。标记任何损坏的链接。
3. **实现徽章链接**（例如，`.claude/commands/`）：验证路径是否存在。

在报告中包含 **URL Validation Log**：

```
URL Validation Log:
#  | Concept     | URL Type  | URL                                           | Status | Notes
1  | Commands    | External  | https://code.claude.com/docs/en/skills         | OK     |
2  | Commands    | Badge     | best-practice/claude-commands.md               | OK     |
3  | Sub-Agents  | External  | https://code.claude.com/docs/en/sub-agents     | OK     |
...
```

**如果有任何 URL 损坏**，将它们添加为 HIGH 优先级操作项。

---

## Phase 3: 提供采取行动的选项

在呈现报告后（并确认 changelog 已更新），询问用户：

1. **执行所有操作** — 添加缺失的概念，更新已变更的概念，移除已弃用的概念
2. **执行特定操作** — 用户选择要执行哪些编号的操作
3. **仅保存报告** — 不做任何更改

执行时：
- **缺失的概念**：在 `README.md` 的 CONCEPTS 表格中添加新行，遵循现有格式：
  ```
  | [**Name**](docs-url) | `location` | [![Best Practice](!/tags/best-practice.svg)](...) [![Implemented](!/tags/implemented.svg)](...) [Supplementary Link](...) |
  ```
  第三列是 **仅徽章和链接——永远不要有散文**。仅当对应文件存在时才添加徽章（best-practice、implemented）。如果没有徽章或链接适用，保持列为空（仅 `|  |`）。
- **变化的概念**：更新发生变化的特定列
- **已弃用的概念**：在移除行之前与用户确认
- **损坏的 URL**：将 URL 修复为当前有效的 URL
- **徽章修复**：更新徽章链接到正确的文件路径
- 保持与现有表格一致的字母或逻辑排序
- 在所有操作后，重新验证 CONCEPTS 表格的一致性

---

## 关键规则

1. **并行启动两个 agents** 在单一消息中——从不顺序执行
2. **在生成报告前等待两个 agents** 完成
3. **从不猜测** 版本、URL 或日期——使用来自 agents 的数据
4. **缺失的概念是 HIGH PRIORITY** — CONCEPTS 表格是开发者首先看到的内容
5. **验证每个 URL** — 损坏的链接会降低对整个项目的信任度
6. **不要自动执行** — 始终先呈现报告
7. **始终追加到 changelog** — Phase 2.5 是强制性的。从不跳过。从不覆盖之前的条目。
8. **与之前运行的结果进行比较** — 读取 changelog 中的先前条目，并将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
9. **如果存在验证检查清单则执行** — 读取 verification-checklist.md 并执行每一条规则。如果文件不存在但有需要持久化规则的发现，则创建该文件。
10. **始终更新最后更新徽章** — Phase 2.6 是强制性的。
11. **始终验证所有 CONCEPTS URL** — Phase 2.7 是强制性的。损坏的 URL 是 HIGH 优先级。
12. **提供可直接粘贴的行** — 对于缺失的概念，提供确切的 markdown 表格行，以便逐字执行。
13. **尊重现有表格格式** — 匹配现有行的列结构、徽章模式和链接风格。
14. **描述列仅用于徽章和链接** — 从不使用散文。CONCEPTS 表格（包括 Hot 子表格）的第三列包含徽章（best-practice、implemented、beta）和补充链接（文档、博客文章、相关报告）。从不编写功能说明的散文——功能名称链接到官方文档，那里才是说明应属的位置。如果某行没有徽章或链接，保持单元格为空。
