# 自定义 Claude Code 的 12 种方式 — 来自 Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 2 月 12 日分享的自定义技巧总结。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Context

Boris Cherny 强调，自定义能力是工程师最喜欢 Claude Code 的特性之一 — hooks、plugins、LSPs、MCPs、skills、effort、custom agents、status lines、output styles 等等。他分享了开发者和团队自定义其设置的 12 种实用方式。

<a href="https://x.com/bcherny/status/2021699851499798911"><img src="assets/boris-26-2-12/0.webp" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ 配置你的 Terminal

为最佳 Claude Code 体验设置你的 terminal：

- **Theme**：运行 `/config` 设置 light/dark mode
- **Notifications**：为 iTerm2 启用通知，或使用自定义 notification hook
- **Newlines**：如果在 IDE terminal、Apple Terminal、Warp 或 Alacritty 中使用 Claude Code，运行 `/terminal-setup` 启用 shift+enter 换行（这样就不需要输入 `\`）
- **Vim mode**：运行 `/vim`

<a href="https://x.com/bcherny/status/2021699859359883608"><img src="assets/boris-26-2-12/1.webp" alt="Configure your terminal" width="50%" /></a>

---

## 2/ 调整 Effort Level

运行 `/model` 选择你偏好的 effort level：

- **Low** — 更少的 tokens，更快的响应
- **Medium** — 平衡行为
- **High** — 更多的 tokens，更多的智能

Boris 的偏好：对所有事情使用 High。

<a href="https://x.com/bcherny/status/2021699860869902424"><img src="assets/boris-26-2-12/2.webp" alt="Adjust effort level" width="50%" /></a>

---

## 3/ 安装 Plugins、MCPs 和 Skills

Plugins 允许你安装 LSPs（适用于每种主要语言）、MCPs、skills、agents 和 custom hooks。

从官方 Anthropic plugin marketplace 安装，或为公司创建自己的 marketplace。将 `settings.json` 检入代码库，为团队自动添加 marketplaces。

运行 `/plugin` 开始。

<a href="https://x.com/bcherny/status/2021699862522364149"><img src="assets/boris-26-2-12/3.webp" alt="Install Plugins, MCPs, and Skills" width="50%" /></a>

---

## 4/ 创建 Custom Agents

在 `.claude/agents` 中放置 `.md` 文件来创建 custom agents。每个 agent 可以拥有自定义的 name、color、tool set、预允许和预禁止的 tools、permission mode 和 model。

你还可以使用 `settings.json` 中的 `"agent"` 字段或 `--agent` 标志设置主会话的默认 agent。

运行 `/agents` 开始。

<a href="https://x.com/bcherny/status/2021700144039903699"><img src="assets/boris-26-2-12/4.webp" alt="Create custom agents" width="50%" /></a>

---

## 5/ 预批准常用 Permissions

Claude Code 使用结合了 prompt injection 检测、static analysis、sandboxing 和 human oversight 的 permission 系统。

开箱即用，一小部分安全命令是预批准的。要预批准更多，运行 `/permissions` 并添加到 allow 和 block 列表中。将这些检入团队的 `settings.json`。

支持完整的通配符语法 — 例如 `Bash(bun run *)` 或 `Edit(/docs/**)`。

<a href="https://x.com/bcherny/status/2021700332292911228"><img src="assets/boris-26-2-12/5.webp" alt="Pre-approve common permissions" width="50%" /></a>

---

## 6/ 启用 Sandboxing

选择使用 Claude Code 的开源 sandbox runtime，在提高安全性的同时减少 permission prompts。

运行 `/sandbox` 启用它。Sandboxing 在你的机器上运行，支持 file 和 network isolation。

<a href="https://x.com/bcherny/status/2021700506465579443"><img src="assets/boris-26-2-12/6.webp" alt="Enable sandboxing" width="50%" /></a>

---

## 7/ 添加 Status Line

自定义 status lines 显示在 composer 正下方，展示 model、directory、remaining context、cost 以及你在工作时想看到的任何其他内容。

每个团队成员可以有不同的 statusline。使用 `/statusline` 让 Claude 基于你的 `.bashrc`/`.zshrc` 生成一个。

<a href="https://x.com/bcherny/status/2021700784019452195"><img src="assets/boris-26-2-12/7.webp" alt="Add a status line" width="50%" /></a>

---

## 8/ 自定义你的 Keybindings

Claude Code 中的每个 key binding 都是可自定义的。运行 `/keybindings` 重新映射任何键。设置实时生效，你可以立即感受到效果。

<a href="https://x.com/bcherny/status/2021700883873165435"><img src="assets/boris-26-2-12/8.webp" alt="Customize your keybindings" width="50%" /></a>

---

## 9/ 设置 Hooks

Hooks 允许你确定性地挂接到 Claude 的生命周期中：

- 自动将 permission 请求路由到 Slack 或 Opus
- 当 Claude 到达 turn 结束时提示它继续（你甚至可以启动一个 agent 或使用 prompt 来决定 Claude 是否应该继续）
- Pre-process 或 post-process tool calls，例如添加你自己的 logging

让 Claude 添加一个 hook 开始。

<a href="https://x.com/bcherny/status/2021701059253874861"><img src="assets/boris-26-2-12/9.webp" alt="Set up hooks" width="50%" /></a>

---

## 10/ 自定义你的 Spinner Verbs

自定义你的 spinner verbs，用你自己的动词添加或替换默认列表。将 `settings.json` 检入 source control 与团队共享动词。

<a href="https://x.com/bcherny/status/2021701145023197516"><img src="assets/boris-26-2-12/10.webp" alt="Customize your spinner verbs" width="50%" /></a>

---

## 11/ 使用 Output Styles

运行 `/config` 并设置 output style，让 Claude 使用不同的语气或格式回复。

- **Explanatory** — 推荐在熟悉新 codebase 时使用，让 Claude 在工作时解释 framework 和 code pattern
- **Learning** — 让 Claude 指导你完成代码更改
- **Custom** — 创建自定义 output styles 来调整 Claude 的语调

<a href="https://x.com/bcherny/status/2021701379409273093"><img src="assets/boris-26-2-12/11.webp" alt="Use output styles" width="50%" /></a>

---

## 12/ 自定义一切！

Claude Code 开箱即用效果很好，但当你进行自定义时，将 `settings.json` 检入 git，让你的团队也能受益。Configuration 支持多个级别：

- 针对你的 codebase
- 针对子文件夹
- 仅针对你自己
- 通过 enterprise-wide policies

凭借 37 个 settings 和 84 个 environment variables（使用 `settings.json` 中的 `"env"` 字段以避免 wrapper scripts），你想要的任何行为大概率都是可配置的。

<a href="https://x.com/bcherny/status/2021701636075458648"><img src="assets/boris-26-2-12/12.webp" alt="Customize all the things" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — February 12, 2026](https://x.com/bcherny)
- [Claude Code Terminal Setup Docs](https://code.claude.com/docs/en/terminal)
- [Claude Code Plugins & Discovery Docs](https://code.claude.com/docs/en/discover-plugins)
- [Claude Code Sub-agents Docs](https://code.claude.com/docs/en/sub-agents)
- [Claude Code Permissions Docs](https://code.claude.com/docs/en/permissions)
- [Claude Code Sandbox Docs](https://code.claude.com/docs/en/sandbox)
- [Claude Code Status Line Docs](https://code.claude.com/docs/en/statusline)
- [Claude Code Keyboard Shortcuts Docs](https://code.claude.com/docs/en/keybindings)
- [Claude Code Hooks Reference](https://code.claude.com/docs/en/hooks)
- [Claude Code Output Styles Docs](https://code.claude.com/docs/en/output-styles)
- [Claude Code Settings Docs](https://code.claude.com/docs/en/settings)
