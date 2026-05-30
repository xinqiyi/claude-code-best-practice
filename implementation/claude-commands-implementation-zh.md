# Commands 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_02%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#weather-orchestrator"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

天气编排命令在此仓库中实现，作为 **Command → Agent → Skill** 架构模式的入口点，展示了 commands 如何编排多步骤工作流。

---

## Weather Orchestrator

**文件**: [`.claude/commands/weather-orchestrator.md`](../.claude/commands/weather-orchestrator.md)

```yaml
---
description: 获取迪拜天气数据并创建 SVG 天气卡片
model: haiku
---

# Weather Orchestrator 命令

获取阿联酋迪拜的当前温度并创建可视化 SVG 天气卡片。

## 工作流

### 步骤 1: 询问用户偏好
使用 AskUserQuestion 工具询问用户希望使用
摄氏度还是华氏度。

### 步骤 2: 获取天气数据
使用 Agent 工具调用 weather agent：
- subagent_type: weather-agent
- prompt: 获取阿联酋迪拜当前温度，单位为 [unit]...

### 步骤 3: 创建 SVG 天气卡片
使用 Skill 工具调用 weather-svg-creator skill：
- skill: weather-svg-creator

...
```

该命令编排了整个工作流：它询问用户偏好的温度单位，通过 Agent 工具调用 `weather-agent`，然后通过 Skill 工具调用 `weather-svg-creator` skill。

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> /weather-orchestrator
```

---

## ![如何实现](../!/tags/how-to-implement.svg)

让 Claude 为您创建一个 — 它将在 `.claude/commands/<name>.md` 中生成带有 YAML frontmatter 和正文的 markdown 文件

---

<a href="https://github.com/shanraisshan/claude-code-best-practice#orchestration-workflow"><img src="../!/tags/orchestration-workflow-hd.svg" alt="编排工作流"></a>

天气编排器是 Command → Agent → Skill 编排模式中的 **Command**。它作为入口点 — 处理用户交互（温度单位偏好），将数据获取委托给 `weather-agent`，并调用 `weather-svg-creator` skill 生成视觉输出。

<p align="center">
  <img src="../orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent 架构流程" width="100%">
</p>

| 组件 | 角色 | 本仓库 |
|-----------|------|-----------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载 skill 获取数据（agent skill） | [`weather-agent`](../.claude/agents/weather-agent.md) 搭配 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |
