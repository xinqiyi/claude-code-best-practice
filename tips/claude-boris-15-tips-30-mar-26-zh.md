# Claude Code 中 15 个隐藏且未被充分利用的功能 — 来自 Boris Cherny

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创建者，于 2026 年 3 月 30 日分享的技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他最喜欢的一批 Claude Code 中隐藏且未被充分利用的功能，重点是他最常使用的那些。

<a href="https://x.com/bcherny/status/2038454336355999749"><img src="assets/boris-26-3-30/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ Claude Code 有移动端 App

你知道吗？Claude Code 有移动端 App。Boris 的很多代码都是从 iOS 应用上编写的——这是一个无需打开笔记本电脑就能进行修改的便捷方式。

- 下载适用于 iOS/Android 的 Claude 应用
- 导航到左侧的 **代码（Code）** 选项卡
- 你可以直接在手机上审查更改、批准 PR 以及编写代码

<a href="https://x.com/bcherny/status/2038454337811386436"><img src="assets/boris-26-3-30/1.png" alt="Claude Code 移动端 App" width="50%" /></a>

---

## 2/ 在移动端/Web/桌面端和终端之间移动会话

运行 `claude --teleport` 或 `/teleport`，将云端会话拉取到你的本地机器上。或者运行 `/remote-control`，从手机/网页端控制本地运行的会话。

- **Teleport（传送）**：将云端会话拉取到本地终端
- **远程控制（Remote Control）**：让你从任何设备控制本地会话
- Boris 在 `/config` 中设置了 **"为所有会话启用远程控制（Enable Remote Control for all sessions）"**

<a href="https://x.com/bcherny/status/2038454339933548804"><img src="assets/boris-26-3-30/2.png" alt="Teleport 和远程控制" width="50%" /></a>

---

## 3/ /loop 和 /schedule — 两个最强大的功能

使用这些命令来调度 Claude 按设定的间隔自动运行，最长可运行一周。Boris 在本地运行着大量循环：

- `/loop 5m /babysit` — 自动处理代码审查、自动变基，并将 PR 护送上线
- `/loop 30m /slack-feedback` — 每 30 分钟自动提交 PR 以获取 Slack 反馈
- `/loop /post-merge-sweeper` — 提交 PR 以处理他遗漏的代码审查意见
- `/loop 1h /pr-pruner` — 关闭陈旧且不再需要的 PR
- ……还有很多！

尝试将 workflow 转化为 skill 加 loop 的组合。这非常强大。

<a href="https://x.com/bcherny/status/2038454341884154269"><img src="assets/boris-26-3-30/3.png" alt="/loop 和 /schedule" width="50%" /></a>

---

## 4/ 使用 Hooks 确定性执行逻辑

使用 hooks 在 agent 生命周期中执行逻辑。例如：

- **每次启动 Claude 时动态加载**上下文（`SessionStart`）
- **记录模型运行的每条 bash 命令**（`PreToolUse`）
- **将权限提示路由到 WhatsApp**供你批准/拒绝（`PermissionRequest`）
- **在 Claude 停止时提醒它继续**（`Stop`）

<a href="https://x.com/bcherny/status/2038454343519932844"><img src="assets/boris-26-3-30/4.png" alt="使用 hooks" width="50%" /></a>

---

## 5/ Cowork Dispatch

Boris 每天使用 Dispatch 来查看 Slack 和邮件、管理文件，以及在他不在电脑前时操作笔记本电脑。当他不写代码时，他就在使用 dispatch。

- Dispatch 是一个适用于 Claude Desktop 应用的**安全远程控制**
- 在你的许可下，它可以使用你的 MCP、浏览器和电脑
- 可以将其视为一种从任何地方将非编码任务委托给 Claude 的方式

<a href="https://x.com/bcherny/status/2038454345419936040"><img src="assets/boris-26-3-30/5.png" alt="Cowork Dispatch" width="50%" /></a>

---

## 6/ 使用 Chrome 扩展进行前端开发

使用 Claude Code 最重要的技巧：**给 Claude 一种验证其输出的方法。** 一旦你做到这一点，Claude 会迭代直到结果令人满意。

- 想象一下，让某人不准使用浏览器来搭建一个网站——结果很可能不好看
- 给 Claude 一个浏览器，它就会编写代码并不断迭代直到看起来不错
- Boris 每次处理 Web 代码时都会使用 Chrome 扩展——它往往比其他类似的 MCP 更可靠

<a href="https://x.com/bcherny/status/2038454347156398333"><img src="assets/boris-26-3-30/6.png" alt="用于前端的 Chrome 扩展" width="50%" /></a>

---

## 7/ 使用 Claude Desktop 应用自动启动和测试 Web 服务器

同理，Desktop 应用内置了让 Claude **自动运行你的 Web 服务器，甚至在内置浏览器中测试它**的能力。

- 你可以在 CLI 或 VSCode 中使用 Chrome 扩展设置类似的功能
- 或者就直接使用 Desktop 应用以获得一体化体验

<a href="https://x.com/bcherny/status/2038454348804714642"><img src="assets/boris-26-3-30/7.png" alt="Desktop 应用 Web 服务器测试" width="50%" /></a>

