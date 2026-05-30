# Claude Code：使用量、速率限制与额外用量

了解 Claude Code 中的使用量限制工作原理，以及在触及限制时如何继续工作。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

订阅计划（Pro、Max 5x、Max 20x）上的 Claude Code 具有使用量限制，这些限制会在滚动时间窗口内重置。三个内置的斜杠命令可帮助你监控和管理用量：

| 命令 | 描述 | 适用用户 |
|---------|-------------|--------------|
| `/usage` | 查看计划限制和速率限制状态 | Pro、Max 5x、Max 20x |
| `/extra-usage` | 配置达到限制时的按量付费溢出用量 | Pro、Max 5x、Max 20x |
| `/cost` | 显示当前会话的 token 用量和花费 | API 密钥用户 |

---

## `/usage` — 查看你的限制

显示你当前计划的用量限制和速率限制状态。在达到限制前检查还有多少可用容量时非常有用。

---

## `/extra-usage` — 超出限制后继续工作

`/extra-usage` 命令配置**按量付费溢出计费**，这样当触及计划的速率限制时，Claude Code 会无缝继续工作，而不是阻止你。

### 工作原理

1. 你触及计划的速率限制（限制每 5 小时重置一次）
2. 如果启用了额外用量且账户有可用资金，Claude Code 将无中断地继续运行
3. 溢出 token 按**标准 API 费率**计费，与你的订阅费用分开

### 设置方法

CLI 中的 `/extra-usage` 命令将引导你完成配置。你也可以在 claude.ai 网站上通过 **设置 > 用量** 进行配置：

1. 启用额外用量
2. 添加支付方式
3. 设置**每月支出上限**（或选择无上限）
4. 可选：添加**预付费资金**，并在余额低于阈值时自动充值

### 关键细节

| 详情 | 数值 |
|--------|-------|
| 每日赎回限额 | 2,000 美元/天 |
| 计费方式 | 与订阅分开，按标准 API 费率计费 |
| 限制重置周期 | 每 5 小时 |

### 已知问题

截至 2026 年 2 月，`/extra-usage` CLI 命令尚未记录在文档中，并且可能会打开一个登录窗口而没有清晰的配置选项。目前，通过 **claude.ai 网页界面** 进行配置是更可靠的方式。

---

## `/cost` — 会话花费（API 用户）

对于使用 API 密钥（而非订阅计划）进行身份验证的用户，`/cost` 显示：

- 当前会话的总花费
- API 持续时间和实际运行时间
- Token 用量明细
- 已做出的代码更改

此命令不适用于 Pro/Max 订阅用户。

---

## 快速模式与额外用量

快速模式（`/fast`）使用 Claude Opus 4.6 并提供更快的输出。它与额外用量有特殊的计费关系：

- 快速模式从第一个 token 起**始终计入额外用量**
- 即使你的订阅计划中还有剩余用量，此规则同样适用
- 快速模式不消耗你计划中附带的速率限制

这意味着你需要启用并充值额外用量才能使用 `/fast`。

---

## CLI 启动参数

两个与用量预算相关的启动参数（仅限 API 密钥用户，print 模式）：

| 参数 | 描述 |
|------|-------------|
| `--max-budget-usd <AMOUNT>` | 在停止前，API 调用的最大美元金额 |
| `--max-turns <NUMBER>` | 限制 agentic 轮次的数量 |

完整列表请参阅 [CLI 启动参数参考](claude-cli-startup-flags.md)。

---

## 来源

- [付费 Claude 计划的额外用量 — Claude 帮助中心](https://support.claude.com/en/articles/12429409-extra-usage-for-paid-claude-plans)
- [使用你的 Pro 或 Max 计划使用 Claude Code — Claude 帮助中心](https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan)
- [/extra-usage 斜杠命令尚未记录 — GitHub Issue #12396](https://github.com/anthropics/claude-code/issues/12396)
- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
