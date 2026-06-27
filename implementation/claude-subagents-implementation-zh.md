# Sub-agents 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026_07%3A59_PM_PKT-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-agent"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

天气 agent 在本仓库中实现，作为 **Command → Agent → Skill** 架构模式的示例，展示了两种不同的 skill 模式。

---

## Weather Agent

**文件**：[`.claude/agents/weather-agent.md`](../.claude/agents/weather-agent.md)

```yaml
---
name: weather-agent
description: Use this agent PROACTIVELY when you need to fetch weather data for
  Dubai, UAE. This agent fetches real-time temperature from Open-Meteo
  using its preloaded weather-fetcher skill.
allowedTools:
  - "Read"
  - "Skill"
model: sonnet
color: green
maxTurns: 5
permissionMode: acceptEdits
memory: project
skills:
  - weather-fetcher
---

# Weather Agent

You are a specialized weather agent that fetches weather data for Dubai,
UAE.

## Your Task

Execute the weather workflow by following the instructions from your preloaded
skill:

1. **Fetch**: Follow the `weather-fetcher` skill instructions to fetch the
   current temperature
2. **Report**: Return the temperature value and unit to the caller
3. **Memory**: Update your agent memory with the reading details for
   historical tracking

...
```

该 agent 有一个预加载的 skill（`weather-fetcher`），提供从 Open-Meteo 获取数据的指令。它将温度值和单位返回给调用方 command。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> what is the weather in dubai?
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

你可以使用 `/agents` 命令创建 agent，
```bash
$ claude
> /agents
```

或者让 Claude 为你创建一个——它会在 `.claude/agents/<name>.md` 中生成包含 YAML frontmatter 和正文的 markdown 文件。

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

天气 agent 是 Command → Agent → Skill 编排模式中的 **Agent**。它从 `/weather-orchestrator` command 接收工作流，并使用其预加载的 skill（`weather-fetcher`）获取温度。然后 command 调用独立的 `weather-svg-creator` skill 来创建可视化输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent Architecture Flow" width="100%">
</p>

| Component | Role | This Repo |
|-----------|------|-----------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载 skill（agent skill）获取数据 | [`weather-agent`](../.claude/agents/weather-agent.md) 配合 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
