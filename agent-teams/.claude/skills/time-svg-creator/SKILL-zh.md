---
name: time-svg-creator
description: 创建显示迪拜当前时间的 SVG 时间卡片。将 SVG 写入 agent-teams/output/dubai-time.svg，并更新 agent-teams/output/output.md。
allowed-tools: Write, Read
---

# Time SVG Creator Skill

为阿联酋迪拜创建可视化的 SVG 时间卡片，并写入输出文件。

## 任务

你将收到来自调用 context 的三个字段：`time`、`timezone` 和 `formatted`。创建一张 SVG 时间卡片，并写入 SVG 和 markdown 概要。

## 指令

1. **创建 SVG** — 使用 [reference.md](reference.md) 中的 SVG 模板，用实际值替换占位符
2. **写入 SVG 文件** — 写入到 `agent-teams/output/dubai-time.svg`
3. **写入概要** — 使用 [reference.md](reference.md) 中的 markdown 模板写入到 `agent-teams/output/output.md`
4. **展示结果** — 完成后，展示一个包含以下内容的响应给用户：
   - 当前迪拜时间已获取
   - 时区
   - 时间卡片位置

## 规则

- 使用提供的**确切**时间值——绝不重新获取或重新计算
- SVG 必须是自包含且有效的
- 两个输出文件都在 `agent-teams/output/` 目录中

## 额外资源

- SVG 模板、输出模板和设计规格请见 [reference.md](reference.md)
- 示例输入/输出对请见 [examples.md](examples.md)
