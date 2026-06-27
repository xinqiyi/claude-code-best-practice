# Code Review 与 Test Time Compute — 来自 Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 3 月 10 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ Introducing Code Review

Claude Code 新功能：**Code Review**。一个 agents 团队对每个 PR 进行深度审查。

- 首先为 Anthropic 自己的团队构建 — 每位工程师的代码产出今年增长了 **200%**，reviews 成为瓶颈
- Boris 已经使用了几周，发现它捕获了许多他本不会注意到的真实 bugs
- 当 PR 打开时，Claude 派遣一组 agents 来查找 bugs

<a href="https://x.com/bcherny/status/2031089411820228645"><img src="assets/boris-26-3-10/0.png" alt="Boris Cherny announcing Code Review" width="50%" /></a>

---

## 2/ Test Time Compute 与 Multiple Context Windows

大致来说，你为一个 coding 问题投入越多的 tokens，结果就越好。Boris 称之为 **test time compute**。

- 使用 **单独的 context windows** 使结果更好 — 这就是 subagents 起作用的原因，也是一个 agent 可能引入 bugs 而另一个（使用完全相同的 model）可以发现它们的原因
- 类似于 engineering teams：如果 Boris 引入了一个 bug，审查代码的同事可能比他更可靠地发现它
- 在极限情况下，agents 可能会编写完美的无 bug 代码 — 在那之前，**多个不相关的 context windows** 往往是一个好方法

<a href="https://x.com/bcherny/status/2031151689219321886"><img src="assets/boris-26-3-10/1.png" alt="Boris Cherny on test time compute" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — March 10, 2026](https://x.com/bcherny)
