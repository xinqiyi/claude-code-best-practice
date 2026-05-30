---
name: time-svg-creator
description: 创建显示迪拜当前时间的 SVG 时间卡片。将 SVG 写入 agent-teams/output/dubai-time.svg 并更新 agent-teams/output/output.md。
allowed-tools: Write, Read
---

# Time SVG Creator 技能

创建迪拜的视觉 SVG 时间卡片并写入输出文件。

## 任务

你将接收来自调用上下文的三个字段：`time`、`timezone` 和 `formatted`。创建一个 SVG 时间卡片，并同时写入 SVG 文件和 markdown 摘要。

## 说明

1. **创建 SVG** — 使用 [reference.md](reference.md) 中的 SVG 模板，将占位符替换为实际值
2. **写入 SVG 文件** — 写入到 `agent-teams/output/dubai-time.svg`
3. **写入摘要** — 使用 [reference.md](reference.md) 中的 markdown 模板写入到 `agent-teams/output/output.md`

## 规则

- 使用提供的确切时间值 — 绝不重新获取或重新计算
- SVG 必须是自包含且有效的
- 两个输出文件都放在 `agent-teams/output/` 目录下

## 其他资源

- 有关 SVG 模板、输出模板和设计规范，请参阅 [reference.md](reference.md)
- 有关输入/输出示例，请参阅 [examples.md](examples.md)
