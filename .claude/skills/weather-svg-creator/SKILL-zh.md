---
name: weather-svg-creator
description: 创建显示迪拜当前温度的 SVG 天气卡片。将 SVG 写入 orchestration-workflow/weather.svg，并更新 orchestration-workflow/output.md。
---

# Weather SVG Creator Skill

为阿联酋迪拜创建可视化 SVG 天气卡片并写入输出文件。

## 任务

你将从调用 context 中收到温度值和单位（Celsius 或 Fahrenheit）。创建一个 SVG 天气卡片，并写入 SVG 和 markdown 摘要。

## 操作说明

1. **创建 SVG** — 使用 [reference.md](reference.md) 中的 SVG 模板，将占位符替换为实际值
2. **写入 SVG 文件** — 读取后写入 `orchestration-workflow/weather.svg`
3. **写入摘要** — 读取后使用 [reference.md](reference.md) 中的 markdown 模板写入 `orchestration-workflow/output.md`

## 规则

- 使用提供的精确温度值和单位 — 不要重新获取或修改
- SVG 必须是自包含且有效的
- 两个输出文件都放在 `orchestration-workflow/` 目录中

## 额外资源

- 关于 SVG 模板、输出模板和设计规范，参见 [reference.md](reference.md)
- 关于示例输入/输出对，参见 [examples.md](examples.md)
