# 编排工作流 (Orchestration Workflow)

本文档描述 **Command → Agent (使用 skill) → Skill** 编排工作流，通过一个天气数据获取和 SVG 渲染系统进行演示。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## 系统概述

天气系统在一个编排工作流中演示了两种不同的 skill 模式：
- **Agent Skills**（预加载）：`weather-fetcher` 在启动时作为领域知识注入到 `weather-agent` 中
- **Skills**（独立）：`weather-svg-creator` 由 command 通过 Skill 工具直接调用

这展示了 **Command → Agent → Skill** 架构模式，其中：
- command 编排工作流并处理用户交互
- agent 使用其预加载的 skill 获取数据
- skill 独立创建视觉输出

## 组件概览

| 组件 | 角色 | 示例 |
|-----------|------|---------|
| **Command** | 入口点，用户交互 | [`/weather-orchestrator`](../.claude/commands/weather-orchestrator.md) |
| **Agent** | 使用预加载 skill（agent skill）获取数据 | [`weather-agent`](../.claude/agents/weather-agent.md) 包含 [`weather-fetcher`](../.claude/skills/weather-fetcher/SKILL.md) |
| **Skill** | 独立创建输出（skill） | [`weather-svg-creator`](../.claude/skills/weather-svg-creator/SKILL.md) |

## 流程图

```
╔══════════════════════════════════════════════════════════════════╗
║                    编排工作流 (ORCHESTRATION WORKFLOW)            ║
║           Command  →  Agent  →  Skill                            ║
╚══════════════════════════════════════════════════════════════════╝

                         ┌───────────────────┐
                         │   用户交互         │
                         └─────────┬─────────┘
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  /weather-orchestrator — Command (入口点)           │
         └─────────────────────────┬───────────────────────────┘
                                   │
                             第 1 步
                                   │
                                   ▼
                      ┌────────────────────────┐
                      │  询问用户 — 摄氏度还是华氏度? │
                      └────────────┬───────────┘
                                   │
                       第 2 步 — Agent 工具
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-agent — Agent ● skill: weather-fetcher     │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          返回：温度 + 单位
                                   │
                       第 3 步 — Skill 工具
                                   │
                                   ▼
         ┌─────────────────────────────────────────────────────┐
         │  weather-svg-creator — Skill ● SVG 卡片 + 输出文件  │
         └─────────────────────────┬───────────────────────────┘
                                   │
                          ┌────────┴────────┐
                          │                 │
                          ▼                 ▼
                   ┌────────────┐    ┌────────────┐
                   │weather.svg │    │ output.md  │
                   └────────────┘    └────────────┘
```

## 组件详情

### 1. Command

#### `/weather-orchestrator` (Command)
- **位置**：`.claude/commands/weather-orchestrator.md`
- **目的**：入口点 — 编排工作流并处理用户交互
- **操作**：
  1. 询问用户偏好的温度单位（摄氏度/华氏度）
  2. 通过 Agent 工具调用 weather-agent
  3. 通过 Skill 工具调用 weather-svg-creator
- **Model**：haiku

### 2. 带预加载 Skill 的 Agent (Agent Skill)

#### `weather-agent` (Agent)
- **位置**：`.claude/agents/weather-agent.md`
- **目的**：使用其预加载的 skill 获取天气数据
- **Skills**：`weather-fetcher`（作为领域知识预加载）
- **可用工具**：Read, Skill
- **Model**：sonnet
- **颜色**：green
- **Memory**：project

该 agent 在启动时已预加载 `weather-fetcher` 到其上下文中。它遵循 skill 的指令获取温度并将值返回给 command。

### 3. Skill

#### `weather-svg-creator` (Skill)
- **位置**：`.claude/skills/weather-svg-creator/SKILL.md`
- **目的**：创建可视化的 SVG 天气卡片并写入输出文件
- **调用方式**：通过 Skill 工具从 command 调用（不预加载到任何 agent 中）
- **输出文件**：
  - `orchestration-workflow/weather.svg` — SVG 天气卡片
  - `orchestration-workflow/output.md` — 天气摘要

### 4. 预加载 Skill

#### `weather-fetcher` (Skill)
- **位置**：`.claude/skills/weather-fetcher/SKILL.md`
- **目的**：获取实时温度数据的指令
- **数据源**：阿联酋迪拜的 Open-Meteo API
- **输出**：温度值和单位（摄氏度或华氏度）
- **说明**：这是一个 agent skill — 预加载到 `weather-agent` 中，不直接调用

## 执行流程

1. **用户调用**：用户运行 `/weather-orchestrator` command
2. **用户提示**：Command 询问用户偏好的温度单位（摄氏度/华氏度）
3. **Agent 调用**：Command 通过 Agent 工具调用 `weather-agent`
4. **Skill 执行**（在 agent 上下文中）：
   - Agent 遵循 `weather-fetcher` skill 指令从 Open-Meteo 获取温度
   - Agent 将温度值和单位返回给 command
5. **SVG 创建**：Command 通过 Skill 工具调用 `weather-svg-creator`
   - Skill 在 `orchestration-workflow/weather.svg` 创建 SVG 天气卡片
   - Skill 将摘要写入 `orchestration-workflow/output.md`
6. **结果显示**：向用户展示摘要，包含：
   - 请求的温度单位
   - 获取到的温度
   - SVG 卡片位置
   - 输出文件位置

## 执行示例

```
输入：/weather-orchestrator
├─ 第 1 步：询问：摄氏度还是华氏度？
│  └─ 用户：摄氏度
├─ 第 2 步：Agent 工具 → weather-agent
│  ├─ 预加载 Skill：
│  │  └─ weather-fetcher（领域知识）
│  ├─ 从 Open-Meteo 获取 → 26°C
│  └─ 返回：temperature=26, unit=Celsius
├─ 第 3 步：Skill 工具 → /weather-svg-creator
│  ├─ 创建：orchestration-workflow/weather.svg
│  └─ 写入：orchestration-workflow/output.md
└─ 输出：
   ├─ 单位：摄氏度
   ├─ 温度：26°C
   ├─ SVG：orchestration-workflow/weather.svg
   └─ 摘要：orchestration-workflow/output.md
```

## 关键设计原则

1. **两种 Skill 模式**：演示了 agent skills（预加载）和 skills（直接调用）
2. **Command 作为编排器**：command 处理用户交互并协调工作流
3. **Agent 用于数据获取**：agent 使用预加载 skill 获取数据，然后返回数据
4. **Skill 用于输出**：SVG 创建器独立运行，从 command 上下文接收数据
5. **职责清晰分离**：获取数据 (agent) → 渲染 (skill) — 每个组件有单一职责

## 架构模式

### Agent Skill (预加载)

```yaml
# 在 agent 定义中 (.claude/agents/weather-agent.md)
---
name: weather-agent
skills:
  - weather-fetcher    # 启动时预加载到 agent 上下文中
---
```

- **Skills 预加载**：完整的 skill 内容在启动时注入到 agent 上下文中
- **Agent 使用 skill 知识**：Agent 遵循预加载 skill 中的指令
- **无动态调用**：Skills 作为参考材料，不单独调用

### Skill (直接调用)

```yaml
# 在 skill 定义中 (.claude/skills/weather-svg-creator/SKILL.md)
---
name: weather-svg-creator
description: 创建 SVG 天气卡片...
---
```

- **通过 Skill 工具调用**：Command 调用 `Skill(skill: "weather-svg-creator")`
- **独立执行**：在 command 的上下文中运行，不在 agent 内部
- **从上下文接收数据**：使用对话中已有的温度数据
