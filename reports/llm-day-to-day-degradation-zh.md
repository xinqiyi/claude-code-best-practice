# LLM 每日退化：迷思与现实

部署后的 LLM 性能是否会在每日之间发生变化——即使模型权重是冻结的？深入探究已证实的原因、基础设施 bug 和心理因素。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<table width="100%">
<tr>
<td width="50%"><a href="https://x.com/nicksdot/status/2029520949176049704"><img src="assets/llm-degradation.png" alt="Twitter 用户报告 Claude 质量每日退化" width="100%" /></a></td>
<td width="50%"><a href="https://x.com/levelsio/status/2029369159893569680"><img src="assets/llm-degradation-2.png" alt="Twitter 用户报告 Claude 质量每日退化" width="100%" /></a></td>
</tr>
</table>

---

---

# 🔥 Claude Code Ops 4.6 分析。High Reasoning

当 Anthropic 发布像 Opus 4.6 这样的模型时，**模型权重**——数十亿学习到的参数——是冻结的。训练非常昂贵（数百万美元，数周的计算时间）。没有人会在夜间重新训练模型。

但权重只是更大系统中的一个层面。研究揭示了至少 **7 种不同的机制**，即使模型权重被冻结，也能导致真实或感知到的质量变化。

| Question | Answer |
|----------|--------|
| 模型权重在发布后会改变吗？ | **不会**——所有 provider 均已确认 |
| 模型在每天之间的表现能否不同？ | **能**——已证实有 ±8-14% 的方差 |
| 这是故意的"削弱"吗？ | **不是**——没有证据表明存在故意的质量下降 |
| 基础设施 bug 是真实的吗？ | **是**——Anthropic 确认了 3 个 bug，影响高达 16% 的请求 |
| 其中有些是心理作用吗？ | **是**——confirmation bias 和 honeymoon effect 是真实存在的 |
| System prompt / post-training 能改变吗？ | **能**——各 provider 均有文档记录 |
| 用户应该相信自己的感知吗？ | **部分**——真实原因存在，但感知会放大它们 |

---

## 完整的 Inference 栈

模型权重是冻结的，但**其上的九个层面**可以独立影响你的体验：

```
┌──────────────────────────────────────────────┐
│  你的 SESSION CONTEXT                        │  ← 在 session 内退化
│  （积累的错误、长对话）                       │
├──────────────────────────────────────────────┤
│  SYSTEM PROMPT                               │  ← 定期更新
│  （安全规则、行为指令）                       │
├──────────────────────────────────────────────┤
│  POST-TRAINING（RLHF / Fine-tuning）         │  ← 可以静默更新
│  （instruction following、安全 alignment）    │
├──────────────────────────────────────────────┤
│  SAMPLING PARAMETERS                         │  ← 可在服务端调整
│  （temperature、top-p、top-k）               │
├──────────────────────────────────────────────┤
│  SPECULATIVE DECODING                        │  ← 草稿模型质量变化
│  （草稿模型预测 + 验证）                     │
├──────────────────────────────────────────────┤
│  MoE ROUTING / BATCH COMPOSITION             │  ← 已证实 ±8-14% 方差
│  （哪些 expert 为每个请求激活）               │
├──────────────────────────────────────────────┤
│  HARDWARE ROUTING                            │  ← TPU vs GPU vs Trainium
│  （哪个集群为你的请求服务）                    │
├──────────────────────────────────────────────┤
│  QUANTIZATION LEVEL                          │  ← 在负载下可能变化
│  （FP16 vs INT8 vs INT4 精度）               │
├──────────────────────────────────────────────┤
│  COMPILER & RUNTIME                          │  ← XLA bug 被证实真实存在
│  （XLA:TPU、CUDA、硬件特定代码）              │
├──────────────────────────────────────────────┤
│  MODEL WEIGHTS（冻结）                        │  ← 这些不变
│  （数十亿学习到的参数）                        │
└──────────────────────────────────────────────┘
```

关键心智模型：**冻结的权重 ≠ 冻结的行为**。这就像说"相同的引擎 = 相同的驾驶体验"，而忽略了轮胎、路况、燃油质量和驾驶员疲劳。

---

## 已证实的原因：基础设施 Bug

### Anthropic 2025 年 9 月事后分析

