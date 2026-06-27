# 为什么 Harness 很重要

为什么 Claude Code 的功能不是"只是被伪装起来的 prompt"——以及为什么 harness 是将玩具级输出与生产级工程工作区分开来的关键。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

在经验丰富的 Claude Code 用户中，一种常见的简化论是：*"skills、commands、subagents、hooks——它们最终都会变成发给模型的 prompt，所以一个强大的 prompt 就等价于使用这些功能。"*

在模型最终 inference 调用的层面，这在技术上是正确的。模型只看到 token。

在其他每个层面——即真正发生软件工程的地方——**这种简化是站不住脚的。** harness 不是一个 prompt 传递系统。它是一个 **prompt 构建系统、确定性执行系统和 context 架构系统**——这些能力无法通过更强的措辞来替代。

本报告解释了这种简化在哪些地方是对的，在哪些地方是错的，以及为什么混淆"模型看到什么"和"系统做什么"会引导实践者远离那些真正赋予 Claude Code 杠杆作用的功能。

---

## 听起来对的简化论

对于一个**单次原子任务**——"给我写一个递归斐波那契函数"——harness 对输出质量没有贡献。将相同的 token 交给相同的模型，你会得到相同的输出分布，无论它们是通过 skill、command 还是原始 prompt 到达的。

在这个狭窄的范围内，简化论成立：

> 输出质量 ≈ Prompt 质量

这是 Claude Code 相对于普通聊天机器人价值不大的范围。也是简化论隐含假设的范围——恰恰是真实工程工作不在的范围。

---

## 简化论在哪里崩溃

harness 的十个架构能力运行在 prompt 无法触及的层面。

| # | Capability | What it does | Why a prompt can't replicate |
|---|------------|--------------|-------------------------------|
| 1 | **Context 隔离** | Subagent 在独立的 context window 中运行 | 一个 prompt 填充一个 window。N 个并行 subagent 提供约 N 倍的有效 context。 |
| 2 | **Harness 强制的工具限制** | `allowed-tools` / `disallowedTools` 在模型使用工具之前阻止它们 | Prompt 指令是建议性的，模型可以忽略。Deny 规则无法被忽略。 |
| 3 | **延迟加载的 rules 和 memory** | `paths:` frontmatter 和后代 `CLAUDE.md` 文件仅在 Claude 接触匹配路径时加载 | Prompt 是静态的——它不能根据运行时正在读取的文件有条件地加载。 |
| 4 | **Hooks：确定性代码执行** | Shell 命令在生命周期事件（PreToolUse、PostToolUse、Stop 等）时运行，并可以**阻止**工具调用 | Prompt 无法拦截自己的工具调用。即使模型不"想要"它们，Hooks 也会执行。 |
| 5 | **模型路由** | `model: haiku` 或 `model: opus` 将调用路由到不同的模型端点 | Prompt 中的任何 token 都不能改变回答问题的模型。 |
| 6 | **并行性** | 多个 subagent 并发执行 | Prompt 是顺序的。harness 调度和收集来自并行进程的结果。 |
| 7 | **跨 session 持久性** | Memory 系统和 settings 层级结构在对话之间持久存在 | Prompt 在 session 结束时消亡。 |
| 8 | **模块化 system prompt** | CLI 根据激活的功能有条件地加载 110+ 个 system prompt 片段 | 用户无法手写或替换 CLI 内部的 prompt 片段。 |
| 9 | **Skill 预加载** | `skills:` 字段将 skill 的完整内容注入 subagent 的起始 context | 用户无法预先填充另一个 agent 的 context——只有 harness 加载器可以。 |
| 10 | **权限分类** | `auto` 权限模式使用后台分类器预先批准或阻止工具调用 | Prompt 无法为自身添加执行前的安全层。 |

每一行都是一个维度，在这些维度上，"更强的措辞"绝对不是替代品。

---

## "Prompt"一词的两种用法

这种简化论依赖于概念混淆。*Prompt* 这个词被用来表示两种非常不同的东西：

| Meaning | Who controls it | Size |
|---------|-----------------|------|
| (a) 用户输入的内容 | 用户 | ~6–60 token |
| (b) 模型在 inference 时看到的内容 | Harness | ~5,000–50,000+ token |

在聊天机器人中，(a) 和 (b) 是同一个东西。
在 Claude Code 中，它们截然不同。

Harness 的职责恰恰是让 (b) 比 (a) 丰富得多：

```
用户输入："write a recursive flatten function"       ← (a) ~6 token

模型在 inference 时实际看到的：                      ← (b) ~15,000 token
  ├── CLAUDE.md（项目约定）
  ├── 匹配的 .claude/rules/*.md（通过 paths: frontmatter 加载）
  ├── 模块化 system prompt 片段
  ├── 工具定义
  ├── 环境 context（cwd、git status、平台）
  ├── 之前的对话历史
  ├── 模型通过 Read/Grep 工具读取的文件
  └── 用户的 6 token 请求
```

