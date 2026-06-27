---
description: 通过并行研究全部 5 个 skill-collection 仓库来更新 SKILL COLLECTIONS 表
---

# Workflow — Skill Collections

通过并行研究 5 个仓库来更新 `README.md` 中的 SKILL COLLECTIONS 表。启动一个研究 agent，合并结果，呈报变更，获批后更新表。

---

## 5 个仓库

| # | Repo | Owner |
|---|------|-------|
| 1 | `anthropics/skills` | Anthropic（官方） |
| 2 | `wshobson/agents` | William Shobson |
| 3 | `mattpocock/skills` | Matt Pocock |
| 4 | `K-Dense-AI/scientific-agent-skills` | K-Dense-AI |
| 5 | `VoltAgent/awesome-agent-skills` | VoltAgent（精选 awesome-list） |

---

## 表格格式

README 表包含以下列：

```markdown
| Name | ★ | <img src="!/tags/s.svg" height="14"> |
```

- **Name**：`[Short Name](github-url)` — 使用仓库的简短名称（例如 `mattpocock/skills`，或者如果 owner 是项目名则只用 `skills`），除非有歧义否则不用完整的 owner/repo
- **★**：Star 计数四舍五入到 `k`（例如 125k、35k、1.2k）。低于 1000 显示精确数字
- **Skill count**：仅数字。对于 skills 是链接而非文件的 awesome-lists，使用 `N+ (curated list)` 形式

**排序方式**：按 stars 降序排列（最高优先）。

---

## Phase 0: 读取当前状态

读取以下文件：

1. `README.md` — `## 🧰 SKILL COLLECTIONS` 表（记录当前 stars 和 skill counts）
2. `changelog/skill-collections/changelog.md` — 之前的 changelog 条目（可能尚不存在）

---

## Phase 1: 启动研究 Agent

**立即**启动一个 `development-workflows-research-agent` 覆盖全部 5 个仓库。（现有的研究 agent 是通用的 — 它可以统计任何仓库的 skills/stars/etc。）

> 研究以下 5 个 Claude Code **skill-collection** 仓库。每个仓库主要是 `SKILL.md` 文件的库，而非完整的 workflow 方法论。
>
> **Repo 1: anthropics/skills** (https://github.com/anthropics/skills) — 官方 Anthropic skills 仓库
> **Repo 2: wshobson/agents** (https://github.com/wshobson/agents) — 插件作用域 skills（skills 嵌套在 domain plugins 下）
> **Repo 3: mattpocock/skills** (https://github.com/mattpocock/skills) — 以 TypeScript 为重点
> **Repo 4: K-Dense-AI/scientific-agent-skills** (https://github.com/K-Dense-AI/scientific-agent-skills) — 科学/研究垂直领域
> **Repo 5: VoltAgent/awesome-agent-skills** (https://github.com/VoltAgent/awesome-agent-skills) — 精选 awesome-list（链接到外部 skills，而非仓库中的 SKILL.md 文件）
>
> 对每个仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Skill count** — 通过 GitHub git tree API 统计仓库中的 `SKILL.md` 文件数：
>    `https://api.github.com/repos/{owner}/{repo}/git/trees/HEAD?recursive=1` 并 grep 路径中的 `SKILL.md`。
>    - 对于 `wshobson/agents`：skills 嵌套在 `plugins/<domain>/skills/` 中 — 统计所有插件中的所有 SKILL.md。
>    - 对于 `VoltAgent/awesome-agent-skills``：统计 README.md 中列出的 skills（例如 bullet 项/表格行）。明确标记为 "curated list, not files"。
>    - 对于 `K-Dense-AI/scientific-agent-skills`：`skills/` 下的子目录可能使用 SKILL.md 或 `.md`；统计仓库实际使用的方式，并报告使用的是哪种。
>    - 对于 `anthropics/skills`：skills 位于 `skills/` 下的子目录中，每个包含 `SKILL.md`。
>    - 对于 `mattpocock/skills`：仅包含 **active** skills，不包括已弃用的（如果明显，记录两者数量）。
> 3. **Notable changes** — 过去 30 天内是否有任何显著的添加或移除？
>
> 返回每个仓库的结构化报告：
> ```
> REPO: anthropics/skills
> STARS: <number>k (<exact>)
> SKILLS: <count> (<file pattern used, e.g., "SKILL.md files via git tree">)
> NOTES: <anything unusual — flat .md vs SKILL.md, deprecated skills, language variants, curated-list disclaimer>
> CHANGES: <changes or "No significant changes">
> CONFIDENCE: <0-1>
> ```

---

## Phase 2: 比较与报告

**等待 agent。** 然后将发现与当前表进行比较并呈报：

```
Skill Collections — Update Report
══════════════════════════════════

Changes Found:
  <repo>: ★ <old>k → <new>k | skills <old>→<new>
  ...

No Changes:
  <repo>: ✓ (all values match)
  ...

Action Items:
#  | Type   | Action                              | Status
1  | Star   | Update <repo> ★ from Xk to Yk       | NEW/RECURRING
2  | Count  | Update <repo> skills from X to Y    | NEW/RECURRING
3  | Sort   | Move <repo> (rank changed)          | NEW/RECURRING
4  | Add    | New collection candidate: <repo>     | NEW
```

与之前的 changelog 条目进行比较，将条目标记为 `NEW`、`RECURRING` 或 `RESOLVED`。

---

## Phase 2.5: 追加到 Changelog

**强制的** — 始终在呈报给用户之前执行。

读取 `changelog/skill-collections/changelog.md`，然后**追加**一个新条目。如果文件不存在，先创建带有 Status Legend 的条目，然后是第一个条目。

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Skill Collections Update

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action> | <status> |
```

通过 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取时间。Status 必须是以下之一：
- `COMPLETE (reason)` | `INVALID (reason)` | `ON HOLD (reason)`

始终追加，绝不覆盖。

---

## Phase 2.6: 更新 Last Updated Badge

**强制的** — 在 Phase 2.5 之后执行。

更新 `README.md` 第 4 行的 badge。通过 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，对其进行 URL 编码，替换 badge 中的日期。不要将此项记录为 action item。

---

## Phase 3: 执行

询问用户：**(1) Execute all** | **(2) Execute specific** | **(3) Skip**

执行时，编辑 `README.md` 中的 `## 🧰 SKILL COLLECTIONS` 表：
- 更新每行的 stars 和 skill counts
- 保持排序：stars 降序（最高优先）
- 严格匹配现有格式（链接风格、stars 用 k 后缀）

---

## 规则

1. **一个研究 agent，5 个仓库** — 单条消息，内部并行子获取
2. **绝不猜测** — 仅使用 agent 的数据
3. **不要自动执行** — 先呈报报告，等待批准
4. **始终追加 changelog** 和 **始终更新 badge** — 强制的
5. **按 stars 降序排序** — 最高优先
6. **统一四舍五入 stars** — `k` 后缀（125k、35k、1.2k）。低于 1000 显示精确数字
7. **Awesome-lists 是不同的** — 对于链接到外部 skills 的仓库（VoltAgent），计数是 "README 中列出的条目数"，而非仓库中的文件数；始终标注 `(curated list)`
8. **与之前的 changelog 进行比较** — 标记条目为 NEW、RECURRING 或 RESOLVED
9. **重用 `development-workflows-research-agent`** — 不要创建新的 agent
