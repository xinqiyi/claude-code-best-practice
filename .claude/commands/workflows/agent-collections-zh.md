---
description: 通过并行调研所有 agent-collection 仓库来更新 AGENT COLLECTIONS 表格
---

# 工作流 — Agent Collections

通过并行调研列表中各仓库来更新 `README.md` 中的 AGENT COLLECTIONS 表格。启动一个 research agent，合并结果，展示变更内容，经批准后更新表格。

---

## 仓库列表

| # | 仓库 | 所有者 |
|---|------|-------|
| 1 | `msitarzewski/agency-agents` | msitarzewski |
| 2 | `VoltAgent/awesome-claude-code-subagents` | VoltAgent（精选 awesome-list） |

> 当发现新的 agent-collection 仓库时，将其添加到此列表以及 Phase 1 的调研 prompt 中。

---

## 表格格式

README 表格包含以下列：

```markdown
| Name | ★ | <img src="!/tags/a.svg" height="14"> |
```

- **Name**：`[Short Name](github-url)` — 使用仓库的易识别简称（如 `msitarzewski/agency-agents`、`awesome-claude-code-subagents`）。仅在简称有歧义时使用完整 `owner/repo` 格式。
- **★**：星级数四舍五入并以 `k` 为单位（如 92k、19k、1.2k）。低于 1000 显示精确数字。
- **Agent 数量**：仅数字。对于精选列表（awesome-list），其中的 agents 是*链接*而非文件，使用 `N+ (curated list)` 格式。

**排序顺序**：按星级数降序排列（最高在前）。

---

## Phase 0：读取当前状态

读取以下文件：

1. `README.md` — 其中的 `## 🤖 AGENT COLLECTIONS` 表格（记录当前星级数和 agent 数量）
2. `changelog/agent-collections/changelog.md` — 之前的 changelog 记录（首次运行时可能不存在 — 需创建）

---

## Phase 1：启动 Research Agent

**立即**启动一个涵盖所有仓库的 `development-workflows-research-agent`。（已有的 research agent 是通用型的 — 可统计任何仓库的 agents/skills/commands/stars 数量。）

> 调研以下 Claude Code **agent-collection** 仓库。这些仓库主要是 subagent 定义文件（定义 agent 的 `.md` 文件）的集合，而非完整的工作流方法论。
>
> **仓库 1：msitarzewski/agency-agents**（https://github.com/msitarzewski/agency-agents）— 机构风格 subagent 集合
> **仓库 2：VoltAgent/awesome-claude-code-subagents**（https://github.com/VoltAgent/awesome-claude-code-subagents）— 精选 awesome-list（指向外部 subagent 的链接，并非所有 agent 都作为文件存储在仓库中）
>
> 对**每个**仓库，返回：
>
> 1. **Stars（星级）** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Agent 数量** — 通过 GitHub git tree API 统计 subagent 定义 `.md` 文件数量：
>    `https://api.github.com/repos/{owner}/{repo}/git/trees/HEAD?recursive=1` 并筛选常规 agent 目录下的路径。
>    - 对于 `msitarzewski/agency-agents`：agent 通常位于 `agents/`、`.claude/agents/` 或分类子目录下。统计看起来像 subagent 定义（包含 `name:` 和 `description:` frontmatter）的 `.md` 文件。排除 README/CHANGELOG/LICENSE/docs。
>    - 对于 `VoltAgent/awesome-claude-code-subagents`：统计 README.md 中*列出*的 agent（如指向外部仓库的列表项/表格行）。明确标注为"curated list, not files in repo"。
>    - 如果某个仓库既有精选索引又有自己的 agent 文件，则同时报告两个数字并加以说明。
> 3. **值得注意的变更** — 过去 30 天内是否有重大新增或删除？
>
> 返回每个仓库的结构化报告：
> ```
> REPO: msitarzewski/agency-agents
> STARS: <number>k (<exact>)
> AGENTS: <count> (<file pattern used, e.g., ".md files under agents/ via git tree">)
> NOTES: <anything unusual — flat layout vs categorized, README-only catalog, deprecated agents, curated-list disclaimer>
> CHANGES: <changes or "No significant changes">
> CONFIDENCE: <0-1>
> ```

---

## Phase 2：比较与报告

**等待 agent 返回结果。** 然后将结果与当前表格进行比较并展示：

```
Agent Collections — Update Report
══════════════════════════════════

Changes Found:
  <repo>: ★ <old>k → <new>k | agents <old>→<new>
  ...

No Changes:
  <repo>: ✓ (all values match)
  ...

Action Items:
#  | Type   | Action                              | Status
1  | Star   | Update <repo> ★ from Xk to Yk       | NEW/RECURRING
2  | Count  | Update <repo> agents from X to Y    | NEW/RECURRING
3  | Sort   | Move <repo> (rank changed)          | NEW/RECURRING
4  | Add    | New collection candidate: <repo>     | NEW
```

与之前的 changelog 记录进行比较，将项目标记为 `NEW`、`RECURRING` 或 `RESOLVED`。

---

## Phase 2.5：追加 Changelog

**必须执行** — 在向用户展示前始终执行。

读取 `changelog/agent-collections/changelog.md`，然后**追加**一条新记录。如果文件不存在，先创建状态图例，再写入第一条记录。

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Agent Collections Update

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action> | <status> |
```

通过 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取时间。Status 必须是以下之一：
- `COMPLETE (reason)` | `INVALID (reason)` | `ON HOLD (reason)`

始终追加，绝不覆盖。

---

## Phase 2.6：更新 Last Updated Badge

**必须执行** — 在 Phase 2.5 之后执行。

更新 `README.md` 第 4 行的 badge。通过 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码，替换 badge 中的日期。不要将此记录为 action item。

---

## Phase 3：执行

询问用户：**(1) 全部执行** | **(2) 指定执行** | **(3) 跳过**

执行时，编辑 `README.md` 中的 `## 🤖 AGENT COLLECTIONS` 表格：
- 逐行更新星级数和 agent 数量
- 保持排序顺序：星级数降序（最高在前）
- 与现有格式完全一致（链接样式、星级数的 `k` 后缀）

---

## 规则

1. **一个 research agent，所有仓库** — 单条消息，内部并行子请求
2. **绝不猜测** — 仅使用 agent 返回的数据
3. **不自动执行** — 先展示报告，等待批准
4. **始终追加 changelog** 且 **始终更新 badge** — 必须执行
5. **按星级数降序排序** — 最高星级在前
6. **统一四舍五入星级** — `k` 后缀（92k、19k、1.2k）。低于 1000 显示精确数字
7. **精选列表（Awesome-list）有区别** — 对于链接到外部 agent 的仓库（如 VoltAgent），数量是"README 中列出的项目数"，而非仓库中的文件数；始终标注 `(curated list)`
8. **与之前的 changelog 进行比较** — 将项目标记为 NEW、RECURRING 或 RESOLVED
9. **复用 `development-workflows-research-agent`** — 不要创建新的 agent
