# 使用 Claude Code 的 10 个技巧 — 来自 Claude Code 团队

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 2 月 1 日分享的团队技巧总结。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Context

Boris 分享了直接来自 Claude Code 团队的使用技巧。团队使用 Claude 的方式与 Boris 个人使用的方式不同。记住：没有唯一正确的 Claude Code 使用方式 — 每个人的设置都不同。你应该实验看看什么对你最有效！

<a href="https://x.com/bcherny/status/2017742741636321619"><img src="assets/boris-26-2-1/0.png" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ 更多地并行工作

同时启动 3–5 个 git worktree，每个运行自己的 Claude 会话并行工作。这是最大的生产力解锁，也是团队的首选技巧。个人而言，Boris 使用多个 git checkout，但 Claude Code 团队的大多数人更喜欢 worktree — 这就是 `@amorisscode` 在 Claude Desktop 应用中为其构建原生支持的原因！

有些人还会命名他们的 worktree 并设置 shell 别名（`2a`、`2b`、`2c`），以便一键在它们之间切换。其他人有一个专用的"analysis" worktree，仅用于读取日志和运行 BigQuery。

See: [Worktrees Docs](https://code.claude.com/docs/en/common...)

<a href="https://x.com/bcherny/status/2017742743125299476"><img src="assets/boris-26-2-1/1.png" alt="Do more in parallel" width="50%" /></a>

---

## 2/ 每个复杂任务都以 Plan Mode 开始

将精力投入计划中，这样 Claude 就可以一次性完成实现。

有人让一个 Claude 编写计划，然后启动第二个 Claude 以 staff engineer 的身份审查它。

另一个人说，一旦事情出现问题，他们就切换回 plan mode 重新规划。不要继续硬推。他们还会明确告诉 Claude 在验证步骤中进入 plan mode，而不仅仅是构建阶段。

<a href="https://x.com/bcherny/status/2017742745365057733"><img src="assets/boris-26-2-1/2.png" alt="Start every complex task in plan mode" width="50%" /></a>

---

## 3/ 投资你的 CLAUDE.md

每次纠正后，以这句话结束："Update your CLAUDE.md so you don't make that mistake again." Claude 在为自身编写规则方面出奇地擅长。

随着时间的推移，无情地编辑你的 `CLAUDE.md`。持续迭代，直到 Claude 的错误率显著下降。

一位工程师告诉 Claude 为每个 task/project 维护一个 notes 目录，每次 PR 后更新。然后他们将 `CLAUDE.md` 指向它。

<a href="https://x.com/bcherny/status/2017742747067945390"><img src="assets/boris-26-2-1/3.png" alt="Invest in your CLAUDE.md" width="50%" /></a>

---

## 4/ 创建你自己的 Skills 并提交到 Git

在每个项目中复用。团队的建议：

- 如果你每天做某件事超过一次，就把它变成一个 skill 或 command
- 构建一个 `/techdebt` slash command，在每个会话结束时运行，以查找并消除重复代码
- 设置一个 slash command，将 7 天的 Slack、GDrive、Asana 和 GitHub 同步到一个 context dump 中
- 构建 analytics-engineer 风格的 agents，编写 dbt model、审查代码并在 dev 环境中测试更改

See: [Extend Claude with Skills — Claude Code Docs](https://code.claude.com/docs/en/skills)

<a href="https://x.com/bcherny/status/2017742748984742078"><img src="assets/boris-26-2-1/4.png" alt="Create your own skills" width="50%" /></a>

---

## 5/ Claude 自己修复大多数 Bug

以下是团队的做法：

启用 Slack MCP，然后将 Slack bug 线程粘贴到 Claude 中，只需说 "fix"。零上下文切换。

或者，直接说 "Go fix the failing CI tests." 不要微观管理如何做。

将 Claude 指向 docker logs 以排查分布式系统 — 它在这方面出奇地有能力。

<a href="https://x.com/bcherny/status/2017742750473720121"><img src="assets/boris-26-2-1/5.png" alt="Claude fixes most bugs by itself" width="50%" /></a>

---

## 6/ 提升你的 Prompting 水平

a. **Challenge Claude.** 说 "Grill me on these changes and don't make a PR until I pass your test." 让 Claude 成为你的 reviewer。或者说 "Prove to me this works" 让 Claude diff 比较 main 和你的 feature branch 之间的行为。

b. **在 mediocre 的修复之后，**说："Knowing everything you know now, scrap this and implement the elegant solution."

c. **编写详细的 specs** 并在交付工作前减少歧义。你越具体，输出就越好。

<a href="https://x.com/bcherny/status/2017742752566632544"><img src="assets/boris-26-2-1/6.png" alt="Level up your prompting" width="50%" /></a>

---

## 7/ Terminal 与环境设置

团队喜欢 Ghostty！多人喜欢它的同步渲染、24-bit color 和正确的 unicode 支持。

为了更轻松地 juggling Claude，使用 `/statusline` 自定义状态栏，始终显示 context 使用情况和当前 git branch。许多人还会给终端标签页进行颜色编码和命名，有时使用 tmux — 每个 tab 对应一个 task/worktree。

使用语音输入。你说话的速度是打字速度的 3 倍，因此你的 prompts 会变得更加详细。（在 macOS 上按 fn x2）

See: [Terminal Setup Docs](https://code.claude.com/docs/en/termin...)

<a href="https://x.com/bcherny/status/2017742753971769626"><img src="assets/boris-26-2-1/7.png" alt="Terminal and environment setup" width="50%" /></a>

---

## 8/ 使用 Subagents

a. 在任何你希望 Claude 投入更多算力的请求后追加 "use subagents"。

b. 将单个任务 offload 给 subagents，以保持主 agent 的 context window 干净和专注。

c. 通过 hook 将 permission 请求路由到 Opus 4.5 — 让它扫描攻击并自动批准安全的请求。See: [Hooks Docs](https://code.claude.com/docs/en/hooks#...)

<a href="https://x.com/bcherny/status/2017742755737555434"><img src="assets/boris-26-2-1/8.png" alt="Use subagents" width="50%" /></a>

---

## 9/ 使用 Claude 进行 Data 与 Analytics

让 Claude Code 使用 "bq" CLI 实时拉取和分析指标。团队将 BigQuery skill 检入代码库，每个人都直接在 Claude Code 中使用它进行 analytics 查询。个人而言，Boris 已经有 6 个多月没写过一行 SQL 了。

这适用于任何具有 CLI、MCP 或 API 的数据库。

<a href="https://x.com/bcherny/status/2017742757666902374"><img src="assets/boris-26-2-1/9.png" alt="Use Claude for data and analytics" width="50%" /></a>

---

## 10/ 使用 Claude 学习

团队分享的一些使用 Claude Code 学习的技巧：

a. 在 `/config` 中启用 "Explanatory" 或 "Learning" output style，让 Claude 解释其更改背后的 "why"。

b. 让 Claude 生成一个可视化的 HTML 演示来解释不熟悉的代码。它能做出令人惊讶的好 slides！

c. 让 Claude 绘制新 protocol 和 codebase 的 ASCII 图表，帮助你理解它们。

d. 构建一个 spaced-repetition 学习 skill：你解释你的理解，Claude 提出 follow-up 问题来填补空白，并存储结果。

<a href="https://x.com/bcherny/status/2017742759218794768"><img src="assets/boris-26-2-1/10.png" alt="Learning with Claude" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — February 1, 2026](https://x.com/bcherny/status/2017742741636321619)
