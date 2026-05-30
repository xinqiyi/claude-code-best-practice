# 视频1：从 Vibe Coding 到 Agentic Engineering — Claude Code 工作流

**总时长：约5分钟**

---

## 引言 — 问题 (0:00 – 0:45)

- "如果你刚开始使用 Claude Code，很可能你还在 vibe coding —— 输入提示词，得到结果，重复这个过程。这虽然可行，但你仅仅用到了 Claude Code 的一小部分能力。"
- "这个仓库是一个精心整理的最佳实践合集，带你从 vibe coding 迈向 agentic engineering —— 让 Claude 不只是回应你，而是为你运行工作流。"
- "在第一期视频中，我将介绍基础内容：**Commands、Agents 和 Skills** —— 以及它们如何串联成可重复的工作流。"

---

## 第一部分 — Ad-Hoc 方式 (0:45 – 2:00)

**演示：Vibe coding 方法**

- 打开一个新的 Claude Code 终端
- 输入：*"迪拜的天气怎么样？把它写入一个输出文件，并创建一个 SVG 卡片。"*
- 展示结果 —— 它能工作，但指出问题：
  - SVG 设计每次都不一样（随机颜色、布局、字体）
  - 你必须坐在那里看着它运行
  - 如果明天再运行一次，你会得到一张完全不同的卡片
- **打开第二个终端，再次运行同样的提示词**
  - 并排展示两张 SVG —— 它们看起来不一样
- "这就是 vibe coding 的问题。它只能工作一次。但它是不可重复的。这不是一个你可以信赖的工作流。"

---

## 第二部分 — 工作流方式 (2:00 – 3:15)

**演示：`/weather-orchestrator` 命令**

- "现在让我展示同样的任务，但以工作流的形式。"
- 输入：`/weather-orchestrator`
- 逐步讲解屏幕上发生的事情：
  1. 它**询问你**选择摄氏度还是华氏度（结构化的用户交互）
  2. 它**生成一个 weather-agent** 来获取温度（你会在终端中看到绿色的 agent）
  3. 它**调用一个 skill** 来创建 SVG 卡片
  4. 输出：`orchestration-workflow/weather.svg` + `orchestration-workflow/output.md`
- "再运行一次 —— 同样的 SVG 布局，同样的文件结构，同样干净的结果。每一次都如此。"
- "你可以启动它然后走开。它会自主运行。"

---

## 第三部分 — 工作原理：Command → Agent → Skill (3:15 – 4:30)

**解释三个基本构建块**

### Commands (`.claude/commands/`)

- "Command 是入口点 —— 就像一个脚本。它是一个 markdown 文件，告诉 Claude *要遵循哪些步骤*。"
- "我们的 `weather-orchestrator` 就是指挥者。它向用户提问，调用一个 agent，然后调用一个 skill。"
- Commands 存放在 `.claude/commands/` 中，并显示为 `/slash-commands`

### Agents (`.claude/agents/`)

- "Agent 是一个专门的工作者。我们的 `weather-agent` 只有一个任务：获取温度。"
- "它有一个**预加载的 skill** 叫做 `weather-fetcher` —— 这个 skill 在启动时被注入到 agent 的上下文中，所以 agent 知道调用哪个 API 以及如何解析响应。"
- Agents 拥有自己的工具、模型和权限。它们是隔离的工作者。

### Skills (`.claude/skills/`)

- "Skill 是一组可复用的指令。把它想象成一个食谱。"
- "我们这里有两种 skill 模式："
  - **Agent skill（预加载）**：`weather-fetcher` 被嵌入到 agent 中 —— 它是领域知识
  - **Invoked skill（调用式）**：`weather-svg-creator` 通过 Skill 工具独立调用 —— 它创建 SVG 卡片
- Skills 可以是背景知识，也可以是独立操作

### 流程示意图（可选在屏幕上展示）

```
/weather-orchestrator (Command)
    → AskUser: C° or F°?
    → weather-agent (Agent + weather-fetcher skill)
    → weather-svg-creator (Skill)
    → Output: weather.svg + output.md
```

---

## 第四部分 — 为什么这很重要 / 总结 (4:30 – 5:00)

- "Vibe coding 和 agentic engineering 之间的区别在于**结构**。"
  - Vibe coding：你输入，你期望，你得到一些东西。
  - Agentic engineering：你定义一次工作流，它每次都按同样的方式运行。
- "Commands、Agents 和 Skills 是三个基本构建块。一旦你理解了这些，你就可以构建任何工作流。"
- "这个仓库还有更多模式 —— hooks、多 agent 团队、CLAUDE.md 配置 —— 我们将在后续视频中介绍。"
- "仓库链接在描述中。给它点个星标，克隆它，然后开始构建你自己的工作流。"

---

## 快速参考

| 概念 | 位置 | 用途 |
|---------|----------|---------|
| Command | `.claude/commands/` | 入口点，编排，`/slash-command` |
| Agent | `.claude/agents/` | 拥有自有工具和模型的专门工作者 |
| Skill | `.claude/skills/` | 可复用的指令（预加载或调用） |
