# Settings Report — 变更日志历史

## 状态图例

| Status | 含义 |
|--------|------|
| ✅ `COMPLETE (reason)` | 已采取行动并成功解决 |
| ❌ `INVALID (reason)` | 发现项不正确、不适用或有意为之 |
| ✋ `ON HOLD (reason)` | 行动推迟 — 等待外部依赖或用户决定 |

---

## [2026-03-05 06:18 AM PKT] Claude Code v2.1.69

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 添加 13 个非 hooks 缺失设置键（`$schema`, `availableModels`, `fastModePerSessionOptIn`, `teammateMode`, `prefersReducedMotion`, `sandbox.filesystem.*`, `sandbox.network.allowManagedDomainsOnly`, `sandbox.enableWeakerNetworkIsolation`, `allowManagedMcpServersOnly`, `blockedMarketplaces`, `includeGitInstructions`, `pluginTrustMessage`, `fileSuggestion` 表条目） | ✅ COMPLETE（已添加到报告） |
| 2 | HIGH | 缺失环境变量 | 添加缺失环境变量，包括 `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`, `CLAUDE_CODE_DISABLE_1M_CONTEXT`, `CLAUDE_CODE_ACCOUNT_UUID`, `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS`, `ENABLE_CLAUDEAI_MCP_SERVERS` 等 | ✅ COMPLETE（已添加 13 个缺失环境变量到报告） |
| 3 | HIGH | Effort 默认值 | 更新 effort level 默认值从 "High" 改为 "Medium"（适用于 Max/Team 订阅用户）；添加 Sonnet 4.6 支持（v2.1.68 变更） | ✅ COMPLETE（已更新默认值并添加 Sonnet 说明） |
| 4 | MED | 设置层级 | 添加通过 macOS plist/Windows Registry 管理的设置（v2.1.61/v2.1.69）；记录跨作用域的数组合并行为 | ✅ COMPLETE（已添加 plist/registry 和合并说明） |
| 5 | MED | Sandbox 文件系统 | 添加 `sandbox.filesystem.allowWrite`, `denyWrite`, `denyRead` 及路径前缀语义（`//`, `~/`, `/`, `./`） | ✅ COMPLETE（已添加到 sandbox 表） |
| 6 | MED | 权限语法 | 添加 `Agent(name)` 权限模式；记录 `MCP(server:tool)` 语法形式 | ✅ COMPLETE（已添加到工具语法表） |
| 7 | MED | 插件缺口 | 添加 `blockedMarketplaces`, `pluginTrustMessage` | ✅ COMPLETE（已添加到插件表） |
| 8 | MED | 模型配置 | 添加 `availableModels` 设置 | ✅ COMPLETE（已添加到通用设置表） |
| 9 | MED | 可疑键 | 验证 `sandbox.network.deniedDomains`, `sandbox.ignoreViolations`, `pluginConfigs` — 报告中有但官方文档中无 | ✋ ON HOLD（保留在报告中等待验证） |
| 10 | LOW | 标题计数 | 更新标题从 "38 settings and 84 env vars" 为实际计数（约 55+ 设置，约 110+ 环境变量） | ✅ COMPLETE（已更新标题） |
| 11 | LOW | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层级（添加 managed/CLI/user 层级） | ✋ ON HOLD（等待用户批准） |
| 12 | LOW | 示例更新 | 更新 Quick Reference 示例，添加 `$schema`、sandbox 文件系统、`Agent(*)`，移除 hooks 示例 | ✅ COMPLETE（已更新示例） |
| 13 | MED | Hooks 重定向 | 用重定向链接替换 hooks 部分，指向 claude-code-hooks 仓库 | ✅ COMPLETE（hooks 已外部化到专用仓库） |

---

## [2026-03-07 02:17 PM PKT] Claude Code v2.1.71

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 行为变更 | 修复 `teammateMode`：类型 `boolean` → `string`，默认值 `false` → `"auto"`，描述 → "Agent team display: auto, in-process, tmux" | ✅ COMPLETE（类型、默认值和描述已更新） |
| 2 | HIGH | 新设置项 | 在 Permissions 表中添加 `allowManagedPermissionRulesOnly`（boolean，仅 managed） | ✅ COMPLETE（已添加到 Permission Keys 表） |
| 3 | HIGH | 缺失环境变量 | 添加约 31 个缺失环境变量，包括已确认的（`CLAUDE_CODE_MAX_OUTPUT_TOKENS`, `CLAUDE_CODE_DISABLE_FAST_MODE`, `CLAUDE_CODE_DISABLE_AUTO_MEMORY`, `CLAUDE_CODE_USER_EMAIL`, `CLAUDE_CODE_ORGANIZATION_UUID`, `CLAUDE_CONFIG_DIR`）和 agent 报告的（Foundry, Bedrock, mTLS, shell prefix 等） | ✅ COMPLETE（已添加 31 个环境变量到表） |
| 4 | MED | 默认值变更 | 修复 `plansDirectory` 默认值从 `.claude/plans/` 改为 `~/.claude/plans` | ✅ COMPLETE（默认值已更新） |
| 5 | MED | 描述变更 | 修复 `sandbox.enableWeakerNetworkIsolation` 描述为 "(macOS only) Allow access to system TLS trust; reduces security" | ✅ COMPLETE（描述已更新） |
| 6 | MED | 作用域修复 | 修复 `extraKnownMarketplaces` 作用域从 "Any" 改为 "Project" | ✅ COMPLETE（作用域和描述已更新） |
| 7 | MED | 边界违规 | 将 `claude-cli-startup-flags.md` 中的 `CLAUDE_CODE_EFFORT_LEVEL` 替换为对 settings report 的交叉引用 | ✅ COMPLETE（已替换为链接） |
| 8 | MED | 版本徽章 | 更新报告版本从 v2.1.69 到 v2.1.71 | ✅ COMPLETE（徽章和标题已更新） |
| 9 | LOW | 可疑键 | 验证 `skipWebFetchPreflight`, `sandbox.ignoreViolations`, `sandbox.network.deniedDomains`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起持续） |
| 10 | LOW | CLAUDE.md 同步 | 更新 CLAUDE.md 配置层级（3 级 → 5+ 级） | ✅ COMPLETE（已更新为 5 级层级，包含 managed 层） |

---

## [2026-03-12 12:23 PM PKT] Claude Code v2.1.74

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 行为变更 | 修复 `dontAsk` 权限模式描述："Auto-accept all tools" → "Auto-denies tools unless pre-approved via `/permissions` or `permissions.allow` rules" | ✅ COMPLETE（描述已根据官方权限文档更正） |
| 2 | HIGH | 新设置项 | 在 Model Configuration 部分添加 `modelOverrides`（object，将 Anthropic 模型 ID 映射到特定提供商 ID，如 Bedrock ARNs） | ✅ COMPLETE（已添加示例和描述） |
| 3 | HIGH | 新设置项 | 将 `allow_remote_sessions` 添加到仅 managed 设置列表（boolean，默认 `true`，控制 Remote Control/web session 访问） | ✅ COMPLETE（已添加到 Permission Keys 表） |
| 4 | HIGH | 默认值变更 | 修复 `$schema` URL 从 `https://www.schemastore.org/...` 为 `https://json.schemastore.org/...` 根据官方文档 | ✅ COMPLETE（已在描述、示例和 Sources 中更新） |
| 5 | MED | 描述变更 | 修复 `ANTHROPIC_CUSTOM_HEADERS` 格式描述从 "JSON string" 为 "Name: Value format, newline-separated" | ✅ COMPLETE（描述已根据官方文档更新） |
| 6 | MED | 未验证模式 | `askEdits` 和 `viewOnly` 权限模式不在官方文档中 — 仅 5 种模式有文档记录（default, acceptEdits, plan, dontAsk, bypassPermissions） | ✅ COMPLETE（表中已标记为 "not in official docs — unverified"） |
| 7 | MED | 缺失环境变量 | 添加 `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS`, `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY`, `CLAUDE_CODE_DISABLE_TERMINAL_TITLE`, `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL`, `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS` | ✅ COMPLETE（已添加 5 个环境变量及 `CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS`） |
| 8 | MED | 新设置项 | 在 Core Configuration 中添加 `autoMemoryDirectory`（string，自定义 auto-memory 目录）— 版本不确定（agents 有分歧：v2.1.68 vs v2.1.74），不在 settings 页面上 | ✅ COMPLETE（已添加在 plansDirectory 附近 — 版本未解决） |
| 9 | LOW | 可疑键 | 验证 `skipWebFetchPreflight`, `sandbox.ignoreViolations`, `sandbox.network.deniedDomains`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍不在官方文档中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起持续） |
| 10 | LOW | 缺失环境变量 | 将 `CLAUDE_CODE_SUBAGENT_MODEL` 添加到环境变量表（已在 Model env 示例块中，但表中缺失） | ✅ COMPLETE（已添加到环境变量表） |
| 11 | LOW | 示例更新 | 更新 Quick Reference 示例以包含 `modelOverrides` 和修正后的 `$schema` URL | ✅ COMPLETE（示例已更新包含两者） |

---

