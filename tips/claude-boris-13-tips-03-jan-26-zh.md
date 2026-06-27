# 我如何使用 Claude Code — 来自 Boris Cherny 的 13 个技巧

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 1 月 3 日分享的个人设置技巧总结。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Context

Boris 分享了他的个人 Claude Code 设置，并指出它"出奇地 vanilla"——Claude Code 开箱即用效果很好，所以他不会过度自定义。没有唯一正确的使用方式：团队有意将其构建为你可以以任何喜欢的方式使用、自定义和 hack。Claude Code 团队的每个人使用方式都截然不同。

<a href="https://x.com/bcherny/status/2007179832300581177"><img src="assets/boris-26-1-3/0.png" alt="Boris Cherny intro tweet" width="50%" /></a>

---

## 1/ 并行运行 5 个 Claude

在你的终端中并行运行 5 个 Claude。将 tabs 编号为 1–5，并使用系统通知来知道 Claude 何时需要输入。

See: [Terminal Setup Docs](https://code.claude.com/docs/en/terminal)

<a href="https://x.com/bcherny/status/2007179833990885678"><img src="assets/boris-26-1-3/1.png" alt="Run 5 Claudes in parallel" width="50%" /></a>

---

## 2/ 使用 claude.ai/code 实现更多并行

在 claude.ai/code 上并行运行 5–10 个 Claude，与本地 Claude 同时工作。使用 `claude.ai/code` 将本地会话交给 web 会话，在 Chrome 中手动启动会话，并在两者之间来回 teleport。

<a href="https://x.com/bcherny/status/2007179836704600237"><img src="assets/boris-26-1-3/2.png" alt="claude.ai/code parallelism" width="50%" /></a>

---

## 3/ 所有事情都使用 Opus with Thinking

所有事情都使用 Opus 4.5 with thinking。这是 Boris 用过的最好的 coding model — 尽管它比 Sonnet 更大更慢，但由于你不需要频繁引导它，而且它在 tool use 方面更出色，最终几乎总是比使用更小的 model 更快。

<a href="https://x.com/bcherny/status/2007179838864666847"><img src="assets/boris-26-1-3/3.png" alt="Opus with thinking" width="50%" /></a>

---

## 4/ 与团队共享一个 CLAUDE.md

为 repo 共享一个 `CLAUDE.md`。将其检入 git，并让整个团队每周贡献多次。每当 Claude 做错事时，将其添加到 `CLAUDE.md` 中，以便 Claude 下次知道不要这样做。

<a href="https://x.com/bcherny/status/2007179840848597422"><img src="assets/boris-26-1-3/4.png" alt="Shared CLAUDE.md" width="50%" /></a>

---

## 5/ 在 PR 上 @claude 以更新 CLAUDE.md

在 code review 期间，在你同事的 PR 上 tag `@claude`，将某些内容作为 PR 的一部分添加到 `CLAUDE.md`。使用 Claude Code GitHub action（[install-@hub-action](https://github.com/apps/claude)）来实现 — 这是 Boris 版本的 Compounding Engineering。

<a href="https://x.com/bcherny/status/2007179842928947333"><img src="assets/boris-26-1-3/5.png" alt="Tag @claude on PRs" width="50%" /></a>

---

## 6/ 大多数会话以 Plan Mode 开始

大多数会话以 Plan mode 开始（按两次 shift+tab）。如果目标是编写一个 Pull Request，使用 Plan mode 并与 Claude 反复交流，直到你喜欢它的计划。从那里切换到 auto-accept edits mode，Claude 通常可以一次性完成。一个好的计划非常重要。

<a href="https://x.com/bcherny/status/2007179845336527000"><img src="assets/boris-26-1-3/6.png" alt="Plan mode" width="50%" /></a>

---

## 7/ 使用 Slash Commands 处理 Inner Loop Workflows

为你每天做多次的每个 "inner loop" workflow 使用 slash commands。这节省了重复的 prompting，并且让 Claude 也可以使用这些 workflows。Commands 检入 git，存储在 `.claude/commands/` 中。

示例：`/commit-push-pr` — Commit、push 并打开一个 PR。

<a href="https://x.com/bcherny/status/2007179847949500714"><img src="assets/boris-26-1-3/7.png" alt="Slash commands" width="50%" /></a>

---

## 8/ 使用 Subagents 自动化常见 Workflows

定期使用几个 subagents：`code-simplifier` 在 Claude 完成工作后简化代码，`verify-app` 有详细指令用于端到端测试 Claude Code，等等。将 subagents 视为自动化最常见的 workflows — 类似于 slash commands。

Subagents 存储在 `.claude/agents/` 中。

<a href="https://x.com/bcherny/status/2007179850139000872"><img src="assets/boris-26-1-3/8.png" alt="Subagents" width="50%" /></a>

---

## 9/ 使用 PostToolUse Hook 自动格式化代码

使用 `PostToolUse` hook 格式化 Claude 的代码。Claude 通常开箱即用地生成格式良好的代码，hook 处理最后 10%，以避免后续在 CI 中出现格式化错误。

```json
"PostToolUse": [
  {
    "matcher": "Write|Edit",
    "hooks": [
      {
        "type": "command",
        "command": "bun run format || true"
      }
    ]
  }
]
```

<a href="https://x.com/bcherny/status/2007179852047335529"><img src="assets/boris-26-1-3/9.png" alt="PostToolUse hook for formatting" width="50%" /></a>

---

## 10/ 预允许 Permissions 而非使用 --dangerously-skip-permissions

不要使用 `--dangerously-skip-permissions`。相反，使用 `/permissions` 预允许你知道在环境中安全的常用 bash 命令，以避免不必要的 permission prompts。其中大多数检入 `.claude/settings.json` 并与团队共享。

<a href="https://x.com/bcherny/status/2007179854077407667"><img src="assets/boris-26-1-3/10.png" alt="Pre-allow permissions" width="50%" /></a>

---

## 11/ 让 Claude 通过 MCP 使用你的所有 Tools

Claude Code 使用你所有的 tools。它经常搜索并发布到 Slack（通过 MCP server），运行 BigQuery 查询来回答 analytics 问题（使用 `bq` CLI），从 Sentry 抓取错误日志等。Slack MCP configuration 检入 `.mcp.json` 并与团队共享。

<a href="https://x.com/bcherny/status/2007179856266789204"><img src="assets/boris-26-1-3/11.png" alt="MCP tools" width="50%" /></a>

---

## 12/ 使用 Background Agents 验证 Long-Running Tasks

对于非常长时间的运行任务，要么 (a) 提示 Claude 在完成后用 background agent 验证其工作，(b) 使用 agent Stop hook 更确定性地做到这一点，或 (c) 使用 ralph-wiggum plugin（最初由 @GeoffreyHuntley 构想）。

<a href="https://x.com/bcherny/status/2007179858435281082"><img src="assets/boris-26-1-3/12.png" alt="Long-running tasks verification" width="50%" /></a>

---

## 13/ 给 Claude 一种验证其工作的方法

可能是从 Claude Code 获得出色结果最重要的一点 — 给 Claude 一种验证其工作的方法。如果 Claude 拥有那个 feedback loop，它将使最终结果的质量提高 2–3 倍。

Claude 测试 Boris 提交的每一个更改。

<a href="https://x.com/bcherny/status/2007179861115511237"><img src="assets/boris-26-1-3/13.png" alt="Give Claude a way to verify" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — January 3, 2026](https://x.com/bcherny/status/2007179832300581177)
