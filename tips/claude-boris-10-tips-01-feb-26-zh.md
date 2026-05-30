# 使用 Claude Code 的 10 个技巧 — 来自 Claude Code 团队

由 Claude Code 创建者 Boris Cherny ([@bcherny](https://x.com/bcherny)) 于 2026 年 2 月 1 日分享的团队技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了直接来自 Claude Code 团队的使用技巧。团队使用 Claude 的方式与 Boris 个人的使用方式不同。请记住：使用 Claude Code 没有唯一正确的方式 — 每个人的配置都不一样。你应该不断尝试，找到适合自己的方法！

<a href="https://x.com/bcherny/status/2017742741636321619"><img src="assets/boris-26-2-1/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 并行处理更多任务

同时启动 3–5 个 git worktrees，每个 worktree 独立运行自己的 Claude 会话。这是最大的生产力提升手段，也是团队的首要建议。Boris 个人使用多个 git checkout，但 Claude Code 团队的大多数人更喜欢 worktrees — 这也是 `@amorisscode` 在 Claude Desktop 应用中内置 worktrees 原生支持的原因！

有些人还会给 worktrees 命名并设置 shell 别名（`2a`、`2b`、`2c`），这样一键即可切换。另一些人则有一个专用的"分析"worktree，只用来读取日志和运行 BigQuery。

参见：[Worktrees 文档](https://code.claude.com/docs/en/common...)

<a href="https://x.com/bcherny/status/2017742743125299476"><img src="assets/boris-26-2-1/1.png" alt="并行处理更多任务" width="50%" /></a>

---

## 2/ 每个复杂任务都从计划模式开始

把你的精力投入到计划中，这样 Claude 就能一次性完成实现。

有人让一个 Claude 编写计划，然后启动第二个 Claude 作为高级工程师来审查这个计划。

另一个人说，一旦事情出现偏差，就立即切回计划模式重新规划。不要硬推下去。他们还明确告诉 Claude 进入计划模式进行验证步骤，而不仅仅是构建阶段。

<a href="https://x.com/bcherny/status/2017742745365057733"><img src="assets/boris-26-2-1/2.png" alt="每个复杂任务都从计划模式开始" width="50%" /></a>

---

## 3/ 投资你的 CLAUDE.md

每次纠正之后，加上一句："更新你的 CLAUDE.md，这样你就不会再犯同样的错误。"Claude 为自己编写规则的能力惊人地出色。

不断精简你的 `CLAUDE.md`。持续迭代，直到 Claude 的错误率明显下降。

有一位工程师让 Claude 为每个任务/项目维护一个笔记目录，每次 PR 后更新。然后他们将 `CLAUDE.md` 指向这个目录。

<a href="https://x.com/bcherny/status/2017742747067945390"><img src="assets/boris-26-2-1/3.png" alt="投资你的 CLAUDE.md" width="50%" /></a>

---

## 4/ 创建你自己的 Skills 并提交到 Git

在每个项目中复用。来自团队的建议：

- 如果某件事你一天要做超过一次，就把它变成 skill 或 command
- 创建一个 `/techdebt` 的 slash command，每次会话结束时运行，查找并消灭重复代码
- 设置一个 slash command，将 7 天的 Slack、GDrive、Asana 和 GitHub 数据同步到一个上下文转储中
- 构建分析工程师风格的 agent，编写 dbt 模型、审查代码并在开发环境中测试变更

参见：[使用 Skills 扩展 Claude — Claude Code 文档](https://code.claude.com/docs/en/skills)

<a href="https://x.com/bcherny/status/2017742748984742078"><img src="assets/boris-26-2-1/4.png" alt="创建你自己的 skills" width="50%" /></a>

---

## 5/ Claude 能自行修复大多数 Bug

团队的做法是这样的：

启用 Slack MCP，然后将 Slack 上的 bug 讨论线程粘贴给 Claude，直接说"修复"。零上下文切换。

或者，直接说"去修复失败的 CI 测试。"不要微观管理具体怎么做。

把 Claude 指向 docker 日志来排查分布式系统问题 — 它在这方面出人意料地强大。

<a href="https://x.com/bcherny/status/2017742750473720121"><img src="assets/boris-26-2-1/5.png" alt="Claude 能自行修复大多数 bug" width="50%" /></a>

---

## 6/ 提升你的 Prompting 技巧

a. **挑战 Claude。** 说"严格审查这些变更，直到你通过我的测试才创建 PR。"让 Claude 做你的审查者。或者说"证明这个方案可行"，让 Claude 对比 main 分支和你的功能分支之间的行为差异。

b. **在得到平庸的修复之后，** 说："基于你现在知道的一切，抛弃这个方案，实现一个优雅的解决方案。"

c. **编写详细的规格说明**，在移交工作之前减少歧义。你越具体，输出质量就越高。

<a href="https://x.com/bcherny/status/2017742752566632544"><img src="assets/boris-26-2-1/6.png" alt="提升你的 prompting 技巧" width="50%" /></a>

---

## 7/ 终端与环境设置

团队钟爱 Ghostty！多人喜欢它的同步渲染、24 位色彩和完整的 unicode 支持。

为了更轻松地管理多个 Claude 会话，使用 `/statusline` 自定义你的状态栏，始终显示上下文使用量和当前 git 分支。很多人还会给终端标签着色和命名，有时使用 tmux — 每个标签对应一个任务或 worktree。

使用语音输入。你说话的速度是打字速度的 3 倍，因此你的 prompt 会变得更加详细。（在 macOS 上连按 fn 键两次）

参见：[终端设置文档](https://code.claude.com/docs/en/termin...)

<a href="https://x.com/bcherny/status/2017742753971769626"><img src="assets/boris-26-2-1/7.png" alt="终端与环境设置" width="50%" /></a>

---

## 8/ 使用 Subagents

a. 在任何你希望 Claude 投入更多算力处理的问题后加上"使用 subagents"。

b. 将独立任务卸载给 subagents，以保持主 agent 的上下文窗口干净且专注。

c. 通过 hook 将权限请求路由到 Opus 4.5 — 让它扫描攻击风险并自动批准安全的请求。参见：[Hooks 文档](https://code.claude.com/docs/en/hooks#...)

<a href="https://x.com/bcherny/status/2017742755737555434"><img src="assets/boris-26-2-1/8.png" alt="使用 subagents" width="50%" /></a>

---

## 9/ 使用 Claude 处理数据分析

让 Claude Code 使用 "bq" CLI 即时拉取和分析指标。团队在代码库中维护了一个 BigQuery skill，每个人都在 Claude Code 中直接使用它进行分析查询。Boris 个人已经有 6 个多月没有写过一行 SQL 了。

这适用于任何拥有 CLI、MCP 或 API 的数据库。

<a href="https://x.com/bcherny/status/2017742757666902374"><img src="assets/boris-26-2-1/9.png" alt="使用 Claude 处理数据分析" width="50%" /></a>

---

## 10/ 利用 Claude 学习

来自团队的一些关于使用 Claude Code 学习的建议：

a. 在 `/config` 中启用"解释型"或"学习型"输出风格，让 Claude 解释其变更背后的"为什么"。

b. 让 Claude 生成可视化的 HTML 演示文稿，解释你不熟悉的代码。它制作的幻灯片出奇地好！

c. 让 Claude 绘制新协议和代码库的 ASCII 图表，帮助你理解它们。

d. 构建一个间隔重复学习 skill：你解释你的理解，Claude 提出后续问题来填补空白，并存储结果。

<a href="https://x.com/bcherny/status/2017742759218794768"><img src="assets/boris-26-2-1/10.png" alt="利用 Claude 学习" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 2 月 1 日](https://x.com/bcherny/status/2017742741636321619)
