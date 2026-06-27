---
description: 获取迪拜（GST，UTC+4）的当前时间并创建可视化的 SVG 时间卡片
model: haiku
---

# Time Orchestrator Command

获取迪拜（Asia/Dubai，UTC+4）的当前时间并创建可视化的 SVG 时间卡片。

## 工作流

### 第 1 步：获取当前迪拜时间

使用 Agent tool 调用 time agent：
- subagent_type: time-agent
- description: 获取当前迪拜时间
- prompt: 获取迪拜（Asia/Dubai，UTC+4）的当前时间。精确返回三个字段：`time`（时间部分，如 "14:30:45"）、`timezone`（"GST (UTC+4)"）和 `formatted`（完整格式化字符串，如 "2026-03-12 14:30:45 +04"）。agent 有一个预加载的 skill（time-fetcher）提供详细指令。
- model: haiku

等待 agent 完成并捕获返回的时间数据。

### 数据契约

time-agent 必须返回以下三个字段：
- **time**：时间部分（如 "14:30:45"）
- **timezone**："GST (UTC+4)"
- **formatted**：完整格式化字符串（如 "2026-03-12 14:30:45 +04"）

### 第 2 步：创建 SVG 时间卡片

使用 Skill tool 调用 time-svg-creator skill：
- skill: time-svg-creator
- args: 传递第 1 步的时间数据——包含 `time`、`timezone` 和 `formatted` 值

Skill 将使用第 1 步的时间数据（在当前 context 中可用）来创建 SVG 卡片并写入输出文件。

## 关键要求

1. **对 time-agent 使用 Agent Tool**：不要使用 bash 命令调用 agent。必须使用 Agent tool，`subagent_type: "time-agent"`。
2. **对 SVG Creator 使用 Skill Tool**：通过 Skill tool 调用 SVG creator，`skill: "time-svg-creator"`，而不是 Agent tool。
3. **顺序流程**：agent 必须在 skill 被调用之前完成并返回时间数据。不要并行运行。
4. **数据传递**：确保在调用 skill 时，agent 响应中的三个字段（time、timezone、formatted）在 context 中可用。

## 输出概要

两个步骤都完成后，向用户提供清晰的概要，显示：
- 获取的当前迪拜时间
- 时区：GST (UTC+4)
- 完整格式化时间戳
- 在 `agent-teams/output/dubai-time.svg` 创建的 SVG 卡片
- 写入 `agent-teams/output/output.md` 的概要
