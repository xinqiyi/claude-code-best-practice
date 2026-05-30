---
description: 跟踪 Claude Code 设置报告的变更并发现需要更新的内容
argument-hint: [要检查的版本数量，默认 10]
---

# 工作流变更日志 — 设置报告

你是 claude-code-best-practice 项目的一名协调员。你的工作是并行启动两个研究 agent，等待它们的结果，合并发现，并提交一份关于 **Settings Reference** 报告（`best-practice/claude-settings.md`）偏差的统一报告。

**要检查的版本数：** `$ARGUMENTS`（若为空或非数字，默认 10）

这是一个**先读取再报告**的工作流。启动 agent，合并结果，生成报告。只有在用户批准后才采取行动。

---

## 阶段 0：并行启动两个 Agent

**立即**使用 Task 工具**在同一条消息中**同时启动两个 agent（并行启动）：

### Agent 1：workflow-claude-settings-agent

使用 `subagent_type: "workflow-claude-settings-agent"` 启动。向其提供以下提示：

> 针对 claude-code-best-practice 项目调研 settings 报告的偏差。检查最近 $ARGUMENTS 个版本（默认：10）。
>
> 获取以下 3 个外部来源：
> 1. 设置文档：https://code.claude.com/docs/en/settings
> 2. CLI 参考：https://code.claude.com/docs/en/cli-reference
> 3. 变更日志：https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
>
> 然后读取本地报告文件（`best-practice/claude-settings.md`）和 CLAUDE.md 文件。分析官方文档中关于 settings key、permission 语法、hook 事件、MCP 配置、sandbox 选项、plugin 设置、model 别名、显示设置和环境变量的内容与我们报告的差异。返回结构化发现报告，涵盖缺失的 settings、已更改的类型/默认值、新增的 settings、已弃用的 settings、permission 语法变更、hook 事件变更、MCP 设置变更、sandbox 设置变更、环境变量完整性、示例准确性、settings 层级准确性以及来源有效性。

### Agent 2：claude-code-guide

使用 `subagent_type: "claude-code-guide"` 启动。向其提供以下提示：

> 调研最新的 Claude Code 设置系统。我需要你查找：
> 1. 当前所有受支持的 settings.json key 的完整列表，包含类型、默认值和描述
> 2. 最近 Claude Code 版本中引入的任何新 settings key
> 3. 现有设置行为的变化（例如新的 permission 模式、新的 hook 事件、新的 sandbox 选项）
> 4. 设置层级的变化（新的优先级级别、新的文件位置）
> 5. permission 语法的变化（新的 tool 模式、新的通配符行为）
> 6. 新的 hook 事件或 hook 配置结构的变更
> 7. MCP 服务器配置的变化（新的匹配字段、新的设置）
> 8. sandbox 设置的变化（新的网络选项、新的命令）
> 9. plugin 配置的变化（新的字段、新的 marketplace 选项）
> 10. 环境变量的变化（新增变量、弃用变量、行为变更）
> 11. model 别名或 model 配置的变化
> 12. 显示/UX 设置的变化（状态行、加载动画、进度条）
> 13. 任何 settings key 的弃用或移除
>
> 请务必详尽——搜索网络、获取文档，并为所有发现提供具体的版本号和详细信息。

两个 agent 独立运行，将分别返回各自的发现。

---

## 阶段 0.5：读取验证检查清单

**在 agent 运行期间**，读取 `changelog/best-practice/claude-settings/verification-checklist.md`。该文件包含累积的验证规则——每条规则指定要检查什么、检查深度以及对照哪个来源。每条规则都必须在阶段 2 中执行。该检查清单是项目用于偏差检测的回归测试套件。

---

## 阶段 1：读取先前的变更日志条目

**在合并发现之前**，读取文件 `changelog/best-practice/claude-settings/changelog.md`，获取最近 25 条变更日志条目。每条条目由 `---` 分隔。解析先前条目中的优先操作，以便与当前发现进行对比。这可以让你识别：
- **重复项**——之前出现且仍未解决的问题
- **新解决项**——之前运行中存在问题但现已修复的项
- **新项**——本次运行中首次出现的问题

---

## 阶段 2：合并发现并生成报告

**等待两个 agent 完成。** 当你获得以下内容后：
- **workflow-claude-settings-agent 的发现**——包含本地文件读取、外部文档获取和偏差检测的详细报告分析
- **claude-code-guide 的发现**——关于最新 Claude Code 设置功能和变更的独立研究

交叉对照两者。专用 agent 提供报告特定的偏差分析，而 claude-code-guide agent 可能会发现它遗漏的内容（例如非常近期的变更、未记录的功能或来自网络搜索的上下文）。标记两者之间的任何矛盾，供用户解决。

**执行验证检查清单：** 对于 `changelog/best-practice/claude-settings/verification-checklist.md` 中的每条规则，使用 agent 发现作为源数据，按指定深度执行检查。在报告中包含一个 **验证日志** 部分，显示每条规则的结果：

