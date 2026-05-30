# 验证检查清单 — Settings 报告

规则随时间累积。每次 workflow-changelog 运行必须按指定深度执行所有规则。当发现某个已有规则本应捕获（但该规则不存在或过于浅层）的新类型漂移时，在此追加一条新规则。

## 深度级别

| 深度 | 含义 | 示例 |
|-------|---------|---------|
| `exists` | 检查某个章节/表格/文件是否存在 | "报告是否有 Sandbox Settings 表格？" |
| `presence-check` | 检查某个特定条目是否存在或缺失 | "Hook Events 表格中是否有 `ConfigChange` 事件？" |
| `content-match` | 逐字将实际值对照来源进行比较 | "`model` 设置的描述是否与官方文档一致？" |
| `field-level` | 验证每个单独的字段是否都已涵盖 | "官方文档中的每个 settings key 是否出现在正确表格中？" |
| `cross-file` | 同一值必须在多个文件中保持一致 | "CLAUDE.md 的 hooks 部分是否与报告的 hook events 一致？" |

---

## 1. Settings Keys 表格

验证 settings key 表格与官方文档一致性的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 1A | Key 完整性 | 对于官方文档中的每个 settings key，验证其出现在报告中正确的章节表格中 | field-level | settings 文档页面 | 2026-03-05 | 初始检查清单 — 确保没有遗漏新的 settings keys |
| 1B | Key 类型 | 对于表格中的每个 key，验证 Type 列与官方文档一致 | content-match | settings 文档页面 | 2026-03-05 | 初始检查清单 — 类型不匹配会导致用户混淆 |
| 1C | Key 默认值 | 对于每个有默认值的 key，验证 Default 列与官方文档一致 | content-match | settings 文档页面 | 2026-03-05 | 初始检查清单 — 错误默认值会导致意外行为 |
| 1D | Key 描述 | 对于每个 key，验证 Description 列准确反映官方文档中的行为 | content-match | settings 文档页面 | 2026-03-05 | 初始检查清单 — 过期描述会误导用户 |
| 1E | Scope 列 | 对于每个有 Scope 列的 key（MCP、Plugin、Permission 表格），验证 scope 值与官方文档一致（例如 "Managed only"、"Any"、"Project"） | content-match | settings 文档页面 | 2026-03-15 | v2.1.71 发现 `extraKnownMarketplaces` scope 错误（"Any" 应为 "Project"），v2.1.75 发现 `autoMemoryDirectory` scope 限制。缺乏系统性验证 scope 列的规则 |
| 1F | 反向完整性 | 对于报告表格中的每个 key，验证其存在于官方文档中，或明确标注为 "not in official docs — unverified"。必须标注没有官方依据的 keys | field-level | settings 文档页面 + JSON schema | 2026-03-15 | 可疑 keys（`sandbox.ignoreViolations`、`skipWebFetchPreflight` 等）因没有规则检查反向方向（报告中的条目不应存在）而持续被搁置 6 个运行周期 |
| 1G | 边界语义 | 对于在边界值（例如 `0`、空字符串、`null`）处有特殊行为的设置，验证边界行为已被文档化且与官方文档一致 | content-match | settings 文档页面 | 2026-03-15 | v2.1.75 较晚才捕获 `cleanupPeriodDays` 零值行为；v2.1.76 补充了 "hooks receive empty transcript_path" 的细节。边界用例验证不足 |
| 1H | 文件作用域检查 | 对于报告中列为 `settings.json` key（特别是 Display Settings）的每个 key，验证其确实属于 `settings.json`，而非 `~/.claude.json` 独有的偏好。官方文档将 "Available settings"（settings.json）与 "Global config settings"（~/.claude.json）分开。key 放错作用域会误导用户并可能导致 schema 验证错误 | content-match | settings 文档页面中 "Available settings" 与 "Global config settings" 章节 | 2026-03-18 | v2.1.78 发现 `showTurnDuration` 和 `terminalProgressBarEnabled` 被列为 Display Settings 中的 settings.json keys，但官方文档明确指出它们属于 `~/.claude.json`，且 "将它们添加到 settings.json 会触发 schema 验证错误。" 没有规则来验证文件作用域 |

