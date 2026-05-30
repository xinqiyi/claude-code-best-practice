# 自定义 Claude Code 的 12 种方法 — Boris Cherny 的技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创建者，于 2026 年 2 月 12 日分享的自定义技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris Cherny 指出，可定制性是工程师们最喜爱 Claude Code 的特性之一——hooks、plugins、LSPs、MCPs、skills、effort、自定义 agents、状态栏、输出样式等等。他分享了开发者和团队自定义其设置的 12 种实用方法。

<a href="https://x.com/bcherny/status/2021699851499798911"><img src="assets/boris-26-2-12/0.webp" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 配置你的终端

设置你的终端以获得最佳的 Claude Code 体验：

- **主题**：运行 `/config` 设置亮色/暗色模式
- **通知**：启用 iTerm2 的通知，或使用自定义通知 hook
- **换行**：如果在 IDE 终端、Apple Terminal、Warp 或 Alacritty 中使用 Claude Code，运行 `/terminal-setup` 启用 shift+enter 换行（这样你就不需要输入 `\`）
- **Vim 模式**：运行 `/vim`

<a href="https://x.com/bcherny/status/2021699859359883608"><img src="assets/boris-26-2-12/1.webp" alt="配置你的终端" width="50%" /></a>

---

## 2/ 调整 Effort 级别

运行 `/model` 选择你偏好的 effort 级别：

- **Low** — 更少的 tokens，更快的响应
- **Medium** — 均衡表现
- **High** — 更多的 tokens，更高的智能

Boris 的偏好：所有场景都使用 High。

<a href="https://x.com/bcherny/status/2021699860869902424"><img src="assets/boris-26-2-12/2.webp" alt="调整 effort 级别" width="50%" /></a>

---

## 3/ 安装 Plugins、MCPs 和 Skills

Plugins 让你安装 LSPs（适用于每种主流语言）、MCPs、skills、agents 和自定义 hooks。

从官方的 Anthropic plugin 市场安装，或为你的公司创建自己的市场。将 `settings.json` 检入代码库，以便为你的团队自动添加市场。

运行 `/plugin` 开始。

<a href="https://x.com/bcherny/status/2021699862522364149"><img src="assets/boris-26-2-12/3.webp" alt="安装 Plugins、MCPs 和 Skills" width="50%" /></a>

---

## 4/ 创建自定义 Agents

在 `.claude/agents` 中放入 `.md` 文件来创建自定义 agents。每个 agent 可以拥有自定义的名称、颜色、工具集、预允许和预禁止的工具、权限模式以及模型。

你也可以使用 `settings.json` 中的 `"agent"` 字段或 `--agent` 标志来设置主会话的默认 agent。

运行 `/agents` 开始。

<a href="https://x.com/bcherny/status/2021700144039903699"><img src="assets/boris-26-2-12/4.webp" alt="创建自定义 agents" width="50%" /></a>

---

## 5/ 预先批准常用权限

Claude Code 使用一套结合了提示注入检测、静态分析、沙箱和人工监督的权限系统。

开箱即用，一组安全命令已被预先批准。要预先批准更多命令，运行 `/permissions` 并添加到允许和阻止列表。将这些检入到你团队的 `settings.json` 中。

支持完整的通配符语法——例如 `Bash(bun run *)` 或 `Edit(/docs/**)`。

<a href="https://x.com/bcherny/status/2021700332292911228"><img src="assets/boris-26-2-12/5.webp" alt="预先批准常用权限" width="50%" /></a>

---

## 6/ 启用沙箱

选择启用 Claude Code 的开源沙箱运行时，以提高安全性并减少权限提示。

运行 `/sandbox` 启用它。沙箱在你的机器上运行，并同时支持文件和网络隔离。

<a href="https://x.com/bcherny/status/2021700506465579443"><img src="assets/boris-26-2-12/6.webp" alt="启用沙箱" width="50%" /></a>

---

## 7/ 添加状态栏

自定义状态栏显示在 composer 正下方，展示模型、目录、剩余上下文、成本，以及你在工作中想要看到的任何其他信息。

每个团队成员都可以有不同的状态栏。使用 `/statusline` 让 Claude 基于你的 `.bashrc`/`.zshrc` 生成一个。

<a href="https://x.com/bcherny/status/2021700784019452195"><img src="assets/boris-26-2-12/7.webp" alt="添加状态栏" width="50%" /></a>

---

## 8/ 自定义你的快捷键

Claude Code 中的每一个按键绑定都是可自定义的。运行 `/keybindings` 重新映射任何按键。设置会实时加载，因此你可以立即感受效果。

<a href="https://x.com/bcherny/status/2021700883873165435"><img src="assets/boris-26-2-12/8.webp" alt="自定义你的快捷键" width="50%" /></a>

---

## 9/ 设置 Hooks

Hooks 让你能够确定性地接入 Claude 的生命周期：

- 自动将权限请求路由到 Slack 或 Opus
- 当 Claude 到达一轮对话结束时，提示它继续（你甚至可以启动一个 agent，或使用一个 prompt 来决定 Claude 是否应该继续）
- 预处理或后处理工具调用，例如添加你自己的日志记录

让 Claude 添加一个 hook 来开始。

<a href="https://x.com/bcherny/status/2021701059253874861"><img src="assets/boris-26-2-12/9.webp" alt="设置 hooks" width="50%" /></a>

---

## 10/ 自定义你的 Spinner 动词

自定义你的 spinner 动词，以添加或替换默认列表为你自己的动词。将 `settings.json` 检入源代码管理，以便与你的团队共享动词。

<a href="https://x.com/bcherny/status/2021701145023197516"><img src="assets/boris-26-2-12/10.webp" alt="自定义你的 spinner 动词" width="50%" /></a>

---

## 11/ 使用输出样式

运行 `/config` 并设置一个输出样式，让 Claude 以不同的语气或格式回复。

- **Explanatory（解释型）** — 推荐在熟悉新代码库时使用，让 Claude 在工作时解释框架和代码模式
- **Learning（学习型）** — 让 Claude 指导你进行代码修改
- **Custom（自定义）** — 创建自定义输出样式来调整 Claude 的语气

<a href="https://x.com/bcherny/status/2021701379409273093"><img src="assets/boris-26-2-12/11.webp" alt="使用输出样式" width="50%" /></a>

---

## 12/ 自定义一切！

Claude Code 开箱即用表现优异，但当你进行自定义时，请将你的 `settings.json` 检入 git，以便你的团队也能受益。配置在多个层级得到支持：

- 针对你的代码库
- 针对子文件夹
- 仅针对你自己
- 通过企业级策略

拥有 37 项设置和 84 个环境变量（在 `settings.json` 中使用 `"env"` 字段以避免包装脚本），你想要的任何行为几乎都可以配置。

<a href="https://x.com/bcherny/status/2021701636075458648"><img src="assets/boris-26-2-12/12.webp" alt="自定义一切" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 2 月 12 日](https://x.com/bcherny)
- [Claude Code 终端设置文档](https://code.claude.com/docs/en/terminal)
- [Claude Code Plugins 和发现文档](https://code.claude.com/docs/en/discover-plugins)
- [Claude Code 子代理文档](https://code.claude.com/docs/en/sub-agents)
- [Claude Code 权限文档](https://code.claude.com/docs/en/permissions)
- [Claude Code 沙箱文档](https://code.claude.com/docs/en/sandbox)
- [Claude Code 状态栏文档](https://code.claude.com/docs/en/statusline)
- [Claude Code 键盘快捷键文档](https://code.claude.com/docs/en/keybindings)
- [Claude Code Hooks 参考](https://code.claude.com/docs/en/hooks)
- [Claude Code 输出样式文档](https://code.claude.com/docs/en/output-styles)
- [Claude Code 设置文档](https://code.claude.com/docs/en/settings)
