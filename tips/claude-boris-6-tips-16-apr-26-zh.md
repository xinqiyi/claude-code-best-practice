# 从 Opus 4.7 中获得更多收益的 6 个技巧 — 来自 Boris Cherny

Boris Cherny（[@bcherny](https://x.com/bcherny)），Claude Code 的创建者，于 2026 年 4 月 16 日分享的一系列技巧 — 来自过去几周亲身使用 Opus 4.7 的经验。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 背景

在亲身使用 Opus 4.7 几周后，Boris 感到"效率惊人地高"，并分享了六种从新模型中获得更多收益的方法 — 从权限自动化到 effort 调优，再到验证模式。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/0.png" alt="Boris Cherny 介绍推文 — 亲身使用 Opus 4.7" width="50%" /></a>

---

## 1/ 自动模式 — 不再有权限提示

Opus 4.7 热爱执行复杂、长时间运行的任务：深度研究、重构代码、构建复杂功能、迭代直到达到性能基准。过去，你要么需要在模型执行这类长时间任务时照看它，要么使用 `--dangerously-skip-permissions`。

Anthropic 最近推出了**自动模式**作为更安全的替代方案。在此模式下，权限提示被路由到基于模型的分类器，由它决定命令是否安全执行：

- 如果安全，自动批准
- 如果风险高，暂停并询问

这意味着模型运行时不再需要照看。更重要的是，这意味着你可以并行运行更多 Claude 实例 — 如果安全，你可以将注意力切换到下一个 Claude。

自动模式现已对 Max、Teams 和 Enterprise 用户的 Opus 4.7 开放。使用 **Shift+Tab** 可在 CLI 中循环切换 `Ask permissions` → `Plan mode` → `Auto mode`，或在 Desktop 或 VS Code 的下拉菜单中选择。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/1.png" alt="Boris Cherny 谈自动模式" width="50%" /></a>

---

## 2/ 新的 /fewer-permission-prompts skill

Anthropic 发布了一个新的 `/fewer-permission-prompts` skill。它会扫描你的会话历史，查找常见且安全但反复要求权限的 bash 和 MCP 命令。然后推荐一系列命令添加到你的权限允许列表中。

使用它来优化你的权限设置，避免不必要的权限提示，特别是在你不使用自动模式的情况下。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/2.png" alt="Boris Cherny 谈 /fewer-permission-prompts skill" width="50%" /></a>

---

## 3/ Recaps（摘要回顾）

Anthropic 本周早些时候推出了 **recaps**，为 Opus 4.7 做准备。Recaps 是 agent 所做工作的简短摘要以及下一步计划。

当你在几分钟或几小时后回到一个长时间运行的会话时非常有用：

```
* Cogitated for 6m 27s

* recap: Fixing the post-submit transcript shift bug. The styling-flash
  part is shipped as PR #29869 (auto-merge on, posted to stamps). Next:
  I need a screen recording of the remaining horizontal rewrap on `cc -c`
  to target that separate cause. (disable recaps in /config)
```

如果你不需要 recaps，可以在 `/config` 中禁用它。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/3.png" alt="Boris Cherny 谈 recaps" width="50%" /></a>

---

## 4/ 专注模式

Boris 一直很喜欢 CLI 中的新**专注模式**，它隐藏所有中间工作，只关注最终结果。模型已经达到了他通常信任它执行正确命令和做出正确修改的程度。他只看最终结果。

使用 `/focus` 来开关。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/4.png" alt="Boris Cherny 谈专注模式" width="50%" /></a>

---

## 5/ 配置你的 Effort 级别

Opus 4.7 使用**自适应思考**而不是思考预算。要调整模型思考更多或更少，请调整 effort。

- **较低的 effort** — 更快的响应和更少的 token 使用量
- **较高的 effort** — 最强的智能和能力

滑动条提供五个级别：`low` · `medium` · `high` · `xhigh` · `max` — 左侧为速度，右侧为智能。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/5.png" alt="Boris Cherny 谈 effort 级别" width="50%" /></a>

---

## 6/ 给 Claude 提供验证其工作的方式

最后，确保 Claude 有办法验证其工作。这一点一直很重要 — 现在 4.7 的能力是以前的 2-3 倍，因此比以往任何时候都更加重要。

验证方式因任务而异：

- **后端工作** — 让 Claude 运行你的 server/service 进行端到端测试
- **前端工作** — 使用 [Claude Chromium 扩展](https://code.claude.com/docs/en/chrome)让 Claude 能够控制你的浏览器
- **桌面应用** — 使用 Computer Use

Boris 现在的 prompt 通常是这样的 `Claude do blah blah /go`，其中 `/go` 是一个 skill，它：

1. 使用 bash、browser 或 computer use 进行端到端自测
2. 运行 `/simplify`
3. 提交 PR

对于长时间运行的工作，验证更为重要 — 当你回到任务时，你知道代码是能正常工作的。

<a href="https://x.com/bcherny"><img src="assets/boris-26-4-16/6.png" alt="Boris Cherny 谈验证" width="50%" /></a>

---

## 来源

- [Boris Cherny (@bcherny) 在 X 上 — 2026 年 4 月 16 日](https://x.com/bcherny)
