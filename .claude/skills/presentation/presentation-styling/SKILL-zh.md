---
name: presentation-styling
description: 关于演示文稿中 CSS class、组件模式和语法高亮的知识
---

# Presentation Styling Skill

`presentation/index.html` 中使用的 CSS class 和 HTML 模式。

## CSS 组件 Class

### 布局

- `.two-col` — 2 列 grid 布局，24px 间距
- `.info-grid` — 用于信息卡片的 2 列 grid
- `.col-card` — 列中的卡片（添加 `.good` 获得绿色边框，`.bad` 获得红色边框）
- `.info-card` — 信息 grid 中的卡片

### 内容块

- `.trigger-box` — 带深色左边框的灰色框（用于关键概念、前置条件）
- `.how-to-trigger` — 带绿色边框的绿色框（用于"Try This"操作）
- `.warning-box` — 带警告边框的橙色框（用于重要警告）
- `.code-block` — 带 monospace 字体的深色代码显示块

### 列表

- `.use-cases` — 图标+文本列表项的容器
- `.use-case-item` — 带图标和文本的单个项目
- `.feature-list` — 简单的带边框列表

### 标签和徽章

- `.matcher-tag` — 灰色内联 pill 标签
- `.weight-badge` — 绿色 pill 徽章（由 JS 为加权 slide 自动注入）

## Code Block 语法高亮

在 `.code-block` 内部，使用以下 span 进行语法着色：

```html
<div class="code-block">
<span class="comment"># This is a comment</span>
<span class="key">field_name</span>: <span class="string">value</span>
<span class="cmd">&gt;</span> command to run
</div>
```

- `.comment` — 绿色（#6a9955）用于注释
- `.key` — 蓝色（#9cdcfe）用于属性名/键名
- `.string` — 橙色（#ce9178）用于字符串值
- `.cmd` — 黄色（#dcdcaa）用于命令/prompt

## Slide 类型模式

### 带双列对比的内容 Slide（Good vs Bad）
```html
<div class="slide" data-slide="N" data-weight="5">
    <h1>Title</h1>
    <div class="two-col">
        <div class="col-card bad">
            <h4>Before (Vibe Coding)</h4>
            <!-- bad example -->
        </div>
        <div class="col-card good">
            <h4>After (Agentic)</h4>
            <!-- good example -->
        </div>
    </div>
</div>
```

不要在 slide HTML 中硬编码 `<span class="weight-badge">`。演示文稿的 JavaScript 会自动注入和移除 weight badge。

### 带代码示例的内容 Slide
```html
<div class="slide" data-slide="N">
    <h1>Title</h1>
    <div class="trigger-box">
        <h4>Key Concept</h4>
        <p>Description</p>
    </div>
    <div class="code-block"><span class="comment"># Example</span>
<span class="key">field</span>: <span class="string">value</span></div>
</div>
```

### 图标列表模式
```html
<div class="use-cases">
    <div class="use-case-item">
        <span class="use-case-icon">EMOJI</span>
        <div class="use-case-text">
            <strong>Title</strong>
            <span>Description text</span>
        </div>
    </div>
</div>
```

## Journey Bar 相关

- `.journey-bar` — 进度条下方的固定 bar
- `.journey-bar.hidden` — 在标题 slide 上隐藏
- Journey bar 颜色通过 HSL 插值从红色（0%）过渡到绿色（100%）
- Weight badge 由 JS 自动注入到加权 slide 的 `h1` 元素中
