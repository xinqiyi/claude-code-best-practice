---
description: 通过并行研究所有 11 个工作流仓库来更新 DEVELOPMENT WORKFLOWS 表
---

# 工作流 — Development Workflows

通过并行研究 11 个仓库来更新 `README.md` 中的 DEVELOPMENT WORKFLOWS 表。启动 agents，合并结果，展示变更，如果获得批准则更新表格。

---

## 11 个仓库

| # | 仓库 | 维护者 |
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

- **Name**: `[Short Name](github-url)` — 使用项目名称，而非 owner/repo
- **★**: 星标数四舍五入到 `k`（例如 98k, 10k, 4.1k）。1000 以下显示精确数字
- **Workflow**: 标准的端到端流水线，表示为 shields.io 徽章组成的扁平从左到右序列，以 ` → ` 连接。每个步骤是仓库中实际的 command/skill/agent 名称（例如 `/speckit.plan`、`bmad-create-prd`、`subagent-driven-development`）。**仅扁平** — 无括号、无英文限定词（"loop"、"per story"、"parallel waves"）、无 `+` 连接符。如果一个步骤包含重要的内部子步骤，将它们列为主链中的同级项，并**用黄色（`fff3b0`）着色**以标记为子循环；顶层步骤保持浅蓝色（`ddf4ff`）。查阅 README 的 "how to use" / "workflow" 部分以获取标准 happy path：idea → spec/plan → tasks → implement → review → ship。
- **Agent/Command/Skill 计数**：仅数字（例如 `25`、`0`、`108+`）

### Workflow 徽章编码 (shields.io)

每个步骤渲染为一个 **HTML `<img>` 标签，带有 `align="middle"`**（非 markdown 图片语法），以使箭头与徽章垂直居中。两种背景色：

| 颜色 | 十六进制 | 何时使用 |
|---|---|---|
| 浅蓝 | `ddf4ff` | 顶层 workflow 步骤 |
| 柔黄 | `fff3b0` | 子循环步骤（在父步骤内按任务/故事重复，或直到验证通过） |

模板：

```html
<img src="https://img.shields.io/badge/<ENCODED>-ddf4ff" alt="<plain-label>" align="middle">    <!-- top-level -->
<img src="https://img.shields.io/badge/<ENCODED>-fff3b0" alt="<plain-label>" align="middle">    <!-- sub-loop -->
```

`align="middle"` 将徽章的垂直中心置于文本基线，使得 ` → ` 箭头在每个徽章上居中，而非位于徽章底部。没有它，箭头在 GitHub 渲染中会明显低于徽章。

表格关闭后，**始终包含以下说明**，作为单独一行的 blockquote：

```markdown
> *Note: yellow tags are sub-loops — steps that repeat inside a parent step (e.g. per task, per story, or until a verify condition passes).*
```

URL 中 `<ENCODED>` 部分的编码规则：

| 输入字符 | 编码为 |
|---|---|
| `/` (leading slash) | `%2F` |
| `-` (literal dash) | `--` |
| `_` (literal underscore) | `__` |
| ` ` (space) | `_` |
| `+` | `%2B` |
| `.` 和 `:` | 保持不变 |

示例：
- `/grill-me` → `%2Fgrill--me`
- `/speckit.plan` → `%2Fspeckit.plan`
- `/opsx:propose` → `%2Fopsx:propose`
- `bmad-create-epics-and-stories` → `bmad--create--epics--and--stories`

使用字面箭头 ` → `（空格-箭头-空格）连接步骤，**位于**一个 img 标签的关闭 `>` 和下一个的开启 `<` **之间**。

**不要**将子步骤包裹在括号中或用英文注释（"loop"、"per story"、"+"、"parallel waves"）。如果一个步骤有内部循环，只需将内部步骤名称作为扁平链中的同级项列出。

**排序顺序**：按星标数降序排列（最高在前）。

---

## 阶段 0：读取当前状态

读取以下文件：

1. `README.md` — `## ⚙️ DEVELOPMENT WORKFLOWS` 表（注意当前星标数、workflow 流水线、计数）
2. `changelog/development-workflows/changelog.md` — 之前的更新日志条目

---

## 阶段 1：启动 2 个 Research Agents

**立即**在**单条消息**中同时启动两个 agents（并行）。每个使用 `subagent_type: "development-workflows-research-agent"`。

### Agent 1（4 个仓库）

