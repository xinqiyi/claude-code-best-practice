# Claude Subagents Changelog

追踪 `best-practice/claude-subagents.md` 报告的 drift 检查。

**Status Legend：**

| Status | Meaning |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现不正确、不适用或为有意为之 |
| `ON HOLD (reason)` | 操作延期，等待外部依赖或用户决策 |

---

此 changelog 记录了由 workflow-claude-subagents-agent 执行的定期 drift 检查运行。每次运行将报告内容与官方 sub-agents 参考页面和仓库中的实际 agent 文件进行对比验证。

**验证范围：**
- Frontmatter 字段表（字段完整性、类型、required 状态、描述）
- Memory scopes 表
- 内置 agent 表（官方 Claude agents）
- 仓库 agent 表（`.claude/agents/` 中的本地 agents）
- 调用方法表
- URL 和链接有效性

**运行历史：** 覆盖 2026 年 2 月至 6 月，频率为每 1-3 天一次。完整的运行条目列表见英文版 changelog.md。
