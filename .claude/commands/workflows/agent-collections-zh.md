---
description: 通过并行研究所有 agent-collection 仓库来更新 AGENT COLLECTIONS 表格
---

# Workflow — Agent Collections

通过并行研究所列的仓库来更新 `README.md` 中的 AGENT COLLECTIONS 表格。启动研究 agent，合并结果，呈现变更，获批后更新表格。

---

## 仓库列表

| # | Repo | Owner |
|---|------|-------|
| 1 | `msitarzewski/agency-agents` | msitarzewski |
| 2 | `VoltAgent/awesome-claude-code-subagents` | VoltAgent（精选 awesome-list） |

> 当发现新的 agent-collection 仓库时，将其添加到此列表以及阶段 1 的研究 prompt 中。

---

## 表格格式

README 表格包含以下列：

```markdown
| Name | ★ | <img src="!/tags/a.svg" height="14"> |
```

- **Name**：`[简短名称](github-url)`——使用仓库可识别的简短名称（例如 `msitarzewski/agency-agents`、`awesome-claude-code-subagents`）。仅当裸名称有歧义时才使用完整的 `owner/repo` 格式。
- **★**：Star 数，四舍五入到 `k`（例如 92k、19k、1.2k）。不足 1000 显示精确数字。
- **Agent 数量**：仅数字。对于 agents 以*链接*而非文件形式存在的 awesome-list，使用 `N+ (curated list)` 形式。

**排序规则**：按 star 数降序排列（最高在前）。

---

## 阶段 0：读取当前状态

读取以下文件：

1. `README.md` — `## 🤖 AGENT COLLECTIONS` 表格（记录当前 star 数和 agent 数量）
2. `changelog/agent-collections/changelog.md` — 之前的 changelog 条目（可能尚不存在——首次运行时创建）

---

## 阶段 1：启动研究 Agent

**立即**启动一个 `development-workflows-research-agent`，覆盖所有仓库。（现有的研究 agent 是通用的——它可以统计任何仓库的 agents/skills/commands/stars。）

> 研究以下 Claude Code **agent-collection** 仓库。每个仓库主要是一个 subagent 定义文件库（定义 agents 的 `.md` 文件），而非完整的工作流方法论。
>
> **仓库 1：msitarzewski/agency-agents** (https://github.com/msitarzewski/agency-agents) — agency 风格的 subagent 集合
> **仓库 2：VoltAgent/awesome-claude-code-subagents** (https://github.com/VoltAgent/awesome-claude-code-subagents) — 精选 awesome-list（链接到外部 subagents，并非所有 agents 都存储为仓库中的文件）
>
> 对每个仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Agent 数量** — 通过 GitHub git tree API 统计 subagent 定义 `.md` 文件：
>    `https://api.github.com/repos/{owner}/{repo}/git/trees/HEAD?recursive=1` 并 grep 常规 agent 目录下的路径。
>    - 对于 `msitarzewski/agency-agents`：agents 通常位于 `agents/`、`.claude/agents/` 或分类子目录下。统计看起来像 subagent 定义的 `.md` 文件（带有 `name:` 和 `description:` 的 frontmatter）。排除 README/CHANGELOG/LICENSE/docs。
>    - 对于 `VoltAgent/awesome-claude-code-subagents`：统计 README.md 中*列出*的 agents（例如，链接到外部仓库的项目符号/表格行）。明确标记为 "curated list, not files in repo"。
>    - 如果仓库同时拥有精选索引和自有 agent 文件，报告两个数字并加以说明。
> 3. **显著变化** — 过去 30 天内是否有任何重要的新增或移除？
>
> 返回每个仓库的结构化报告：
> ```
> REPO: msitarzewski/agency-agents
> STARS: <number>k (<exact>)
> AGENTS: <count> (<使用的文件匹配模式，例如 ".md files under agents/ via git tree">)
> NOTES: <任何异常——扁平布局 vs 分门别类、README-only 目录、已弃用的 agents、curated-list 免责声明>
> CHANGES: <变更内容或 "No significant changes">
> CONFIDENCE: <0-1>
> ```

---

## 阶段 2：比较并报告

**等待 agent 完成。** 然后将发现结果与当前表格进行比较并呈现：

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

与之前的 changelog 条目比较，将项目标记为 `NEW`、`RECURRING` 或 `RESOLVED`。

---

## 阶段 2.5：追加到 Changelog

**强制步骤** — 始终在呈现给用户之前执行。

读取 `changelog/agent-collections/changelog.md`，然后**追加**一个新条目。如果文件不存在，创建一个带有 Status Legend 的文件，然后添加第一个条目。

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

## 阶段 2.6：更新 Last Updated Badge

**强制步骤** — 在阶段 2.5 之后执行。

更新 `README.md` 第 4 行的 badge。通过 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，对其进行 URL 编码，替换 badge 中的日期部分。不要将此记录为 action item。

---

## 阶段 3：执行

询问用户：**(1) Execute all** | **(2) Execute specific** | **(3) Skip**

执行时，编辑 `README.md` 中的 `## 🤖 AGENT COLLECTIONS` 表格：
- 逐行更新 star 数和 agent 数量
- 保持排序规则：star 数降序（最高在前）
- 严格匹配现有格式（链接样式、star 数的 k 后缀）

---

## 规则

1. **一个研究 agent，所有仓库** — 单条消息，内部并行子获取
2. **绝不猜测** — 仅使用 agent 的数据
3. **不要自动执行** — 先呈现报告，等待批准
4. **始终追加 changelog**，**始终更新 badge** — 强制步骤
5. **按 star 数降序排列** — 最高 star 数在前
6. **统一四舍五入 star 数** — `k` 后缀（92k、19k、1.2k）。不足 1000 显示精确数字
7. **Awesome-list 不同** — 对于链接到外部 agents 的仓库（VoltAgent），数量是 "README 中列出的条目"，而非仓库中的文件；始终标注 `(curated list)`
8. **与之前的 changelog 比较** — 将项目标记为 NEW、RECURRING 或 RESOLVED
9. **复用 `development-workflows-research-agent`** — 不要创建新的 agent
