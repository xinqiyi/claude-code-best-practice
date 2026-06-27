# Claude Code 中 15 个隐藏和未充分利用的功能 — 来自 Boris Cherny

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 3 月 30 日分享的技巧总结。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Context

Boris 分享了一堆他最喜欢的隐藏和未充分利用的 Claude Code 功能，重点关注他使用最多的那些。

<a href="https://x.com/bcherny/status/2038454336355999749"><img src="assets/boris-26-3-30/0.png" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ Claude Code 有 Mobile App

你知道 Claude Code 有 mobile app 吗？Boris 很多代码都是在 iOS app 上写的 — 这是一种无需打开笔记本电脑即可进行更改的便捷方式。

- 下载 Claude app for iOS/Android
- 导航到左侧的 **Code** tab
- 你可以直接从手机 review changes、approve PRs 和 write code

<a href="https://x.com/bcherny/status/2038454337811386436"><img src="assets/boris-26-3-30/1.png" alt="Claude Code mobile app" width="50%" /></a>

---

## 2/ 在 Mobile/Web/Desktop 和 Terminal 之间移动会话

运行 `claude --teleport` 或 `/teleport` 在你的机器上继续 cloud session。或运行 `/remote-control` 从手机/web 控制本地运行的 session。

- **Teleport**：将 cloud session 拉到本地 terminal
- **Remote Control**：让你从任何设备控制本地 session
- Boris 在他的 `/config` 中设置了 **"Enable Remote Control for all sessions"**

<a href="https://x.com/bcherny/status/2038454339933548804"><img src="assets/boris-26-3-30/2.png" alt="Teleport and Remote Control" width="50%" /></a>

---

## 3/ /loop 和 /schedule — 最强大的两个功能

使用这些来安排 Claude 按设定的时间间隔自动运行，最长可持续一周。Boris 在本地运行了一堆 loops：

- `/loop 5m /babysit` — 自动处理 code review、auto-rebase 并引导 PRs 到 production
- `/loop 30m /slack-feedback` — 每 30 分钟自动提交 Slack feedback 的 PRs
- `/loop /post-merge-sweeper` — 提交 PRs 以处理他遗漏的 code review 评论
- `/loop 1h /pr-pruner` — 关闭 stale 和不再需要的 PRs
- ……还有更多！

尝试将 workflows 转化为 skills + loops。这非常强大。

<a href="https://x.com/bcherny/status/2038454341884154269"><img src="assets/boris-26-3-30/3.png" alt="/loop and /schedule" width="50%" /></a>

---

## 4/ 使用 Hooks 确定性运行 Logic

使用 hooks 作为 agent lifecycle 的一部分来运行 logic。例如：

- **动态加载** 每次启动 Claude 时的 context（`SessionStart`）
- **Log 每个 bash command** model 运行的命令（`PreToolUse`）
- **将 permission prompts 路由** 到 WhatsApp 供你批准/拒绝（`PermissionRequest`）
- **Poke Claude** 在它停止时继续工作（`Stop`）

<a href="https://x.com/bcherny/status/2038454343519932844"><img src="assets/boris-26-3-30/4.png" alt="Use hooks" width="50%" /></a>

---

## 5/ Cowork Dispatch

Boris 每天都在使用 Dispatch 来 catch up Slack 和 emails、管理文件以及在不使用电脑时在他的笔记本电脑上执行操作。当他不 coding 时，他就在 dispatching。

- Dispatch 是 Claude Desktop app 的 **安全远程控制**
- 它可以在你的许可下使用你的 MCPs、browser 和 computer
- 将其视为从任何地方委派非编码任务给 Claude 的方式

<a href="https://x.com/bcherny/status/2038454345419936040"><img src="assets/boris-26-3-30/5.png" alt="Cowork Dispatch" width="50%" /></a>

---

## 6/ 使用 Chrome Extension 进行 Frontend 工作

使用 Claude Code 最重要的技巧：**给 Claude 一种验证其输出的方法。** 一旦你这样做了，Claude 就会迭代直到结果出色。

- 想象一下，要求某人建一个网站但不允许他们使用浏览器 — 结果可能不会好看
- 给 Claude 一个 browser，它会编写代码并迭代直到看起来不错
- Boris 每次处理 web 代码时都使用 Chrome extension — 它往往比类似的 MCPs 更可靠

<a href="https://x.com/bcherny/status/2038454347156398333"><img src="assets/boris-26-3-30/6.png" alt="Chrome extension for frontend" width="50%" /></a>

---

## 7/ 使用 Claude Desktop App 自动启动和测试 Web Servers

同样，Desktop app 内置了 Claude **自动运行你的 web server 甚至在内置 browser 中测试它**的能力。

- 你可以在 CLI 或 VSCode 中使用 Chrome extension 设置类似的功能
- 或者直接使用 Desktop app 获得集成体验

<a href="https://x.com/bcherny/status/2038454348804714642"><img src="assets/boris-26-3-30/7.png" alt="Desktop app web server testing" width="50%" /></a>

---