## [2026-03-14 01:35 AM PKT] Claude Code v2.1.75

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 设置层级 | 重构以匹配官方 5 级层级：Managed (#1) > CLI args > Local > Project > User。移除 `~/.claude/settings.local.json` 行。添加 managed 层内部优先级（server-managed > MDM > file > HKCU）。Managed "cannot be overridden by any other level, including CLI args" | ✅ COMPLETE（已将表重构为 5 级，Managed 为 #1，添加了交付方式、内部优先级和文件路径） |
| 2 | HIGH | 行为变更 | 修复 `availableModels` 描述：从复杂对象数组（`title`/`modelId`/`effortOptions`）改为简单字符串数组 `["sonnet", "haiku"]` 根据官方文档 | ✅ COMPLETE（描述已更新为匹配官方文档格式） |
| 3 | HIGH | 行为变更 | 添加 `cleanupPeriodDays` 的 `0` 值行为："Setting to `0` deletes all existing transcripts at startup and disables session persistence entirely" | ✅ COMPLETE（已添加 0 值行为到描述） |
| 4 | HIGH | 权限语法 | 在 Permissions 部分添加评估顺序说明："Rules are evaluated in order: deny rules first, then ask, then allow. The first matching rule wins." | ✅ COMPLETE（已在 Bash wildcard 说明之前添加评估顺序） |
| 5 | MED | 描述变更 | 添加 `autoMemoryDirectory` 作用域限制："Not accepted in project settings (`.claude/settings.json`). Accepted from policy, local, and user settings" | ✅ COMPLETE（已添加作用域限制到描述） |
| 6 | MED | 描述变更 | 添加 `permissions.defaultMode` 的 Remote 环境说明：仅 `acceptEdits` 和 `plan` 在 Remote 环境中有效（v2.1.70） | ✅ COMPLETE（已添加 Remote 限制到描述） |
| 7 | MED | 模型配置 | 添加 Opus 4.6 1M 上下文默认说明：自 v2.1.75 起，1M 上下文是 Max/Team/Enterprise 计划的默认配置 | ✅ COMPLETE（已添加到 Effort Level 说明中） |
| 8 | MED | 设置层级 | 添加 Windows managed 路径说明：v2.1.75 移除了已弃用的 `C:\ProgramData\ClaudeCode\` 后备路径 — 使用 `C:\Program Files\ClaudeCode\managed-settings.json` | ✅ COMPLETE（已在层级部分添加弃用说明） |
| 9 | MED | 显示与 UX | 添加 `fileSuggestion` 标准输入 JSON 格式（`{"query": "..."}`）和 15 路径输出限制详情 | ✅ COMPLETE（已在 File Suggestion 部分添加标准输入格式和输出限制） |
| 10 | MED | 设置层级 | 更新数组合并说明从 "merged" 为 "concatenated and deduplicated" 根据官方文档 | ✅ COMPLETE（已在层级 Important 部分更新措辞） |
| 11 | LOW | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍不在官方文档或 JSON schema 顶层 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起持续） |
| 12 | LOW | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 确认在 JSON schema 中但不在官方 settings 页面上 | ✋ ON HOLD（保留在报告中 — 根据 schema 有效，自 2026-03-05 起持续） |

---

## [2026-03-15 12:52 PM PKT] Claude Code v2.1.76

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 General Settings 或 Model Configuration 中添加 `effortLevel` — 跨 session 持久化 effort level（`"low"`, `"medium"`, `"high"`）。已在官方 settings 页面确认 | ✋ ON HOLD（等待用户批准） |
| 2 | HIGH | 新设置项 | 添加 Worktree Settings 部分，包含 `worktree.sparsePaths`（array，sparse-checkout cone mode）和 `worktree.symlinkDirectories`（array，symlink 目录以避免重复）。已在官方 settings 页面确认 | ✋ ON HOLD（等待用户批准） |
| 3 | HIGH | 新设置项 | 在 General Settings 中添加 `feedbackSurveyRate` — session 质量调查的概率（0-1）。已在官方 settings 页面确认 | ✋ ON HOLD（等待用户批准） |
| 4 | HIGH | 缺失环境变量 | 在表中添加 20 个缺失环境变量：`CLAUDE_CODE_AUTO_COMPACT_WINDOW`, `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION`, `CLAUDE_CODE_PLAN_MODE_REQUIRED`, `CLAUDE_CODE_TEAM_NAME`, `CLAUDE_CODE_TASK_LIST_ID`, `CLAUDE_ENV_FILE`, `FORCE_AUTOUPDATE_PLUGINS`, `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY`, `MCP_TOOL_TIMEOUT`, `MCP_CLIENT_SECRET`, `MCP_OAUTH_CALLBACK_PORT`, `IS_DEMO`, `SLASH_COMMAND_TOOL_CHAR_BUDGET`, `VERTEX_REGION_CLAUDE_3_5_HAIKU`, `VERTEX_REGION_CLAUDE_3_7_SONNET`, `VERTEX_REGION_CLAUDE_4_0_OPUS`, `VERTEX_REGION_CLAUDE_4_0_SONNET`, `VERTEX_REGION_CLAUDE_4_1_OPUS`。已在官方 /en/env-vars 页面确认 | ✋ ON HOLD（等待用户批准） |
| 5 | HIGH | 缺失环境变量 | 将 `ANTHROPIC_DEFAULT_OPUS_MODEL`, `ANTHROPIC_DEFAULT_SONNET_MODEL`, `MAX_THINKING_TOKENS` 从仅代码块移至 Common Environment Variables 表 | ✋ ON HOLD（等待用户批准） |
| 6 | HIGH | 损坏链接 | 修复 `https://claudelog.com/configuration/` — 返回 ECONNREFUSED。移除或替换为可用的源 | ✋ ON HOLD（等待用户批准） |
| 7 | MED | 描述变更 | 更新 `cleanupPeriodDays` 描述，添加：当设置为 0 时 "hooks receive an empty `transcript_path`"。根据官方文档 | ✋ ON HOLD（等待用户批准） |
| 8 | MED | 未验证环境变量 | 将报告中但不在官方文档中的 7 个环境变量标记为未验证：`CLAUDE_CODE_DISABLE_MCP`, `CLAUDE_CODE_DISABLE_TOOLS`, `CLAUDE_CODE_HIDE_ACCOUNT_INFO`, `CLAUDE_CODE_MAX_TURNS`, `CLAUDE_CODE_PROMPT_CACHING_ENABLED`, `CLAUDE_CODE_SKIP_SETTINGS_SETUP`, `DISABLE_NON_ESSENTIAL_MODEL_CALLS` | ✋ ON HOLD（等待用户批准） |
| 9 | MED | 新来源 | 在 Sources 部分添加 `https://code.claude.com/docs/en/env-vars` — 官方环境变量参考页面 | ✋ ON HOLD（等待用户批准） |
| 10 | MED | 示例更新 | 更新 Quick Reference 示例以包含 `effortLevel` 和 `worktree` 设置 | ✋ ON HOLD（等待用户批准） |
| 11 | LOW | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍不在官方文档 sandbox 表中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起持续） |
| 12 | LOW | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍在 JSON schema 中但不在官方 settings 页面上 | ✋ ON HOLD（保留在报告中 — 根据 schema 有效，自 2026-03-05 起持续） |

---

## [2026-03-15 01:10 PM PKT] Claude Code v2.1.76

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 Model Configuration 中添加 `effortLevel` — 跨 session 持久化 effort level（`"low"`, `"medium"`, `"high"`）。同时添加了 `/effort` 命令到 Useful Commands 并更新了 Effort Level 操作指南部分 | ✅ COMPLETE（已添加到 Model Overrides 表，更新了操作指南，添加了 /effort 命令） |
| 2 | HIGH | 新设置项 | 添加 Worktree Settings 部分，包含 `worktree.sparsePaths`（array，sparse-checkout cone mode）和 `worktree.symlinkDirectories`（array，symlink 目录以避免重复） | ✅ COMPLETE（Core Configuration 中新增 Worktree Settings 子部分，包含表和示例） |
| 3 | HIGH | 新设置项 | 在 General Settings 中添加 `feedbackSurveyRate` — session 质量调查的概率（0-1） | ✅ COMPLETE（已添加到 General Settings 表） |
| 4 | HIGH | 缺失环境变量 | 在表中添加 23 个缺失环境变量（20 个真正新增 + 3 个从仅代码块移入） | ✅ COMPLETE（已将所有 23 个环境变量添加到 Common Environment Variables 表） |
| 5 | HIGH | 损坏链接 | 上一轮将 `https://claudelog.com/configuration/` 标记为 ECONNREFUSED — 现在成功加载 | ✅ COMPLETE（链接已恢复，无需操作） |
| 6 | MED | 权限语法 | 添加 Read/Edit gitignore 风格路径模式（`//path`, `~/path`, `/path`, `./path`）、词边界通配符详情和遗留 `:*` 弃用说明 | ✅ COMPLETE（已添加路径模式表、词边界说明和 `:*` 弃用说明） |
| 7 | MED | 描述变更 | 更新 `cleanupPeriodDays`，当设置为 0 时添加 "hooks receive an empty `transcript_path`" | ✅ COMPLETE（已添加到描述） |
| 8 | MED | 未验证环境变量 | 将不在官方文档中的 7 个环境变量标记为未验证 | ✅ COMPLETE（已添加 "not in official docs — unverified" 标记） |
| 9 | MED | 新来源 | 在 Sources 部分添加 `https://code.claude.com/docs/en/env-vars` 和 `https://code.claude.com/docs/en/permissions` | ✅ COMPLETE（已添加两个 URL） |
| 10 | MED | 示例更新 | 更新 Quick Reference 示例以包含 `effortLevel` 和 `worktree` 设置 | ✅ COMPLETE（已向示例添加 effortLevel 和 worktree 块） |
| 11 | LOW | 可疑键 | `sandbox.ignoreViolations`, `sandbox.network.deniedDomains` 仍不在官方文档 sandbox 表中 | ✋ ON HOLD（保留在报告中等待验证 — 自 2026-03-05 起持续） |
| 12 | LOW | 可疑键 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 仍在 JSON schema 中但不在官方 settings 页面上 | ✋ ON HOLD（保留在报告中 — 根据 schema 有效，自 2026-03-05 起持续） |

---

## [2026-03-17 12:54 PM PKT] Claude Code v2.1.77

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 Sandbox Settings 表中添加 `sandbox.filesystem.allowRead` — 在 `denyRead` 区域内重新允许读取访问（array，默认 `[]`）。已根据 v2.1.77 变更日志确认 | ✅ COMPLETE（已在 Sandbox Settings 表中添加在 denyRead 行之后） |
| 2 | HIGH | 描述变更 | 更新 `CLAUDE_CODE_MAX_OUTPUT_TOKENS` 描述：Opus 4.6 的默认值增加到 64k，Opus 4.6 和 Sonnet 4.6 的上限增加到 128k（v2.1.77 变更日志） | ✅ COMPLETE（描述已更新，包含模型特定的默认值和上限） |
| 3 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDECODE` — 在生成的 shell 环境中设置为 `1`。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加到环境变量表） |
| 4 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_SKIP_FAST_MODE_NETWORK_ERRORS` — 允许在组织状态检查失败时使用 fast mode。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加到环境变量表） |
| 5 | MED | 环境变量表 | 将 `ANTHROPIC_MODEL` 和 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 从仅代码块移至 Common Environment Variables 表。两者已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加两者到环境变量表，靠近其他 ANTHROPIC_ 变量） |
| 6 | MED | 可疑键升级 | `sandbox.network.deniedDomains` — 连续 8 次 ON HOLD（自 2026-03-05 起）。不在官方文档页面或 JSON schema 中。根据 Rule 10B：标记为 "not in official docs — unverified" | ✅ COMPLETE（已向描述添加未验证注释） |
| 7 | MED | 可疑键升级 | `allow_remote_sessions` — 不在官方文档页面或 JSON schema 中。标记为 "not in official docs — unverified" | ✅ COMPLETE（已向描述添加未验证注释） |
| 8 | LOW | 可疑键解决 | `sandbox.ignoreViolations` — 连续 8 次 ON HOLD。确认在 JSON schema 中。注释："in JSON schema, not on official settings page" | ✅ COMPLETE（已向描述添加 schema 注释） |
| 9 | LOW | 可疑键解决 | `skipWebFetchPreflight`, `skippedMarketplaces`, `skippedPlugins`, `pluginConfigs` — 连续 8 次 ON HOLD。全部确认在 JSON schema 中。注释："in JSON schema, not on official settings page" | ✅ COMPLETE（已向所有 4 个描述添加 schema 注释） |
| 10 | LOW | 标题计数 | 更新标题环境变量计数从 "160+" 为 "100+" — 实际表中有 97 个环境变量 | ✅ COMPLETE（标题已更新为 "100+ environment variables"，版本为 v2.1.77） |

---

## [2026-03-18 11:53 PM PKT] Claude Code v2.1.78

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 在 General Settings 表中添加 `voiceEnabled` — 启用 push-to-talk 语音输入（boolean，由 `/voice` 写入，需要 Claude.ai 账户）。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 General Settings 表 feedbackSurveyRate 之前） |
| 2 | HIGH | 缺失设置项 | 在 Sandbox Settings 表中添加 `filesystem.allowManagedReadPathsOnly` — 仅 managed，仅受管的 `allowRead` 路径生效（boolean，默认 false）。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Sandbox Settings 表 enableWeakerNetworkIsolation 之前） |
| 3 | HIGH | 显示位置 | 将 `showTurnDuration` 和 `terminalProgressBarEnabled` 从 Display Settings 表移至单独的 "Global Config Settings (~/.claude.json)" 子部分。官方文档指出："Adding them to settings.json will trigger a schema validation error" | ✅ COMPLETE（已创建新的子部分并包含表；已从 settings.json Display Settings 表和示例中移除） |
| 4 | HIGH | 默认值变更 | 修复 `MAX_MCP_OUTPUT_TOKENS` 默认值从 50000 为 25000。官方 /en/env-vars 页面确认默认值：25000 | ✅ COMPLETE（默认值已更新，添加了警告阈值说明） |
| 5 | HIGH | 缺失环境变量 | 在环境变量表中添加 `CLAUDE_CODE_NEW_INIT`, `CLAUDE_CODE_PLUGIN_SEED_DIR`, `DISABLE_FEEDBACK_COMMAND`。全部已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加全部 3 个环境变量到表） |
| 6 | MED | 验证修复 | 从 `allow_remote_sessions` 移除 "unverified" 注释 — 现在已在官方 permissions 页面上确认为仅 managed 设置。上一轮（v2.1.77 #7）错误地将其标记为未验证 | ✅ COMPLETE（已移除 "unverified" 注释） |
| 7 | MED | 环境变量重命名 | 将 `DISABLE_BUG_COMMAND` 更新为 `DISABLE_FEEDBACK_COMMAND` — 官方文档指出 `DISABLE_FEEDBACK_COMMAND` 是当前名称，`DISABLE_BUG_COMMAND` 是 "the older name" | ✅ COMPLETE（已重命名并添加别名说明） |
| 8 | MED | 描述变更 | 更新 `CLAUDE_CODE_EFFORT_LEVEL` 以包含 `max`（仅 Opus 4.6）和 `auto` 值。官方 /en/env-vars 页面确认："Values: low, medium, high, max (Opus 4.6 only), or auto" | ✅ COMPLETE（描述已更新，包含所有值和优先级说明） |
| 9 | MED | 描述变更 | 修复 `CLAUDE_CODE_ENABLE_TASKS` 描述 — 官方："Set to true to enable task tracking in non-interactive mode (-p flag). Tasks are on by default in interactive mode." 报告当前说 "Set to false to disable" | ✅ COMPLETE（描述已更正以匹配官方文档） |
| 10 | MED | 描述变更 | 更新 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 以注明："Equivalent of setting DISABLE_AUTOUPDATER, DISABLE_FEEDBACK_COMMAND, DISABLE_ERROR_REPORTING, and DISABLE_TELEMETRY" | ✅ COMPLETE（描述已更新，包含等效变量列表） |
| 11 | MED | 示例更新 | 从 Quick Reference 示例中移除 `showTurnDuration` — 根据官方文档不属于 settings.json | ✅ COMPLETE（已从 Quick Reference 示例和 Display & UX 示例中移除） |
| 12 | LOW | 环境变量默认值 | 验证 `MCP_TIMEOUT` 默认值（报告说 10000）— 官方文档没有指定默认值 | ✅ COMPLETE（已移除未经验证的默认值 — 官方文档省略了它） |

---

## [2026-03-19 12:38 PM PKT] Claude Code v2.1.79

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `ANTHROPIC_CUSTOM_MODEL_OPTION`, `ANTHROPIC_CUSTOM_MODEL_OPTION_NAME`, `ANTHROPIC_CUSTOM_MODEL_OPTION_DESCRIPTION` — 用于向 `/model` 选择器添加自定义条目的模型配置变量。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已添加 3 个环境变量到表中 ANTHROPIC_BASE_URL 之后） |
| 2 | HIGH | 描述变更 | 将 `CLAUDE_CODE_PLUGIN_SEED_DIR` 从单数更新为复数："Path to one or more read-only plugin seed directories, separated by `:` on Unix or `;` on Windows"。在 v2.1.79 变更日志中更改。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（描述已更新为多目录支持） |
| 3 | HIGH | Sandbox 路径前缀 | 修复 sandbox.filesystem 路径前缀文档：`/` = absolute（标准 Unix），`./` = project-relative，`//` = legacy 仍然有效。报告当前显示了相反的规定。官方文档明确指出："This syntax differs from Read and Edit permission rules" | ✅ COMPLETE（已更新所有 4 个 sandbox.filesystem 条目为正确的前缀规定，添加了与 Read/Edit 权限规则的交叉引用说明，添加了跨作用域合并详情） |
| 4 | MED | 描述变更 | 扩展 `CLAUDE_CODE_AUTO_COMPACT_WINDOW` 描述 — 当前的 "Auto-compact window behavior configuration" 过于简短。官方文档描述了：token 容量、默认值（200K standard / 1M extended）、与 `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 的交互、状态行解耦 | ✅ COMPLETE（描述已扩展，包含 token 容量、模型默认值、AUTOCOMPACT_PCT 交互和状态行解耦） |

---
## [2026-03-20 08:41 AM PKT] Claude Code v2.1.80

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 MCP Settings 表中添加 `channelsEnabled` — 仅 managed boolean，控制 Team 和 Enterprise 用户的频道消息传递。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 MCP Settings 表 allowManagedMcpServersOnly 之后） |
| 2 | MED | 版本徽章 | 更新报告版本从 v2.1.79 到 v2.1.80 | ✅ COMPLETE（徽章和标题已更新） |

---

## [2026-03-21 09:17 PM PKT] Claude Code v2.1.81

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 (~/.claude.json) | 在 Global Config Settings 表中添加 `autoConnectIde`（boolean，默认 `false`）和 `autoInstallIdeExtension`（boolean，默认 `true`）。已在官方 settings 页面 "Global config settings" 下确认 | ✅ COMPLETE（已添加两个键到 ~/.claude.json 表 showTurnDuration 之前） |
| 2 | HIGH | 错误设置项 | `allow_remote_sessions` 在 Permission Keys 表中列为仅 managed boolean，但官方 permissions 页面指出："Access to Remote Control and web sessions is not controlled by a managed settings key." 标记为未验证或移除 | ✅ COMPLETE（重新添加了未验证注释，附带了官方文档引用和管理员 UI 链接） |
| 3 | MED | 版本号更新 | 更新报告版本徽章从 v2.1.80 到 v2.1.81 | ✅ COMPLETE（徽章、标题版本和标题文本已更新） |
| 4 | MED | 新设置项 | 添加 `showClearContextOnPlanAccept` — 已在 v2.1.81 变更日志中确认。当 `true` 时，恢复计划接受时的 "clear context" 选项（默认隐藏）。尚未在官方 settings 页面上 — 可能是 `~/.claude.json` 键 | ✅ COMPLETE（已添加到 Global Config Settings 表并附带了变更日志来源说明） |
| 5 | MED | 插件文档 | 在 Plugin Settings 部分记录 `source: 'settings'` 作为市场来源类型。官方 settings 页面将其列为 `extraKnownMarketplaces` 的 7 种来源类型之一 | ✅ COMPLETE（已添加全部 7 种来源类型列表，内联市场示例） |
| 6 | MED | 状态行字段 | 在 Status Line Input Fields 表中添加 `rate_limits` 字段组 — 包括 `five_hour.used_percentage`, `five_hour.resets_at`, `seven_day.used_percentage`, `seven_day.resets_at`。已在 v2.1.80 中添加 | ✅ COMPLETE（已添加 4 个 rate_limits 字段到 Status Line Input Fields 表） |

---

## [2026-03-23 10:02 PM PKT] Claude Code v2.1.81

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 (~/.claude.json) | 在 Global Config Settings 表中添加 `editorMode`（string，默认 `"normal"`，值：`"normal"` 或 `"vim"`）。运行 `/vim` 时自动写入。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Global Config Settings 表 autoInstallIdeExtension 之后） |
| 2 | HIGH | 文件作用域修复 | 将 `showClearContextOnPlanAccept` 从 Global Config Settings (~/.claude.json) 移至 General Settings（settings.json）。官方文档现在将其列在主 Available settings 表中，而非 Global config 表。移除过时的注释 "not yet on official settings page" | ✅ COMPLETE（已移至 General Settings 表 feedbackSurveyRate 之前，移除了过时注释） |
| 3 | MED | 描述变更 | 修复 `terminalProgressBarEnabled` 支持的终端从 "Windows Terminal, iTerm2" 为 "ConEmu, Ghostty 1.2.0+, and iTerm2 3.6.6+" 根据官方文档 | ✅ COMPLETE（终端列表已更新） |
| 4 | MED | 描述变更 | 在 `availableModels` 描述中添加 "Config tool" — 官方文档说 "via `/model`, `--model`, Config tool, or `ANTHROPIC_MODEL`"。报告当前省略了 "Config tool" | ✅ COMPLETE（已添加 "Config tool" 到描述） |

---

## [2026-03-25 08:16 PM PKT] Claude Code v2.1.83

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 Permissions 部分添加 `autoMode` — 包含 `environment`, `allow`, `soft_deny` 数组的对象，用于配置 auto mode 分类器。不从共享项目设置中读取（`.claude/settings.json`）。在 user、local 和 managed 设置中可用。已在官方 settings + permissions 页面确认 | ✅ COMPLETE（已添加到 Permission Keys 表，包含完整描述、作用域限制和 `claude auto-mode defaults` 说明） |
| 2 | HIGH | 新设置项 | 在 Permissions 部分添加 `disableAutoMode` — string，设置为 `"disable"` 以阻止 auto mode 激活。从 Shift+Tab 循环中移除 `auto`。可在任何设置级别设置，在 managed 设置中最有用。已在官方 settings + permissions 页面确认 | ✅ COMPLETE（已添加到 Permission Keys 表 `autoMode` 之后） |
| 3 | HIGH | 新权限模式 | 在 Permission Modes 表中添加 `auto` — 后台分类器替代手动提示。研究预览。需要 Team 计划 + Sonnet/Opus 4.6。已在官方 permission-modes 页面确认 | ✅ COMPLETE（已添加到 Permission Modes 表，包含分类器详情和回退行为） |
| 4 | HIGH | 新设置项 | 在 Sandbox Settings 表中添加 `sandbox.failIfUnavailable` — boolean，默认 `false`，当 sandbox 启用但无法启动时退出并报错，而不是在无 sandbox 状态下运行。已在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加到 Sandbox Settings 表 `sandbox.enabled` 之后） |
| 5 | HIGH | 新设置项 | 在 General Settings 表中添加 `disableDeepLinkRegistration` — boolean，阻止 `claude-cli://` 协议处理器注册。已在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加到 General Settings 表 `feedbackSurveyRate` 之前） |
| 6 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` — 设置为 `1` 以从子进程环境（Bash tool、hooks、MCP stdio servers）中清除 Anthropic 和云提供商凭据。已在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已添加到环境变量表 `CLAUDE_CODE_SUBAGENT_MODEL` 之后） |
| 7 | HIGH | 设置层级 | 在 Managed Settings 部分添加 `managed-settings.d/` 投放目录 — 与 `managed-settings.json` 并排的独立策略片段，按字母顺序合并。已在 v2.1.83 变更日志中确认 | ✅ COMPLETE（已作为 managed 设置交付方式下的要点添加） |
| 8 | HIGH | 损坏链接 | 在 Sources 中修复 `https://claudelog.com/configuration/` — 返回 403 Forbidden。移除或替换为可用的源 | ✅ COMPLETE（已替换为已验证可用的 `https://claudelog.com/claude-code-changelog/`） |
| 9 | MED | 版本徽章 | 更新报告版本从 v2.1.81 到 v2.1.83 | ✅ COMPLETE（徽章和标题已在 Phase 2.6 中更新） |
| 10 | MED | 示例更新 | 在 Quick Reference 示例中添加 `autoMode` 以演示 auto mode 分类器配置 | ✅ COMPLETE（已添加包含 `environment` 数组的 `autoMode` 块在 `permissions` 块之前） |
| 11 | MED | 路径变更 | 修复 Windows registry 路径从 `Software\Anthropic\ClaudeCode` 为 `SOFTWARE\Policies\ClaudeCode`（HKLM 和 HKCU）。官方文档已更新为使用 `Policies` 子键 | ✅ COMPLETE（已更新为 `HKLM\SOFTWARE\Policies\ClaudeCode` 和 `HKCU\SOFTWARE\Policies\ClaudeCode`，包含优先级说明） |
| 12 | LOW | 缺失别名 | 在 Model Aliases 表中添加 `opus[1m]` — Opus 4.6 带 1M 上下文，自 v2.1.75 起在 Max/Team/Enterprise 上默认可用 | ✅ COMPLETE（已添加到 Model Aliases 表 `sonnet[1m]` 之后） |

---

## [2026-03-26 01:04 PM PKT] Claude Code v2.1.84

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 General Settings 中添加 `defaultShell` — string，默认 `"bash"`，接受 `"bash"` 或 `"powershell"`。通过 PowerShell 路由交互式 `!` 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 General Settings 表 teammateMode 之后） |
| 2 | HIGH | 新设置项 | 在 MCP Settings 中添加 `allowedChannelPlugins` — array，仅 managed。允许推送消息的频道插件的白名单。设置后替换默认的 Anthropic 白名单。需要 `channelsEnabled: true`。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 MCP Settings 表 channelsEnabled 之后） |
| 3 | HIGH | 新设置项 | 在 Permission Keys 中添加 `useAutoModeDuringPlan` — boolean，默认 `true`。Plan mode 在 auto mode 可用时使用 auto mode 语义。不从共享项目设置中读取。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Permission Keys 表 disableAutoMode 之后） |
| 4 | HIGH | 缺失环境变量 | 添加 9 个模型定制环境变量：`ANTHROPIC_DEFAULT_{OPUS,SONNET,HAIKU}_MODEL_{NAME,DESCRIPTION,SUPPORTED_CAPABILITIES}` 用于 Bedrock/Vertex/Foundry 上的 `/model` 选择器定制。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在每个基础模型变量 Haiku、Opus、Sonnet 之后添加 3 个变量） |
| 5 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` — 当流式传输失败时禁用非流式回退。防止通过代理重复执行工具。已在官方 /en/env-vars 页面确认（v2.1.83 中添加，上一轮遗漏） | ✅ COMPLETE（已在 CLAUDE_CODE_DISABLE_FAST_MODE 之后添加） |
| 6 | HIGH | 缺失环境变量 | 添加 `CLAUDE_CODE_USE_POWERSHELL_TOOL` — 在 Windows 上启用 PowerShell 工具（预览，需选择加入）。仅原生 Windows，不适用于 WSL。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_USE_FOUNDRY 之后添加） |
| 7 | HIGH | 损坏链接 | 在 Sources 中修复 `https://claudelog.com/claude-code-changelog/` — 返回 403 Forbidden。替换为官方 GitHub 变更日志 URL | ✅ COMPLETE（已替换为 github.com/anthropics/claude-code/blob/main/CHANGELOG.md） |
| 8 | MED | 设置层级 | 更新 managed 层优先级："file-based（`managed-settings.d/*.json` + `managed-settings.json`）"并添加 "across tiers" 限定符。根据官方文档添加层内合并说明 | ✅ COMPLETE（已更新优先级描述，包含基于文件的层和跨层限定符） |
| 9 | MED | 设置层级 | 扩展投放目录合并语义：systemd 约定、标量覆盖、数组拼接+去重、深度合并、隐藏文件排除、数字前缀提示。根据官方 settings 页面 | ✅ COMPLETE（已扩展，包含完整的 systemd 约定详情和数字前缀提示） |
| 10 | MED | 注释 | 根据 Rule 1F 逆向完整性检查，在 `disableDeepLinkRegistration` 上添加 "in changelog, not on official settings page" 注释 | ✅ COMPLETE（已添加到描述） |
| 11 | MED | 示例更新 | 在 Quick Reference 示例中添加 `defaultShell` 以演示 PowerShell 配置 | ✅ COMPLETE（已添加 "defaultShell": "bash" 到示例） |

---

## [2026-03-27 06:32 PM PKT] Claude Code v2.1.85

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_STREAM_IDLE_TIMEOUT_MS` — 流式空闲看门狗关闭停滞连接前的超时时间（毫秒）（默认：90000）。已在官方 /en/env-vars 页面确认。在 v2.1.84 中添加但在上一轮中遗漏 | ✅ COMPLETE（已在环境变量表 CLAUDE_CODE_OTEL_HEADERS_HELPER_DEBOUNCE_MS 之后添加） |
| 2 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.84 到 v2.1.85 | ✅ COMPLETE（徽章、标题版本和标题文本已在 Phase 2.6 中更新） |
| 3 | MED | 新环境变量 | 在环境变量表中添加 `OTEL_LOG_TOOL_DETAILS` — 控制 OpenTelemetry 事件中的 `tool_parameters`。仅 v2.1.85 变更日志（尚未在官方 env-vars 页面上）。添加变更日志来源注释 | ✅ COMPLETE（已添加并附有 "in v2.1.85 changelog, not yet on official env-vars page" 注释） |
| 4 | MED | 新环境变量（所有权） | 决定 `CLAUDE_CODE_MCP_SERVER_NAME` 和 `CLAUDE_CODE_MCP_SERVER_URL` 的所有权 — 传递给 MCP `headersHelper` 脚本的环境变量（v2.1.85 变更日志）。可能属于 hooks 仓库而非 settings report | ✅ COMPLETE（已添加到 settings report 并附带变更日志注释 — 这些变量可通过 `env` 键配置环境，并非仅 hooks 专用） |

---

## [2026-03-28 06:10 PM PKT] Claude Code v2.1.86

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 文件作用域 | 将 `teammateMode` 从 General Settings（settings.json）移至 Global Config Settings（~/.claude.json）。官方 settings 页面将其列在 "Global config settings" 下 — 添加到 settings.json 会触发 schema 验证错误（Rule 1H）。与 v2.1.78 `showTurnDuration` 修复模式相同 | ✅ COMPLETE（已从 General Settings 表中移除，添加到 Global Config Settings 表 terminalProgressBarEnabled 之后，并附带 agent-teams 文档链接） |
| 2 | HIGH | 类型+注释 | 修复 `disableDeepLinkRegistration`：类型从 `boolean` 改为 `string`（值：`"disable"`），更新描述以匹配官方文档，移除过时的 "(in changelog, not on official settings page)" 注释。现在已在官方 settings 页面确认（第 169 行） | ✅ COMPLETE（类型已改为 string，描述已更新以匹配官方文档，变更日志注释已移除） |
| 3 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.85 到 v2.1.86 | ✅ COMPLETE（徽章和标题已在 Phase 2.6 中更新） |

---

## [2026-03-31 07:02 PM PKT] Claude Code v2.1.88

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_NO_FLICKER` — 启用无闪烁的 alt-screen 渲染（v2.1.88）。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_DISABLE_TERMINAL_TITLE 之后添加） |
| 2 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_SCROLL_SPEED` 和 `CLAUDE_CODE_DISABLE_MOUSE` — 全屏 UI 控制。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_NO_FLICKER 之后添加） |
| 3 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.86 到 v2.1.88 | ✅ COMPLETE（徽章、标题版本和标题文本已在 Phase 2.6 中更新） |
| 4 | HIGH | 损坏链接 | 在 Sources 中修复 `https://www.eesel.ai/blog/settings-json-claude-code` — 返回仅 CSS 内容，无可读博客文章 | ✅ COMPLETE（已从 Sources 部分移除损坏链接） |
| 5 | MED | 设置层级 | 添加 `managed-mcp.json` 到基于文件的 managed 交付方式 — 官方 settings 页面将其与 `managed-settings.json` 并列列出，用于 MCP 服务器配置 | ✅ COMPLETE（已添加到 Settings Hierarchy 的 File 交付方式要点中） |
| 6 | MED | 插件来源类型 | 将 `url`, `npm`, `file` 市场来源类型注释为 "not in official docs — unverified"（仅 `github`, `git`, `directory`, `hostPattern`, `settings` 已确认） | ✅ COMPLETE（已添加未验证注释到全部 3 种来源类型） |
| 7 | LOW | 标题计数 | 更新标题从 "60+ settings" 以匹配添加后的实际表计数 | ❌ INVALID（计数准确 — 60+ settings 和 125 env vars，均在所述范围内） |

---

## [2026-04-01 12:32 PM PKT] Claude Code v2.1.89

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 在 Permission Keys 表中添加 `skipDangerousModePermissionPrompt` — boolean，跳过 bypass mode 确认提示。在项目设置中被忽略。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Permission Keys 表 disableBypassPermissionsMode 之后） |
| 2 | HIGH | 新设置项 | 在 General Settings 中添加 `showThinkingSummaries` — boolean，默认 `false`。Thinking summaries 默认不再生成；设置为 `true` 以恢复。仅 v2.1.89 变更日志 — 尚未在官方 settings 页面上 | ✅ COMPLETE（已在 feedbackSurveyRate 之前添加并附带变更日志注释） |
| 3 | HIGH | 行为变更 | 更新 `cleanupPeriodDays` 描述 — v2.1.89 变更日志说 `0` 现在会被拒绝并返回验证错误。矛盾之处：官方 settings 页面仍将 `0` 描述为有效。标记给用户 | ✅ COMPLETE（已更新描述，包含变更日志和文档页面之间的矛盾说明） |
| 4 | HIGH | 缺失环境变量 | 添加约 46 个在官方 /en/env-vars 页面确认的缺失环境变量：`ANTHROPIC_BEDROCK_BASE_URL`, `ANTHROPIC_VERTEX_BASE_URL`, `ANTHROPIC_BETAS`, `ANTHROPIC_VERTEX_PROJECT_ID`, `CLAUDE_CODE_DISABLE_THINKING`, `DISABLE_INTERLEAVED_THINKING`, `ENABLE_PROMPT_CACHING_1H_BEDROCK`, `DISABLE_AUTO_COMPACT`, `DISABLE_COMPACT`, `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING`, `CLAUDE_CODE_DISABLE_ATTACHMENTS`, `CLAUDE_CODE_DISABLE_CLAUDE_MDS`, `CLAUDE_CODE_GLOB_HIDDEN`, `CLAUDE_CODE_GLOB_NO_IGNORE`, `CLAUDE_CODE_GLOB_TIMEOUT_SECONDS`, `CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL`, `CLAUDE_CODE_SYNC_PLUGIN_INSTALL`, `CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS`, `CLAUDE_CODE_AUTO_CONNECT_IDE`, `CLAUDE_CODE_IDE_HOST_OVERRIDE`, `CLAUDE_CODE_IDE_SKIP_VALID_CHECK`, `CLAUDE_CODE_MAX_RETRIES`, `API_TIMEOUT_MS`, `CLAUDE_CODE_OTEL_FLUSH_TIMEOUT_MS`, `CLAUDE_CODE_OTEL_SHUTDOWN_TIMEOUT_MS`, `CLAUDE_ENABLE_STREAM_WATCHDOG`, `CLAUDE_CODE_ENABLE_FINE_GRAINED_TOOL_STREAMING`, `CLAUDE_CODE_DEBUG_LOGS_DIR`, `CLAUDE_CODE_DEBUG_LOG_LEVEL`, `CLAUDE_CODE_ACCESSIBILITY`, `CLAUDE_CODE_SYNTAX_HIGHLIGHT`, `CLAUDE_CODE_RESUME_INTERRUPTED_TURN`, `CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY`, `CLAUDE_CODE_DISABLE_LEGACY_MODEL_REMAP`, `FALLBACK_FOR_ALL_PRIMARY_MODELS`, `CLAUDE_CODE_GIT_BASH_PATH`, `CLAUDE_AUTO_BACKGROUND_TASKS`, `CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS`, `CLAUDE_AGENT_SDK_MCP_NO_PREFIX`, `DISABLE_DOCTOR_COMMAND`, `DISABLE_LOGIN_COMMAND`, `DISABLE_LOGOUT_COMMAND`, `DISABLE_UPGRADE_COMMAND`, `DISABLE_EXTRA_USAGE_COMMAND`, `DISABLE_INSTALL_GITHUB_APP_COMMAND`, `CLAUDE_CODE_PLUGIN_CACHE_DIR`, `CLAUDE_CODE_SIMPLE` | ✅ COMPLETE（已添加全部 46 个环境变量到表中相关变量附近） |
| 5 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.88 到 v2.1.89 | ✅ COMPLETE（徽章和标题已在 Phase 2.6 中更新） |
| 6 | MED | 新环境变量 | 在环境变量表中添加 `MCP_CONNECTION_NONBLOCKING` — 在 `-p` 模式下设置为 `true` 以跳过 MCP 连接等待。仅 v2.1.89 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 CLAUDE_AGENT_SDK_MCP_NO_PREFIX 之后添加并附带变更日志注释） |
| 7 | MED | 所有权边界 | `CLAUDE_CODE_SIMPLE` 在 CLI startup flags 文件中被列为仅 startup，但官方 /en/env-vars 页面将其列为可配置。调和所有权 | ✅ COMPLETE（已添加到 settings report env 表；更新 CLI 文件以交叉引用 settings report） |
| 8 | MED | 示例更新 | 如果添加了 `showThinkingSummaries`，则更新 Quick Reference 示例 | ✅ COMPLETE（已添加 showThinkingSummaries: true 到示例） |

---

## [2026-04-02 09:24 PM PKT] Claude Code v2.1.90

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 类型+描述变更 | 修复 `forceLoginOrgUUID`：类型从 `string` 改为 `string \| string[]`。扩展描述以包含数组行为（任何列出的组织均可接受而无需预选）、managed 设置强制（如果账户不在列出的组织中则登录失败）、以及空数组 fail-closed 行为 | ✅ COMPLETE（类型已更新为 string \| array，描述已扩展包含数组行为、managed 强制、fail-closed 语义，示例已更新） |
| 2 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_OAUTH_TOKEN`, `CLAUDE_CODE_OAUTH_REFRESH_TOKEN`, `CLAUDE_CODE_OAUTH_SCOPES`。全部已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 ANTHROPIC_AUTH_TOKEN 之后添加 3 个 OAuth 环境变量） |
| 3 | HIGH | 描述+注释变更 | 更新 `showThinkingSummaries`：移除 "(in v2.1.89 changelog, not yet on official settings page)" 注释 — 现在已在官方 settings 页面确认。更新描述以匹配官方："When unset or false (default in interactive mode), thinking blocks are redacted by the API and shown as a collapsed stub. Redaction only changes what you see, not what the model generates" | ✅ COMPLETE（注释已移除，描述已更新以匹配官方文档） |
| 4 | HIGH | Sandbox 跨合并 | 更新 `sandbox.filesystem.allowWrite` 描述，添加 "Also merged with paths from `Edit(...)` allow permission rules"。更新 `denyWrite` 添加 "Also merged with paths from `Edit(...)` deny permission rules"。更新 `denyRead` 添加 "Also merged with paths from `Read(...)` deny permission rules"。已在官方 settings 页面确认 | ✅ COMPLETE（跨合并行为已添加到所有 3 个文件系统条目） |
| 5 | HIGH | 描述变更 | 简化 `cleanupPeriodDays` 描述：移除矛盾说明，与官方文档对齐，官方现在说 "minimum 1, Setting to 0 is rejected with a validation error"。旧行为不再在官方页面上记录 | ✅ COMPLETE（矛盾说明已移除，描述已与官方文档对齐，添加了 --no-session-persistence 替代方案） |
| 6 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.89 到 v2.1.90 | ✅ COMPLETE（徽章、标题版本和标题文本已更新） |
| 7 | MED | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` — 在 git pull 失败时保留市场缓存（v2.1.90 变更日志，尚未在官方 /en/env-vars 页面上） | ✅ COMPLETE（已在 CLAUDE_CODE_SYNC_PLUGIN_INSTALL_TIMEOUT_MS 之后添加并附带变更日志注释） |
| 8 | MED | Hooks 重定向计数 | 更新重定向文本从 "all 19 hook events" 为 "all 25 hook events" 根据官方 hooks 页面计数 | ✅ COMPLETE（hooks 重定向部分中的计数已更新） |
| 9 | MED | 所有权边界 | `CLAUDE_CODE_TMPDIR` 在官方 /en/env-vars 页面上列为可通过 `env` 键配置，但 CLI startup flags 报告将其列为仅 startup。调和所有权 | ✅ COMPLETE（已添加到 settings report env 表；更新 CLI flags 文件以交叉引用 settings report） |

---

## [2026-04-03 08:44 PM PKT] Claude Code v2.1.91

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 General Settings 表中添加 `disableSkillShellExecution` — boolean，禁用技能、自定义斜杠命令和插件命令中的内联 shell 执行。已在 v2.1.91 变更日志中确认。尚未在官方 settings 页面或 JSON schema 中 | ✅ COMPLETE（已在 showThinkingSummaries 之后添加并附带变更日志注释） |
| 2 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.90 到 v2.1.91 | ✅ COMPLETE（徽章和标题已在 Phase 2.6 中更新） |

---

## [2026-04-04 10:48 PM PKT] Claude Code v2.1.92

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 General Settings 中添加 `forceRemoteSettingsRefresh` — boolean，仅 managed，阻塞 CLI 启动直到远程 managed 设置被新鲜获取（fail-closed）。已在官方 settings 页面确认 | ✅ COMPLETE（已在 General Settings 表 feedbackSurveyRate 之前添加） |
| 2 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX` — 自动生成的 Remote Control session 名称前缀，默认为机器主机名。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_ENABLE_TELEMETRY 之前添加） |
| 3 | MED | 描述变更 | 更新 `disableSkillShellExecution` — 移除 "(in v2.1.91 changelog, not yet on official settings page)" 注释。现在已在官方 settings 页面确认并附有扩展描述 | ✅ COMPLETE（注释已移除，描述已根据官方文档扩展） |
| 4 | MED | 描述变更 | 从市场来源类型 `url`、`npm` 和 `file` 移除 "not in official docs — unverified" 标签。官方 settings 页面现在记录了全部 8 种来源类型 | ✅ COMPLETE（未验证注释已移除 — 自 2026-03-31 起持续，现已解决） |
| 5 | MED | 描述变更 | 丰富 `cleanupPeriodDays` — 添加 "Also controls the age cutoff for automatic removal of orphaned subagent worktrees at startup" 根据官方 settings 页面 | ✅ COMPLETE（已添加 worktree 清理详情） |
| 6 | MED | 描述变更 | 丰富 `disableDeepLinkRegistration` — 添加通过 `%0A` 的多行提示支持根据官方 settings 页面 | ✅ COMPLETE（已添加多行提示详情） |
| 7 | MED | 描述变更 | 丰富 `includeGitInstructions` — 更新为包含 git status snapshot 和环境变量优先级根据官方 settings 页面 | ✅ COMPLETE（描述已扩展，包含 git status snapshot 和 CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS 优先级） |
| 8 | MED | 描述变更 | 丰富 `language` — 添加 "Also sets the voice dictation language" 根据官方 settings 页面 | ✅ COMPLETE（已添加语音输入语言详情） |
| 9 | MED | 描述变更 | 丰富 `allowUnsandboxedCommands` — 根据官方 settings 页面添加企业策略详情 | ✅ COMPLETE（已扩展，包含 fail-closed 行为和企业用例） |

---

## [2026-04-08 09:51 PM PKT] Claude Code v2.1.96

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_USE_MANTLE`, `ANTHROPIC_BEDROCK_MANTLE_BASE_URL`, `CLAUDE_CODE_SKIP_MANTLE_AUTH` — Bedrock Mantle 端点支持（v2.1.94）。全部已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在相关云提供商变量附近添加） |
| 2 | HIGH | 默认值变更 | 更新 Effort Level 部分 — 默认值从 Medium 改为 High，适用于 API-key、Bedrock/Vertex/Foundry、Team 和 Enterprise 用户（v2.1.94）。更新表默认标记和历史说明 | ✅ COMPLETE（表已更新 High 为默认值，历史说明已扩展包含 v2.1.94 变更） |
| 3 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.92 到 v2.1.96 | ✅ COMPLETE（徽章、标题版本和标题文本已在 Phase 2.6 中更新） |
| 4 | MED | 过时注释 | 从 `CLAUDE_CODE_PLUGIN_KEEP_MARKETPLACE_ON_FAILURE` 移除 "(in v2.1.90 changelog, not yet on official env-vars page)" — 现在已在官方 /en/env-vars 页面确认。更新描述以匹配官方措辞 | ✅ COMPLETE（注释已移除，描述已根据官方文档更新） |
| 5 | MED | 描述变更 | 更新 `CLAUDE_CODE_GLOB_HIDDEN` 描述以匹配官方："Set to `false` to exclude dotfiles from Glob results. Included by default. Does not affect `@` file autocomplete, `ls`, Grep, or Read" | ✅ COMPLETE（描述已根据官方 env-vars 页面重写） |

---

## [2026-04-09 11:39 PM PKT] Claude Code v2.1.97

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 Sandbox Settings 表中添加 `sandbox.network.allowMachLookup` — array，仅 macOS，XPC/Mach 服务名称支持尾随 `*` 通配符。已在官方 settings 页面确认 | ✅ COMPLETE（已在 sandbox network 子键的 allowManagedDomainsOnly 之后添加） |
| 2 | HIGH | 显示与 UX | 在 Status Line Configuration 部分添加 `refreshInterval` 字段 — 可选，每 N 秒重新运行命令，最小值为 1（v2.1.97）。已在官方 status line 文档中确认 | ✅ COMPLETE（已添加到配置表及其 `padding` 字段，已更新 JSON 示例） |
| 3 | HIGH | 显示与 UX | 将 Status Line Input Fields 表从 9 个扩展到 30+ 个字段以匹配官方 status line 文档。添加 `model.*`, `workspace.*`, `cost.*`, `session_id`, `session_name`, `transcript_path`, `version`, `output_style.name`, `vim.mode`, `agent.name`, `worktree.*` 字段 | ✅ COMPLETE（已从 9 个扩展到 30 个字段根据官方 status line 文档） |
| 4 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.96 到 v2.1.97 | ✅ COMPLETE（徽章和标题已在 Phase 2.6 中更新） |
| 5 | MED | 字段命名 | 在 Status Line Input Fields 表中修复 `current_usage` → `context_window.current_usage` | ✅ COMPLETE（已重命名并附有完整路径和扩展描述） |
| 6 | MED | 所有权边界 | 为 `claude-cli-startup-flags.md` 添加 `CCR_FORCE_BUNDLE` — 用于 `claude --remote` 打包的仅 startup 变量。在官方 /en/env-vars 页面上但不在任一文件中 | ✅ COMPLETE（已添加到 CLI startup flags 环境变量表） |
| 7 | MED | 描述变更 | 更新 `CLAUDE_CODE_GLOB_NO_IGNORE` 描述以匹配官方："Set to `false` to make the Glob tool respect `.gitignore` patterns. By default, Glob returns all matching files including gitignored ones. Does not affect `@` file autocomplete" | ✅ COMPLETE（描述已根据官方 env-vars 页面重写） |
| 8 | MED | 描述变更 | 更新 `editorMode` 描述 — 移除过时的 `/vim` 引用（在 v2.1.94 中移除），将配置标签从 "Key binding mode" 改为 "Editor mode" 根据官方文档 | ✅ COMPLETE（已移除 /vim 引用，配置标签已更新） |

---

## [2026-04-13 08:10 PM PKT] Claude Code v2.1.101

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_CERT_STORE` — 逗号分隔的 CA 证书来源用于 TLS（`bundled`, `system`）。默认值：`bundled,system`。需要原生二进制用于系统存储。v2.1.101。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_CLIENT_KEY_PASSPHRASE 之后添加） |
| 2 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_PERFORCE_MODE` — 设置为 `1` 以启用 Perforce 感知的写入保护。如果目标文件缺少 owner-write 位，Edit/Write/NotebookEdit 将失败并显示 `p4 edit` 提示。v2.1.98。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_SCRIPT_CAPS 之后添加） |
| 3 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_SCRIPT_CAPS` — JSON 对象，限制每个 session 的脚本调用次数，当设置了 `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` 时生效。键是与命令文本匹配的子字符串；值是整数调用限制。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_SUBPROCESS_ENV_SCRUB 之后添加） |
| 4 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.97 到 v2.1.101 | ✅ COMPLETE（徽章、标题版本和标题文本已在 Phase 2.6 中更新） |
| 5 | MED | 描述变更 | 更新 `disableSkillShellExecution` — 添加 ` ```! `（三反引号 shell）块语法和 "from user, project, plugin, or additional-directory sources" 限定符根据官方 settings 页面 | ✅ COMPLETE（描述已根据官方文档扩展） |
| 6 | MED | 所有权边界 | 将 `DISABLE_AUTOUPDATER` 添加到 settings report 环境变量表 — 在官方 /en/env-vars 页面上作为可通过 `env` 键配置，目前仅在 CLI startup flags 文件中。添加并交叉引用 CLI flags 文件 | ✅ COMPLETE（已在 settings report 中 DISABLE_TELEMETRY 之前添加；CLI flags 文件已更新带交叉引用） |

---

## [2026-04-14 11:22 PM PKT] Claude Code v2.1.107

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 在 General Settings 表中添加 `viewMode` — string，值 `"default"`, `"verbose"`, `"focus"`。启动时的默认 transcript 视图模式，覆盖粘性 Ctrl+O 选择。已在官方 settings 页面确认 | ✅ COMPLETE（已在 General Settings 中 showClearContextOnPlanAccept 之后添加） |
| 2 | HIGH | 缺失环境变量 | 添加 5 个在官方 /en/env-vars 页面确认的缺失环境变量：`ANTHROPIC_CUSTOM_MODEL_OPTION_SUPPORTED_CAPABILITIES`, `CLAUDE_CODE_DISABLE_VIRTUAL_SCROLL`, `CLAUDE_ENABLE_BYTE_WATCHDOG`, `CLAUDE_CODE_MAX_CONTEXT_TOKENS`, `CLAUDE_CODE_SKIP_PROMPT_HISTORY` | ✅ COMPLETE（已在环境变量表中相关变量附近添加） |
| 3 | HIGH | 描述变更 | 更新 `disableAllHooks` 描述 — 添加 "and any custom status line" 根据官方 settings 页面第 180 行 | ✅ COMPLETE（已在 hooks 重定向部分内联更新） |
| 4 | HIGH | 默认值变更 | 修复 Global Config Settings 表中 `teammateMode` 默认值从 `"in-process"` 为 `"auto"`。官方文档将 `auto` 描述为主要行为。在 v2.1.86 文件作用域移动期间回归 | ✅ COMPLETE（默认值已更新为 "auto" — 自 2026-03-07 起持续，v2.1.86 移动导致的回归） |
| 5 | MED | 描述变更 | 更新 `CLAUDE_STREAM_IDLE_TIMEOUT_MS` 描述 — 区分字节看门狗（默认/最小 300000ms）和事件看门狗（默认 90000ms）。根据官方 /en/env-vars 页面 | ✅ COMPLETE（描述已扩展，包含双看门狗详情和 CLAUDE_ENABLE_BYTE_WATCHDOG 交叉引用） |
| 6 | MED | 注释修复 | 从 `CLAUDE_CODE_GIT_BASH_PATH` 移除 "(startup-only)" — 官方 /en/env-vars 页面将其列为 env 可配置 | ✅ COMPLETE（描述已根据官方文档重写，startup-only 注释已移除） |
| 7 | MED | 示例更新 | 在 Quick Reference 示例中 `showThinkingSummaries` 之后添加 `viewMode` | ✅ COMPLETE（已添加 "viewMode": "default" 到示例） |
| 8 | MED | 过时注释 | `OTEL_LOG_TOOL_DETAILS` 仍标记为 "in v2.1.85 changelog, not yet on official env-vars page" — 在 10+ 版本和 7 次连续运行后确认仍不在官方页面上 | ✋ ON HOLD（注释准确 — 保持原样等待官方文档更新） |
| 7 | MED | 所有权边界 | 将 `CCR_FORCE_BUNDLE` 添加到 settings report 环境变量表 — 在官方 /en/env-vars 页面上作为可通过 `env` 键配置，目前仅在 CLI startup flags 文件中。添加并交叉引用 CLI flags 文件 | ✅ COMPLETE（已在 settings report 中 CLAUDE_CODE_GIT_BASH_PATH 之前添加；CLI flags 文件已更新带交叉引用） |

---

## [2026-04-16 08:25 PM PKT] Claude Code v2.1.110

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 缺失设置项 | 在 General Settings 表中添加 `minimumVersion` — string，阻止自动更新器降级到特定版本以下。已在官方 settings 页面确认 | ✅ COMPLETE（已在 General Settings 表 autoUpdatesChannel 之后添加） |
| 2 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_TMUX_TRUECOLOR` — 设置为 `1` 以允许在 tmux 内输出 24 位真彩色。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_CODE_NO_FLICKER 之前添加） |
| 3 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_REMOTE` — 只读，在云 session 中设置为 `true`。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX 之前添加） |
| 4 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_REMOTE_SESSION_ID` — 只读，云 session ID。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 CLAUDE_REMOTE_CONTROL_SESSION_NAME_PREFIX 之前添加） |
| 5 | HIGH | 逆向环境变量检查 | 将 `ENABLE_PROMPT_CACHING_1H_BEDROCK` 标记为 "not in official docs — unverified"。不再在官方 /en/env-vars 页面上。根据 Rule 5D | ✅ COMPLETE（已添加未验证注释并附有弃用说明） |
| 6 | MED | 新设置项（变更日志） | 在 General Settings 中添加 `autoScrollEnabled` — boolean，在全屏模式下禁用对话自动滚动。仅 v2.1.110 变更日志，尚未在官方 settings 页面上 | ✅ COMPLETE（已在 feedbackSurveyRate 之前添加并附带变更日志注释） |
| 7 | MED | 新设置项（变更日志） | 在 General Settings 中添加 `tui` — 用于无闪烁渲染模式的设置（`/tui fullscreen`）。仅 v2.1.110 变更日志，尚未在官方 settings 页面上 | ✅ COMPLETE（已在 feedbackSurveyRate 之前添加并附带变更日志注释） |
| 8 | MED | 新环境变量（变更日志） | 在环境变量表中添加 `ENABLE_PROMPT_CACHING_1H` — 1 小时提示缓存 TTL（替换已弃用的 `ENABLE_PROMPT_CACHING_1H_BEDROCK`）。仅 v2.1.108 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 DISABLE_PROMPT_CACHING 之前添加并附带变更日志注释） |
| 9 | MED | 新环境变量（变更日志） | 在环境变量表中添加 `FORCE_PROMPT_CACHING_5M` — 强制使用 5 分钟 TTL。仅 v2.1.108 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 DISABLE_PROMPT_CACHING 之前添加并附带变更日志注释） |
| 10 | MED | Effort Level 表 | 在 Effort Level 表中添加 `Max` 行 — 仅 Opus 4.6，在环境变量中有记录但表中缺失 | ✅ COMPLETE（已添加为 Effort Level 表的首行） |
| 11 | MED | Sandbox 描述 | 添加平台特定说明：`allowUnixSockets`（仅 macOS），`allowAllUnixSockets`（Linux/WSL2 详情），`enableWeakerNestedSandbox`（仅 Linux/WSL2） | ✅ COMPLETE（全部 3 个描述已更新，包含平台特定说明根据官方文档） |
| 12 | LOW | 仅变更日志环境变量 | 考虑添加 `CLAUDE_CODE_ENABLE_AWAY_SUMMARY`（v2.1.108），`OTEL_LOG_USER_PROMPTS`（v2.1.101），`OTEL_LOG_TOOL_CONTENT`（v2.1.101）— 全部仅变更日志，不在官方 env-vars 页面上。根据 Rule 8A 推迟直到官方文档确认 | ✋ ON HOLD（已推迟 — 仅变更日志，未获官方文档确认） |

---

## [2026-04-18 07:56 PM PKT] Claude Code v2.1.114

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.110 到 v2.1.114，标题 "As of v2.1.110" → "As of v2.1.114" | ✅ COMPLETE（徽章和标题文本已更新） |
| 2 | HIGH | 新设置项 | 在 General Settings 表中添加 `awaySummaryEnabled` — boolean，控制是否生成空闲 session 回顾（"away summary"）。已在官方 settings 页面确认。与 `CLAUDE_CODE_ENABLE_AWAY_SUMMARY` 环境变量配对 | ✅ COMPLETE（已添加到 General Settings 表 `tui` 和 `feedbackSurveyRate` 之间并附带配对说明） |
| 3 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_ENABLE_AWAY_SUMMARY` — 选择退出 away summary/session recap。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 `FORCE_PROMPT_CACHING_5M` 之后添加 — 持续问题已解决，首次出现 2026-04-16） |
| 4 | HIGH | 缺失值 | 向 `effortLevel` 有效值添加 `xhigh`（第 497 行）— v2.1.111 为 Opus 4.7 引入了 `xhigh`。当前仅列出了 `"low"`, `"medium"`, `"high"` | ✅ COMPLETE（描述已更新，包含 `"xhigh"` 值、Opus 4.7 支持和回退行为） |
| 5 | HIGH | Effort Level 表 | 在 Effort Level 表中添加 `xhigh` 行（在 Max 和 High 之间）— 仅 Opus 4.7，在 v2.1.111 中引入 | ✅ COMPLETE（XHigh 行已添加；默认标记已更新为 "default on Opus 4.6/Sonnet 4.6"；Note 部分已扩展包含 v2.1.111 xhigh 在 Opus 4.7 上的默认值） |
| 6 | HIGH | 文件作用域移动 | 将 `autoScrollEnabled` 从 General Settings（第 88 行）移至 Global Config Settings（`~/.claude.json`）表 — 官方文档将其列为 `~/.claude.json` 键，而不是 `settings.json`。默认 `true`。根据 Rule 1H。添加到 settings.json 可能触发 schema 验证错误 | ✅ COMPLETE（已从 General Settings 移除，已添加到 Global Config Settings 表 `autoInstallIdeExtension` 和 `editorMode` 之间，默认值 `true`） |
| 7 | HIGH | 过时注释 | 从 `tui` 描述（第 89 行）移除 "(in v2.1.110 changelog, not yet on official settings page)" — 现在已官方记录。根据官方文档更新描述：`"fullscreen"` 或 `"default"` | ✅ COMPLETE（注释已移除，描述已更新包含值和 v2.1.110 引用） |
| 8 | HIGH | 新设置项 | 在 Global Config Settings（`~/.claude.json`）表中添加 `externalEditorContext` — 已在官方 settings 页面 "Global config settings" 部分确认。根据 Rule 1A | ✅ COMPLETE（已添加到 Global Config Settings 表 `editorMode` 之后，默认值 `true`） |
| 9 | HIGH | 过时注释 | 从 `sandbox.network.deniedDomains`（第 388 行）移除 "(not in official docs — unverified)" — 在 v2.1.113 变更日志中官方添加。根据官方文档更新描述（优先于 `allowedDomains`）。根据 Rule 10B，在 11 次连续 ON HOLD 运行后解除阻塞 | ✅ COMPLETE（注释已移除，描述已重写包含优先级和 glob 支持说明 — 持续问题已解决，首次出现 2026-03-05） |
| 10 | MED | 示例更新 | 更新 Quick Reference 示例（第 938-1023 行）以包含 `awaySummaryEnabled`, `tui: "fullscreen"` 和 `effortLevel: "xhigh"` 以展示 v2.1.111–v2.1.114 的新增内容 | ✅ COMPLETE（全部 3 个键已添加到示例；`effortLevel` 已从 `"medium"` 提升为 `"xhigh"`） |
| 11 | MED | 标题设置计数 | 验证在添加 `awaySummaryEnabled` 和 `externalEditorContext` 后 "60+ settings" 计数仍然准确；环境变量计数 "175+" 仍然合理 | ✅ COMPLETE（两个计数仍在所述范围内 — 净增 +2 个设置，+1 个环境变量，标题准确为 "60+" 和 "175+"） |
| 12 | LOW | 交叉链接 | 为 `CLAUDE_CODE_SIMPLE` 和 `CLAUDE_CODE_EFFORT_LEVEL` 在 settings report 和 `claude-cli-startup-flags.md` 之间添加双向交叉链接。目前是单向的；startup-flags 文件链接到 settings report 但 settings report 没有链接回去 | ✅ COMPLETE（已为两个环境变量添加了指向 `./claude-cli-startup-flags.md#environment-variables` 的交叉引用链接） |
| 13 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 11+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B：考虑解决 — 要么通过官方文档确认，要么移除。当前 agent 研究确认仍不在官方文档中 | ✋ ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续，注释仍然准确） |
| 14 | LOW | 可疑键复发 | `OTEL_LOG_USER_PROMPTS`, `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ✋ ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |

---

## [2026-04-24 12:27 AM PKT] Claude Code v2.1.118

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 添加 `wslInheritsWindowsSettings` 到仅 managed 设置（仅 Windows HKLM registry / `C:\Program Files\ClaudeCode\managed-settings.json` — WSL 继承 Windows 策略链）。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 General Settings 表 `forceRemoteSettingsRefresh` 之后，包含完整描述和 v2.1.118 归属） |
| 2 | HIGH | 行为变更 | 更新 `autoMode` 描述以记录 `"$defaults"` 哨兵 — 允许在內建规则旁边添加自定义规则而非替换它们（v2.1.118）。已在官方 settings 页面确认 | ✅ COMPLETE（描述已更新 — 哨兵继承行为已根据官方文档记录，包含 v2.1.118 归属） |
| 3 | HIGH | 新环境变量（变更日志） | 在环境变量表中添加 `DISABLE_UPDATES` — 完全阻止所有更新路径，包括手动 `claude update`（比 `DISABLE_AUTOUPDATER` 更严格）。仅 v2.1.118 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 `DISABLE_AUTOUPDATER` 之后添加，附带仅变更日志注释） |
| 4 | MED | 过时可疑键 | 从 Permission Modes 表中移除 `askEdits` 和 `viewOnly` 行 — 官方 permissions 页面仅列出 6 种模式（`default`, `acceptEdits`, `plan`, `auto`, `dontAsk`, `bypassPermissions`）。报告自 v2.1.74（2026-03-12）起将两者标记为 "unverified"。根据 Rule 10B（5+ 次 ON HOLD 升级），通过移除解决 | ✅ COMPLETE（两行均已移除 — 持续问题已解决，首次出现 2026-03-12） |
| 5 | MED | 过时可疑键 | 从 Permission Keys 表中移除 `allow_remote_sessions` — 官方 permissions 页面明确指出 "Access to Remote Control and web sessions is not controlled by a managed settings key." 管理员通过 Claude Code 管理员设置 UI 管理此功能。首次添加于 v2.1.74（2026-03-12）作为推测性 managed 设置；此后未再记录 | ✅ COMPLETE（已从 Permission Keys 表中移除行 — 持续问题已解决，首次出现 2026-03-12） |
| 6 | MED | 行为变更 | 更新 `cleanupPeriodDays` 描述 — v2.1.117 将清除范围扩展到也清理 `~/.claude/tasks/`、`~/.claude/shell-snapshots/` 和 `~/.claude/backups/`（以前仅 transcripts 和孤立 worktrees） | ✅ COMPLETE（描述已重写以覆盖完整的清除范围，包含 v2.1.117 归属） |
| 7 | MED | Effort Level 说明 | 在 Effort Level 部分添加 v2.1.117 Pro/Max 默认 effort 变更 — Pro/Max 订阅者在 Opus 4.6 和 Sonnet 4.6 上的默认值从 `medium` 改为 `high`。当前说明仅提到较早的 v2.1.94 变更（适用于 API-key/Bedrock/Vertex/Foundry/Team/Enterprise 用户） | ✅ COMPLETE（Effort Level Note 已扩展，包含 v2.1.117 Pro/Max 对齐说明） |
| 8 | LOW | 示例更新 | 更新 Quick Reference 示例以展示 v2.1.118 功能 — 要么 `wslInheritsWindowsSettings` 要么 `autoMode.soft_deny` 中的 `"$defaults"` 哨兵 | ✅ COMPLETE（已添加 `"soft_deny": ["$defaults", "Never run terraform apply"]` 到 Quick Reference 的 autoMode 块） |
| 9 | LOW | 新环境变量（变更日志） | 考虑在环境变量表中添加 `CLAUDE_CODE_FORK_SUBAGENT` — 在外部构建上启用 forked 子代理。仅 v2.1.117 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 `OTEL_LOG_RAW_API_BODIES` 之后添加，附带仅变更日志注释） |
| 10 | LOW | 新环境变量（变更日志） | 考虑在环境变量表中添加 `OTEL_LOG_RAW_API_BODIES` — 将完整 API 请求/响应体作为 OpenTelemetry 日志事件发出。仅 v2.1.111 变更日志，尚未在官方 /en/env-vars 页面上 | ✅ COMPLETE（已在 `OTEL_LOG_TOOL_DETAILS` 之后添加，附带仅变更日志注释） |
| 11 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 12+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ✋ ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 12 | LOW | 可疑键复发 | `OTEL_LOG_USER_PROMPTS`, `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ✋ ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 13 | INVALID | 虚假漂移声明 | `workflow-claude-settings-agent` 报告 `sandbox.allowUnsandboxedCommands` 默认值错误（声称文档说 `false`）。与官方 settings 页面核实 — 记录的默认值是 **`true`**。报告保持原样正确 | ❌ INVALID（agent 报告与重新核实后的官方文档相矛盾） |

---

## [2026-04-26 01:10 PM PKT] Claude Code v2.1.119

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.118 到 v2.1.119，标题 "As of v2.1.118" → "As of v2.1.119" | ✅ COMPLETE（徽章、标题版本和标题文本已在 Phase 2.6 中更新） |
| 2 | HIGH | 新设置项 | 在 Attribution Settings 表中添加 `prUrlTemplate` — string，URL 模板，控制提交归属中的 PR 徽章如何链接到拉取请求 UI。适用于自托管 GitLab/Bitbucket/GitHub Enterprise 实例。已在 v2.1.119 变更日志中确认 | ✅ COMPLETE（已添加到 Attribution Settings 表 `attribution.pr` 和 `includeCoAuthoredBy` 之间，包含 v2.1.119 归属和自托管用例） |
| 3 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 `CLAUDE_CODE_HIDE_CWD` — 设置为 `1` 以隐藏启动徽标横幅中的当前工作目录。在屏幕录制或 CWD 路径敏感的共享 session 中有用。已在 v2.1.119 变更日志中确认 | ✅ COMPLETE（已在 `CLAUDE_CODE_DISABLE_MOUSE` 之后添加到环境变量表，与其他 UI/显示变量分组） |
| 4 | HIGH | 行为变更 | 更新 `auto` 权限模式描述（第 247 行）— 移除过时的 `--enable-auto-mode` 标志引用（该标志在 v2.1.111 中已移除）。根据官方 permissions 文档，当前描述是："Auto-approves tool calls with background safety checks that verify actions align with your request. Currently a research preview." Auto mode 现在在默认的 `Shift+Tab` 循环中 | ✅ COMPLETE（描述已使用官方措辞重写；`--enable-auto-mode` 标志引用已移除；说明了自 v2.1.111 起的 Shift+Tab 循环包含情况和 `--permission-mode auto` 作为当前入口点） |
| 5 | MED | 行为变更 | 更新 Plugin Settings 中的 `blockedMarketplaces` 描述 — 说明 v2.1.119 对 `hostPattern` 和 `pathPattern` 匹配的强制执行。被阻止的源现在在下载到达文件系统之前正确拒绝 | ✅ COMPLETE（描述已扩展，包含 hostPattern/pathPattern 匹配器和 v2.1.119 预下载强制实施详情） |
| 6 | MED | 语音设置扩展 | 将 `voiceEnabled`（boolean，第 81 行）扩展为根据 v2.1.118 记录的完整 `voice` 对象 — 支持 `enabled`（boolean）、`mode`（`"hold"` 或 `"tap"`）和 `autoSubmit`（boolean）。保留 `voiceEnabled` 作为遗留别名并附上已弃用标记 | ✅ COMPLETE（已添加包含全部 3 个字段的新 `voice` 对象行；`voiceEnabled` 行已更新为指向 `voice` 对象的已弃用遗留别名） |
| 7 | MED | 新子命令 | 在 Useful Commands 表中添加 `claude plugin tag` — 在 v2.1.118 中添加，用于在市场中对插件版本打标签 | ✅ COMPLETE（已在 Useful Commands 表中 `/plugin` 之后添加，包含 v2.1.118 归属和从市场仓库运行的使用说明） |
| 8 | LOW | 来源 URL 漂移 | `https://json.schemastore.org/claude-code-settings.json` 现在 301 重定向到 `https://www.schemastore.org/claude-code-settings.json`。v2.1.74 #4 明确将其修正为另一个方向。来源仍然通过重定向解析 — 决定是否翻转或保留重定向 | ❌ INVALID（URL 仍然通过 301 重定向正确解析；翻转将与 v2.1.74 #4 的修正来回振荡且没有功能收益。仅在 schemastore 弃用重定向时重新评估） |
| 9 | LOW | MCP OAuth 说明 | 在 MCP Servers 部分添加简短的 MCP OAuth RFC 9728 说明 — 在 v2.1.111 中添加。仅供参考，非设置键 | ✅ COMPLETE（已添加块引用标注在 MCP Settings 表上方，包含 RFC 9728 链接、`/.well-known/oauth-protected-resource` 发现端点和说明兼容服务器不再需要 `apiKeyHelper`/`headersHelper`） |
| 10 | LOW | Quick Reference 更新 | 添加新键后，在 Quick Reference 示例的 attribution 块之后添加 `prUrlTemplate` | ✅ COMPLETE（已添加 `"prUrlTemplate": "https://gitlab.example.com/{owner}/{repo}/-/merge_requests/{number}"` 在示例的 `attribution` 块之后） |
| 11 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 13+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ✋ ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 12 | LOW | 可疑键复发 | `OTEL_LOG_USER_PROMPTS`, `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ✋ ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 13 | INVALID | 虚假漂移声明 | `claude-code-guide` agent 报告 `attribution.pr` 是新的 v2.1.119 设置。与当前报告（第 149 行）核实 — 已在 Attribution Settings 表中记录 | ❌ INVALID（已在报告中） |
| 14 | INVALID | 虚假漂移声明 | `claude-code-guide` agent 声称 `sandbox.network.deniedDomains` 是在 v2.1.116 中添加的。Workflow agent 和报告（第 386 行）都确认是 v2.1.113 引入（匹配最近的 v2.1.114 变更日志条目，该条目解决了其之前的未验证状态）。报告值保留 | ❌ INVALID（agent 与特定报告 agent + 先前变更日志相矛盾） |

---

## [2026-04-29 12:49 AM PKT] Claude Code v2.1.121

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.119 → v2.1.121，标题 "As of v2.1.119" → "As of v2.1.121" | ✅ COMPLETE（徽章已在 Phase 2.6 中更新，标题文本已更新为 v2.1.121） |
| 2 | HIGH | 新设置项 | 添加 `sshConfigs` 到新的 Workspace & Teams 子部分（object[]，必需：`id`/`name`/`sshHost`；可选：`sshPort`/`sshIdentityFile`/`startDirectory`）。在 Desktop 中提供 SSH 连接下拉菜单。根据官方 settings 页面第 14 部分 | ✅ COMPLETE（已添加新的 Workspace & Teams 子部分，包含 Key 表、字段引用表和 JSON 示例） |
| 3 | HIGH | 新 MCP 选项 | 在 MCP Settings 中添加 `alwaysLoad` — 每个服务器的 boolean 选项，免于工具搜索延迟；适用于所有服务器类型；需要 v2.1.121+。每个工具的变体通过 `_meta: {"anthropic/alwaysLoad": true}`。已在官方 MCP 页面上确认 | ✅ COMPLETE（已在 MCP Servers 下添加新的 "Per-Server Tool Loading" 子部分，包含原理说明、JSON 示例和每个工具的 `_meta` 变体） |
| 4 | HIGH | 状态行字段 | 在 Status Line Input Fields 表中添加 `effort.level`（low/medium/high/xhigh/max）和 `thinking.enabled`（v2.1.121）。已在官方 statusline 文档中确认 | ✅ COMPLETE（两个字段已在 `agent.name` 行之后添加，包含 v2.1.121 归属） |
| 5 | HIGH | 文件作用域迁移 | 将 `autoScrollEnabled`, `editorMode`, `showTurnDuration`, `teammateMode`, `terminalProgressBarEnabled` 从 Global Config Settings（`~/.claude.json`）表移回主 Display Settings 表（settings.json）。添加历史说明："Versions before v2.1.119 stored these in `~/.claude.json`"。逆转了 v2.1.78/v2.1.86/v2.1.107/v2.1.114 的 Rule 1H 发现 — 官方文档现在明确将它们列在 "Available settings" 下并附有历史说明 | ✅ COMPLETE（5 个键已添加到 Display Settings 表，每个键附有历史说明；已从 Global Config Settings 表中移除；Global Config Settings 前言中添加了 v2.1.119 迁移说明） |
| 6 | HIGH | 缺失环境变量 | 在环境变量表中添加 `AI_AGENT` — Claude Code 注入到子进程的环境变量（类似于 `CLAUDECODE`）。仅 v2.1.120 变更日志 — 相应注释 | ✅ COMPLETE（已在 `CLAUDECODE` 行之后添加，附带仅变更日志注释） |
| 7 | HIGH | 缺失环境变量 | 在环境变量表中添加 `OTEL_LOG_USER_PROMPTS` — 控制 OTel LLM 请求跨度中的 `user_system_prompt` 字段。仅 v2.1.121 变更日志。**持续问题**（首次出现 2026-04-16 v2.1.110）— 之前根据 Rule 8A 推迟，但现在根据新的 v2.1.121 变更日志提及而可操作 | ✅ COMPLETE（已在 `OTEL_LOG_RAW_API_BODIES` 之后添加，附带仅变更日志注释 — 持续问题已解决，首次出现 2026-04-16） |
| 8 | MED | 权限行为 | 在 Permissions 部分添加 v2.1.121 `--dangerously-skip-permissions` 排除说明。重新与官方 permission-modes 文档核实：变化在于对 `.claude/commands/`、`.claude/agents/`、`.claude/skills/` 和 `.claude/worktrees/` 的写入在 bypassPermissions 下免于受保护路径提示 — 即这些子目录现在自动批准，与 agent 最初的表述相反 | ✅ COMPLETE（描述已添加到 Permission Modes 表的 `bypassPermissions` 行，根据官方文档修正了表述 — 是豁免而非限制） |
| 9 | MED | 描述变更 | 更新 `language` 设置描述 — v2.1.121 也将语言应用于终端标签标题 | ✅ COMPLETE（终端标签标题行为已附加到 `language` 描述，包含 v2.1.121 归属） |
| 10 | MED | Useful Commands | 更新 `/effort` 行以包含 `xhigh` 值（目前仅列出 `low`/`medium`/`high`） | ✅ COMPLETE（`/effort` 行现在列出 `low`、`medium`、`high`、`xhigh`（仅 Opus 4.7，v2.1.111）和 `max`（仅 Opus 4.6）） |
| 11 | LOW | 技能变量说明 | 添加简短的 `${CLAUDE_EFFORT}` 技能模板变量说明（v2.1.120）— 仅供参考，严格来说不是设置键 | ✅ COMPLETE（已添加到 Effort Level Note 部分作为 "Skill template variable" 句子，包含 v2.1.120 归属） |
| 12 | LOW | 示例更新 | 更新 Quick Reference 示例以展示 v2.1.121 功能（MCP `alwaysLoad`、`sshConfigs` 或新的状态行字段） | ✅ COMPLETE（已将包含 `alwaysLoad: true` 的 `mcpServers` 块和 `sshConfigs` 块添加到 Quick Reference 示例） |
| 13 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 14+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ✋ ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 14 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ✋ ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 15 | HIGH | 损坏链接 | 修复第 237 和 238 行的两个 `[auto mode](/en/permission-modes#eliminate-prompts-with-auto-mode)` 链接（在 `autoMode` 和 `disableAutoMode` 描述中）— 相对路径缺少域名前缀。替换为 `https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode`。锚点在官方 permission-modes 页面上验证有效 | ✅ COMPLETE（两个链接已更新为绝对 `https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode` URL） |

---

## [2026-05-01 03:29 PM PKT] Claude Code v2.1.126

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.121 → v2.1.126，标题 "As of v2.1.121" → "As of v2.1.126" | ✅ COMPLETE（徽章已在 Phase 2.6 中更新，正文标题文本已更新为 v2.1.126） |
| 2 | HIGH | 新设置项 | 在 Display Settings 表中添加 `preferredNotifChannel` — string，默认 `"auto"`，值：`"auto"`, `"terminal_bell"`, `"iterm2"`, `"iterm2_with_bell"`, `"kitty"`, `"ghostty"`, `"notifications_disabled"`。任务完成和权限提示通知的方法。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Display Settings 表 `terminalProgressBarEnabled` 之后，包含完整枚举值、默认值和 `/en/terminal-config` 交叉链接） |
| 3 | HIGH | 新环境变量 | 在环境变量表中添加 `ANTHROPIC_BEDROCK_SERVICE_TIER` — Bedrock 服务层级（`default`, `flex` 或 `priority`）；作为 `X-Amzn-Bedrock-Service-Tier` 标头发送。v2.1.122。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 `ANTHROPIC_BEDROCK_MANTLE_BASE_URL` 之后添加，包含 v2.1.122 归属和 `/en/amazon-bedrock#service-tiers` 交叉链接） |
| 4 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` — 由嵌入 Claude Code 的主机平台设置；设置时忽略 settings.json 中的提供者/认证环境变量；遥测遵循标准 `DISABLE_TELEMETRY` 选择退出而非在 Bedrock/Vertex/Foundry 上自动禁用。v2.1.126。已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（已在 `ANTHROPIC_BEDROCK_SERVICE_TIER` 之后添加，包含完整描述、忽略变量列表和 v2.1.126 归属） |
| 5 | MED | 权限模式 | 更新 `bypassPermissions` 描述（第 248 行）— v2.1.126 将豁免扩展到也绕过对 `.claude/`、`.git/`、`.vscode/` 和 shell 配置文件的写入。灾难性移除命令仍然提示。在 v2.1.121 `.claude/commands/`、`.claude/agents/`、`.claude/skills/`、`.claude/worktrees/` 豁免的基础上构建 | ✅ COMPLETE（描述已扩展，包含对 `.claude/`、`.git/`、`.vscode/` 和 shell 配置文件的 v2.1.126 豁免；保留了灾难性移除安全网） |
| 6 | MED | 描述变更 | 丰富 `defaultShell` 描述 — v2.1.126：当 PowerShell 启用时（`CLAUDE_CODE_USE_POWERSHELL_TOOL=1`），它被视为**主要**shell。v2.1.120：当 Git for Windows 不可用时，PowerShell 是后备 shell。同时说明 v2.1.126 PowerShell 7 检测（Microsoft Store、不带 PATH 的 MSI、.NET global tool） | ✅ COMPLETE（描述已丰富，包含 v2.1.120 回退行为、v2.1.126 主要 shell 切换和 PowerShell 7 检测来源） |
| 7 | LOW | spinnerTipsOverride 说明 | 可选地丰富 `spinnerTipsOverride.excludeDefault` 描述（第 580 行）包含 v2.1.121 详情 "suppresses time-based spinner tips"。当前根据官方 settings 页面措辞准确但缺少 v2.1.121 变更日志优化 | ✅ COMPLETE（描述已扩展，包含官方文档中的 `excludeDefault` 语义和 v2.1.121 基于时间的提示抑制优化） |
| 8 | LOW | /config 持久化说明 | 在 Settings Hierarchy 部分添加简短说明，`/config` 现在将更改持久化到 `~/.claude/settings.json`（v2.1.126）。仅供参考，非新键 | ✅ COMPLETE（已在 Settings Hierarchy 的 v2.1.75 Windows 路径说明之后添加 v2.1.126 `> Note` 块） |
| 9 | LOW | 示例更新 | 更新 Quick Reference 示例以展示 v2.1.122–v2.1.126 功能 — `preferredNotifChannel` 和 `ANTHROPIC_BEDROCK_SERVICE_TIER` 环境示例 | ✅ COMPLETE（已在 `prefersReducedMotion` 之后添加 `"preferredNotifChannel": "terminal_bell"`，并在 `env` 块中添加了 `"ANTHROPIC_BEDROCK_SERVICE_TIER": "priority"`） |
| 10 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 16+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ✋ ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 11 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ✋ ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 12 | INVALID | 虚假漂移声明 | `workflow-claude-settings-agent` 报告 `autoSummaryEnabled` 是与 `awaySummaryEnabled` 分开的设置（高置信度声明）。直接与官方 settings 页面核实 — `autoSummaryEnabled` 不存在；只有 `awaySummaryEnabled` 有文档记录 | ❌ INVALID（agent 与直接文档核实相矛盾） |
| 13 | INVALID | 虚假漂移声明 | `workflow-claude-settings-agent` 声称 `Agent` 权限规则语法应为 `Agent(agent:name)` 带 `agent:` 前缀。与官方 settings 页面核实 — 仅引用 `Agent` 规则而未显示 `agent:` 前缀语法。报告现有的 `Agent(name)` 形式匹配既定惯例；没有来源确认前缀变体 | ❌ INVALID（没有来源验证的 `agent:` 前缀证据） |

---

## [2026-05-09 06:58 PM PKT] Claude Code v2.1.138

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.126 → v2.1.138，标题 "As of v2.1.126" → "As of v2.1.138" | ✅ COMPLETE（徽章已在 Phase 2.6 中更新；正文标题第 6 行已更新为 v2.1.138） |
| 2 | HIGH | 新设置项 | 在 Worktree Settings 中添加 `worktree.baseRef` — string，值 `"fresh"` 或 `"head"`，控制新 worktree 是从新的主 HEAD 分支还是从当前 HEAD 分支。已在 v2.1.133 变更日志中确认 | ✅ COMPLETE（已在 `worktree.sparsePaths` 之后添加，默认值 `"fresh"`，包含 v2.1.133 归属） |
| 3 | HIGH | 新设置项 | 在 Sandbox 表中添加 `sandbox.bwrapPath` — string（仅 Linux/WSL managed-only），`bwrap`（bubblewrap）二进制文件的自定义路径。已在 v2.1.133 变更日志中确认 | ✅ COMPLETE（已在 `sandbox.enableWeakerNetworkIsolation` 之后添加，附带仅 managed 注释） |
| 4 | HIGH | 新设置项 | 在 Sandbox 表中添加 `sandbox.socatPath` — string（仅 Linux/WSL managed-only），`socat` 二进制文件的自定义路径。已在 v2.1.133 变更日志中确认 | ✅ COMPLETE（已在 `sandbox.bwrapPath` 之后添加，附带仅 managed 注释） |
| 5 | HIGH | 行为变更 | 更新 `autoMode` 描述以记录新的 `hard_deny` 数组 — auto-mode 分类器规则，无条件阻止，是 `allow` 和 `soft_deny` 的兄弟。已在 v2.1.136 变更日志中确认 | ✅ COMPLETE（描述已扩展，包含 `hard_deny` 语义、哨兵不兼容说明和 v2.1.136 归属） |
| 6 | HIGH | 新设置项 | 在 Settings Hierarchy 部分添加 `parentSettingsBehavior`（仅 managed）— string `"first-wins"` 或 `"merge"`，控制 SDK `managedSettings` 父级如何合并。已在 v2.1.133 变更日志中确认 | ✅ COMPLETE（已添加到 Settings Hierarchy 下新的 "Dynamic & Parent-Tier Policy" 子部分） |
| 7 | HIGH | 新设置项 | 添加 `policyHelper`（仅 managed）到 managed-policy 子部分 — object 包含 `path`、`timeoutMs`、`refreshIntervalMs`。动态计算 managed 设置的 managed 可执行文件。已在 v2.1.136 变更日志中确认 | ✅ COMPLETE（已添加到新的 "Dynamic & Parent-Tier Policy" 子部分，包含字段引用和用例说明） |
| 8 | HIGH | 新设置项 | 在 General Settings 中添加 `skillOverrides` — string `"off"` / `"user-invocable-only"` / `"name-only"`，控制自动技能调用行为。已在 v2.1.129 变更日志中确认 | ✅ COMPLETE（已在 `disableSkillShellExecution` 之后添加，包含全部 3 个枚举值和 v2.1.129 归属） |
| 9 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` — 为启用了 OpenTelemetry 的企业重新启用 session 质量反馈调查。仅 v2.1.136 变更日志 | ✅ COMPLETE（已在 display/UI 环境变量集群中 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` 之后添加） |
| 10 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_SESSION_ID` — 注入到 Bash 子进程环境中的当前 session ID。仅 v2.1.132 变更日志 | ✅ COMPLETE（已在 `CLAUDECODE` 之后添加，附带只读注释和 v2.1.132 归属） |
| 11 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` — 选择退出全屏渲染器；使用经典回滚。仅 v2.1.132 变更日志 | ✅ COMPLETE（已在 `CLAUDE_CODE_DISABLE_VIRTUAL_SCROLL` 之后添加） |
| 12 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_FORCE_SYNC_OUTPUT` — 强制同步输出（调试辅助）。仅 v2.1.129 变更日志 | ✅ COMPLETE（已在 `CLAUDE_CODE_HIDE_CWD` 之后添加） |
| 13 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` — 控制后台包管理器自动更新行为。仅 v2.1.129 变更日志 | ✅ COMPLETE（已在 `CLAUDE_CODE_FORCE_SYNC_OUTPUT` 之后添加，附带对 `DISABLE_AUTOUPDATER` 的交叉引用） |
| 14 | HIGH | 新环境变量 | 在环境变量表中添加 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` — 选择加入从 LLM 网关获取可用模型。仅 v2.1.129 变更日志 | ✅ COMPLETE（已在 `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` 之后添加） |
| 15 | MED | 新环境变量（Hook 输入） | 在环境变量表中添加 `$CLAUDE_EFFORT` — 暴露活跃 effort level 的 Bash 子进程和 hook 输入环境（与 `CLAUDE_CODE_EFFORT_LEVEL` 配合）。Hooks 也接收 `effort.level` JSON 字段。仅 v2.1.133 变更日志 | ✋ ON HOLD（推迟 — 用户将此运行的范围限定为仅 HIGH 优先级项目；将在下次运行中处理） |
| 16 | MED | 插件市场 | 添加简短说明，插件市场 `source: 'settings'` 现在支持直接在 settings.json 中的内联插件条目（仅 v2.1.137 变更日志） | ON HOLD（等待用户批准） |
| 17 | MED | MCP 保留名称 | 在 MCP Servers 部分添加 `workspace` 到 "Reserved server names" 说明 — v2.1.128 为此名称保留给 workspace MCP 集成 | ON HOLD（等待用户批准） |
| 18 | MED | 缺失设置项 | 将 `disableRemoteControl` 添加到 Permissions/Managed-only 设置 — 官方 permissions 文档明确指出 "Remote Control can additionally be disabled per device with the `disableRemoteControl` managed setting" | ON HOLD（等待用户批准） |
| 19 | MED | 缺失设置项 | 将 `claudeMdExcludes` 添加到 Core Configuration — array，跳过匹配 glob 的 CLAUDE.md 文件。列在官方 settings 页面上 | ON HOLD（等待用户批准） |
| 20 | MED | 缺失设置项 | 将 `autoMemoryEnabled` 添加到 Core Configuration — boolean（默认 `true`），启用 auto memory。当前仅列出了 `autoMemoryDirectory` | ON HOLD（等待用户批准） |
| 21 | MED | 示例更新 | 更新 Quick Reference 示例以展示 v2.1.129–v2.1.138 功能 — `worktree.baseRef`、`autoMode.hard_deny`、`skillOverrides` 以及一个新的环境变量 | ON HOLD（等待用户批准） |
| 22 | LOW | 标题计数 | 更新标题声明从 "60+ settings" → "80+ settings" 以更好地反映添加后的实际官方计数 | ON HOLD（等待用户批准） |
| 23 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 17+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 24 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 25 | INVALID | 虚假漂移声明 | `claude-code-guide` agent 将 `disableSkillShellExecution` 列为 v2.1.137 中的新内容。与当前报告（第 89 行）核实 — 已在 General Settings 表中记录 | ❌ INVALID（已在报告中；预先存在的键） |
| 26 | INVALID | 虚假漂移声明 | `workflow-claude-settings-agent` 标记 `syntaxHighlightingDisabled` 为缺失的 settings.json 键。无法在官方文档上确认它是 `settings.json` 键（环境变量 `CLAUDE_CODE_SYNTAX_HIGHLIGHT` 有记录，但 settings-key 形式未获得来源验证）。根据 Rule 8A | ❌ INVALID（没有来源验证的 evidence 表明 settings.json 键存在） |
| 14 | INVALID | 虚假漂移声明 | `workflow-claude-settings-agent` 将 Core Configuration 中的 `model` 默认值（`"default"`）和 `language` 默认值（`"english"`）标记为外观错误，因为官方文档显示 `-`。报告的值是描述未设置时行为的说明性占位符；翻转为 `-` 会丢失信息而没有用户可见的收益 | ❌ INVALID（外观重新核实没有用户可见的收益） |

---

## [2026-05-12 11:40 PM PKT] Claude Code v2.1.139

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 新设置项 | 将 `autoMode.hard_deny` 字段添加到 `autoMode` 描述（v2.1.136）— 无条件阻止规则，不能被 `allow` 例外或 `$defaults` 哨兵覆盖。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 `autoMode` 字段列表描述） |
| 2 | HIGH | 新设置项 | 在 Worktree Settings 表中添加 `worktree.baseRef`（`"fresh" \| "head"`，默认 `"fresh"`，v2.1.133）— 新 worktree 从哪个 ref 分支。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Worktree Settings 表 `worktree.sparsePaths` 之后） |
| 3 | HIGH | 新设置项 | 在 Sandbox Settings 表中添加 `sandbox.bwrapPath` 和 `sandbox.socatPath`（仅 managed，Linux/WSL2，v2.1.133）— bubblewrap 和 socat 二进制文件的绝对路径，覆盖 `PATH` 检测。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Sandbox Settings 表 `enableWeakerNetworkIsolation` 之后） |
| 4 | HIGH | 新设置项 | 添加 `parentSettingsBehavior`（`"first-wins" \| "merge"`，仅 managed，默认 `"first-wins"`，v2.1.133）— 控制当管理员部署的 managed 层也存在时 SDK 宿主管 managed 设置如何应用。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Settings Hierarchy 下新的 "Managed-only policy keys" 子部分） |
| 5 | HIGH | 新设置项 | 添加 `policyHelper`（`{path: string}`，仅通过 MDM 或文件 managed，v2.1.136）— 管理员部署的可执行文件，在启动时动态计算 managed 设置。已在官方 settings 页面确认 | ✅ COMPLETE（已在 Managed-only policy keys 子部分中与 `parentSettingsBehavior` 一起添加） |
| 6 | HIGH | 新设置项 | 在 General Settings 表中添加 `skillOverrides`（object，v2.1.129）— 每个技能的可见性（`"on" / "name-only" / "user-invocable-only" / "off"`）。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 General Settings 表 `awaySummaryEnabled` 和 `disableRemoteControl` 之间） |
| 7 | HIGH | 新设置项 | 在 General Settings 表中添加 `disableRemoteControl`（boolean，任何作用域，v2.1.128）— 阻止 `claude remote-control`、`--remote-control` 标志、自动启动和 session 内切换。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 General Settings 表 `feedbackSurveyRate` 之前） |
| 8 | HIGH | 新设置项 | 在 AWS & Cloud Credentials 部分添加 `gcpAuthRefresh`（string script）— 在 GCP ADC 过期时刷新它的自定义脚本。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Authentication Helpers 表 `forceLoginOrgUUID` 之后） |
| 9 | HIGH | 缺失环境变量 | 在 Common Environment Variables 表中添加 6 个环境变量：`CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL`（v2.1.136）、`CLAUDE_CODE_SESSION_ID`（v2.1.132）、`CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN`（v2.1.132）、`CLAUDE_CODE_FORCE_SYNC_OUTPUT`（v2.1.129）、`CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE`（v2.1.129）、`CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY`（v2.1.129）。全部已在官方 /en/env-vars 页面确认 | ✅ COMPLETE（全部 6 个环境变量已添加到 Common Environment Variables 表的分组部分） |
| 10 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.126 → v2.1.139，标题 "As of v2.1.126" → "As of v2.1.139" | ✅ COMPLETE（徽章已在 Phase 2.6 中更新；正文标题文本已更新；环境变量计数从 175+ 提升到 180+） |
| 11 | MED | 权限语法 | 向 Tool Permission Syntax 添加 `Skill(name *)` 通配符前缀匹配说明（v2.1.139）— 镜像 `Bash(ls *)` 行为，允许像 `Skill(weather *)` 这样的模式匹配 `weather-fetcher`、`weather-svg-creator` | ✅ COMPLETE（Tool Permission Syntax 表中的 Skill 行已更新，包含前缀匹配示例和 v2.1.139 归属） |
| 12 | MED | 权限行为 | 添加 v2.1.136 plan-mode 说明：即使存在匹配的 `Edit(...)` 允许规则，写入也会被阻止 | ✅ COMPLETE（Permission Modes 表中的 plan-mode 行已更新，包含 v2.1.136 覆盖允许规则的说明） |
| 13 | MED | MCP 保留名称 | 添加 `Workspace` 保留 MCP 服务器名称说明（v2.1.128）— 名为 "workspace" 的现有服务器将被跳过并发出警告 | ✅ COMPLETE（已在 MCP Servers 部分标题下添加块引用） |
| 14 | MED | Effort 变量作用域 | 扩展 `${CLAUDE_EFFORT}` 技能模板说明：v2.1.133 也将环境变量传递给 Bash 工具子进程和 hook 处理器（不仅是技能模板） | ✅ COMPLETE（技能模板变量说明已重写为 "Effort env propagation"，包含 v2.1.133 Bash + hook 作用域详情） |
| 15 | MED | 示例更新 | 更新 Quick Reference 示例以包含 `worktree.baseRef`、`autoMode.hard_deny` 和 `skillOverrides` 以展示 v2.1.129–v2.1.136 功能 | ✅ COMPLETE（全部 3 个键已添加到 Quick Reference 示例） |
| 16 | LOW | Useful Commands | 在 Useful Commands 表中添加 `claude plugin prune`（v2.1.121）和 `claude plugin details`（v2.1.139） | ✅ COMPLETE（两个命令已在 `claude plugin tag` 之后添加） |
| 17 | LOW | Sandbox 脚注 | 添加 v2.1.139 `autoAllowBashIfSandboxed` shell 扩展修复说明 — 现在正确处理 `$VAR` 和 `$(cmd)` | ✅ COMPLETE（Sandbox Settings 表中的描述已扩展） |
| 18 | LOW | MCP 改进 | 在 MCP Servers 部分添加 v2.1.139 `.mcp.json` 热重载（通过 `/mcp` Reconnect）和 `CLAUDE_PROJECT_DIR` MCP stdio 注入说明 | ✅ COMPLETE（已合并为 MCP Servers 部分下的单个块引用） |
| 19 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 17+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ✋ ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 20 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ✋ ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 21 | INVALID | 虚假漂移声明 | `claude-code-guide` 列出 effort 值为 `"fast"`, `"balanced"`, `"thorough"`。与官方 /en/env-vars 核实 — 有效值为 `low`, `medium`, `high`, `xhigh`, `max`, `auto`。报告保留 | ❌ INVALID（agent 与官方文档相矛盾） |
| 22 | INVALID | 虚假漂移声明 | `claude-code-guide` 声称 `CLAUDE_EFFORT` 是一个有记录的环境变量。已核实 — 只有 `CLAUDE_CODE_EFFORT_LEVEL` 存在于 /en/env-vars 上。`CLAUDE_EFFORT` 仅变更日志（子进程注入，非用户可配置）。报告保留 | ❌ INVALID（agent 与官方文档相矛盾） |

---

## [2026-05-21 12:07 AM PKT] Claude Code v2.1.145

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH | 版本号更新 | 更新报告版本徽章从 v2.1.139 → v2.1.145，标题 "As of v2.1.139" → "As of v2.1.145" | ✅ COMPLETE（标题文本已更新为 v2.1.145；徽章已在 Phase 2.6 中同步） |
| 2 | HIGH | 新设置项 | 在 Core Configuration 中添加 `claudeMdExcludes`（array，要跳过的 CLAUDE.md 文件的 glob 模式）— 列在官方 settings 页面上。持续问题（首次出现 2026-05-09 v2.1.138 #19，被推迟到仅 HIGH 作用域） | ✅ COMPLETE（已添加到 General Settings 表并附带官方措辞） |
| 3 | HIGH | 新设置项 | 在 Core Configuration 中添加 `autoMemoryEnabled`（boolean，默认 `true`，启用自动 memory 读/写）— 当前仅记录了 `autoMemoryDirectory`。持续问题（首次出现 2026-05-09 v2.1.138 #20） | ✅ COMPLETE（已添加到 Plans & Memory Directories 表 `autoMemoryDirectory` 旁边） |
| 4 | HIGH | 新 Worktree 设置 | 在 Worktree Settings 表中添加 `worktree.bgIsolation`（string，`"worktree" \| "none"`，默认 `"worktree"`）— `"none"` 让后台 session 直接编辑工作副本（v2.1.143）。已在官方 settings 页面确认 | ✅ COMPLETE（已添加到 Worktree Settings 表并附带官方措辞） |
| 5 | HIGH | 新环境变量 | 在 Common Environment Variables 表中添加 `ANTHROPIC_WORKSPACE_ID` — 工作负载身份联邦，限定铸造令牌的作用域（v2.1.139/v2.1.141）。在变更日志中确认 | ✅ COMPLETE（已添加到环境变量表 — 本轮确认在官方 /en/env-vars 页面上，非仅变更日志） |
| 6 | HIGH | 缺失权限模式 | 确认/保留 `dontAsk` 和 `auto` 模式；验证 `acceptEdits` 描述与官方 permissions 页面措辞匹配（"Automatically accepts file edits and common filesystem commands `mkdir`, `touch`, `mv`, `cp`, etc. for paths in the working directory or `additionalDirectories`"）— 报告的当前描述较短 | ✅ COMPLETE（acceptEdits 描述已扩展为官方措辞） |
| 7 | MED | 权限行为 | 根据官方 permissions 页面用 v2.1.144/v2.1.145 详情更新 `bypassPermissions` 描述："Removals targeting the filesystem root or home directory such as `rm -rf /` and `rm -rf ~` still prompt as a circuit breaker"（当前报告说 "Catastrophic removal commands still prompt"） | ✅ COMPLETE（bypassPermissions 断路器措辞已更新为官方文本） |
| 8 | MED | 权限语法 | 在 Tool Permission Syntax 表中添加 PowerShell 工具权限规则 — 官方 permissions 页面记录了与 Bash 相同形状的 `PowerShell(cmd *)` 规则、别名规范化、AST 基复合命令检查。当前不在报告的工具语法表中 | ✅ COMPLETE（已添加 PowerShell 行，包含别名规范化和 AST 复合命令说明） |
| 9 | MED | 权限语义 | 将复合命令/进程包装器语义添加到 Bash 通配符说明 — 官方 permissions 页面记录了包装器剥离（`timeout`, `time`, `nice`, `nohup`, `stdbuf`, 裸 `xargs`）、exec 包装器提示（`watch`, `setsid`, `ionice`, `flock`, `find -exec`/`-delete`）以及 `&&`/`||`/`;`/`|` 的每个子命令匹配 | ✅ COMPLETE（复合命令+进程包装器要点已添加到 Bash 通配符说明） |
| 10 | MED | 权限语义 | 将符号链接解析行为添加到 Read/Edit 路径模式 — 官方 permissions 页面：允许规则要求符号链接路径和目标都匹配；拒绝规则如果任一匹配则应用 | ✅ COMPLETE（符号链接解析说明已在 Read/Edit 前缀表之后添加） |
| 11 | MED | 新环境变量（Hook 输入） | 在环境变量表中添加 `$CLAUDE_EFFORT` — 暴露活跃 effort level 的 Bash 子进程和 hook 输入环境（与 `CLAUDE_CODE_EFFORT_LEVEL` 配合）。持续问题（首次出现 2026-05-09 v2.1.138 #15，被推迟） | ✅ COMPLETE（已添加到环境变量表；确认不在官方 /en/env-vars 页面上 — 根据 Rule 5D 注释为只读/仅变更日志） |
| 12 | MED | 新环境变量（变更日志） | 在环境变量表中添加 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` — 设置为 `1` 以使用 Opus 4.6 进行 fast mode（v2.1.142 将 fast-mode 默认值改为 Opus 4.7）。仅 v2.1.142 变更日志 | ✅ COMPLETE（已添加到环境变量表 — 本轮确认在官方 /en/env-vars 页面上，非最初标记的仅变更日志） |
| 13 | MED | 新环境变量（变更日志） | 在环境变量表中添加 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY` — 设置为 `1` 以选择退出 PowerShell 工具传递 `-ExecutionPolicy Bypass`（v2.1.143）。仅变更日志 | ✅ COMPLETE（已添加到环境变量表 — 本轮确认在官方 /en/env-vars 页面上，非最初标记的仅变更日志） |
| 14 | MED | 新环境变量（变更日志） | 在环境变量表中添加 `CLAUDE_CODE_PLUGIN_PREFER_HTTPS` — 通过 HTTPS 克隆 GitHub 插件源（v2.1.141）。仅变更日志 | ✅ COMPLETE（已添加到环境变量表 — 本轮确认在官方 /en/env-vars 页面上，非最初标记的仅变更日志） |
| 15 | MED | 插件市场 | 添加简短说明，插件市场 `source: 'settings'` 支持直接在 settings.json 中的内联插件条目（v2.1.137）。持续问题（首次出现 2026-05-09 v2.1.138 #16） | ✅ COMPLETE（已验证已在 Plugin Settings + 内联示例中记录；无需更改） |
| 16 | MED | 状态行 | 添加 `terminalSequence` hook 输出字段引用，并说明状态行 JSON 现在在检测到时包含 GitHub 仓库 + PR 信息（v2.1.145）到 Status Line 部分 | ✅ COMPLETE（github 状态行输入字段已添加；terminalSequence 是 hook 输出字段，根据 Rule 4A 推迟到 claude-code-hooks 仓库） |
| 17 | MED | 模型别名 | 验证 Model Aliases 表反映 Opus 4.7 / fast-mode-default-Opus-4.7（v2.1.142）和 `/model` 仅 session 内更改行为（v2.1.144，按 `d` 设置默认值）。已存在的 `opus[1m]` 在 Max/Team/Enterprise 上的默认说明；添加 `/model` 更改仅 session 作用域的说明 | ✅ COMPLETE（已添加 /model 仅 session 作用域说明 v2.1.144；opus 别名保留为 4.6 — 版本取决于平台：Anthropic API 上为 4.7，Bedrock/Vertex 上为 4.6） |
| 18 | MED | Useful Commands | 在 Useful Commands 中添加 `/usage-credits`（在 v2.1.144 中从 `/extra-usage` 重命名；旧名称仍有效） | ✅ COMPLETE（/usage-credits 已添加到 Useful Commands） |
| 19 | MED | 环境变量重命名 | 注释 `DISABLE_EXTRA_USAGE_COMMAND` — `/extra-usage` 在 v2.1.144 中重命名为 `/usage-credits`；验证现在是否存在 `DISABLE_USAGE_CREDITS_COMMAND` 别名 | ✅ COMPLETE（已注释 DISABLE_EXTRA_USAGE_COMMAND 的重命名；本轮未观察到单独的 DISABLE_USAGE_CREDITS_COMMAND 变量） |
| 20 | LOW | 标题计数 | 更新标题声明从 "60+ settings" → "80+ settings" 以反映添加后的实际官方计数。持续问题（首次出现 2026-05-09 v2.1.138 #22） | ✅ COMPLETE（标题计数已更新 60+ → 80+） |
| 21 | LOW | 示例更新 | 更新 Quick Reference 示例以展示 v2.1.140–v2.1.145 功能（例如，`worktree.bgIsolation`、`claudeMdExcludes` 或 `ANTHROPIC_WORKSPACE_ID` 环境示例） | ✅ COMPLETE（Quick Reference 现在展示 claudeMdExcludes + worktree.bgIsolation） |
| 22 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_DETAILS` 在 18+ 次连续运行后仍标记为 "in v2.1.85 changelog, not yet on official env-vars page"。根据 Rule 10B，推迟等待官方文档更新 | ON HOLD（保留 — 自 2026-04-14 v2.1.107 起持续） |
| 23 | LOW | 可疑键复发 | `OTEL_LOG_TOOL_CONTENT` 仍仅为变更日志。根据 Rule 8A 推迟 | ON HOLD（保留 — 自 2026-04-16 v2.1.110 起持续） |
| 24 | INVALID | 所有权边界（无操作） | 交叉检查了 `claude-cli-startup-flags.md` 中的所有环境变量（CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS, CLAUDE_CODE_TMPDIR, CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD, DISABLE_AUTOUPDATER, CLAUDE_CODE_EFFORT_LEVEL, USE_BUILTIN_RIPGREP, CLAUDE_CODE_SIMPLE, CLAUDE_BASH_NO_LOGIN, CCR_FORCE_BUNDLE）与 settings report。未发现无交叉引用的重复；共享变量正确携带双向交叉引用链接 | ❌ INVALID（无边界违规 — Rule 5B 通过） |
| 25 | INVALID | 超链接验证（无操作） | 所有报告超链接已验证：12 个 ToC 锚点解析到标题；外部 URL（settings、env-vars、permissions、schemastore via 301、feiskyer 仓库、shipyard 博客、GitHub 变更日志）均返回有效页面。`json.schemastore.org` 仍 301 重定向到 `www.schemastore.org`（可用）— 根据 v2.1.119 #8 决定保持原样 | ❌ INVALID（无损坏链接 — Rules 9A/9B/9C 通过） |