```
验证日志：
规则 # | 类别              | 深度         | 结果   | 备注
1      | Settings Keys     | field-level   | PASS   | 所有 key 均匹配
2      | Permission Syntax | content-match | FAIL   | 新增 tool 模式
...
```

**根据需要更新检查清单：** 如果某项发现揭示了一种新的偏差类型，而现有检查清单规则未覆盖（或覆盖深度不足），则在 `changelog/best-practice/claude-settings/verification-checklist.md` 中追加一条新规则。规则必须包含：类别、检查内容、深度级别、对照来源、添加日期以及来源（是什么错误触发了此规则）。不要为不会重复发生的单次问题添加规则。

同时将当前发现与先前的变更日志条目（来自阶段 1）进行比较。对于每个优先操作，将其标记为：
- `NEW`——该问题首次出现
- `RECURRING`——在之前的运行中出现过且仍未解决（包含首次出现的运行日期）
- `RESOLVED`——在之前的运行中出现过但现已修复（包含解决日期）

生成一份包含以下部分的结构化报告：

1. **新的 Settings Key**——官方文档中存在但报告中缺失的 key，附带引入版本
2. **已更改的设置行为**——类型、默认值或描述发生变更的设置
3. **已弃用/移除的设置**——报告中存在但官方文档中不再存在的设置
4. **Permission 语法变更**——新的 tool 模式、通配符行为或 permission 模式变更
5. **MCP 设置变更**——新的 MCP 配置 key、匹配行为或服务器设置
6. **Sandbox 设置变更**——新的 sandbox 选项、网络设置或命令排除
7. **Plugin 设置变更**——新的 plugin 配置 key 或 marketplace 选项
8. **Model 配置变更**——新的 model 别名、effort 级别或 model 环境变量
9. **显示与 UX 变更**——新的状态行字段、加载动画选项或显示设置
10. **环境变量完整性**——官方文档中存在但报告中缺失的变量，或报告中存在但不再被文档记录的变量
11. **设置层级准确性**——验证优先级级别、文件位置和覆盖行为
12. **示例准确性**——快速参考中的完整示例是否反映当前设置
13. **来源准确性**——验证所有来源链接是否有效并指向正确的文档
14. **claude-code-guide Agent 发现**——该 agent 提供的独特见解，但未被专用 agent 捕获的内容。仅包含新增信息的发现。如果两个 agent 之间存在矛盾，标记供用户解决。不要列出"已确认一致"的内容。

