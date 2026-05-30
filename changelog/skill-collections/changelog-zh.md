# Skill Collections 变更日志

**状态图例:**

| 状态 | 含义 |
|--------|---------|
| 已完成 (原因) | 已执行操作并成功解决 |
| 无效 (原因) | 发现不正确、不适用或属于有意为之 |
| 待处理 (原因) | 操作已推迟，等待外部依赖或用户决定 |

---

## [2026-04-28 04:39 PM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 低 | 初始运行 | 在 README 中创建了 SKILL COLLECTIONS 部分，包含 5 个仓库：anthropics/skills (125k/17), wshobson/agents (35k/152), mattpocock/skills (33k/17), K-Dense-AI/scientific-agent-skills (20k/134), VoltAgent/awesome-agent-skills (19k/1,100+ 精选) | 已完成 (从 research-agent 发现结果进行初始填充，2026-04-28 会话) |

---

## [2026-04-29 12:52 AM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 星标数 | 更新 mattpocock/skills ★ 从 33k 到 36k (精确值 36,476) | 新增 |
| 2 | 中 | 数量 | 更新 mattpocock/skills skill 数量从 17 到 18 (添加了 setup-matt-pocock-skills，重新组织了 deprecated/ 文件夹，2026-04-28) | 新增 |
| 3 | 低 | 星标数 | 更新 wshobson/agents ★ 从 35k 到 34k (精确值 34,477 — 略有下降) | 新增 |
| 4 | 中 | 排序 | 将 mattpocock/skills 行移至 wshobson/agents 行之上 (因星标数变化导致排名互换) | 新增 |
| 5 | 低 | 数量 | 更新 VoltAgent/awesome-agent-skills 精选数量从 1,100+ 到 930+ (实际 README 列表项解析；徽章多算了约 170) | 新增 |
| 6 | 低 | 无变更 | anthropics/skills (125k/17) 和 K-Dense-AI/scientific-agent-skills (20k/134) — 数值匹配，无需编辑 | 已完成 (已验证，无偏差) |

---

## [2026-05-01 03:31 PM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 星标数 | 更新 anthropics/skills ★ 从 125k 到 127k (精确值 126,746) | 新增 |
| 2 | 高 | 星标数 | 更新 mattpocock/skills ★ 从 36k 到 51k (精确值 50,819 — 约 3 天内激增 +15k，可能受外部因素放大) | 新增 |
| 3 | 低 | 星标数 | 更新 wshobson/agents ★ 从 34k 到 35k (精确值 34,595) | 新增 |
| 4 | 低 | 星标数 | 更新 VoltAgent/awesome-agent-skills ★ 从 19k 到 20k (精确值 19,729) | 新增 |
| 5 | 低 | 无变更 | 全部 5 个 skill 数量保持不变 (anthropics 17, mattpocock 18, wshobson 152, scientific 134, voltagent 930 精选) | 已完成 (已验证，无偏差) |
| 6 | 低 | 排序 | 顺序保持不变 — scientific (19,829) 仍领先 voltagent (19,729) 约 100 星；无需重新排序行 | 已完成 (已验证) |

---

## [2026-05-01 04:05 PM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增 | 在第 4 行添加了 addyosmani/agent-skills (27k 星 / 21 个 SKILL.md 文件)，位于 wshobson/agents (35k) 和 scientific-agent-skills (20k) 之间；用户请求的手动添加 | 已完成 (已插入 SKILL COLLECTIONS 表格) |
| 2 | 低 | 备注 | 仓库具有双重分类 — 同时被添加到 DEVELOPMENT WORKFLOWS 表格中，因为它提供了完整的 /spec → /plan → /build → /test → /review → /ship 生命周期，而不仅仅是 SKILL.md 库 | 已完成 (已交叉引用) |

---

