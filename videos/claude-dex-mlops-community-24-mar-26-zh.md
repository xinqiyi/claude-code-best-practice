# 关于 Research-Plan-Implement 我们搞错的一切 — MLOps Community

Dexter Horthy（[@daborhey](https://x.com/daborhey)），HumanLayer 联合创始人，在 MLOps Community 的演讲文字记录，发布于 2026 年 3 月 24 日。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 视频详情

- **演讲者：** Dexter Horthy（HumanLayer 联合创始人）
- **主持人：** Demetrios（MLOps Community）
- **发布日期：** 2026 年 3 月 24 日
- **YouTube：** [在 YouTube 上观看](https://youtu.be/YwZR6tc7qYg)

---

## 核心要点

本演讲分享了 Dex 和他的团队在使用和推广 Research-Plan-Implement (RPI) 方法论时发现的问题和经验教训。

### 我们搞错的事

1. **"不读代码没关系"——错了。** Dex 承认他之前说可以不读代码是正确的，但现在认识到这是错误的。经过 6 个月的实践，"必须读代码"。这是 2026 年——"不再有 slop"。

2. **长计划文件没有杠杆效应。** 1000 行计划大约对应 1000 行代码。审查计划和审查代码的工作量差不多。新的建议：不要读计划，但一定要读代码。

3. **单一大 prompt（85+ 条指令）不可靠。** 前沿 LLM 只能可靠地遵循约 150-200 条指令。如果 prompt 有 85 条指令，加上 CLAUDE.md、system prompt、工具和 MCP——全量遵循工作流的可能性不高。

4. **"Magic words"问题。** 用户必须知道说"work back and forth with me starting with your open questions and outline before writing the plan"才能获得好的计划结果。这是工具的问题，不是用户的问题。

### 解决方案：从 RPI 到 CRISPY

团队将工作流从 3 步（Research → Plan → Implement）拆分为 7 步：

**Questions → Research → Design → Structure → Plan → Implement → PR**

关键改进：

- **更好的 Research：** 向 research agent 隐藏 ticket 内容，使其保持客观（只有事实，没有意见）
- **Smaller prompts：** 拆分大的单体 prompt，每个 prompt 现在少于 40 条指令
- **Design 文档（约 200 行）：** 捕获当前状态、目标状态、要遵循的模式、开放问题——在对齐阶段纠正 agent 的错误
- **Structure outline（约 2 页）：** "C header file"级别的详细程度——签名和类型，足以验证 agent 的思路
- **不要用 prompt 做控制流。** 用 if 语句做控制流，用 LLM 做分类。这对任何基于 LLM 的系统都适用

### 关键教训

- **"Do not outsource the thinking"**（不要外包思考）——Jake from Netflix 的原则
- **Mind your instruction budget**（注意你的指令预算）——更少的指令和更简单的 task 比把所有东西塞进一个 context window 更有效
- **Vertical plans over horizontal plans**（垂直计划而非水平计划）。在完成所有数据库工作后再做所有服务层工作再然后做 API 层会导致 1200 行代码跑不通时完全不知道哪里出了问题
- **前置对齐：** 在写代码之前，利用短文档和 team 进行架构审查，减少 code review 阶段的返工
- **Context window：** 保持 context 在 40% 以下（对新手保持 30%），如果到 60% 就该考虑收尾了

---

## Sources

- [Everything We Got Wrong About Research-Plan-Implement — MLOps Community — YouTube](https://youtu.be/YwZR6tc7qYg)
- [HumanLayer](https://humanlayer.dev)
