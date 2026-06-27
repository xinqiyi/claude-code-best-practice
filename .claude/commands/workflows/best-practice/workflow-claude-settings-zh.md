---
description: 跟踪 Claude Code settings 报告的变更，发现需要更新的内容
argument-hint: [要检查的版本数，默认 10]
---

# Workflow Changelog — Settings 报告

你是 claude-code-best-practice 项目的协调者。你的工作是并行启动两个研究 agent，等待它们的结果，合并发现结果，并呈现关于 **Settings 参考报告**（`best-practice/claude-settings.md`）漂移情况的统一报告。

**要检查的版本数：** `$ARGUMENTS`（默认为空或非数字时为 10）

这是一个**先读后报**工作流。启动 agents，合并结果，生成报告。仅在用户批准后才执行操作。

---

## 阶段 0：并行启动两个 Agent

**立即**使用 Task tool 在**同一条消息中**启动两个 agent（并行启动）：

### Agent 1：workflow-claude-settings-agent

使用 `subagent_type: "workflow-claude-settings-agent"` 启动。给它以下 prompt：

> 研究 claude-code-best-practice 项目，检测 settings 报告的漂移情况。检查最后 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 3 个外部数据源：
> 1. Settings 文档：https://code.claude.com/docs/en/settings
> 2. CLI 参考文档：https://code.claude.com/docs/en/cli-reference
> 3. Changelog：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告文件（`best-practice/claude-settings.md`）和 CLAUDE.md 文件。分析官方文档中关于 settings keys、permission 语法、hook events、MCP 配置、sandbox 选项、plugin 设置、model aliases、display 设置和 environment variables 的内容与我们的报告所记录内容之间的差异。返回结构化的发现结果报告，涵盖缺失的 settings、变更的类型/默认值、新增 settings、已弃用 settings、permission 语法变更、hook event 变更、MCP setting 变更、sandbox setting 变更、environment variable 完整性、示例准确性、settings hierarchy 准确性和数据源有效性。

### Agent 2：claude-code-guide

使用 `subagent_type: "claude-code-guide"` 启动。给它以下 prompt：

> 研究最新的 Claude Code settings 系统。我需要你查找：
> 1. 当前支持的所有 settings.json keys 的完整列表，包含类型、默认值和描述
> 2. 近期 Claude Code 版本中引入的任何新 settings keys
> 3. 现有 settings 行为的变化（例如新的 permission modes、新的 hook events、新的 sandbox 选项）
> 4. Settings hierarchy 的变化（新的优先级层级、新的文件位置）
> 5. Permission 语法的变化（新的 tool patterns、新的通配符行为）
> 6. 新的 hook events 或 hook 配置结构的变化
> 7. MCP server 配置的变化（新的匹配字段、新的 settings）
> 8. Sandbox settings 的变化（新的网络选项、新的命令）
> 9. Plugin 配置的变化（新字段、新的 marketplace 选项）
> 10. Environment variables 的变化（新变量、已弃用变量、变更的行为）
> 11. Model aliases 或 model 配置的变化
> 12. Display/UX settings 的变化（status line、spinners、progress bars）
> 13. Settings keys 的任何弃用或移除
>
> 请深入研究——搜索网络，获取文档，并为发现的每项内容提供具体的版本号和详细信息。

两个 agent 独立运行并返回各自的发现结果。

---

## 阶段 0.5：读取验证 Checklist

**在 agents 运行期间**，读取 `changelog/best-practice/claude-settings/verification-checklist.md`。此文件包含累积的验证规则——每条规则指定检查什么、检查深度及对照的数据源。每条规则必须在阶段 2 期间执行。该 checklist 是项目的漂移检测回归测试套件。

---

## 阶段 1：读取之前的 Changelog 条目

**在合并发现结果之前**，读取 `changelog/best-practice/claude-settings/changelog.md` 文件以获取最后 25 个 changelog 条目。每个条目之间用 `---` 分隔。解析之前条目中的 priority actions，以便与当前发现结果进行比较。这可以识别：
- **重复出现的项目** — 之前出现过且仍未解决的问题
- **新近解决的项目** — 之前运行中的问题现已修复
- **新出现的项目** — 本次运行中首次出现的问题

---

## 阶段 2：合并发现结果并生成报告

**等待两个 agent 完成。** 一旦你获得：
- **workflow-claude-settings-agent 的发现结果** — 详细的报告分析，包含本地文件读取、外部文档获取和漂移检测
- **claude-code-guide 的发现结果** — 对最新 Claude Code settings 功能和变更的独立研究

