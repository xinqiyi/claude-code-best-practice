# 编排工作流

本文档描述了 **Command → Agent（带 skill）→ Skill** 编排工作流，通过天气数据获取和 SVG 渲染系统进行演示。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 系统概述

天气系统在单个编排工作流中展示了两种不同的 skill pattern：
- **Agent Skills**（预加载）：`weather-fetcher` 在启动时注入到 `weather-agent` 中作为领域知识
- **Skills**（独立）：`weather-svg-creator` 通过 command 的 Skill tool 直接调用

这展示了 **Command → Agent → Skill** 架构模式，其中：
- command 编排工作流并处理用户交互
- agent 使用其预加载的 skill 获取数据
- skill 独立创建视觉输出

## 组件概要

| Component | Role | Example |
|-----------|------|---------|
| **Command** | 入口点、用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 用预加载 skill 获取数据（agent skill） | [`weather-agent`](../.claude/agents/weather-agent.md) 带有 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |

## 流程示意图

```
╔══════════════════════════════════════════════════════════════════╗
║                     编排工作流                                    ║
║           Command  →  Agent  →  Skill                            ║
╚══════════════════════════════════════════════════════════════════╝

                         ┌───────────────────┐
                         │    用户交互         │
                         └─────────┬─────────┘
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  /weather-orchestrator — Command（入口点）            │
         └─────────────────────────┬───────────────────────────┘
                                   │
                              第 1 步
                                   │
                                   ▼
                      ┌────────────────────────┐
                      │  AskUser — C° 还是 F°? │
                      └────────────┬───────────┘
                                   │
                         第 2 步 — Agent tool
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-agent — Agent ● skill: weather-fetcher     │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          返回：temp + unit
                                   │
                          第 3 步 — Skill tool
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-svg-creator — Skill ● SVG 卡片 + 输出       │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          ┌────────┴────────┐
                          │                 │
                          ▼                 ▼
                   ┌────────────┐    ┌────────────┐
                   │weather.svg │    │ output.md  │
                   └────────────┘    └────────────┘
```

## 组件的详细信息

### 1. Command

#### `/weather-orchestrator`（Command）
- **位置**：`.claude/commands/weather-orchestrator.md`
- **目的**：入口点——编排工作流并处理用户交互
- **动作**：
  1. 询问用户温度单位偏好（Celsius/Fahrenheit）
  2. 通过 Agent tool 调用 weather-agent
  3. 通过 Skill tool 调用 weather-svg-creator
- **Model**：haiku

### 2. 带预加载 Skill 的 Agent（Agent Skill）

#### `weather-agent`（Agent）
- **位置**：`.claude/agents/weather-agent.md`
- **目的**：使用预加载的 skill 获取天气数据
- **Skills**：`weather-fetcher`（预加载为领域知识）
- **可用工具**：Read、Skill
- **Model**：sonnet
- **Color**：green
- **Memory**：project

agent 在启动时在其 context 中预加载了 `weather-fetcher`。它遵循 skill 的指令获取温度，并将值返回给 command。

### 3. Skill

#### `weather-svg-creator`（Skill）
- **位置**：`.claude/skills/weather-svg-creator/SKILL.md`
- **目的**：创建可视化的 SVG 天气卡片并写入输出文件
- **调用**：通过 command 的 Skill tool（不预加载到任何 agent 中）
- **输出**：
  - `orchestration-workflow/weather.svg` — SVG 天气卡片
  - `orchestration-workflow/output.md` — 天气概要

### 4. 预加载 Skill

#### `weather-fetcher`（Skill）
- **位置**：`.claude/skills/weather-fetcher/SKILL.md`
- **目的**：获取实时温度数据的指令
- **数据源**：迪拜（阿联酋）的 Open-Meteo API
- **输出**：温度值和单位（Celsius 或 Fahrenheit）
- **注意**：这是一个 agent skill——预加载到 `weather-agent` 中，不直接调用

## 执行流程

1. **用户调用**：用户运行 `/weather-orchestrator` command
2. **用户提示**：Command 询问用户偏好的温度单位（Celsius/Fahrenheit）
3. **Agent 调用**：Command 通过 Agent tool 调用 `weather-agent`
4. **Skill 执行**（在 agent context 内）：
   - agent 遵循 `weather-fetcher` skill 指令从 Open-Meteo 获取温度
   - agent 将温度值和单位返回给 command
5. **SVG 创建**：Command 通过 Skill tool 调用 `weather-svg-creator`
   - skill 在 `orchestration-workflow/weather.svg` 创建 SVG 天气卡片
   - skill 将概要写入 `orchestration-workflow/output.md`
6. **结果显示**：向用户显示概要，包含：
   - 请求的温度单位
   - 获取的温度
   - SVG 卡片位置
   - 输出文件位置

## 执行示例

```
Input: /weather-orchestrator
├─ Step 1: 询问：Celsius 还是 Fahrenheit？
│  └─ 用户：Celsius
├─ Step 2: Agent tool → weather-agent
│  ├─ 预加载 Skill：
│  │  └─ weather-fetcher（领域知识）
│  ├─ 从 Open-Meteo 获取 → 26°C
│  └─ 返回：temperature=26, unit=Celsius
├─ Step 3: Skill tool → /weather-svg-creator
│  ├─ 创建：orchestration-workflow/weather.svg
│  └─ 写入：orchestration-workflow/output.md
└─ 输出：
   ├─ Unit: Celsius
   ├─ Temperature: 26°C
   ├─ SVG: orchestration-workflow/weather.svg
   └─ 概要: orchestration-workflow/output.md
```

## 关键设计原则

1. **两种 Skill Pattern**：展示了 agent skills（预加载）和 skills（直接调用）
2. **Command 作为编排器**：command 处理用户交互并协调工作流
3. **Agent 用于数据获取**：agent 使用其预加载 skill 获取数据，然后返回
4. **Skill 用于输出**：SVG creator 独立运行，从 command context 接收数据
5. **清晰的分离**：获取（agent）→ 渲染（skill）——每个组件有单一职责

## 架构模式

### Agent Skill（预加载）

```yaml
# 在 agent 定义中（.claude/agents/weather-agent.md）
---
name: weather-agent
skills:
  - weather-fetcher    # 启动时预加载到 agent context
---
```

- **Skills 被预加载**：完整 skill 内容在启动时注入到 agent 的 context
- **Agent 使用 skill 知识**：agent 遵循来自预加载 skills 的指令
- **无动态调用**：Skills 是参考材料，不单独调用

### Skill（直接调用）

```yaml
# 在 skill 定义中（.claude/skills/weather-svg-creator/SKILL.md）
---
name: weather-svg-creator
description: 创建 SVG 天气卡片...
---
```

- **通过 Skill tool 调用**：Command 调用 `Skill(skill: "weather-svg-creator")`
- **独立执行**：在 command 的 context 中运行，而不是在 agent 内部
- **从 context 接收数据**：使用对话中已有的温度数据
