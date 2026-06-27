# Claude Code 的秘密：来自构建它的工程师 — Every

Cat 和 Boris（Claude Code 工程师）在 Every 播客上的访谈文字记录，发布于 2025 年 10 月 29 日。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **嘉宾：** Cat 和 Boris（Claude Code 工程师，Anthropic）
- **主持人：** Every
- **发布日期：** 2025 年 10 月 29 日
- **YouTube：** [在 YouTube 上观看](https://youtu.be/IDSAMqip6ms)

---

## 核心要点

### Claude Code 的核心设计理念

- **终端优先：** Claude Code 在终端运行，意味着"工程师能做的任何事情，Claude Code 都能做。没有中间层"
- 这一设计最初并非有意为之——Boris 在终端中原型化是因为这是最简单的方式（不需要构建 UI）
- 当给模型提供工具（bash）时，它开始自发使用它们——"模型只是想要使用工具"

### 工具选择的演变

- Claude Code 目前有十几个工具，团队"大多数每一两周都会添加和移除工具"
- 最近移除了操作系统级别的工具，因为 bash 权限系统可以统一执行
- **原则：** 给模型更少的选择，更少的 context 内容，越多工具意味着越多 context 消耗

### 内部采用情况

- Anthropic 内部有越来越多的人在 Claude Code 上花费超过 $1,000/月
- 出现了"power user"行为模式
- 这符合 YC 的经验："如果你能解决自己的问题，你更有可能在解决别人的问题"

### Latent Demand 产品哲学

- Claude Code 被设计成"可被 hack"的——足够开放，用户可以为非设计用途滥用它
- 这是 Facebook 的传统：观察用户如何滥用产品，然后围绕已有的意图构建
- Claude Code 的很多成功功能来自于用户找到了意想不到的用途

### Claude Code 的前身

- Claude Code 的前身叫 "Clyde"（CLI + Claude）
- 这是一个研究项目，需要一分钟才能启动，是"非常重的 Python 项目"
- Boris 加入 Anthropic 的第一个 PR 被拒绝了——因为他是手写的！
- 他的 ramp-up buddy Adam Wolf 说："你手写的？你在干什么？用 Clyde！"

### 范式转变的关键时刻

- 当 Claude Code 获得了 bash 工具后，它"开始写 AppleScript 来自动化事情"
- Boris 说："这是最疯狂的事情。我从未见过这样的事情"
- Cat 的关键时刻是 Sonnet 4 / Opus 4 发布时——"天哪，这个东西真的能用了"

---

## 关键引用

- "What made it work really well is that quad code has access to everything that an engineer does at the terminal. Everything you can do, quad code can do. There's nothing in between."（它如此有效的原因在于 Claude Code 能访问工程师在终端能做的一切。你能做的一切，Claude Code 都能做。中间没有任何东西。）

- "There's actually an increasing number of people internally at anthropic that are using like a lot of credits like spending like over a,000 bucks every month."（Anthropic 内部有越来越多的人每月花费超过 $1,000 的 credits。）

- "This is something that they teach in YC. If you can solve your own problem, it's much more likely you're solving the problem for others."（这是 YC 教的东西。如果你能解决自己的问题，你更有可能在解决别人的问题。）

- "There's this like really old idea in product called latent demand. You build a product in a way that is hackable that is kind of open-ended enough that people can abuse it for other use cases."（产品中有一个非常古老的概念叫 latent demand。你以一种可被 hack 的方式构建产品，足够开放，以至于人们可以为其他用例滥用它。）

---

## Sources

- [The Secrets of Claude Code From the Engineers Who Built It — Every — YouTube](https://youtu.be/IDSAMqip6ms)