交叉验证两者的结果。专用 agent 提供报告特定性的漂移分析，而 claude-code-guide agent 可能发现它遗漏的内容（例如非常近期的变更、未记录的功能或来自网络搜索的上下文）。标记两者之间的任何矛盾，供用户解决。

**执行验证 checklist：** 对 `changelog/best-practice/claude-settings/verification-checklist.md` 中的每条规则，使用 agent 发现结果作为源数据，按指定深度执行检查。在报告中包含一个 **Verification Log** 部分，显示每条规则的结果：

```
Verification Log:
Rule # | Category              | Depth         | Result | Notes
1      | Settings Keys         | field-level   | PASS   | All keys match
2      | Permission Syntax     | content-match | FAIL   | New tool pattern added
...
```

**必要时更新 checklist：** 如果某个发现揭示了一种新的漂移类型，而现有 checklist 规则未覆盖（或覆盖深度不足），请向 `changelog/best-practice/claude-settings/verification-checklist.md` 追加一条新规则。规则必须包含：category、what to check、depth level、what source to compare against、date added，以及 origin（导致此规则的具体错误）。不要为不会再次出现的一次性问题添加规则。

同时将当前发现结果与之前的 changelog 条目（来自阶段 1）进行比较。对每个 priority action，标记为：
- `NEW` — 此问题首次出现
- `RECURRING` — 在之前运行中出现过且仍未解决（包含首次出现的运行日期）
- `RESOLVED` — 在之前运行中出现过但现已修复（包含解决日期）

生成结构化报告，包含以下部分：

1. **新增 Settings Keys** — 在官方文档中存在但报告中缺失的 keys，包含引入版本
2. **变更的 Settings 行为** — 类型、默认值或描述已发生变化的 settings
3. **已弃用/移除的 Settings** — 在报告中存在但官方文档中不再包含的 settings
4. **Permission 语法变更** — 新的 tool patterns、通配符行为或 permission mode 变更
5. **MCP Setting 变更** — 新的 MCP 配置 keys、匹配行为或 server settings
6. **Sandbox Setting 变更** — 新的 sandbox 选项、网络设置或命令排除项
7. **Plugin Setting 变更** — 新的 plugin 配置 keys 或 marketplace 选项
8. **Model 配置变更** — 新的 model aliases、effort levels 或 model environment variables
9. **Display & UX 变更** — 新的 status line 字段、spinner 选项或 display settings
10. **Environment Variable 完整性** — 官方文档中存在但报告中缺失的变量，或报告中存在但不再有文档的变量
11. **Settings Hierarchy 准确性** — 验证优先级层级、文件位置和覆盖行为
12. **示例准确性** — Quick Reference 完整示例是否反映当前 settings
13. **数据源准确性** — 验证所有数据源链接有效并指向正确的文档
14. **claude-code-guide Agent 发现结果** — agent 提供的未被专用 agent 捕获的独到见解。仅包含添加新信息的发现结果。如果两个 agent 之间存在矛盾，标记出来供用户解决。不要列出"确认一致"的内容。

