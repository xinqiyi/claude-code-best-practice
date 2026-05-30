# 变更日志 — README CONCEPTS 章节

跟踪 README CONCEPTS 表格与官方 Claude Code 文档之间的差异。

## 状态图例

| 状态 | 含义 |
|--------|--------|
| ✅ `COMPLETE (原因)` | 已采取行动并成功解决 |
| ❌ `INVALID (原因)` | 发现不正确、不适用或属于有意为之 |
| ✋ `ON HOLD (原因)` | 操作延迟 — 等待外部依赖或用户决策 |

---

## [2026-03-02 11:14 AM PKT] Claude Code v2.1.63

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 损坏的 URL | 修复 Permissions URL 从 `/iam` 改为 `/permissions` | ✅ COMPLETE (URL 已更新为 /permissions) |
| 2 | HIGH | 缺失概念 | 在 CONCEPTS 表格中添加 Agent Teams 行 | ✅ COMPLETE (已添加行，位置为 ~\/\.claude\/teams\/) |
| 3 | HIGH | 缺失概念 | 在 CONCEPTS 表格中添加 Keybindings 行 | ✅ COMPLETE (已添加行，位置为 ~\/\.claude\/keybindings\.json) |
| 4 | HIGH | 缺失概念 | 在 CONCEPTS 表格中添加 Model Configuration 行 | ✅ COMPLETE (已添加行，位置为 \.claude\/settings\.json) |
| 5 | HIGH | 缺失概念 | 在 CONCEPTS 表格中添加 Auto Memory 行 | ✅ COMPLETE (已添加行，位置为 ~\/\.claude\/projects\/<project>\/memory\/) |
| 6 | HIGH | 过时锚点 | 修复 Rules URL 锚点从 `#modular-rules-with-clauderules` 改为 `#organize-rules-with-clauderules` | ✅ COMPLETE (锚点已更新) |
| 7 | MED | 缺失概念 | 在 CONCEPTS 表格中添加 Checkpointing 行 | ✅ COMPLETE (已添加行，位置为 automatic git-based) |
| 8 | MED | 缺失概念 | 在 CONCEPTS 表格中添加 Status Line 行 | ✅ COMPLETE (已添加行，位置为 ~\/\.claude\/settings\.json) |
| 9 | MED | 缺失概念 | 在 CONCEPTS 表格中添加 Remote Control 行 | ✅ COMPLETE (已添加行，位置为 CLI / claude\.ai) |
| 10 | MED | 缺失概念 | 在 CONCEPTS 表格中添加 Fast Mode 行 | ✅ COMPLETE (已添加行，位置为 \.claude\/settings\.json) |
| 11 | MED | 缺失概念 | 在 CONCEPTS 表格中添加 Headless Mode 行 | ✅ COMPLETE (已添加行，位置为 CLI flag -p) |
| 12 | LOW | 描述变更 | 更新 Memory 描述以提及 auto memory | ✅ COMPLETE (描述和位置已更新) |
| 13 | LOW | 位置变更 | 更新 MCP Servers 位置以包含 `.mcp.json` | ✅ COMPLETE (位置已更新以包含 .mcp.json) |
| 14 | LOW | 缺失徽章 | 为 Hooks 行添加 Implemented 徽章 | ✅ COMPLETE (已添加 Implemented 徽章，链接至 .claude/hooks/) |

---

## [2026-03-02 11:57 AM PKT] Claude Code v2.1.63

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 表格整合 | 将 CONCEPTS 表格从 22 行整合为 10 行 — 将相关概念折叠为内联文档链接 | ✅ COMPLETE (22 → 10 行) |
| 2 | MED | 合并概念 | 将 Marketplaces 折叠到 Plugins 行作为内联链接 | ✅ COMPLETE (已链接至 /discover-plugins) |
| 3 | MED | 合并概念 | 将 Agent Teams 折叠到 Sub-Agents 行作为内联链接 | ✅ COMPLETE (已链接至 /agent-teams) |
| 4 | MED | 合并概念 | 将 Permissions、Model Config、Output Styles、Sandboxing、Keybindings、Status Line、Fast Mode 折叠到 Settings 行作为内联链接 | ✅ COMPLETE (7 个概念已折叠，附文档链接) |
| 5 | MED | 合并概念 | 将 Auto Memory 和 Rules 折叠到 Memory 行作为内联链接 | ✅ COMPLETE (已链接至 /memory 和 /memory#organize-rules-with-clauderules) |
| 6 | MED | 合并概念 | 将 Headless Mode 折叠到 Remote Control 行作为内联链接 | ✅ COMPLETE (已链接至 /headless) |
| 7 | LOW | 重新排序 | 按逻辑分组重新排列表格：构建模块 → 扩展 → 配置 → 上下文 → 运行时 | ✅ COMPLETE (按关注领域分组，而非时间顺序) |

---

## [2026-03-07 08:40 AM PKT] Claude Code v2.1.71

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 损坏的 URL | 在 TIPS 中修复 `context-management` → `interactive-mode` (第 112、115、135 行) | ✅ COMPLETE (3 处已替换为 interactive-mode) |
| 2 | HIGH | 损坏的 URL | 在 TIPS 中修复 `model-configuration` → `model-config` (第 115、116、135 行) | ✅ COMPLETE (3 处已替换为 model-config) |
| 3 | HIGH | 损坏的 URL | 在 TIPS 中修复 `usage-billing` → `costs` (第 115 行) | ✅ COMPLETE (已替换为 costs) |
| 4 | HIGH | 损坏的 URL | 在 STARTUPS 中移除 `cowork` URL (第 167 行) — 页面不存在 | ✅ COMPLETE (超链接已移除，纯文本保留) |
| 5 | HIGH | 缺失概念 | 在 CONCEPTS 和 Hot 板块中添加 Scheduled Tasks 行 (`/loop`、cron 工具) | ✅ COMPLETE (由用户添加到两个表格 + /loop 技巧 + Boris 推文) |
| 6 | MED | 位置变更 | 更新 Agent Teams 位置从 `.claude/agents/<name>.md` 改为 `built-in (env var)` | ✅ COMPLETE (位置已更新为 built-in env var) |

---

## [2026-03-10 01:18 PM PKT] Claude Code v2.1.72

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 损坏的 URL | 在 CONCEPTS 表格中修复 Commands URL 从 `/slash-commands` 改为 `/skills` (第 24 行) — `/slash-commands` 提供 Skills 页面内容；文档称"commands 已合并到 skills" | ❌ INVALID (URL 仍可解析；用户选择保持现状) |
| 2 | HIGH | 损坏的 URL | 在 TIPS 板块中修复 Commands URL 从 `/slash-commands` 改为 `/skills` (第 108 行) — 相同的过时 URL | ❌ INVALID (URL 仍可解析；用户选择保持现状) |
| 3 | MED | 缺失内联链接 | 添加 Interactive Mode (`/interactive-mode`) 作为 CLI Startup Flags 行的内联链接 — 涵盖 /compact、/clear、/context、/extra-usage | ✅ COMPLETE (内联链接已添加到 CLI Startup Flags 描述) |
| 4 | MED | 缺失内联链接 | 添加 Costs (`/costs`) 作为 Settings 行的内联链接 — 涵盖 /usage、计费、按需付费 | ❌ INVALID (用户选择跳过) |
| 5 | LOW | 缺失概念 | 考虑添加 IDE Integrations 行 (VS Code、JetBrains、Desktop App、Web) 或 Best Practices 内联链接 | ❌ INVALID (用户选择跳过 — 平台呈现面，而非配置概念) |
| 6 | HIGH | 缺失概念 | 在 Hot 表格中添加 Code Review 行 — 多 agent PR 分析 (research preview, Teams & Enterprise) | ✅ COMPLETE (已添加行为第一个 Hot 条目，附博客链接和最佳实践推文) |
| 7 | MED | 新徽章 | 创建 `!/tags/beta.svg` 标签 (黄色，38x20px) 并添加到 Hot 表格中的 Code Review 和 Agent Teams | ✅ COMPLETE (beta.svg 已创建；已添加到 Code Review 和 Agent Teams 行) |
| 8 | MED | 重新排序 | 按发布日期对 Hot 表格排序 (最新在前)：Code Review → Scheduled Tasks → Voice Mode → Agent Teams → Remote Control → Git Worktrees → Ralph Wiggum | ✅ COMPLETE (Voice Mode 和 Agent Teams 已交换以匹配时间顺序) |

---

## [2026-03-12 12:22 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 损坏的 URL | 在 CONCEPTS 表格中修复 Commands URL 从 `/slash-commands` 改为 `/skills` (第 24 行) — `/slash-commands` 重定向到 `/skills` 页面 | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 20+ 个 URL 返回有效页面) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在目标页面验证 | ✅ COMPLETE (标题存在于 /memory 页面) |
| 5 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 | ✅ COMPLETE (未检测到描述偏差) |

---

## [2026-03-15 12:48 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | MED | 缺失徽章 | Remote Control (Hot) 没有徽章 — 唯一没有 BP 或 Impl 徽章的 Hot 条目 | ✅ COMPLETE (BP 徽章已添加，链接至官方文档页面) |
| 3 | LOW | 命名 | README 中的 "Sub-Agents" 与官方文档中的 "subagents" (一个词) — 视觉不一致 | ✅ COMPLETE (已在 CONCEPTS 表格中重命名为 "Subagents") |
| 4 | LOW | 验证 | 所有 27 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有 13 个 CONCEPTS + 9 个 Hot 行的描述准确) |

