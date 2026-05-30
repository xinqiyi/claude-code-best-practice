# Goal 实现

![最后更新](https://img.shields.io/badge/Last_Updated-May_13%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#goal-tips-from-the-community"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

`/goal` 让你的 agent 跨轮次持续工作，直到某个条件被满足 — Claude Code、Codex 和 Hermes Agent 都支持它。社区正在汇聚一些与之配合良好的高杠杆提示技巧。

---

## 来自社区的 Goal 技巧

### 1. 让 agent 提出自己的目标

<p align="center">
  <img src="assets/impl-goal-claude.png" alt="Alex Finn 推文 — /goal 是 2026 年被最低估的 AI 功能" width="50%">
</p>

> 官方消息。Claude Code 刚刚发布了 /goal
>
> 2026 年被最低估的 AI 功能
>
> 现在 Claude Code、Codex 和 Hermes agent 都拥有它
>
> 它让你的 agent 能够完成长时间运行的任务，有时甚至持续数天
>
> 每个人都应该立即运行这个 prompt：
>
> '基于你对我的了解、我的目标、抱负以及我们已经共同构建的成果，有哪些 3 个 /goal 我们现在可以运行，能够长时间执行并产生最佳结果？'
>
> 选择一个，然后让它为你构建一个 prompt
>
> 你会得到几个超级强大的 goal prompt 选项，让你选择的 agent 完成长时间运行的任务，带来令人惊叹的结果。
>
> 今晚抽出 15 分钟来做这件事。之后再来感谢我。

**来源：** [Alex Finn (@AlexFinn) 在 X 上](https://x.com/AlexFinn/status/2053976411296452887)

---

### 2. 让 agent 为你起草 /goal prompt

<p align="center">
  <img src="assets/impl-goal-codex.png" alt="Meta Alchemist 推文 — Codex 的 /goal 技巧" width="50%">
</p>

> 想知道 Codex 最好的 /goal 技巧吗？
>
> 直接告诉你的 Codex：
>
> "阅读此 session 和 repo，深入分析我们要在此实现的确切意图和目标，然后为我写出 /goal prompt。
>
> 确保深入挖掘历史记录和文档，做到 100% 清晰"
>
> 你也可以添加：
>
> "如果对某些部分不确定，或者想问我几个问题来进一步明确某些目标，请尽管问"
>
> 然后直接复制粘贴 Codex 给出的内容，将开头部分改为 /goal
>
> 它就会在该 session / repo 中持续不断地完成你原本想做的事情，直到全部完成。

**来源：** [Meta Alchemist (@meta_alchemist) 在 X 上](https://x.com/meta_alchemist/status/2054214497443995694)

---

## ![使用方法](../!/tags/how-to-use.svg)

```bash
$ claude
> /goal <condition>
> /goal clear
```

`/goal <condition>` 让 Claude 跨轮次持续工作，直到一个由 Haiku 评估的条件成立。它与 `/loop`（时间驱动）和 auto mode（按工具驱动）是互补的。需要 Claude Code v2.1.139+。

查看[官方文档](https://code.claude.com/docs/en/goal)了解完整行为。