> 研究以下 4 个 Claude Code workflow 仓库：
>
> **仓库 1: github/spec-kit** (https://github.com/github/spec-kit)
> **仓库 2: affaan-m/everything-claude-code** (https://github.com/affaan-m/everything-claude-code)
> **仓库 3: obra/superpowers** (https://github.com/obra/superpowers)
> **仓库 4: mattpocock/skills** (https://github.com/mattpocock/skills)
>
> 对**每个**仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Agent 计数** — 统计 `agents/` 或 `.claude/agents/` 中的 `.md` 文件。对于 obra，还要统计由 skills 分发的隐式子 agents。对于 mattpocock，计数为 0（仅 skills 仓库）。
> 3. **Skill 计数** — 统计 `skills/` 或 `.claude/skills/` 中的文件夹。对于 mattpocock，统计仓库根目录 `skills/` 中的文件夹。
> 4. **Command 计数** — 统计 `commands/` 或 `.claude/commands/` 中的 `.md` 文件。对于 spec-kit，统计 `templates/commands/` 中的文件。对于 mattpocock，计数为 0（skills 充当斜杠命令）。
> 5. **Workflow** — 标准的端到端流水线，以 ` → ` 连接的扁平从左到右步骤名称序列。查阅 README 的 "how to use" / "workflow" 部分以获取 happy path：idea → spec/plan → tasks → implement → review → ship。使用仓库中实际的 command/skill/agent 名称。**仅扁平** — 无括号、无英文限定词（"loop"、"per story"、"parallel waves"）、无 `+` 连接符。如果一个步骤包含内部子步骤，将它们列为主链中的同级项。将每个步骤标记为 `top`（顶层）或 `sub`（子循环，在父步骤内重复），以便编排器着色。输出为纯文本 — 编排器会将每个步骤编码为 shields.io HTML img 徽章。
> 6. **值得注意的变更** — 是否有任何重大的近期变更？新的 agents/skills/commands、主要版本？
>
> 为每个仓库返回结构化报告：
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
> **仓库 1: Fission-AI/OpenSpec** (https://github.com/Fission-AI/OpenSpec)
> **仓库 2: humanlayer/humanlayer** (https://github.com/humanlayer/humanlayer)
> **仓库 3: gsd-build/get-shit-done** (https://github.com/gsd-build/get-shit-done)
> **仓库 4: garrytan/gstack** (https://github.com/garrytan/gstack)
> **仓库 5: bmad-code-org/BMAD-METHOD** (https://github.com/bmad-code-org/BMAD-METHOD)
> **仓库 6: EveryInc/compound-engineering-plugin** (https://github.com/EveryInc/compound-engineering-plugin)
> **仓库 7: Yeachan-Heo/oh-my-claudecode** (https://github.com/Yeachan-Heo/oh-my-claudecode)
>
> 对**每个**仓库，返回：
>
> 1. **Stars** — 使用 GitHub API `https://api.github.com/repos/{owner}/{repo}`，读取 `stargazers_count`。四舍五入到 `k`。
> 2. **Agent 计数** — 统计 `agents/` 或 `.claude/agents/` 中的 `.md` 文件。对于 BMAD，统计 `src/bmm-skills/` 中的 agent-persona skills。对于 compound-engineering-plugin，统计 `plugins/compound-engineering/agents/` 所有子目录中的 `.md` 文件。对于 oh-my-claudecode，统计仓库根目录 `agents/` 中的 `.md` 文件。
> 3. **Skill 计数** — 统计 `skills/` 或 `.claude/skills/` 中的文件夹。对于 gstack，skills 是包含 SKILL.md 的根级目录。对于 BMAD，统计 `src/bmm-skills/` 和 `src/core-skills/` 中的所有 skills。对于 compound-engineering-plugin，统计 `plugins/compound-engineering/skills/` 中的文件夹加上 `plugins/coding-tutor/skills/`。对于 oh-my-claudecode，统计仓库根目录 `skills/` 中的文件夹。
> 4. **Command 计数** — 统计 `commands/` 或 `.claude/commands/` 中的 `.md` 文件。对于 GSD，统计 `commands/gsd/`。对于 OpenSpec，统计 `/opsx:*` 命令。对于 BMAD，计数为 0（命令在安装时生成）。对于 compound-engineering-plugin，统计 `.claude/commands/` 中的 `.md` 文件加上 `plugins/coding-tutor/commands/`。对于 oh-my-claudecode，计数为 0（skills 充当斜杠命令）。
> 5. **Workflow** — 标准的端到端流水线，以 ` → ` 连接的扁平从左到右步骤名称序列。查阅 README 的 "how to use" / "workflow" 部分以获取 happy path：idea → spec/plan → tasks → implement → review → ship。使用仓库中实际的 command/skill/agent 名称。**仅扁平** — 无括号、无英文限定词（"loop"、"per story"、"parallel waves"）、无 `+` 连接符。如果一个步骤包含内部子步骤，将它们列为主链中的同级项。将每个步骤标记为 `top`（顶层）或 `sub`（子循环，在父步骤内重复），以便编排器着色。输出为纯文本 — 编排器会将每个步骤编码为 shields.io HTML img 徽章。
> 6. **值得注意的变更** — 是否有任何重大的近期变更？新的 agents/skills/commands、主要版本？
>
> 为每个仓库返回结构化报告：
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

## 阶段 2：比较与报告

**等待两个 agents。** 然后将结果与当前表格对比并展示：

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

与之前的更新日志条目对比，并将项目标记为 `NEW`、`RECURRING` 或 `RESOLVED`。

---

## 阶段 2.5：追加到更新日志

**强制** — 始终在展示给用户之前执行。

读取 `changelog/development-workflows/changelog.md`，然后**追加**一条新条目。如果文件不存在，先创建状态图例再添加第一条条目。

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Development Workflows Update

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action> | <status> |
```

通过 `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"` 获取时间。状态必须是以下之一：
- `COMPLETE (reason)` | `INVALID (reason)` | `ON HOLD (reason)`

始终追加，从不覆盖。

---

## 阶段 2.6：更新最后更新徽章

**强制** — 在阶段 2.5 之后执行。

更新 `README.md` 第 4 行的徽章。通过 `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"` 获取时间，进行 URL 编码，替换徽章中的日期。不要将此记录为操作项。

---

## 阶段 3：执行

询问用户：**(1) 全部执行** | **(2) 选择性执行** | **(3) 跳过**

执行时，编辑 `README.md` 中的 `## ⚙️ DEVELOPMENT WORKFLOWS` 表：
- 按行更新星标数、计数**以及 Workflow 列**
- 维护排序顺序：星标数降序（最高在前）
- 精确匹配现有格式（图标、徽章 URL、链接样式）
- 对于 Workflow 列，将 agent 返回的每个纯文本步骤按照表格格式部分的说明编码为 shields.io HTML img 徽章。标记为 `(top)` 的步骤使用 `ddf4ff`，标记为 `(sub)` 的步骤使用 `fff3b0`。使用 ` → ` 连接。
- 确保说明 `> *Note: yellow tags are sub-loops — steps that repeat inside a parent step (e.g. per task, per story, or until a verify condition passes).*` 紧跟在表格之后；如果缺失则添加。

---

## 规则

1. **并行启动两个 agents** — 单条消息，绝不顺序执行
2. **从不猜测** — 仅使用 agents 返回的数据
3. **不要自动执行** — 先展示报告，等待批准
4. **始终追加更新日志**并且**始终更新徽章** — 强制
5. **按星标数降序排序** — 最高星标在前
6. **Workflow 徽章使用带 align="middle" 的 HTML img** — `<img src="https://img.shields.io/badge/<ENCODED>-<COLOR>" alt="<plain-label>" align="middle">`。`align="middle"` 是必需的，以便 ` → ` 箭头与徽章垂直居中。两种颜色：顶层步骤用 `ddf4ff`，子循环步骤用 `fff3b0`。编码规则：空格用 `_`，连字符用 `--`，下划线用 `__`，`/` 用 `%2F`，`+` 用 `%2B`。点和冒号保持不变。用 ` → ` 连接步骤。当上游仓库中任何步骤名称发生变化时，始终更新 Workflow 列。
7. **Agents、commands、skills 是不同的** — 从各自目录分别计数，不要混淆
8. **统一四舍五入星标数** — `k` 后缀（98k, 10k, 4.1k）。1000 以下显示精确数字
9. **与之前的更新日志对比** — 将项目标记为 NEW、RECURRING 或 RESOLVED
10. **Workflow 列是强制且扁平的** — 每一行必须有 Workflow 单元格。查阅 README 的 "how to use" / 标准 happy path；不要编造虚构的流水线。**无括号、无英文限定词、无 `+` 连接符** — 如果一个步骤包含内部子步骤，将它们列在扁平链中作为同级项，并用黄色（`fff3b0`）着色；顶层步骤保持蓝色（`ddf4ff`）。
11. **子循环说明是强制性的** — 紧跟在表格之后，必须有 `> *Note: yellow tags are sub-loops — steps that repeat inside a parent step (e.g. per task, per story, or until a verify condition passes).*` 这一行。如果被移除则添加回来；绝不修改措辞。
