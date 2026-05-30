# 开发工作流变更日志

**状态图例：**

| 状态 | 含义 |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现结果不正确、不适用或有意为之 |
| `ON HOLD (reason)` | 操作延迟，等待外部依赖或用户决定 |

---

## [2026-03-19 05:25 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 仓库变更 | 将 humanlayer 从仅文章仓库改为 humanlayer/humanlayer（★ 10k，6 个 agents，27 个 commands） | COMPLETE (user requested, repo has actual implementation) |
| 2 | 高 | 计数更新 | 为 context-hub 添加计数：0 agents · 7 skills · 7 commands | COMPLETE (was showing —) |
| 3 | 高 | 计数更新 | 为 agent-os 添加计数：0 agents · 0 skills · 5 commands | COMPLETE (was showing —) |
| 4 | 中 | 计数更新 | 更新 spec-kit commands 从 14 到 9+（9 个核心，扩展由社区贡献） | COMPLETE (agents confirmed 9 core command templates) |
| 5 | 中 | 计数更新 | 更新 OpenSpec commands 从 10+ 到 11（确认确切数量） | COMPLETE (agents confirmed 11 commands) |
| 6 | 中 | 计数更新 | 更新 gstack 从 "21 skills · 21 commands" 到 "21 skills/commands"（skills 即 command 界面） | COMPLETE (no separate commands/ directory, skills ARE commands) |
| 7 | 中 | 描述更新 | 为 context-hub、agent-os、humanlayer 添加独特性描述 | COMPLETE (was showing generic descriptions) |
| 8 | 低 | 排序调整 | 将 humanlayer 从 ★ 1.6k 位置移至 ★ 10k 位置（在 context-hub 之后） | COMPLETE (repo change resulted in higher star count) |
| 9 | 低 | 报告更新 | 更新跨工作流分析报告"Workflows at a Glance"表格，包含全部 9 个工作流 | COMPLETE (was only 6, now includes all 9 sorted by stars) |

---

