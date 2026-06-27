# Squash Merging 与 PR Size Distribution — 来自 Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 3 月 25 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 单日 266 次 Contributions — Always Squash

Boris 分享了他的 GitHub contribution graph，显示 **3 月 24 日 266 次 contributions** — 来自 **141 个 PRs，always squashed**，每个 PR 中位数为 **118 lines**。

- Squash merging 将所有 branch commits 合并为 target branch 上的单个 commit — 保持历史记录干净且线性
- 每个 PR = 一个 commit 使得回滚整个 features 变得容易，并简化了 `git bisect`
- 在高速度 AI 辅助 workflows（每天 141 个 PRs）中，squash 是务实的选择 — branch 内的个别 "fix lint"、"try this" commits 只是噪音

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-26-3-25/1.png" alt="Boris Cherny — 266 contributions, always squashed" width="50%" /></a>

---

## 2/ PR Size Distribution — 保持 PRs 小而精

Boris 分享了那 141 个 PRs 的大小分布，总计 **45,032 lines changed**（additions + deletions）：

| Metric | Lines (add+del) | Meaning |
|--------|---------------:|---------|
| **p50** | **118** | Median PR size — 一半的 PRs 为 118 lines 或更少 |
| p90 | 498 | 90% 的 PRs 低于 500 lines |
| **p99** | **2,978** | 只有大约 1 个 PR 超过约 3K lines |
| min | 2 | Smallest PR — 一个快速的 2-line fix |
| max | 10,459 | Largest single PR — 可能是 migration 或 generated code |

- **中位数 118 lines** 意味着大多数 PRs 是专注且可审查的，即使在每天 141 个 PRs 的情况下
- 分布严重右偏 — 偶尔会有大型 PR 是不可避免的（批量重命名、migrations），但常态是紧凑的
- 小型 PRs 降低了 merge conflict 风险，更易于审查，并且与 squash merging 搭配完美实现干净的 reverts

<a href="https://x.com/bcherny/status/2038552880018538749"><img src="assets/boris-26-3-25/2.png" alt="Boris Cherny — PR size distribution table" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — March 25, 2026](https://x.com/bcherny)
