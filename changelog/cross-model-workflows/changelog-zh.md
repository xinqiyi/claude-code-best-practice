# 跨模型工作流变更日志

**状态图例:**

| 状态 | 含义 |
|--------|---------|
| `COMPLETE (原因)` | 已执行并成功解决 |
| `INVALID (原因)` | 发现结果不正确、不适用或属于有意为之 |
| `ON HOLD (原因)` | 操作已推迟，等待外部依赖或用户决策 |

---

## [2026-05-13 PKT] 跨模型工作流章节创建

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 章节 | 在 README.md 中创建了新的 `## 🔀 CROSS-MODEL WORKFLOWS` 章节，插入在 DEVELOPMENT WORKFLOWS 和 SKILL COLLECTIONS 之间；该章节解释了三种桥接机制（Plugin / MCP / Router），并将现有的跨模型方法论文档作为章节介绍链接 | COMPLETE (新章节已上线) |
| 2 | 高 | 添加 | 以 10k+ stars 门槛为基础，向表格填充了 4 个仓库：musistudio/claude-code-router (34k, Router)、router-for-me/CLIProxyAPI (32k, Router)、openai/codex-plugin-cc (18k, Plugin)、BeehiveInnovations/pal-mcp-server (12k, MCP — 前身为 zen-mcp-server) | COMPLETE (表格已填充) |
| 3 | 中 | 移动 | 从 `DEVELOPMENT WORKFLOWS → Others` 列表中移除了 `Cross-Model (Claude Code + Codex) Workflow` 条目；将其重新安置在新章节内作为方法论介绍行，以便用户编写的双终端流程保持可发现性且不重复 | COMPLETE (已去重) |
| 4 | 低 | 门槛 | 设定了 10k+ stars 的收录门槛（与 AGENT COLLECTIONS 门槛一致）；未来运行中将自动排除低于此门槛的仓库 | COMPLETE (策略已记录在 memory 中) |
| 5 | 低 | 暂缓 | decolua/9_router (9.3k stars — "FREE Claude/GPT/Gemini via 40+ providers") 刚好低于门槛；下次运行时重新评估 | ON HOLD (低于门槛) |
| 6 | 低 | 已排除 | EveryInc/compound-engineering-plugin (17k) 最初被标记为跨模型 — 经核实为纯 Claude 工作流插件（37 个 skills + 51 个 agents 均在 Claude 内部运行）；保留在 DEVELOPMENT WORKFLOWS 表格中 | INVALID (非跨模型) |
| 7 | 低 | 已排除 | 低于 10k 且具有独特角度的仓库已记录以供参考（若超越门槛则重新评估）：1rgs/claude-code-proxy (4k, Router→OpenAI/Gemini via LiteLLM)、jamubc/gemini-mcp-tool (2k, MCP→Gemini context window)、LLM-Red-Team/kimi-cc (2k, Router→Kimi K2)、jarrodwatts/claude-delegator (1k, Plugin→Codex/Gemini delegation) | ON HOLD (低于门槛，候选观察列表) |