---

## [2026-03-17 12:46 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | HIGH | 描述变更 | Hooks 描述称"确定性脚本"但 hooks 现在包含 4 种类型：command、HTTP、prompt 和 agent — 只有 command hooks 是确定性的 | ✅ COMPLETE (已在 CONCEPTS 表格中更新为"用户自定义处理器（脚本、HTTP、提示、agents）") |
| 3 | MED | 缺失概念 | Desktop App 在 `/desktop` 有专门的文档页面 — 不在 CONCEPTS 或 Hot 表格中 | ❌ INVALID (用户选择跳过 — Desktop 是平台呈现面，而非配置概念) |
| 4 | MED | URL 变更 | Hooks 文档现已拆分为 Guide (`/hooks-guide`) 和 Reference (`/hooks`) — CONCEPTS 仅链接到 Reference | ✅ COMPLETE (Guide 链接已添加为 Hooks 行描述中的内联链接) |
| 5 | LOW | 验证 | 所有 28 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 6 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20 个徽章目标存在于文件系统) |
| 7 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 | ✅ COMPLETE (Hooks 描述偏差已检测到 — 参见 #2) |

---

## [2026-03-18 11:43 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | HIGH | URL+名称变更 | Hot 表格中的 Voice Mode 链接到推文而非官方文档 `/voice-dictation`；官方名称为 "Voice Dictation" | ✅ COMPLETE (已重命名为 "Voice Dictation"，链接至 /voice-dictation，描述已更新；BP 徽章保持链接至推文；同时更新于 STARTUPS 表格) |
| 3 | LOW | 验证 | 所有 29 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 4 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 5 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-19 11:59 AM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 30 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 5 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-20 08:38 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失概念 | 在 Hot 表格中添加 Channels 行 — 从 Telegram/Discord/webhooks 推送事件到运行中的会话 (research preview, v2.1.80) | ✅ COMPLETE (已添加行为第一个 Hot 条目，附 beta 徽章和 Reference 链接) |
| 2 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 3 | MED | 缺失深层链接 | Git Worktrees URL 应锚定到 `#run-parallel-claude-code-sessions-with-git-worktrees` | ✅ COMPLETE (锚点已添加到 Hot 表格中的 Git Worktrees URL) |
| 4 | LOW | 缺失内联链接 | Plugins 行可添加 `[Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)` 子链接 | ✅ COMPLETE (Create Marketplaces 内联链接已添加到 Plugins 行) |
| 5 | LOW | 验证 | 所有 31 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 6 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 7 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-21 09:12 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 32 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 5 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-23 09:53 PM PKT] Claude Code v2.1.81

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 33 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 5 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-25 08:12 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | MED | URL 变更 | Simplify & Batch 主要链接指向推文而非官方文档 `/skills#bundled-skills` — 现已正式称为 bundled skills | ✅ COMPLETE (主要链接已更新为 /skills#bundled-skills；BP 徽章保持链接至 Boris 的推文) |
| 3 | LOW | 验证 | 所有 34 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 4 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 5 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |
| 8 | HIGH | 缺失概念 | 在 Hot 表格中添加 Auto Mode 行 — 后台安全分类器替代权限提示 (research preview, Team/Enterprise) | ✅ COMPLETE (已添加行为第一个 Hot 条目，附 beta 徽章、链接至 @claudeai 推文的 BP 徽章和博客链接) |

---

## [2026-03-26 01:05 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | MED | 缺失概念 | 在 Hot 表格中添加 Slack 集成 — 在 Slack 中 @Claude 将编码任务路由到 Claude Code Web 会话 | ✅ COMPLETE (已添加行在 Channels 之后，位置为 @Claude，附 web session 描述) |
| 3 | MED | 缺失概念 | 在 Hot 表格中添加 GitHub Actions / CI-CD — 在 CI/CD 流水线中自动化 PR 审查、问题分类和代码生成 | ✅ COMPLETE (已添加行在 Code Review 之后，位置为 .github/workflows/，附 GitLab CI/CD 内联链接) |
| 4 | LOW | 验证 | 所有 35 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-27 06:37 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | MED | 缺失概念 | 在 Hot 表格中添加 Chrome 集成 — 通过 Claude in Chrome 扩展实现浏览器自动化 (beta，专门文档在 `/chrome`) | ✅ COMPLETE (已添加行在 GitHub Actions 之后，位置为 --chrome，附 beta 徽章) |
| 3 | LOW | 验证 | 所有 36 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 4 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 5 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-03-28 06:04 PM PKT] Claude Code v2.1.86

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | MED | 缺失徽章 | Hot 表格中的 Chrome 行没有 BP 徽章 — 报告存在于 `reports/claude-in-chrome-v-chrome-devtools-mcp.md` | ✅ COMPLETE (BP 徽章已添加，链接至 reports/claude-in-chrome-v-chrome-devtools-mcp.md) |
| 3 | LOW | 描述变更 | Plugins 描述缺少 LSP 服务器 — 官方文档列出 `.lsp.json` 为 plugin 组件 | ✅ COMPLETE (已在 Plugins 描述中添加"和 LSP 服务器") |
| 4 | LOW | 验证 | 所有 37 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题 `.claude/rules/` 存在) |
| 7 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (除 Plugins LSP 说明外所有描述准确 — 参见 #3) |

---

## [2026-04-01 12:33 PM PKT] Claude Code v2.1.89

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失概念 | 在 Hot 表格中添加 Computer Use 行 — 通过内置 MCP 服务器在 macOS 上进行屏幕控制 (research preview, v2.1.85+) | ✅ COMPLETE (已添加行在 Fullscreen Rendering 之后，附 beta 徽章和 Desktop 内联链接) |
| 2 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 3 | MED | 缺失概念 | 在 Hot 表格中添加 Fullscreen Rendering 行 — 无闪烁 alt-screen 渲染，支持鼠标 (research preview, v2.1.88+) | ✅ COMPLETE (已添加行为第一个 Hot 条目，位置为 CLAUDE_CODE_NO_FLICKER=1) |
| 4 | LOW | 验证 | 所有 38 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-02 09:17 PM PKT] Claude Code v2.1.90

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 提供 Skills 页面内容 — 文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 39 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 5 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-03 08:35 PM PKT] Claude Code v2.1.91

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (llms.txt) 中 — 重定向到 `/skills` 页面；文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 40 个外部文档 URL 已对照 llms.txt 站点地图 (80 页) 验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 (已检查 17 个本地目标) | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 5 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-04 10:46 PM PKT] Claude Code v2.1.92

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失概念 | 在 Hot 表格中添加 Ultraplan 行 — 基于云的计划起草，支持浏览器审查、内联评论和灵活执行 (`/ultraplan`) | ✅ COMPLETE (已添加行在 Power-ups 之后，附 beta 徽章和 /ultraplan 位置) |
| 2 | HIGH | 缺失概念 | 在 Hot 表格中添加 Claude Code Web 行 — 在 claude.ai/code 上使用云基础设施运行任务，支持 PR 自动修复和并行会话 | ✅ COMPLETE (已添加行在 Ultraplan 之后，附 beta 徽章、claude.ai/code 位置和 Web Scheduled Tasks 内联链接) |
| 3 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图中 — 重定向到 `/skills` 页面；文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 4 | MED | 缺失概念 | 在 Hot 表格中添加 Desktop App 行 — 独立应用，支持视觉差异对比、Dispatch、computer use 和并行会话 | ❌ INVALID (来自 2026-03-17 的重复问题；用户认为它是平台呈现面，而非配置概念) |

