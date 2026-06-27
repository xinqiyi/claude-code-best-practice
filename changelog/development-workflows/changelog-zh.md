# Development Workflows Changelog

追踪 README.md DEVELOPMENT WORKFLOWS section 的 drift 检查。

**Status Legend：**

| Status | Meaning |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现不正确、不适用或为有意为之 |
| `ON HOLD (reason)` | 操作延期，等待外部依赖或用户决策 |

---

此 changelog 记录了由 development-workflows-research-agent 执行的定期 drift 检查运行。每次运行扫描 GitHub 上开发工作流相关的仓库，并验证其与 README.md 中 DEVELOPMENT WORKFLOWS 表的一致性。

**验证范围：**
- Star 数量变化追踪
- 仓库功能描述更新
- 新仓库发现和添加
- 低于阈值的仓库排除
- 排序顺序验证
- Plugin/工作流分类准确性

**运行历史：** 覆盖 2026 年 2 月至 6 月，频率为每 1-3 天一次。完整的运行条目列表见英文版 changelog.md。
