# Goal 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-May_13%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#goal-tips-from-the-community"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

`/goal` 让你的 agent 跨多个 turn 持续工作，直到某个条件满足——Claude Code、Codex 和 Hermes Agent 均支持。社区正在围绕几个高杠杆的提示技巧形成共识，这些技巧与 `/goal` 配合使用效果极佳。

---

## 来自社区的 Goal 使用技巧

### 1. 让 agent 为自己提议 goals

<p align="center">
  <img src="assets/impl-goal-claude.png" alt="Alex Finn tweet — /goal is the most underrated AI feature of 2026" width="50%">
</p>

> 来了。Claude Code 刚刚发布了 /goal
>
> 2026 年最被低估的 AI 功能
>
> 现在 Claude Code、Codex 和 Hermes agent 都有了
>
> 它能让你的 agent 完成长时间运行的任务，有时持续数天
>
> 每个人都应该立刻运行这个 prompt：
>
> '基于你对我的了解——我的目标、志向，以及我们已经一起构建的东西——有哪些 3 个 /goals 可以现在就运行，会持续较长时间并带来最好结果？'
>
> 选择一个，然后让它为你构建一个 prompt
>
> 你会得到几个超级强大的 goal prompt 选项，让你的 agent 完成那些能带来惊人成果的长时间任务。
>
> 今晚抽出 15 分钟来试试。你会感谢我的。

**来源：**[Alex Finn (@AlexFinn) on X](https://x.com/AlexFinn/status/2053976411296452887)

---

### 2. 让 agent 为你起草 /goal prompt

<p align="center">
  <img src="assets/impl-goal-codex.png" alt="Meta Alchemist tweet — /goal trick for Codex" width="50%">
</p>

> 想知道 Codex 最好的 /goal 技巧吗？
>
> 只需要告诉你的 Codex：
>
> "阅读这个 session 和仓库，深度分析我们想要实现的确切意图和 goals，然后为我写一个 /goal prompt。
>
> 确保深入挖掘历史记录和我们拥有的文档，做到 100% 明确"
>
> 你还可以加上：
>
> "如果你对某些部分不确定，或者想问我几个问题以进一步明确某些 goals，尽管问"
>
> 然后把 Codex 输出的内容复制粘贴，将开头部分改为 /goal
>
> 它就会不间断地完成你在这个 session/仓库中想要做的所有事情，直到最终完成。

**来源：**[Meta Alchemist (@meta_alchemist) on X](https://x.com/meta_alchemist/status/2054214497443995694)

---

## ![如何使用](../!/tags/how-to-use.svg)

```bash
$ claude
> /goal <condition>
> /goal clear
```

`/goal <condition>` 让 Claude 跨 turn 持续工作，直到一个由 Haiku 评估的条件成立。它与 `/loop`（时间驱动）和 auto 模式（per-tool）互补。需要 Claude Code v2.1.139+。

完整行为请参见[官方文档](https://code.claude.com/docs/en/goal)。