---

## 2. Settings 层级

验证 settings 层级表格的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 2A | 优先级层级 | 验证层级表格中的所有优先级级别与官方文档一致（5 级链 + managed policy） | field-level | settings 文档页面 | 2026-03-05 | 初始检查清单 — 优先级错误会导致覆盖混乱 |
| 2B | 文件位置 | 对于每个优先级级别，验证文件路径与官方文档一致 | content-match | settings 文档页面 | 2026-03-05 | 初始检查清单 — 路径错误会导致设置被忽略 |
| 2C | 合并语义 | 验证数组/对象合并行为描述（例如 "concatenated and deduplicated"）与官方文档措辞完全一致 | content-match | settings 文档页面 | 2026-03-15 | v2.1.75 发现 "merged" 改为 "concatenated and deduplicated"。没有规则检查合并行为的措辞 |
| 2D | Managed 内部结构 | 验证 managed 层级的交付方法（server-managed、MDM、registry、file）和内部优先级顺序与官方文档一致。验证特定平台的文件路径和弃用说明 | field-level | settings 文档页面 | 2026-03-15 | v2.1.75 重构了 managed 层级，新增内部优先级和 Windows 路径弃用说明。这些子细节没有专用规则 |

---

## 3. 权限

验证权限配置准确性的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 3A | 权限模式 | 验证表格中的所有权限模式与官方文档一致 | field-level | settings 文档页面 | 2026-03-05 | 初始检查清单 — 遗漏模式会限制用户选项 |
| 3B | 工具语法模式 | 验证所有工具权限语法模式和示例与官方文档一致 | content-match | settings 文档页面 | 2026-03-05 | 初始检查清单 — 错误语法会导致权限失败 |
| 3C | 双向模式检查 | 验证报告中的每个权限模式存在于官方文档中，且官方文档中的每个模式也存在于报告中。报告中存在但文档中不存在的模式必须标注 "unverified" | field-level | settings + permissions 文档页面 | 2026-03-15 | v2.1.74 发现报告中有 `askEdits`/`viewOnly` 但官方文档中没有 — 它们自第一个运行周期起就未被验证。单向检查（文档→报告）遗漏了 3 个运行周期 |
| 3D | 评估语义 | 验证权限评估顺序（deny-first）、远程环境限制和路径前缀解析含义（`//`、`~/`、`/`、`./`）已被文档化且与官方文档一致 | content-match | permissions 文档页面 | 2026-03-15 | v2.1.75 发现缺失评估顺序；v2.1.76 发现缺失路径前缀模式。语义行为规则没有专用检查 |

---

## 4. Hooks（已重定向）

