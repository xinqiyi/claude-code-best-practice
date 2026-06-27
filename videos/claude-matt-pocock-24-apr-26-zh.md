# 完整演练：AI 编码工作流 — Matt Pocock

Matt Pocock（[@mattpocockuk](https://x.com/mattpocockuk)），教育者和 AI 教师，在大会 workshop 上的文字记录，发布于 2026 年 4 月 24 日。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **演讲者：** Matt Pocock（教育者、AI 教师）
- **形式：** 大会 workshop（约 2 小时）
- **发布日期：** 2026 年 4 月 24 日
- **YouTube：** [在 YouTube 上观看](https://youtu.be/-QFHIoCo-Ko)

---

## 核心要点

### 核心论点

当我们谈论 AI 是新范式时，我们忘记了**软件工程基础**——真正对与人类合作至关重要的东西——对 AI 也同样有效。

### LLM 的约束：Smart Zone vs Dumb Zone

- 来自 Dex Hy（HumanLayer）的概念：
  - **Smart Zone：** 新鲜 session 时，attention 关系最不紧张，LLM 表现最好
  - **Dumb Zone：** 每增加一个 token，attention 关系以二次方增长（像足球队联赛一样）
  - 大约在 context 的 40% 或约 100K token 处，LLM 开始变笨
  - 无论使用 1M context window 还是 200K，这个临界点大致相同

**对策：** 将任务大小控制在 smart zone 内。

### 多阶段计划 vs Ralph Wiggum Loop

- 传统方法：多阶段计划（Phase 1→2→3→4），在每个阶段内保持在 smart zone
- **Ralph Wiggum 方法：** 指定最终目标（PRD），然后对 AI 说"做一个小的改变，让我们离目标更近"，不断循环
- Matt 偏好更多结构，但原则相同——小步快跑

### 关键教训

- "不要 bite off more than you can chew"（Martin Fowler、《Pragmatic Programmer》的经典建议，对 AI 同样适用）
- 当 context 填满时，compact/重置是必要的
- 软件工程基础（分解、模块化、逐步验证）在 AI 时代比以往更重要

---

## Sources

- [Full Walkthrough: Workflow for AI Coding — Matt Pocock — YouTube](https://youtu.be/-QFHIoCo-Ko)
