# Verification Checklist — Settings Report

规则随时间的推移而积累。每次 workflow-changelog 运行必须按指定深度执行所有规则。

## Depth Levels

| Depth | Meaning | Example |
|-------|---------|---------|
| `exists` | 检查 section/table/file 是否存在 | "报告有 Sandbox Settings 表吗？" |
| `presence-check` | 检查特定项是否存在或缺失 | "Hook Events 表中有 `ConfigChange` 事件吗？" |
| `content-match` | 逐字比较实际值与来源 | "`model` 设置描述与官方文档匹配吗？" |
| `field-level` | 验证每个字段都被覆盖 | "官方文档中的每个 settings key 都出现在正确的表中吗？" |
| `cross-file` | 同一值必须在多个文件中匹配 | "CLAUDE.md hooks section 与报告的 hook events 匹配吗？" |

---

## 1. Settings Keys Tables

验证 settings key 表与官方文档一致性的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 1A | Key Completeness | 对于官方文档中的每个 settings key，验证其出现在报告的正确 section 表中 | field-level | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 1B | Key Types | 对于表中的每个 key，验证 Type 列与官方文档匹配 | content-match | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 1C | Key Defaults | 对于有默认值的每个 key，验证 Default 列与官方文档匹配 | content-match | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 1D | Key Descriptions | 对于每个 key，验证 Description 列准确反映官方文档行为 | content-match | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 1E | Scope Column | 对于有 Scope 列的每个 key，验证 scope 值与官方文档匹配 | content-match | settings 文档页面 | 2026-03-15 | v2.1.71 发现 `extraKnownMarketplaces` scope 错误 |
| 1F | Inverse Completeness | 对于报告表中的每个 key，验证其在官方文档中存在或被显式标记为"未在官方文档中 — unverified" | field-level | settings 文档页面 + JSON schema | 2026-03-15 | 可疑 key 在 6 次运行中一直保持 ON HOLD |
| 1G | Edge-Case Semantics | 对于边界值处有特殊行为的设置，验证边界行为有文档记录并与官方文档匹配 | content-match | settings 文档页面 | 2026-03-15 | v2.1.75 发现 `cleanupPeriodDays` 零值行为 |
| 1H | File Scope Check | 对于报告中列为 `settings.json` key 的每个 key，验证其确实是 `settings.json` key 而非 `~/.claude.json` 专属 | content-match | settings 文档页面 | 2026-03-18 | v2.1.78 发现 `showTurnDuration` 和 `terminalProgressBarEnabled` 被错误地列为 settings.json keys |
| 1I | Skills Settings Keys | 对于官方 settings 页面上的每个 skill 相关的 `settings.json` key，验证其出现在报告中 | field-level | settings 文档页面 | 2026-05-25 | v2.1.150 发现 `maxSkillDescriptionChars` 和 `skillListingBudgetFraction` 缺失 |

## 2. Settings Hierarchy

验证 settings hierarchy 表的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 2A | Priority Levels | 验证 hierarchy 表中所有优先级级别与官方文档匹配 | field-level | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 2B | File Locations | 对于每个优先级级别，验证文件位置路径与官方文档匹配 | content-match | settings 文档页面 | 2026-03-05 | 初始 checklist |

## 3. Permissions

验证权限配置准确性的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 3A | Permission Modes | 验证表中所有权限模式与官方文档匹配 | field-level | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 3B | Tool Syntax Patterns | 验证所有工具权限语法模式和示例与官方文档匹配 | content-match | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 3C | Bidirectional Mode Check | 验证报告中的每个权限模式在官方文档中存在，且官方文档中的每个模式在报告中存在 | field-level | settings + permissions 文档页面 | 2026-03-15 | v2.1.74 发现 `askEdits`/`viewOnly` 在报告中但不在官方文档中 |

## 4. Hooks (已重定向)

## 5. Environment Variables

验证环境变量完整性的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 5A | Env Var Completeness | 验证官方文档中的所有 `env` 可配置环境变量出现在报告中 | field-level | settings 文档页面 | 2026-03-05 | 初始 checklist |
| 5B | Ownership Boundary | 验证不会在 settings 报告中重复 `claude-cli-startup-flags.md` 中的 env vars | cross-file | claude-cli-startup-flags.md vs settings report | 2026-03-05 | 初始 checklist |

## 6-10.（略——字段结构与前述 checklist 相同模式）

---

## 9. Hyperlinks

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 9A | Local File Links | 验证所有相对文件链接解析为存在的文件 | exists | 本地文件系统 | 2026-03-05 |
| 9B | External URL Links | 验证所有外部 URL 返回有效页面 | exists | HTTP response | 2026-03-05 |
| 9C | Anchor Links | 验证所有内部锚点链接指向同一文件内的存在标题 | exists | 文件标题 | 2026-03-05 |
