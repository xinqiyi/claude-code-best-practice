# Cross-Model Workflows Changelog

**Status Legend：**

| Status | Meaning |
|--------|---------|
| `COMPLETE (reason)` | 已执行操作并成功解决 |
| `INVALID (reason)` | 发现不正确、不适用或为有意为之 |
| `ON HOLD (reason)` | 操作延期，等待外部依赖或用户决策 |

---

## [2026-05-13 PKT] Cross-Model Workflows Section 创建

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | Section | 在 README.md 中创建了新的 `## 🔀 CROSS-MODEL WORKFLOWS` section，插入在 DEVELOPMENT WORKFLOWS 和 SKILL COLLECTIONS 之间；section 解释了三种 bridge 机制（Plugin / MCP / Router），并将现有的 cross-model 方法论文档链接为 section 介绍 | COMPLETE (new section live) |
| 2 | HIGH | Add | 以 10k+ stars 阈值为表格种子添加了 4 个仓库：musistudio/claude-code-router（34k，Router）、router-for-me/CLIProxyAPI（32k，Router）、openai/codex-plugin-cc（18k，Plugin）、BeehiveInnovations/pal-mcp-server（12k，MCP——前身为 zen-mcp-server） | COMPLETE (table seeded) |
| 3 | MEDIUM | Move | 从 `DEVELOPMENT WORKFLOWS → Others` 列表中移除 `Cross-Model (Claude Code + Codex) Workflow`；重新安置为新的 section 内的方法论介绍行，使用户编写的双终端流程保持可发现性而不重复 | COMPLETE (deduplicated) |
| 4 | LOW | Threshold | 建立了 10k+ stars 最低纳入标准（与 AGENT COLLECTIONS 阈值一致）；未来运行中自动拒绝低于阈值的仓库 | COMPLETE (policy recorded in memory) |
| 5 | LOW | On Hold | decolua/9_router（9.3k stars — "通过 40+ providers 免费使用 Claude/GPT/Gemini"）刚好低于阈值；下次运行时重新评估 | ON HOLD (below threshold) |
| 6 | LOW | Excluded | EveryInc/compound-engineering-plugin（17k）最初被标记为 cross-model——经验证为 Claude-only workflow plugin（37 skills + 51 agents 全部运行在 Claude 内部）；保留在 DEVELOPMENT WORKFLOWS 表中 | INVALID (not cross-model) |
| 7 | LOW | Excluded | 记录了 sub-10k 仓库的独特角度以供参考（如果超过阈值则重新评估）：1rgs/claude-code-proxy（4k，Router→OpenAI/Gemini via LiteLLM）、jamubc/gemini-mcp-tool（2k，MCP→Gemini context window）、LLM-Red-Team/kimi-cc（2k，Router→Kimi K2）、jarrodwatts/claude-delegator（1k，Plugin→Codex/Gemini delegation） | ON HOLD (below threshold, candidate watchlist) |