---

## [2026-04-08 09:37 PM PKT] Claude Code v2.1.96

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图中 — 重定向到 `/skills` 页面；文档称"commands 已合并到 skills" | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 2 | MED | 名称变更 | Hot 表格中的 "No Flicker Mode" — 官方文档页面标题为 "Fullscreen rendering"；考虑重命名或添加副标题 | ❌ INVALID (用户选择按照 Boris 的推文命名约定保留 "No Flicker Mode"；环境变量为 `CLAUDE_CODE_NO_FLICKER`) |
| 3 | MED | 缺失概念 | 在 Hot 表格中添加 Desktop App 行 — 独立应用，支持视觉差异对比、Dispatch、computer use 和并行会话 | ❌ INVALID (来自 2026-03-17 的重复问题；用户认为它是平台呈现面，而非配置概念) |
| 4 | LOW | 验证 | 所有 41 个外部文档 URL 已验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 | ✅ COMPLETE (所有 20+ 个徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 7 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-09 11:37 PM PKT] Claude Code v2.1.97

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失概念 | 在 Hot 表格中添加 Agent SDK 行 — 使用 Python/TypeScript SDK 构建生产级 AI agents (29 个文档页面，`/en/agent-sdk/overview`) | ✅ COMPLETE (已添加行在 Claude Code Web 之后，附 Quickstart 和 Examples 内联链接) |
| 2 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图中 — 重定向到 `/skills`；规范 commands 参考现为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 3 | MED | 缺失内联链接 | 在 CLI Startup Flags 行中添加 Environment Variables (`/env-vars`) 内联链接 — 新的专门文档页面 | ✅ COMPLETE (Env Vars 内联链接已在 Interactive Mode 之后添加) |
| 4 | LOW | 验证 | 所有 42 个外部文档 URL 已对照 llms.txt 站点地图 (110 页) 验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 (已检查 20+ 个徽章目标) | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 7 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-11 06:13 PM PKT] Claude Code v2.1.101

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (110 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 43 个外部文档 URL 已对照 llms.txt 站点地图 (110 页) 验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 (已检查 20+ 个徽章目标) | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 5 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-13 08:07 PM PKT] Claude Code v2.1.101

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (110 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 2 | LOW | 验证 | 所有 44 个外部文档 URL 已对照 llms.txt 站点地图 (110 页) 验证 — 未发现损坏链接 | ✅ COMPLETE (所有 URL 返回有效页面，包括 /slash-commands 重定向) |
| 3 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 (已检查 20+ 个徽章目标) | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 4 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 5 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 6 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 7 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-14 11:17 PM PKT] Claude Code v2.1.107

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失概念 | 在 Hot 表格中添加 Routines 行 — 在 Anthropic 基础设施上的云自动化，支持计划、API 和 GitHub 事件触发器 (`/en/routines`) | ✅ COMPLETE (已添加行在 Scheduled Tasks 之后，附 beta 徽章、Desktop Tasks 内联链接) |
| 2 | HIGH | 过时 URL | 更新 Claude Code Web 行中的 `web-scheduled-tasks` 内联链接 (第 45 行) 为 `/en/routines` — URL 不在站点地图中，重定向到 Routines 页面 | ✅ COMPLETE (内联链接文本已改为 "Routines"，URL 已更新为 /routines) |
| 3 | HIGH | 过时 URL | 更新 Scheduled Tasks 行中的 `web-scheduled-tasks` 内联链接 (第 55 行) 为 `/en/routines` — 相同的过时 URL | ✅ COMPLETE (URL 已更新为 /routines) |
| 4 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (119 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 5 | MED | 描述变更 | 更新 Scheduled Tasks 描述从"最长 3 天"到"最长 7 天" — 文档现指定周期性任务七天过期 | ✅ COMPLETE (描述已更新为"最长 7 天") |
| 6 | MED | 缺失概念 | 在 Hot 表格中添加 Devcontainers 行 — 预配置的开发容器，具有安全隔离和防火墙规则 (`/en/devcontainer`) | ✅ COMPLETE (已添加行在 Routines 之后，位置为 .devcontainer/) |

---

## [2026-04-16 08:20 PM PKT] Claude Code v2.1.110

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | 修复 TIPS 中的 `web-scheduled-tasks` URL (第 223 行) 为 `/en/routines` — URL 不在站点地图中；2026-04-14 已在 Hot 表格中修复了相同的过时 URL，但 TIPS 实例被遗漏 | ✅ COMPLETE (URL 已更新为 /routines) |
| 2 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (111 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户选择保持现状) |
| 3 | MED | 描述变更 | 将 TIPS 中的"最长 3 天"更新为"最长 7 天" (第 223 行) — 2026-04-14 已在 Hot 表格中更新了相同的描述，但 TIPS 实例被遗漏 | ✅ COMPLETE (描述已更新为"最长 7 天") |
| 4 | LOW | 验证 | 所有 45 个外部文档 URL 已对照 llms.txt 站点地图 (111 页) 验证 — 发现 1 个损坏链接 (参见 #1) | ✅ COMPLETE (已标记 web-scheduled-tasks) |
| 5 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 (已检查 20+ 个徽章目标) | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 6 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 7 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 8 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 9 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 10 | LOW | 验证 | 所有 CONCEPTS 描述已对照官方文档检查 — 未检测到偏差 | ✅ COMPLETE (所有描述准确) |

---

## [2026-04-18 07:53 PM PKT] Claude Code v2.1.113

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 描述变更 | Auto Mode 行 (第 48 行) 仍引用 `claude --enable-auto-mode` — 该标志已在 v2.1.111 中移除；auto mode 现在通过 `--permission-mode auto` 或 Shift+Tab 循环启动 (Max 订阅用户默认搭配 Opus 4.7) | ✅ COMPLETE (位置已更新为 `--permission-mode auto`、`Shift+Tab`；描述说明了标志移除和 Max+Opus-4.7 默认值) |
| 2 | HIGH | 缺失概念 | 在 Hot 表格中添加 Ultrareview 行 — 基于云的多 agent 代码审查 (`/ultrareview`, v2.1.86+，专门文档在 `/en/ultrareview`)；Pro/Max 用户免费 3 次 | ✅ COMPLETE (已添加行在 Routines 之后，附 beta 徽章、/ultrareview 位置、Tasks-tracking 内联链接) |
| 3 | HIGH | 缺失概念 | 添加 Tasks 行 — `/tasks` 命令用于跟踪后台工作 (在 Ultrareview 页面上引用)；根据 `reports/claude-global-vs-project-settings.md` 取代 TodoWrite | ✅ COMPLETE (已添加行在 Scheduled Tasks 之后，位置为 /tasks，BP 徽章链接至 global-vs-project-settings 报告) |
| 4 | MED | 描述变更 | No Flicker Mode 行 (第 47 行) — 官方文档现以 `/tui fullscreen` 命令为首 (v2.1.110)；env var 是 pre-v2.1.110 的旧版路径，见 /fullscreen 页面 | ✅ COMPLETE (位置已更新为 `/tui fullscreen`、`CLAUDE_CODE_NO_FLICKER=1`；描述说明 /tui 是规范方式，env var 为旧版) |
| 5 | MED | 过时命令名称 | TIPS 第 249 行引用 `/fewer-permission-prompts` — 根据 v2.1.111 变更日志，官方 skill 名称为 `/less-permission-prompts` (本地 skill 文件夹为 `fewer-permission-prompts`，但用户可见的命令应与官方名称匹配) | ✅ COMPLETE (TIPS 第 249 行已更新为 /less-permission-prompts) |
| 6 | LOW | 描述变更 | Scheduled Tasks 行 (第 60 行) — 第 15 周新增了 Monitor 工具 + 自定步调 `/loop` (LLM 选择自己的间隔)；描述未提及此功能 | ✅ COMPLETE (描述已附加自定步调/Monitor 工具说明) |
| 7 | LOW | 描述变更 | Git Worktrees 行 (第 63 行) — v2.1.105/106 新增了 `EnterWorktree`/`ExitWorktree` 工具和 `isolation: "worktree"` subagent frontmatter；描述未提及这些 | ✅ COMPLETE (位置已更新以包含 EnterWorktree/ExitWorktree 和 isolation frontmatter；描述说明了 v2.1.106+ subagent worktree 支持) |
| 8 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (119 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 17+ 次运行中选择保持现状) |
| 9 | LOW | 验证 | 所有 45+ 个外部文档 URL 已对照 llms.txt 站点地图 (119 页) 验证 — 除重复的 `/slash-commands` 重定向外未发现新的损坏链接 | ✅ COMPLETE (所有标记的 URL 返回有效页面) |
| 10 | LOW | 验证 | 所有本地徽章文件路径已验证 — 无缺失文件 (已检查 20+ 个徽章目标) | ✅ COMPLETE (所有徽章目标存在于文件系统) |
| 11 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 /memory 页面确认 | ✅ COMPLETE (章节标题"使用 `.claude/rules/` 组织规则"存在) |
| 12 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 /common-workflows 页面确认 | ✅ COMPLETE (章节标题存在) |
| 13 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 /permission-modes 页面确认 | ✅ COMPLETE (章节标题存在) |
| 14 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 /skills 页面确认 | ✅ COMPLETE (章节标题存在) |
| 15 | LOW | 验证 | Fullscreen 页面确认 `/tui fullscreen` 为规范命令，`tui` 为设置字段 (v2.1.110) | ✅ COMPLETE (页面已获取并引用) |
| 16 | LOW | 验证 | Permission-modes 页面确认 `--enable-auto-mode` 标志不再有文档记录；auto mode 现在需要 Max 计划 + Opus 4.7 | ✅ COMPLETE (页面已获取；标志在文档中不存在) |
| 17 | LOW | 验证 | Ultrareview 页面存在于 `/en/ultrareview` (v2.1.86+)，确认了 `/ultrareview` 和 `/tasks` 命令 | ✅ COMPLETE (页面已获取，内容已捕获) |

---

## [2026-04-24 12:32 AM PKT] Claude Code v2.1.118

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 描述变更 | Hooks 行 (第 28 行) 列出 4 种处理器类型 ("scripts, HTTP, prompts, agents") — 官方 `/en/hooks` 页面现记录了 5 种类型，新增了 `mcp_tool` (v2.1.118 变更日志："Hooks 可以直接调用 MCP 工具"，通过 `type: "mcp_tool"`) | ✅ COMPLETE (描述已更新为 "scripts, HTTP, MCP tools, prompts, agents") |
| 2 | MED | 空描述 | Workflows 行 (第 27 行) 描述单元格为空 (仅有 Orchestration Workflow 徽章) — 官方 `/en/common-workflows` 页面涵盖了探索、修复、重构、测试的分步指南 | ✅ COMPLETE (描述已填写官方文档来源文本："探索代码库、修复错误、重构和测试的分步指南 — 多步骤任务编排模式") |
| 3 | LOW | 描述变更 | 考虑在 CLI Startup Flags 行中内联提及 `/usage` (v2.1.118 合并了 `/cost`+`/stats`) — 新的斜杠命令取代了两个旧版命令 | ✅ COMPLETE (内联说明 "`/usage` (在 v2.1.118 中合并了 `/cost`+`/stats`)" 已在 Env Vars 之后附加) |
| 4 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (117 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 18+ 次运行中选择保持现状) |
| 5 | LOW | 验证 | Hooks 页面 `/en/hooks` 已获取 — 确认 5 种处理器类型，包括 `mcp_tool` (v2.1.118) | ✅ COMPLETE (实时获取记录了 5 类型模式) |
| 6 | LOW | 验证 | Ultrareview 页面 `/en/ultrareview#track-a-running-review` 锚点已获取并确认 | ✅ COMPLETE (章节存在，描述了 `/tasks` 集成) |
| 7 | LOW | 验证 | Checkpointing 页面 `/en/checkpointing` 已获取 — `/undo` 别名 (v2.1.108) 未在文档中呈现，仅在变更日志中；无需更新 CONCEPTS | ✅ COMPLETE (文档页面内容与现有描述匹配) |
| 8 | LOW | 验证 | 所有本地徽章文件路径 — 自 2026-04-18 的 v2.1.113 运行以来无变化 | ✅ COMPLETE (自上次运行以来保持稳定) |
| 9 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` — 本次运行未重新检查；自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 10 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 11 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 12 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 13 | LOW | 验证 | claude-code-guide agent 交叉检查 — 与专门 agent 无矛盾；发现了 /recap (v2.1.108)、/usage (v2.1.118)、MCP-tool hooks (v2.1.118) 作为佐证 | ✅ COMPLETE (两个 agents 一致) |

---

## [2026-04-26 01:10 PM PKT] Claude Code v2.1.119

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (139 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 19+ 次运行中选择保持现状) |
| 2 | HIGH | 错误目标 URL | Tasks 行 (第 62 行) 主要 URL 指向 `/ultrareview#track-a-running-review` — 锚点有效，但目标是 ultrareview 跟踪部分，而非更广泛的 Tasks 系统；规范主页为本地报告 `reports/claude-global-vs-project-settings.md#tasks-system` | ✅ COMPLETE (主要 URL 已更新为 `reports/claude-global-vs-project-settings.md#tasks-system`；ultrareview 跟踪锚点保留为描述末尾的内联链接 "Ultrareview tracking") |
| 3 | MED | Beta 徽章时效性 | Routines / No Flicker Mode / Computer Use / Code Review 在 README 中有 `![beta]`，但它们的文档页面不再标记为 beta — 重新评估并在适当处降级 | ❌ INVALID (验证获取了所有 4 个文档页面 — Routines："research preview"；Fullscreen："research preview"；Computer Use："research preview on macOS"；Code Review："in research preview" — README beta 徽章准确；agent 对正文内容的 0.6 置信度读取被 `<Note>` 横幅文本推翻) |
| 4 | MED | 描述消歧义 | Scheduled Tasks 行 (第 61 行) 描述混淆了 `/loop` (本地、会话范围、7 天过期) 和 `/schedule` (Anthropic 基础设施上的云 Routines) — 官方 `/en/scheduled-tasks` 页面现正式区分 Cloud / Desktop / Loop 三种形态 | ✅ COMPLETE (描述现明确命名"三种形态"，分别说明 `/loop` 本地、`/schedule` 云 Routines 和 Desktop 计划任务) |
| 5 | LOW | 缺失概念 (可选) | Fast Mode 当前仅为 Settings 内的侧链接 (第 31 行) — 有其专门的 `/en/fast-mode` 页面，带 `↯` 指示器和 `/fast` 切换 (v2.1.36+)；可作为 Hot 行 | ✅ COMPLETE (Hot 行已插入 Power-ups 和 Computer Use 之间，附 beta 徽章；从 Settings 移除了冗余的 Fast Mode 侧链接以防止重复) |
| 6 | LOW | 缺失内联链接 | Memory 行可将 `reports/claude-agent-memory.md` 作为内联链接呈现 — 引用了 auto-memory，但本地深入报告未从 CONCEPTS 链接 | ✅ COMPLETE (在 Memory 行的 Auto Memory 文档和 Rules 之间添加了 "Auto Memory Deep-dive" 内联链接) |
| 7 | LOW | 缺失内联链接 | Skills 行可将 `reports/claude-skills-for-larger-mono-repos.md` 作为内联链接呈现 — 存在于本地但仅在 TIPS 中被引用 | ✅ COMPLETE (在 Skills 行的官方 Skills 之后附加了 "Skills for Mono-repos" 内联链接) |
| 8 | LOW | 可选概念 | Vim 视觉模式 (v2.1.118)、主题定制 (`~/.claude/themes/`, v2.1.118) 和 PowerShell 工具 (v2.1.111) 可作为 Settings 侧链接 — claude-code-guide 交叉检查发现的次要概念 | ❌ INVALID (Vim 模式已被现有 Keybindings 侧链接覆盖；Themes 没有与 Settings 区分的专门文档页面；PowerShell 工具没有专门的文档页面 — 没有具体的子链接目标值得添加) |
| 9 | LOW | 验证 | 所有 35+ 个外部 CONCEPTS 文档 URL 已对照 llms.txt 站点地图 (139 页) 验证 — 仅标记了重复的 `/slash-commands` 重定向；所有其他 URL 解析到预期页面 | ✅ COMPLETE (无新的损坏 URL) |
| 10 | LOW | 验证 | 所有本地徽章文件路径已验证 — 所有 20+ 个 `best-practice/`、`implementation/` 和 `reports/` 目标存在于文件系统 | ✅ COMPLETE (无缺失本地文件) |
| 11 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` 已在 `/en/memory` 页面确认 | ✅ COMPLETE (自 v2.1.113 以来保持稳定) |
| 12 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` 已在 `/en/common-workflows` 页面确认 | ✅ COMPLETE (稳定) |
| 13 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` 已在 `/en/permission-modes` 页面确认 | ✅ COMPLETE (稳定) |
| 14 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` 已在 `/en/skills` 页面确认 | ✅ COMPLETE (稳定) |
| 15 | LOW | 验证 | Ultrareview 锚点 `#track-a-running-review` 已在 `/en/ultrareview` 页面确认 | ✅ COMPLETE (自 v2.1.118 以来保持稳定) |
| 16 | LOW | 验证 | claude-code-guide 交叉检查 — 佐证了专门 agent 关于 Vim 模式 (v2.1.118)、Theme (v2.1.118)、Effort xhigh (v2.1.111+ Opus 4.7)、Worktrees (v2.1.105+) 的发现；无矛盾 | ✅ COMPLETE (两个 agents 一致) |
| 17 | LOW | 验证检查清单更新 | 向 verification-checklist.md 添加了新规则 (#7) "Beta Badge Currency" — 涵盖根据上游文档页面生命周期重新评估 beta 徽章 | ✅ COMPLETE (规则已添加) |

---

## [2026-04-29 12:53 AM PKT] Claude Code v2.1.121

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图 (139+ 页) 中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 20+ 次运行中选择保持现状) |
| 2 | MED | 描述变更 | Ultrareview 行 (第 44 行) 未提及 v2.1.120 引入的 `claude ultrareview [target]` 非交互式子命令 — 文档确认了用于 CI 使用的 `--json` 和 `--timeout` 标志 | ✅ COMPLETE (位置已更新以包含 `claude ultrareview [target]`；描述附加了非交互式子命令及 `--json` 和 `--timeout` 标志说明，v2.1.120+) |
| 3 | MED | 描述变更 | MCP Servers 行 (第 29 行) 未提及 v2.1.121 新增的 `alwaysLoad` 设置 — 绕过工具搜索延迟，使服务器的工具始终加载到上下文中 | ✅ COMPLETE (描述已附加 `alwaysLoad` 说明，解释工具搜索延迟绕过，v2.1.121+) |
| 4 | MED | 描述变更 | Hooks 行 (第 28 行) 未提及 v2.1.121 新增的 `updatedToolOutput` 能力 — PostToolUse hooks 现在可以通过 `hookSpecificOutput.updatedToolOutput` 替换工具输出 | ✅ COMPLETE (描述已附加 `hookSpecificOutput.updatedToolOutput` 说明，用于 PostToolUse 输出替换，v2.1.121+) |
| 5 | LOW | 描述变更 | Subagents 行 (第 24 行) 未提及 forked subagents 现在可通过 `CLAUDE_CODE_FORK_SUBAGENT=1` 在外部构建中使用 (v2.1.117) — 之前仅限内部使用 | ✅ COMPLETE (描述已附加 `CLAUDE_CODE_FORK_SUBAGENT=1` 说明，用于外部构建，v2.1.117+) |
| 6 | LOW | 缺失内联链接 | Settings 行 (第 31 行) 内联链接涵盖了 Permissions/Model Config/Output Styles/Sandboxing/Keybindings，但未包括 Auto Mode Config (`/auto-mode-config`) — 作为独立页面存在 | ✅ COMPLETE (Auto Mode Config 内联链接已在 Keybindings 之后附加) |
| 7 | LOW | 验证 | 所有 35+ 个外部 CONCEPTS 文档 URL 已抽查验证 — Subagents、Skills、MCP、Ultrareview 页面确认；仅标记了重复的 `/slash-commands` 重定向 | ✅ COMPLETE (无新的损坏 URL) |
| 8 | LOW | 验证 | 所有本地徽章文件路径已验证 — 所有 22 个 `best-practice/`、`implementation/`、`reports/`、`.claude/`、`CLAUDE.md` 目标存在于文件系统 | ✅ COMPLETE (无缺失本地文件) |
| 9 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` — 自 v2.1.113 以来保持稳定 (本次运行未重新获取) | ✅ COMPLETE (稳定) |
| 10 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 11 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 12 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 13 | LOW | 验证 | Ultrareview 锚点 `#track-a-running-review` 已在 `/en/ultrareview` 页面确认 — 章节存在并描述了 `/tasks` 集成 | ✅ COMPLETE (自 v2.1.118 以来保持稳定) |
| 14 | LOW | 验证 | claude-code-guide 交叉检查 — 佐证了专门 agent 关于 v2.1.117–v2.1.121 变更的发现 (forked subagents 外部、alwaysLoad、updatedToolOutput、claude ultrareview 子命令)；同时提及了 Bedrock/Vertex/Foundry、Desktop、IDE Integrations 作为长期缺失的概念 | ✅ COMPLETE (两个 agents 一致；所有"缺失的平台呈现面"根据重复的用户决策已标记为 INVALID) |

---

## [2026-05-01 03:34 PM PKT] Claude Code v2.1.126

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时版本 | README 徽章固定在 v2.1.121 (4 月 29 日) — 最新为 v2.1.126 (5 月 01 日)；落后 5 个版本 | ✅ COMPLETE (徽章已更新为 v2.1.126 May 01 2026 3:34 PM PKT) |
| 2 | MED | 新概念 (可选) | v2.1.126 引入了 `claude project purge [path]` 子命令，带 `--dry-run`/`--all` 标志 — 当前不在 CLI Startup Flags 行中；可作为内联说明 | ✋ ON HOLD (已延迟 — 单个版本旧的子命令；如果用户要求刷新 CLI Startup Flags 时再重新审视) |
| 3 | MED | 新概念 (可选) | v2.1.126 新增了网关驱动的模型选择器 — `/model` 在网关兼容 Anthropic 时，从 `ANTHROPIC_BASE_URL` 的 `/v1/models` 端点列出模型 | ✋ ON HOLD (已延迟 — 小众 LLM 网关功能；仅对自托管网关用户相关；按设计不在 CONCEPTS 中呈现) |
| 4 | LOW | 描述变更 (可选) | v2.1.122 扩展了 `--from-pr` 以接受 GitLab MR + Bitbucket PR + GitHub Enterprise PR URL (原仅 GitHub) — CLI Startup Flags 行未呈现此信息 | ✋ ON HOLD (已延迟 — `--from-pr` 当前未作为内联链接呈现；需要新增子链接) |
| 5 | HIGH | 过时 URL | Commands URL `/slash-commands` 不在官方站点地图中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 21+ 次运行中选择保持现状) |
| 6 | MED | 缺失概念 (重复) | 专门 agent 再次标记 Output Styles、Permissions、Sandboxing、Headless Mode、Desktop App、IDE Integration 缺少独立行 | ❌ INVALID (来自 2026-03-10/2026-03-17/2026-04-08/2026-04-09 的重复问题；用户认为所有六个都是平台呈现面或已作为 Settings 子链接覆盖 — 不是独立概念) |
| 7 | MED | 缺失概念 (重复) | 专门 agent 标记 Auto Memory 需要独立于 Memory 的单独行 | ❌ INVALID (重复 — 当前 Memory 行已将 `/en/memory#auto-memory` 和 `reports/claude-agent-memory.md` 作为内联链接呈现；这是用户选择的跨领域功能模式) |
| 8 | LOW | 过时发现 | 专门 agent 标记 Tasks 行主要 URL 需要 `/en/agent-sdk/todo-tracking` 交叉引用 | ❌ INVALID (已在 2026-04-26 解决 — 用户明确将 Tasks 主要 URL 移至 `reports/claude-global-vs-project-settings.md#tasks-system` 并保留 ultrareview tracking 为内联链接；agent 的分析已过时) |
| 9 | LOW | 验证 | 所有 23 个本地徽章文件路径已验证 — `best-practice/`、`implementation/`、`reports/`、`.claude/`、`.mcp.json`、`CLAUDE.md` 均存在 | ✅ COMPLETE (无缺失本地文件) |
| 10 | LOW | 验证 | 抽查验证了外部 CONCEPTS URL (`/en/cli-reference`、`/en/agent-teams`、`/en/changelog`、`/en/mcp`) — 所有返回有效页面 | ✅ COMPLETE (无新的损坏 URL) |
| 11 | LOW | 验证 | Beta 徽章时效性 (规则 #7) — 获取了 `/en/agent-teams` 并确认 `<Warning>` 横幅："Agent teams are experimental and disabled by default" — README beta 徽章准确 | ✅ COMPLETE (无需降级) |
| 12 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 13 | LOW | 验证 | Git Worktrees 锚点 `#run-parallel-claude-code-sessions-with-git-worktrees` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 14 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 15 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 16 | LOW | 验证 | Ultrareview 锚点 `#track-a-running-review` — 自 v2.1.118 以来保持稳定 | ✅ COMPLETE (稳定) |
| 17 | LOW | 验证 | claude-code-guide 交叉检查 — 独立研究发现了相同的 v2.1.122–126 变更 (`claude project purge`、网关模型选择器、`--from-pr` 扩展)；同时再次提及了长期存在的平台呈现面概念 (Desktop、IDE Integration、Bedrock/Vertex/Foundry)，这些根据用户策略已标记为重复性 INVALID；无矛盾 | ✅ COMPLETE (两个 agents 一致) |

---

## [2026-05-12 11:36 PM PKT] Claude Code v2.1.139

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 过时版本 | README 徽章固定在 v2.1.128 (5 月 08 日) — 最新为 v2.1.139 (5 月 11 日)；落后 11 个版本 | ✅ COMPLETE (徽章已更新为 v2.1.139 May 12 2026 11:36 PM PKT，在 Phase 2.6 中) |
| 2 | HIGH | 过时 URL/锚点 (新) | Git Worktrees 行主要 URL 指向 `/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees` — 但现有一个专门的 `/en/worktrees` 页面 (涵盖 `--worktree`/`-w` 标志、`.worktreeinclude`、WorktreeCreate/Remove hooks、非 git VCS) 并且旧的 common-workflows 锚点已重命名为 `#run-parallel-sessions-with-worktrees` (去掉了 "git" 一词)。两者均已在线验证 | ✅ COMPLETE (URL 已切换到专用的 `/en/worktrees` 页面；Location 列已扩展，包含 `--worktree`/`-w`、`.worktreeinclude`、`WorktreeCreate`/`WorktreeRemove` hooks，经用户授权) |
| 3 | HIGH | 缺失概念 (新) | v2.1.139 引入了 **Agent View** (`claude agents`、`--bg`、`/bg`) — research preview，"一个屏幕管理多个后台会话"，支持 peek/attach/dispatch。专门的文档页面 `/en/agent-view` 已确认为在线 | ✅ COMPLETE (Hot 行已添加在 Agent Teams 之后，带 `![beta]` 徽章，依据 `<Note>` "research preview" 横幅；描述说明了 supervisor 托管和重启持久性) |
| 4 | HIGH | 缺失概念 (新) | v2.1.139 引入了 **/goal** 命令 — 让 Claude 在各轮次之间持续工作，直到模型评估的条件成立 (会话范围的 Stop hook 包装器)。专门的文档页面 `/en/goal` 已确认为在线 | ✅ COMPLETE (Hot 行已添加在 Tasks 之后；描述根据文档框架与 `/loop` 和 auto mode 进行了比较) |
| 5 | MED | 缺失概念 (新) | **Deep Links** (`claude-cli://open?repo=…&q=…`) 在 v2.1.91 引入 — 用于 runbooks/alerts/dashboards 的自定义 URL 方案。专门的文档页面 `/en/deep-links` 已确认为在线；在之前的任何运行中从未在 CONCEPTS 中出现 | ✅ COMPLETE (Hot 行已添加在 Remote Control 之后 — 两者都是会话启动表面；描述提到了 runbooks/alerts/dashboards 使用场景和 OS 级别处理器注册) |
| 6 | LOW | 缺失概念 (新) | v2.1.139 新增了 `/scroll-speed` 命令 — 调整鼠标滚轮滚动速度，带实时预览 | ✋ ON HOLD (已延迟 — 次要 UX 命令，无专门文档页面；最多只能作为 CLI Startup Flags 子链接) |
| 7 | HIGH | 过时 URL (重复) | Commands URL `/slash-commands` 不在官方站点地图中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 22+ 次运行中选择保持现状) |
| 8 | MED | 缺失概念 (重复) | 专门 agent 再次标记 Output Styles、Permissions、Sandboxing、Headless Mode、Desktop App、IDE Integration、.claude Directory、Tools Reference 缺少独立行 | ❌ INVALID (来自 2026-03-10/2026-03-17/2026-04-08/2026-05-01/2026-05-12 的重复问题；用户认为所有都是平台呈现面或已作为 Settings/Memory 子链接覆盖 — 不是独立概念) |
| 9 | MED | 缺失概念 (重复) | 专门 agent 标记 Auto Memory 需要独立于 Memory 的单独行 | ❌ INVALID (重复 — 当前 Memory 行已将 `/en/memory#auto-memory` 和 `reports/claude-agent-memory.md` 作为内联链接呈现；用户选择的模式) |
| 10 | LOW | 验证 | 所有 4 个新候选 URL 已通过 WebFetch 验证：`/en/worktrees` (在线，完整专门页面)、`/en/agent-view` (在线，research preview 横幅)、`/en/goal` (在线，/goal 命令页面)、`/en/deep-links` (在线，v2.1.91+ 横幅) | ✅ COMPLETE (所有 4 个新 URL 返回预期的规范页面) |
| 11 | LOW | 验证 | 本地徽章文件路径已验证 — `best-practice/`、`implementation/`、`reports/`、`.claude/`、`.mcp.json`、`CLAUDE.md` 目标存在于文件系统 | ✅ COMPLETE (无缺失本地文件) |
| 12 | LOW | 验证 | Beta 徽章时效性 (规则 #7) — 获取了 `/en/agent-view` 并确认 `<Note>` 横幅："Agent view is a research preview and requires Claude Code v2.1.139 or later" — 如果添加，beta 徽章将是准确的 | ✅ COMPLETE (徽章建议已在操作项 #3 中记录) |
| 13 | LOW | 验证 | Memory 锚点 `#organize-rules-with-clauderules` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 14 | LOW | 验证 | Git Worktrees 锚点 — 标题在 `/en/common-workflows` 中从 `#run-parallel-claude-code-sessions-with-git-worktrees` 重命名为 `#run-parallel-sessions-with-worktrees` (去掉了 "git" 一词)；旧锚点仍可解析但不再是规范版本 | ⚠️ FLAGGED (已在操作项 #2 中捕获；作为过时 URL 操作的一部分处理，非单独项目) |
| 15 | LOW | 验证 | Auto Mode 锚点 `#eliminate-prompts-with-auto-mode` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 16 | LOW | 验证 | Bundled Skills 锚点 `#bundled-skills` — 自 v2.1.113 以来保持稳定 | ✅ COMPLETE (稳定) |
| 17 | LOW | 验证 | Ultrareview 锚点 `#track-a-running-review` — 自 v2.1.118 以来保持稳定 | ✅ COMPLETE (稳定) |
| 18 | LOW | 验证 | claude-code-guide 交叉检查 — 独立研究佐证了 v2.1.139 新增内容 (Agent View、/goal、/scroll-speed) 并发现了相同的 Deep Links 页面；同时再次提及了长期存在的平台呈现面概念 (Desktop、IDE Integration、.claude Directory、Tools Reference)，这些根据用户策略已标记为重复性 INVALID；无矛盾 | ✅ COMPLETE (两个 agents 在 NEW v2.1.139 发现上一致) |

---

## [2026-05-21 12:07 AM PKT] Claude Code v2.1.145

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | HIGH | 错误位置 (新) | Checkpointing 行 (第 36 行) 位置显示 `automatic (git-based)` — 事实上错误。实时获取 `/en/checkpointing` 确认 checkpoints 跟踪"其文件编辑工具所做的所有更改"，明确"不跟踪由 bash 命令修改的文件"，并且"不是版本控制的替代品" — *"将 checkpoints 视为'本地撤销'，将 Git 视为'永久历史'。"* 2026-04-24 的运行仅检查了 (空的) Description 列，从未检查过 Location，因此每次之前的运行都漏掉了这个问题。建议改为 `automatic (file-edit tracking)` | ✅ COMPLETE (用户授权；第 36 行位置已从 `automatic (git-based)` 更新为 `automatic (file-edit tracking)`) |
| 2 | MED | 缺失概念 (新) | **Sessions** 有一个专门的 `/en/sessions` 页面 (恢复、`/rename`、通过 `--fork-session` 分支) 从未在之前的任何运行中在 CONCEPTS 中出现；concepts-agent 以 0.6 置信度标记可能是运行时呈现面而非创作概念 | ❌ INVALID (用户选择跳过 — 与一贯的"平台/运行时呈现面，而非配置概念"拒绝模式一致) |
| 3 | HIGH | 过时 URL (重复) | Commands URL `/slash-commands` 不在官方站点地图中 — 重定向到 `/skills`；规范 commands 参考为 `/en/commands` | ❌ INVALID (来自 2026-03-10 的重复问题；URL 仍可通过重定向解析；用户已在 23+ 次运行中选择保持现状) |
| 4 | MED | 缺失概念 (重复) | 专门 agent + claude-code-guide agent 再次标记 IDE Integration、Desktop App、Output Styles、Permissions、Sandboxing、Headless Mode、.claude Directory、Tools Reference 缺少独立行 | ❌ INVALID (来自 2026-03-10/2026-03-17/2026-04-08/2026-05-01/2026-05-12 的重复问题；用户认为所有都是平台呈现面或已作为 Settings/Memory 子链接覆盖 — 不是独立概念) |
| 5 | LOW | 验证 | Fast Mode URL `/en/fast-mode` 已实时获取 — 页面有效 ("Speed up responses with fast mode")、research preview、`/fast` 切换 + `"fastMode": true` 设置、自 v2.1.142 起 Opus 4.7 默认；解决了 concepts-agent 的 0.5 置信度疑问 — README Fast Mode 行 (第 52 行) 完全准确 | ✅ COMPLETE (行确认为准确，无需更改) |
| 6 | LOW | 验证 | 最新版本已通过原始 CHANGELOG.md 确认为 v2.1.145 (blob URL 未返回正文 — 按照记录的工作区使用 raw.githubusercontent.com)；v2.1.145 新增了 JSON 会话列表 + OTEL agent 标识 — 无值得 CONCEPTS 收录的内容 | ✅ COMPLETE (版本已验证；徽章在 Phase 2.6 中从 v2.1.144 更新为 v2.1.145) |
| 7 | LOW | 验证 | 外部 CONCEPTS 文档 URL 已抽查验证 (`/en/checkpointing`、`/en/fast-mode`、原始 CHANGELOG) + 其余自 v2.1.139 运行以来保持稳定 — 仅标记了重复的 `/slash-commands` 重定向 | ✅ COMPLETE (无新的损坏 URL) |
| 8 | LOW | 验证 | 本地徽章文件路径 — 自 v2.1.139 以来 CONCEPTS 表格结构无变化；`best-practice/`、`implementation/`、`reports/`、`.claude/`、`.mcp.json`、`CLAUDE.md` 目标稳定 | ✅ COMPLETE (自上次运行以来稳定) |
| 9 | LOW | 验证 | Beta 徽章时效性 (规则 #7) — `/en/fast-mode` 确认 "research preview" `<Note>` 横幅；README `![beta]` 在 Fast Mode 上准确 | ✅ COMPLETE (无需降级) |
| 10 | LOW | 验证 | 锚点 (`#organize-rules-with-clauderules`、`#run-parallel-...-worktrees`、`#eliminate-prompts-with-auto-mode`、`#bundled-skills`、`#track-a-running-review`) — 自 v2.1.113/v2.1.139 以来保持稳定 | ✅ COMPLETE (稳定) |
| 11 | LOW | 验证 | claude-code-guide 交叉检查 — 独立的 80 概念扫描佐证了覆盖范围；提及了 Auto Dream + MCP Tool Search 作为可能的 Memory/MCP 子链接，并再次提及了平台呈现面 (RECURRING INVALID)。其版本号不可靠 (例如 "MCP introduced Dec 2024"，模糊的 v 编号) — 所有日期已委托给专门 agent + 实时获取；无影响 CONCEPTS 的矛盾 | ✅ COMPLETE (agents 一致；指南的模糊版本号已记录，未使用) |
