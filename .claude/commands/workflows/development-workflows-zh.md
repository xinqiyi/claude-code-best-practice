---
description: 通过并行研究全部 11 个 workflow 仓库来更新 DEVELOPMENT WORKFLOWS 表
---

# Workflow — Development Workflows

通过并行研究 11 个仓库来更新 `README.md` 中的 DEVELOPMENT WORKFLOWS 表。启动 agents，合并结果，呈报变更，获批后更新表。

---

## 11 个仓库

| # | Repo | Owner |
|---|------|-------|
| 1 | `github/spec-kit` | GitHub (John Lam / Den Delimarsky) |
| 2 | `Fission-AI/OpenSpec` | Fission-AI (@0xTab) |
| 3 | `humanlayer/humanlayer` | HumanLayer (Dex Horthy) |
| 4 | `affaan-m/everything-claude-code` | Affaan Mustafa |
| 5 | `gsd-build/get-shit-done` | Lex Christopherson |
| 6 | `obra/superpowers` | Jesse Vincent |
| 7 | `garrytan/gstack` | Garry Tan (YC CEO) |
| 8 | `bmad-code-org/BMAD-METHOD` | BMAD Code Org |
| 9 | `EveryInc/compound-engineering-plugin` | Every.to |
| 10 | `Yeachan-Heo/oh-my-claudecode` | Yeachan Heo (@bellman_ych) |
| 11 | `mattpocock/skills` | Matt Pocock |

---

## 表格格式

README 表包含以下列：

```markdown
| Name | ★ | Workflow | <img src="!/tags/a.svg" height="14"> | <img src="!/tags/c.svg" height="14"> | <img src="!/tags/s.svg" height="14"> |
```

- **Name**：`[Short Name](github-url)` — 使用项目名称而非 owner/repo
- **★**：Star 计数四舍五入到 `k`（例如 98k、10k、4.1k）。低于 1000 显示精确数字
- **Workflow**：规范化的端到端流水线，以扁平从左到右的 shields.io badges 序列表示，由 ` → ` 连接。每个步骤是仓库中的实际 command/skill/agent 名称（例如 `/speckit.plan`、`bmad-create-prd`、`subagent-driven-development`）。**仅限扁平的** — 不含括号、不含英文修饰语（"loop"、"per story"、"parallel waves"）、不含 `+` 连接符。如果某个步骤有重要的内部子步骤，将它们作为兄弟步骤列在主链中，并将它们**标为黄色（`fff3b0`）**以标记为 sub-loops；顶级步骤保持浅蓝色（`ddf4ff`）。追溯 README 的 "how to use" / "workflow" 部分以获取规范化的 happy path：idea → spec/plan → tasks → implement → review → ship。
- **Agent/Command/Skill 计数**：仅数字（例如 `25`、`0`、`108+`）

### Workflow badge 编码（shields.io）

每个步骤渲染为带有 `align="middle"` 的 **HTML `<img>` 标签**（而非 markdown 图片语法），使箭头与 badges 保持垂直居中。两种背景颜色：

| 颜色 | Hex | 使用场景 |
|---|---|---|
| 浅蓝色 | `ddf4ff` | 顶级 workflow 步骤 |
| 柔和黄色 | `fff3b0` | Sub-loop 步骤（在父步骤内部每 task/story/直到验证通过重复执行） |

模板：

```html
<img src="https://img.shields.io/badge/<ENCODED>-ddf4ff" alt="<plain-label>" align="middle">    <!-- top-level -->
<img src="https://img.shields.io/badge/<ENCODED>-fff3b0" alt="<plain-label>" align="middle">    <!-- sub-loop -->
```

`align="middle"` 将 badge 的垂直中心放在文本基线上，使 ` → ` 箭头在每个 badge 上居中，而不是位于 badge 底部。不这样做的话，在 GitHub 的渲染中箭头会明显掉到 badges 下方。

表格结束后，**始终在单独一行上以 blockquote 包含以下图例**：

