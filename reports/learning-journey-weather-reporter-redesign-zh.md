# Learning Journey — Weather Reporter 重新设计方案

← 返回 [README](../README.md)

## 概述

围绕一个贯穿始终的示例——**weather reporter agent**——重新设计 slide 7 及之后的所有 slides。叙事弧线匹配 TOC 可见顺序（Agents → Skills → Context → CLAUDE.md → Commands+Workflow），让观众先认识 weather reporter，然后理解它知道什么、如何思考、遵循什么规则，最后了解如何用一个 command 触发它。

---

## 1. 当前 → 新 Section 映射

| Current section | Current slides | Action | New position |
|---|---|---|---|
| Topic 1: Context | 7-11（section 在 7） | 移至 Topic 3 | slides 17-21 |
| Topic 2: CLAUDE.md | 12-17（section 在 12） | 移至 Topic 4 | slides 22-27 |
| Topic 3: Agents | 18-23（section 在 18） | 移至 Topic 1 | slides 7-12 |
| Topic 4: Skills | 24-29（section 在 24） | 移至 Topic 2 | slides 13-18 |
| Topic 5: Commands | 30-32（section 在 30） | 与 Workflow 合并为 Topic 5 | slides 28-32 |
| Topic 6: Workflow | 33-36（section 在 33） | 合并到 Commands section | （无独立 section slide） |
| Closing slide | 37 | 保留，更新副标题 | slide 33 |

**新总计：33 slides**（与当前的 37 相同，减去 Workflow section slide 和 3 个合并到 Commands section 的 Workflow content slides，该 section 因此增加 3 个 slides）。

等等——让我重新计数：

当前：slides 7-37 = 31 slides。
- Agents section：6 slides（18-23）→ 变为 Topic 1（7-12）
- Skills section：6 slides（24-29）→ 变为 Topic 2（13-18）
- Context section：5 slides（7-11）→ 变为 Topic 3（19-23）
- CLAUDE.md section：6 slides（12-17）→ 变为 Topic 4（24-29）
- Commands+Workflow 合并：3 + 1 section + 4 content = Commands (3) + Workflow (1 section + 3 content) = 7 slides → 变为 Topic 5 (30-36)
- Closing：1 slide (37)

**新总计：37 slides。**（没有 slides 被删除；Workflow section slide 成为合并的 Commands+Workflow section 的一部分——我们保留它作为子 section 或移除其 `data-level` 以避免第二个 section divider。）

**决定**：保留所有 37 个 slides。移除旧的 Workflow section divider（slide 33）上的 `data-level`，使其被视为 content slide 而非 section divider。Commands section 涵盖 30-36。Workflow section divider 成为 Commands section 内的视觉"章节标题"。

实际上，更简单：保留 Workflow section divider 作为无 `data-level` 的 content slide。journey bar 停留在 `commands` 级别。section 编号文本从"Topic 6"改为仅一个子标题。

---

## 2. 新的 LEVELS 映射（键或颜色不变）

新的 section 顺序是：**Agents → Skills → Context → CLAUDE.md → Commands**。`workflow` 级别键从 `data-level` 使用中退役（section divider 失去 `data-level`）。`LEVELS` 映射仍为 journey-bar 历史显示保留 `workflow`，但没有 slide 触发它。

**修订后方法**：完全从 LEVELS 映射中删除 `workflow` 级别，因为没有 slide 带有 `data-level="workflow"`。journey bar 最高到 `commands`（83%）。这没问题——Workflow section 是作为 Commands section *内部*的高潮呈现的，而不是独立主题。

实际上，journey bar 在 closing section 填到 83% 而非 100% 让人不太满意。更好的方案：**将 Commands+Workflow 合并为一个名为 "Commands & Workflow" 的 section**，使用 `data-level="commands"`。在 LEVELS 中保留 `workflow` 级别在 100%，并为*旧的* workflow section-divider slide 分配 `data-level="workflow"`——它成为 Commands section 内的视觉过渡。这样 bar 在 workflow slides 处填到 100%。

**最终决定**：在 LEVELS 中同时保留 `commands` (83%) 和 `workflow` (100%)。为 Commands section divider 分配 `data-level="commands"`，为 Workflow 子 section slide 分配 `data-level="workflow"`。Journey ticks 保持不变。这与当前结构完全匹配——只是 content slides 重新排序。

---

## 3. Slide-by-Slide 内容大纲

### Slides 1-6（不变）

Slides 1（title）、2（Boris GIF）、3（Vibe→Agentic）、4（What is Vibe Coding）、5（Good vs Bad Prompts）、6（TOC——仅更新 goToSlide 目标）。

