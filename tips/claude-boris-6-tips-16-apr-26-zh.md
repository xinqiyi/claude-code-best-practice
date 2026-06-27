# 从 Opus 4.7 获得更多收益的 6 个技巧 — 来自 Boris Cherny

Boris Cherny（[@bcherny](https://x.com/bcherny)，Claude Code 的创建者）于 2026 年 4 月 16 日分享的一系列技巧 — 在 dogfooding Opus 4.7 数周之后。

<table width="100%">
<tr>
<td><a href="../">← Back to Claude Code Best Practice</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Context

在 dogfooding Opus 4.7 数周后，Boris 感到"生产力极高"，并分享了六个从新 model 中获得更多收益的方法 — 从 permission automation 到 effort tuning 再到 verification patterns。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/0.png" alt="Boris Cherny intro tweet — dogfooding Opus 4.7" width="50%" /></a>

---

## 1/ Auto Mode — 不再有 Permission Prompts

Opus 4.7 喜欢执行复杂、长时间运行的任务：deep research、refactoring code、building complex features、迭代直到 performance benchmark 被达到。过去，你要么需要在 model 执行这些长时间任务时 babysit 它，要么使用 `--dangerously-skip-permissions`。

Anthropic 最近推出了 **auto mode** 作为一种更安全的替代方案。在此模式下，permission prompts 被路由到一个基于 model 的 classifier，该分类器决定命令是否可以安全执行：

- 如果安全，自动批准
- 如果有风险，暂停并询问

这意味着在 model 运行时不再需要 babysitting。不仅如此，这意味着你可以并行运行更多 Claude — 如果安全，你可以将注意力切换到下一个 Claude。

Auto mode 现在适用于 Opus 4.7 的 Max、Teams 和 Enterprise 用户。在 CLI 中按 **Shift+Tab** 在 `Ask permissions` → `Plan mode` → `Auto mode` 之间循环切换，或在 Desktop 或 VS Code 中从下拉菜单选择。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/1.png" alt="Boris Cherny on auto mode" width="50%" /></a>

---

## 2/ 新的 /fewer-permission-prompts Skill

Anthropic 发布了一个新的 `/fewer-permission-prompts` skill。它扫描你的 session history，找出安全但反复请求权限的常用 bash 和 MCP 命令。然后推荐一个命令列表添加到你的 permissions allowlist 中。

使用这个来优化你的 permissions 并避免不必要的 permission prompts，特别是如果你不使用 auto mode。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/2.png" alt="Boris Cherny on /fewer-permission-prompts skill" width="50%" /></a>

---

## 3/ Recaps

Anthropic 本周早些时候发布了 **recaps**，为 Opus 4.7 做准备。Recaps 是 agent 做了什么以及接下来要做什么的简短摘要。

在几分钟或几小时后返回到长时间运行的 session 时非常有用：

```
* Cogitated for 6m 27s

* recap: Fixing the post-submit transcript shift bug. The styling-flash
  part is shipped as PR #29869 (auto-merge on, posted to stamps). Next:
  I need a screen recording of the remaining horizontal rewrap on `cc -c`
  to target that separate cause. (disable recaps in /config)
```

如果不需要，可以在 `/config` 中禁用 recaps。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/3.png" alt="Boris Cherny on recaps" width="50%" /></a>

---

## 4/ Focus Mode

Boris 一直很喜欢 CLI 中新的 **focus mode**，它隐藏所有中间工作，只关注最终结果。Model 已经达到一个水平，他通常信任它运行正确的命令并做出正确的编辑。他只看最终结果。

使用 `/focus` 切换开关。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/4.png" alt="Boris Cherny on focus mode" width="50%" /></a>

---

## 5/ 配置你的 Effort Level

Opus 4.7 使用 **adaptive thinking** 而不是 thinking budgets。要调整 model 思考更多或更少，调整 effort。

- **Lower effort** — 更快的响应和更低的 token usage
- **Higher effort** — 最大的智能和能力

滑块提供五个级别：`low` · `medium` · `high` · `xhigh` · `max` — 左侧是 Speed，右侧是 Intelligence。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/5.png" alt="Boris Cherny on effort levels" width="50%" /></a>

---

## 6/ 给 Claude 一种验证其工作的方法

最后，确保 Claude 有办法验证其工作。这一直很重要 — 现在 4.7 能让你从 Claude 获得的产出提高 2-3 倍，所以比以往任何时候都重要。

Verification 根据任务不同看起来也不同：

- **Backend work** — 让 Claude 运行你的 server/service 以端到端测试
- **Frontend work** — 使用 [Claude Chromium extension](https://code.claude.com/docs/en/chrome) 给 Claude 一种控制你的 browser 的方式
- **Desktop apps** — 使用 Computer Use

Boris 现在的 prompts 看起来像 `Claude do blah blah /go`，其中 `/go` 是一个 skill，它：

1. 使用 bash、browser 或 computer use 进行端到端自测
2. 运行 `/simplify`
3. 提交 PR

对于长时间运行的工作，verification 更加重要 — 当你返回到一个任务时，你知道代码可以正常工作。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/6.png" alt="Boris Cherny on verification" width="50%" /></a>

---

## Sources

- [Boris Cherny (@bcherny) on X — April 16, 2026](https://x.com/bcherny)