---

## 8/ 分支你的会话

人们经常问如何分支一个现有会话。两种方法：

1. 在会话中运行 `/branch`
2. 在 CLI 中运行 `claude --resume <session-id> --fork-session`

`/branch` 创建一个分支对话——你现在就在这个分支中。要恢复原始会话，使用 `claude -r <original-session-id>`。

<a href="https://x.com/bcherny/status/2038454350214041740"><img src="assets/boris-26-3-30/8.png" alt="分支你的会话" width="50%" /></a>

---

## 9/ 使用 /btw 进行侧边查询

Boris 经常用它来在 agent 工作时快速回答一些小问题。`/btw` 让你可以在不中断 agent 当前任务的情况下提出一个侧边问题。

示例：
```
/btw 如何拼写 dachshund（腊肠犬）？
> dachshund — 德语中意为"獾狗"（dachs = 獾, hund = 狗）。
↑/↓ 滚动 · Space、Enter 或 Escape 关闭
```

<a href="https://x.com/bcherny/status/2038454351849787485"><img src="assets/boris-26-3-30/9.png" alt="/btw 侧边查询" width="50%" /></a>

---

## 10/ 使用 Git Worktrees

Claude Code 内置了对 git worktree 的深度支持。Worktree 对于在同一个仓库中进行大量并行工作至关重要。Boris **同时运行着几十个 Claude**，这就是他实现的方式。

- 使用 `claude -w` 在 worktree 中启动一个新会话
- 或者在 Claude Desktop 应用中勾选 **"worktree"复选框**
- 对于非 git VCS 用户，使用 `WorktreeCreate` hook 添加你自己的 worktree 创建逻辑

<a href="https://x.com/bcherny/status/2038454353787519164"><img src="assets/boris-26-3-30/10.png" alt="Git worktrees" width="50%" /></a>

---

## 11/ 使用 /batch 分发大规模变更集

`/batch` 会先与你对话，然后让 Claude 将工作分发给所需数量的 **worktree agent**（几十个、几百个，甚至几千个）来完成。

- 用于大型代码迁移和其他可并行化的工作
- 每个 worktree agent 在各自独立的代码库副本上独立工作

<a href="https://x.com/bcherny/status/2038454355469484142"><img src="assets/boris-26-3-30/11.png" alt="/batch 大规模变更集" width="50%" /></a>

---

## 12/ 使用 --bare 将 SDK 启动速度提升最多 10 倍

默认情况下，当你运行 `claude -p`（或 TypeScript、Python SDK）时，Claude 会搜索本地的 CLAUDE.md、settings 和 MCP。但对于非交互式使用，大多数时候你希望通过 `--system-prompt`、`--mcp-config`、`--settings` 等明确指定要加载的内容。

- 这是 SDK 最初构建时的设计疏忽
- 在未来的版本中，他们将把默认行为改为 `--bare`
- 目前，通过加上该标志来获得最高 **10 倍的启动加速**

```bash
claude -p "总结这个代码库" \
    --output-format=stream-json \
    --verbose \
    --bare
```

<a href="https://x.com/bcherny/status/2038454357088457168"><img src="assets/boris-26-3-30/12.png" alt="--bare 标志加速 SDK 启动" width="50%" /></a>

---

## 13/ 使用 --add-dir 让 Claude 访问更多文件夹

当在多个仓库之间工作时，Boris 通常在一个仓库中启动 Claude，然后使用 `--add-dir`（或 `/add-dir`）让 Claude 看到另一个仓库。

- 这不仅让 Claude 知道该仓库，还**授予它在其中工作的权限**
- 或者，将 `"additionalDirectories"` 添加到团队的 `settings.json` 中，以便在启动 Claude Code 时始终加载额外的文件夹

<a href="https://x.com/bcherny/status/2038454359047156203"><img src="assets/boris-26-3-30/13.png" alt="--add-dir 用于多个仓库" width="50%" /></a>

---

## 14/ 使用 --agent 为 Claude Code 提供自定义 System Prompt 和工具

自定义 agent 是一个常被忽视的强大基础功能。要使用它，只需在 `.claude/agents/` 中定义一个新的 agent，然后运行：

```bash
claude --agent=<你的 agent 名称>
```

- Agent 可以拥有受限的工具、自定义描述和特定的模型
- 它们非常适合创建只读 agent、专门的审查 agent 或领域特定的工具

<a href="https://x.com/bcherny/status/2038454360418787764"><img src="assets/boris-26-3-30/14.png" alt="--agent 自定义 System Prompt" width="50%" /></a>

---

## 15/ 使用 /voice 启用语音输入

有趣的事实：Boris 大部分时间都是通过对着 Claude 说话来编程，而不是打字。

- 在 CLI 中运行 `/voice`，然后按住空格键说话
- 在 Desktop 上按下语音按钮
- 或者在 iOS 设置中启用听写功能

<a href="https://x.com/bcherny/status/2038454362226467112"><img src="assets/boris-26-3-30/15.png" alt="/voice 语音输入" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 3 月 30 日](https://x.com/bcherny/status/2038454336355999749)