**输出质量是 (b) 的函数，而不是 (a) 的函数。** Harness 构建了 (b)。"仅靠强大的 prompt"无法复制 (b)，因为它的大部分不是用户写的。

---

## 即使对输出质量，Harness 也在发挥作用

考虑相同的 prompt——"write a recursive flatten function"——在三种环境中：

| Environment | 模型看到的内容 | 典型结果 |
|-------------|---------------------|----------------|
| 聊天机器人，无工具 | 这句话 | 教科书式的递归，通用风格 |
| Claude Code，不读取文件 | 这句话 + CLAUDE.md | 匹配声明的项目约定 |
| Claude Code，agentic loop | 这句话 + CLAUDE.md + 读取相邻文件 + 运行测试 | 匹配实际代码库模式，通过测试，处理现有代码处理的边缘情况 |

相同的模型。相同的用户 prompt。**三种不同的输出质量。** 区别在于 harness——具体来说，是它组装的有效 context 和它启用的迭代循环。

对于非平凡任务，输出质量是以下因素的函数：

```
输出质量 = f(有效 context, 模型能力, 迭代循环)
```

用户控制着*有效 context* 的一小部分（他们输入的 prompt）。Harness 控制其余部分——以及整个迭代循环。

---

## 简化论说对的部分（以及说错的部分）

| Claim | Verdict |
|-------|---------|
| "在 inference 时，模型只看到 token。" | ✅ 正确 |
| "Skills、commands 和 subagent prompt 都为某个 context 贡献 token。" | ✅ 正确 |
| "对于真空中的原子任务，prompt 质量主导输出质量。" | ✅ 正确 |
| "因此一个强大的 prompt 等价于使用功能。" | ❌ 错误 |
| "因此 harness 对输出质量不重要。" | ❌ 在实际工程任务上是错误的 |

前三个陈述是准确的观察。跳跃到第四个是推理失败的地方：它混淆了模型和包装它的系统，并混淆了原子任务和真实工程工作。

---

## 正确的心智模型

> **Prompt 控制模型被要求做什么。**
> **Harness 控制系统在模型无法触及的层面做什么**——在 token 到达之前、在 token 产生之后、跨 session、跨 context、跨进程。

功能不是带有额外步骤的 prompt。它们是 **harness 级别的原语**——确定性执行、context 架构和基础设施路由——运行在模型没有发言权的层面。

一个有用的类比：

| Layer | 聊天机器人 | Claude Code |
|-------|---------|-------------|
| 食谱 | 用户的消息 | 用户的消息 + harness 组装的 context |
| 厨房 | 无——只有一个学生 | 工具、hooks、memory、并行工作线程、生命周期事件 |

你可以写出世界上最好的食谱。没有厨房，你无法大规模烹饪。

---

## 对实践者的启示

1. **对于原子问题，prompt 质量几乎就是一切。** Harness 是无关的。如果你只需要这些，用聊天机器人。
2. **对于真实代码库工作，harness 在做无声的重活。** 最终 inference 时的有效 prompt 大部分是由 harness 构建的，而不是用户输入的。
3. **将功能用于 prompt 绝对无法做到的事情：** 确定性（hooks）、隔离（subagents）、延迟加载（带有 `paths:` 的 rules）、持久性（memory）、路由（每个 agent 的 `model:`）和并行性。
4. **强大的 prompt 是必要的但不充分。** 功能为你提供 prompt 无法提供的确定性、隔离和组合能力。两者是互补的，不是替代品。

---

## Sources

- [Agents vs Commands vs Skills](claude-agent-command-skill.md) — 展示每个功能的 context 隔离、模型覆盖和工具限制
- [Claude Agent SDK vs CLI System Prompts](claude-agent-sdk-vs-cli-system-prompts.md) — 记录了 110+ 个模块化 system prompt 片段
- [Claude Agent Memory](claude-agent-memory.md) — 通过 `memory:` scope 的跨 session 持久性
- [Claude Memory Best Practice](../best-practice/claude-memory.md) — 延迟加载的后代 `CLAUDE.md` 文件
- [Claude Subagents Best Practice](../best-practice/claude-subagents.md) — harness 强制能力的 frontmatter 参考
- [Claude Settings Best Practice](../best-practice/claude-settings.md) — 权限规则评估和 `auto` 模式分类器
- [Orchestration Workflow](../orchestration-workflow/orchestration-workflow.md) — 证明简化论失败的具体演示