```markdown
> *Note: yellow tags are sub-loops — steps that repeat inside a parent step (e.g. per task, per story, or until a verify condition passes).*
```

URL 中 `<ENCODED>` 部分的编码规则：

| 输入字符 | 编码为 |
|---|---|
| `/`（前导斜杠） | `%2F` |
| `-`（字面连字符） | `--` |
| `_`（字面下划线） | `__` |
| ` `（空格） | `_` |
| `+` | `%2B` |
| `.` 和 `:` | 保持不变 |

示例：
- `/grill-me` → `%2Fgrill--me`
- `/speckit.plan` → `%2Fspeckit.plan`
- `/opsx:propose` → `%2Fopsx:propose`
- `bmad-create-epics-and-stories` → `bmad--create--epics--and--stories`

在一个 img 标签的闭合 `>` 和下一个 img 标签的开放 `<` 之间使用字面箭头 ` → `（空格-箭头-空格）连接步骤。

**不要**将子步骤放在括号中或用英文注释（"loop"、"per story"、"+"、"parallel waves"）。如果某个步骤有内部循环，只需将内部步骤名称作为兄弟步骤列在扁平链中。

**排序方式**：按 stars 降序排列（最高优先）。

---

## Phase 0: 读取当前状态

读取以下文件：

1. `README.md` — `## ⚙️ DEVELOPMENT WORKFLOWS` 表（记录当前 stars、workflow pipelines、counts）
2. `changelog/development-workflows/changelog.md` — 之前的 changelog 条目

---

## Phase 1: 启动 2 个研究 Agent

**立即**在**单条消息**中（并行）启动两个 agent。每个使用 `subagent_type: "development-workflows-research-agent"`。

### Agent 1（4 个仓库）

