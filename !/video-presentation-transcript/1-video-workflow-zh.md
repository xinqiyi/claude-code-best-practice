# 视频 1：从 Vibe Coding 到 Agentic Engineering — 使用 Claude Code 的工作流

**总时长：约 5 分钟**

---

## 引言 — 问题（0:00 – 0:45）

- "如果你刚开始使用 Claude Code，很可能你在做 vibe coding——输入 prompt，得到结果，重复。这也能工作，但你只使用了 Claude Code 能力的一小部分。"
- "本仓库是一个精心策划的最佳实践集合，带你从 vibe coding 走向 agentic engineering——Claude 不仅回应你的指令，而是为你运行工作流。"
- "在这个第一个视频中，我将介绍基础：**Commands、Agents 和 Skills**——以及它们如何链结成可重复的工作流。"

---

## 第一部分 — 临时方式（0:45 – 2:00）

**演示：Vibe coding 方法**

- 打开一个全新的 Claude Code 终端
- 输入：*"What is the weather in Dubai? Write it to an output file and create an SVG card for it."*
- 展示结果——它工作，但指出：
  - SVG 设计每次都不同（随机的颜色、布局、字体）
  - 你得坐着看它工作
  - 如果明天再次运行，你会得到一张完全不同的卡片
- **打开第二个终端，再次运行相同的 prompt**
  - 并排展示 SVG——它们不一样
- "这就是 vibe coding 的问题。它一次能用，但不可重复。不是你可以信赖的工作流。"

---

## 第二部分 — 工作流方式（2:00 – 3:15）

**演示：`/weather-orchestrator` command**

- "现在让我展示同样的任务，但作为工作流。"
- 输入：`/weather-orchestrator`
- 逐步讲解屏幕上发生的事情：
  1. 它**问你**Celsius 还是 Fahrenheit（结构化的用户交互）
  2. 它**唤起一个 weather-agent** 来获取温度（你在终端看到绿色 agent）
  3. 它**调用一个 skill** 来创建 SVG 卡片
  4. 输出：`orchestration-workflow/weather.svg` + `orchestration-workflow/output.md`
- "再次运行——相同的 SVG 布局，相同的文件结构，相同的干净结果。每次如此。"
- "你可以启动它然后走开。它自主运行。"

---

## 第三部分 — 工作原理：Command → Agent → Skill（3:15 – 4:30）

**解释三个构建块**

### Commands（`.claude/commands/`）

- "Command 是入口点——像一个脚本。这是一个 markdown 文件，告诉 Claude *要遵循哪些步骤*。"
- "我们的 `weather-orchestrator` 是指挥者。它问用户一个问题，调用一个 agent，然后调用一个 skill。"
- Commands 位于 `.claude/commands/`，显示为 `/slash-commands`

### Agents（`.claude/agents/`）

- "Agent 是一个专门的工人。我们的 `weather-agent` 只有一个工作：获取温度。"
- "它有一个**预加载的 skill** 叫做 `weather-fetcher`——该 skill 在启动时被注入到 agent 的 context 中，所以它确切知道要调用哪个 API 以及如何解析响应。"
- Agents 有自己的工具、模型和权限。它们是隔离的工人。

### Skills（`.claude/skills/`）

- "Skill 是一组可重用的指令。把它想象成一个配方。"
- "我们这里有两种 skill pattern："
  - **Agent skill**（预加载）：`weather-fetcher` 被嵌入 agent——这是领域知识
  - **Invoked skill**：`weather-svg-creator` 通过 Skill tool 独立调用——它创建 SVG 卡片
- Skills 可以是背景知识或者是独立的操作

### 流程示意图

```
/weather-orchestrator（Command）
    → AskUser：C° 还是 F°？
    → weather-agent（Agent + weather-fetcher skill）
    → weather-svg-creator（Skill）
    → 输出：weather.svg + output.md
```

---

## 第四部分 — 为什么这很重要 / 总结（4:30 – 5:00）

- "Vibe coding 和 agentic engineering 之间的区别是**结构**。"
  - Vibe coding：你输入，你希望，你得到些东西。
  - Agentic engineering：你定义一次工作流，它每次都按相同方式运行。
- "Commands、Agents 和 Skills 是三个构建块。一旦你理解了这些，你可以构建任何工作流。"
- "这个仓库有更多模式——hooks、多 agent 团队、CLAUDE.md 配置——我们将在未来的视频中介绍。"
- "仓库链接在描述中。Star 它、clone 它，然后开始构建你自己的工作流。"

---

## 快速参考

| Concept | Location | Purpose |
|---------|----------|---------|
| Command | `.claude/commands/` | 入口点、编排、`/slash-command` |
| Agent | `.claude/agents/` | 具有自己的工具和模型的专门工人 |
| Skill | `.claude/skills/` | 可重用的指令（预加载或调用） |