## 8/ Fork 你的 Session

人们经常问如何 fork 一个现有 session。两种方法：

1. 从你的 session 运行 `/branch`
2. 从 CLI 运行 `claude --resume <session-id> --fork-session`

`/branch` 创建一个分支会话 — 你现在处于分支中。要恢复原始会话，使用 `claude -r <original-session-id>`。

<a href="https://x.com/bcherny/status/2038454350214041740"><img src="assets/boris-26-3-30/8.png" alt="Fork your session" width="50%" /></a>

---

## 9/ 使用 /btw 进行 Side Queries

Boris 经常使用这个功能在 agent 工作时快速回答问题。`/btw` 让你在不中断 agent 当前任务的情况下提出 side question。

示例：
```
/btw how do I spell dachshund?
> dachshund — German for "badger dog" (dachs + badger, hund + dog).
↑/↓ to scroll · Space, Enter, or Escape to dismiss
```

<a href="https://x.com/bcherny/status/2038454351849787485"><img src="assets/boris-26-3-30/9.png" alt="/btw for side queries" width="50%" /></a>

---

## 10/ 使用 Git Worktrees

Claude Code 内置了对 git worktrees 的深度支持。Worktrees 对于在同一 repository 中进行大量并行工作至关重要。Boris **始终运行着数十个 Claude**，这就是他实现的方式。

- 使用 `claude -w` 在 worktree 中启动新 session
- 或在 Claude Desktop app 中点击 **"worktree" checkbox**
- 对于 non-git VCS 用户，使用 `WorktreeCreate` hook 为 worktree 创建添加自己的逻辑

<a href="https://x.com/bcherny/status/2038454353787519164"><img src="assets/boris-26-3-30/10.png" alt="Git worktrees" width="50%" /></a>

---

## 11/ 使用 /batch 展开大规模 Changesets

`/batch` 会询问你，然后让 Claude 将工作展开到尽可能多的 **worktree agents**（数十、数百甚至数千个）来完成。

- 用于大型代码迁移和其他可并行化的工作
- 每个 worktree agent 在自己的 codebase 副本上独立工作

<a href="https://x.com/bcherny/status/2038454355469484142"><img src="assets/boris-26-3-30/11.png" alt="/batch for massive changesets" width="50%" /></a>

---

## 12/ 使用 --bare 将 SDK Startup 加速高达 10 倍

默认情况下，当你运行 `claude -p`（或 TypeScript 或 Python SDKs）时，Claude 会搜索本地的 CLAUDE.md's、settings 和 MCPs。但对于非交互式使用，大多数情况下你希望通过 `--system-prompt`、`--mcp-config`、`--settings` 等明确指定要加载的内容。

- 这是 SDK 最初构建时的一个设计疏忽
- 在未来的版本中，他们会将默认值翻转为 `--bare`
- 目前，使用该标志可获 **高达 10 倍的 faster startup**

```bash
claude -p "summarize this codebase" \
    --output-format=stream-json \
    --verbose \
    --bare
```

<a href="https://x.com/bcherny/status/2038454357088457168"><img src="assets/boris-26-3-30/12.png" alt="--bare flag for SDK startup" width="50%" /></a>

---

## 13/ 使用 --add-dir 让 Claude 访问更多 Folders

在跨多个 repositories 工作时，Boris 通常在一个 repo 中启动 Claude，然后使用 `--add-dir`（或 `/add-dir`）让 Claude 看到其他 repo。

- 这不仅告诉 Claude 有关该 repo 的信息，还 **赋予它权限** 在该 repo 中工作
- 或者，将 `"additionalDirectories"` 添加到团队的 `settings.json`，在启动 Claude Code 时始终加载额外的文件夹

<a href="https://x.com/bcherny/status/2038454359047156203"><img src="assets/boris-26-3-30/13.png" alt="--add-dir for multiple repos" width="50%" /></a>

---

## 14/ 使用 --agent 给 Claude Code 自定义 System Prompt 和 Tools

Custom agents 是一个经常被忽视的强大原语。要使用它，只需在 `.claude/agents/` 中定义一个新的 agent，然后运行：

```bash
claude --agent=<your agent's name>
```

- Agents 可以有受限的 tools、自定义 descriptions 和特定的 models
- 它们非常适合创建只读 agents、专门的 review agents 或领域特定的 tools

<a href="https://x.com/bcherny/status/2038454360418787764"><img src="assets/boris-26-3-30/14.png" alt="--agent for custom system prompts" width="50%" /></a>

---

## 15/ 使用 /voice 启用 Voice Input

有趣的事实：Boris 大部分编码工作是通过与 Claude 对话而非打字完成的。

- 在 CLI 中运行 `/voice`，然后按住空格键说话
- 在 Desktop 上按下 voice button
- 或在你的 iOS settings 中启用 dictation

<a href="https://x.com/bcherny/status/2038454362226467112"><img src="assets/boris-26-3-30/15.png" alt="/voice for voice input" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — March 30, 2026](https://x.com/bcherny/status/2038454336355999749)