> 研究以下 4 个 Claude Code workflow 仓库：
>
> **Repo 1: github/spec-kit** (https://github.com/github/spec-kit)
> **Repo 2: affaan-m/everything-claude-code** (https://github.com/affaan-m/everything-claude-code)
> **Repo 3: obra/superpowers** (https://github.com/obra/superpowers)
> **Repo 4: mattpocock/skills** (https://github.com/mattpocock/skills)
>
> 对每个仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Agent count** — 统计 `agents/` 或 `.claude/agents/` 中的 `.md` 文件数。对于 obra，同时统计由 skills 分派的隐式 sub-agents。对于 mattpocock，计数为 0（skills-only 仓库）。
> 3. **Skill count** — 统计 `skills/` 或 `.claude/skills/` 中的文件夹数。对于 mattpocock，统计仓库根目录 `skills/` 中的文件夹数。
> 4. **Command count** — 统计 `commands/` 或 `.claude/commands/` 中的 `.md` 文件数。对于 spec-kit，统计 `templates/commands/` 中的文件数。对于 mattpocock，计数为 0（skills 充当 slash commands）。
> 5. **Workflow** — 规范化的端到端流水线，以扁平从左到右的步骤名称序列表示，由 ` → ` 连接。追溯 README 的 "how to use" / "workflow" 部分以获取 happy path：idea → spec/plan → tasks → implement → review → ship。使用仓库中的实际 command/skill/agent 名称。**仅限扁平的** — 不含括号、不含英文修饰语（"loop"、"per story"、"parallel waves"）、不含 `+` 连接符。如果某个步骤有内部子步骤，将它们作为兄弟步骤列在主链中。将每个步骤标记为 `top`（顶级）或 `sub`（sub-loop，在父步骤内部重复），以便协调员可以为其着色。输出为纯文本 — 协调员会将每个步骤编码为 shields.io HTML img badge。
> 6. **Notable changes** — 是否有任何显著的近期变更？新的 agents/skills/commands，主要版本？
>
> 返回每个仓库的结构化报告：
> ```
> REPO: github/spec-kit
> STARS: <number>k
> AGENTS: <count>
> COMMANDS: <count>
> SKILLS: <count>
> WORKFLOW: <step1>(top) → <step2>(top) → <step3>(sub) → ... → <stepN>(top)
> CHANGES: <changes or "No significant changes">
> ```

### Agent 2（7 个仓库）

> 研究以下 7 个 Claude Code workflow 仓库：
>
> **Repo 1: Fission-AI/OpenSpec** (https://github.com/Fission-AI/OpenSpec)
> **Repo 2: humanlayer/humanlayer** (https://github.com/humanlayer/humanlayer)
> **Repo 3: gsd-build/get-shit-done** (https://github.com/gsd-build/get-shit-done)
> **Repo 4: garrytan/gstack** (https://github.com/garrytan/gstack)
> **Repo 5: bmad-code-org/BMAD-METHOD** (https://github.com/bmad-code-org/BMAD-METHOD)
> **Repo 6: EveryInc/compound-engineering-plugin** (https://github.com/EveryInc/compound-engineering-plugin)
> **Repo 7: Yeachan-Heo/oh-my-claudecode** (https://github.com/Yeachan-Heo/oh-my-claudecode)
>
> 对每个仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Agent count** — 统计 `agents/` 或 `.claude/agents/` 中的 `.md` 文件数。对于 BMAD，统计 `src/bmm-skills/` 中的 agent-persona skills。对于 compound-engineering-plugin，统计 `plugins/compound-engineering/agents/` 所有子目录中的 `.md` 文件。对于 oh-my-claudecode，统计仓库根目录 `agents/` 中的 `.md` 文件。
> 3. **Skill count** — 统计 `skills/` 或 `.claude/skills/` 中的文件夹数。对于 gstack，skills 是包含 SKILL.md 的根级目录。对于 BMAD，统计 `src/bmm-skills/` 和 `src/core-skills/` 中的所有 skills。对于 compound-engineering-plugin，统计 `plugins/compound-engineering/skills/` 加 `plugins/coding-tutor/skills/` 中的文件夹数。对于 oh-my-claudecode，统计仓库根目录 `skills/` 中的文件夹数。
> 4. **Command count** — 统计 `commands/` 或 `.claude/commands/` 中的 `.md` 文件数。对于 GSD，统计 `commands/gsd/` 中的文件数。对于 OpenSpec，统计 `/opsx:*` commands。对于 BMAD，计数为 0（commands 在安装时生成）。对于 compound-engineering-plugin，统计 `.claude/commands/` 加 `plugins/coding-tutor/commands/` 中的 `.md` 文件。对于 oh-my-claudecode，计数为 0（skills 充当 slash commands）。
> 5. **Workflow** — 规范化的端到端流水线，以扁平从左到右的步骤名称序列表示，由 ` → ` 连接。追溯 README 的 "how to use" / "workflow" 部分以获取 happy path：idea → spec/plan → tasks → implement → review → ship。使用仓库中的实际 command/skill/agent 名称。**仅限扁平的** — 不含括号、不含英文修饰语（"loop"、"per story"、"parallel waves"）、不含 `+` 连接符。如果某个步骤有内部子步骤，将它们作为兄弟步骤列在主链中。将每个步骤标记为 `top`（顶级）或 `sub`（sub-loop，在父步骤内部重复），以便协调员可以为其着色。输出为纯文本 — 协调员会将每个步骤编码为 shields.io HTML img badge。
> 6. **Notable changes** — 是否有任何显著的近期变更？新的 agents/skills/commands，主要版本？
>
> 返回每个仓库的结构化报告：
> ```
> REPO: Fission-AI/OpenSpec
> STARS: <number>k
> AGENTS: <count>
> COMMANDS: <count>
> SKILLS: <count>
> WORKFLOW: <step1>(top) → <step2>(top) → <step3>(sub) → ... → <stepN>(top)
> CHANGES: <changes or "No significant changes">
> ```

---

## Phase 2: 比较与报告

**等待两个 agent。** 然后将发现与当前表进行比较并呈报：

```
Development Workflows — Update Report
══════════════════════════════════════

Changes Found:
  <repo>: ★ <old>k → <new>k | agents <old>→<new> | commands <old>→<new> | skills <old>→<new>
  <repo>: workflow updated: <old workflow> → <new workflow>
  ...

No Changes:
  <repo>: ✓ (all values match)
  ...

Action Items:
#  | Type        | Action                                | Status
1  | Star        | Update <repo> ★ from Xk to Yk         | NEW/RECURRING
2  | Count       | Update <repo> agents from X to Y      | NEW/RECURRING
3  | Workflow    | Update <repo> workflow pipeline       | NEW/RECURRING
4  | Sort        | Move <repo> (stars changed)           | NEW/RECURRING
```

与之前的 changelog 条目进行比较，将条目标记为 `NEW`、`RECURRING` 或 `RESOLVED`。

---

## Phase 2.5: 追加到 Changelog

**强制的** — 始终在呈报给用户之前执行。

读取 `changelog/development-workflows/changelog.md`，然后**追加**一个新条目。如果文件不存在，先创建带有 Status Legend 的条目，然后是第一个条目。

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Development Workflows Update

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

执行时，编辑 `README.md` 中的 `## ⚙️ DEVELOPMENT WORKFLOWS` 表：
- 更新每行的 stars、counts 和 Workflow 列
- 保持排序：stars 降序（最高优先）
- 严格匹配现有格式（图标、badge URL、链接风格）
- 对于 Workflow 列，将 agent 返回的每个纯文本步骤按照表格格式部分编码为 shields.io HTML img badge。对标记为 `(top)` 的步骤使用 `ddf4ff`，对标记为 `(sub)` 的步骤使用 `fff3b0`。使用 ` → ` 连接。
- 确保图例 `> *Note: yellow tags are sub-loops — steps that repeat inside a parent step (e.g. per task, per story, or until a verify condition passes).*` 紧接在表格之后；如果缺失则添加。

---

## 规则

1. **并行启动两个 agent** — 单条消息，绝不顺序执行
2. **绝不猜测** — 仅使用 agents 的数据
3. **不要自动执行** — 先呈报报告，等待批准
4. **始终追加 changelog** 和 **始终更新 badge** — 强制的
5. **按 stars 降序排序** — 最高优先
6. **Workflow badges 使用带有 align="middle" 的 HTML img** — `<img src="https://img.shields.io/badge/<ENCODED>-<COLOR>" alt="<plain-label>" align="middle">`。`align="middle"` 是必需的，使 ` → ` 箭头与 badges 保持垂直居中。两种颜色：`ddf4ff` 用于顶级步骤，`fff3b0` 用于 sub-loop 步骤。编码：`_` 表示空格，`--` 表示连字符，`__` 表示下划线，`%2F` 表示 `/`，`%2B` 表示 `+`。点和冒号保持原样。使用 ` → ` 连接步骤。当上游仓库的任何步骤名称变更时，始终更新 Workflow 列。
7. **Agents、commands、skills 是不同的** — 从各自的目录统计，不要混淆
8. **统一四舍五入 stars** — `k` 后缀（98k、10k、4.1k）。低于 1000 显示精确数字
9. **与之前的 changelog 进行比较** — 标记条目为 NEW、RECURRING 或 RESOLVED
10. **Workflow 列是强制的且必须是扁平的** — 每行必须有 Workflow 单元格。追溯 README 的 "how to use" / 规范化的 happy path；不要编造虚构的流水线。**不含括号、不含英文修饰语、不含 `+` 连接符** — 如果某个步骤有内部子步骤，将它们作为兄弟步骤列在扁平链中并着色为黄色（`fff3b0`）；顶级步骤保持蓝色（`ddf4ff`）。
11. **Sub-loop 图例是强制的** — 紧接在表格之后，必须有 `> *Note: yellow tags are sub-loops — steps that repeat inside a parent step (e.g. per task, per story, or until a verify condition passes).*` 这一行。如果被移除则重新添加；绝不修改措辞。
