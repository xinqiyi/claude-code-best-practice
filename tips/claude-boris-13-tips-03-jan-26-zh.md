# 我如何使用 Claude Code — 来自 Boris Cherny 的 13 条建议

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创建者，于 2026 年 1 月 3 日分享的设置技巧总结。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

Boris 分享了他个人的 Claude Code 设置，并指出其"出奇地简单"——Claude Code 开箱即用效果很好，所以他并没有做太多自定义。没有唯一正确的使用方式：团队有意将其设计成你可以按照自己的喜好使用、定制和改造。Claude Code 团队中的每个人使用方式都大相径庭。

<a href="https://x.com/bcherny/status/2007179832300581177"><img src="assets/boris-26-1-3/0.png" alt="Boris Cherny 介绍推文" width="50%" /></a>

---

## 1/ 并行运行 5 个 Claude

在终端中并行运行 5 个 Claude。将标签页编号 1-5，并使用系统通知来了解何时某个 Claude 需要输入。

参见：[终端设置文档](https://code.claude.com/docs/en/terminal)

<a href="https://x.com/bcherny/status/2007179833990885678"><img src="assets/boris-26-1-3/1.png" alt="并行运行 5 个 Claude" width="50%" /></a>

---

## 2/ 使用 claude.ai/code 实现更高并行度

在 claude.ai/code 上并行运行 5-10 个 Claude，配合本地的 Claude 使用。使用 `claude.ai/code` 将会话从本地切换到 Web 端，在 Chrome 中手动启动会话，并来回切换。

<a href="https://x.com/bcherny/status/2007179836704600237"><img src="assets/boris-26-1-3/2.png" alt="claude.ai/code 并行" width="50%" /></a>

---

## 3/ 在所有任务中使用 Opus with Thinking

在所有任务中使用 Opus 4.5 with thinking。这是 Boris 使用过的最好的编程模型——尽管它比 Sonnet 更大更慢，但由于你需要引导的次数更少，且它的 tool use 能力更强，总体上几乎总是比使用较小模型更快。

<a href="https://x.com/bcherny/status/2007179838864666847"><img src="assets/boris-26-1-3/3.png" alt="Opus with thinking" width="50%" /></a>

---

## 4/ 与团队共享一个 CLAUDE.md

为仓库共享一个 `CLAUDE.md`。将其提交到 git 中，并让整个团队每周多次贡献内容。每当 Claude 做了不正确的事情，将其添加到 `CLAUDE.md` 中，以便 Claude 下次知道不要这样做。

<a href="https://x.com/bcherny/status/2007179840848597422"><img src="assets/boris-26-1-3/4.png" alt="共享 CLAUDE.md" width="50%" /></a>

---

## 5/ 在 PR 上 @提及 @claude 以更新 CLAUDE.md

在代码审查期间，在你同事的 PR 中 `@claude` 提及 Claude，以便在 PR 中向 `CLAUDE.md` 添加内容。使用 Claude Code GitHub action（[install-@hub-action](https://github.com/apps/claude)）来实现这一点——这是 Boris 所说的"复利工程"（Compounding Engineering）的体现。

<a href="https://x.com/bcherny/status/2007179842928947333"><img src="assets/boris-26-1-3/5.png" alt="在 PR 上 @提及 @claude" width="50%" /></a>

---

## 6/ 大多数会话以 Plan 模式启动

大多数会话以 Plan 模式启动（按 shift+tab 两次）。如果目标是编写一个 Pull Request，使用 Plan 模式与 Claude 反复交流，直到你满意它的计划。然后切换到自动接受编辑模式，Claude 通常可以一次性完成。一个好的计划非常重要。

<a href="https://x.com/bcherny/status/2007179845336527000"><img src="assets/boris-26-1-3/6.png" alt="Plan 模式" width="50%" /></a>

---

## 7/ 使用 Slash Commands 处理内循环工作流

对你每天多次执行的每个"内循环"工作流使用 slash commands。这可以避免重复提示，并且使 Claude 也可以使用这些工作流。Commands 被提交到 git 中，位于 `.claude/commands/`。

示例：`/commit-push-pr` — 提交、推送并打开一个 PR。

<a href="https://x.com/bcherny/status/2007179847949500714"><img src="assets/boris-26-1-3/7.png" alt="Slash commands" width="50%" /></a>

---

## 8/ 使用 Subagents 自动化常见工作流

定期使用几个 subagents：`code-simplifier` 在 Claude 完成工作后简化代码，`verify-app` 包含测试 Claude Code 端到端的详细说明，等等。可以将 subagents 视为自动化最常见工作流的方式——类似于 slash commands。

Subagents 位于 `.claude/agents/`。

<a href="https://x.com/bcherny/status/2007179850139000872"><img src="assets/boris-26-1-3/8.png" alt="Subagents" width="50%" /></a>

---

## 9/ 使用 PostToolUse Hook 自动格式化代码

使用 `PostToolUse` hook 来格式化 Claude 的代码。Claude 通常开箱即用地生成格式良好的代码，而 hook 则负责处理最后 10%，以避免后续在 CI 中出现格式错误。

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

<a href="https://x.com/bcherny/status/2007179852047335529"><img src="assets/boris-26-1-3/9.png" alt="用于格式化的 PostToolUse hook" width="50%" /></a>

---

## 10/ 预授权权限，而非使用 --dangerously-skip-permissions

不要使用 `--dangerously-skip-permissions`。相反，使用 `/permissions` 来预授权在你的环境中已知安全的常见 bash 命令，以避免不必要的权限提示。大多数这些设置在 `.claude/settings.json` 中提交并与团队共享。

<a href="https://x.com/bcherny/status/2007179854077407667"><img src="assets/boris-26-1-3/10.png" alt="预授权权限" width="50%" /></a>

---

## 11/ 通过 MCP 让 Claude 使用你所有的工具

Claude Code 会使用你所有的工具。它经常搜索并发布到 Slack（通过 MCP server），运行 BigQuery 查询来回答分析问题（使用 `bq` CLI），从 Sentry 获取错误日志等。Slack MCP 配置在 `.mcp.json` 中提交并与团队共享。

<a href="https://x.com/bcherny/status/2007179856266789204"><img src="assets/boris-26-1-3/11.png" alt="MCP 工具" width="50%" /></a>

---

## 12/ 使用 Background Agents 验证长时间运行的任务

对于非常长时间运行的任务，可以做以下之一：(a) 提示 Claude 在完成任务后使用 background agent 验证其工作，(b) 使用 agent Stop hook 更确定性地执行此操作，或 (c) 使用 ralph-wiggum 插件（最初由 @GeoffreyHuntley 构思）。

<a href="https://x.com/bcherny/status/2007179858435281082"><img src="assets/boris-26-1-3/12.png" alt="长时间运行任务的验证" width="50%" /></a>

---

## 13/ 给 Claude 验证其工作的方法

可能是从 Claude Code 获得出色结果的最重要的事情——给 Claude 一种验证其工作的方法。如果 Claude 有了这个反馈循环，最终结果的质量将提高 2-3 倍。

Claude 会测试 Boris 提交的每一个变更。

<a href="https://x.com/bcherny/status/2007179861115511237"><img src="assets/boris-26-1-3/13.png" alt="给 Claude 验证的方法" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) on X — 2026 年 1 月 3 日](https://x.com/bcherny/status/2007179832300581177)
