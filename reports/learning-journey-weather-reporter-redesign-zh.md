# 学习历程 — 天气记者重新设计计划

← 返回 [README](../README.md)

## 概述

围绕一个贯穿始终的示例重新设计第7张幻灯片之后的所有幻灯片：**天气记者 agent**。叙述线索与 TOC 可见顺序一致（Agents → Skills → Context → CLAUDE.md → Commands+Workflow），让观众先认识天气记者，然后了解他们知道什么、如何思考、遵循哪些规则，最后了解如何用一个命令触发他们。

---

## 1. 当前 → 新章节映射

| 当前章节 | 当前幻灯片 | 操作 | 新位置 |
|---|---|---|---|
| 主题 1: Context | 7-11 (章节起始于 7) | 移至主题 3 | 幻灯片 17-21 |
| 主题 2: CLAUDE.md | 12-17 (章节起始于 12) | 移至主题 4 | 幻灯片 22-27 |
| 主题 3: Agents | 18-23 (章节起始于 18) | 移至主题 1 | 幻灯片 7-12 |
| 主题 4: Skills | 24-29 (章节起始于 24) | 移至主题 2 | 幻灯片 13-18 |
| 主题 5: Commands | 30-32 (章节起始于 30) | 与 Workflow 合并为主题 5 | 幻灯片 28-32 |
| 主题 6: Workflow | 33-36 (章节起始于 33) | 合并到 Commands 章节 | (无独立章节幻灯片) |
| 结束幻灯片 | 37 | 保留，更新副标题 | 幻灯片 33 |

**新总计: 33 张幻灯片**（当前 37 张减去 Workflow 章节幻灯片和 3 张合并到 Commands 章节的 Workflow 内容幻灯片，后者增加 3 张）。

等等——让我重新计算：

当前: 幻灯片 7-37 = 31 张。
- Agents 章节: 6 张 (18-23) → 成为主题 1 (7-12)
- Skills 章节: 6 张 (24-29) → 成为主题 2 (13-18)
- Context 章节: 5 张 (7-11) → 成为主题 3 (19-23)
- CLAUDE.md 章节: 6 张 (12-17) → 成为主题 4 (24-29)
- Commands+Workflow 合并: 3 + 1 章节 + 4 内容 = Commands (3) + Workflow (1 章节 + 3 内容) = 7 张 → 成为主题 5 (30-36)
- 结束: 1 张 (37)

**新总计: 37 张幻灯片。**（没有幻灯片被删除；Workflow 章节幻灯片成为合并后的 Commands+Workflow 章节的一部分——我们将其保留为子章节或移除其 `data-level` 以避免出现第二个章节分隔。）

**决定**: 保留全部 37 张幻灯片。移除旧 Workflow 章节分隔（幻灯片 33）上的 `data-level`，使其被视为内容幻灯片而非章节分隔。Commands 章节覆盖 30-36。Workflow 章节分隔成为 Commands 章节内的一个可视化"章节标题"。

实际上，更简单的方法：将 Workflow 章节分隔保留为内容幻灯片，不带 `data-level`。历程栏保持在 `commands` 级别。章节编号文字从"主题 6"改为子标题。

---

## 2. 新的 LEVELS 映射（键和颜色不变）

新的章节顺序为：**Agents → Skills → Context → CLAUDE.md → Commands**。`workflow` 级别键从 `data-level` 使用中退役（章节分隔失去 `data-level`）。`LEVELS` 映射仍保留 `workflow` 用于历程栏历史显示，但没有幻灯片触发它。

**修订方案**: 由于没有幻灯片携带 `data-level="workflow"`，将 `workflow` 级别完全从 LEVELS 映射中移除。历程栏最高到 `commands` (83%)。这没问题——Workflow 章节作为 Commands 章节内部的*高潮*呈现，而不是一个独立主题。

实际上，历程栏填充到 83% 而不是 100% 作为结束章节，这令人不满意。更好的方案：**将 Commands+Workflow 合并为一个名为"Commands & Workflow"的章节**，使用 `data-level="commands"`。在 LEVELS 中将 `workflow` 级别保留为 100%，并将 `data-level="workflow"` 分配给*旧的* workflow 章节分隔幻灯片——它成为 Commands 章节内的一个视觉过渡。这样，历程栏在 workflow 幻灯片处填满到 100%。

**最终决定**: 在 LEVELS 中同时保留 `commands` (83%) 和 `workflow` (100%)。将 `data-level="commands"` 分配给 Commands 章节分隔，将 `data-level="workflow"` 分配给 Workflow 子章节幻灯片。历程标记保持不变。这与当前结构完全一致——只是内容幻灯片重新排序。

---

## 3. 逐张幻灯片内容大纲

### 幻灯片 1-6（不变）

幻灯片 1（标题）、2（Boris GIF）、3（Vibe→Agentic）、4（什么是 Vibe Coding）、5（好提示与差提示）、6（TOC——仅更新 `goToSlide` 目标）。

