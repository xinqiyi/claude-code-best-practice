# Concepts Changelog

追踪 README.md CONCEPTS 和 HOT 表的 drift 检查。

**Status Legend：**

| Status | Meaning |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现不正确、不适用或为有意为之 |
| `ON HOLD (reason)` | 操作延期，等待外部依赖或用户决策 |

---

此 changelog 记录了由 workflow-concepts-agent 执行的定期 drift 检查运行。每次运行验证 README.md 中的 CONCEPTS 表和 HOT 表与官方 Claude Code 文档的一致性。

**验证范围：**
- URL 有效性（外部链接加载、锚点片段）
- 缺失的概念（docs 索引 vs CONCEPTS 表）
- Badge 有效性（本地路径、beta 标记时效性）
- 描述时效性（README 描述 vs 官方文档）
- Location 列事实准确性
- 重命名的 commands/skills 追踪
- 与 TIPS section 的跨引用一致性

**运行历史：** 覆盖 2026 年 2 月至 6 月，频率为每 1-3 天一次。完整的运行条目列表（包含数百个概念/URL/drift 项目）见英文版 changelog.md。