2025 年 9 月，Anthropic 发布了详细的事后分析，揭示了**三个独立的基础设施 bug**，在 2025 年 8 月至 9 月期间导致 Claude 质量下降。其官方声明：

> "我们从未因需求、时段或服务器负载而降低模型质量。用户报告的问题仅由基础设施 bug 引起。"

### Bug #1 — Context Window 路由错误

Sonnet 4 请求被意外路由到配置为 1M token context window 的服务器，而不是标准服务器。

- **时间线**：8 月 5 日引入，8 月 29 日在负载均衡更改后恶化
- **峰值影响**：最严重时（8 月 31 日），16% 的 Sonnet 4 请求受到影响
- **用户影响**：约 30% 的 Claude Code 用户至少有一条退化的消息
- **隐蔽细节**：路由是"粘性的"——一旦你到达了一个坏的服务器，后续请求会继续到达那里
- **修复**：9 月 4 日至 18 日（跨平台推出）

### Bug #2 — TPU 输出损坏

TPU 服务器上的配置错误在 token 生成过程中导致错误，为不应频繁出现的 token 分配了高概率。

- **症状**：英文回复中出现泰文或中文字符、明显的代码语法错误
- **影响**：Opus 4.1 和 Opus 4（8 月 25 日至 28 日）、Sonnet 4（8 月 25 日至 9 月 2 日）
- **范围**：仅 Claude API；第三方平台未受影响
- **修复**：9 月 2 日回滚

### Bug #3 — XLA:TPU Compiler 错误编译（最糟糕的）

一个修复精度问题的代码更改意外暴露了 Google XLA:TPU 中的**潜在 compiler bug**。

- **根本原因**：approximate top-k 操作（用于选择最可能的下一个 token）"有时返回完全错误的结果，但仅在特定 batch size 和模型配置下"
- **难以发现的原因**：它的行为取决于之前或之后运行的操作，以及是否启用了调试工具
- **隐藏数月**：2024 年 12 月的一个先前 workaround 意外掩盖了这个更深的 bug
- **影响**：确认影响 Haiku 3.5；怀疑影响 Sonnet 4 和 Opus 3 的子集
- **解决方案**：从 approximate top-k 切换到 exact top-k；接受"轻微的效率影响"，因为"模型质量是不可协商的"

### 为什么检测困难

Anthropic 自己的自动化评估未能捕捉到用户报告的质量下降，"部分原因是 Claude 通常能从孤立错误中很好地恢复。"每个 bug 在不同平台上以不同速率产生不同症状，形成了"不指向任何单一原因的报告混乱组合。"

关键 context：Claude 运行在**三种不同的硬件平台**上（AWS Trainium、NVIDIA GPU、Google TPU），每种都有不同的故障模式、compiler 和精度行为。你的请求在不同日子可能到达不同的硬件。

---

## 已证实的原因：MoE Routing Variance

现代大型模型通常使用 **Mixture-of-Experts (MoE)** 架构，每次输入只激活模型参数的一个子集（"experts"）。一个学习的 router 决定使用哪些 experts。

Scale AI 的研究揭示了一个关键发现：

> "Sparse MoE 和 batched inference 的组合产生了不可预测的结果，因为一个 batch 的组成可以决定你的查询被路由到哪个 expert，而同一 batch 中来自其他用户的查询混合不是确定性的。"

### 各 Provider 测量的每日方差

| Provider | Daily Score Variance |
|----------|--------------------------|
| OpenAI（GPT-4 variants） | ±10–12% |
| Anthropic（Claude variants） | ±8–11% |
| Google（Gemini variants） | ±9–14% |

具体例子：同一模型**某天在 jailbreak 防御上得分 77%，第二天得分 63%**。相同的模型，相同的权重，相同的测试——仅基础设施就造成了 14 个百分点的波动。

这意味着即使零 bug 和零更改，同一模型在不同日子也能产生明显不同的输出质量，纯粹由于请求如何被批处理和路由。当日间噪声为 10-15% 时，A/B 测试无法可靠地检测到 5% 的质量信号。

---

## 已证实的原因：System Prompt 和 Post-Training 更新

### System Prompt 变更

模型权重不变，但包装这些权重的 **system prompt** 可以随时更新。对 Claude system prompt 演化的分析显示了数十次迭代，"hot-fixes"——为修补不希望的行为而添加的简短指令——被定期添加和移除。