**幻灯片 6 上的 TOC 更新：**
- Agents 行: `goToSlide(7)`（原为 18）
- Skills 行: `goToSlide(13)`（原为 24）
- Context 行: `goToSlide(19)`（原为 7）
- CLAUDE.md 行: `goToSlide(25)`（原为 12）
- Commands 行: `goToSlide(30)`（原为 30——不变）

---

### 章节 1: Agents（幻灯片 7-12）——"这个人"

**幻灯片 7** — 章节分隔（`data-level="agents"`，主题 1）
- 标题: "Agents — 天气记者"
- 描述: "Agent 是扮演特定角色的 Claude。认识一下天气记者——一位受雇获取并报告迪拜天气数据的专家。"

**幻灯片 8** — "餐厅厨房"（当前幻灯片 19）
- 内容: 保持相同类比（普通提示 = 在随机厨房里喊叫；agent = 特定专家）
- 更新 agent 示例，全程使用"天气记者"框架
- 保留比较普通提示与 weather-agent 的两栏卡片

**幻灯片 9** — "Prompting vs. Agent — 并排对比"（当前幻灯片 20）
- 保持表格完整。已经很好地使用了天气示例。

**幻灯片 10** — "Agents 拥有自己的大脑"（当前幻灯片 21）
- 保留 Thariq 的建议。与之关联："天气记者在自己的大脑中工作——所有网络抓取都留在你的大脑之外。"

**幻灯片 11** — "如何创建你自己的 Agent"（当前幻灯片 22）
- 保留 `/agents` 操作指南模式
- 更新代码块以显示真实的 `weather-agent.md` 路径

**幻灯片 12** — "Agent 配置字段"（当前幻灯片 23）
- 保留字段行表格。添加一个标注框，展示 `skills: [weather-fetcher]` 字段在上下文中的应用。

---

### 章节 2: Skills（幻灯片 13-18）——"记者知道什么"

**幻灯片 13** — 章节分隔（`data-level="skills"`，主题 2）
- 标题: "Skills — 天气记者知道什么"
- 描述: "Skills 是记者经过训练能够完成的特定任务。我们的记者有两项：获取数据，并将其渲染为卡片。"

**幻灯片 14** — "训练手册"（当前幻灯片 25）
- 重新框架：天气记者有两项 skill：weather-fetcher（获取温度）和 weather-svg-creator（创建可视化卡片）。
- 用天气记者的两项 skill 替换"Shayan"示例。

**幻灯片 15** — "何时将某事物转化为 Skill"（当前幻灯片 26）
- 保留 Boris 建议。添加 weather-fetcher 和 weather-svg-creator 作为示例之二。

**幻灯片 16** — "为什么将 Agents 和 Skills 分开？"（当前幻灯片 27）
- 保留两栏布局。更新以强调：weather-agent = 这个人，weather-fetcher = 他们的训练。

**幻灯片 17** — "如何创建你自己的 Skill"（当前幻灯片 28）
- 保留。代码块已经展示了真实的 `weather-fetcher` SKILL.md 内容——完美。

**幻灯片 18** — "Skill 配置字段"（当前幻灯片 29）
- 保留。添加说明：weather-fetcher 上设置了 `user-invocable: false`，因为它仅供 agent 使用。

---

### 章节 3: Context（幻灯片 19-23）——"记者的大脑"

**幻灯片 19** — 章节分隔（`data-level="context"`，主题 3）
- 标题: "Context — 记者的大脑"
- 描述: "现在你已经认识了记者并了解他们的技能，让我们来理解他们实际上能同时记住多少信息。"

**幻灯片 20** — "Claude 的大脑"（当前幻灯片 8）
- 保留。添加一句与天气记者关联的话："当 weather-agent 被派遣时，它获得自己全新的大脑——并且 weather-fetcher 在启动时就被固定在其中。"
- 保留两个图示（context-window.jpeg 保留在此处）。

**幻灯片 21** — "会话启动时加载什么"（当前幻灯片 9）
- 保留。与天气记者关联："启动时，Claude 知道*关于* weather-fetcher 的信息（仅描述）。当命令运行时，完整的 skill 内容被加载到 agent 的大脑中。"
- 保留 context.jpg 在此处。

**幻灯片 22** — "保持大脑清晰"（当前幻灯片 10）
- 保留分支点表格。

**幻灯片 23** — "如何管理你的 Context"（当前幻灯片 11）
- 保留 `/context`、`/compact`、`/clear` 操作指南。

---

### 章节 4: CLAUDE.md（幻灯片 24-29）——"口袋规则手册"

**幻灯片 24** — 章节分隔（`data-level="claude-md"`，主题 4）
- 标题: "CLAUDE.md — 记者的口袋规则手册"
- 描述: "记者每次轮班开始时都会查阅这本手册——即使他们的大脑在夜间重置。"

**幻灯片 25** — "员工手册"（当前幻灯片 13）
- 保留。更新为天气记者框架：CLAUDE.md 是记者在播报前阅读的规则手册——"除非被要求，始终以摄氏度报告，始终注明来源。"

**幻灯片 26** — "如何创建你的 CLAUDE.md"（当前幻灯片 14）
- 保留 `/init` 操作指南。

