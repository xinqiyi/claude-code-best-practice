---
description: 获取迪拜天气并创建 SVG 天气卡片
model: haiku
allowed-tools:
  - AskUserQuestion
  - Agent
  - Skill
---

# Weather Orchestrator 命令

获取阿联酋迪拜的当前温度并创建可视化 SVG 天气卡片。

## 执行契约（不可协商）

你必须通过委托给 `weather-agent` subagent 来完成此命令。你被禁止：

- 通过 Bash、WebFetch 或任何其他工具自行获取天气数据
- 跳过步骤 1（用户的单位偏好是 agent 的必要输入）
- 在 agent 返回温度之前调用 `weather-svg-creator`

如果你无法调用 Agent tool，请停止并向用户报告错误。不要临时替代。

## 工作流

### 步骤 1：询问用户偏好

使用 AskUserQuestion tool 询问用户想要摄氏度还是华氏度。在继续之前捕获选择的单位。

### 步骤 2：通过 Agent 获取天气数据

使用 Agent tool 调用 weather agent：

- subagent_type: weather-agent
- description: Fetch Dubai weather data
- prompt: Fetch the current temperature for Dubai, UAE in [unit requested by user]. Return the numeric temperature value and unit. The agent has a preloaded skill (weather-fetcher) that provides the detailed instructions.
- model: haiku

等待 agent 完成并捕获返回的温度值和单位。

**Fail-closed guardrail**：如果 agent 没有返回数字温度值和单位，不要进入步骤 3。向用户报告失败并停止。

### 步骤 3：创建 SVG 天气卡片

使用 Skill tool 调用 weather-svg-creator skill：

- skill: weather-svg-creator

该 skill 将使用步骤 2 中的温度值和单位（在当前 context 中可用）来创建 SVG 卡片并写入输出文件。

## 输出摘要

向用户提供清晰的摘要，显示：

- 请求的温度单位
- 从迪拜获取的温度
- SVG 卡片创建于 `orchestration-workflow/weather.svg`
- 摘要写入 `orchestration-workflow/output.md`
