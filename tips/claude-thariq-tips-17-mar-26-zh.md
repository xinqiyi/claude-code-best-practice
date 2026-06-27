# 构建 Claude Code 的经验：我们如何使用 Skills — Thariq

一份关于 Anthropic 内部如何使用 skills 的全面指南，由 Thariq（[@trq212](https://x.com/trq212)）于 2026 年 3 月 17 日分享。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Skills 已成为 Claude Code 中使用最多的扩展点之一。它们灵活、易于制作、易于分发。但这种灵活性也使得难以知道最佳做法。Thariq 分享了在 Anthropic 大量使用 skills（数百个活跃使用中）所获得的经验教训。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/1.png" alt="Thariq intro tweet" width="50%" /></a>

---

## 什么是 Skills？

一个常见的误解是 skills "只是 markdown 文件"，但最有趣的部分是它们是**文件夹**，可以包含脚本、asset、数据等——agent 可以发现、探索和操作的东西。Skills 还有多种配置选项，包括注册动态 hooks。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/2.png" alt="What are Skills?" width="50%" /></a>

---

## Skills 的类型

在梳理了所有 skills 之后，团队注意到它们聚集为 9 个反复出现的类别。最好的 skills 清晰地属于其中一类；比较混乱的则跨越多类。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/3.png" alt="Types of Skills grid" width="50%" /></a>

---

### 1/ Library & API Reference

解释如何正确使用 library、CLI 或 SDK 的 skills。这些可以是内部库或 Claude Code 有时会遇到困难的常见库。它们通常包括一个参考代码片段文件夹，以及编写脚本时需要避免的陷阱列表。

**示例：** billing-lib、internal-platform-cli、frontend-design

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/4.png" alt="Library & API Reference" width="50%" /></a>

---

### 2/ Product Verification

描述如何测试或验证代码是否正常工作的 skills。这些通常与外部工具（如 Playwright、tmux 等）配对使用。Verification skills 对于确保 Claude 输出的正确性极其有用。值得让一名工程师花一整周时间专门优化你的 verification skills。

**示例：** signup-flow-driver、checkout-verifier、tmux-cli-driver

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/5.png" alt="Product Verification" width="50%" /></a>

---

### 3/ Data Fetching & Analysis

连接你的数据和监控栈的 skills。这些可能包括获取数据的 library（带有凭证）、特定 dashboard ID 等，以及常见工作流或获取数据方式的说明。

**示例：** funnel-query、cohort-compare、grafana

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/6.png" alt="Data Fetching & Analysis" width="50%" /></a>

---

### 4/ Business Process & Team Automation

将重复的工作流自动化合并为一个 command 的 skills。这些通常是指令较为简单，但可能对其他 skills 或 MCP 有较复杂的依赖。将先前的结果保存在日志文件中可以帮助模型保持一致并反思工作流的先前执行。

**示例：** standup-post、create-\<ticket-system\>-ticket、weekly-recap

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/7.png" alt="Business Process & Team Automation" width="50%" /></a>

---

### 5/ Code Scaffolding & Templates

为代码库中的特定功能生成 framework boilerplate 的 skills。你可以将这些 skills 与可组合的脚本结合使用。当你的 scaffolding 有自然语言需求而无法纯粹由代码覆盖时，它们特别有用。

**示例：** new-\<framework\>-workflow、new-migration、create-app

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/8.png" alt="Code Scaffolding & Templates" width="50%" /></a>

---

### 6/ Code Quality & Review

在组织内部强制执行代码质量并帮助 review 代码的 skills。这些可以包括确定性脚本或工具以达到最大可靠性。你可能希望作为 hooks 的一部分或在 GitHub Action 中自动运行这些 skills。

**示例：** adversarial-review、code-style、testing-practices

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/9.png" alt="Code Quality & Review" width="50%" /></a>

---

### 7/ CI/CD & Deployment

帮助你在代码库中获取、推送和部署代码的 skills。这些 skills 可能引用其他 skills 来收集数据。

**示例：** babysit-pr、deploy-\<service\>、cherry-pick-prod

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/10.png" alt="CI/CD & Deployment" width="50%" /></a>

---

### 8/ Runbooks

接收一个症状（如 Slack 线程、告警或错误签名），引导完成多工具调查并生成结构化报告的 skills。

**示例：** \<service\>-debugging、oncall-runner、log-correlator

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/11.png" alt="Runbooks" width="50%" /></a>

---

### 9/ Infrastructure Operations

执行日常维护和操作程序的 skills——其中一些涉及破坏性操作，受益于 guardrail。这些使工程师更容易在关键操作中遵循最佳实践。

**示例：** \<resource\>-orphans、dependency-management、cost-investigation

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/12.png" alt="Infrastructure Operations" width="50%" /></a>

---

## 制作 Skills 的技巧

9 个编写有效 skills 的最佳实践，外加分发和测量的指导。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/13.png" alt="Tips for Making Skills grid" width="50%" /></a>

---

### 技巧 1：不要说显而易见的事

Claude Code 对你的代码库了解很多，Claude 对编程也了解很多，包括许多默认观点。如果你发布一个主要关于知识的 skill，尽量专注于推动 Claude 跳出其常规思维方式的信息。Frontend design skill 是一个很好的例子——它是通过与客户迭代改进 Claude 的设计品味构建的，避免了经典模式如 Inter 字体和紫色渐变。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/14.png" alt="Don't State the Obvious" width="50%" /></a>

---

### 技巧 2：建立 Gotchas Section

任何 skill 中信号最强的部分就是 Gotchas section。这些 section 应该从 Claude 使用你的 skill 时遇到的常见失败点构建。理想情况下，你将随时间更新你的 skill 来捕获这些陷阱。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/15.png" alt="Build a Gotchas Section" width="50%" /></a>

---

### 技巧 3：使用文件系统和渐进式披露

一个 skill 是一个文件夹，而不仅仅是 markdown 文件。你应该将整个文件系统视为 context engineering 和 progressive disclosure 的一种形式。告诉 Claude 你的 skill 中有哪些文件，它会在适当的时候读取它们。最简单的形式是指向其他 markdown 文件——例如，将详细的函数签名和使用示例拆分到 `references/api.md`。你可以有参考文件夹、脚本、示例等。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/16.png" alt="Progressive Disclosure" width="50%" /></a>

---

### 技巧 4：避免限制 Claude

Claude 通常会尽量遵循你的指令，但由于 skills 可重用性极强，你要注意不要过于具体。给 Claude 它需要的信息，但给它适应情况的灵活性。不要给出规定性的逐步指令，而是给出目标和约束。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/17.png" alt="Avoid Railroading Claude" width="50%" /></a>

---

### 技巧 5：思考 Setup

有些 skills 可能需要通过用户提供的 context 来设置。一个好的模式是将这些设置信息存储在 skill 目录中的 `config.json` 文件里。如果配置未设置，agent 可以询问用户信息。你可以指示 Claude 使用 AskUserQuestion 工具来进行结构化的选择题提问。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/18.png" alt="Think through the Setup" width="50%" /></a>

---

### 技巧 6：Description 字段是为模型准备的

当 Claude Code 启动一个 session 时，它会构建每个可用 skill 及其 description 的列表。这个列表是 Claude 扫描以决定"这个请求有对应的 skill 吗？"的依据。这意味着 description 字段不是摘要——它描述了**何时触发**此 skill。务必为模型编写此字段。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/19.png" alt="Description = Trigger" width="50%" /></a>

---

### 技巧 7：Memory 和存储数据

有些 skills 可以通过在其中存储数据来包含一种 memory 形式。你可以将数据存储在简单的 append-only 文本日志文件或 JSON 文件中，也可以复杂到 SQLite 数据库。存储在 skill 目录中的数据可能在你升级 skill 时被删除，因此使用 `${CLAUDE_PLUGIN_DATA}` 作为每个 plugin 的稳定文件夹来存储数据。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/20.png" alt="Memory & Storing Data" width="50%" /></a>

---

### 技巧 8：存储脚本并生成代码

你能给 Claude 的最强大工具之一是代码。给 Claude 提供脚本和 library，让 Claude 把回合花在组合上，决定下一步做什么，而不是重建 boilerplate。然后 Claude 可以动态生成脚本来组合这个功能，进行更高级的分析。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/21.png" alt="Store Scripts & Generate Code" width="50%" /></a>

---

### 技巧 9：按需 Hooks

Skills 可以包含仅在调用 skill 时才激活并在 session 期间持续有效的 hooks。用那些你不想一直运行但有时极其有用的、更具主观性的 hooks。

**示例：**
- `/careful`——通过 PreToolUse matcher on Bash 阻止 rm -rf、DROP TABLE、force-push、kubectl delete
- `/freeze`——阻止任何不在特定目录中的 Edit/Write

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/22.png" alt="On Demand Hooks" width="50%" /></a>

---

## 分发 Skills

两种与团队共享 skills 的方式：
- **Check into your repo**（在 `.claude/skills` 下）——最适合跨少量仓库工作的小团队
- **Make a plugin** 并拥有一个 Claude Code Plugin marketplace，用户可以在其中上传和安装 plugins

每个被检入的 skill 也会为模型的 context 增加一点点。随着规模扩展，内部 plugin marketplace 允许你分发 skills 并让团队决定安装哪些。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/23.png" alt="Distributing Skills" width="50%" /></a>

---

## 管理 Marketplace

没有集中的团队决定哪些 skills 进入 marketplace。相反，尝试有机地寻找最有用的 skills。上传到 GitHub 中的 sandbox 文件夹，并在 Slack 或其他论坛中指向它。一旦一个 skill 获得了 traction（这由 skill 的所有者决定），他们可以提交 PR 将其移入 marketplace。发布前的策划对于避免冗余 skills 很重要。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/24.png" alt="Managing a Marketplace" width="50%" /></a>

---

## 组合 Skills

你可能希望有互相依赖的 skills。例如，一个上传文件的 file upload skill，以及一个生成 CSV 并上传的 CSV generation skill。这种依赖管理目前尚未原生内置到 marketplace 或 skills 中，但你可以按名称引用其他 skills，如果它们已安装，模型将调用它们。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/25.png" alt="Composing Skills" width="50%" /></a>

---

## 衡量 Skills

要了解一个 skill 的表现，使用 PreToolUse hook 让你在公司内部记录 skill 使用情况。这意味着你可以找到流行的 skills 或与预期相比触发不足的 skills。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/26.png" alt="Measuring Skills" width="50%" /></a>

---

## 结论

Skills 对于 agent 来说是非常强大、灵活的工具，但仍处于早期阶段，我们都在探索如何最好地使用它们。把这份指南更多地看作我们见过有效的有用技巧的集合，而不是一份权威指南。理解 skills 的最佳方式就是开始、实验，看看什么对你有用。我们的大多数 skills 最初只有几行和一个 gotcha，因为人们不断添加 Claude 遇到新边缘情况时发现的内容而变得更好。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/27.png" alt="Conclusion" width="50%" /></a>

---

## Sources

- [Thariq (@trq212) on X — March 17, 2026](https://x.com/trq212/status/2033949937936085378)
- [Skilljar — Agent Skills course](https://code.claude.com/docs/en/skills)
- [Skill Creator](https://code.claude.com/docs/en/skills)