**幻灯片 27** — "每次犯错后完善 CLAUDE.md"（当前幻灯片 15）
- 保留 Boris 建议。

**幻灯片 28** — "CLAUDE.md 中包含什么"（当前幻灯片 16）
- 保留代码块。天气记者关联：添加一条注释，展示天气特定的规则。

**幻灯片 29** — "CLAUDE.md 如何加载"（当前幻灯片 17）
- 保留。

---

### 章节 5: Commands + Workflow（幻灯片 30-36）——"触发器"

**幻灯片 30** — 章节分隔（`data-level="commands"`，主题 5）
- 标题: "Commands — 触发器"
- 描述: "一个词启动整个链条。`/weather-orchestrator` → agent → skill → SVG 卡片。"

**幻灯片 31** — "Commands — 入口点"（当前幻灯片 31）
- 保留。很好的介绍。已经引用了 weather-orchestrator。

**幻灯片 32** — "如何创建你自己的 Command"（当前幻灯片 32）
- 保留。代码块已经展示了 weather-orchestrator.md。

**幻灯片 33** — Workflow 子章节（原为幻灯片 33，`data-level="workflow"`）
- 将章节编号文字从"主题 6"改为"融会贯通"
- 保留 `data-level="workflow"` 以便历程栏填满至 100%。
- 将标题更新为："Workflow — 五个部分整合在一起"
- 描述: "观看天气记者示例从一次按键运行到 SVG 卡片输出。"

**幻灯片 34** — "Command → Agent → Skill"（当前幻灯片 34）
- 保留代码块流程图示。它已经很完美了。

**幻灯片 35** — "Skills 的两种使用方式"（当前幻灯片 35）
- 保留比较预加载与直接调用的两栏布局。

**幻灯片 36** — "如何搭建你自己的 Workflow"（当前幻灯片 36）
- 保留。已经使用天气 workflow 作为示例。

**幻灯片 37** — 结束（当前幻灯片 37）
- 保留。将副标题更新为："五个概念，一个贯穿始终的示例"
- 更新正文文字以引用天气记者的叙述线索。

---

## 4. 资产复用清单

| 资产 | 当前位置 | 新位置 | 操作 |
|---|---|---|---|
| `context-window.jpeg` | 幻灯片 8 (Claude's Brain) | 幻灯片 20 (相同内容，重新编号) | 保留——无需更改 |
| `context.jpg` | 幻灯片 9 (What Loads at Session Start) | 幻灯片 21 (相同内容，重新编号) | 保留——无需更改 |
| `../../!/claude-jumping.svg` | 幻灯片 1，页眉 | 不变 | 无需操作 |
| `../../!/root/boris-slider.gif` | 幻灯片 2 | 不变 | 无需操作 |

两个 context 图示原封不动地保留——包含它们的幻灯片只是被重新编号（8→20，9→21）。

---

## 5. 簿记影响

### 新的章节分隔位置和 `data-level` 分配

| 幻灯片 | 主题 | `data-level` |
|---|---|---|
| 7 | Agents | `agents` |
| 13 | Skills | `skills` |
| 19 | Context | `context` |
| 25 | CLAUDE.md | `claude-md` |
| 30 | Commands | `commands` |
| 33 | Workflow 子章节 | `workflow` |

### TOC `goToSlide` 目标（幻灯片 6）

| 行 | 主题 | 旧目标 | 新目标 |
|---|---|---|---|
| 行 1 | Agents | 18 | 7 |
| 行 2 | Skills | 24 | 13 |
| 行 3 | Context | 7 | 19 |
| 行 4 | CLAUDE.md | 12 | 25 |
| 行 5 | Commands | 30 | 30 |

### 历程标记（不变）

历程标记轨道已按从上到下的顺序排列为：Workflow, Commands, Skills, Agents, CLAUDE.md, Context。这与叙述线索顺序*相反*（顶部 = 最高级别 = 最后达到）。无需更改。

### LEVELS 映射（不变）

所有 6 个级别键（`context`, `claude-md`, `agents`, `skills`, `commands`, `workflow`）保持不变。没有添加或移除任何键。

---

## 6. 实施方案

HTML 是一个大文件。对于新的叙述线索，幻灯片的顺序是错误的。最清晰的实施方法是：

1. 剪切幻灯片 div 并按新顺序粘贴（7-12 = 旧 18-23，13-18 = 旧 24-29，19-23 = 旧 7-11，24-29 = 旧 12-17，30-37 不变）。
2. 按顺序重新编号所有 `data-slide` 属性。
3. 更新章节幻灯片的 `data-level` 属性。
4. 更新章节分隔上的章节编号文字和 h1。
5. 更新幻灯片 6 上的 TOC `goToSlide` 目标。
6. 更新 Workflow 章节幻灯片（旧幻灯片 33）的章节编号文字。
7. 在需要的地方对天气记者框架进行有针对性的内容编辑。

幻灯片总数：**37**（不变）。

---

## 7. 模糊点——无关键性影响

所有模糊点已在上述内容中解决。直接进入实施阶段。