Claude 3.7 的 system prompt 包含多个针对常见 LLM "陷阱"的 hot-fix 指令。Claude 4.0 的 system prompt 全部移除了它们，相关行为在 post-training 期间通过 reinforcement learning 解决。

### Post-Training 理论

对无法解释的质量变化最合理的理论：公司可以在不改变基础模型权重的情况下更新 **fine-tuning 和 RLHF**（reinforcement learning from human feedback）。这在技术上使得说"模型没有改变"是真实的，同时通过更新的安全 guardrail 和 instruction-following 调整来改变行为。

---

## 已证实的原因：静默模型切换

OpenAI 多次被记录在案静默更改用户交互的模型：

- 一夜之间移除模型选择器，迫使用户从 GPT-4o 切换到 GPT-5
- 使 GPT-4o 成为一个隐藏的"legacy model"，需要在设置中手动切换，没有应用内通知
- 一个"autoswitcher" bug 将用户路由到错误的模型
- Plus 订阅用户报告模型在未经同意的情况下切换到"受限版本"

Sam Altman 承认推出"比我们希望的更坎坷一些。"Reddit 帖子获得数千点赞，称新模型是"灾难"和"降级"。

这表明模型切换**确实发生**在行业中——有时是有意的（产品决策），有时是意外的（路由 bug）。

---

## 促成因素

### 负载下的 Quantization

为了经济高效地为数百万用户服务，公司可能提供模型的 **quantized** 版本——将精度从 FP16 降低到 INT8 或 INT4。这可以减少 2-4 倍的内存使用并加速 inference，但会引入微妙的质量损失。Provider 是否在负载下动态切换 quantization 级别存在争议，但技术能力确实存在，并在 vLLM 和 TensorRT 等 serving framework 中有详细文档。

### Speculative Decoding

现代 serving 栈使用较小的"草稿"模型预测多个 token，然后让真实模型验证它们。理论上，这保持了相同的输出分布，但在实践中，接受率因领域和 context 而异。开箱即用的草稿模型在某些情况下可能工作良好，但通常难以处理领域特定任务或非常长的 context。

### Context Window 污染

在长时间的编码 session 中，早期的错误会积累在 context 中。模型看到自己的错误，可能会延续它们。这是单个 session 内"Claude 变笨了"的最常见原因——不是模型退化，而是 context contamination。

**实用建议**：当质量感觉不对劲时，使用 `/compact` 或开始新的 session。这是你可以做的最具可操作性的单一事情。

---

## Stanford 研究——以及为什么它很复杂

Stanford 和 UC Berkeley（Chen, Zaharia, Zou）在 2023 年的里程碑研究——"How is ChatGPT's Behavior Changing Over Time?"——经常被引用为 LLM 退化的证据。标题发现：

> GPT-4 在"这个数字是质数吗？逐步思考"上的准确率从 2023 年 3 月到 6 月从 **97.6% 降至 2.4%**。

### 研究证明了什么

- "相同"LLM 服务的行为在短期内**可以显著变化**
- 不同能力可以向相反方向移动（GPT-4 数学变差，GPT-3.5 变好）
- 代码生成质量下降（GPT-4 可执行代码：52% → 10%）
- 该研究创造了术语 **"LLM drift"**

### 方法论批评

- 3 月版本使用 **temperature 0.0**，而 6 月版本使用 **temperature 1.0**——一个增加随机性的根本性混杂变量
- 每个任务只有 **500 次查询**——对于确定的统计声明来说太小
- "数学问题"实际上是一个是/否问题，模型的猜测模式发生了变化，而不是其数学能力
- 变化可能反映了有意的 **post-training 安全更新**，而不是退化

这项研究证明了重要的事情——LLM 行为会随时间变化——但机制可能是有意的更新，而非无意的退化。

---

## 心理学

### Confirmation Bias

一旦有人在 Twitter 上说"Claude 今天很笨"，你就会开始注意到每一个错误。在无人抱怨的日子里，你会忽略同样的错误。社交媒体放大了这种效应。

### Honeymoon Effect

用户对新模型经历了初始的"蜜月期"，然后逐渐发现局限性。模型没有改变——只是期望值上升的速度超过了能力的支撑。

### 任务难度方差

你的任务每天不同。一天困难的问题会感觉是模型变差了，即使它并没有。

