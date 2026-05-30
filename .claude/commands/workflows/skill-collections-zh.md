---
description: 通过并行研究所有 5 个 skill-collection 仓库来更新 SKILL COLLECTIONS 表格
---

# 工作流 — Skill Collections

通过并行研究 5 个仓库来更新 `README.md` 中的 SKILL COLLECTIONS 表格。启动一个 research agent，合并结果，展示变更，如果获得批准则更新表格。

---

## 5 个仓库

| # | 仓库 | 所有者 |
|---|------|-------|
| 1 | `anthropics/skills` | Anthropic（官方） |
| 2 | `wshobson/agents` | William Shobson |
| 3 | `mattpocock/skills` | Matt Pocock |
| 4 | `K-Dense-AI/scientific-agent-skills` | K-Dense-AI |
| 5 | `VoltAgent/awesome-agent-skills` | VoltAgent（精选 awesome-list） |

---

## 表格格式

README 表格包含以下列：

```markdown
| Name | ★ | <img src="!/tags/s.svg" height="14"> |
```

- **Name**: `[Short Name](github-url)` — 使用仓库的短名称（例如 `mattpocock/skills`，如果所有者就是项目本身则只用 `skills`），除非有歧义，否则不使用完整 owner/repo
- **★**: 星标数四舍五入到 `k`（例如 125k、35k、1.2k）。低于 1000 显示确切数字
- **Skill count**: 仅数字。对于 skill 是 *链接* 而非文件的 awesome-list，使用 `N+ (curated list)` 格式

**排序顺序**：按星标数降序排列（最高在前）。

---

## 阶段 0：读取当前状态

读取以下文件：

1. `README.md` — 其中的 `## 🧰 SKILL COLLECTIONS` 表格（记录当前星标数和 skill 数量）
2. `changelog/skill-collections/changelog.md` — 之前的变更日志条目（可能尚不存在）

---

## 阶段 1：启动 Research Agent

**立即**生成一个覆盖所有 5 个仓库的 `development-workflows-research-agent`。（现有的 research agent 是通用型的——它能为任何仓库统计 skill/星标数等。）

> 研究以下 5 个 Claude Code **skill-collection** 仓库。每个仓库主要是一个 `SKILL.md` 文件的库，而非完整的工作流方法论。
>
> **仓库 1: anthropics/skills** (https://github.com/anthropics/skills) — 官方 Anthropic skills 仓库
> **仓库 2: wshobson/agents** (https://github.com/wshobson/agents) — plugin 作用域下的 skills（skill 嵌套在领域 plugin 下）
> **仓库 3: mattpocock/skills** (https://github.com/mattpocock/skills) — 专注于 TypeScript
> **仓库 4: K-Dense-AI/scientific-agent-skills** (https://github.com/K-Dense-AI/scientific-agent-skills) — 科学/研究垂直领域
> **仓库 5: VoltAgent/awesome-agent-skills** (https://github.com/VoltAgent/awesome-agent-skills) — 精选 awesome-list（链接到外部 skill，而非仓库内的 SKILL.md 文件）
>
> 对每个仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Skill count** — 通过 GitHub git tree API 统计仓库中的 `SKILL.md` 文件数：
>    `https://api.github.com/repos/{owner}/{repo}/git/trees/HEAD?recursive=1` 并搜索路径中的 `SKILL.md`。
>    - 对于 `wshobson/agents`：skill 嵌套在 `plugins/<domain>/skills/` 内——统计所有 plugin 中的所有 SKILL.md。
>    - 对于 `VoltAgent/awesome-agent-skills`：统计 README.md 中 *列出* 的 skill（例如，列表项/表格行）。明确标注为 "curated list, not files"。
>    - 对于 `K-Dense-AI/scientific-agent-skills`：`skills/` 下的子目录可能使用 SKILL.md 或 `.md`；统计仓库实际使用的格式，并报告使用哪种。
>    - 对于 `anthropics/skills`：skill 位于 `skills/` 下的子目录中，内含 `SKILL.md`。
>    - 对于 `mattpocock/skills`：仅统计 **活跃的** skill，不包括已弃用的（如果区分明显请注明两个数字）。
> 3. **Notable changes** — 过去 30 天内是否有任何重大新增或删除？
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

## 阶段 2：比较与报告

**等待 agent 完成。** 然后将结果与当前表格进行比较并展示：

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

与之前的变更日志条目进行比较，将项目标记为 `NEW`、`RECURRING` 或 `RESOLVED`。

---

## 阶段 2.5：追加到变更日志

**必需** — 在向用户展示之前务必执行。

读取 `changelog/skill-collections/changelog.md`，然后 **追加** 一个新条目。如果文件不存在，则创建包含状态图例的文件，然后添加第一个条目。

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Skill Collections Update

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action> | <status> |
```

通过 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取时间。Status 必须是以下之一：
- `COMPLETE (reason)` | `INVALID (reason)` | `ON HOLD (reason)`

始终追加，从不覆盖。

---

## 阶段 2.6：更新最后更新徽章

**必需** — 在阶段 2.5 之后执行。

更新 `README.md` 第 4 行的徽章。通过 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码，替换徽章中的日期。不要将此记录为 action item。

---

## 阶段 3：执行

询问用户：**(1) 全部执行** | **(2) 选择执行** | **(3) 跳过**

执行时，编辑 `README.md` 中的 `## 🧰 SKILL COLLECTIONS` 表格：
- 更新每行的星标数和 skill 数量
- 保持排序顺序：星标数降序（最高在前）
- 完全匹配现有格式（链接样式、星标的 k 后缀）

---

## 规则

1. **一个 research agent，5 个仓库** — 单条消息，内部并行子请求
2. **绝不猜测** — 仅使用 agent 提供的数据
3. **不自动执行** — 先展示报告，等待批准
4. **始终追加变更日志** 和 **始终更新徽章** — 必须执行
5. **按星标数降序排序** — 星标最高的在前
6. **星标数统一四舍五入** — `k` 后缀（125k、35k、1.2k）。低于 1000 显示确切数字
7. **Awesome-list 不同** — 对于链接到外部 skill 的仓库（如 VoltAgent），计数为 "README 中列出的项目数"，而非仓库中的文件数；始终标注 `(curated list)`
8. **与之前的变更日志进行比较** — 将项目标记为 NEW、RECURRING 或 RESOLVED
9. **重用 `development-workflows-research-agent`** — 不要创建新的 agent