## [2026-05-12 11:40 PM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标数 | 更新 anthropics/skills ★ 从 127k 到 133k (精确值 132,946) | 新增 |
| 2 | 高 | 星标数 | 更新 mattpocock/skills ★ 从 51k 到 76k (精确值 75,562 — 约 11 天内激增 +25k，连续第二次放大事件) | 重复 (类似 +15k 激增记录于 2026-05-01) |
| 3 | 中 | 数量 | 更新 mattpocock/skills 活跃 skill 数量从 18 到 24 (添加了 handoff 2026-05-11, review 2026-05-10，以及 engineering/in-progress 新增内容；4 个已弃用的保持不变) | 新增 |
| 4 | 低 | 数量 | 更新 wshobson/agents skill 数量从 152 到 153 (README 数量同步至 2026-05-09 提交) | 新增 |
| 5 | 低 | 星标数 | 更新 K-Dense-AI/scientific-agent-skills ★ 从 20k 到 21k (精确值 20,758) | 新增 |
| 6 | 低 | 数量 | 更新 K-Dense-AI/scientific-agent-skills 数量从 134 到 135 (添加了 exa-search 2026-05-06 PR #143, autoskill 2026-05-03 PR #141) | 新增 |
| 7 | 中 | 星标数 | 更新 VoltAgent/awesome-agent-skills ★ 从 20k 到 21k (精确值 21,417 — 星标数已超过 K-Dense-AI) | 新增 |
| 8 | 中 | 数量 | 更新 VoltAgent/awesome-agent-skills 精选数量从 930+ 到 1,100+ (恢复使用 README 徽章作为来源；之前的 930+ 是保守的列表项解析) | 重复 (数量来源方法于 2026-04-29 有争议) |
| 9 | 高 | 排序 | 交换第 5 行 (K-Dense-AI 20,758) 和第 6 行 (VoltAgent 21,417) — VoltAgent 因领先约 660 星而上移 | 新增 |
| 10 | 低 | 无变更 | addyosmani/agent-skills (27k/21) 保持不变 — 超出标准的 5 仓库研究范围，等待单独审查 | 已完成 (已验证，手动条目已保留) |

---

## [2026-05-13 PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增 | 在第 4 行添加了 pbakaus/impeccable (27k 星 / 1 个 SKILL.md 包含 7 个设计领域引用)，位于 wshobson/agents (35k) 和 addyosmani/agent-skills (27k) 之间；用户请求的手动添加 | 已完成 (已插入 SKILL COLLECTIONS 表格) |
| 2 | 低 | 备注 | 单 skill 仓库，包含 7 个参考文件 (typography, color-and-contrast, spatial-design, motion-design, interaction-design, responsive-design, ux-writing)，23 个命令，27 条反模式规则 — 面向前端 AI 工作的设计语言 skill | 已完成 (数量标注遵循 VoltAgent 模式的括号说明) |

---

## [2026-05-13 01:28 AM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增 | 在 SKILL COLLECTIONS 表格第 8 行添加了 alirezarezvani/claude-skills (精确值 14,550 → 15k / 246 个 skill 涵盖 9 个领域) (位于 K-Dense-AI/scientific-agent-skills 21k 之后)；用户请求的手动添加 | 已完成 (已插入 SKILL COLLECTIONS 表格) |
| 2 | 中 | 备注 | 将 SKILL COLLECTIONS 经验性星标下限从 21k 降低至约 15k。该表格没有明确的星标阈值记录 (只有 AGENT COLLECTIONS 和 CROSS-MODEL WORKFLOWS 有 10k+ 规则)，因此这是一个开创先例的新增，而非违反规则 | 已完成 (决策已记录) |
| 3 | 低 | 备注 | 仓库设计为跨工具 (支持 Claude Code, Codex, Gemini CLI, Cursor 以及根据其 README 描述的另外 8 个)。未来审查中可能加入 CROSS-MODEL WORKFLOWS 表格，但根据用户指示分类至此 | 已完成 (跨分类已备注) |

---

## [2026-05-20 11:55 PM PKT] Skill Collections 更新

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 星标数 | 更新 mattpocock/skills ★ 从 76k 到 97k (精确值 96,663 — 约 8 天内激增 +21k，连续第三次放大事件) | 重复 (类似 +25k 激增记录于 2026-05-12, +15k 记录于 2026-05-01) |
| 2 | 中 | 星标数 | 更新 anthropics/skills ★ 从 133k 到 138k (精确值 138,169) | 重复 (常规星标增长，记录于 2026-05-12) |
| 3 | 低 | 星标数 | 更新 wshobson/agents ★ 从 35k 到 36k (精确值 35,706) | 重复 (星标增长记录于 2026-05-01, 2026-05-12) |
| 4 | 低 | 数量 | 更新 wshobson/agents skill 数量从 153 到 155 (添加了 recsys-pipeline-architect 2026-05-17, ship-mate 插件 2026-05-11) | 重复 (数量偏差记录于 2026-05-12) |
| 5 | 中 | 星标数 | 更新 K-Dense-AI/scientific-agent-skills ★ 从 21k 到 25k (精确值 24,924 — +4k，超过了 VoltAgent) | 重复 (星标增长记录于 2026-05-12) |
| 6 | 低 | 数量 | 更新 K-Dense-AI/scientific-agent-skills 数量从 135 到 138 (v2.39.0 社区贡献 2026-05-19, Hugging Science 2026-05-01) | 重复 (数量偏差记录于 2026-05-12) |
| 7 | 低 | 星标数 | 更新 VoltAgent/awesome-agent-skills ★ 从 21k 到 22k (精确值 22,473) | 重复 (星标增长记录于 2026-05-12) |
| 8 | 中 | 排序 | 将 K-Dense-AI (24,924) 交换至 VoltAgent (22,473) 之上 — K-Dense-AI 以约 2,450 星的领先优势重新夺回更高排名 | 重复 (逆转了 2026-05-12 记录的 VoltAgent 上移交换) |
| 9 | 低 | 无变更 | anthropics 和 mattpocock 活跃 skill 数量保持不变 (17, 24)；VoltAgent 精选数量保持不变 (1,100+) | 已完成 (已验证，无偏差) |
| 10 | 低 | 无变更 | 手动条目保持不变 — impeccable (27k/1), addyosmani/agent-skills (27k/21), alirezarezvani/claude-skills (15k/246) — 超出 5 仓库研究范围 | 已完成 (已验证，手动条目已保留) |
