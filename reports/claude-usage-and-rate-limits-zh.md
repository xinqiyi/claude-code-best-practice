# Claude Code：用量、速率限制与额外用量

理解 Claude Code 中用量限制的工作原理，以及达到限制时如何继续工作。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

Claude Code 在订阅计划（Pro、Max 5x、Max 20x）上有用量限制，按滚动窗口重置。三个内置 slash 命令帮助你监控和管理用量：

| Command | Description | Available To |
|---------|-------------|--------------|
| `/usage` | 检查计划限制和速率限制状态 | Pro、Max 5x、Max 20x |
| `/extra-usage` | 配置达到限制时的按量付费溢出 | Pro、Max 5x、Max 20x |
| `/cost` | 显示当前 session 的 token 用量和花费 | API key 用户 |

---

## `/usage` — 检查你的限制

显示当前计划的用量限制和速率限制状态。适合用于检查在达到限制前还剩多少容量。

---

## `/extra-usage` — 超过限制后继续工作

`/extra-usage` 命令配置**按量付费溢出计费**，这样当你达到计划速率限制时，Claude Code 可以无缝继续工作，而不是阻止你。

### 工作原理

1. 你达到计划的速率限制（限制每 5 小时重置）
2. 如果启用了 extra usage 且有可用资金，Claude Code 无中断继续
3. 溢出 token 按**标准 API 费率**计费，与你的订阅费用分开

### 设置

CLI 中的 `/extra-usage` 命令会引导你完成配置。你也可以在 claude.ai 上的 **Settings > Usage** 进行配置：

1. 启用 extra usage
2. 添加支付方式
3. 设置**月度消费上限**（或选择无限制）
4. 可选：添加**预充资金**，余额低于阈值时自动充值

### 关键细节

| Detail | Value |
|--------|-------|
| 每日兑换限额 | $2,000/天 |
| 计费方式 | 与订阅分开，按标准 API 费率 |
| 限制重置窗口 | 每 5 小时 |

### 已知问题

截至 2026 年 2 月，`/extra-usage` CLI 命令[尚未有文档](https://github.com/anthropics/claude-code/issues/12396)，可能只会打开一个登录窗口而没有清晰的配置选项。目前通过 **claude.ai 网页界面**配置是更可靠的路径。

---

## `/cost` — Session 花费（API 用户）

对于使用 API key（而非订阅计划）认证的用户，`/cost` 显示：

- 当前 session 的总花费
- API 持续时间和实际时间
- Token 用量细分
- 所做的代码变更

此命令不适用于 Pro/Max 订阅用户。

---

## Fast Mode 与 Extra Usage

Fast mode（`/fast`）使用 Claude Opus 4.6，输出更快。它与 extra usage 有特殊的计费关系：

- Fast mode 的用量**始终从 extra usage 计费**，从第一个 token 开始
- 即使你的订阅计划还有剩余用量，也是如此
- Fast mode 不消耗你计划包含的速率限制

这意味着你需要启用并充值 extra usage 才能使用 `/fast`。

---

## CLI 启动标志

两个与用量预算相关的启动标志（仅限 API key 用户，print mode）：

| Flag | Description |
|------|-------------|
| `--max-budget-usd <AMOUNT>` | API 调用的最大美元金额 |
| `--max-turns <NUMBER>` | 限制 agentic turns 数量 |

完整列表参见 [CLI 启动标志参考](claude-cli-startup-flags.md)。

---

## Sources

- [Extra usage for paid Claude plans — Claude Help Center](https://support.claude.com/en/articles/12429409-extra-usage-for-paid-claude-plans)
- [Using Claude Code with your Pro or Max plan — Claude Help Center](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
- [/extra-usage slash command is undocumented — GitHub Issue #12396](https://github.com/anthropics/claude-code/issues/12396)
- [Claude Code CLI Reference](https://code.claude.com/docs/en/cli-reference)
