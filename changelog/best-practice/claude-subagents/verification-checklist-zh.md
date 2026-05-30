# 验证清单 — Subagents 报告

规则会随时间累积。每次 workflow-changelog 运行必须按照指定深度执行所有规则。当发现现有规则本应捕获（但因不存在或过于浅层而未能捕获）的新类型漂移时，在此处追加新规则。

## 深度级别

| 深度 | 含义 | 示例 |
|-------|---------|-------|
| `exists` | 检查章节/表格/文件是否存在 | "报告是否有 Memory Scopes 表格？" |
| `presence-check` | 检查特定项是否出现或缺失 | "Frontmatter Fields 表格中是否有 `color` 字段？" |
| `content-match` | 逐字对照来源比较实际值 | "`model` 字段描述是否与官方文档匹配？" |
| `field-level` | 验证每个单独字段是否都已覆盖 | "官方文档中的每个 frontmatter 字段是否都出现在表格中？" |
| `cross-file` | 同一值必须在多个文件中匹配 | "CLAUDE.md 的 agent 部分是否与报告的字段列表匹配？" |

---

## 1. Frontmatter Fields 表格

用于对照官方文档验证 Frontmatter Fields 表格的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 1A | 字段完整性 | 对于官方文档中的每个 agent frontmatter 字段，验证其是否出现在报告的 Frontmatter Fields 表格中 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 确保没有遗漏新字段 |
| 1B | 字段类型 | 对于表格中的每个字段，验证 Type 列是否与官方文档匹配 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 类型不匹配会导致用户困惑 |
| 1C | 必需状态 | 对于每个字段，验证 Required 列是否与官方文档匹配 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 错误的必需状态会导致 agent 损坏 |
| 1D | 字段描述 | 对于每个字段，验证 Description 列是否准确反映官方文档的行为 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 过时的描述会误导用户 |

---

## 2. Memory Scopes

用于验证 Memory Scopes 表格的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 2A | Scope 完整性 | 验证官方文档中的所有 memory scopes 是否都出现在 Memory Scopes 表格中 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 可能有新的 scope 被添加 |
| 2B | 存储位置 | 对于每个 scope，验证 Storage Location 列是否与官方文档匹配 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 错误的路径会导致数据丢失 |

---

## 3. 示例

用于验证示例准确性的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 3A | 最小示例 | 验证最小示例仅使用必需字段且语法有效 | content-match | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 最小示例应保持最小 |
| 3B | 功能完整示例 | 验证功能完整示例展示了所有可用的 frontmatter 字段 | field-level | sub-agents 参考页面 | 2026-02-28 | 初始清单 — 完整示例必须展示每个字段 |

---

## 4. 范围与优先级

用于验证范围和优先级信息的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 4A | 优先级顺序 | 验证 Scope and Priority 表格按正确优先级顺序列出了所有 agent 位置 | content-match | sub-agents 参考页面 + CLI 参考页面 | 2026-02-28 | 初始清单 — 错误的优先级顺序会导致解析错误 |
| 4B | 调用方式 | 验证调用方式表格列出了 CLI 参考和 sub-agents 文档中的所有调用方式，包括 `--agent`（单数）、`--agents`（复数）、`/agents`、`claude agents`、Agent 工具以及 agent 恢复 | field-level | CLI 参考页面 + sub-agents 参考页面 | 2026-03-07 | `--agent` CLI 标志在调用表格中缺失 — 这是一种以特定 agent 身份运行 Claude 的独立调用方式 |

---

## 5. 跨文件一致性

用于验证报告与其他仓库文件之间一致性的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 5A | CLAUDE.md 同步 | 验证 CLAUDE.md 的 Subagent Definition Structure 部分是否列出与报告的 Frontmatter Fields 表格相同的字段 | cross-file | CLAUDE.md 与报告对比 | 2026-02-28 | 初始清单 — CLAUDE.md 可能与报告产生偏差 |

---

## 6. 流程

关于 workflow 验证过程本身的元规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 6A | 来源可信度防护 | 仅当官方来源（sub-agents 参考页面、CLI 参考页面、GitHub changelog）确认后才将项目标记为漂移。第三方博客来源可能已过时或错误 — 仅将其用作线索，在标记前需与官方文档核实 | content-match | 仅限官方文档 | 2026-02-28 | 从 hooks workflow 采纳 — 防止来自博客来源的误报 |

---

## 7. Agent 表格

用于验证 Official Claude Agents 和 Agents in This Repository 表格的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 7A | 内置 Agent 完整性 | 验证 "Official Claude Agents" 表格列出了所有内置 agent 类型，且 model、tools 和 description 正确 | field-level | sub-agents 参考页面 + changelog | 2026-02-28 | 报告仅包含 5 个内置 agent 中的 3 个 — 缺少 `claude-code-guide` 和 `statusline-setup` |
| 7B | 仓库 Agent 完整性 | 扫描 `.claude/agents/**/*.md` 并验证每个 agent 文件都出现在 "Agents in This Repository" 表格中，且 model、color、tools、skills 和 memory 列正确 | field-level | `.claude/agents/**/*.md` 文件 frontmatter | 2026-02-28 | 仓库 agent 是手动维护的 — 添加到仓库的新 agent 未反映在报告中 |
| 7C | 仓库 Agent 链接 | 验证 "Agents in This Repository" 表格中的每个 agent 名称都有指向正确 `.md` 文件的可点击链接 | exists | 从 `best-practice/` 解析的文件路径 | 2026-02-28 | Agent 名称已设为可点击 — 文件移动后链接必须保持有效 |

---

## 8. 超链接

用于验证报告中所有超链接是否有效的规则。

| # | 类别 | 检查项 | 深度 | 对比来源 | 添加日期 | 来源 |
|---|----------|-------|-------|-----------------|-------|--------|
| 8A | 本地文件链接 | 验证所有相对文件链接（例如 `../.claude/agents/weather-agent.md`）可解析到现有文件 | exists | 本地文件系统 | 2026-02-28 | 文件移动（reports/ → best-practice/）导致相对链接失效 — 必须捕获未来的断裂 |
| 8B | 外部 URL 链接 | 验证所有外部 URL（例如 `https://code.claude.com/docs/en/sub-agents`）返回有效页面 | exists | HTTP 响应 | 2026-02-28 | 外部文档页面可能被重组或移除 — 必须在每次运行时验证 |
| 8C | 跨文件引用链接 | 验证指向其他报告文件的链接（例如 `../reports/claude-agent-memory.md`）可解析到现有文件 | exists | 本地文件系统 | 2026-02-28 | 报告可能被移动或重命名 — 交叉引用必须保持同步 |
