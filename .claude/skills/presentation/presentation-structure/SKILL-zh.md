---
name: presentation-structure
description: 关于演示文稿幻灯片格式、层级系统、导航和章节结构的知识
---

# 演示文稿结构 Skill

关于 `presentation/index.html` 中演示文稿结构的知识。

## 文件位置

`presentation/index.html` — 一个单文件 HTML 演示文稿，包含内联 CSS 和 JS。

## 幻灯片格式

每张幻灯片是一个带有 `data-slide`（顺序编号）和可选的 `data-level`（在过渡点的旅程层级）的 div：

```html
<!-- 普通幻灯片 — 继承前一个 data-level 幻灯片的层级 -->
<div class="slide" data-slide="12">
    <h1>幻灯片标题</h1>
    <!-- 内容 -->
</div>

<!-- 层级过渡幻灯片 — 为此幻灯片及之后所有幻灯片设置新层级 -->
<div class="slide section-slide" data-slide="10" data-level="low">
    <h1>章节名称</h1>
    <p class="section-desc">层级: Low — 此章节的描述</p>
</div>

<!-- 标题幻灯片（居中） -->
<div class="slide title-slide" data-slide="1">
    <h1>演示文稿标题</h1>
    <p class="subtitle">副标题文本</p>
</div>
```

## 旅程栏层级系统

该演示文稿使用四级系统而非累计百分比：

- 层级通过在关键过渡幻灯片（章节分隔符）上设置 `data-level` 属性来确定
- 所有跟在 `data-level` 幻灯片之后的幻灯片继承该层级，直到下一个过渡点
- 旅程栏填充至 25% / 50% / 75% / 100%，分别对应 Low / Medium / High / Pro
- 该栏在幻灯片 1（标题幻灯片）上隐藏；从幻灯片 2 开始显示
- 在第一个 `data-level` 之前的幻灯片（幻灯片 2–9）显示空白栏（尚未设置层级）
- `.level-badge` 由 JS 在带有 `data-level` 的幻灯片的 `<h1>` 上注入——请勿在 HTML 中硬编码

### 按章节划分的层级过渡

| 章节 | 幻灯片范围 | data-level | 栏高度 |
|---------|-------------|------------|------------|
| 第 0 部分：引言 | 幻灯片 1-4 | (无) | 隐藏 / 空 |
| 第 1 部分：前置条件 | 幻灯片 5-9 | (无) | 空 |
| 第 2 部分：更好的提示技巧 | 幻灯片 10-17 | `low` | 25% |
| 第 3 部分：项目记忆 | 幻灯片 18-24 | `medium` | 50% |
| 第 4 部分：结构化工作流 | 幻灯片 25-28 | (继承 medium) | 50% |
| 第 5 部分：领域知识 | 幻灯片 29-33 | `high` | 75% |
| 第 6 部分：Agentic Engineering | 幻灯片 34-46 | `high` | 75% |
| 附录 | 幻灯片 47+ | (继承 high) | 75% |

## 导航系统

- `goToSlide(n)` — 用于目录链接中，必须与实际 `data-slide` 编号匹配
- `totalSlides` 从 DOM 自动计算（`document.querySelectorAll('[data-slide]').length`）
- 方向键、空格键和触摸滑动用于导航
- 幻灯片计数器在左下角显示 `当前 / 总数`

## 重新编号规则

添加、删除或重新排序幻灯片后：
1. 将所有 `data-slide` 属性从 1 开始按顺序重新编号
2. 更新目录/旅程地图幻灯片中的所有 `goToSlide()` 调用
3. JS 的 `totalSlides` 自动计算——无需手动更新
4. 确认没有编号间隙或重复

## 章节分隔符格式

章节分隔符使用 `section-slide` 类。层级过渡的章节分隔符带有 `data-level`，并在描述中显示层级名称：

```html
<div class="slide section-slide" data-slide="10" data-level="low">
    <p class="section-number">第 2 部分</p>
    <h1>更好的提示技巧</h1>
    <p class="section-desc">层级: Low — 有效的提示技巧，获得真正成果。</p>
</div>
```

JS 会在层级过渡时在 `<h1>` 中注入 `.level-badge`（例如"→ Low"）——请勿在 HTML 中手动添加。