### "周末 Claude"迷思

尽管许多用户相信一周中的天数模式，严谨的分析发现**没有一致的证据**表明确实存在按天变化的质量模式。一篇标题为"AI is Dumber on Mondays"的分析最终也毫无发现。

### LLM 的随机性

LLM 是概率性的。相同的 prompt 每次可以产生不同的输出。在运气不好的时候，你可能会连续得到几个差回复——纯粹的随机性，不是退化。

---

## 结论

用户描述的现象是**真实的但归因错误**：

- **正确**：他们的体验在某些日子确实下降了
- **不正确**：模型被故意"削弱"

实际原因包括以下组合：

1. **基础设施 bug** — Anthropic 事后分析已证实（影响高达 16% 的请求）
2. **MoE routing variance** — Scale AI 测量到 ±8-14% 的质量波动，即使在零更改下
3. **System prompt 和 post-training 更新** — 各 provider 均有文档记录
4. **硬件异构性** — TPU vs GPU vs Trainium，每种都有不同的故障模式
5. **Context 污染** — 长时间 session 会降低 session 内的质量
6. **Confirmation bias** — 社交媒体放大感知模式
7. **随机方差** — 相同模型、相同 prompt，每次输出不同

测量问题很严重：±8-14% 的日间方差意味着你无法从噪声中区分真实的 5% 质量变化。这就是"全都是你的心理作用"阵营和"他们削弱了它"阵营都感到自信的原因——信噪比使得仅凭个人经验无法判断。

---

## Sources