**Slide 6 上的 TOC 更新：**
- Agents 行：`goToSlide(7)`（原为 18）
- Skills 行：`goToSlide(13)`（原为 24）
- Context 行：`goToSlide(19)`（原为 7）
- CLAUDE.md 行：`goToSlide(25)`（原为 12）
- Commands 行：`goToSlide(30)`（原为 30——不变）

---

### Section 1：Agents（slides 7-12）——"人"

**Slide 7** — Section divider（`data-level="agents"`，Topic 1）
- 标题："Agents — The Weather Reporter"
- 描述："An agent is Claude playing a specific role. Meet the weather reporter — a specialist hired to fetch and report weather data for Dubai."

**Slide 8** — "The Restaurant Kitchen"（当前 slide 19）
- 内容：相同的类比（plain prompting = 在随机厨房里大喊；agent = 特定专家）
- 更新 agent 示例，全程使用"weather reporter"框架
- 保留对比 plain prompting vs weather-agent 的两列卡片

**Slide 9** — "Prompting vs. Agent — Side by Side"（当前 slide 20）
- 保留表格不变。已经很好地使用了天气示例。

**Slide 10** — "Agents Get Their Own Brain"（当前 slide 21）
- 保留 Thariq 的提示。关联到："the weather reporter works in their own brain — all that web fetching stays out of yours."

**Slide 11** — "How to Create Your Own Agent"（当前 slide 22）
- 保留 `/agents` how-to 模式
- 更新代码块以显示真实的 `weather-agent.md` 路径

**Slide 12** — "Agent Config Fields"（当前 slide 23）
- 保留 field-row 表格。添加一个 callout box，在 context 中显示 `skills: [weather-fetcher]` 字段。

---

### Section 2：Skills（slides 13-18）——"Reporter 知道什么"

**Slide 13** — Section divider（`data-level="skills"`，Topic 2）
- 标题："Skills — What the Weather Reporter Knows"
- 描述："Skills are the specific things the reporter has been trained to do. Our reporter has two: fetch the data, and render it as a card."

**Slide 14** — "The Training Manual"（当前 slide 25）
- 重新框架：weather reporter 有两个 skills：weather-fetcher（获取温度）和 weather-svg-creator（创建视觉卡片）。
- 用 weather reporter 的两个 skills 替换"Shayan"示例。

**Slide 15** — "When to Turn Something Into a Skill"（当前 slide 26）
- 保留 Boris 提示。将 weather-fetcher 和 weather-svg-creator 添加为其中两个示例。

**Slide 16** — "Why Separate Agents and Skills?"（当前 slide 27）
- 保留两列。更新以强调：weather-agent = 人，weather-fetcher = 他们的培训。

**Slide 17** — "How to Create Your Own Skill"（当前 slide 28）
- 保留。代码块已经显示了真实的 `weather-fetcher` SKILL.md 内容——完美。

**Slide 18** — "Skill Config Fields"（当前 slide 29）
- 保留。添加注释：weather-fetcher 上设置了 `user-invocable: false`，因为它是 agent-only 的。

---

### Section 3：Context（slides 19-23）——"Reporter 的大脑"

**Slide 19** — Section divider（`data-level="context"`，Topic 3）
- 标题："Context — The Reporter's Brain"
- 描述："Now that you've met the reporter and know their skills, let's understand what they can actually hold in mind at once."

**Slide 20** — "Claude's Brain"（当前 slide 8）
- 保留。添加一句关联到 weather reporter："When the weather-agent is dispatched, it gets its own fresh brain — and weather-fetcher is pinned into it at startup."
- 保留两个图表（context-window.jpeg 留在此处）。

**Slide 21** — "What Loads at Session Start"（当前 slide 9）
- 保留。关联到 weather reporter："At startup, Claude knows *about* weather-fetcher (description only). When the command runs, the full skill content is loaded into the agent's brain."
- 将 context.jpg 保留在此处。

**Slide 22** — "Keep the Brain Clear"（当前 slide 10）
- 保留 branching-point 表格。

**Slide 23** — "How to Manage Your Context"（当前 slide 11）
- 保留 `/context`、`/compact`、`/clear` 的 how-to。

---

### Section 4：CLAUDE.md（slides 24-29）——"口袋规则手册"

**Slide 24** — Section divider（`data-level="claude-md"`，Topic 4）
- 标题："CLAUDE.md — The Reporter's Pocket Rulebook"
- 描述："The reporter consults this at the start of every shift — even though their brain resets overnight."

**Slide 25** — "The Employee Handbook"（当前 slide 13）
- 保留。更新为 weather-reporter 框架：CLAUDE.md 是 reporter 上镜前阅读的规则手册——"always report in Celsius unless asked, always cite the source."

**Slide 26** — "How to Create Your CLAUDE.md"（当前 slide 14）
- 保留 `/init` how-to。

