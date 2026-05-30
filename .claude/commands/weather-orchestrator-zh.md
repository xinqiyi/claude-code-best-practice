---
description: 获取迪拜天气并创建 SVG 天气卡片
model: haiku
allowed-tools:
  - AskUserQuestion
  - Agent
  - Skill
---

# Weather Orchestrator 命令

获取阿联酋迪拜的当前温度并创建可视化的 SVG 天气卡片。

## 执行契约（不可协商）

你必须通过委托给 `weather-agent` 子代理来完成此命令。禁止以下行为：

- 通过 Bash、WebFetch 或任何其他工具自行获取天气数据
- 跳过步骤 1（用户的单位偏好是 agent 的必要输入）
- 在 agent 返回温度之前调用 `weather-svg-creator`

如果无法调用 Agent 工具，请停止并向用户报告错误。不要自行变通。

## 工作流程

### 步骤 1：询问用户偏好

使用 AskUserQuestion 工具询问用户希望温度使用摄氏度还是华氏度。在继续之前记录所选单位。

### 步骤 2：通过 Agent 获取天气数据

使用 Agent 工具调用 weather agent：

- subagent_type: weather-agent
- description: 获取迪拜天气数据
- prompt: 获取阿联酋迪拜当前的温度，单位为 [用户请求的单位]。返回数值温度值和单位。该 agent 有一个预加载的 skill（weather-fetcher）提供详细说明。
- model: haiku

等待 agent 完成并记录返回的温度值和单位。

**故障封闭防护**：如果 agent 没有返回数值温度值和单位，不要继续执行步骤 3。向用户报告失败并停止。

### 步骤 3：创建 SVG 天气卡片

使用 Skill 工具调用 weather-svg-creator skill：

- skill: weather-svg-creator

该 skill 将使用步骤 2 中的温度值和单位（在当前上下文中可用）来创建 SVG 卡片并写入输出文件。

## 输出摘要

向用户提供清晰的摘要，包括：

- 请求的温度单位
- 从迪拜获取的温度
- SVG 卡片创建于 `orchestration-workflow/weather.svg`
- 摘要写入 `orchestration-workflow/output.md`