> **注意：** Hook 相关分析（events、properties、matchers、exit codes、HTTP hooks、hook env vars）**不包含**在此工作流中。Hooks 在 [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 仓库中维护。

最后附上带优先级的 **Action Items** 摘要表格。每个项目必须包含 `Status` 列，显示 `NEW`、`RECURRING (first seen: <date>)` 或 `RESOLVED`：

```
Priority Actions:
#  | Type                  | Action                                    | Status
1  | New Setting           | Add <key> to <section> table               | NEW
2  | Changed Behavior      | Update <key> description                   | NEW
3  | Deprecated Setting    | Remove <key> from table                    | RECURRING (first seen: 2026-03-05)
4  | Permission Syntax     | Add new tool pattern syntax                | NEW
5  | Env Variable          | Add <var> to environment variables table   | NEW
7  | Example Update        | Update Quick Reference example             | NEW
```

同时包含一个 **Resolved Since Last Run** 部分，列出之前运行中已不再存在的问题。

---

## 阶段 2.5：将摘要追加到 Changelog

**此阶段是强制性的——始终在向用户呈现报告之前执行。**

读取现有的 `changelog/best-practice/claude-settings/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一个新条目。条目格式必须严格如下：

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
- 如果 `changelog/best-practice/claude-settings/changelog.md` 不存在或为空，创建一个带有 Status Legend 表格的文件（参见文件顶部），然后添加第一个条目
- 每个条目之间用 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的项目**——省略 NONE 优先级的项目（无需行动的事项）

---

## 阶段 2.6：更新 Last Updated Badge

**此阶段是强制性的——始终在阶段 2.5 之后、向用户呈现报告之前立即执行。**

更新 `best-practice/claude-settings.md` 顶部的 "Last Updated" badge。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，对其进行 URL 编码（空格转 `%20`，逗号转 `%2C`），替换 badge 中的日期部分。如果 Claude Code 版本有变化，同时更新 badge 中的版本号。

**不要将 badge 更新记录为 changelog 或报告中的 action item。** Badge 同步是每次运行的例行操作，不是发现结果。

---

## 阶段 2.7：验证所有超链接

**此阶段是强制性的——始终在阶段 2.6 之后、向用户呈现报告之前执行。**

扫描 `best-practice/claude-settings.md` 中的每个超链接（包括 markdown `[text](url)` 和内联 URL）。对每个链接：

1. **本地文件链接**（相对路径）：使用 Read tool 验证文件在解析后的路径中存在。标记任何失效链接。
2. **外部 URL**（例如 `https://code.claude.com/docs/en/settings`）：使用 WebFetch 获取每个 URL，验证返回的是有效页面（不是 404 或重定向到错误页面）。标记任何失效或移动的链接。
3. **锚点链接**（例如 `#section-name`）：验证目标标题在同一文件中存在。

在报告中包含一个 **Hyperlink Validation Log**：

```
Hyperlink Validation Log:
#  | Type     | Link                                          | Status | Notes
1  | Local    | ../                                            | OK     |
2  | External | https://code.claude.com/docs/en/settings       | OK     |
3  | External | https://www.schemastore.org/claude-code-settings.json | BROKEN | 404
...
```

**如果有任何链接失效**，将其添加为报告中的 HIGH priority action items。失效链接会降低报告的可用性，必须优先于任何其他变更进行修复。

---

## 阶段 3：提供执行选项

在呈现报告之后（并确认 changelog 和 badge 均已更新），询问用户：

1. **Execute all actions** — 处理所有事项（添加缺失的 settings、更新描述、修复示例）
2. **Execute specific actions** — 用户选择要执行的编号
3. **Just save the report** — 不做变更

执行时：
- **新增 settings**：添加到相应部分的表格中，包含正确的类型、默认值和描述
- **变更的行为**：更新表格中的 setting 描述或默认值
- **已弃用 settings**：在移除前与用户确认
- **Permission 语法变更**：用新的 patterns 更新 Permission Syntax 表格
- **MCP setting 变更**：更新 MCP Settings 部分
- **Sandbox setting 变更**：更新 Sandbox Settings 部分
- **Plugin setting 变更**：更新 Plugin Settings 部分
- **Model 变更**：更新 Model Configuration 部分
- **Display 变更**：更新 Display & UX 部分
- **Environment variable 变更**：在 Environment Variables 部分新增/更新/移除变量
- **Settings hierarchy 变更**：更新 Settings Hierarchy 表格
- **示例更新**：更新 Quick Reference 完整示例以反映当前 settings
- **失效链接**：修复或替换失效的 URL
- 完成所有操作后，重新运行验证以确认一致性

---

## 关键规则

1. **在同一条消息中并行启动两个 agent** — 绝不可顺序启动
2. **在生成报告之前等待两个 agent 都完成**
3. **绝不猜测**版本或日期——使用 agents 的数据
4. **新增 settings keys 是高优先级** — 它们需要表格和示例更新
5. **交叉验证 setting 计数** — 每个表格中的 settings 数量必须与官方文档一致
6. **不要自动执行** — 始终先呈现报告
7. **始终追加到 changelog** — 阶段 2.5 是强制性的。绝不可跳过。绝不可覆盖之前的条目。
8. **与之前的运行结果比较** — 读取 changelog 的最后 25 个条目，将每个 action item 标记为 NEW、RECURRING 或 RESOLVED。
9. **始终执行验证 checklist** — 读取 verification-checklist.md 并执行每条规则。在报告中包含 Verification Log。当发现新的漂移类型时追加新规则。
10. **Checklist 规则是仅追加的** — 绝不可移除或弱化现有规则。仅添加新规则或提升检查深度。
11. **始终更新 Last Updated badge** — 阶段 2.6 是强制性的。绝不可跳过。
12. **始终验证所有超链接** — 阶段 2.7 是强制性的。绝不可跳过。失效链接是 HIGH priority。
13. **Environment variables 分布在两个文件中** — `claude-settings.md` 负责 `env` 可配置的变量；`claude-cli-startup-flags.md` 负责仅启动时的变量。不要将属于 CLI 文件的 env vars 标记为缺失。交叉参考 `best-practice/claude-cli-startup-flags.md` 以验证归属边界。
14. **验证 settings hierarchy** — 5 层覆盖链加上 managed policy 层必须与官方文档完全一致。