Hook 分析已从本 workflow 中排除。Hooks 由 [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 仓库维护。仅需验证重定向链接仍然有效。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 4A | Hooks 重定向 | 验证报告中的 hooks 部分包含指向 claude-code-hooks 仓库的有效重定向链接 | exists | 报告文件 | 2026-03-05 | Hooks 已外部化到专用仓库 — 仅检查重定向链接的有效性 |

---

## 5. 环境变量

验证环境变量完整性和所属边界的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 5A | 环境变量完整性 | 验证官方文档中所有可通过 `env` 配置的环境变量都出现在报告中 | field-level | settings 文档页面 | 2026-03-05 | 初始检查清单 — 缺失环境变量会限制用户配置选项 |
| 5B | 所属边界 | 验证 `best-practice/claude-cli-startup-flags.md` 中的环境变量没有重复出现在 settings 报告中，反之亦然 | cross-file | claude-cli-startup-flags.md 与 settings 报告 | 2026-03-05 | 初始检查清单 — 环境变量重构将变量拆分到两个文件中，必须防止重复 |
| 5C | 环境变量描述 | 对于表格中的每个环境变量，验证描述（格式、取值、行为）与官方 /en/env-vars 页面一致 | content-match | env-vars 文档页面 | 2026-03-15 | v2.1.74 发现 `ANTHROPIC_CUSTOM_HEADERS` 被描述为 "JSON string" 而非 "Name: Value format, newline-separated"。规则 5A 仅检查存在性，未检查描述准确性 |
| 5D | 反向环境变量检查 | 对于报告表格中的每个环境变量，验证其存在于官方 /en/env-vars 页面中，或明确标注为 "not in official docs — unverified" | field-level | env-vars 文档页面 | 2026-03-15 | v2.1.76 发现报告中有 7 个环境变量没有官方依据。如果没有反向检查，未文档化的变量会悄然累积 |

---

## 6. 示例

验证示例准确性的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 6A | 快速参考示例 | 验证 Quick Reference 完整示例使用有效的当前 settings、正确的语法和真实的取值 | content-match | settings 文档页面 | 2026-03-05 | 初始检查清单 — 示例必须展示当前最佳实践 |
| 6B | 示例 URL 验证 | 验证 JSON 示例代码块中嵌入的 URL（例如 `$schema`、API 端点）能正确解析并使用当前域名 | exists | HTTP 响应 | 2026-03-15 | v2.1.74 发现 `$schema` URL 使用了错误的域名（`www.schemastore.org` 而非 `json.schemastore.org`）。规则 9B 仅检查 markdown 链接，未覆盖代码块内的 URL |

---

## 7. 跨文件一致性

验证报告与其他仓库文件一致性的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 7A | CLAUDE.md 同步 | 验证 CLAUDE.md 的 Configuration Hierarchy 和 Hooks System 部分与报告一致 | cross-file | CLAUDE.md 与报告对比 | 2026-03-05 | 初始检查清单 — CLAUDE.md 可能与报告产生漂移 |

---

## 8. 流程

关于 workflow 验证过程本身的元规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 8A | 来源可信度防护 | 仅在得到官方来源（settings 文档页面、CLI 参考页面、GitHub changelog）确认时，才将条目标记为漂移。第三方博客来源可能已过时或有误 — 仅将其用作线索，在标记前需对照官方文档验证 | content-match | 仅官方文档 | 2026-03-05 | 从 subagents workflow 采纳 — 防止来自博客来源的误报 |

---

## 10. 版本元数据与可疑 Key 生命周期

验证报告元数据准确性并防止未解决条目无限累积的元规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 10A | 版本元数据 | 验证报告的版本徽章、标题 settings 计数和环境变量计数反映实际审计的版本以及当前表格行数 | content-match | 报告文件内部一致性 | 2026-03-15 | v2.1.71 发现版本徽章不匹配；v2.1.69 发现标题计数错误。没有规则验证这些元字段 |
| 10B | 可疑 Key 升级 | 连续 5 次运行处于 ON HOLD 状态后，可疑 keys 必须被解决：(a) 通过 JSON schema 确认并标注为 "in JSON schema, not on official page"，或 (b) 从报告中移除。报告每个可疑 key 的运行次数 | exists | changelog 历史 | 2026-03-15 | 可疑 keys（`sandbox.ignoreViolations`、`skipWebFetchPreflight` 等）因缺乏解决机制而跨 6 个运行周期持续搁置。无限累积不提供任何价值 |
| 10C | 双向完整性 | 通用元规则：报告中的每个 settings key、权限模式和环境变量必须可追溯至官方来源，或明确标注为 "unverified"。这是规则 1A/3A/5A 的反向。是 1F、3C、5D 的超集 | field-level | 官方文档与报告对比 | 2026-03-15 | 从研究中总结出的跨领域规则：由于仅存在 docs→report 检查，6 个条目被较晚发现。反向方向（report→docs）可捕获孤立条目 |

---

## 9. 超链接

验证报告中所有超链接有效性的规则。

| # | 类别 | 检查项 | 深度 | 对照来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 9A | 本地文件链接 | 验证所有相对文件链接指向存在的文件 | exists | 本地文件系统 | 2026-03-05 | 初始检查清单 — 文件移动可能破坏相对链接 |
| 9B | 外部 URL 链接 | 验证所有外部 URL 返回有效页面（非 404 或错误） | exists | HTTP 响应 | 2026-03-05 | 初始检查清单 — 外部文档页面可能被重构或移除 |
| 9C | 锚点链接 | 验证所有内部锚点链接指向同一文件中存在的标题 | exists | 文件标题 | 2026-03-05 | 初始检查清单 — 章节重命名可能破坏锚点链接 |