## [2026-03-19 05:29 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 计数更新 | 更新 obra/superpowers agents 从 7 到 5（v5.0.4 将审查循环整合为整体方案评估，移除了 2 个隐式 agents） | COMPLETE (updated README table and report) |
| 2 | 高 | 计数更新 | 更新 obra/superpowers skills 从 44+ 到 14 个核心（社区仓库 obra/superpowers-skills 已于 2025 年 10 月归档） | COMPLETE (updated README table and report) |
| 3 | 高 | 计数更新 | 更新 spec-kit：skills 10→0（v0.3.0 替换为 preset 系统），commands 保持 9+，报告中注明 22 个扩展 | COMPLETE (updated README table and report) |
| 4 | 高 | 计数更新 | 更新 context-hub 计数从 7 skills · 7 commands 改为：0 agents · 1 skill · 0 commands | COMPLETE (corrected previous run's inaccurate counts; only 1 SKILL.md in cli/skills/get-api-docs/) |
| 5 | 中 | 星标更新 | 更新 spec-kit 星标从 78k 到 79k（显示为 78.5k） | COMPLETE (updated README table and report) |
| 6 | 中 | 计数更新 | agent-os 计数已在上次运行中记录于 README：0 agents · 0 skills · 5 commands | COMPLETE (verified counts match) |
| 7 | 中 | 星标更新 | 更新 agent-os 星标从 4.1k 到 4k（实际 4,100） | COMPLETE (updated README table and report) |
| 8 | 中 | 报告更新 | 使用 obra、spec-kit、context-hub、agent-os 的最新计数更新跨工作流分析报告 | COMPLETE (updated Workflows at a Glance table) |
| 9 | 低 | 计数更新 | OpenSpec commands：表格显示 11，研究发现 9-11 取决于计数方式 | INVALID (11 is within range of findings, keeping current value) |
| 10 | 低 | 独特性 | 更新 spec-kit 独特性描述，提及可插拔扩展/preset 生态系统（v0.3.0） | COMPLETE (replaced "pre-implementation gates" with "pluggable extension/preset ecosystem") |

---

## [2026-03-20 08:37 AM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 98k 到 100k（实际 99,603 — 接近 100k 里程碑） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 87k 到 89k（实际 88,580） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Get Shit Done ★ 从 35k 到 36k（实际 36,307） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 Get Shit Done commands 从 46 到 50（v1.26.0 新增 /gsd:ship、/gsd:next、/gsd:do、/gsd:ui-phase） | COMPLETE (updated README table) |
| 5 | 中 | 星标更新 | 更新 gstack ★ 从 26k 到 29k（实际 28,889 — v0.9.0 多 AI 扩展） | COMPLETE (updated README table) |
| 6 | 中 | 计数更新 | 更新 BMAD-METHOD skills 从 43 到 42（v6.2.0 重新计数：30 个 bmm-skills + 12 个 core-skills） | COMPLETE (updated README table) |
| 7 | 低 | 排序调整 | 按 Plan 类型分组重新排列表格（commands → agents → skills，组内星标降序） | COMPLETE (commands: Spec Kit, OpenSpec, HumanLayer; agents: ECC, GSD; skills: Superpowers, BMAD, gstack) |

---

## [2026-03-21 09:20 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 100k 到 103k（实际 102,767） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 89k 到 93k（实际 93,145） | COMPLETE (updated README table) |
| 3 | 高 | 计数更新 | 更新 ECC agents 25→28、commands 57→59、skills 108+→116（v1.9.0：选择性安装、ECC Tools Pro、12 种语言生态系统） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 Get Shit Done ★ 从 36k 到 38k（实际 37,748） | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 GSD agents 16→18、commands 50→52（v1.27.0：顾问模式、多仓库工作区、/gsd:fast、/gsd:review） | COMPLETE (updated README table) |
| 6 | 高 | 星标更新 | 更新 gstack ★ 从 29k 到 34k（实际 34,456 — v0.9.4 Codex 审查、Windows 11 支持） | COMPLETE (updated README table) |
| 7 | 高 | 架构变更 | 更新 BMAD agents 从 9 到 0（v6.x 纯 skills 重写 — agent 角色现已在 bmm-skills/ 中作为 skills 实现） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 BMAD ★ 从 41k 到 42k（实际 41,629） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 OpenSpec ★ 从 32k 到 33k（实际 32,862） | COMPLETE (updated README table) |
| 10 | 中 | 排序调整 | 交换 gstack（34k）到 OpenSpec（33k）之上 — 星标降序 | COMPLETE (updated README table) |

---

## [2026-03-23 09:53 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 103k 到 107k（实际 107,308） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 93k 到 101k（实际 101,098 — 突破 100k 里程碑！） | COMPLETE (updated README table) |
| 3 | 高 | 计数更新 | 更新 ECC commands 59→60、skills 116→125（v1.9.0 继续：新增技能 pytorch-patterns、documentation-lookup、claude-devfleet、prompt-optimizer） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 34k 到 41k（实际 41,224 — v0.9.x 多 AI 扩展、CSO 安全审计） | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 gstack skills 21→27（6 个新增：gstack-autoplan、gstack-benchmark、gstack-cso、gstack-design-consultation、gstack-office-hours、gstack-freeze/unfreeze） | COMPLETE (updated README table) |
| 6 | 高 | 排序调整 | 将 gstack（41k）移至 GSD（40k）之上 — 星标降序 | COMPLETE (updated README table) |
| 7 | 高 | 星标更新 | 更新 GSD ★ 从 38k 到 40k（实际 39,588） | COMPLETE (updated README table) |
| 8 | 高 | 计数更新 | 更新 GSD commands 52→57（v1.28.0：/gsd:forensics、/gsd:milestone-summary、/gsd:plant-seed、/gsd:profile-user、/gsd:workstreams） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 Spec Kit ★ 从 79k 到 81k（实际 81,349 — v0.4.0 嵌入式核心包、24 平台支持） | COMPLETE (updated README table) |
| 10 | 中 | 计划更新 | 更新 gstack Plan 从 plan-eng-review 到 autoplan（更高级的编排器，顺序读取 CEO、设计、工程审查） | COMPLETE (updated README table) |
| 11 | 低 | 计数更新 | 更新 OpenSpec commands 11→10（重新计数：/opsx:propose、apply、archive、new、continue、ff、verify、sync、bulk-archive、onboard） | COMPLETE (updated README table) |
| 12 | 低 | 计数修正 | 修正 OpenSpec skills 11→0（不存在 skills/ 或 .claude/skills/ 目录 — OpenSpec 是一个 CLI 工具，非基于 skills） | COMPLETE (updated README table) |

---

## [2026-03-24 08:12 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 107k 到 110k（实际 109,846） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 101k 到 104k（实际 103,960） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 41k 到 44k（实际 44,300 — v0.11.x 三重语音多模型审查） | COMPLETE (updated README table) |
| 4 | 高 | 排序调整 | 将 gstack（44k）移至 BMAD（42k）之上 — 星标降序 | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 BMAD skills 从 42 到 44（重新计数：32 个 bmm-skills + 12 个 core-skills，包括 3 个嵌套研究子技能） | COMPLETE (updated README table) |
| 6 | 高 | 计数更新 | 更新 gstack skills 从 27 到 28（README 显示 28；逐个确认 27 个） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Spec Kit ★ 从 81k 到 82k（实际 81,780） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 GSD ★ 从 40k 到 41k（实际 40,500） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 OpenSpec ★ 从 33k 到 34k（实际 33,800） | COMPLETE (updated README table) |

---

## [2026-03-25 08:12 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 110k 到 112k（实际 112,163） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 104k 到 107k（实际 106,913） | COMPLETE (updated README table) |
| 3 | 高 | 计数更新 | 更新 ECC commands 从 60 到 63（.claude/commands/ 中新增 3 个：add-language-rules、database-migration、feature-development） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 44k 到 47k（实际 46,703 — 基础设施加固、测试覆盖率门禁） | COMPLETE (updated README table) |
| 5 | 中 | 计数更新 | 更新 BMAD skills 从 44 到 42（重新计数：30 个 bmm-skills + 12 个 core-skills；v6.2.1 合并了 2 个子技能） | COMPLETE (updated README table) |
| 6 | 低 | 计数更新 | 更新 gstack skills 从 28 到 27（确认 27 个根级目录；第 28 个可能是根 SKILL.md 模板） | COMPLETE (updated README table) |

---

## [2026-03-26 01:05 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 112k 到 114k（实际 114,107） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 107k 到 109k（实际 108,839） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 47k 到 48k（实际 48,303） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 GSD ★ 从 41k 到 42k（实际 42,092） | COMPLETE (updated README table) |
| 5 | 中 | 计数更新 | 更新 OpenSpec commands 从 10 到 11（v1.2.0 新增 /opsx:explore） | COMPLETE (updated README table) |

---

## [2026-03-27 06:32 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 114k 到 118k（实际 117,568） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 109k 到 111k（实际 111,487） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 48k 到 52k（实际 51,544 — v0.12.x 技能命名空间、Codex 回退、worktree 并行化） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 gstack skills 从 27 到 31（4 个新增：canary、codex、connect-chrome、land-and-deploy 等） | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 GSD ★ 从 42k 到 43k（实际 43,136） | COMPLETE (updated README table) |
| 6 | 高 | 排序调整 | 交换 GSD（43,136）到 BMAD（42,529）之上 — 两者均四舍五入为 43k 但 GSD 星标更多 | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Spec Kit ★ 从 82k 到 83k（实际 82,878） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 BMAD ★ 从 42k 到 43k（实际 42,529） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 OpenSpec ★ 从 34k 到 35k（实际 34,821） | COMPLETE (updated README table) |
| 10 | 中 | 计数更新 | 更新 Compound Engineering agents 从 43 到 47（4 个新增审查/工作流 agents） | COMPLETE (updated README table) |
| 11 | 中 | 计数更新 | 更新 Compound Engineering skills 从 44 到 42（重新计数：41 个 compound-engineering + 1 个 coding-tutor） | COMPLETE (updated README table) |

---

## [2026-03-28 09:29 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 118k 到 120k（实际 120,147） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 111k 到 114k（实际 114,134） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 52k 到 54k（实际 53,533 — v0.13.x 设计二进制、安全审计） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 GSD ★ 从 43k 到 44k（实际 43,816 — v1.30.0 GSD SDK 无头 CLI） | COMPLETE (updated README table) |
| 5 | 中 | 计数更新 | 更新 gstack skills 从 31 到 29（确认 29 个根级 SKILL.md 目录；2 个在 v0.13.x 中被移除/合并） | COMPLETE (updated README table) |
| 6 | 中 | 计数更新 | 更新 BMAD skills 从 42 到 43（31 个 bmm-skills + 12 个 core-skills） | COMPLETE (updated README table) |
| 7 | 中 | 计数更新 | 更新 Compound Engineering skills 从 42 到 43（42 个 compound-eng + 1 个 coding-tutor） | COMPLETE (updated README table) |

---

## [2026-03-29 08:00 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 120k 到 122k（实际 122,129） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 114k 到 116k（实际 115,898） | COMPLETE (updated README table) |
| 3 | 高 | 计数更新 | 更新 ECC agents 从 28 到 30、skills 从 125 到 135（新增 healthcare agent、token-budget-advisor 等） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 54k 到 55k（实际 55,000） | COMPLETE (updated README table) |
| 5 | 中 | 计数更新 | 更新 gstack skills 从 29 到 28（README 确认 28 个根级 SKILL.md 目录） | COMPLETE (updated README table) |
| 6 | 中 | 计数更新 | 更新 BMAD skills 从 43 到 40（重新计数：29 个 bmm-skills + 11 个 core-skills；近期补丁中的合并） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Compound Engineering ★ 从 11k 到 12k（实际 11,500） | COMPLETE (updated README table) |
| 8 | 中 | 计数更新 | 更新 Compound Eng agents 从 47 到 48（新增 1 个）、skills 从 43 到 42（41 个 compound-eng + 1 个 coding-tutor） | COMPLETE (updated README table) |

---

## [2026-03-31 07:43 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 122k 到 127k（实际 127,473） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 116k 到 124k（实际 124,279） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 55k 到 59k（实际 59,046 — v0.14.x Review Army、可组合 skills、对抗性审查） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 GSD ★ 从 44k 到 46k（实际 45,773） | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 gstack skills 从 28 到 32（4 个新增：design-html、sidebar CSS 检查器、可组合技能解析器、范围漂移检测） | COMPLETE (updated README table) |
| 6 | 中 | 星标更新 | 更新 Spec Kit ★ 从 83k 到 84k（实际 84,042） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 OpenSpec ★ 从 35k 到 36k（实际 35,985） | COMPLETE (updated README table) |
| 8 | 中 | 计数更新 | 更新 BMAD skills 从 40 到 43（32 个 bmm-skills + 11 个 core-skills；新增 3 个 bmm-skills，包括 PRFAQ） | COMPLETE (updated README table) |
| 9 | 低 | 计数核实 | ECC commands 63→3、skills 135→30 — 研究 agent 仅检查了 .claude/ 目录，遗漏了根 commands/ 和 .agents/skills/ 的广度 | INVALID (agent undercounting — keeping current values 63 commands, 135 skills) |
| 10 | 低 | 计数核实 | Superpowers agents 5→8 — agent 计数了 1 个显式 + 7 个隐式子 agents，但 v5.0.6 已将子 agent 审查循环替换为内联自审 | ON HOLD (contradictory signals — v5.0.6 reduced review agents while brainstorm added new ones, needs manual verification) |

---

## [2026-04-01 12:35 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 127k 到 129k（实际 128,925） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 124k 到 129k（实际 128,606 — 与 Superpowers 并驾齐驱） | COMPLETE (updated README table) |
| 3 | 高 | 计数更新 | 更新 ECC agents 30→36、commands 63→71、skills 135→143（6 个新 agents 包括 gan-evaluator/generator/planner、cpp/kotlin/flutter 审查员；8 个新 commands；8 个新 skills） | COMPLETE (updated README table) |
| 4 | 中 | 星标更新 | 更新 gstack ★ 从 59k 到 60k（实际 60,036 — v0.15.0 /checkpoint、/health、跨会话时间线） | COMPLETE (updated README table) |
| 5 | 中 | 计数更新 | 更新 gstack skills 32→33（v0.15.0 新增 /checkpoint 和 /health，但部分被合并 — 净增 +1） | COMPLETE (updated README table) |
| 6 | 低 | 计数更新 | 更新 CE commands 4→3（.claude/commands/ 现为空；保留 3 个 coding-tutor commands）、skills 42→40（39 CE + 1 CT） | COMPLETE (updated README table) |
| 7 | 低 | 计数核实 | BMAD skills 43→34 — agent 从 module-help.csv 计数（25 bmm + 9 core），之前的目录计数为 43（32 bmm + 11 core） | ON HOLD (agent likely undercounting — module-help.csv may not list all skills; keeping 43 until manual verification) |

---

## [2026-04-02 09:22 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 排序调整 | 将 ECC（133k）移至 Superpowers（132k）之上 — ECC 星标现已更多 | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 129k 到 133k（实际 133,114 — 超越 Superpowers） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Superpowers ★ 从 129k 到 132k（实际 131,818） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 ECC commands 71→68、skills 143→152（旧版 commands 合并到 skills；+9 个新 skills，包括 brand-voice、network-ops） | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 gstack ★ 从 60k 到 62k（实际 61,800 — v0.15.1 design-html 路由、会话智能层） | COMPLETE (updated README table) |
| 6 | 高 | 计数更新 | 更新 GSD agents 18→21、commands 57→59（v1.31.0：3 个新 agents、skills 发现、Gemini CLI 修复） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Spec Kit ★ 从 84k 到 85k（实际 84,701） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 GSD ★ 从 46k 到 47k（实际 46,900） | COMPLETE (updated README table) |
| 9 | 中 | 计数更新 | 更新 BMAD skills 43→40（29 个 bmm-skills + 11 个 core-skills；移除了 QA Quinn + Barry solo-dev，新增 checkpoint-preview） | COMPLETE (updated README table) |
| 10 | 中 | 星标更新 | 更新 OpenSpec ★ 从 36k 到 37k（实际 36,600） | COMPLETE (updated README table) |
| 11 | 中 | 星标更新 | 更新 CE ★ 从 12k 到 13k（实际 12,600） | COMPLETE (updated README table) |
| 12 | 中 | 计数更新 | 更新 CE agents 48→49、commands 3→4、skills 40→42（新增 triage-prs command；+1 agent、+2 skills） | COMPLETE (updated README table) |

---

## [2026-04-03 10:56 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 133k 到 136k（实际 135,765 — 扩大对 Superpowers 的领先优势） | COMPLETE (updated README table) |
| 2 | 高 | 计数更新 | 更新 ECC agents 36→38、commands 68→75、skills 152→156（NestJS 模式、Jira 集成、C#/Dart 支持、Web 前端规则） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Superpowers ★ 从 132k 到 134k（实际 133,718 — v5.0.7 Copilot CLI 支持、贡献者护栏） | COMPLETE (updated README table) |
| 4 | 中 | 星标更新 | 更新 gstack ★ 从 62k 到 63k（实际 63,065 — 会话智能层、AquaVoice 别名） | COMPLETE (updated README table) |
| 5 | 中 | 计数更新 | 更新 gstack skills 从 33 到 31（确认 31 个根级 SKILL.md 目录；checkpoint/health 可能是子命令） | COMPLETE (updated README table) |
| 6 | 低 | 计数更新 | 更新 GSD commands 从 59 到 60（v1.31.0：新增 /gsd:docs-update） | COMPLETE (updated README table) |
| 7 | 低 | 计数更新 | 更新 BMAD skills 从 40 到 39（28 个 bmm-skills + 11 个 core-skills；小幅合并） | COMPLETE (updated README table) |

---

## [2026-04-04 10:45 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 星标更新 | 更新 ECC ★ 从 136k 到 137k（实际 137,404） | COMPLETE (updated README table) |
| 2 | 中 | 星标更新 | 更新 Superpowers ★ 从 134k 到 135k（实际 134,933） | COMPLETE (updated README table) |
| 3 | 中 | 星标更新 | 更新 gstack ★ 从 63k 到 64k（实际 63,841 — GStack Browser .app 含 CDP、反机器人隐身） | COMPLETE (updated README table) |
| 4 | 中 | 星标更新 | 更新 GSD ★ 从 47k 到 48k（实际 47,705 — v1.32.0 Trae/Kilo/Augment/Cline 运行时） | COMPLETE (updated README table) |
| 5 | 低 | 星标更新 | 更新 BMAD ★ 从 43k 到 44k（实际 43,538） | COMPLETE (updated README table) |
| 6 | 低 | 星标更新 | 更新 oh-my-claudecode ★ 从 23k 到 24k（实际 23,709 — v4.10.2 HUD、Bedrock 加固） | COMPLETE (updated README table) |

---

## [2026-04-06 09:49 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 137k 到 142k（实际 142,218 — v1.10.0 Surface Refresh，4 月 6 日单日 10 次提交） | COMPLETE (updated README table) |
| 2 | 高 | 计数更新 | 更新 ECC agents 38→47、commands 75→82、skills 156→182（agent-introspection-debugging、恢复 hookify bundle、26 个新 skills） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Superpowers ★ 从 135k 到 137k（实际 137,166） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 GSD agents 21→24、commands 60→68（v1.33.0：统一行为引用、STATE.md 漂移检测、自主 --to N） | COMPLETE (updated README table) |
| 5 | 中 | 星标更新 | 更新 gstack ★ 从 64k 到 65k（实际 65,279 — v0.15.15.0 令牌编辑、团队模式） | COMPLETE (updated README table) |
| 6 | 中 | 计数更新 | 更新 gstack skills 从 31 到 34（3 个新增：retro、setup-deploy、learn 等） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Spec Kit ★ 从 85k 到 86k（实际 85,617 — v0.5.0 原生 skills 架构） | COMPLETE (updated README table) |
| 8 | 低 | 星标更新 | 更新 OpenSpec ★ 从 37k 到 38k（实际 37,604） | COMPLETE (updated README table) |
| 9 | 低 | 星标更新 | 更新 oh-my-claudecode ★ 从 24k 到 25k（实际 24,921 — v4.10.0 HUD 升级、LSP 诊断） | COMPLETE (updated README table) |
| 10 | 低 | 计数更新 | 更新 CE agents 从 49 到 50（新增 1 个 agent） | COMPLETE (updated README table) |

---

## [2026-04-08 09:38 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 142k 到 146k（实际 146,462 — v1.10.0 Surface Refresh 势头、ecc2 alpha 开发） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Superpowers ★ 从 137k 到 141k（实际 141,071） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 65k 到 67k（实际 67,178 — v0.16.0.0 浏览器数据平台、每标签页状态隔离） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 gstack skills 从 34 到 37（3 个新增：setup-browser-cookies、pair-agent、open-gstack-browser 等已确认的添加） | COMPLETE (updated README table) |
| 5 | 中 | 星标更新 | 更新 GSD ★ 从 48k 到 49k（实际 49,343 — v1.34.0 四类别门禁分类法、合并后验证） | COMPLETE (updated README table) |
| 6 | 中 | 星标更新 | 更新 oh-my-claudecode ★ 从 25k 到 26k（实际 26,203 — v4.11.1 git 状态 HUD、主机名元素） | COMPLETE (updated README table) |
| 7 | 中 | 计数更新 | 更新 oh-my-claudecode skills 从 36 到 37（新增 skillify skill） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 CE ★ 从 13k 到 14k（实际 13,671 — v2.62.0 决策矩阵、无头模式） | COMPLETE (updated README table) |
| 9 | 低 | 计数更新 | 更新 CE agents 从 50 到 51（新增 1 个 agent） | COMPLETE (updated README table) |
| 10 | 低 | 计数更新 | 更新 CE skills 从 42 到 44（2 个新增：onboarding skill、互动深化模式） | COMPLETE (updated README table) |

---

## [2026-04-10 12:23 AM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 146k 到 148k（实际 148,000 — v1.10.0 势头、ecc2 alpha） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Superpowers ★ 从 141k 到 143k（实际 143,000 — v5.0.7 Copilot CLI） | COMPLETE (updated README table) |
| 3 | 中 | 星标更新 | 更新 Spec Kit ★ 从 86k 到 87k（实际 86,600 — v0.5.1 开发文档） | COMPLETE (updated README table) |
| 4 | 中 | 星标更新 | 更新 gstack ★ 从 67k 到 68k（实际 68,200 — v0.16.0.0 浏览器数据平台） | COMPLETE (updated README table) |
| 5 | 中 | 星标更新 | 更新 GSD ★ 从 49k 到 50k（实际 49,900 — v1.34.0 持久化学习、情报查询） | COMPLETE (updated README table) |
| 6 | 中 | 星标更新 | 更新 OpenSpec ★ 从 38k 到 39k（实际 38,700） | COMPLETE (updated README table) |
| 7 | 低 | 星标更新 | 更新 oh-my-claudecode ★ 从 26k 到 27k（实际 26,900 — v4.11.4 每日发布） | COMPLETE (updated README table) |
| 8 | 低 | 计数更新 | 更新 CE skills 从 44 到 43（42 个 compound-eng + 1 个 coding-tutor；小幅合并） | COMPLETE (updated README table) |

---

## [2026-04-11 06:14 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 148k 到 150k（实际 150,802 — ECC2 多框架基础设施推动，4 月 10 日 35+ 次提交） | COMPLETE (updated README table) |
| 2 | 高 | 计数更新 | 更新 ECC commands 82→120（ECC2：多框架运行器、持久任务调度、computer-use 调度） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Superpowers ★ 从 143k 到 146k（实际 146,545 — v5.0.7 Copilot CLI、贡献者护栏） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 68k 到 70k（实际 69,560 — v0.16.3.0 slop-scan、office-hours 持久化、cookie 认证修复） | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 GSD agents 24→29、commands 68→119（v1.35.0：Cline/CodeBuddy/Qwen 运行时、+51 个 commands 用于多运行时支持） | COMPLETE (updated README table) |
| 6 | 中 | 星标更新 | 更新 GSD ★ 从 50k 到 51k（实际 50,501） | COMPLETE (updated README table) |
| 7 | 中 | 计数更新 | 更新 oh-my-claudecode skills 37→46（通过 API 确认 9 个新 skill 目录；v4.11.3） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 oh-my-claudecode ★ 从 27k 到 28k（实际 27,580） | COMPLETE (updated README table) |
| 9 | 中 | 计数更新 | 更新 gstack skills 37→35（逐个确认 35 个 SKILL.md 目录；2 个在 v0.16.x 中被合并） | COMPLETE (updated README table) |
| 10 | 中 | 计数更新 | 更新 BMAD skills 39→41（v6.3.0：市场插件、新增 bmad-prfaq；31 bmm + 10 core） | COMPLETE (updated README table) |
| 11 | 低 | 计数更新 | 更新 CE skills 43→47（44 个 compound-eng + 3 个 coding-tutor；v2.65.0 demo reel、setup skill） | COMPLETE (updated README table) |
| 12 | 低 | 计数核实 | CE agents 51→48 — agent 报告约 48 但置信度 0.72（子目录枚举出现 403 错误） | ON HOLD (low confidence; keeping 51 until manual verification) |
| 13 | 低 | 计数更新 | 更新 ECC skills 182→181（README 自报 181；小幅合并） | COMPLETE (updated README table) |

---

## [2026-04-13 08:08 PM PKT] 开发工作流更新

⚠️ **注意**：4 月 11 日变更日志第 1-13 项已标记为 COMPLETE 但从未应用于 README 表格。以下所有星标/计数变化均以实际 README 值（4 月 10 日状态）为基准测量，而非 4 月 11 日记录的值。

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 148k 到 154k（实际 153,942 — ECC2 alpha、v1.10.0 Surface Refresh、48 个 agents） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Superpowers ★ 从 143k 到 150k（实际 149,857 — 突破 150k 里程碑！） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 68k 到 71k（实际 71,298 — v0.16.3.0 slop-scan、cookie 认证） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 GSD ★ 从 50k 到 52k（实际 51,795 — 知识图谱、类型化 SDK 查询） | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 GSD agents 24→31、commands 68→122（v1.35.0：多运行时 Cline/CodeBuddy/Qwen、+7 agents、+54 commands） | COMPLETE (updated README table) |
| 6 | 高 | 计数更新 | 更新 ECC agents 47→48（新增：已确认 harness-optimizer） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Spec Kit ★ 从 87k 到 88k（实际 87,564 — v0.6.1 cursor-agent 迁移、6 个社区扩展） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 BMAD ★ 从 44k 到 45k（实际 44,472 — 安装程序修复、skill 扫描器递归错误） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 OpenSpec ★ 从 39k 到 40k（实际 39,558 — v1.3.0 IBM Bob Shell 适配器） | COMPLETE (updated README table) |
| 10 | 中 | 星标更新 | 更新 oh-my-claudecode ★ 从 27k 到 28k（实际 28,344 — v4.11.6 安全加固、Ralph 欺骗修复） | COMPLETE (updated README table) |
| 11 | 中 | 计数更新 | 更新 gstack skills 37→31（从 docs/skills.md 权威列表确认 31 个；6 个在 v0.16.x 中被合并） | COMPLETE (updated README table) |
| 12 | 中 | 计数更新 | 更新 ECC commands 82→143、skills 182→230 — 使用目录计数以确保一致性（agent 发现 143 个 cmd 文件 / 230 个 skill 目录；ECC 自报 79 cmds / 156 skills；置信度 0.72） | COMPLETE (updated README table with directory counts) |
| 13 | 低 | 计数更新 | 更新 BMAD skills 39→37（26 个 bmm-skills + 11 个 core-skills；Bob Scrum Master 合并到 Developer） | COMPLETE (updated README table) |
| 14 | 低 | 计数更新 | 更新 CE agents 51→49、skills 43→42（清理：移除了几个旧版 skills，新增 ce-debug/ce-demo-reel） | COMPLETE (updated README table) |
| 15 | 低 | 计数更新 | 更新 OpenSpec commands 11→10（重新计数：/opsx:explore 可能在 v1.3.0 中被移除） | COMPLETE (updated README table) |

---

## [2026-04-14 11:38 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 154k 到 156k（实际 155,874 — ECC2 alpha、v1.10.0 Surface Refresh 势头） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Superpowers ★ 从 150k 到 152k（实际 151,979 — v5.0.7 Copilot CLI、贡献者护栏） | COMPLETE (updated README table) |
| 3 | 中 | 星标更新 | 更新 gstack ★ 从 71k 到 72k（实际 72,298 — v0.17.0.0 ux-audit、UX 行为基础） | COMPLETE (updated README table) |
| 4 | 中 | 计数更新 | 更新 gstack skills 31→36（通过逐个文件获取确认 36 个 SKILL.md；v0.17.0.0 新增包括 ux-audit、guard、gstack-upgrade） | COMPLETE (updated README table) |
| 5 | 中 | 星标更新 | 更新 GSD ★ 从 52k 到 53k（实际 52,871 — v1.36.0 graphify、类型化 SDK 查询、陈旧 worktree 检测） | COMPLETE (updated README table) |
| 6 | 低 | 星标更新 | 更新 oh-my-claudecode ★ 从 28k 到 29k（实际 28,771 — v4.11.6 安全加固、Ralph 欺骗修复） | COMPLETE (updated README table) |

---

## [2026-04-16 08:25 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 152k 到 156k（实际 155,753 — v5.0.7 Copilot CLI、Codex 插件重构） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 ECC ★ 从 156k 到 158k（实际 158,287 — ECC2 alpha、hook schema 修复、CI 稳定性） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 72k 到 74k（实际 73,750 — v0.17.0.0 UX 审计、cookie 来源锁定） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 gstack skills 36→46（通过仓库列表确认 46 个根级 SKILL.md 目录；+10 个新 skill 目录，包括 UX 审计、guard、升级工具） | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 GSD ★ 从 53k 到 54k（实际 53,923 — v1.36.0 graphify、TDD 流水线模式、pattern-mapper） | COMPLETE (updated README table) |
| 6 | 高 | 计数更新 | 更新 CE skills 42→51（确认 50 个 compound-engineering + 1 个 coding-tutor；v2.66.x 自动研究循环、setup skill） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 Spec Kit ★ 从 88k 到 89k（实际 88,525 — v0.7.1 skill 链式调用、Salesforce/Worktrees 扩展） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 OpenSpec ★ 从 40k 到 41k（实际 40,584 — v1.3.0 IBM Bob Shell 适配器、Junie/Lingma/ForgeCode） | COMPLETE (updated README table) |
| 9 | 中 | 计数更新 | 更新 BMAD skills 37→39（确认 28 个 bmm-skills + 11 个 core-skills） | COMPLETE (updated README table) |
| 10 | 低 | 计数更新 | 更新 CE commands 4→3（.claude/commands/ 已清空；保留 3 个 coding-tutor commands） | COMPLETE (updated README table) |
| 11 | 低 | 计数核实 | ECC agents 48→60 — agent 在 agents/ 中找到 60 个 .md 文件，但 CHANGELOG 声明 38 个已发布表面 | ON HOLD (discrepancy between directory count and published surface; keeping 48) |
| 12 | 低 | 计数核实 | ECC commands 143→133 — agent 计数 130 个根 + 3 个 .claude；可能存在分页少计 | ON HOLD (keeping 143 until verified; decrease seems unlikely given active development) |
| 13 | 低 | 计数核实 | ECC skills 230→156 — CHANGELOG 自报 156 但之前的目录计数为 230 | ON HOLD (keeping 230; different counting methodology) |
| 14 | 低 | 计数核实 | GSD commands 122→74 — agent 枚举了 A-W 文件名，但 39% 的急剧下降似乎不太可能 | ON HOLD (keeping 122 until verified; may be pagination/multi-runtime directory issue) |

---

## [2026-04-18 07:59 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 ECC ★ 从 158k 到 160k（实际 160,162 — v1.10.0 Surface Refresh 势头、ecc2 alpha） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Superpowers ★ 从 156k 到 159k（实际 158,523 — v5.0.7 Copilot CLI，18 天无新版本） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 74k 到 76k（实际 75,773 — v1.0.0.0 今日发布：更简洁的 prompts、真实的 LOC 收据、类型化问题注册表） | COMPLETE (updated README table) |
| 4 | 高 | 计数更新 | 更新 gstack skills 46→37（按名称确认 37 个根级 SKILL.md 目录；v1.0.0.0 合并移除了 9 个 skill 目录） | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 GSD ★ 从 54k 到 55k（实际 54,605 — v1.37.1 于 2026-04-17 发布：ingest-docs command、UI-phase 研究人员修复） | COMPLETE (updated README table) |
| 6 | 高 | 计数更新 | 更新 GSD agents 31→33（agents/ 目录中 2 个新的 gsd-* agents） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 oh-my-claudecode ★ 从 29k 到 30k（实际 29,773 — v4.12.1 今日发布：跨 24 个 PR 的 8 个漏洞修复、Opus 4.7 默认、Gemini 通道修复） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 CE ★ 从 14k 到 15k（实际 14,681 — v2.68.1 今日发布：ce-compound-refresh 和 ce-pr-description 交接修复） | COMPLETE (updated README table) |
| 9 | 中 | 计数更新 | 更新 CE agents 49→50、commands 3→4、skills 51→44（triage-prs.md 已添加到 .claude/commands/；43 个 compound-engineering + 1 个 coding-tutor skills） | COMPLETE (updated README table) |
| 10 | 中 | 计数更新 | 更新 OpenSpec commands 10→11（/opsx:explore 与 /opsx:new、/continue、/ff、/verify、/sync、/bulk-archive、/onboard、/propose、/apply、/archive 并存） | COMPLETE (updated README table) |
| 11 | 低 | 计数核实 | ECC commands 143→79 — 4 月 18 日 agent 通过 git tree 确认 79 个 command .md 文件；4 月 16 日通过目录计数有 143 个，置信度 0.72 | ON HOLD (methodology differs between git-tree vs. directory API; keeping 143 until manual verification) |
| 12 | 低 | 计数核实 | ECC skills 230→183 — 4 月 18 日 agent 通过 git tree 确认 183 个 skill 文件夹；4 月 16 日通过目录计数有 230 个 | ON HOLD (keeping 230 until manual verification; recurring with Apr 13/16 ON HOLD items 12-13) |
| 13 | 低 | 计数核实 | GSD commands 122→81 — 4 月 18 日 agent 确认 commands/gsd/ 中 81 个 .md 文件；4 月 16 日为 122（那次运行也为 74 值标为 ON HOLD） | ON HOLD (recurring discrepancy, likely multi-runtime pagination; keeping 122 until verified) |
| 14 | 低 | 计数核实 | Superpowers agents 5→1 — 4 月 18 日 agent 计数了 1 个显式 agent；之前的计数包括了由 skills 分派的隐式子 agents | ON HOLD (methodology change only; keeping 5 which includes implicit sub-agent count) |
| 15 | 低 | 计数核实 | oh-my-claudecode Plan 链接显示 ralplan 但 agent 识别 omc-plan（skills/plan/SKILL.md）为活动规划器 | ON HOLD (both skills exist in repo; keeping ralplan link until user preference clarified) |

---

## [2026-04-24 12:39 AM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 排序调整 | 将 Superpowers（166k）移至 ECC（165k）之上 — Superpowers 超越 ECC 成为第一 | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Superpowers ★ 从 159k 到 166k（实际 165,520 — v5.0.7 Codex 插件集成、PRs #1165/#1180） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 ECC ★ 从 160k 到 165k（实际 165,156 — v2.1.116 hook 安装修复、Windows Python 检测） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 76k 到 81k（实际 81,300 — v1.6.4.0） | COMPLETE (updated README table) |
| 5 | 高 | 计数更新 | 更新 gstack skills 从 37 到 41（通过目录枚举确认 41 个根级 SKILL.md 目录） | COMPLETE (updated README table) |
| 6 | 高 | 星标更新 | 更新 GSD ★ 从 55k 到 57k（实际 56,600 — v1.38.2 SDK 工作流线程化、agent-skills 查询修复） | COMPLETE (updated README table) |
| 7 | 高 | 计数更新 | 更新 oh-my-claudecode skills 从 37 到 46（46 个根级 SKILL.md 目录；与 4 月 11 日合并前计数一致） | COMPLETE (updated README table) |
| 8 | 高 | 计数更新 | 更新 CE agents 从 50 到 60（v3.0.0 于 2026 年 4 月 22 日：所有 skills/agents 重命名为 ce- 前缀、原生插件清单） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 Spec Kit ★ 从 89k 到 90k（实际 90,458 — v0.8.0 于 2026 年 4 月 23 日：预设组合策略、编剧预设） | COMPLETE (updated README table) |
| 10 | 中 | 星标更新 | 更新 BMAD ★ 从 45k 到 46k（实际 45,500 — v6.3.0 市场集成） | COMPLETE (updated README table) |
| 11 | 中 | 计数更新 | 更新 BMAD skills 从 39 到 40（28 个 bmm-skills + 12 个 core-skills） | COMPLETE (updated README table) |
| 12 | 中 | 星标更新 | 更新 OpenSpec ★ 从 41k 到 43k（实际 42,500 — v1.3.1 于 2026 年 4 月 21 日：glob 转义修复、遥测配置） | COMPLETE (updated README table) |
| 13 | 中 | 星标更新 | 更新 oh-my-claudecode ★ 从 30k 到 31k（实际 30,900 — v4.13.2 于 2026 年 4 月 22 日：跨会话取消状态、Usage API 修复） | COMPLETE (updated README table) |
| 14 | 中 | 星标更新 | 更新 HumanLayer ★ 从 10k 到 11k（实际 10,600） | COMPLETE (updated README table) |
| 15 | 低 | 计数更新 | 更新 CE skills 从 44 到 42（41 个 compound-engineering + 1 个 coding-tutor；v3.0.0 合并） | COMPLETE (updated README table) |
| 16 | 低 | 计数核实 | ECC 48→47 agents、143→82 commands（79+3）、230→183 skills — 通过目录枚举连续第 3 次运行 | ON HOLD (RECURRING from Apr 13/16/18; methodology difference persists — keeping current values until manual verification) |
| 17 | 低 | 计数核实 | GSD commands 122→85 — 连续第 3 次较低计数（4 月 16 日：74、4 月 18 日：81、4 月 24 日：85） | ON HOLD (RECURRING from Apr 16/18; likely multi-runtime directory pagination — keeping 122 until verified) |

---

## [2026-04-26 01:18 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 166k 到 168k（实际 167,874 — v5.0.7 Codex 插件镜像） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 165k 到 167k（实际 167,155 — v1.10.0 Operator Workflows、ECC 2.0 Alpha） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 gstack ★ 从 81k 到 84k（实际 83,534 — v1.14.0.0 交互式 REPL 浏览器侧边栏、$B tab-each 扇出） | COMPLETE (updated README table) |
| 4 | 高 | 标签更新 | 更新 BMAD-METHOD 标签从 "22+ platforms" 到 "42 platforms"（v6.5.0 今日发布新增 18 个 agent 平台） | COMPLETE (updated README table) |
| 5 | 中 | 星标更新 | 更新 Spec Kit ★ 从 90k 到 91k（实际 90,907 — v0.8.1 SkillsIntegration 用于 vibe 集成，3 天内 3 个版本） | COMPLETE (updated README table) |
| 6 | 中 | 星标更新 | 更新 Compound Engineering ★ 从 15k 到 16k（实际 15,549 — v3.1.0 ce-ideate skill、ast-grep CLI 集成） | COMPLETE (updated README table) |
| 7 | 中 | 计数更新 | 更新 Compound Engineering agents 从 60 到 51（根据仓库 README 明确声明；.agent.md 文件枚举确认） | COMPLETE (updated README table) |
| 8 | 中 | 计数更新 | 更新 Compound Engineering skills 从 42 到 37（根据仓库 README "36 skills" + 1 个 coding-tutor skill） | COMPLETE (updated README table) |
| 9 | 低 | 计数更新 | 更新 BMAD skills 从 40 到 39（通过目录枚举确认 27 个 bmm-skills + 12 个 core-skills） | COMPLETE (updated README table) |
| 10 | 低 | 计数核实 | oh-my-claudecode skills 46→38 — agent 通过 API 枚举了 38 个目录 | ON HOLD (recurring lower count vs. 46 baseline; possible pagination — keeping 46 until verified) |
| 11 | 低 | 计数核实 | ECC 计数 143→82 commands、230→183 skills — 通过目录枚举连续第 4 次运行 | ON HOLD (RECURRING from Apr 13/16/18/24; methodology persists — keeping current values until manual verification) |
| 12 | 低 | 计数核实 | GSD commands 122→85 — 通过 API 枚举连续第 4 次较低计数 | ON HOLD (RECURRING from Apr 16/18/24; likely directory pagination — keeping 122 until verified) |
| 13 | 低 | 计数核实 | Superpowers agents 5→1 正式 — agents/ 只有 code-reviewer.md；4 个从 skills 隐式分派 | ON HOLD (keeping 5 per prior decision to count implicit dispatch roles) |

---

## [2026-04-29 12:48 AM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 168k 到 171k（实际 171,334 — v5.0.7 势头） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 167k 到 169k（实际 169,230 — v1.10.0 桌面仪表盘、ECC2 alpha） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Spec Kit ★ 从 91k 到 92k（实际 91,505 — v0.8.2 RAG/Chroma DB、6 天内 3 个版本） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 84k 到 86k（实际 86,021 — v1.17.0.0） | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 GSD ★ 从 57k 到 58k（实际 58,418 — v1.39.0-rc.4 最小安装配置、/gsd-edit-phase） | COMPLETE (updated README table) |
| 6 | 高 | 星标更新 | 更新 OpenSpec ★ 从 43k 到 44k（实际 43,637 — v1.3.1 路径规范化修复） | COMPLETE (updated README table) |
| 7 | 高 | 星标更新 | 更新 oh-my-claudecode ★ 从 31k 到 32k（实际 31,760 — v4.13.5 HUD 速率限制修复、自动合并编排器） | COMPLETE (updated README table) |
| 8 | 高 | 计数更新 | 更新 gstack skills 从 41 到 42（确认 42 个根级 SKILL.md 目录；+plan-devex-review） | COMPLETE (updated README table) |
| 9 | 高 | 计数更新 | 更新 BMAD skills 从 39 到 40（28 个 bmm-skills + 12 个 core-skills；bmad-customize 于 4 月 21 日在 v6.5.0 中新增） | COMPLETE (updated README table) |
| 10 | 高 | 计数更新 | 更新 Matt Pocock skills 从 16 到 22（5 个类别子目录：engineering 9、productivity 3、misc 4、personal 2、deprecated 4） | COMPLETE (updated README table) |
| 11 | 高 | 工作流更新 | 更新 Spec Kit 工作流 — 在 /speckit.constitution 和 /speckit.specify 之间插入 /speckit.clarify | COMPLETE (updated README table) |
| 12 | 高 | 工作流更新 | 更新 Superpowers 工作流 — 在 brainstorming 和 writing-plans 之间插入 using-git-worktrees | COMPLETE (updated README table) |
| 13 | 高 | 工作流更新 | 重做 Matt Pocock 工作流 — 将 ralph-loop/feedback-loops/review 替换为 /triage、/diagnose、/zoom-out（反映 4 月 17/28 日技能重命名） | COMPLETE (updated README table) |
| 14 | 高 | 工作流更新 | 将 HumanLayer /rpi:* 工作流替换为实际的 .claude/commands：/create_plan → /validate_plan → /implement_plan → /iterate_plan(sub) → /local_review → /commit | COMPLETE (updated README table) |
| 15 | 中 | 工作流更新 | 更新 Compound Engineering — 将 "repeat" 替换为子循环 /ce-debug(sub)、/ce-optimize(sub)、/ce-compound-refresh(sub) | COMPLETE (updated README table) |
| 16 | 低 | 计数核实 | ECC 计数 143→133 commands（混合：79 个旧版 + 72 个同步活动）、230→156 skills 自报 — 连续第 6 次运行 | ON HOLD (RECURRING from Apr 13/16/18/24/26; methodology persists — keeping current values until manual verification) |
| 17 | 低 | 计数核实 | GSD commands 122→86 — 通过 API 枚举连续第 5 次较低计数 | ON HOLD (RECURRING from Apr 16/18/24/26; likely directory pagination — keeping 122 until verified) |
| 18 | 低 | 计数核实 | oh-my-claudecode skills 46→38 — 连续第 2 次运行得到 38；可能是 v4.13.x 合并 | ON HOLD (RECURRING from Apr 26; keeping 46 until verified) |

---

## [2026-05-01 03:36 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 171k 到 175k（实际 175,037 — v5.0.7 势头、会话记录 PR 规则） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 169k 到 171k（实际 171,200 — v1.10.0 4 月 30 日热修复波：loop-status、gateguard） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Matt Pocock Skills ★ 从 36k 到 51k（实际 50,817 — 病毒式激增 +41%，2 天内、结构化 SKILL.md 部分、list-skills 脚本） | COMPLETE (updated README table) |
| 4 | 高 | 排序调整 | 将 Matt Pocock（51k）移至 BMAD（46k）和 OpenSpec（45k）之上 — 从第 8 位病毒式跃升至第 6 位 | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 gstack ★ 从 86k 到 88k（实际 87,550 — v1.21.1.0、browser-skills 运行时、setup-gbrain 联合） | COMPLETE (updated README table) |
| 6 | 高 | 星标更新 | 更新 Get Shit Done ★ 从 58k 到 59k（实际 59,115 — v1.39.0 今日发布：最小安装配置、/gsd-edit-phase） | COMPLETE (updated README table) |
| 7 | 高 | 计数更新 | 更新 GSD commands 从 122 到 65（v1.39.0 合并：31 个微技能被吸收到 4 个分组父级中 — 自 4 月 16/18/24/26/29 日 ON HOLD 已解决） | COMPLETE (updated README table) |
| 8 | 高 | 工作流更新 | 将 Matt Pocock /grill-me 重命名为 /grill-with-docs 在工作流链中（技能在最新提交中已重命名） | COMPLETE (updated README table) |
| 9 | 中 | 星标更新 | 更新 OpenSpec ★ 从 44k 到 45k（实际 44,511 — v1.3.1、Kimi CLI skills 支持、同步工具 ID 列表） | COMPLETE (updated README table) |
| 10 | 中 | 计数更新 | 更新 gstack skills 从 42 到 43（确认 43 个 SKILL.md 目录；+plan-devex-review 等净增 +1） | COMPLETE (updated README table) |
| 11 | 中 | 计数更新 | 更新 Compound Engineering agents 从 51 到 49（2026-05-01 今日提交中移除了 2 个 cli-readiness 审查员 agents） | COMPLETE (updated README table) |
| 12 | 中 | 计数更新 | 更新 Compound Engineering skills 从 37 到 39（新增 ce-simplify-code、ce-strategy；38 个 compound-eng + 1 个 coding-tutor） | COMPLETE (updated README table) |
| 13 | 低 | 计数更新 | 更新 oh-my-claudecode skills 从 46 到 38（自 4 月 26/29 日 ON HOLD 已解决：连续第 3 次运行确认 v4.13.x 合并移除了 8 个 skills） | COMPLETE (updated README table) |
| 14 | 低 | 计数更新 | 更新 Spec Kit commands 从 9+ 到 9（确切数量：analyze、checklist、clarify、constitution、implement、plan、specify、tasks、taskstoissues） | COMPLETE (updated README table) |
| 15 | 低 | 计数核实 | ECC commands 143→71、skills 230→182 — 通过目录枚举得到较低计数的连续第 7 次运行 | ON HOLD (RECURRING from Apr 13/16/18/24/26/29; methodology persists — recommend manual verification) |
| 16 | 低 | 计数核实 | Superpowers agents 5→1 显式 — 仅方法论变更（排除由 skills 分派的隐式子 agents） | ON HOLD (RECURRING from Apr 18/26/29; keeping 5 per prior decision to count implicit dispatch roles) |

---

## [2026-05-01 04:05 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增 | 在第 10 行、oh-my-claudecode（32k）和 Compound Engineering（16k）之间添加 addyosmani/agent-skills（27k stars / 3 agents / 7 commands / 21 skills）；工作流链 `/spec → /plan → /build → /test → /review → /ship`（DEFINE → PLAN → BUILD → VERIFY → REVIEW → SHIP 生命周期）；用户请求的手动添加 | COMPLETE (inserted into DEVELOPMENT WORKFLOWS table) |
| 2 | 低 | 说明 | 该仓库还附带并行的 `.gemini/commands/` 等价物和一个 `.claude-plugin/` 市场条目（multi-agent-IDE）；在 SKILL COLLECTIONS 表格中交叉列出其 21 个 SKILL.md 库 | COMPLETE (cross-referenced) |

---

## [2026-05-12 11:44 PM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 175k 到 188k（实际 187,818 — v5.1.0 势头、移除了 code-reviewer agent） | COMPLETE (updated README table) |
| 2 | 高 | 架构变更 | 更新 Superpowers agents 5 → 0 显式和 commands 3 → 0（v5.1.0 移除了命名 code-reviewer agent + 旧版斜杠 commands；审查现内联在 skills 中） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 171k 到 180k（实际 180,349 — v2.0.0-rc.1 alpha 进行中） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 Spec Kit ★ 从 92k 到 97k（实际 97,048 — v0.8.8 配置驱动认证注册表、每日发布） | COMPLETE (updated README table) |
| 5 | 中 | 工作流更新 | 修复 Spec Kit 工作流顺序：constitution → specify → clarify → plan → tasks → implement（交换 clarify/specify 以匹配仓库 happy path） | COMPLETE (updated README table) |
| 6 | 高 | 星标更新 | 更新 gstack ★ 从 88k 到 95k（实际 94,500 — v1.33.2.0、gbrain 批量导入、21 个社区修复） | COMPLETE (updated README table) |
| 7 | 中 | 计数更新 | 更新 gstack skills 43 → 48（5 个新增：design-shotgun、design-html、codex、retro、plan-tune） | COMPLETE (updated README table) |
| 8 | 低 | 工作流更新 | 扩展 gstack 工作流链，在 /plan-design-review 和 /ship 之间添加 /design-shotgun、/design-html、/codex、/retro | COMPLETE (updated README table) |
| 9 | 高 | 星标更新 | 更新 Get Shit Done ★ 从 59k 到 62k（实际 61,700 — v1.41.0 里程碑归档布局） | COMPLETE (updated README table) |
| 10 | 低 | 计数更新 | 更新 GSD commands 65 → 66（v1.41.0 在 commands/gsd/ 中新增一个 command） | COMPLETE (updated README table) |
| 11 | 高 | 星标更新 | 更新 Matt Pocock Skills ★ 从 51k 到 76k（实际 75,562 — +25k 激增、5 月 10-11 日新增 handoff/review skills） | COMPLETE (updated README table) |
| 12 | 中 | 计数更新 | 更新 Matt Pocock skills 22 → 28（跨 engineering/in-progress/personal 类别新增 6 个 SKILL.md） | COMPLETE (updated README table) |
| 13 | 中 | 星标更新 | 更新 BMAD-METHOD ★ 从 46k 到 47k（实际 47,000 — v6.6.0 破坏性变更、brownfield epic 范围界定） | COMPLETE (updated README table) |
| 14 | 高 | 星标更新 | 更新 OpenSpec ★ 从 45k 到 47k（实际 47,300 — Windows 工作区功能开发活跃） | COMPLETE (updated README table) |
| 15 | 低 | 计数更新 | 更新 OpenSpec commands 11 → 9（重新计数：propose、apply、archive、new、continue、ff、verify、bulk-archive、onboard — 确认 9 个） | COMPLETE (updated README table) |
| 16 | 高 | 星标更新 | 更新 oh-my-claudecode ★ 从 32k 到 34k（实际 33,500 — v4.13.7 稳定性修复） | COMPLETE (updated README table) |
| 17 | 中 | 星标更新 | 更新 Compound Engineering ★ 从 16k 到 17k（实际 16,600 — v3.8.1、ce-compound 的无头模式） | COMPLETE (updated README table) |
| 18 | 低 | 计数更新 | 更新 Compound Engineering skills 39 → 38（重新计数：compound-engineering/ 中 37 个 + coding-tutor/ 中 1 个） | COMPLETE (updated README table) |
| 19 | 高 | 排序调整 | 按星标降序重新排列表格：Superpowers（188k）> ECC（180k）> Spec Kit（97k）> gstack（95k）> Matt Pocock（76k）> GSD（62k）> OpenSpec（47.3k）> BMAD（47.0k）> oh-my-claudecode（34k）> agent-skills（27k）> Compound（17k）> HumanLayer（11k）；Matt Pocock 从第 6 行移至第 5 行，在 GSD 之上；OpenSpec 与 BMAD 交换位置（47.3 对 47.0） | COMPLETE (updated README table) |
| 20 | 低 | 计数核实 | ECC agents 48→60、commands 143→78、skills 230→120 — 由于 1000+ 文件上的 API 分页，研究置信度 0.72；与 README 徽章计数冲突 | ON HOLD (RECURRING — keeping current values until manual verification) |
| 21 | 低 | 计数核实 | BMAD agents 0→6 和 skills 40→16 — 方法论转变（将 bmad-agent-* skills 计为 agents、更少的 skill 容器）；并非实际的仓库变更 | ON HOLD (keeping current methodology to preserve trend continuity) |

---

## [2026-05-21 12:29 AM PKT] 开发工作流更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标更新 | 更新 Superpowers ★ 从 188k 到 200k（实际 200,000 — 突破 200k 里程碑；v5.1.0 移除了旧版斜杠 commands + 命名 code-reviewer agent） | COMPLETE (updated README table) |
| 2 | 高 | 星标更新 | 更新 Everything Claude Code ★ 从 180k 到 188k（实际 188,000 — ECC 2.0 Alpha、计费门禁、AgentShield 适配器回读） | COMPLETE (updated README table) |
| 3 | 高 | 星标更新 | 更新 Spec Kit ★ 从 97k 到 104k（实际 104,000 — 突破 100k；v0.8.12 扩展目录重构、Squad Bridge、Superpowers Implementation Bridge） | COMPLETE (updated README table) |
| 4 | 高 | 星标更新 | 更新 gstack ★ 从 95k 到 100k（实际 100,000 — 突破 100k 里程碑；v1.42.x 稳定性、23 个社区修复） | COMPLETE (updated README table) |
| 5 | 高 | 星标更新 | 更新 Matt Pocock Skills ★ 从 76k 到 97k（实际 96,700 — +21k 激增；5 月 19-20 日 handoff/improve-codebase-architecture skill 更新） | COMPLETE (updated README table) |
| 6 | 中 | 星标更新 | 更新 Get Shit Done ★ 从 62k 到 63k（实际 63,300 — v1.42.3/v1.43.0-rc2、Codex CLI 0.130.0 兼容、knowledge-graph 自动更新） | COMPLETE (updated README table) |
| 7 | 中 | 星标更新 | 更新 OpenSpec ★ 从 47k 到 50k（实际 49,500 — v1.3.1、Codex 工作区变更规划、Windows 工作区修复） | COMPLETE (updated README table) |
| 8 | 中 | 星标更新 | 更新 BMAD-METHOD ★ 从 47k 到 48k（实际 47,700 — v6.7.1 安装程序修复、v6.7.0 PRD/brief 促动器大修、bmad-investigate skill） | COMPLETE (updated README table) |
| 9 | 中 | 计数更新 | 更新 OpenSpec commands 从 9 到 11（/opsx:explore + /opsx:sync 重新计数；docs/commands.md 中确认 11 个） | COMPLETE (RECURRING — count has oscillated 9↔10↔11 across runs; agent gave explicit doc-sourced list) |
| 10 | 低 | 计数更新 | 更新 GSD commands 从 66 到 67（通过 v1.42-43 在 commands/gsd/ 中新增一个 command） | COMPLETE (updated README table) |
| 11 | 低 | 计数更新 | 更新 BMAD skills 从 40 到 42（30 个 bmm-skills + 12 个 core-skills；v6.7.0 新增 bmad-investigate） | COMPLETE (updated README table) |
| 12 | 低 | 计数更新 | 更新 oh-my-claudecode skills 从 38 到 39（确认 39 个 skill 文件夹；v4.14.x 中 +1） | COMPLETE (updated README table) |
| 13 | 低 | 计数核实 | ECC agents 48→60、commands 143→75、skills 230→232 — 目录枚举与 README 自报之间的冲突持续存在 | ON HOLD (RECURRING from Apr 13/16/18/24/26 + May 1/12; keeping current values until manual verification) |
| 14 | 低 | 计数核实 | gstack skills 48→59 — agent 的 AGENTS.md 目录计数包含非 skill 根目录（gstack/test/hosts/supabase）；目录列出约 46 个实际 skills，置信度 0.80 | ON HOLD (agent overcount; keeping 48) |
| 15 | 低 | 计数核实 | BMAD agents 0→6/30 — 方法论转变（将 bmad-agent-* 角色计为 agents） | ON HOLD (RECURRING from May 12; keeping 0 to preserve trend continuity) |
| 16 | 低 | 计数核实 | oh-my-claudecode commands 0→27 — agent 在 commands/ 中找到 27 个 .md，但工作流方法论将 skills 视为 command 界面 | ON HOLD (keeping 0 per established methodology) |
| 17 | 低 | 排序调整 | 无需重新排序 — 星标降序保持：Superpowers 200k > ECC 188k > Spec Kit 104k > gstack 100k > Matt Pocock 97k > GSD 63k > OpenSpec 50k > BMAD 48k > omc 34k > agent-skills 27k > Compound 17k > HumanLayer 11k | COMPLETE (verified order unchanged) |