> **注意：** 与 Hook 相关的分析（事件、属性、匹配器、退出码、HTTP hook、hook 环境变量）**不包含**在此工作流中。Hook 在 [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 仓库中进行维护。

以一份按优先级排序的 **操作项** 汇总表结束。每个项目必须包含一个 `Status` 列，显示 `NEW`、`RECURRING（首次出现：<日期>）` 或 `RESOLVED`：

```
优先操作：
#  | 类型                  | 操作                                    | 状态
1  | New Setting           | 在 <section> 表中添加 <key>              | NEW
2  | Changed Behavior      | 更新 <key> 的描述                        | NEW
3  | Deprecated Setting    | 从表中移除 <key>                        | RECURRING（首次出现：2026-03-05）
4  | Permission Syntax     | 添加新的 tool 模式语法                   | NEW
5  | Env Variable          | 在环境变量表中添加 <var>                 | NEW
7  | Example Update        | 更新快速参考示例                         | NEW
```

同时包含一个 **自上次运行以来已解决** 部分，列出上次运行中存在但已不再成为问题的项。

---

## 阶段 2.5：将摘要追加到变更日志

**此阶段为强制阶段——在向用户展示报告之前必须执行。**

读取现有的 `changelog/best-practice/claude-settings/changelog.md` 文件，然后在末尾**追加**（不要覆盖）一条新条目。条目格式必须为：

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | 优先级  | 类型  | 操作  | 状态  |
|---|---------|-------|-------|-------|
| 1 | HIGH/MED/LOW | <类型> | <操作描述> | <状态> |
| ... | ... | ... | ... | ... |
```

**状态格式——必须使用以下三种格式之一：**
- `COMPLETE（原因）`——操作已执行并成功解决
- `INVALID（原因）`——发现不正确、不适用或有意为之
- `ON HOLD（原因）`——操作推迟，等待外部依赖或用户决策

`（原因）` 为必填项，必须简要说明所做的事项或原因。

**追加规则：**
- 始终追加——绝不覆盖或替换先前的条目
- 日期和时间是命令在巴基斯坦标准时间（PKT，UTC+5）执行的时间；通过运行 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取。版本来自 agent 发现
- 如果 `changelog/best-practice/claude-settings/changelog.md` 不存在或为空，先创建状态图例表（参见文件顶部），然后创建第一个条目
- 每个条目由 `---` 分隔
- **仅包含 HIGH、MEDIUM 或 LOW 优先级的项**——省略 NONE 优先级的项（无需操作的事项）

---

## 阶段 2.6：更新最后更新徽章

**此阶段为强制阶段——必须在阶段 2.5 之后、展示报告之前立即执行。**

更新 `best-practice/claude-settings.md` 顶部的"最后更新"徽章。运行 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码（空格转 `%20`，逗号转 `%2C`），并替换徽章中的日期部分。如果 Claude Code 版本已变更，也同时更新徽章中的版本号。

**不要将徽章更新记录为变更日志或报告中的操作项。** 徽章同步是每次运行的常规部分，而非发现项。

---

## 阶段 2.7：验证所有超链接

**此阶段为强制阶段——必须在阶段 2.6 之后、展示报告之前立即执行。**

扫描 `best-practice/claude-settings.md` 中的每个超链接（包括 markdown `[text](url)` 和内联 URL）。对于每个链接：

1. **本地文件链接**（相对路径）：使用 Read 工具验证文件在解析路径下是否存在。标记任何损坏的链接。
2. **外部 URL**（例如 `https://code.claude.com/docs/en/settings`）：使用 WebFetch 获取每个 URL 并验证其返回有效页面（非 404 或重定向到错误页面）。标记任何失效或移动的链接。
3. **锚点链接**（例如 `#section-name`）：验证目标标题在同一文件中存在。

在报告中包含一个 **超链接验证日志**：

```
超链接验证日志：
#  | 类型     | 链接                                          | 状态   | 备注
1  | Local    | ../                                            | OK     |
2  | External | https://code.claude.com/docs/en/settings       | OK     |
3  | External | https://www.schemastore.org/claude-code-settings.json | BROKEN | 404
...
```

**如果任何链接损坏**，将它们作为 HIGH 优先级操作项添加到报告中。损坏的链接会降低报告的可用性，必须在任何其他更改之前修复。

---

## 阶段 3：提供执行操作的选项

在展示报告（并确认变更日志和徽章均已更新）后，询问用户：

1. **执行所有操作**——处理所有事项（添加缺失的设置、更新描述、修复示例）
2. **执行特定操作**——用户选择要执行的编号
3. **仅保存报告**——不做任何更改

执行时：
- **新设置**：将正确的类型、默认值和描述添加到相应部分表格
- **已变更的行为**：更新表格中的设置描述或默认值
- **已弃用的设置**：在移除前与用户确认
- **Permission 语法变更**：使用新模式更新 Permission Syntax 表格
- **MCP 设置变更**：更新 MCP Settings 部分
- **Sandbox 设置变更**：更新 Sandbox Settings 部分
- **Plugin 设置变更**：更新 Plugin Settings 部分
- **Model 变更**：更新 Model Configuration 部分
- **显示变更**：更新 Display & UX 部分
- **环境变量变更**：在 Environment Variables 部分添加/更新/移除变量
- **设置层级变更**：更新 Settings Hierarchy 表格
- **示例更新**：更新快速参考中的完整示例以反映当前设置
- **损坏的链接**：修复或替换损坏的 URL
- 在所有操作完成后，重新运行验证以确认一致性

---

## 关键规则

1. **并行启动两个 Agent** 在单条消息中——绝不要串行
2. **等待两个 agent 完成** 后再生成报告
3. **绝不要猜测** 版本或日期——使用 agent 提供的数据
4. **新的 settings key 为高优先级**——它们需要更新表格和示例
5. **交叉对照设置数量**——每个表格中的设置数量必须与官方文档匹配
6. **不要自动执行**——始终先展示报告
7. **始终追加到变更日志**——阶段 2.5 为强制要求。绝不要跳过。绝不要覆盖先前的条目。
8. **与先前运行进行比较**——读取变更日志中最近 25 条条目，并将每个操作项标记为 NEW、RECURRING 或 RESOLVED。
9. **始终执行验证检查清单**——读取 verification-checklist.md 并执行每条规则。在报告中包含验证日志。当发现新类型的偏差时追加新规则。
10. **检查清单规则仅可追加**——绝不要移除或削弱现有规则。只能添加新规则或升级深度级别。
11. **始终更新最后更新徽章**——阶段 2.6 为强制要求。绝不要跳过。
12. **始终验证所有超链接**——阶段 2.7 为强制要求。绝不要跳过。损坏的链接为高优先级。
13. **环境变量分布在两个文件中**——`claude-settings.md` 拥有 `env` 可配置的变量；`claude-cli-startup-flags.md` 拥有仅启动时可用的变量。如果环境变量属于 CLI 文件中，不要标记为缺失。交叉对照 `best-practice/claude-cli-startup-flags.md` 以验证所有权边界。
14. **验证设置层级**——5 级覆盖链加上 managed 策略层必须与官方文档完全一致。