- [Anthropic: A Postmortem of Three Recent Issues](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) — 官方事后分析，详述三个基础设施 bug（2025 年 9 月）
- [Anthropic Reveals Three Infrastructure Bugs — InfoQ](https://www.infoq.com/news/2025/10/anthropic-infrastructure-bugs/) — 事后分析的技术分析
- [How is ChatGPT's Behavior Changing Over Time? — Stanford/UC Berkeley](https://arxiv.org/abs/2307.09009) — LLM drift 的里程碑研究（2023）
- [The Truth About ChatGPT's Degrading Capabilities — TechTalks](https://bdtechtalks.com/2023/07/24/chatgpt-capabilities-degrading-study/) — Stanford 研究的方法论批评
- [LLMs Are Getting Dumber and We Have No Idea Why — Ignorance.ai](https://www.ignorance.ai/p/llms-are-getting-dumber-and-we-have) — 感知退化的五种理论
- [When Claude Forgets How to Code — Robert Matsuoka](https://hyperdev.matsuoka.com/p/when-claude-forgets-how-to-code) — Claude 质量波动和基础设施原因分析
- [Smoothing Out LLM Variance — Scale AI](https://scale.com/blog/smoothing-out-llm-variance) — 测量各 provider ±8-14% 的日间方差
- [What We Can Learn from Anthropic's System Prompt Updates — PromptLayer](https://blog.promptlayer.com/what-we-can-learn-from-anthropics-system-prompt-updates/) — System prompt 演化分析
- [Claude's System Prompt Changes Reveal Anthropic's Priorities — Drew Breunig](https://www.dbreunig.com/2025/06/03/comparing-system-prompts-across-claude-versions.html) — System prompt 中的 hot-fix 模式
- [Complaints About Secretly Switching Models — OpenAI Forum](https://community.openai.com/t/complaints-about-secretly-switching-models/1360150) — 记录在案的静默模型切换
- [Speculative Decoding — BentoML LLM Inference Handbook](https://bentoml.com/llm/inference-optimization/speculative-decoding) — 草稿模型如何影响 serving
- [A Visual Guide to Mixture of Experts — Maarten Grootendorst](https://newsletter.maartengrootendorst.com/p/a-visual-guide-to-mixture-of-experts) — MoE 架构和 routing 解释

---

---

# 🔥 Codex 5.3 High Reason and Finding

### 报告范围

本节解释了为什么用户可能经历 Claude 输出质量下降的短暂窗口，而 Codex 5.3 在编码任务上感觉稳定或更强。重点不是永久的模型质量排名。重点是在真实 serving 条件下的短周期生产行为。

报告日期：2026 年 3 月 5 日。

### 观察到的模式

报告的模式是：

1. 模型质量在一段时间内可接受。
2. 质量似乎在几天内下降。
3. 质量恢复到先前的基线。

这种形态通常是 serving 栈或 rollout 模式，而不是永久的基础模型能力变化。永久的能力衰退通常不会在没有显式回滚或修复的情况下恢复得这么快。

### High Reason：为什么 Codex 5.3 在坏窗口中看起来更好

Codex 5.3 在另一个 provider 的质量下降期可能看起来明显更强，有几个可同时发生的技术原因：

1. 产品-目标匹配。Codex 5.3 针对代码生成和 agentic coding 工作流进行了优化，所以即使同等的原始模型强度，由于工具编排、仓库 reasoning 和以代码为中心的 instruction tuning，也可以产生更好的编码结果。
2. Inference 策略差异。Provider 独立调整延迟、reasoning 深度和解码默认值。一个 provider 的更保守策略在同一天可能看起来比另一个 provider 的激进速度优化策略"更聪明"。
3. Serving 路径分离。即使两个 provider 都托管最先进的模型，它们运行不同的 routing 层、compiler/runtime 栈和 rollout 管道。一个栈中的事件不意味着另一个栈的相关质量下降。
4. Rollout 和回滚时机。如果一个 provider 处于中期 rollout 而另一个是稳定的，用户可能看到大幅的临时质量差异，而底层长期模型权重没有变化。
5. Session 级别的污染效应。在长时间的编码对话中，错误积累会放大感知的下降。一个竞争助手可能仅仅因为失败的 session 被重置或其工具循环恢复得更快而感觉更好。

### 详细发现

对于"Claude 感到非常弱大约四天，然后又回来了"这样的报告，最可能的解释是：

1. Provider 端的事件、路由问题、解码/runtime bug 或 rollout regression 影响了部分请求。
2. 该问题持续足够长以在实际工作流中被反复注意到。
3. 该问题被修复或回滚。
4. 感知质量迅速恢复。

在同一时期，Codex 5.3 可能感觉到明显更好，因为它不共享相同的事件路径，并且编码任务优化放大了实际结果的差距。

### 此模式的假设排名

| Hypothesis | Likelihood | Rationale |
|------------|------------|-----------|
| Provider 事件加回滚 | 高 | 最好匹配多日下降后快速恢复 |
| Serving 配置更改（sampling/latency/reasoning budget） | 高 | 无模型重新训练的行为突然变化的常见来源 |
| 静默的 alias 或 snapshot 变更 | 中高 | 可以在没有可见用户操作的情况下改变行为 |
| 仅 prompt drift 和 context contamination | 中 | 可以降低 session 质量，但不太可能单独解释广泛的多日报告 |
| 永久的基础模型退化 | 低 | 与快速回到先前质量不一致 |

### 什么可以确认或推翻此发现

要将此从高置信度推断转变为确凿证据，需要收集跨天的相同任务集的请求级遥测数据：

1. 请求时的精确模型标识符和 snapshot/alias。
2. Provider 暴露的任何 backend fingerprint 或发布标记。
3. 解码参数（temperature、top_p、top_k、max tokens）。
4. 延迟、超时和错误率追踪。
5. 在固定编码 benchmark prompt 集上的结构化质量分数。
6. 失败点的 session 长度和 token-context 深度。

如果质量下降与事件窗口、配置更改或 backend fingerprint 变化相关，事件/配置假设得到确认。如果不存在这样的变化，并且质量下降仅在长时间 session 中出现，context contamination 成为主要解释。

### 实用工程指导

在生产中减少日间方差：

1. 在可用时固定模型 snapshot，而不是使用浮动的 alias。
2. 存储请求元数据（模型 ID、参数、延迟、错误、响应质量标签）。
3. 为编码任务运行固定的每日 canary 套件，并在 regression 时告警。
4. 在多次失败的 turn 后重置或 compact 长时间运行的 session。
5. 为事件窗口保留 fallback provider/model 路径。
6. 在内部仪表板中将"模型质量"与"serving 可靠性"分离。

### 最终结论

Codex 5.3 在短暂的 Claude 质量下降窗口中看起来更好，是现代 LLM 运营中技术上合理且可预期的结果。最强的解释不是永久模型崩溃。最强的解释是一个 provider 的临时 serving 路径退化，加上另一个 provider 在同一时期的编码特定优化和稳定运营。
