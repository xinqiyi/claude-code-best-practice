---
name: weather-svg-creator
description: 创建一个显示迪拜当前温度的 SVG 天气卡片。将 SVG 写入 orchestration-workflow/weather.svg，并更新 orchestration-workflow/output.md。
---

# Weather SVG Creator Skill

创建迪拜（阿联酋）的可视化 SVG 天气卡片，并写入输出文件。

## Task

你将收到来自调用上下文的温度值和单位（摄氏或华氏）。创建一个 SVG 天气卡片，并同时写入 SVG 和 markdown 摘要。

## Instructions

1. **Create SVG** — 使用 [reference.md](reference.md) 中的 SVG 模板，将占位符替换为实际值
2. **Write SVG file** — 读取并写入 `orchestration-workflow/weather.svg`
3. **Write summary** — 使用 [reference.md](reference.md) 中的 markdown 模板，读取并写入 `orchestration-workflow/output.md`

## Rules

- 使用提供的准确温度值和单位 — 不要重新获取或修改
- SVG 必须是自包含且有效的
- 两个输出文件都放在 `orchestration-workflow/` 目录下

## Additional resources

- 有关 SVG 模板、输出模板和设计规范，请参见 [reference.md](reference.md)
- 有关示例输入/输出对，请参见 [examples.md](examples.md)
