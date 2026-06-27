---
paths:
  - "**/*.md"
---

# Markdown Docs

## 文档标准

- 保持文件重点突出、内容简洁 — 每个文件一个主题
- 在文档之间使用相对链接（例如 `../best-practice/claude-memory.md`），而非绝对 GitHub URL
- 在 best-practice 和 report 文档的顶部包含返回导航链接（参见现有文件的模式）
- 当添加新概念或报告时，更新 README.md 中对应的表格（CONCEPTS 或 REPORTS）

## 结构规范

- Best practice 文档放在 `best-practice/` 中
- Implementation 文档放在 `implementation/` 中
- Reports 放在 `reports/` 中
- Tips 放在 `tips/` 中
- Changelog tracking 放在 `changelog/<category>/` 中

## 格式化

- 使用表格进行结构化比较（参见 README CONCEPTS 表作为参考）
- 在链接 best-practice 或 implementation 文档时，使用来自 `!/tags/` 的 badge 图片以保持视觉一致性
- 保持标题层次结构 — 不要跳过标题级别（例如不要从 `##` 直接跳到 `####`）
