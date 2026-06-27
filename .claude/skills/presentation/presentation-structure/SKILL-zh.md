---
name: presentation-structure
description: 关于演示文稿 slide 格式、权重系统、导航和章节结构的知识
---

# Presentation Structure Skill

关于 `presentation/index.html` 中演示文稿结构的知识。

## 文件位置

`presentation/index.html` — 一个包含内联 CSS 和 JS 的单文件 HTML 演示文稿。

## Slide 格式

每个 slide 是一个带有 `data-slide`（顺序编号）和可选的 `data-level`（在转折点处的 journey level）的 div：

```html
<!-- 常规 slide — 从前一个 data-level slide 继承 level -->
<div class="slide" data-slide="12">
    <h1>Slide Title</h1>
    <!-- content -->
</div>

<!-- Level 转折 slide — 为此 slide 及其后所有 slide 设置新的 level -->
<div class="slide section-slide" data-slide="10" data-level="low">
    <h1>Section Name</h1>
    <p class="section-desc">Level: Low — description of this section</p>
</div>

<!-- 标题 slide（居中） -->
<div class="slide title-slide" data-slide="1">
    <h1>Presentation Title</h1>
    <p class="subtitle">Subtitle text</p>
</div>
```

## Journey Bar Level 系统

该演示文稿使用 4-level 系统，而非累积百分比：

- Level 通过关键转折 slide（章节分隔页）上的 `data-level` 属性设置
- `data-level` slide 之后的所有 slide 继承该 level，直到下一个转折点
- Journey bar 分别填充至 25% / 50% / 75% / 100%，对应 Low / Medium / High / Pro
- 在 slide 1（标题 slide）上该 bar 被隐藏；从 slide 2 开始显示
- 第一个 `data-level` 之前的 slide（slide 2–9）显示空 bar（尚未设置 level）
- `.level-badge` 由 JS 注入到带有 `data-level` 的 slide 的 `<h1>` 上 — 不要在 HTML 中硬编码

### 各章节的 Level 转折

| 章节 | Slide 范围 | data-level | Bar 高度 |
|---------|-------------|------------|------------|
| Part 0: Introduction | Slides 1-4 | (无) | hidden / empty |
| Part 1: Prerequisites | Slides 5-9 | (无) | empty |
| Part 2: Better Prompting | Slides 10-17 | `low` | 25% |
| Part 3: Project Memory | Slides 18-24 | `medium` | 50% |
| Part 4: Structured Workflows | Slides 25-28 | (继承 medium) | 50% |
| Part 5: Domain Knowledge | Slides 29-33 | `high` | 75% |
| Part 6: Agentic Engineering | Slides 34-46 | `high` | 75% |
| Appendix | Slides 47+ | (继承 high) | 75% |

## 导航系统

- `goToSlide(n)` — 用于 TOC 链接，必须与实际的 `data-slide` 编号匹配
- `totalSlides` 从 DOM 自动计算（`document.querySelectorAll('[data-slide]').length`）
- 方向键、空格键和触摸滑动用于导航
- Slide 计数器在左下角显示 `current / total`

## 重新编号规则

添加、删除或重新排序 slide 后：
1. 从 1 开始按顺序重新编号所有 `data-slide` 属性
2. 更新 TOC/Journey Map slide 中所有的 `goToSlide()` 调用
3. JS 的 `totalSlides` 自动计算 — 无需手动更新
4. 确认没有空缺或重复

## 章节分隔页格式

章节分隔页使用 `section-slide` class。带有 level 转折的章节分隔页携带 `data-level`，并在描述中显示 level 名称：

```html
<div class="slide section-slide" data-slide="10" data-level="low">
    <p class="section-number">Part 2</p>
    <h1>Better Prompting</h1>
    <p class="section-desc">Level: Low — effective prompting for real results.</p>
</div>
```

当 level 发生转折时，JS 会在运行时将 `.level-badge`（例如 "→ Low"）注入到 `<h1>` 中 — 不要手动在 HTML 中添加这些。
