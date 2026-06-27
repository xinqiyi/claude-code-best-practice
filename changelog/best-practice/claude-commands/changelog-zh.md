# Claude Commands Changelog

追踪 `best-practice/claude-commands.md` 报告的 drift 检查。

**Status Legend：**

| Status | Meaning |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现不正确、不适用或为有意为之 |
| `ON HOLD (reason)` | 操作延期，等待外部依赖或用户决策 |

---

此 changelog 记录了由 workflow-claude-commands-agent 执行的定期 drift 检查运行。每次运行验证 Claude Commands 报告与官方 Claude Code 文档的一致性。

**验证范围：**
- 内置 slash commands 列表的完整性
- Command 参数和行为的准确性
- 仓库中的自定义 commands（`.claude/commands/`）
- 调用语法和示例
- URL 和链接有效性

**运行历史：** 覆盖 2026 年 2 月至 6 月，频率为每 1-3 天一次。完整的运行条目列表见英文版 changelog.md。
