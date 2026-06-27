# Commands 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-orchestrator"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

天气编排器 command 在本仓库中实现，是 **Command → Agent → Skill** 架构模式的入口点，展示了 command 如何编排多步工作流。

---

## 天气编排器

**文件**：[`.claude/commands/weather-orchestrator.md`](../.claude/commands/weather-orchestrator.md)

```yaml
---
description: Fetch weather data for Dubai and create an SVG weather card
model: haiku
---

# Weather Orchestrator Command

Fetch the current temperature for Dubai, UAE and create a visual SVG weather card.

## Workflow

### Step 1: Ask User Preference
Use the AskUserQuestion tool to ask the user whether they want the temperature
in Celsius or Fahrenheit.

### Step 2: Fetch Weather Data
Use the Agent tool to invoke the weather agent:
- subagent_type: weather-agent
- prompt: Fetch the current temperature for Dubai, UAE in [unit]...

### Step 3: Create SVG Weather Card
Use the Skill tool to invoke the weather-svg-creator skill:
- skill: weather-svg-creator

...
```

该 command 编排整个工作流：询问用户偏好的温度单位，通过 Agent 工具调用 `weather-agent`，然后通过 Skill 工具调用 `weather-svg-creator` skill。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> /weather-orchestrator
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

让 Claude 为你创建一个——它会在 `.claude/commands/<name>.md` 中生成包含 YAML frontmatter 和正文的 markdown 文件。

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

天气编排器是 Command → Agent → Skill 编排模式中的 **Command**。它作为入口点——处理用户交互（温度单位偏好），将数据获取委托给 `weather-agent`，并调用 `weather-svg-creator` skill 生成可视化输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent Architecture Flow" width="100%">
</p>

| Component | Role | This Repo |
|-----------|------|-----------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载 skill（agent skill）获取数据 | [`weather-agent`](../.claude/agents/weather-agent.md) 配合 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
