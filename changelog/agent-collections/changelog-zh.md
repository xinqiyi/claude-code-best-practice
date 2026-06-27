# Agent Collections — Changelog

追踪 `README.md` 中 AGENT COLLECTIONS 表的更新。

## Status Legend

- `COMPLETE (reason)` — action item 已成功执行
- `INVALID (reason)` — action item 被确定为不需要或不正确
- `ON HOLD (reason)` — action item 延期待后续处理

---

## 最近更新摘要

此 changelog 追踪两个主要 agent collection 仓库的定期（每日/半每日）自动化 drift 检查：

- **msitarzewski/agency-agents**（115k+ stars，271 agents）— 最全面的多领域 agent 集合，覆盖 16+ 个领域
- **VoltAgent/awesome-claude-code-subagents**（22k stars，156 agents）— Claude Code 专用的 subagent 集合

每次运行检查：
1. Star 数量变化
2. Agent 数量变化（对抗跨运行的不一致——README badge 数 vs 实际目录文件数 vs GitHub code search 数）
3. 排序顺序验证

### 关键观察

- agency-agents 持续增长，从 109k 涨至 115k stars 跨越多个 k-boundaries
- agent 数量在 README 自报数（~232）与实际文件数（~271）之间存在持续振荡（跨越 20+ 次运行）
- VoltAgent 的 agent 数量在 153-159 范围内波动（±3-5），无确认的净方向性变化

（完整条目见英文版 changelog.md，包含每次运行的详细数据）
