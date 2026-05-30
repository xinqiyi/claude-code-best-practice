# 定时任务实现

![上次更新](https://img.shields.io/badge/Last_Updated-Mar_10%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#loop-demo"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

`/loop` skill 用于在 cron 间隔上调度重复任务。以下是 `/loop 1m "tell current time"` 的演示 — 一个每分钟触发一次的简单重复任务。

---

## Loop 演示

### 1. 调度任务

<p align="center">
  <img src="assets/impl-loop-1.png" alt="/loop 1m tell current time — 调度和 cron 设置" width="100%">
</p>

`/loop 1m "tell current time"` 解析间隔（`1m` → 每分钟一次），创建一个 cron job，并确认调度。关键注意点：

- Cron 的最小粒度是 **1 分钟** — `1m` 映射为 `*/1 * * * *`
- 重复任务 **3 天后自动过期**
- Job 是 **会话作用域** 的 — 它们仅存在于内存中，当 Claude 退出时停止
- 随时使用 `cron cancel <job-id>` 取消

---

### 2. Loop 运行中

<p align="center">
  <img src="assets/impl-loop-2.png" alt="每分钟触发的重复任务" width="100%">
</p>

任务每分钟触发一次，运行 `date` 并报告当前时间。每次迭代都会触发异步的 **UserPromptSubmit** 和 **Stop** hooks — 与本仓库用于声音通知的 hook 系统相同。

---

## ![使用方法](../!/tags/how-to-use.svg)

```bash
$ claude
> /loop 1m "tell current time"
> /loop 5m /simplify
> /loop 10m "check deploy status"
```

---

## ![实现方法](../!/tags/how-to-implement.svg)

`/loop` 是一个内置的 Claude Code skill — 无需任何设置。它在底层使用 cron 工具（`CronCreate`、`CronList`、`CronDelete`）来管理重复调度。
