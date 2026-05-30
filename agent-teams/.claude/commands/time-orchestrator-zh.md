---
description: 获取迪拜当前时间（GST，UTC+4）并创建可视化 SVG 时间卡片
model: haiku
---

# 时间编排命令

获取迪拜（Asia/Dubai，UTC+4）的当前时间并创建可视化 SVG 时间卡片。

## 工作流程

### 步骤 1：获取当前迪拜时间

使用 Agent 工具调用 time agent：
- subagent_type: time-agent
- description: 获取当前迪拜时间
- prompt: 获取迪拜（Asia/Dubai，UTC+4）的当前时间。精确返回三个字段：`time`（时间部分，例如 "14:30:45"）、`timezone`（"GST (UTC+4)"）和 `formatted`（完整格式化字符串，例如 "2026-03-12 14:30:45 +04"）。该 agent 预加载了 time-fetcher skill，提供详细说明。
- model: haiku

等待 agent 完成并捕获返回的时间数据。

### 数据契约

time-agent 必须返回以下三个字段：
- **time**：时间部分（例如 "14:30:45"）
- **timezone**："GST (UTC+4)"
- **formatted**：完整格式化字符串（例如 "2026-03-12 14:30:45 +04"）

### 步骤 2：创建 SVG 时间卡片

使用 Skill 工具调用 time-svg-creator skill：
- skill: time-svg-creator
- args: 传入步骤 1 的时间数据 — 包含 `time`、`timezone` 和 `formatted` 值

该 skill 将使用步骤 1 的时间数据（在当前 context 中可用）来创建 SVG 卡片并写入输出文件。

## 关键要求

1. **对 time-agent 使用 Agent 工具**：不要使用 bash 命令调用 agents。必须使用 Agent 工具并设置 `subagent_type: "time-agent"`。
2. **对 SVG 创建使用 Skill 工具**：通过 Skill 工具调用 SVG creator，设置 `skill: "time-svg-creator"`，不要使用 Agent 工具。
3. **顺序执行**：agent 必须完成并返回时间数据后才能调用 skill。不要并行运行。
4. **数据传递**：确保调用 skill 时 agent 响应的所有三个字段（time、timezone、formatted）在 context 中可用。

## 输出摘要

两个步骤完成后，向用户提供清晰的摘要，显示：
- 已获取的当前迪拜时间
- 时区：GST (UTC+4)
- 完整格式化时间戳
- SVG 卡片已创建于 `agent-teams/output/dubai-time.svg`
- 摘要已写入 `agent-teams/output/output.md`
