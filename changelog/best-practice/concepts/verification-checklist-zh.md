# Verification Checklist — README CONCEPTS Section

CONCEPTS 表格准确性的验证规则。每次 workflow 运行期间检查每条规则。

## 规则

### 1. External URL Liveness
- **Category**: URL Accuracy
- **检查内容**: CONCEPTS 表中的每个外部 URL（docs 链接）返回有效页面
- **Depth**: 获取每个 URL 并确认加载的是预期页面（而非重定向到错误页面）
- **对比来源**: `https://code.claude.com/docs/llms.txt` 获取标准 URL 列表
- **添加日期**: 2026-03-02
- **来源**: 发现 Permissions URL `/iam` 重定向到 Authentication 页面而非 Permissions

### 2. Anchor Fragment Validity
- **Category**: URL Accuracy
- **检查内容**: 任何带有锚点片段（`#section-name`）的 URL 匹配目标页面上的实际标题
- **Depth**: 获取页面并验证标题存在且锚点正确
- **对比来源**: 获取的页面内容
- **添加日期**: 2026-03-02
- **来源**: Rules 锚点 `#modular-rules-with-clauderules` 已过期；section 重命名为 `#organize-rules-with-clauderules`

### 3. Missing Docs Pages
- **Category**: Missing Concepts
- **检查内容**: 官方文档索引（`llms.txt`）中每个代表面向用户功能的页面在 CONCEPTS 表中都有对应行
- **Depth**: 对照 CONCEPTS 表条目比较完整文档索引
- **对比来源**: `https://code.claude.com/docs/llms.txt`
- **添加日期**: 2026-03-02
- **来源**: 发现多个缺失的概念（Agent Teams、Keybindings、Model Configuration 等）

### 4. Local Badge Link Validity
- **Category**: Badge Accuracy
- **检查内容**: CONCEPTS 表中的每个 badge 目标路径（`best-practice/*.md`、`implementation/*.md`、`.claude/*/`）指向存在的文件或目录
- **Depth**: 使用 Read/Glob 验证文件存在
- **对比来源**: 本地文件系统
- **添加日期**: 2026-03-02
- **来源**: 初始 checklist 创建

### 5. Description Currency
- **Category**: Description Accuracy
- **检查内容**: 每个概念的描述准确反映当前官方文档描述
- **Depth**: 将 README 描述与官方页面的 meta description 或第一段进行比较
- **对比来源**: 官方文档页面内容
- **添加日期**: 2026-03-02
- **来源**: Memory 描述缺少 auto memory；MCP Servers location 缺少 `.mcp.json`

### 6. TIPS Section URL Consistency
- **Category**: URL Accuracy
- **检查内容**: 当 CONCEPTS 或 Hot 表中的 URL 更新时，同时检查 TIPS section 中是否存在相同的过期 URL
- **Depth**: 在 TIPS section 中搜索 CONCEPTS/Hot 表中被标记的每个 URL
- **对比来源**: llms.txt sitemap + CONCEPTS 表 URLs
- **添加日期**: 2026-04-16
- **来源**: Hot 表中修复了 `web-scheduled-tasks` URL（2026-04-14）但 TIPS 中相同的过期 URL 依然存在

### 7. Beta Badge Currency
- **Category**: Badge Accuracy
- **检查内容**: Hot 表中标记为 `![beta](!/tags/beta.svg)` 的概念在其官方文档页面中仍应被标记为 beta / research preview
- **Depth**: 获取每个上游页面并检查明确的 beta/preview/experimental 措辞
- **对比来源**: 官方文档页面 banner 文本 + GA 标记
- **添加日期**: 2026-04-26
- **来源**: workflow-concepts-agent 标记 Routines、No Flicker Mode、Computer Use 和 Code Review 带有 README beta badges，但其文档页面读起来像 GA

### 8. Location Column Factual Accuracy
- **Category**: Description Accuracy
- **检查内容**: 每个概念的 Location 列值必须事实性地匹配官方文档中描述的机制
- **Depth**: 对于任何做出机制声明的 Location 值，获取官方页面并确认声明与文档自身的描述一致
- **对比来源**: 官方文档页面正文
- **添加日期**: 2026-05-21
- **来源**: Checkpointing Location 之前每次运行都读作 `automatic (git-based)`，但官方文档明确说明 checkpoints 追踪文件编辑工具的变化

### 9. Bundled Skill / Command Rename Tracking
- **Category**: Description Accuracy（Location column）
- **检查内容**: CONCEPTS/Hot 的 Location 列中命名的任何 slash command 或 bundled skill 仍以该确切名称存在
- **Depth**: 扫描 CHANGELOG 中"重命名"、"移除"或"弃用"的 command/skill 事件
- **对比来源**: GitHub CHANGELOG + 官方 commands/skills 参考页面
- **添加日期**: 2026-05-25
- **来源**: `/simplify` 在 v2.1.147 中被重命名为 `/code-review`
