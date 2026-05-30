# 来自构建 Claude Code 的经验：我们如何使用 Skills — Thariq

一份关于 Anthropic 内部如何使用 skills 的综合指南，由 Thariq ([@trq212](https://x.com/trq212)) 于 2026 年 3 月 17 日分享。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Skills 已成为 Claude Code 中最常用的扩展点之一。它们灵活、易于创建，且便于分发。但这种灵活性也让人难以判断什么是最佳实践。Thariq 分享了 Anthropic 内部大量使用 skills（已有数百个在活跃使用）的经验教训。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/1.png" alt="Thariq 介绍推文" width="50%" /></a>

---

## 什么是 Skills？

一个常见的误解是 skills 只是"markdown 文件"，但最有趣的部分在于它们是**文件夹**，可以包含脚本、资源、数据等——agent 可以发现、探索和操作这些东西。Skills 还拥有多种配置选项，包括注册动态 hooks。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/2.png" alt="什么是 Skills？" width="50%" /></a>

---

## Skills 的类型

在整理所有 skills 后，团队发现它们可以归为 9 个重复出现的类别。最好的 skills 能清晰地归入其中一类；而较混乱的则横跨多个类别。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/3.png" alt="Skills 类型网格" width="50%" /></a>

---

### 1/ 库与 API 参考

解释如何正确使用库、CLI 或 SDK 的 skills。这些可以针对内部库，也可以针对 Claude Code 有时难以处理的常见库。它们通常包含一个参考代码片段文件夹，以及编写脚本时需避免的常见问题列表。

**示例：** billing-lib, internal-platform-cli, frontend-design

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/4.png" alt="库与 API 参考" width="50%" /></a>

---

### 2/ 产品验证

描述如何测试或验证代码是否正常工作的 skills。这些通常与 Playwright、tmux 等外部工具配对使用。验证 skills 对于确保 Claude 的输出正确性极为有用。值得让工程师花一周时间来打磨你的验证 skills。

**示例：** signup-flow-driver, checkout-verifier, tmux-cli-driver

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/5.png" alt="产品验证" width="50%" /></a>

---

### 3/ 数据获取与分析

连接到数据和监控栈的 skills。这些可能包含用于获取数据的库（含凭证）、特定的仪表盘 ID 等，以及常见工作流程或数据获取方式的说明。

**示例：** funnel-query, cohort-compare, grafana

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/6.png" alt="数据获取与分析" width="50%" /></a>

---

### 4/ 业务流程与团队自动化

将重复性工作流自动化为一条命令的 skills。这些通常是指令较简单的 skills，但可能对其他 skills 或 MCP 有较复杂的依赖。将之前的结果保存到日志文件中，有助于模型保持一致，并能对之前的工作流执行进行反思。

**示例：** standup-post, create-\<ticket-system\>-ticket, weekly-recap

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/7.png" alt="业务流程与团队自动化" width="50%" /></a>

---

### 5/ 代码脚手架与模板

为代码库中特定功能生成框架样板代码的 skills。你可以将这些 skills 与可组合的脚本结合使用。当你的脚手架有纯代码无法完全覆盖的自然语言需求时，它们尤其有用。

**示例：** new-\<framework\>-workflow, new-migration, create-app

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/8.png" alt="代码脚手架与模板" width="50%" /></a>

---

### 6/ 代码质量与审查

在组织内部强制执行代码质量并协助审查代码的 skills。这些可以包含确定性脚本或工具以获得最大稳健性。你可能希望将这些 skills 作为 hooks 的一部分或 GitHub Action 中自动运行。

**示例：** adversarial-review, code-style, testing-practices

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/9.png" alt="代码质量与审查" width="50%" /></a>

---

### 7/ CI/CD 与部署

帮助你在代码库中获取、推送和部署代码的 skills。这些 skills 可能会引用其他 skills 来收集数据。

**示例：** babysit-pr, deploy-\<service\>, cherry-pick-prod

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/10.png" alt="CI/CD 与部署" width="50%" /></a>

---

### 8/ 运行手册

接收一个症状（如 Slack 线程、告警或错误特征），通过多工具调查并生成结构化报告的 skills。

**示例：** \<service\>-debugging, oncall-runner, log-correlator

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/11.png" alt="运行手册" width="50%" /></a>

---

### 9/ 基础设施运维

执行日常维护和运维流程的 skills——其中一些涉及破坏性操作，需要 guardrails 的保护。这些 skills 使工程师更容易在关键操作中遵循最佳实践。

**示例：** \<resource\>-orphans, dependency-management, cost-investigation

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/12.png" alt="基础设施运维" width="50%" /></a>

---

## 编写 Skills 的技巧

编写有效 skills 的 9 个最佳实践，以及分发和衡量的指导。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/13.png" alt="编写 Skills 的技巧网格" width="50%" /></a>

---

### 技巧 1：不要陈述显而易见的内容

Claude Code 对你的代码库了解很多，Claude 对编码也有很多了解，包括许多默认的惯用做法。如果你发布的主要是知识类 skill，请专注于推动 Claude 跳出其常规思维方式的信息。前端设计 skill 就是一个很好的例子——它是通过与客户反复迭代来改进 Claude 的设计品味而构建的，避免了像 Inter 字体和紫色渐变这样的经典模式。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/14.png" alt="不要陈述显而易见的内容" width="50%" /></a>

---

### 技巧 2：构建"注意事项"部分

任何 skill 中信号价值最高的内容就是注意事项（Gotchas）部分。这些部分应该从 Claude 使用你的 skill 时遇到的常见故障点中积累而来。理想情况下，你应该随着时间的推移更新 skill 来捕捉这些注意事项。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/15.png" alt="构建注意事项部分" width="50%" /></a>

---

### 技巧 3：善用文件系统与渐进式披露

一个 skill 是一个文件夹，而不仅仅是一个 markdown 文件。你应该把整个文件系统看作一种上下文工程和渐进式披露的手段。告诉 Claude 你的 skill 中包含哪些文件，它会在适当时机读取它们。最简单的形式是指向其他 markdown 文件——例如，将详细的函数签名和使用示例拆分到 `references/api.md` 中。你可以有引用文件夹、脚本文件夹、示例文件夹等。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/16.png" alt="渐进式披露" width="50%" /></a>

---

### 技巧 4：避免过度约束 Claude

Claude 通常会尽量遵循你的指令，由于 skills 的可复用性很高，你需要小心不要过于具体。给 Claude 它需要的信息，但也要给它根据情况调整的灵活性。与其给出规定性的逐步指令，不如给出目标和约束条件。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/17.png" alt="避免过度约束 Claude" width="50%" /></a>

---

### 技巧 5：考虑好设置流程

有些 skill 可能需要用户提供上下文来进行设置。一个好的模式是将这些设置信息存储在 skill 目录下的 `config.json` 文件中。如果配置未设置，agent 可以询问用户提供信息。你可以指示 Claude 使用 AskUserQuestion 工具进行结构化的多项选择题。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-18/18.png" alt="考虑好设置流程" width="50%" /></a>

---

### 技巧 6：描述字段是为模型服务的

当 Claude Code 启动一个会话时，它会构建一个包含每个可用 skill 及其描述的列表。Claude 扫描这个列表来决策"有没有适合这个请求的 skill？"这意味着描述字段不是一个摘要——它描述的是**何时触发**这个 skill。请为模型编写它。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/19.png" alt="描述 = 触发条件" width="50%" /></a>

---

### 技巧 7：记忆与数据存储

有些 skill 可以通过在其内部存储数据来实现某种形式的记忆。你可以将数据存储在简单的追加型文本日志文件或 JSON 文件中，也可以存储在像 SQLite 数据库这样复杂的数据结构中。存储在 skill 目录中的数据在升级 skill 时可能会被删除，因此请使用 `${CLAUDE_PLUGIN_DATA}` 作为每个 plugin 的稳定数据存储文件夹。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/20.png" alt="记忆与数据存储" width="50%" /></a>

---

### 技巧 8：存储脚本与生成代码

你可以给 Claude 的最强大工具之一就是代码。给 Claude 脚本和库，可以让 Claude 把它的 turns 用在组合上，决定下一步做什么，而不是重新构建样板代码。然后 Claude 可以即时生成脚本来组合这些功能，进行更高级的分析。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/21.png" alt="存储脚本与生成代码" width="50%" /></a>

---

### 技巧 9：按需 Hooks

Skills 可以包含仅在 skill 被调用时才激活的 hooks，并且在会话期间持续有效。将这些用于那些你不希望一直运行但有时非常实用的、更具侵入性的 hooks。

**示例：**
- `/careful` — 通过 Bash 上的 PreToolUse matcher 阻止 rm -rf、DROP TABLE、force-push、kubectl delete
- `/freeze` — 阻止任何不在特定目录中的 Edit/Write

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/22.png" alt="按需 Hooks" width="50%" /></a>

---

## 分发 Skills

与团队分享 skills 的两种方式：
- **检入到你的仓库**（放在 `.claude/skills` 下）——最适合在相对较少的仓库中协作的小团队
- **制作一个 plugin**，并建立一个 Claude Code Plugin 市场，让用户可以上传和安装 plugins

每个检入的 skill 也会为模型的上下文增加一点负担。随着规模扩大，一个内部的 plugin 市场可以让你分发 skills，并让团队决定安装哪些。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/23.png" alt="分发 Skills" width="50%" /></a>

---

## 管理市场

没有一个集中的团队来决定哪些 skills 进入市场。相反，尝试自然地找到最有用的 skills。上传到 GitHub 中的沙箱文件夹，并在 Slack 或其他论坛中让人们知晓。一旦某个 skill 获得了 traction（由 skill 所有者自行判断），他们可以提交 PR 将其移入市场。发布前的筛选很重要，以避免冗余的 skills。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/24.png" alt="管理市场" width="50%" /></a>

---

## 组合 Skills

你可能希望让 skills 之间相互依赖。例如，一个文件上传 skill 负责上传文件，一个 CSV 生成 skill 负责生成 CSV 并上传。这种依赖管理尚未原生集成到市场或 skills 中，但你可以直接按名称引用其他 skills，如果它们已安装，模型会调用它们。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/25.png" alt="组合 Skills" width="50%" /></a>

---

## 衡量 Skills

要了解 skill 的表现，可以使用 PreToolUse hook 在公司内部记录 skill 的使用情况。这样你可以发现哪些 skills 受欢迎，或者哪些技能的触发频率低于预期。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/26.png" alt="衡量 Skills" width="50%" /></a>

---

## 结论

Skills 是为 agents 提供的极其强大且灵活的工具，但目前仍处于早期阶段，我们都在探索如何最好地使用它们。请将本文更像是我们已验证有效的一些实用技巧集合，而非权威指南。理解 skills 的最佳方式是开始使用、尝试，并找到适合你的方法。我们的大多数 skills 最初只有几行描述和一个注意事项，随着 Claude 遇到新的边界情况，人们不断添加内容，它们变得越来越好。

<a href="https://x.com/trq212/status/2033949937936085378"><img src="assets/thariq-26-3-17/27.png" alt="结论" width="50%" /></a>

---

## 来源

- [Thariq (@trq212) 在 X 上 — 2026 年 3 月 17 日](https://x.com/trq212/status/2033949937936085378)
- [Skilljar — Agent Skills 课程](https://code.claude.com/docs/en/skills)
- [Skill 创建器](https://code.claude.com/docs/en/skills)
