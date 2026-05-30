# 命令报告 — 更新日志历史

## 状态图例

| 状态 | 含义 |
|--------|---------|
| ✅ `已完成 (原因)` | 已采取行动并成功解决 |
| ❌ `无效 (原因)` | 发现结果不正确、不适用或属于预期行为 |
| ✋ `已搁置 (原因)` | 操作已推迟 — 等待外部依赖或用户决策 |

---

## [2026-03-13 04:23 PM PKT] Claude Code v2.1.74

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增字段 | 在 frontmatter 表中添加 `name` — 该 skill 的显示名称 | ❌ 无效 (仅适用于 skill 的字段，不适用于命令 frontmatter) |
| 2 | 高 | 新增字段 | 在 frontmatter 表中添加 `disable-model-invocation` — 禁止自动加载 | ❌ 无效 (仅适用于 skill 的字段，不适用于命令 frontmatter) |
| 3 | 高 | 新增字段 | 在 frontmatter 表中添加 `user-invocable` — 从 `/` 菜单中隐藏 | ❌ 无效 (仅适用于 skill 的字段，不适用于命令 frontmatter) |
| 4 | 高 | 新增字段 | 在 frontmatter 表中添加 `context` — fork 在 subagent 上下文中运行 | ❌ 无效 (仅适用于 skill 的字段，不适用于命令 frontmatter) |
| 5 | 高 | 新增字段 | 在 frontmatter 表中添加 `agent` — context: fork 的 subagent 类型 | ❌ 无效 (仅适用于 skill 的字段，不适用于命令 frontmatter) |
| 6 | 高 | 新增字段 | 在 frontmatter 表中添加 `hooks` — 作用于 skill 的生命周期 hook | ❌ 无效 (仅适用于 skill 的字段，不适用于命令 frontmatter) |
| 7 | 高 | 新增命令 | 添加 `/btw <question>` — 快速询问一个边角问题，不加入对话 | ✅ 已完成 (已添加到 Session tag 中的 #53) |
| 8 | 高 | 新增命令 | 添加 `/hooks` — 管理工具事件的 hook 配置 | ✅ 已完成 (已添加到 Extensions tag 中的 #30) |
| 9 | 高 | 新增命令 | 添加 `/insights` — 生成 session 分析报告 | ✅ 已完成 (已添加到 Context tag 中的 #17) |
| 10 | 高 | 新增命令 | 添加 `/plugin` — 管理 Claude Code 插件 | ✅ 已完成 (已添加到 Extensions tag 中的 #33) |
| 11 | 高 | 新增命令 | 添加 `/skills` — 列出可用 skills | ✅ 已完成 (已添加到 Extensions tag 中的 #35) |
| 12 | 高 | 新增命令 | 添加 `/upgrade` — 打开升级页面切换套餐等级 | ✅ 已完成 (已添加到 Auth tag 中的 #3) |
| 13 | 高 | 移除命令 | 移除 `/output-style` — 在 v2.1.73 中已弃用，改用 `/config` | ✅ 已完成 (已从 Config tag 中移除) |
| 14 | 高 | 移除命令 | 移除 `/bug` 行 — 现已列为 `/feedback` 的别名 | ✅ 已完成 (已移除行，在 `/feedback` 描述中添加了"别名: /bug") |
| 15 | 高 | 描述变更 | 更新 `/passes` — 从审查传递重新定义为推荐分享 | ✅ 已完成 (已更新描述，保留在 Model tag 中) |
| 16 | 高 | 描述变更 | 更新 `/review` — 已弃用，由 `code-review` 市场插件替代 | ✅ 已完成 (已更新 Project tag 中的描述) |
| 17 | 中 | 描述变更 | 更新 `/stickers` — 从 UI 贴纸包改为订购实体贴纸 | ✅ 已完成 (已更新 Config tag 中的描述) |

---

## [2026-03-15 12:50 PM PKT] Claude Code v2.1.76

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Config tag 中添加 `/color [color|default]` — 为当前 session 设置提示栏颜色 | ✅ 已完成 (已添加到 Config tag 中的 #4) |
| 2 | 高 | 新增命令 | 在 Model tag 中添加 `/effort [low|medium|high|max|auto]` — 设置模型 effort 级别 | ✅ 已完成 (已添加到 Model tag 中的 #38) |
| 3 | 中 | 描述变更 | 更新 `/status` — 现为"打开设置界面 (状态选项卡)"而非"显示简洁的 session 状态摘要" | ✅ 已完成 (已更新 Context tag 中 #20 的描述) |
| 4 | 中 | 描述变更 | 更新 `/desktop` — 现为"在 Claude Code Desktop 应用中继续当前 session。仅限 macOS 和 Windows。" | ✅ 已完成 (已更新 Remote tag 中 #49 的描述) |
| 5 | 低 | 参数变更 | 更新 `/init` — 官方文档已移除 `[prompt]` 参数提示 | ✅ 已完成 (已移除 Project tag 中 #45 的 `[prompt]` 提示) |

---

## [2026-03-17 12:45 PM PKT] Claude Code v2.1.77

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增别名 | 在 `/fork` 条目中添加 `别名: /branch` (v2.1.77 将 fork 重命名为 branch) | ✅ 已完成 (已在 Session tag 中 #59 的 `/fork` 中添加"别名: /branch") |
| 2 | 高 | 新增别名 | 为 8 个命令添加别名: `/clear` (+/reset, /new), `/config` (+/settings), `/desktop` (+/app), `/exit` (+/quit), `/rewind` (+/checkpoint), `/resume` (+/continue), `/remote-control` (+/rc), `/mobile` (+/ios, /android) | ✅ 已完成 (已为全部 8 个命令描述添加别名标注) |
| 3 | 中 | 描述变更 | 更新 `/diff` — "打开交互式差异查看器，显示未提交的更改和每轮差异" | ✅ 已完成 (已更新 Project tag 中 #44 的描述) |
| 4 | 中 | 描述变更 | 更新 `/memory` — "编辑 CLAUDE.md 内存文件，启用或禁用自动内存，以及查看自动内存条目" | ✅ 已完成 (已更新 Memory tag 中 #37 的描述) |
| 5 | 中 | 描述变更 | 更新 `/copy` — "将上一条助手回复复制到剪贴板。显示代码块的交互式选择器" | ✅ 已完成 (已更新 Export tag 中 #27 的描述) |
| 6 | 中 | 描述变更 | 更新 `/mobile` — "显示二维码以下载 Claude 移动端应用" | ✅ 已完成 (已更新 Remote tag 中 #52 的描述和别名) |
| 7 | 中 | 描述变更 | 更新 `/remote-control` — "使当前 session 可从 claude.ai 进行远程控制" | ✅ 已完成 (已更新 Remote tag 中 #53 的描述和别名) |
| 8 | 低 | Frontmatter 范围 | 6 个仅适用于 skill 的字段仍不在报告中 (有意限定范围) | ❌ 无效 (仅适用于 skill 的字段 — 与 v2.1.74 运行结果判断相同) |

---

## [2026-03-18 11:38 PM PKT] Claude Code v2.1.78

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Config tag 中添加 `/voice` — 切换按键通话语音输入 | ✅ 已完成 (已添加为 Config tag 中的 #15) |
| 2 | 高 | 别名反转 | 将 `/fork` → `/branch` 互换，`/branch` 为主要命令，`/fork` 为别名 | ✅ 已完成 (已在 Session tag 中 #56 处互换为 `/branch`，重新按字母排序) |
| 3 | 中 | 新增别名 | 为 `/permissions` 添加 `/allowed-tools` 别名 | ✅ 已完成 (已为 Config tag 中 #7 添加别名) |
| 4 | 中 | 新增参数 | 为 `/copy` 添加 `[N]` 参数语法 | ✅ 已完成 (已更新 Export tag 中 #28 为 `/copy [N]`) |
| 5 | 低 | Frontmatter 范围 | 6 个仅适用于 skill 的字段仍不在报告中 (有意限定范围) | ❌ 无效 (仅适用于 skill 的字段 — 与 v2.1.74 和 v2.1.77 运行结果判断相同) |

---

## [2026-03-19 11:54 AM PKT] Claude Code v2.1.79

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 低 | Frontmatter 范围 | 6 个仅适用于 skill 的字段仍不在报告中 (有意限定范围) | ❌ 无效 (仅适用于 skill 的字段 — 与 v2.1.74、v2.1.77 和 v2.1.78 运行结果判断相同) |

---

## [2026-03-20 08:33 AM PKT] Claude Code v2.1.80

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 新增字段 | 在 frontmatter 表中添加 `effort` — 调用命令时覆盖模型 effort 级别 (v2.1.80) | ✅ 已完成 (已添加为第 5 个字段，后在添加完整字段集时重定位到第 8 个) |
| 2 | 高 | QA 修正 | 添加 6 个缺失字段 (`name`, `disable-model-invocation`, `user-invocable`, `context`, `agent`, `hooks`) — 官方文档说明命令支持"与 skills 相同的 frontmatter"；之前标记为无效的判断 (v2.1.74–v2.1.79) 是错误的 | ✅ 已完成 (已添加全部 6 个字段，计数从 5 更新为 11，字段顺序与官方文档一致) |
| 3 | 高 | 跨报告修复 | 在 skills 报告 (`claude-skills.md`) 中添加 `effort` — 该字段此前也缺失 | ✅ 已完成 (已添加为 skills 报告中的第 8 个字段，计数从 10 更新为 11) |

---

## [2026-03-21 09:08 PM PKT] Claude Code v2.1.81

无需优先处理的操作项 — 报告与官方文档完全同步 (11 个 frontmatter 字段，63 个内置命令)。

---

## [2026-03-23 09:48 PM PKT] Claude Code v2.1.81

无需优先处理的操作项 — 报告与官方文档完全同步 (11 个 frontmatter 字段，63 个内置命令)。

---

## [2026-03-25 08:07 PM PKT] Claude Code v2.1.83

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Remote tag 中添加 `/schedule [description]` — 创建、更新、列出或运行 Cloud 定时任务 | ✅ 已完成 (已添加为 Remote tag 中的 #56，计数从 63 更新为 64) |

---

## [2026-03-26 01:01 PM PKT] Claude Code v2.1.84

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增字段 | 在 frontmatter 表中添加 `shell` — `!command` 代码块的 shell 类型 (`bash` 或 `powershell`) | ✅ 已完成 (已添加为 `hooks` 前的第 12 个字段，计数从 11 更新为 12) |
| 2 | 低 | 参数变更 | 为 `/fast` 命令添加 `[on|off]` 参数提示 | ✅ 已完成 (已将 `/fast` 更新为 `/fast [on|off]`，位于 Model tag 中 #40) |

---

## [2026-03-27 06:25 PM PKT] Claude Code v2.1.85

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增字段 | 在 frontmatter 表中添加 `paths` — 限制 skill 激活时机的 glob 模式 | ✅ 已完成 (已添加为 `user-invocable` 后的第 6 个字段，计数从 12 更新为 13) |

---

## [2026-03-28 06:05 PM PKT] Claude Code v2.1.86

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 参数变更 | 更新 `/add-dir` — 根据官方文档添加 `<path>` 必需参数提示 | ✅ 已完成 (已更新 Project tag 中 #44) |
| 2 | 中 | 参数变更 | 更新 `/branch` — 根据官方文档添加 `[name]` 可选参数提示 | ✅ 已完成 (已更新 Session tag 中 #57) |
| 3 | 中 | 参数变更 | 更新 `/model` — 根据官方文档添加 `[model]` 可选参数提示 | ✅ 已完成 (已更新 Model tag 中 #41) |
| 4 | 中 | 参数变更 | 更新 `/plan` — 根据官方文档添加 `[description]` 可选参数提示 | ✅ 已完成 (已更新 Model tag 中 #43) |
| 5 | 中 | 参数变更 | 更新 `/pr-comments` — 根据官方文档添加 `[PR]` 可选参数提示 | ✅ 已完成 (已更新 Project tag 中 #47) |
| 6 | 中 | 参数变更 | 更新 `/passes` — 移除 `[number]` 参数提示 (官方文档中不存在) | ✅ 已完成 (已更新 Model tag 中 #42) |
| 7 | 中 | 参数变更 | 更新 `/rename` — 从 `<name>` (必需) 改为 `[name]` (可选)，依据官方文档 | ✅ 已完成 (已更新 Session tag 中 #62) |
| 8 | 低 | 参数变更 | 更新 `/compact` — 将参数标签从 `[prompt]` 改为 `[instructions]`，依据官方文档 | ✅ 已完成 (已更新 Session tag 中 #60) |
| 9 | 低 | 参数变更 | 更新 `/feedback` — 将参数标签从 `[description]` 改为 `[report]`，依据官方文档 | ✅ 已完成 (已更新 Debug tag 中 #24) |

---

## [2026-03-31 06:55 PM PKT] Claude Code v2.1.88

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 描述同步 | 同步全部 43 个命令描述以匹配官方文档 — 行为说明 (`/vim` 切换, `/sandbox` 切换, `/hooks` 查看)、扩展细节 (`/effort` 持久性, `/copy` SSH 写入, `/model` effort 箭头)、以及跨 Auth、Config、Context、Debug、Export、Extensions、Model、Project、Remote 和 Session 标签的措辞对齐 | ✅ 已完成 (全部 64 个描述现已与 code.claude.com/docs/en/commands 的官方文档匹配) |

---

## [2026-04-01 12:26 PM PKT] Claude Code v2.1.89

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 低 | 描述变更 | 更新 `/init` — 官方文档现在使用 `CLAUDE_CODE_NEW_INIT=1` 而不是 `=true` | ✅ 已完成 (已将环境变量值从 `=true` 更新为 `=1` 以匹配官方文档) |

---

## [2026-04-02 09:14 PM PKT] Claude Code v2.1.90

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 描述变更 | 更新 `/permissions` — 官方文档扩展描述为包含作用域规则、目录管理和自动模式拒绝审查的交互式对话框 | ✅ 已完成 (已更新描述以匹配官方文档) |
| 2 | 中 | 新增别名 | 根据官方文档为 `/tasks` 命令添加 `/bashes` 别名 | ✅ 已完成 (已在 Debug tag 中 #27 的 `/tasks` 中添加"别名: /bashes") |

---

## [2026-04-03 08:34 PM PKT] Claude Code v2.1.91

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Config tag 中添加 `/powerup` — 通过带有动画演示的快速交互式课程探索 Claude Code 功能 | ✅ 已完成 (已添加为 Debug tag 中的 #26 — 在 v2.1.92 运行中解决) |

---

## [2026-04-04 10:40 PM PKT] Claude Code v2.1.92

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Debug tag 中添加 `/powerup` — 通过带有动画演示的快速交互式课程探索 Claude Code 功能 | ✅ 已完成 (已添加为 Debug tag 中的 #26，从 v2.1.91 延续) |
| 2 | 高 | 新增命令 | 在 Auth tag 中添加 `/setup-bedrock` — 通过交互式向导配置 Amazon Bedrock 认证、区域和模型固定 | ✅ 已完成 (已添加为 Auth tag 中的 #3) |
| 3 | 高 | 新增命令 | 在 Model tag 中添加 `/ultraplan <prompt>` — 在 ultraplan session 中草拟计划，在浏览器中审查，然后远程执行或发送回来 | ✅ 已完成 (已添加为 Model tag 中的 #45) |
| 4 | 高 | 移除命令 | 从 Config tag 中移除 `/vim` — 在 v2.1.92 中移除 (最高版本: 2.1.91)，改用 `/config` 编辑器模式 | ✅ 已完成 (已从 Config tag 中移除) |
| 5 | 高 | 移除命令 | 从 Project tag 中移除 `/pr-comments [PR]` — 在 v2.1.91 中移除 (最高版本: 2.1.90)，直接询问 Claude | ✅ 已完成 (已从 Project tag 中移除) |
| 6 | 中 | 描述变更 | 更新 `/release-notes` — 现为"在交互式版本选择器中查看更新日志。选择特定版本查看其发布说明，或选择显示所有版本。" | ✅ 已完成 (已更新 Debug tag 中 #27 的描述) |

---

## [2026-04-08 09:35 PM PKT] Claude Code v2.1.96

无需优先处理的操作项 — 报告与官方文档完全同步 (13 个 frontmatter 字段，65 个内置命令)。

---

## [2026-04-09 11:31 PM PKT] Claude Code v2.1.97

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Remote tag 中添加 `/autofix-pr [prompt]` — 生成一个 web session，监视当前分支的 PR，当 CI 失败或审阅者留下评论时推送修复 | ✅ 已完成 (已添加为 Remote tag 中的 #51，计数从 65 更新为 68) |
| 2 | 高 | 新增命令 | 在 Remote tag 中添加 `/teleport` — 将 web session 上的 Claude Code 拉入当前终端。别名: `/tp` | ✅ 已完成 (已添加为 Remote tag 中的 #59) |
| 3 | 高 | 新增命令 | 在 Remote tag 中添加 `/web-setup` — 使用本地 `gh` CLI 凭据将 GitHub 账号连接到 web 上的 Claude Code | ✅ 已完成 (已添加为 Remote tag 中的 #60) |
| 4 | 中 | 描述变更 | 更新 `/add-dir` — 官方文档现在包含关于从添加的目录无法发现 `.claude/` 配置的注意事项 | ✅ 已完成 (已更新 Project tag 中 #46 的描述) |

---

## [2026-04-13 08:00 PM PKT] Claude Code v2.1.101

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Auth tag 中添加 `/setup-vertex` — 通过交互式向导配置 Google Vertex AI 认证、项目、区域和模型固定。仅在设置 `CLAUDE_CODE_USE_VERTEX=1` 时可见 | ✅ 已完成 (已添加为 Auth tag 中的 #4，计数从 68 更新为 69) |

---

## [2026-04-14 11:13 PM PKT] Claude Code v2.1.107

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增字段 | 在 frontmatter 表中添加 `when_to_use` — 关于 Claude 何时应调用该 skill 的额外上下文，附加到列表中的 `description` 之后 (计数 13 → 14) | ✅ 已完成 (已在 `description` 字段后添加，计数从 13 更新为 14) |
| 2 | 高 | 新增命令 | 在 Project tag 中添加 `/team-onboarding` — 根据 Claude Code 使用历史生成团队上手指南 (计数 69 → 70) | ✅ 已完成 (已添加为 Project tag 中的 #52，计数从 69 更新为 70) |
| 3 | 中 | 范围决策 | 官方文档统一表中列出了 5 个捆绑 skills (`/batch`, `/claude-api`, `/debug`, `/loop`, `/simplify`)，但根据报告当前的限定范围声明被排除在外 | ❌ 无效 (用户选择保持报告仅限定于内置命令 — 保留免责声明) |
| 4 | 中 | 描述变更 | 更新 `/doctor` — 添加"按 `f` 让 Claude 修复任何报告的问题" | ✅ 已完成 (已在描述中添加状态图标和 `f` 键修复详情) |
| 5 | 中 | 描述变更 | 更新 `/schedule` — 术语从"Cloud 定时任务"改为"例行程序" | ✅ 已完成 (已更新描述中的术语) |

---

## [2026-04-16 08:20 PM PKT] Claude Code v2.1.110

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 中 | 新增别名 | 为 `/rewind` 条目添加 `/undo` 别名 — 在 v2.1.108 中添加 | ✅ 已完成 (已在 Session tag 中 #70 的现有 `/checkpoint` 别名旁添加 `/undo`) |

---

## [2026-04-18 07:54 PM PKT] Claude Code v2.1.114

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Session tag 中添加 `/recap` — 按需生成当前 session 的一行摘要 (v2.1.108) | ✅ 已完成 (已添加为 Session tag 中的 #72，计数从 70 更新为 75) |
| 2 | 高 | 新增命令 | 在 Config tag 中添加 `/focus` — 切换焦点视图，仅显示最后一条提示、工具调用摘要和最终回复 (v2.1.110) | ✅ 已完成 (已添加为 Config tag 中的 #8) |
| 3 | 高 | 新增命令 | 在 Config tag 中添加 `/tui [default|fullscreen]` — 设置终端 UI 渲染器并重新启动，对话保持不变 (v2.1.110) | ✅ 已完成 (已添加为 Config tag 中的 #17) |
| 4 | 高 | 新增命令 | 在 Project tag 中添加 `/ultrareview [PR]` — 在云沙箱中运行深度多 agent 代码审查 (v2.1.111) | ✅ 已完成 (已添加为 Project tag 中的 #56) |
| 5 | 高 | 新增命令 | 在 Debug tag 中添加 `/heapdump` — 将 JavaScript 堆快照和内存分解写入 `~/Desktop`，用于诊断高内存使用情况 | ✅ 已完成 (已添加为 Debug tag 中的 #28) |
| 6 | 高 | 描述变更 | 将 `/review` 从已弃用恢复为实时内置命令，依据官方文档 ("在当前的 session 中本地审查拉取请求。如需更深入的基于云的审查，请参见 `/ultrareview`") — 撤销 v2.1.74 的更新 | ✅ 已完成 (已更新 Project tag 中 #53 的描述，现引用 `/ultrareview`) |
| 7 | 中 | 描述变更 | 更新 `/effort` 描述 — 官方现列出 `xhigh` 级别，并在无参数时打开交互式滑块 (v2.1.111) | ✅ 已完成 (已更新参数提示以包含 `xhigh`，描述已提及交互式滑块) |
| 8 | 中 | 描述变更 | 更新 `/theme` 描述 — 官方添加"自动 (匹配终端)"选项 (v2.1.111) | ✅ 已完成 (已在 Config tag 中 #16 的描述中添加"自动 (匹配终端)") |
| 9 | 中 | 描述变更 | 更新 `/model` 描述 — 官方提示其在对话中途切换前会发出警告 (v2.1.108) | ✅ 已完成 (已在 Model tag 中 #46 添加对话中途切换警告详情) |
| 10 | 中 | 新增别名 | 根据官方文档为 `/schedule` 命令添加 `/routines` 别名 | ✅ 已完成 (已在 Remote tag 中 #64 添加"别名: /routines") |

---

## [2026-04-24 12:29 AM PKT] Claude Code v2.1.118

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增字段 | 在 frontmatter 表中添加 `arguments` — 用于 `$name` 替换的命名位置参数 (计数 14 → 15) | ✅ 已完成 (已在 `argument-hint` 后添加，计数从 14 更新为 15) |
| 2 | 高 | 描述变更 | 更新 `/cost` — 现仅为 `/usage` 的别名 | ✅ 已完成 (描述简化为"`/usage` 的别名") |
| 3 | 高 | 描述变更 | 更新 `/stats` — 现为 `/usage` 的别名，打开 Stats 选项卡 | ✅ 已完成 (描述已更新为"`/usage` 的别名。打开 Stats 选项卡") |
| 4 | 高 | 描述变更 | 更新 `/usage` — 整合 `/cost` 和 `/stats` 的规范命令；注明别名 | ✅ 已完成 (扩展为"显示 session 花费、套餐使用限制和活动统计。`/cost` 和 `/stats` 是别名") |
| 5 | 中 | 参数变更 | 将 `/voice` 签名更新为 `/voice [hold|tap|off]` | ✅ 已完成 (签名和描述已更新) |
| 6 | 中 | 描述变更 | 更新 `/theme` — 添加自定义主题支持 (`~/.claude/themes/`、插件、"新建自定义主题…") | ✅ 已完成 (自定义主题详情已添加到描述) |
| 7 | 中 | 描述变更 | 更新 `/terminal-setup` — 更换终端列表 (移除 Warp；添加 Cursor、Windsurf、Zed) | ✅ 已完成 (终端列表已替换为: VS Code、Cursor、Windsurf、Alacritty、Zed) |
| 8 | 低 | 描述变更 | 更新 `/effort` — 注明 `max` 级别仅限当前 session | ✅ 已完成 (在描述中为 `max` 添加了"(仅限当前 session)"限定语) |

---

## [2026-04-26 01:10 PM PKT] Claude Code v2.1.119

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 描述变更 | 更新 `/branch` — 添加 `CLAUDE_CODE_FORK_SUBAGENT` 环境变量说明，解释 `/fork` 的分化行为 (v2.1.117) | ✅ 已完成 (已在 Session tag 中 #67 的描述后追加 fork-subagent 说明) |
| 2 | 中 | 描述变更 | 更新 `/focus` — 添加"仅在全屏渲染模式下可用"限定语 (v2.1.110) | ✅ 已完成 (已在 Config tag 中 #8 追加仅全屏限定语) |
| 3 | 中 | 描述变更 | 更新 `/skills` — 添加"按 `t` 按 token 数量排序" (v2.1.110/111) | ✅ 已完成 (已在 Extensions tag 中 #42 追加按 token 数量排序详情) |
| 4 | 中 | 描述变更 | 更新 `/clear` — 根据官方文档重新措辞以与 `/compact` 对比 | ✅ 已完成 (已将描述替换为"开始一个新的对话，上下文为空……请改用 `/compact`"，位于 Session tag 中 #69) |
| 5 | 低 | 范围决策 | 上游统一表中列出了 6 个捆绑 skills (`/batch`, `/claude-api`, `/debug`, `/fewer-permission-prompts`, `/loop`, `/simplify`)，但根据报告范围被排除 | ❌ 无效 (从 v2.1.107 延续 — 用户此前选择保持报告仅限定于内置命令) |

---

## [2026-04-29 12:50 AM PKT] Claude Code v2.1.121

无需优先处理的操作项 — 报告与官方文档完全同步 (15 个 frontmatter 字段，75 个内置命令)。

---

## [2026-05-01 03:31 PM PKT] Claude Code v2.1.126

无需优先处理的操作项 — 报告与官方文档完全同步 (15 个 frontmatter 字段，75 个内置命令)。

---

## [2026-05-12 11:39 PM PKT] Claude Code v2.1.139

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 新增命令 | 在 Session tag 中添加 `/background [prompt]` — 将当前 session 分离为后台 agent 运行。别名: `/bg` | ✅ 已完成 (已添加为 Session tag 中的 #69，计数从 75 更新为 80) |
| 2 | 高 | 新增命令 | 在 Session tag 中添加 `/goal [condition|clear]` — Claude 跨轮次持续工作，直到条件满足 (v2.1.139) | ✅ 已完成 (已添加为 Session tag 中的 #75) |
| 3 | 高 | 新增命令 | 在 Config tag 中添加 `/radio` — 在浏览器中打开 Claude FM lo-fi 电台 | ✅ 已完成 (已添加为 Config tag 中的 #12) |
| 4 | 高 | 新增命令 | 在 Config tag 中添加 `/scroll-speed` — 交互式调整鼠标滚轮滚动速度 (v2.1.139) | ✅ 已完成 (已添加为 Config tag 中的 #14) |
| 5 | 高 | 新增命令 | 在 Session tag 中添加 `/stop` — 停止当前后台 session；保留对话记录和 worktree | ✅ 已完成 (已添加为 Session tag 中的 #80) |
| 6 | 低 | 范围决策 | 上游统一表中列出了 6 个捆绑 skills (`/batch`, `/claude-api`, `/debug`, `/fewer-permission-prompts`, `/loop`, `/simplify`)，但根据报告范围被排除 | ❌ 无效 (从 v2.1.107 和 v2.1.119 延续 — 用户此前选择保持报告仅限定于内置命令) |

---

## [2026-05-21 12:06 AM PKT] Claude Code v2.1.145

| # | 优先级 | 类型 | 操作 | 状态 |
|---|----------|------|--------|--------|
| 1 | 高 | 命令重命名 | 在 Context tag 中将 `/extra-usage` 重命名为 `/usage-credits` (v2.1.144)；保留 `/extra-usage` 记为曾用名；在 Context 组内重新排序 (`e`→`u`) | ✅ 已完成 (已在 Context tag 中 #27 处重命名，移至 `/usage` 之后，行 23–27 重新编号；计数保持 80 不变) |
| 2 | 中 | 新增别名 | 为 `/feedback` 添加 `/share` 别名，并扩展描述为"提交反馈、报告 bug 或分享您的对话。别名: `/bug`、`/share`" | ✅ 已完成 (已更新 Debug tag 中 #29 的描述) |
| 3 | 低 | 值变更 | 在 `effort` frontmatter 字段的选项列表中添加 `xhigh` (`low`、`medium`、`high`、`xhigh`、`max`) | ✅ 已完成 (已将 `xhigh` 添加到 effort 字段行；同步值列表，非字段新增/移除) |
| 4 | 低 | 范围决策 | 上游统一表中的 9 个捆绑 skills (`/batch`, `/claude-api`, `/debug`, `/fewer-permission-prompts`, `/loop`, `/run`, `/run-skill-generator`, `/simplify`, `/verify`) 根据报告范围被排除 | ❌ 无效 (从 v2.1.107/119/139 延续 — 报告有意限定于仅内置命令) |
