# Verification Checklist — README CONCEPTS 部分

用于验证 CONCEPTS 表格准确性的规则。每条规则在每次 workflow 运行时都会被检查。

## 规则

### 1. 外部 URL 可用性
- **类别**: URL 准确性
- **检查内容**: CONCEPTS 表格中的每个外部 URL（文档链接）都返回有效页面
- **检查深度**: 获取每个 URL 并确认其加载了预期页面（而非重定向到错误页面）
- **对比来源**: `https://code.claude.com/docs/llms.txt` 作为规范 URL 列表
- **添加日期**: 2026-03-02
- **起因**: Permissions URL `/iam` 被发现重定向到 Authentication 页面而非 Permissions 页面

### 2. 锚点片段有效性
- **类别**: URL 准确性
- **检查内容**: 任何带有锚点片段（`#section-name`）的 URL 都能匹配目标页面上的实际标题
- **检查深度**: 获取页面并验证该标题存在且具有预期的锚点
- **对比来源**: 获取的页面内容
- **添加日期**: 2026-03-02
- **起因**: Rules 锚点 `#modular-rules-with-clauderules` 已失效；该部分已重命名为 `#organize-rules-with-clauderules`

### 3. 缺失的文档页面
- **类别**: 遗漏的概念
- **检查内容**: 官方文档索引（`llms.txt`）中每个代表面向用户功能的页面，在 CONCEPTS 表格中都有对应的行
- **检查深度**: 将完整文档索引与 CONCEPTS 表格条目进行对比
- **对比来源**: `https://code.claude.com/docs/llms.txt`
- **添加日期**: 2026-03-02
- **起因**: 发现多个遗漏的概念（Agent Teams、Keybindings、Model Configuration 等）

### 4. 本地徽章链接有效性
- **类别**: 徽章准确性
- **检查内容**: CONCEPTS 表格中的每个徽章目标路径（`best-practice/*.md`、`implementation/*.md`、`.claude/*/`）都指向一个存在的文件或目录
- **检查深度**: 使用 Read/Glob 验证文件是否存在
- **对比来源**: 本地文件系统
- **添加日期**: 2026-03-02
- **起因**: 初始检查清单创建

### 5. 描述时效性
- **类别**: 描述准确性
- **检查内容**: 每个概念的描述准确反映当前官方文档的描述
- **检查深度**: 将 README 描述与官方页面的 meta description 或第一段进行对比
- **对比来源**: 官方文档页面内容
- **添加日期**: 2026-03-02
- **起因**: Memory 描述缺少 auto memory；MCP Servers 位置缺少 `.mcp.json`

### 6. TIPS 部分 URL 一致性
- **类别**: URL 准确性
- **检查内容**: 当 CONCEPTS 或 Hot 表格中的 URL 被更新时，同时检查 TIPS 部分是否存在相同的过时 URL
- **检查深度**: 搜索 TIPS 部分（第 125–267 行），查找所有在 CONCEPTS/Hot 表格中被标记的 URL
- **对比来源**: llms.txt sitemap + CONCEPTS 表格 URL
- **添加日期**: 2026-04-16
- **起因**: `web-scheduled-tasks` URL 已在 Hot 表格中修复（2026-04-14），但相同的过时 URL 仍存在于 TIPS 部分（第 223 行）

### 7. Beta 徽章时效性
- **类别**: 徽章准确性
- **检查内容**: Hot 表格中标记为 `![beta](!/tags/beta.svg)` 的概念，在其官方文档页面中仍应被标记为 beta / research preview（标题横幅、"research preview" 字样或环境变量门控）
- **检查深度**: 获取每个上游页面并检查是否有明确的 beta/preview/experimental 措辞；如果没有，则 README 徽章可能已过时
- **对比来源**: 官方文档页面的横幅文本 + GA 标记字样
- **添加日期**: 2026-04-26
- **起因**: Workflow-concepts-agent 标记出 Routines、No Flicker Mode、Computer Use 和 Code Review 在 README 中带有 beta 徽章，而其文档页面已显示为 GA（置信度 0.6）——这种漂移类型未被现有的描述时效性规则覆盖

### 8. Location 列事实准确性
- **类别**: 描述准确性
- **检查内容**: 每个概念的 **Location** 列（第 2 列）值必须在事实上与官方文档中描述的机制相匹配——不仅仅是路径/命令/标志存在，还要确保其特征描述正确（例如，某个功能如何存储状态、它跟踪什么、配置存放在哪里）
- **检查深度**: 对于任何做出机制性声明的 Location 值（例如 "git-based"、"automatic"、"built-in (env var)"、"cloud backend"），获取官方页面并确认该声明与文档自身对机制的描述一致
- **对比来源**: 官方文档页面正文（机制/"how it works" 部分）
- **添加日期**: 2026-05-21
- **起因**: Checkpointing 的 Location 在之前的每次运行中都显示为 `automatic (git-based)`，但 `/en/checkpointing` 明确说明 checkpoints 跟踪的是文件编辑工具（file-editing-tool）的变更（而非 git，也非 bash），并且"不能替代版本控制"——*"checkpoints 作为'本地撤销'，Git 作为'永久历史'。"* 现有规则 #5（描述时效性）仅检查 Description 列，因此 Location 列中的事实错误长期未被发现
