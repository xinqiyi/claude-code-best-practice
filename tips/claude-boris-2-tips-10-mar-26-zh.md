# 代码审查与测试时计算 — Boris Cherny 的见解

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创建者，于 2026 年 3 月 10 日分享的见解总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 1/ 引入代码审查

Claude Code 新功能：**代码审查**。一个 agent 团队对每个 PR 执行深入审查。

- 首先为 Anthropic 自身团队构建 — 今年每位工程师的代码产出提升了 **200%**，代码审查成为瓶颈
- Boris 已使用数周，发现它能捕获许多他本不会注意到的真实 bug
- 当 PR 开启时，Claude 会派遣一个 agent 团队来寻找 bug

<a href="https://x.com/bcherny/status/2031089411820228645"><img src="assets/boris-26-3-10/0.png" alt="Boris Cherny 宣布代码审查" width="50%" /></a>

---

## 2/ 测试时计算与多上下文窗口

大致来说，你投入到一个编程问题上的 token 越多，结果就越好。Boris 称之为**测试时计算**。

- 使用**独立的上下文窗口**会使结果更好 — 这正是 subagent 工作的原理，也是为什么一个 agent 可能引入 bug，而另一个（使用完全相同的模型）却能发现它们
- 类似于工程团队：如果 Boris 引入了一个 bug，他的同事在审查代码时可能比他本人更可靠地发现它
- 从长远来看，agent 可能会写出完美无 bug 的代码 — 在那之前，**多个不相关的上下文窗口**通常是一个好方法

<a href="https://x.com/bcherny/status/2031151689219321886"><img src="assets/boris-26-3-10/1.png" alt="Boris Cherny 谈论测试时计算" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 3 月 10 日](https://x.com/bcherny)