**Slide 27** — "Grow CLAUDE.md With Every Mistake"（当前 slide 15）
- 保留 Boris 提示。

**Slide 28** — "What Goes in CLAUDE.md"（当前 slide 16）
- 保留代码块。Weather reporter 触感：添加一个注释展示天气特定规则。

**Slide 29** — "How CLAUDE.md Loads"（当前 slide 17）
- 保留。

---

### Section 5：Commands + Workflow（slides 30-36）——"触发器"

**Slide 30** — Section divider（`data-level="commands"`，Topic 5）
- 标题："Commands — The Trigger"
- 描述："One word kicks off the whole chain. `/weather-orchestrator` → agent → skill → SVG card."

**Slide 31** — "Commands — The Entry Point"（当前 slide 31）
- 保留。很好的介绍。已引用 weather-orchestrator。

**Slide 32** — "How to Create Your Own Command"（当前 slide 32）
- 保留。代码块已显示 weather-orchestrator.md。

**Slide 33** — Workflow 子 section（原为 slide 33，`data-level="workflow"`）
- 将 section-number 文本从"Topic 6"改为"Putting It All Together"
- 保留 `data-level="workflow"`，使 bar 填到 100%。
- 更新标题为："Workflow — All Five Pieces Together"
- 描述："Watch the weather reporter example run from one keystroke to SVG card output."

**Slide 34** — "Command → Agent → Skill"（当前 slide 34）
- 保留代码块流程图。已经很完美。

**Slide 35** — "Two Ways Skills Are Used"（当前 slide 35）
- 保留两列对比 preloaded vs direct invocation。

**Slide 36** — "How to Wire Your Own Workflow"（当前 slide 36）
- 保留。已使用 weather workflow 作为示例。

**Slide 37** — Closing（当前 slide 37）
- 保留。更新副标题为："Five concepts, one running example"
- 更新正文引用 weather reporter 弧线。

---

## 4. Asset 复用清单

| Asset | Current location | New location | Action |
|---|---|---|---|
| `context-window.jpeg` | Slide 8（Claude's Brain） | Slide 20（相同内容，重新编号） | 保留——无需更改 |
| `context.jpg` | Slide 9（What Loads at Session Start） | Slide 21（相同内容，重新编号） | 保留——无需更改 |
| `../../!/claude-jumping.svg` | Slides 1, header | 不变 | 无需操作 |
| `../../!/root/boris-slider.gif` | Slide 2 | 不变 | 无需操作 |

两个 context 图表都保留在原来的位置——包含它们的 slides 只是重新编号（8→20, 9→21）。

---

## 5. Bookkeeping 影响

### 新的 section-divider 位置和 `data-level` 分配

| Slide | Topic | `data-level` |
|---|---|---|
| 7 | Agents | `agents` |
| 13 | Skills | `skills` |
| 19 | Context | `context` |
| 25 | CLAUDE.md | `claude-md` |
| 30 | Commands | `commands` |
| 33 | Workflow sub-section | `workflow` |

### Slide 6 上的 TOC `goToSlide` 目标

| Row | Topic | Old target | New target |
|---|---|---|---|
| Row 1 | Agents | 18 | 7 |
| Row 2 | Skills | 24 | 13 |
| Row 3 | Context | 7 | 19 |
| Row 4 | CLAUDE.md | 12 | 25 |
| Row 5 | Commands | 30 | 30 |

### Journey ticks（不变）

Journey tick rail 已经从上到下排序为：Workflow、Commands、Skills、Agents、CLAUDE.md、Context。这与弧线顺序*相反*（顶部 = 最高级别 = 最后达到）。无需更改。

### LEVELS 映射（不变）

所有 6 个 level keys（`context`、`claude-md`、`agents`、`skills`、`commands`、`workflow`）保留。没有添加或删除 keys。

---

## 6. 实现方法

HTML 是一个大文件。Slides 的当前顺序与新弧线不匹配。最干净的实现方法是：

1. 剪切 slide divs 并按新顺序粘贴（7-12 = old 18-23, 13-18 = old 24-29, 19-23 = old 7-11, 24-29 = old 12-17, 30-37 不变）。
2. 按顺序重新编号所有 `data-slide` 属性。
3. 更新 section-slide `data-level` 属性。
4. 更新 section dividers 上的 section-number 文本和 h1。
5. 更新 slide 6 上的 TOC `goToSlide` 目标。
6. 更新 Workflow section-slide（旧 33）的 section-number 文本。
7. 在需要的地方进行有针对性的 weather-reporter 框架内容编辑。

总 slide 数量：**37**（不变）。

---

## 7. 歧义——无负载相关

所有歧义已在上文中解决。直接进入实现阶段。
