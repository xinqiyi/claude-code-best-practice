# Verification Checklist — Subagents Report

规则随时间的推移而积累。每次 workflow-changelog 运行必须按指定深度执行所有规则。当发现一种新类型的 drift，且本应被现有规则捕获但该规则不存在或深度不够时，在此追加新规则。

## Depth Levels

| Depth | Meaning | Example |
|-------|---------|---------|
| `exists` | 检查 section/table/file 是否存在 | "报告有 Memory Scopes 表吗？" |
| `presence-check` | 检查特定项是否存在或缺失 | "Frontmatter Fields 表中有 `color` 字段吗？" |
| `content-match` | 逐字比较实际值与来源 | "`model` 字段描述与官方文档匹配吗？" |
| `field-level` | 验证每个字段都被覆盖 | "官方文档中的每个 frontmatter 字段都出现在表中吗？" |
| `cross-file` | 同一值必须在多个文件中匹配 | "CLAUDE.md agent section 与报告的字段列表匹配吗？" |

---

## 1. Frontmatter Fields Table

验证 Frontmatter Fields 表与官方文档一致性的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 1A | Field Completeness | 对于官方文档中的每个 agent frontmatter 字段，验证其出现在报告的 Frontmatter Fields 表中 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始 checklist — 确保不遗漏新字段 |
| 1B | Field Types | 对于表中的每个字段，验证 Type 列与官方文档匹配 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始 checklist — 类型不匹配会导致用户困惑 |
| 1C | Required Status | 对于每个字段，验证 Required 列与官方文档匹配 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始 checklist — 错误的 required 状态导致 agent 损坏 |
| 1D | Field Descriptions | 对于每个字段，验证 Description 列准确反映官方文档行为 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始 checklist — 过时的描述误导用户 |

## 2. Memory Scopes

验证 Memory Scopes 表的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 2A | Scope Completeness | 验证官方文档中的所有 memory scopes 出现在 Memory Scopes 表中 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始 checklist — 可能添加新 scopes |
| 2B | Storage Locations | 对于每个 scope，验证 Storage Location 列与官方文档匹配 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始 checklist — 错误路径导致数据丢失 |

## 3. Examples

验证示例准确性的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 3A | Minimal Example | 验证 minimal example 仅使用 required 字段且语法有效 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始 checklist |
| 3B | Full-Featured Example | 验证 full-featured example 展示了所有可用的 frontmatter 字段 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始 checklist |

## 4. Scope & Priority

验证 scope 和 priority 信息的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 4A | Priority Order | 验证 Scope and Priority 表按正确优先级顺序列出所有 agent 位置 | content-match | sub-agents 参考页面 + CLI 参考页面 | 2026-02-28 | 初始 checklist |
| 4B | Invocation Methods | 验证 invocation methods 表列出了所有调用方法，包括 `--agent`、`--agents`、`/agents`、`claude agents`、Agent tool 和 agent resumption | field-level | CLI 参考页面 + sub-agents 参考页面 | 2026-03-07 | `--agent` CLI flag 在调用表中缺失 |

## 5. Cross-File Consistency

验证报告与其他仓库文件之间一致性的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 5A | CLAUDE.md Sync | 验证 CLAUDE.md 的 Subagent Definition Structure section 列出与报告 Frontmatter Fields 表相同的字段 | cross-file | CLAUDE.md vs report | 2026-02-28 | 初始 checklist |

## 6. Process

关于 workflow verification 过程本身的元规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 6A | Source Credibility Guard | 仅当被官方来源确认时才将项目标记为 drift | content-match | 仅官方文档 | 2026-02-28 | 从 hooks workflow 采纳 |

## 7. Agent Tables

验证 Official Claude Agents 和 Agents in This Repository 表的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 7A | Built-in Agent Completeness | 验证 "Official Claude Agents" 表列出所有 built-in agent types | field-level | sub-agents 参考页面 + changelog | 2026-02-28 | 报告只列了 5 个 built-in agents 中的 3 个 |
| 7B | Repository Agent Completeness | 扫描 `.claude/agents/**/*.md` 并验证每个 agent 文件出现在 "Agents in This Repository" 表中 | field-level | `.claude/agents/**/*.md` 文件 frontmatter | 2026-02-28 | 仓库 agent 是手动维护的 |
| 7C | Repository Agent Links | 验证 "Agents in This Repository" 表中每个 agent 名称有可点击的链接 | exists | 从 `best-practice/` 解析的文件路径 | 2026-02-28 |

## 8. Hyperlinks

验证报告中所有超链接有效的规则。

| # | Category | Check | Depth | Compare Against | Added | Origin |
|---|----------|-------|-------|-----------------|-------|--------|
| 8A | Local File Links | 验证所有相对文件链接解析为存在的文件 | exists | 本地文件系统 | 2026-02-28 |
| 8B | External URL Links | 验证所有外部 URL 返回有效页面 | exists | HTTP response | 2026-02-28 |
| 8C | Cross-File Reference Links | 验证指向其他报告文件的链接解析为存在的文件 | exists | 本地文件系统 | 2026-02-28 |
