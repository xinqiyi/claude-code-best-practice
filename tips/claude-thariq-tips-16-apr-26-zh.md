# 使用 Claude Code：Session Management 与 1M Context — Thariq

Thariq（[@trq212](https://x.com/trq212)）于 2026 年 4 月 16 日分享的关于管理 sessions、context windows 和 compaction 的指南。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Context

借助 1M token context window，Claude Code 可以更可靠地处理更长的任务 — 但如果你不刻意管理 sessions，它也打开了 context pollution 的大门。Session management 比以往任何时候都重要：何时重新开始、何时 compact、何时 rewind 以及何时委托给 subagents。

<img src="assets/thariq-26-4-16/1.png" alt="Thariq intro tweet" width="50%" />

<img src="assets/thariq-26-4-16/2.png" alt="Session management intro" width="50%" />

---

## Context、Compaction 与 Context Rot 快速入门

Context window 是 model 在生成其下一个响应时一次性"看到"的一切。它包括你的 system prompt、到目前为止的对话、每个 tool call 及其输出，以及每个被读取的文件。Claude Code 有 **一百万个 tokens** 的 context window。

不幸的是，使用 context 有一个轻微的代价 — **context rot**。Model 性能随着 context 增长而下降，因为 attention 分散到更多的 tokens 上，而较旧的、不相关的内容开始分散当前任务的注意力。对于 1M context model，某种程度的 context rot 发生在大约 **~300-400k tokens** 时，但这高度依赖于任务 — 不是一个硬性规则。

Context windows 是硬性截止。当你接近末尾时，你需要总结任务并在新的 context window 中继续 — 这就是 **compaction**。你也可以自己触发 compaction。

<img src="assets/thariq-26-4-16/3.png" alt="Context window diagram" width="50%" />

<img src="assets/thariq-26-4-16/4.png" alt="Context rot explanation" width="50%" />

---

## 每个 Turn 都是一个 Branching Point

Claude 完成一个 turn 后，你对于下一步有出人意料多的选择：

- **Continue** — 在同一 session 中发送另一条消息
- **/rewind (esc esc)** — 跳回到之前的消息并从那里重试
- **/clear** — 开始一个新 session，通常附带你从刚才学到的内容中提炼的 brief
- **Compact** — 总结到目前为止的 session 并在总结的基础上继续
- **Subagents** — 将下一块工作委托给一个拥有自己干净 context 的 agent，只将其结果拉回

虽然最自然的是继续，但其他四个选项的存在是为了帮助你管理 context。

<img src="assets/thariq-26-4-16/5.png" alt="Compaction and branching diagram" width="50%" />

<img src="assets/thariq-26-4-16/6.png" alt="Five options after a turn" width="50%" />

每个选项携带不同数量的现有 context：

| Fresh session | Compact | Subagent | Rewind | Continue |
|:---:|:---:|:---:|:---:|:---:|
| your brief only | lossy summary | all + result | prefix kept, tail cut | everything stays |
| *none of it* | | | | *all of it* |

<img src="assets/thariq-26-4-16/7.png" alt="Context carry-forward spectrum" width="50%" />

---

## 何时开始 New Session

新的 1M context windows 意味着你现在可以更可靠地完成更长的任务 — 例如从头构建一个 full-stack app。但仅仅因为你的 model 没有耗尽 context，并不意味着你不应该开始一个新的 session。

**一般经验法则：当你开始一个新任务时，你也应该开始一个新 session。**

一个灰色地带是当你想做相关任务，其中一些 context 仍然是必要的但并非全部时。例如，为你刚刚实现的一个 feature 做文档。虽然你可以开始一个新 session，但 Claude 将不得不重新读取文件，这会变慢且更贵。由于文档可能不是一个对智能高度敏感的任务，额外的 context 可能值得效率的提升。

<img src="assets/thariq-26-4-16/8.png" alt="When to start a new session" width="50%" />

---

## Rewinding 而非 Correcting

如果 Thariq 必须选出一个代表良好 context management 的习惯，那就是 **rewind**。

在 Claude Code 中，双击 Esc（或运行 `/rewind`）让你跳回到任何之前的消息并从那里重新 prompt。该点之后的消息将从 context 中丢弃。

**Correcting**（在失败的尝试 A 后说 "no, try B"）将失败的尝试留在 context 中：
> context = reads + 2 failed attempts + 2 corrections + the fix

**Rewinding**（回到失败尝试之前，用你学到的重新 prompt）更干净：
> context = reads + one informed prompt + the fix

Rewind 通常是更好的方法。例如，Claude 读取了五个文件，尝试了一种方法，结果不成功。你的本能可能是输入 "that didn't work, try X instead." 但更好的做法是 rewind 到文件读取之后，用你学到的重新 prompt："Don't use approach A, the foo module doesn't expose that — go straight to B."

你还可以使用 **"summarize from here"** 让 Claude 总结其学习并提供交接消息，有点像来自未来自己的消息，告诉之前的 Claude 迭代 "我尝试了，但不行"。

<img src="assets/thariq-26-4-16/9.png" alt="Correcting vs rewinding diagram" width="50%" />

<img src="assets/thariq-26-4-16/10.png" alt="Rewind with summarize from here" width="50%" />

---

## Compacting 与 Fresh Sessions

一旦 session 变长，你有两种方式减负：`/compact` 或 `/clear`（然后重新开始）。它们感觉相似但行为非常不同。

**Compact** 要求 model 总结到目前为止的对话，然后用该总结替换历史。这是有损的 — 你信任 Claude 来决定什么是重要的，但你自己不需要写任何东西。Claude 在包含重要学习或文件方面可能更彻底。你也可以通过传递指令来引导它（`/compact focus on the auth refactor, drop the test debugging`）。

- **任务进行中**，保持动量 — 细节可能模糊
- 便宜，继续前进

**Fresh + brief**（`/clear`）意味着*你*写下重要的内容（"we're refactoring the auth middleware, the constraint is X, the files that matter are A and B, we've ruled out approach Y"）并全新开始。工作量更大，但产生的 context 是*你*决定相关的。

- **高风险的**下一步 — 在 100K 的探索中发现了一个事实
- 更多工作，更精确

<img src="assets/thariq-26-4-16/11.png" alt="Compacting vs fresh sessions" width="50%" />

<img src="assets/thariq-26-4-16/12.png" alt="Compact vs fresh diagram" width="50%" />

---

## 什么导致 Bad Compact？

如果你运行很多长时间运行的 sessions，你可能注意到某些时候 compacting 可能特别糟糕。Bad compacts 发生在 model 无法预测你工作方向的时候。

例如，autocompact 在长时间 debugging session 后触发并总结了调查。你的下一条消息是 "now fix that other warning we saw in bar.ts." 但由于 session 专注于 debugging，另一个 warning 可能已从总结中丢弃。

这特别困难，因为由于 context rot，model 在 compacting 时处于最不智能的状态。拥有一百万 context，你有更多时间用你想做的事情的描述来主动 `/compact`。

<img src="assets/thariq-26-4-16/13.png" alt="Bad compact diagram" width="50%" />

<img src="assets/thariq-26-4-16/14.png" alt="Bad compact explanation" width="50%" />

---

## Subagents 与 Fresh Context Windows

Subagents 是 context management 的一种形式，适用于你提前知道某块工作会产生大量你以后不需要的中间输出的情况。

当 Claude 通过 Agent tool spawn 一个 subagent 时，该 subagent 获得自己的 fresh context window。它可以做尽可能多的工作，然后合成其结果，只将最终报告返回给父级。

心理测试：**我以后会需要这个 tool output 还是只需要结论？**

探索噪音在 subagent 退出时被垃圾回收 — 20 个文件读取、12 个 greps、3 个死胡同 — 只有最终报告返回到父级 context。

虽然 Claude Code 会自动调用 subagents，你可能想告诉它明确这样做。例如：

- "Spin up a subagent to verify the result of this work based on the following spec file"
- "Spin off a subagent to read through this other codebase and summarize how it implemented the auth flow, then implement it yourself in the same way"
- "Spin off a subagent to write the docs on this feature based on my git changes"

<img src="assets/thariq-26-4-16/15.png" alt="Subagent context diagram" width="50%" />

<img src="assets/thariq-26-4-16/16.png" alt="Subagent explanation" width="50%" />

<img src="assets/thariq-26-4-16/17.png" alt="When to use subagents" width="50%" />

---

## Summary

当 Claude 结束一个 turn 且你即将发送新消息时，你有一个决策点。随着时间的推移，Claude 会自己处理这个问题，但目前这是你引导 Claude 输出的方式之一。

| Situation | Reach for | Why |
|-----------|-----------|-----|
| Same task, context is still relevant | **Continue** | 窗口中的所有内容仍然有效 — 不值得花成本重建 |
| Claude went down a wrong path | **Rewind** (double-Esc) | 保留有用的文件读取，丢弃失败的尝试，用你学到的重新 prompt |
| Mid-task but session is bloated with stale debugging/exploration | **/compact \<hint\>** | 低努力；Claude 决定什么重要。需要时用提示引导它 |
| Starting a genuinely new task | **/clear** | 零 rot；你完全控制什么被携带前进 |
| Next step will generate lots of output you'll only need the conclusion from | **Subagent** | 中间工具噪音停留在子 context 中；只有结果返回 |

<img src="assets/thariq-26-4-16/18.png" alt="Summary" width="50%" />

<img src="assets/thariq-26-4-16/19.png" alt="Decision table" width="50%" />

---

## Sources

- [Thariq (@trq212) on X — April 16, 2026](https://x.com/trq212)
