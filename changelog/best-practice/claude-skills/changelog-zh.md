# Claude Skills Changelog

追踪 Claude Code Skills 文档和报告的 drift 检查。

**Status Legend：**

| Status | Meaning |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现不正确、不适用或为有意为之 |
| `ON HOLD (reason)` | 操作延期，等待外部依赖或用户决策 |

---

此 changelog 记录了由 workflow-claude-skills-agent 执行的定期 drift 检查运行。每次运行验证 `best-practice/claude-skills.md` 内容与官方 Claude Code 文档的一致性，检查内容包括：

- Frontmatter 字段完整性
- Skill 定义准确性
- Bundled skills 和 slash commands 覆盖率
- 与仓库 SKILL.md 文件的一致性
- URL 和链接有效性

运行频率为每 1-2 天一次，跨越 2026 年 2 月至 6 月。完整的运行条目列表见英文版 changelog.md。
