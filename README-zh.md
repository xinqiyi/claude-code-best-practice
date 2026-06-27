# claude-code-best-practice
从 vibe coding 到 agentic engineering - 熟能生巧，让 claude 更完美

![updated with Claude Code](https://img.shields.io/badge/updated_with_Claude_Code-Jun%2026%2C%202026%209%3A40%20AM%20PKT-white?style=flat&labelColor=555) <a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers"><img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=flat&label=%E2%98%85&labelColor=555&color=white" alt="GitHub Stars"></a><br>

[![Best Practice](!/tags/best-practice.svg)](best-practice/) [![Implemented](!/tags/implemented.svg)](implementation/) [![Orchestration Workflow](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) [![Claude](!/tags/claude.svg)](https://code.claude.com/docs) [![Boris](!/tags/boris-cherny.svg)](#-tips-and-tricks) [![Community](!/tags/community.svg)](#-subscribe) ![Click on these badges below to see the actual sources](!/tags/click-badges.svg)<br>
<img src="!/tags/a.svg" height="14"> = Agents · <img src="!/tags/c.svg" height="14"> = Commands · <img src="!/tags/s.svg" height="14"> = Skills

<p align="center">
  <img src="!/claude-jumping.svg" alt="Claude Code mascot jumping" width="120" height="100"><br>
  <a href="https://github.com/trending"><img src="!/root/github-trending-day.svg" alt="GitHub Trending #1 Repository Of The Day"></a>
</p>

<p align="center">
  <img src="!/root/supported-label.svg" alt="Supported by:" height="34">&nbsp;&nbsp;<a href="https://disrupt.com"><img src="!/root/supported-disrupt.svg" alt="Disrupt.com — Ventures Reimagined" height="34"></a>&nbsp;&nbsp;<a href="https://claudekit.cc/?utm_source=github&utm_medium=sponsorship&utm_campaign=shayan_claude_code_best_practice"><img src="!/root/supported-claudekit.svg" alt="ClaudeKit — Production-ready skills and workflows" height="34"></a>
</p>

<p align="center">
  <img src="!/root/boris-slider.gif" alt="Boris Cherny on Claude Code" width="600"><br>
  Boris Cherny 在 X 上的分享（<a href="https://x.com/bcherny/status/2007179832300581177">tweet 1</a> · <a href="https://x.com/bcherny/status/2017742741636321619">tweet 2</a> · <a href="https://x.com/bcherny/status/2021699851499798911">tweet 3</a>）
</p>

> [!TIP]
> 访问 [**How to Use**](#how-to-use) 章节以充分利用本仓库。

## 🧠 CONCEPTS

| Feature | Location | Description |
|---------|----------|-------------|
| <img src="!/tags/a.svg" height="14"> [**Subagents**](https://code.claude.com/docs/en/sub-agents) | `.claude/agents/<name>.md` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-subagents.md) [![Implemented](!/tags/implemented.svg)](implementation/claude-subagents-implementation.md) |
| <img src="!/tags/c.svg" height="14"> [**Commands**](https://code.claude.com/docs/en/slash-commands) | `.claude/commands/<name>.md` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-commands.md) [![Implemented](!/tags/implemented.svg)](implementation/claude-commands-implementation.md) |
| <img src="!/tags/s.svg" height="14"> [**Skills**](https://code.claude.com/docs/en/skills) | `.claude/skills/<name>/SKILL.md` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-skills.md) [![Implemented](!/tags/implemented.svg)](implementation/claude-skills-implementation.md) [Official Skills](https://github.com/anthropics/skills/tree/main/skills) · [Skills for Mono-repos](reports/claude-skills-for-larger-mono-repos.md) |
| [**Workflows**](https://code.claude.com/docs/en/common-workflows) | [`.claude/commands/weather-orchestrator.md`](.claude/commands/weather-orchestrator.md) | [![Orchestration Workflow](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) |
| [**Hooks**](https://code.claude.com/docs/en/hooks) | `.claude/hooks/` | [![Best Practice](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-hooks) [![Implemented](!/tags/implemented.svg)](https://github.com/shanraisshan/claude-code-hooks) [Guide](https://code.claude.com/docs/en/hooks-guide) |
| [**MCP Servers**](https://code.claude.com/docs/en/mcp) | `.claude/settings.json`, `.mcp.json` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-mcp.md) [![Implemented](!/tags/implemented.svg)](.mcp.json) |
| [**Plugins**](https://code.claude.com/docs/en/plugins) | distributable packages | [Marketplaces](https://code.claude.com/docs/en/discover-plugins) · [Create Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces) |
| [**Settings**](https://code.claude.com/docs/en/settings) | `.claude/settings.json` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-settings.md) [![Implemented](!/tags/implemented.svg)](.claude/settings.json) [Permissions](https://code.claude.com/docs/en/permissions) · [Model Config](https://code.claude.com/docs/en/model-config) · [Output Styles](https://code.claude.com/docs/en/output-styles) · [Sandboxing](https://code.claude.com/docs/en/sandboxing) · [Keybindings](https://code.claude.com/docs/en/keybindings) · [Auto Mode Config](https://code.claude.com/docs/en/auto-mode-config) |
| [**Status Line**](https://code.claude.com/docs/en/statusline) | `.claude/settings.json` | [![Best Practice](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-status-line) [![Implemented](!/tags/implemented.svg)](.claude/settings.json) |
| [**Memory**](https://code.claude.com/docs/en/memory) | `CLAUDE.md`, `.claude/rules/`, `~/.claude/rules/`, `~/.claude/projects/<project>/memory/` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-memory.md) [![Implemented](!/tags/implemented.svg)](CLAUDE.md) [Auto Memory](https://code.claude.com/docs/en/memory) · [Auto Memory Deep-dive](reports/claude-agent-memory.md) · [Rules](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| [**Checkpointing**](https://code.claude.com/docs/en/checkpointing) | automatic (file-edit tracking) |  |
| [**CLI Startup Flags**](https://code.claude.com/docs/en/cli-reference) | `claude [flags]` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-cli-startup-flags.md) [Interactive Mode](https://code.claude.com/docs/en/interactive-mode) · [Env Vars](https://code.claude.com/docs/en/env-vars) |
| **AI Terms** | | [![Best Practice](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-codex-cursor-gemini/blob/main/reports/ai-terms.md) |
| [**Best Practices**](https://code.claude.com/docs/en/best-practices) | | [Prompt Engineering](https://github.com/anthropics/prompt-eng-interactive-tutorial) · [Extend Claude Code](https://code.claude.com/docs/en/features-overview) |

### 🔥 Hot

| Feature | Location | Description |
|---------|----------|-------------|
| [**Ultrareview**](https://code.claude.com/docs/en/ultrareview) ![beta](!/tags/beta.svg) | `/code-review ultra`, `claude ultrareview [target]` | [Tasks tracking](https://code.claude.com/docs/en/ultrareview#track-a-running-review) |
| [**Devcontainers**](https://code.claude.com/docs/en/devcontainer) | `.devcontainer/` |  |
| [**Channels**](https://code.claude.com/docs/en/channels) ![beta](!/tags/beta.svg) | `--channels`, plugin-based | [Reference](https://code.claude.com/docs/en/channels-reference) |
| [**Ultraplan**](https://code.claude.com/docs/en/ultraplan) ![beta](!/tags/beta.svg) | `/ultraplan` |  |
| [**No Flicker Mode**](https://code.claude.com/docs/en/fullscreen) ![beta](!/tags/beta.svg) | `/tui fullscreen`, `CLAUDE_CODE_NO_FLICKER=1` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2039421575422980329) |
| [**Auto Mode**](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) ![beta](!/tags/beta.svg) | `--permission-mode auto`, `Shift+Tab` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/claudeai/status/2036503582166393240) [Blog](https://claude.com/blog/auto-mode) |
| [**Power-ups**](best-practice/claude-power-ups.md) | `/powerup` | [![Best Practice](!/tags/best-practice.svg)](best-practice/claude-power-ups.md) |
| [**Fast Mode**](https://code.claude.com/docs/en/fast-mode) ![beta](!/tags/beta.svg) | `/fast`, `"fastMode": true` |  |
| [**Advisor**](https://code.claude.com/docs/en/advisor) ![beta](!/tags/beta.svg) | `/advisor`, `advisorModel`, `--advisor` | [Blog](https://claude.com/blog/the-advisor-strategy) |
| [**Computer Use**](https://code.claude.com/docs/en/computer-use) ![beta](!/tags/beta.svg) | `computer-use` MCP server | [Desktop](https://code.claude.com/docs/en/desktop#let-claude-use-your-computer) |
| [**Agent SDK**](https://code.claude.com/docs/en/agent-sdk/overview) | `npm` / `pip` package | [Quickstart](https://code.claude.com/docs/en/agent-sdk/quickstart) · [Examples](https://github.com/anthropics/claude-agent-sdk-demos) |
| [**Ralph Wiggum Loop**](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) | plugin | [![Best Practice](!/tags/best-practice.svg)](https://github.com/ghuntley/how-to-ralph-wiggum) [![Implemented](!/tags/implemented.svg)](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop) |
| [**Chrome**](https://code.claude.com/docs/en/chrome) ![beta](!/tags/beta.svg) | `--chrome`, extension | [![Best Practice](!/tags/best-practice.svg)](reports/claude-in-chrome-v-chrome-devtools-mcp.md) |
| [**Claude Code Web**](https://code.claude.com/docs/en/claude-code-on-the-web) ![beta](!/tags/beta.svg) | `claude.ai/code` | [Routines](https://code.claude.com/docs/en/routines) |
| [**Artifacts**](https://code.claude.com/docs/en/artifacts) ![beta](!/tags/beta.svg) | `/share`, `Artifact` tool |  |
| [**Slack**](https://code.claude.com/docs/en/slack) | `@Claude` in Slack |  |
| [**Code Review**](https://code.claude.com/docs/en/code-review) ![beta](!/tags/beta.svg) | GitHub App (managed) | [![Best Practice](!/tags/best-practice.svg)](https://x.com/claudeai/status/2031088171262554195) [Blog](https://claude.com/blog/code-review) [Local /code-review](https://code.claude.com/docs/en/commands) |
| [**GitHub Actions**](https://code.claude.com/docs/en/github-actions) | `.github/workflows/` | [GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd) |
| [**Remote Control**](https://code.claude.com/docs/en/remote-control) | `/remote-control`, `/rc` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/noahzweben/status/2032533699116355819) [Headless Mode](https://code.claude.com/docs/en/headless) |
| [**Deep Links**](https://code.claude.com/docs/en/deep-links) | `claude-cli://open?repo=…&q=…` |  |
| [**Dynamic Workflows**](https://code.claude.com/docs/en/workflows) | `/workflows`, `ultracode` keyword, `/effort ultracode`, `.claude/workflows/` | [Deep Research](https://code.claude.com/docs/en/workflows#run-a-bundled-workflow) |
| [**Agent Teams**](https://code.claude.com/docs/en/agent-teams) ![beta](!/tags/beta.svg) | built-in (env var) | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2019472394696683904) [![Implemented](!/tags/implemented.svg)](implementation/claude-agent-teams-implementation.md) |
| [**Agent View**](https://code.claude.com/docs/en/agent-view) ![beta](!/tags/beta.svg) | `claude agents`, `--bg`, `/bg` |  |
| [**Scheduled Tasks**](https://code.claude.com/docs/en/scheduled-tasks) | `/loop`, `/schedule`, cron tools | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2030193932404150413) [![Implemented](!/tags/implemented.svg)](implementation/claude-scheduled-tasks-implementation.md) [Desktop scheduled tasks](https://code.claude.com/docs/en/desktop-scheduled-tasks) · [Announcement](https://x.com/noahzweben/status/2036129220959805859) |
| [**Routines**](https://code.claude.com/docs/en/routines) ![beta](!/tags/beta.svg) | `claude.ai/code/routines`, `/schedule` | [Desktop Tasks](https://code.claude.com/docs/en/desktop-scheduled-tasks) |
| [**Tasks**](reports/claude-global-vs-project-settings.md#tasks-system) | `/tasks`, `~/.claude/tasks/` | [![Best Practice](!/tags/best-practice.svg)](reports/claude-global-vs-project-settings.md) [Ultrareview tracking](https://code.claude.com/docs/en/ultrareview#track-a-running-review) |
| [**Goal**](https://code.claude.com/docs/en/goal) | `/goal <condition>`, `/goal clear` | [![Implemented](!/tags/implemented.svg)](implementation/claude-goal-implementation.md) |
| [**Voice Dictation**](https://code.claude.com/docs/en/voice-dictation) ![beta](!/tags/beta.svg) | `/voice` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/trq212/status/2028628570692890800) |
| [**Bundled Skills**](https://code.claude.com/docs/en/skills#bundled-skills) | `/code-review`, `/batch` | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2027534984534544489) |
| [**Git Worktrees**](https://code.claude.com/docs/en/worktrees) | `--worktree`/`-w`, `.worktreeinclude`, `EnterWorktree`/`ExitWorktree`, `isolation: "worktree"`, `WorktreeCreate`/`WorktreeRemove` hooks | [![Best Practice](!/tags/best-practice.svg)](https://x.com/bcherny/status/2025007393290272904) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="orchestration-workflow"></a>

## <a href="orchestration-workflow/orchestration-workflow.md"><img src="!/tags/orchestration-workflow-hd.svg" alt="Orchestration Workflow"></a>

参见 [orchestration-workflow](orchestration-workflow/orchestration-workflow.md) 了解 <img src="!/tags/c.svg" height="14"> **Command** → <img src="!/tags/a.svg" height="14"> **Agent** → <img src="!/tags/s.svg" height="14"> **Skill** 模式的实现细节。


<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent Architecture Flow" width="100%">
</p>

<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.gif" alt="Orchestration Workflow Demo" width="600">
</p>

![How to Use](!/tags/how-to-use.svg)

```bash
claude
/weather-orchestrator
```

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## ⚙️ DEVELOPMENT WORKFLOWS

所有主要 workflows 汇聚于相同的架构模式: **Research → Plan → Execute → Review → Ship**

| Name | ★ | Workflow | <img src="!/tags/a.svg" height="14"> | <img src="!/tags/c.svg" height="14"> | <img src="!/tags/s.svg" height="14"> |
|------|---|----------|---|---|---|
| [Superpowers](https://github.com/obra/superpowers) | 239k | <img src="https://img.shields.io/badge/brainstorming-ddf4ff" alt="brainstorming" align="middle"> → <img src="https://img.shields.io/badge/using--git--worktrees-ddf4ff" alt="using-git-worktrees" align="middle"> → <img src="https://img.shields.io/badge/writing--plans-ddf4ff" alt="writing-plans" align="middle"> → <img src="https://img.shields.io/badge/subagent--driven--development-ddf4ff" alt="subagent-driven-development" align="middle"> → <img src="https://img.shields.io/badge/implementer-fff3b0" alt="implementer" align="middle"> → <img src="https://img.shields.io/badge/task--reviewer-fff3b0" alt="task-reviewer" align="middle"> → <img src="https://img.shields.io/badge/fix--subagent-fff3b0" alt="fix-subagent" align="middle"> → <img src="https://img.shields.io/badge/final--code--reviewer-fff3b0" alt="final-code-reviewer" align="middle"> → <img src="https://img.shields.io/badge/test--driven--development-fff3b0" alt="test-driven-development" align="middle"> → <img src="https://img.shields.io/badge/requesting--code--review-ddf4ff" alt="requesting-code-review" align="middle"> → <img src="https://img.shields.io/badge/verification--before--completion-ddf4ff" alt="verification-before-completion" align="middle"> → <img src="https://img.shields.io/badge/finishing--a--development--branch-ddf4ff" alt="finishing-a-development-branch" align="middle"> | 0 | 0 | 14 |
| [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) | 222k | <img src="https://img.shields.io/badge/%2Fecc:plan-ddf4ff" alt="/ecc:plan" align="middle"> → <img src="https://img.shields.io/badge/%2Ftdd-ddf4ff" alt="/tdd" align="middle"> → <img src="https://img.shields.io/badge/%2Fcode--review-ddf4ff" alt="/code-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fsecurity--scan-ddf4ff" alt="/security-scan" align="middle"> → <img src="https://img.shields.io/badge/%2Fe2e-ddf4ff" alt="/e2e" align="middle"> → <img src="https://img.shields.io/badge/merge-ddf4ff" alt="merge" align="middle"> | 67 | 84 | 271 |
| [Matt Pocock Skills](https://github.com/mattpocock/skills) | 147k | <img src="https://img.shields.io/badge/%2Fask--matt-ddf4ff" alt="/ask-matt" align="middle"> → <img src="https://img.shields.io/badge/%2Fgrill--with--docs-ddf4ff" alt="/grill-with-docs" align="middle"> → <img src="https://img.shields.io/badge/%2Fto--prd-ddf4ff" alt="/to-prd" align="middle"> → <img src="https://img.shields.io/badge/%2Fto--issues-ddf4ff" alt="/to-issues" align="middle"> → <img src="https://img.shields.io/badge/%2Fprototype-ddf4ff" alt="/prototype" align="middle"> → <img src="https://img.shields.io/badge/%2Ftriage-ddf4ff" alt="/triage" align="middle"> → <img src="https://img.shields.io/badge/%2Ftdd-fff3b0" alt="/tdd" align="middle"> → <img src="https://img.shields.io/badge/%2Fdiagnosing--bugs-fff3b0" alt="/diagnosing-bugs" align="middle"> → <img src="https://img.shields.io/badge/%2Fimprove--codebase--architecture-ddf4ff" alt="/improve-codebase-architecture" align="middle"> → <img src="https://img.shields.io/badge/%2Fhandoff-ddf4ff" alt="/handoff" align="middle"> | 0 | 0 | 35 |
| [gstack](https://github.com/garrytan/gstack) | 116k | <img src="https://img.shields.io/badge/%2Foffice--hours-ddf4ff" alt="/office-hours" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--ceo--review-ddf4ff" alt="/plan-ceo-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--eng--review-fff3b0" alt="/plan-eng-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--design--review-fff3b0" alt="/plan-design-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--devex--review-fff3b0" alt="/plan-devex-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fspec-ddf4ff" alt="/spec" align="middle"> → <img src="https://img.shields.io/badge/%2Fdesign--consultation-fff3b0" alt="/design-consultation" align="middle"> → <img src="https://img.shields.io/badge/%2Freview-ddf4ff" alt="/review" align="middle"> → <img src="https://img.shields.io/badge/%2Fqa-ddf4ff" alt="/qa" align="middle"> → <img src="https://img.shields.io/badge/%2Fship-ddf4ff" alt="/ship" align="middle"> → <img src="https://img.shields.io/badge/%2Fland--and--deploy-ddf4ff" alt="/land-and-deploy" align="middle"> → <img src="https://img.shields.io/badge/%2Fcanary-ddf4ff" alt="/canary" align="middle"> → <img src="https://img.shields.io/badge/%2Fretro-ddf4ff" alt="/retro" align="middle"> | 0 | 0 | 55 |
| [Spec Kit](https://github.com/github/spec-kit) | 116k | <img src="https://img.shields.io/badge/%2Fspeckit.constitution-ddf4ff" alt="/speckit.constitution" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.specify-ddf4ff" alt="/speckit.specify" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.clarify-ddf4ff" alt="/speckit.clarify" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.plan-ddf4ff" alt="/speckit.plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.tasks-ddf4ff" alt="/speckit.tasks" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.taskstoissues-ddf4ff" alt="/speckit.taskstoissues" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.implement-ddf4ff" alt="/speckit.implement" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.analyze-ddf4ff" alt="/speckit.analyze" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.checklist-ddf4ff" alt="/speckit.checklist" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.converge-ddf4ff" alt="/speckit.converge" align="middle"> | 0 | 10 | 0 |
| [Get Shit Done](https://github.com/gsd-build/get-shit-done) | 65k | <img src="https://img.shields.io/badge/%2Fgsd--new--project-ddf4ff" alt="/gsd-new-project" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--spec--phase-ddf4ff" alt="/gsd-spec-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--plan--phase-ddf4ff" alt="/gsd-plan-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--execute--phase-ddf4ff" alt="/gsd-execute-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--code--review-fff3b0" alt="/gsd-code-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--validate--phase-fff3b0" alt="/gsd-validate-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--ship-ddf4ff" alt="/gsd-ship" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--extract--learnings-ddf4ff" alt="/gsd-extract-learnings" align="middle"> | 33 | 67 | 0 |
| [agent-skills](https://github.com/addyosmani/agent-skills) | 61k | <img src="https://img.shields.io/badge/%2Fspec-ddf4ff" alt="/spec" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan-ddf4ff" alt="/plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fbuild-ddf4ff" alt="/build" align="middle"> → <img src="https://img.shields.io/badge/%2Ftest-ddf4ff" alt="/test" align="middle"> → <img src="https://img.shields.io/badge/%2Freview-ddf4ff" alt="/review" align="middle"> → <img src="https://img.shields.io/badge/%2Fship-ddf4ff" alt="/ship" align="middle"> | 3 | 7 | 21 |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | 57k | <img src="https://img.shields.io/badge/%2Fopsx:propose-ddf4ff" alt="/opsx:propose" align="middle"> → <img src="https://img.shields.io/badge/%2Fopsx:apply-fff3b0" alt="/opsx:apply" align="middle"> → <img src="https://img.shields.io/badge/%2Fopsx:verify-fff3b0" alt="/opsx:verify" align="middle"> → <img src="https://img.shields.io/badge/%2Fopsx:archive-ddf4ff" alt="/opsx:archive" align="middle"> → <img src="https://img.shields.io/badge/%2Fopsx:bulk--archive-ddf4ff" alt="/opsx:bulk-archive" align="middle"> | 0 | 11 | 0 |
| [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | 50k | <img src="https://img.shields.io/badge/bmad--product--brief-ddf4ff" alt="bmad-product-brief" align="middle"> → <img src="https://img.shields.io/badge/bmad--prfaq-fff3b0" alt="bmad-prfaq" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--prd-ddf4ff" alt="bmad-create-prd" align="middle"> → <img src="https://img.shields.io/badge/bmad--validate--prd-fff3b0" alt="bmad-validate-prd" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--architecture-ddf4ff" alt="bmad-create-architecture" align="middle"> → <img src="https://img.shields.io/badge/bmad--check--implementation--readiness-ddf4ff" alt="bmad-check-implementation-readiness" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--epics--and--stories-ddf4ff" alt="bmad-create-epics-and-stories" align="middle"> → <img src="https://img.shields.io/badge/bmad--dev--story-fff3b0" alt="bmad-dev-story" align="middle"> → <img src="https://img.shields.io/badge/bmad--code--review-fff3b0" alt="bmad-code-review" align="middle"> → <img src="https://img.shields.io/badge/bmad--qa--generate--e2e--tests-ddf4ff" alt="bmad-qa-generate-e2e-tests" align="middle"> → <img src="https://img.shields.io/badge/bmad--retrospective-ddf4ff" alt="bmad-retrospective" align="middle"> | 6 | 0 | 42 |
| [oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) | 37k | <img src="https://img.shields.io/badge/team--plan-ddf4ff" alt="team-plan" align="middle"> → <img src="https://img.shields.io/badge/team--prd-ddf4ff" alt="team-prd" align="middle"> → <img src="https://img.shields.io/badge/team--exec-ddf4ff" alt="team-exec" align="middle"> → <img src="https://img.shields.io/badge/team--verify-ddf4ff" alt="team-verify" align="middle"> → <img src="https://img.shields.io/badge/team--fix-fff3b0" alt="team-fix" align="middle"> → <img src="https://img.shields.io/badge/team--verify-fff3b0" alt="team-verify" align="middle"> | 19 | 0 | 40 |
| [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) | 22k | <img src="https://img.shields.io/badge/%2Fce--strategy-ddf4ff" alt="/ce-strategy" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--ideate-ddf4ff" alt="/ce-ideate" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--brainstorm-ddf4ff" alt="/ce-brainstorm" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--plan-ddf4ff" alt="/ce-plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--work-ddf4ff" alt="/ce-work" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--code--review-ddf4ff" alt="/ce-code-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--compound-ddf4ff" alt="/ce-compound" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--product--pulse-ddf4ff" alt="/ce-product-pulse" align="middle"> | 0 | 1 | 27 |
| [HumanLayer](https://github.com/humanlayer/humanlayer) | 11k | <img src="https://img.shields.io/badge/%2Fresearch__codebase-ddf4ff" alt="/research_codebase" align="middle"> → <img src="https://img.shields.io/badge/%2Fcreate__plan-ddf4ff" alt="/create_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fvalidate__plan-fff3b0" alt="/validate_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fiterate__plan-fff3b0" alt="/iterate_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fimplement__plan-ddf4ff" alt="/implement_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Flocal__review-ddf4ff" alt="/local_review" align="middle"> → <img src="https://img.shields.io/badge/%2Fcreate__handoff-ddf4ff" alt="/create_handoff" align="middle"> → <img src="https://img.shields.io/badge/%2Fresume__handoff-ddf4ff" alt="/resume_handoff" align="middle"> → <img src="https://img.shields.io/badge/%2Fcommit-ddf4ff" alt="/commit" align="middle"> → <img src="https://img.shields.io/badge/%2Fdescribe__pr-ddf4ff" alt="/describe_pr" align="middle"> | 6 | 27 | 0 |

> *Note: 黄色标签表示 sub-loops — 在父步骤内重复执行的步骤（例如按 task、按 story 或直到 verify 条件通过）。*

### Others
- [RPI](development-workflows/rpi/rpi-workflow.md) [![Implemented](!/tags/implemented.svg)](development-workflows/rpi/rpi-workflow.md)
- [Ralph Wiggum Loop](https://www.youtube.com/watch?v=eAtvoGlpeRU) [![Implemented](!/tags/implemented.svg)](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop)
- [Andrej Karpathy (Founding Member, OpenAI) Workflow](https://x.com/karpathy/status/2015883857489522876)
- [Peter Steinberger (Creator of OpenClaw) Workflow](https://youtu.be/8lF7HmQ_RgY?t=2582)
- Boris Cherny (Creator of Claude Code) Workflow — [13 Tips](tips/claude-boris-13-tips-03-jan-26.md) · [10 Tips](tips/claude-boris-10-tips-01-feb-26.md) · [12 Tips](tips/claude-boris-12-tips-12-feb-26.md) · [2 Tips](tips/claude-boris-2-tips-25-mar-26.md) · [15 Tips](tips/claude-boris-15-tips-30-mar-26.md) · [6 Tips](tips/claude-boris-6-tips-16-apr-26.md) [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny)
- Thariq (Anthropic) Workflow — [Skills](tips/claude-thariq-tips-17-mar-26.md) · [Session Management](tips/claude-thariq-tips-16-apr-26.md) [![Thariq](!/tags/thariq.svg)](https://x.com/trq212)

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 🔀 CROSS-MODEL WORKFLOWS

将 Claude Code 与其他模型 — Codex, Gemini, GPT, Kimi, DeepSeek, local — 一起使用，通过三种机制：

- **Plugin** — 另一个模型的 CLI 在 Claude Code 内运行（slash commands 如 `/codex:review`）
- **MCP** — Claude Code 通过 Model Context Protocol 将另一个模型作为 tool 调用
- **Router** — Claude Code 的 API endpoint 被切换到不同的 provider

方法论: [Cross-Model (Claude Code + Codex) Workflow](development-workflows/cross-model-workflow/cross-model-workflow.md) [![Implemented](!/tags/implemented.svg)](development-workflows/cross-model-workflow/cross-model-workflow.md) — 手动双终端流程，在 Claude 中 Plan，在 Codex 中 QA-Review。

| Name | ★ | Type | Bridges to | What it does |
|------|---|------|------------|--------------|
| [musistudio/claude-code-router](https://github.com/musistudio/claude-code-router) | 34k | Router | OpenRouter, DeepSeek, Ollama, Gemini, Kimi, Qwen, Groq, +more | 将 Claude Code 的 API 路由到任何兼容的 provider，支持按任务选择 model |
| [router-for-me/CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI) | 32k | Router | Gemini CLI, Codex, Claude Code, Antigravity | 将每个 CLI 包装为 OpenAI/Gemini/Claude/Codex 兼容的 API 服务 |
| [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc) | 18k | Plugin | Codex / GPT-5 | 官方 OpenAI plugin: 在 Claude Code 内使用 `/codex:review`, `/codex:adversarial-review`, `/codex:rescue` |
| [BeehiveInnovations/pal-mcp-server](https://github.com/BeehiveInnovations/pal-mcp-server) | 12k | MCP | Gemini, OpenAI, Azure, Grok, Ollama, OpenRouter (50+ models) | 多模型 MCP server（原名 `zen-mcp-server`）— 将其他模型作为 Claude tools 调用 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 🧰 SKILL COLLECTIONS

主要以策展的 `SKILL.md` 文件库而闻名的仓库（与上述完整的 workflow 方法论不同）。按 stars 降序排列。

| Name | ★ | <img src="!/tags/s.svg" height="14"> |
|------|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) | 154k | 17 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | 144k | 30 |
| [Egonex-AI/Understand-Anything](https://github.com/Egonex-AI/Understand-Anything) | 67k | 8 |
| [wshobson/agents](https://github.com/wshobson/agents) | 37k | 156 |
| [scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills) | 29k | 147 |
| [impeccable](https://github.com/pbakaus/impeccable) | 27k | 1 (with 7 design domain references) |
| [agent-skills](https://github.com/addyosmani/agent-skills) | 27k | 21 |
| [awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 26k | 1,497+ (curated list) |
| [claude-skills](https://github.com/alirezarezvani/claude-skills) | 15k | 246 (across 9 domains) |
| [shanraisshan/draw-json-architecture-skill](https://github.com/shanraisshan/draw-json-architecture-skill) | 3 | 1 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 🤖 AGENT COLLECTIONS

主要以策展的 subagent 定义库（`.claude/agents/*.md`）而闻名的仓库。按 stars 降序排列。

| Name | ★ | <img src="!/tags/a.svg" height="14"> |
|------|---|---|
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | 115k | 271 |
| [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) | 22k | 156 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 💡 TIPS AND TRICKS (83)

🚫👶 = do not babysit

[Prompting](#tips-prompting) · [Planning](#tips-planning) · [Context](#tips-context) · [Session](#tips-session) · [CLAUDE.md + .claude/rules](#tips-claudemd) · [Agents](#tips-agents) · [Commands](#tips-commands) · [Skills](#tips-skills) · [Hooks](#tips-hooks) · [Workflows](#tips-workflows) · [Advanced](#tips-workflows-advanced) · [Git / PR](#tips-git-pr) · [Debugging](#tips-debugging) · [Utilities](#tips-utilities) · [Daily](#tips-daily)

![Community](!/tags/community.svg)

<a id="tips-prompting"></a>■ **Prompting (3)**

| Tip | Source |
|-----|--------|
| challenge Claude — "grill me on these changes and don't make a PR until I pass your test." 或 "prove to me this works"，然后让 Claude 在 main 和你的 branch 之间 diff 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 在 mediocre fix 之后 — "knowing everything you know now, scrap this and implement the elegant solution" 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| Claude 能自行修复大多数 bugs — 粘贴 bug，说 "fix"，不要 micromanage 如何做 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742750473720121) |

<a id="tips-planning"></a>■ **Planning/Specs (7)**

| Tip | Source |
|-----|--------|
| 始终从 [plan mode](https://code.claude.com/docs/en/common-workflows) 开始 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179845336527000) |
| 从 minimal spec 或 prompt 开始，让 Claude 使用 [AskUserQuestion](https://code.claude.com/docs/en/cli-reference) tool 来 interview 你，然后用新 session 执行 spec | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2005315275026260309) |
| 始终制定 phase-wise gated plan，每个 phase 有多个 tests（unit, automation, integration） | [![Dex](!/tags/community-dex.svg)](videos/claude-dex-mlops-community-24-mar-26.md) [![Video](!/tags/video.svg)](https://youtu.be/YwZR6tc7qYg?t=1032) |
| 将 PRDs 分解为跨越所有 layers（DB + service + UI）的 vertical slices（tracer bullets）— AI 默认采用 horizontal phasing（DB phase, 然后 API phase, 然后 frontend phase），会将 end-to-end feedback 推迟到最后一个 phase。来自 The Pragmatic Programmer 的建议 🚫👶 | [![Matt](!/tags/community-matt.svg)](videos/claude-matt-pocock-24-apr-26.md) [![Video](!/tags/video.svg)](https://youtu.be/-QFHIoCo-Ko) |
| 启动第二个 Claude 以 staff engineer 的身份 review 你的 plan，或使用 [cross-model](development-workflows/cross-model-workflow/cross-model-workflow.md) 进行 review | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742745365057733) |
| 在交付工作前编写详细的 specs 并减少歧义 — 你越具体，输出越好 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| prototype > PRD — 构建 20-30 个版本而不是写 specs，构建成本很低，所以多尝试 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3630) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3630) |

<a id="tips-context"></a>■ **Context (5)**

| Tip | Source |
|-----|--------|
| 在 1M context model 上，context rot 约在 ~300-400k tokens 时开始出现 — 对于 intelligence-sensitive 的工作，不要让 sessions 超过这个量 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| dumb zone 约在 ~40% context 时开始 — "你到了这个点，结果开始退化"。新手: "尽量保持在 40% 以下，如果到了 60%，考虑收尾"。有经验者: "aggressively 保持在 30% 以下" — 只在简单任务上推到 60%。切换任务时手动 [/compact](https://code.claude.com/docs/en/interactive-mode) 或 [/clear](https://code.claude.com/docs/en/cli-reference) 来重置 | [![Dex](!/tags/community-dex.svg)](videos/claude-dex-mlops-community-24-mar-26.md) [![Video](!/tags/video.svg)](https://youtu.be/YwZR6tc7qYg?t=1541) |
| rewind > correct — 双击 Esc 或 [/rewind](https://code.claude.com/docs/en/checkpointing) 回到失败尝试之前，用你学到的东西重新 prompt，而不是让失败的尝试 + 更正污染 context 🚫👶 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 带提示的 [/compact](https://code.claude.com/docs/en/interactive-mode)（/compact focus on the auth refactor, drop the test debugging）优于让 autocompact 触发 — 由于 context rot，model 在 auto-compacting 时处于智能最低点 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 使用 subagents 进行 context management — 问自己 "我会再次需要这个 tool output 吗，还是只需要结论？" — 20 次 file reads + 12 次 greps + 3 次 dead ends 留在 child 的 context 中，只有最终 report 返回 🚫👶 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |

<a id="tips-session"></a>■ **Session Management (6)**

| Tip | Source |
|-----|--------|
| 每个 turn 都是一个 branching point — Claude 结束一个 turn 后，根据你需要向前 carry 多少现有 context，在 Continue, /rewind, /clear, /compact 或 Subagent 中选择 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 新 task = 新 session — 相关 tasks（例如为你刚构建的内容写 docs）可以复用 context 以提高效率，但真正新的 tasks 应使用 fresh session | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 在 rewind 前使用 "summarize from here" 让 Claude 写一条 handoff message — 就像从未来自己写给过去 Claude iteration 的便条 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| /compact vs /clear — compact 是有损的但利于 momentum（mid-task, fuzzy details 可接受）；/clear + brief 工作量更大但你能精确控制 carry forward 什么（高风险下一步） | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 对 long-running sessions 使用 recaps — Claude 做了什么以及下一步是什么的简短总结，在几分钟或几小时后回来时很有用。在 /config 中用 recaps 禁用 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| [/rename](https://code.claude.com/docs/en/cli-reference) 重要 sessions（例如 [TODO - refactor task]）并稍后 [/resume](https://code.claude.com/docs/en/cli-reference) 它们 — 同时运行多个 Claudes 时给每个 instance 打标签 | [![Cat](!/tags/cat-wu.svg)](https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it) |

<a id="tips-claudemd"></a>■ **CLAUDE.md + .claude/rules (8)**

| Tip | Source |
|-----|--------|
| 每个文件的 [CLAUDE.md](https://code.claude.com/docs/en/memory) 应控制在 [200 行](https://code.claude.com/docs/en/memory#write-effective-instructions) 以内。humanlayer 中是 [60 行](https://www.humanlayer.dev/blog/writing-a-good-claude-md)（[仍然不是 100% 保证](https://www.reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/)） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179840848597422) [![Dex](!/tags/community-dex.svg)](https://www.humanlayer.dev/blog/writing-a-good-claude-md) |
| .claude/rules/*.md 像 CLAUDE.md 一样 auto-load 到每个 session 中 — 添加 paths: YAML frontmatter 使其仅在 Claude 触及匹配 glob 的文件时才 lazy-load | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| 将特定领域的 CLAUDE.md 规则包装在 [\<important if="..."\> tags](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md) 中，以防止随着文件变长 Claude 忽略它们 | [![Dex](!/tags/community-dex.svg)](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md) |
| 对 monorepos 使用 [multiple CLAUDE.md](best-practice/claude-memory.md) — ancestor + descendant loading | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2016339448863355206) |
| 使用 [.claude/rules/](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) 来拆分大型指令 | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| 任何开发者都应该能够启动 Claude，说 "run the tests"，并且第一次就能运行 — 如果不能，你的 CLAUDE.md 缺少了必要的 setup/build/test 命令 | [![Dex](!/tags/community-dex.svg)](https://x.com/dexhorthy/status/2034713765401551053) |
| 保持 codebases 干净并完成 migrations — 部分迁移的 frameworks 会混淆可能选择错误模式的 models | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=1112) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=1112) |
| 使用 [settings.json](best-practice/claude-settings.md) 实现 harness-enforced 行为（attribution, permissions, model）— 不要在 CLAUDE.md 中写 "NEVER add Co-Authored-By"，当 attribution.commit: "" 是确定性的时候 | [![davila7](!/tags/community-davila7.svg)](https://x.com/dani_avila7/status/2036182734310195550) |

<a id="tips-agents"></a><img src="!/tags/a.svg" height="14"> **Agents (4)**

| Tip | Source |
|-----|--------|
| 使用功能特定的 [sub-agents](https://code.claude.com/docs/en/sub-agents)（extra context）配合 [skills](https://code.claude.com/docs/en/skills)（progressive disclosure），而非 general qa, backend engineer | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179850139000872) |
| 说 "use subagents" 来投入更多 compute 解决问题 — 将 tasks offload 以保持主 context 干净和专注 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| [agent teams with tmux](https://code.claude.com/docs/en/agent-teams) 和 [git worktrees](https://x.com/bcherny/status/2025007393290272904) 用于 parallel development | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2025007393290272904) |
| 使用 [test time compute](https://code.claude.com/docs/en/sub-agents) — 分离的 context windows 使结果更好；一个 agent 可以制造 bugs，另一个（相同 model）可以发现它们 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031151689219321886) |

<a id="tips-commands"></a><img src="!/tags/c.svg" height="14"> **Commands (3)**

| Tip | Source |
|-----|--------|
| 使用 [commands](https://code.claude.com/docs/en/slash-commands) 构建你的 workflows，而非 [sub-agents](https://code.claude.com/docs/en/sub-agents) | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 为每天做很多次的每个 "inner loop" workflow 使用 [slash commands](https://code.claude.com/docs/en/slash-commands) — 节省重复 prompting，commands 存放在 .claude/commands/ 并检入 git | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 如果你每天做某件事超过一次，将其变为 [skill](https://code.claude.com/docs/en/skills) 或 [command](https://code.claude.com/docs/en/slash-commands) — 构建 /techdebt, context-dump 或 analytics commands | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742748984742078) |

<a id="tips-skills"></a><img src="!/tags/s.svg" height="14"> **Skills (9)**

| Tip | Source |
|-----|--------|
| 使用 [context: fork](https://code.claude.com/docs/en/skills) 在隔离的 subagent 中运行 skill — 主 context 只看到最终结果，看不到中间 tool calls。agent 字段允许你设置 subagent 类型 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2033603164398883042) |
| 对 monorepos 使用 [skills in subfolders](reports/claude-skills-for-larger-mono-repos.md) | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/skills) |
| skills 是文件夹，不是文件 — 使用 references/, scripts/, examples/ 子目录实现 [progressive disclosure](https://code.claude.com/docs/en/skills) | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在每个 skill 中构建 Gotchas 章节 — 这是最高信号的 content，随时间添加 Claude 的 failure points | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| skill 的 description 字段是一个 trigger，不是 summary — 为 model 编写（"我应该在什么时候 fire？"） | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在 skills 中陈述显而易见的内容 — 专注于推动 Claude 偏离 default behavior 的内容 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在 skills 中 railroad Claude — 给 goals 和 constraints，而非 prescriptive step-by-step instructions 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在 skills 中包含 scripts 和 libraries，以便 Claude 组合而非重建 boilerplate | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在 SKILL.md 中嵌入 !command 以将动态 shell output inject 到 prompt 中 — Claude 在 invocation 时运行它，model 只看到结果 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2034337963820327017) |

<a id="tips-hooks"></a>■ **Hooks (5)**

| Tip | Source |
|-----|--------|
| 在 skills 中使用 [on-demand hooks](https://code.claude.com/docs/en/skills) — /careful 阻止 destructive commands，/freeze 阻止编辑 directory 外的内容 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 PreToolUse hook [measure skill usage](https://code.claude.com/docs/en/skills)，以发现流行或触发不足的 skills | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 [PostToolUse hook](https://code.claude.com/docs/en/hooks) 自动格式化代码 — Claude 生成良好格式的代码，hook 处理最后 10% 以避免 CI 失败 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179852047335529) |
| 通过 hook 将 [permission requests](https://code.claude.com/docs/en/hooks) route 到 Opus — 让它扫描攻击并自动批准安全的请求 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| 使用 [Stop hook](https://code.claude.com/docs/en/hooks) 在 turn 结束时 nudge Claude 继续或验证其工作 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701059253874861) |

<a id="tips-workflows"></a>■ **Workflows (5)**

| Tip | Source |
|-----|--------|
| 使用 [/model](https://code.claude.com/docs/en/model-config) 选择 model 和 reasoning，[/context](https://code.claude.com/docs/en/interactive-mode) 查看 context 使用量，[/usage](https://code.claude.com/docs/en/costs) 检查 plan limits，[/extra-usage](https://code.claude.com/docs/en/interactive-mode) 配置 overflow billing，[/config](https://code.claude.com/docs/en/settings) 配置 settings — 使用 Opus 做 plan mode，Sonnet 写 code，兼得两者之长 | [![Cat](!/tags/cat-wu.svg)](https://x.com/_catwu/status/1955694117264261609) |
| 始终在 [/config](https://code.claude.com/docs/en/settings) 中使用 [thinking mode](https://code.claude.com/docs/en/model-config) true（以查看 reasoning）和 [Output Style](https://code.claude.com/docs/en/output-styles) Explanatory（以查看带有 ★ Insight 盒子的详细输出），更好地理解 Claude 的决策 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179838864666847) |
| 在 prompts 中使用 ultrathink 关键词进行 [high effort reasoning](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#tips-and-best-practices) | [![Claude](!/tags/claude.svg)](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#tips-and-best-practices) |
| /focus mode 隐藏所有中间工作，只显示最终结果 — 信任 model 运行正确的命令，只看结果（用 /focus 切换） | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| 使用 Opus 4.7 的 adaptive thinking 调整 effort level — low 追求速度和更少 tokens，max 追求最高智能（slider: low · medium · high · xhigh · max） | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |

<a id="tips-workflows-advanced"></a>■ **Workflows Advanced (9)**

| Tip | Source |
|-----|--------|
| 大量使用 ASCII diagrams 来理解你的 architecture | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742759218794768) |
| 使用 [/loop](https://code.claude.com/docs/en/scheduled-tasks) 进行本地定期监控（最长 7 天）· 使用 [/schedule](https://code.claude.com/docs/en/routines) 进行即使在你的机器关闭时也能运行的基于云的定期 tasks | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038454341884154269) |
| 使用 [Ralph Wiggum plugin](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop) 进行长时间运行的 autonomous tasks | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179858435281082) |
| [/permissions](https://code.claude.com/docs/en/permissions) 使用 wildcard 语法（Bash(npm run *), Edit(/docs/**)）而非 dangerously-skip-permissions | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179854077407667) |
| [/sandbox](https://code.claude.com/docs/en/sandboxing) 通过文件和网络隔离减少 permission prompts — 内部减少了 84% | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700506465579443) [![Cat](!/tags/cat-wu.svg)](https://creatoreconomy.so/p/inside-claude-code-how-an-ai-native-actually-works-cat-wu) |
| 投入于 [product verification](https://code.claude.com/docs/en/skills) skills（signup-flow-driver, checkout-verifier）— 值得花一周来完善 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 [auto mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) 而非 dangerously-skip-permissions — 基于 model 的 classifier 判断每个命令是否安全并自动批准，有风险时暂停询问。Shift+Tab 循环切换 Ask → Plan → Auto modes 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| 使用 /less-permission-prompts skill 扫描 session history 中反复提示的 safe bash/MCP 命令，然后获得推荐的 allowlist 粘贴到 [settings](best-practice/claude-settings.md) 中 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| 构建一个 /go skill，它 (1) 通过 bash/browser/computer use 进行端到端 test (2) 运行 /simplify (3) 提交 PR — 这样当你回来时，你知道代码是能工作的 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |

<a id="tips-git-pr"></a>■ **Git / PR (5)**

| Tip | Source |
|-----|--------|
| 保持 PRs 小而专注 — [p50 为 118 行](tips/claude-boris-2-tips-25-mar-26.md)（141 个 PRs，一天内更改 45K 行），每个 PR 一个 feature，更容易 review 和 revert | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 始终 [squash merge](tips/claude-boris-2-tips-25-mar-26.md) PRs — 清晰的 linear history，每个 feature 一个 commit，便于 git revert 和 git bisect | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 经常 commit — 尽量至少每小时 commit 一次，task 一完成就 commit | ![Shayan](!/tags/community-shayan.svg) |
| 在同事的 PR 上 tag [@claude](https://github.com/apps/claude) 来自动生成针对重复 review feedback 的 lint rules — 让自己从 code review 中自动化出来 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=2715) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=2715) |
| 使用 [/code-review](https://code.claude.com/docs/en/code-review) 进行 multi-agent PR 分析 — 在 merge 前捕获 bugs, security vulnerabilities 和 regressions | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031089411820228645) |

<a id="tips-debugging"></a>■ **Debugging (6)**

| Tip | Source |
|-----|--------|
| 养成习惯，无论何时遇到任何 issue 都截图并与 Claude 分享 | ![Shayan](!/tags/community-shayan.svg) |
| 使用 mcp（[Claude in Chrome](https://code.claude.com/docs/en/chrome), [Playwright](https://github.com/microsoft/playwright-mcp), [Chrome DevTools](https://developer.chrome.com/blog/chrome-devtools-mcp)）让 claude 自行查看 chrome console logs | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/chrome) |
| 始终让 claude 将你想要查看日志的终端作为 background task 运行，以便更好地 debugging | ![Shayan](!/tags/community-shayan.svg) |
| [/doctor](https://code.claude.com/docs/en/cli-reference) 用于诊断 installation, authentication 和 configuration 问题 | ![Shayan](!/tags/community-shayan.svg) |
| 使用 [cross-model](development-workflows/cross-model-workflow/cross-model-workflow.md) 进行 QA — 例如用 [Codex](https://github.com/shanraisshan/codex-cli-best-practice) 做 plan 和 implementation review | ![Shayan](!/tags/community-shayan.svg) |
| agentic search（glob + grep）优于 RAG — Claude Code 尝试并放弃了 vector databases，因为代码会不同步且 permissions 复杂 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3095) [![Video](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3095) |

<a id="tips-utilities"></a>■ **Utilities (5)**

| Tip | Source |
|-----|--------|
| [iTerm](https://iterm2.com/)/[Ghostty](https://ghostty.org/)/[tmux](https://github.com/tmux/tmux) 终端而非 IDE（[VS Code](https://code.visualstudio.com/)/[Cursor](https://www.cursor.com/)） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742753971769626) |
| [/voice](https://code.claude.com/docs/en/voice-dictation) 或 [Wispr Flow](https://wisprflow.ai) 用于语音 prompting（10x 生产力） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038454362226467112) |
| [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 用于 claude 反馈 | ![Shayan](!/tags/community-shayan.svg) |
| [status line](https://github.com/shanraisshan/claude-code-status-line) 用于 context 感知和快速 compacting | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700784019452195) ![Shayan](!/tags/community-shayan.svg) |
| 探索 [settings.json](best-practice/claude-settings.md) features 如 [Plans Directory](best-practice/claude-settings.md#plans-directory), [Spinner Verbs](best-practice/claude-settings.md#display--ux) 以获得个性化体验 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701145023197516) |

<a id="tips-daily"></a>■ **Daily (2)**

| Tip | Source |
|-----|--------|
| 每天 [update](https://code.claude.com/docs/en/setup) Claude Code | ![Shayan](!/tags/community-shayan.svg) |
| 每天以阅读 [changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) 开始 | ![Shayan](!/tags/community-shayan.svg) |

![Boris Cherny + Team](!/tags/claude.svg)

| Article / Tweet | Source |
|-----------------|--------|
| [6 Tips for Getting More Out of Opus 4.7 (Boris) \| 16/Apr/26](tips/claude-boris-6-tips-16-apr-26.md) | [Tweet](https://x.com/bcherny) |
| [Session Management & 1M Context (Thariq) \| 16/Apr/26](tips/claude-thariq-tips-16-apr-26.md) | [Tweet](https://x.com/trq212) |
| [15 Hidden & Under-Utilized Features in Claude Code (Boris) \| 30/Mar/26](tips/claude-boris-15-tips-30-mar-26.md) | [Tweet](https://x.com/bcherny/status/2038454336355999749) |
| [Squash Merging & PR Size Distribution (Boris) \| 25/Mar/26](tips/claude-boris-2-tips-25-mar-26.md) | [Tweet](https://x.com/bcherny/status/2038552880018538749) |
| [Lessons from Building Claude Code: How We Use Skills (Thariq) \| 17/Mar/26](tips/claude-thariq-tips-17-mar-26.md) | [Article](https://x.com/trq212/status/2033949937936085378) |
| [Code Review & Test Time Compute (Boris) \| 10/Mar/26](tips/claude-boris-2-tips-10-mar-26.md) | [Tweet](https://x.com/bcherny/status/2031089411820228645) |
| /loop — 规划最长 3 天的定期 tasks (Boris) \| 07 Mar 2026 | [Tweet](https://x.com/bcherny/status/2030193932404150413) |
| AskUserQuestion + ASCII Markdowns (Thariq) \| 28 Feb 2026 | [Tweet](https://x.com/trq212/status/2027543858289250472) |
| Seeing like an Agent - lessons from building Claude Code (Thariq) \| 28 Feb 2026 | [Article](https://x.com/trq212/status/2027463795355095314) |
| Git Worktrees - 5 ways how boris is using \| 21 Feb 2026 | [Tweet](https://x.com/bcherny/status/2025007393290272904) |
| Lessons from Building Claude Code: Prompt Caching Is Everything (Thariq) \| 20 Feb 2026 | [Article](https://x.com/trq212/status/2024574133011673516) |
| [12 ways how people are customizing their claudes (Boris) \| 12/Feb/26](tips/claude-boris-12-tips-12-feb-26.md) | [Tweet](https://x.com/bcherny/status/2021699851499798911) |
| [10 tips for using Claude Code from the team (Boris) \| 01/Feb/26](tips/claude-boris-10-tips-01-feb-26.md) | [Tweet](https://x.com/bcherny/status/2017742741636321619) |
| [How I use Claude Code — 13 tips from my surprisingly vanilla setup (Boris) \| 03/Jan/26](tips/claude-boris-13-tips-03-jan-26.md) | [Tweet](https://x.com/bcherny/status/2007179832300581177) |
| Ask Claude to interview you using AskUserQuestion tool (Thariq) \| 28/Dec/25 | [Tweet](https://x.com/trq212/status/2005315275026260309) |
| 始终使用 plan mode，给 Claude 一种验证方式，使用 /code-review (Boris) \| 27/Dec/25 | [Tweet](https://x.com/bcherny/status/2004711722926616680) |

#### 来自 Claude code CLI 二进制的 Tips

[Spinner Verbs & Tips (extracted from CLI binary v2.1.121)](reports/claude-spinner-verbs-and-tips.md)

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 🎬 VIDEOS / PODCASTS

| Video / Podcast | Source | YouTube |
|-----------------|--------|--------|
| From Vibe Coding to Agentic Engineering (Andrej) \| 02 May 2026 \| AI Engineer | [![Karpathy](!/tags/community-karpathy.svg)](https://x.com/karpathy) | [YouTube](https://www.youtube.com/watch?v=96jN2OCOfLs) |
| Full Walkthrough: Workflow for AI Coding (Matt) \| 24 Apr 2026 \| Matt Pocock | [![Matt](!/tags/community-matt.svg)](https://x.com/mattpocockuk) | [YouTube](https://youtu.be/-QFHIoCo-Ko) |
| Everything We Got Wrong About Research-Plan-Implement (Dex) \| 24 Mar 2026 \| MLOps Community | [![Dex](!/tags/community-dex.svg)](https://x.com/daborhyde) | [YouTube](https://youtu.be/YwZR6tc7qYg) |
| Building Claude Code with Boris Cherny (Boris) \| 04 Mar 2026 \| The Pragmatic Engineer | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/julbw1JuAz0) |
| Head of Claude Code: What happens after coding is solved (Boris) \| 19 Feb 2026 \| Lenny's Podcast | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/We7BZVKbCVw) |
| Inside Claude Code With Its Creator Boris Cherny (Boris) \| 17 Feb 2026 \| Y Combinator | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/PQU9o_5rHC4) |
| Boris Cherny (Creator of Claude Code) On What Grew His Career (Boris) \| 15 Dec 2025 \| Ryan Peterman | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/AmdLVWMdjOk) |
| The Secrets of Claude Code From the Engineers Who Built It (Cat) \| 29 Oct 2025 \| Every | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/IDSAMqip6ms) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## 🔔 SUBSCRIBE

| Source | Name | Badge |
|--------|------|-------|
| ![Reddit](https://img.shields.io/badge/-FF4500?style=flat&logo=reddit&logoColor=white) | [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/), [r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/), [r/Anthropic](https://www.reddit.com/r/Anthropic/) | ![Boris + Team](!/tags/claude.svg) |
| ![X](https://img.shields.io/badge/-000?style=flat&logo=x&logoColor=white) | [Claude](https://x.com/claudeai), [Claude Devs](https://x.com/ClaudeDevs), [Anthropic](https://x.com/AnthropicAI), [Boris](https://x.com/bcherny), [Thariq](https://x.com/trq212), [Cat](https://x.com/_catwu), [Lydia](https://x.com/lydiahallie), [Noah](https://x.com/noahzweben), [Anthony](https://x.com/amorriscode), [Alex](https://x.com/alexalbert__), [Kenneth](https://x.com/neilhtennek) | ![Boris + Team](!/tags/claude.svg) |
| ![X](https://img.shields.io/badge/-000?style=flat&logo=x&logoColor=white) | [Jesse Kriss](https://x.com/obra) ([Superpowers](https://github.com/obra/superpowers)), [Affaan Mustafa](https://x.com/affaanmustafa) ([ECC](https://github.com/affaan-m/everything-claude-code)), [Garry Tan](https://x.com/garrytan) ([gstack](https://github.com/garrytan/gstack)), [Dex Horthy](https://x.com/dexhorthy) ([HumanLayer](https://github.com/humanlayer/humanlayer)), [Kieran Klaassen](https://x.com/kieranklaassen) ([Compound Eng](https://github.com/EveryInc/compound-engineering-plugin)), [Tabish Gilani](https://x.com/0xTab) ([OpenSpec](https://github.com/Fission-AI/OpenSpec)), [Brian McAdams](https://x.com/BMadCode) ([BMAD](https://github.com/bmad-code-org/BMAD-METHOD)), [Lex Christopherson](https://x.com/official_taches) ([GSD](https://github.com/gsd-build/get-shit-done)), [Matt Pocock](https://x.com/mattpocockuk) ([Skills](https://github.com/mattpocock/skills)), [Dani Avila](https://x.com/dani_avila7) ([CC Templates](https://github.com/davila7/claude-code-templates)), [Dan Shipper](https://x.com/danshipper) ([Every](https://every.to/)), [Andrej Karpathy](https://x.com/karpathy) ([AutoResearch](https://x.com/karpathy/status/2015883857489522876)), [Peter Steinberger](https://x.com/steipete) ([OpenClaw](https://x.com/openclaw)), [Sigrid Jin](https://x.com/realsigridjin) ([claw-code](https://github.com/ultraworkers/claw-code)), [Yeachan Heo](https://x.com/bellman_ych) ([oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode)) | ![Community](!/tags/community.svg) |
| ![YouTube](https://img.shields.io/badge/-F00?style=flat&logo=youtube&logoColor=white) | [Anthropic](https://www.youtube.com/@anthropic-ai) | ![Boris + Team](!/tags/claude.svg) |
| ![YouTube](https://img.shields.io/badge/-F00?style=flat&logo=youtube&logoColor=white) | [Lenny's Podcast](https://www.youtube.com/@LennysPodcast), [Y Combinator](https://www.youtube.com/@ycombinator), [The Pragmatic Engineer](https://www.youtube.com/@pragmaticengineer), [Ryan Peterman](https://www.youtube.com/@ryanlpeterman), [Every](https://www.youtube.com/@every_media), [MLOps Community](https://www.youtube.com/@MLOps) | ![Community](!/tags/community.svg) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## ☠️ STARTUPS / BUSINESSES

| Claude | Replaced |
|-|-|
|[**Code Review**](https://code.claude.com/docs/en/code-review)|[Greptile](https://greptile.com), [CodeRabbit](https://coderabbit.ai), [Devin Review](https://devin.ai), [OpenDiff](https://opendiff.com), [Cursor BugBot](https://bugbot.dev)|
|[**Voice Dictation**](https://code.claude.com/docs/en/voice-dictation)|[Wispr Flow](https://wisprflow.ai), [SuperWhisper](https://superwhisper.com/)|
|[**Remote Control**](https://code.claude.com/docs/en/remote-control)|[OpenClaw](https://openclaw.ai/)
|[**Claude in Chrome**](https://code.claude.com/docs/en/chrome)|[Playwright MCP](https://github.com/microsoft/playwright-mcp), [Chrome DevTools MCP](https://developer.chrome.com/blog/chrome-devtools-mcp)|
|[**Computer Use**](https://docs.anthropic.com/en/docs/agents-and-tools/computer-use)|[OpenAI CUA](https://openai.com/index/computer-using-agent/)|
|[**Cowork**](https://claude.com/blog/cowork-research-preview)|[ChatGPT Agent](https://openai.com/chatgpt/agent/), [Perplexity Computer](https://www.perplexity.ai/computer/), [Manus](https://manus.im)|
|[**Tasks**](https://x.com/trq212/status/2014480496013803643)|[Beads](https://github.com/steveyegge/beads)
|[**Plan Mode**](https://code.claude.com/docs/en/common-workflows)|[Agent OS](https://github.com/buildermethods/agent-os)|
|[**Design**](https://claude.com/design)|[Figma](https://figma.com), [Framer](https://framer.com), [Sketch](https://sketch.com), [v0](https://v0.dev)|
|[**Agent SDK**](https://code.claude.com/docs/en/agent-sdk/overview)|[LangChain](https://langchain.com), [LangGraph](https://www.langchain.com/langgraph), [CrewAI](https://www.crewai.com), [AutoGen](https://github.com/microsoft/autogen), [OpenAI Assistants API](https://platform.openai.com/docs/assistants/overview)|
|[**Skills / Plugins**](https://code.claude.com/docs/en/plugins)|YC AI wrapper startups ([reddit](https://reddit.com/r/ClaudeAI/comments/1r6bh4d/claude_code_skills_are_basically_yc_ai_startup/))|

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="billion-dollar-questions"></a>
![Billion-Dollar Questions](!/tags/billion-dollar-questions.svg)

*如果你有答案，请通过 shanraisshan@gmail.com 告诉我*

**Memory & Instructions (4)**

1. 你的 CLAUDE.md 中到底应该放什么 — 又不该放什么？
2. 如果你已经有 CLAUDE.md，是否真的需要一个单独的 constitution.md 或 rules.md？
3. 应该多久更新一次 CLAUDE.md，如何知道它已经过时了？
4. 为什么 Claude 仍然会忽略 CLAUDE.md 中的指令 — 即使它们用全大写的 MUST？（[reddit](https://reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/)）

**Agents, Skills & Workflows (6)**

1. 什么时候应该使用 command、agent 还是 skill — 以及什么时候 vanilla Claude Code 更好？
2. 随着 models 的改进，应该多久更新一次 agents、commands 和 workflows？
3. 应该有 generalist subagent 还是 feature-specific/role-specific agent？给 subagent 一个详细的 persona 是否会提高质量，"完美的 persona prompt"（用于 research/vision）是什么样子的？
4. 应该依赖 Claude Code 的内置 plan mode — 还是构建自己的 planning command/agent 来强制执行团队的 workflow？
5. 如果你有一个 personal skill（例如带有你编码风格的 /implement），如何整合 community skills（例如 /simplify）而不产生冲突 — 当它们意见不一致时谁会赢？
6. 我们到达那个阶段了吗？能否将现有 codebase 转换为 specs，删除代码，然后仅从这些 specs 中让 AI 重新生成完全相同的代码？

**Specs & Documentation (3)**

1. 仓库中的每个 feature 是否都应该有一个 markdown 文件作为 spec？
2. 需要多频繁更新 specs 以确保它们在实现新 feature 时不会过时？
3. 在实现新 feature 时，如何处理对其他 features 的 specs 产生的连锁反应？

### 🤔 [代码还重要吗？](https://github.com/shanraisshan/agentic-engineering)

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## REPORTS

<p align="center">
  <a href="reports/claude-agent-sdk-vs-cli-system-prompts.md"><img src="https://img.shields.io/badge/Agent_SDK_vs_CLI-555?style=for-the-badge" alt="Agent SDK vs CLI"></a>
  <a href="reports/claude-in-chrome-v-chrome-devtools-mcp.md"><img src="https://img.shields.io/badge/Browser_Automation_MCP-555?style=for-the-badge" alt="Browser Automation MCP"></a>
  <a href="reports/claude-global-vs-project-settings.md"><img src="https://img.shields.io/badge/Global_vs_Project_Settings-555?style=for-the-badge" alt="Global vs Project Settings"></a>
  <a href="reports/claude-skills-for-larger-mono-repos.md"><img src="https://img.shields.io/badge/Skills_in_Monorepos-555?style=for-the-badge" alt="Skills in Monorepos"></a>
  <br>
  <a href="reports/claude-agent-memory.md"><img src="https://img.shields.io/badge/Agent_Memory-555?style=for-the-badge" alt="Agent Memory"></a>
  <a href="reports/claude-advanced-tool-use.md"><img src="https://img.shields.io/badge/Advanced_Tool_Use-555?style=for-the-badge" alt="Advanced Tool Use"></a>
  <a href="reports/claude-usage-and-rate-limits.md"><img src="https://img.shields.io/badge/Usage_&_Rate_Limits-555?style=for-the-badge" alt="Usage & Rate Limits"></a>
  <a href="reports/claude-agent-command-skill.md"><img src="https://img.shields.io/badge/Agents_vs_Commands_vs_Skills-555?style=for-the-badge" alt="Agents vs Commands vs Skills"></a>
  <br>
  <a href="reports/llm-day-to-day-degradation.md"><img src="https://img.shields.io/badge/LLM_Degradation-555?style=for-the-badge" alt="LLM Degradation"></a>
  <a href="reports/why-harness-is-important.md"><img src="https://img.shields.io/badge/Why_Harness_is_Important-555?style=for-the-badge" alt="Why Harness is Important"></a>
  <a href="reports/claude-spinner-verbs-and-tips.md"><img src="https://img.shields.io/badge/Spinner_Verbs_&_Tips-555?style=for-the-badge" alt="Spinner Verbs & Tips"></a>
</p>

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<a id="how-to-use"></a>

## <img src="!/tags/how-to-use-hd.svg" alt="How to Use">

按照以下步骤最大限度地利用本仓库：

1. **将本仓库作为课程阅读，而非 workflow 或 skill。** 它首先是参考材料，你会稍后运行其中的内容。
2. **不要将 Claude 当作 chatbot 使用。** 学习 primitives — agents、commands、skills、hooks — 并将它们组合成你自己的 workflow。
3. **运行 [`/weather-orchestrator`](orchestration-workflow/orchestration-workflow.md)** 来查看完整的 command → agent → skill 流程。将其作为从 planning 到 shipping 的任何开发 workflow 的模板。
4. **在工作时注意自定义 hook 声音。** 它们的实现位于专门的 [Claude Code Hooks 仓库](https://github.com/shanraisshan/claude-code-hooks) 中；其他模式如 [Agent Teams](implementation/claude-agent-teams-implementation.md) 则在本仓库的 `implementation/` 目录内。
5. **从 [🔥 Hot](#-hot) 子表中学习高级主题及其实现** — 例如，[Ralph Wiggum self-evolving loop](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop) 是一个完整的工作仓库，你可以克隆它以端到端地查看其中一种模式。
6. **将 Claude 指向你自己项目中的 [tips and tricks](#-tips-and-tricks-83) 章节**，要求它建议编辑 — 特别是如何重构你的 `CLAUDE.md`。每条 tip 都来自 Claude 团队或社区。
7. **订阅 [Subscribe 章节](#-subscribe) 中的 Reddit 和 YouTube 频道**以跟上社区动态。

**🎬 Videos**

<a href="https://www.youtube.com/watch?v=AkAhkalkRY4"><img src="!/thumbnail/video-1.png" alt="Watch on YouTube" width="240"></a>
<a href="https://youtu.be/lPjhM6BBK0Q"><img src="!/thumbnail/video-2.png" alt="Watch on YouTube" width="240"></a>

**📊 Presentations**

<a href="https://github.com/shanraisshan/claude-code-best-practice/tree/main/presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini"><img src="!/thumbnail/presentation-1.png" alt="Claude Code & Gemini CLI — GDG Kolachi" width="240"></a>

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

<p align="center">
  <a href="https://github.com/trending?since=monthly"><img src="!/root/github-trending.png" alt="GitHub Trending" width="1200"></a><br>
  ✨2026 年 3 月在 Github 上 Trending✨
</p>

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=shanraisshan/claude-code-best-practice&type=Date&v=2)](https://star-history.com/#shanraisshan/claude-code-best-practice&Date)

<a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers"><img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=flat&label=%E2%98%85&labelColor=555&color=white" alt="GitHub Stars" align="center"></a> stars 并持续增长

## Other Repos

<table>
<tr>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/claude-code-hooks"><img src="!/claude-speaking.svg" alt="Claude Code Hooks" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/claude-code-hooks"><strong>Claude Code<br>Hooks</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/codex-cli-best-practice"><img src="!/codex-jumping.svg" alt="Codex CLI Best Practice" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/codex-cli-best-practice"><strong>Codex CLI<br>Best Practice</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/codex-cli-hooks"><img src="!/codex-speaking.svg" alt="Codex CLI Hooks" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/codex-cli-hooks"><strong>Codex CLI<br>Hooks</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/gemini-cli-best-practice"><img src="!/gemini-jumping.svg" alt="Gemini CLI Best Practice" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/gemini-cli-best-practice"><strong>Gemini CLI<br>Best Practice</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/gemini-cli-hooks"><img src="!/gemini-speaking.svg" alt="Gemini CLI Hooks" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/gemini-cli-hooks"><strong>Gemini CLI<br>Hooks</strong></a>
</td>
</tr>
</table>

## Developed by

![Developed by](!/tags/developed-by.svg)

> | # | Workflow | Description |
> |---|----------|-------------|
> | 1 | /workflows:development-workflows | 通过并行研究所有 10 个 workflow 仓库，更新 DEVELOPMENT WORKFLOWS 表和跨 workflow 分析报告 |
> | 2 | /workflows:skill-collections | 通过并行研究所有 5 个 skill-collection 仓库，更新 SKILL COLLECTIONS 表 |
> | 3 | /workflows:agent-collections | 通过并行研究所有 agent-collection 仓库，更新 AGENT COLLECTIONS 表 |
> | 4 | /workflows:best-practice:workflow-concepts | 使用最新的 Claude Code 功能和概念更新 README CONCEPTS 章节 |
> | 5 | /workflows:best-practice:workflow-claude-settings | 跟踪 Claude Code settings 报告变化，找出需要更新的内容 |
> | 6 | /workflows:best-practice:workflow-claude-subagents | 跟踪 Claude Code subagents 报告变化，找出需要更新的内容 |
> | 7 | /workflows:best-practice:workflow-claude-commands | 跟踪 Claude Code commands 报告变化，找出需要更新的内容 |
> | 8 | /workflows:best-practice:workflow-claude-skills | 跟踪 Claude Code skills 报告变化，找出需要更新的内容 |

## Extras

[![Claude for OSS](!/tags/claude-for-oss.svg)](https://claude.com/contact-sales/claude-for-oss)
[![Claude Community Ambassador](!/tags/claude-community-ambassador.svg)](https://claude.com/community/ambassadors)
[![Claude Certified Architect](!/tags/claude-certified-architect.svg)](https://anthropic.skilljar.com/claude-certified-architect-foundations-access-request)
[![Anthropic Academy](!/tags/anthropic-academy.svg)](https://anthropic.skilljar.com/)
[![Join Claude Pakistan community on WhatsApp](!/tags/whatsapp-claude-pakistan.svg)](https://chat.whatsapp.com/BDUV2stIS0c7X5uY7RY6nS)

<p align="center">
  <img src="!/claude-jumping.svg" alt="section divider" width="60" height="50">
</p>

## <img src="!/tags/sponsor-heart.svg" width="22" height="22" align="center"> Sponsor My Work

如果你喜欢我的工作，请我喝杯 doodh patti 🍵

<a href="https://buy.polar.sh/polar_cl_R6wjUESl8RiJD0iVaTyStBUV6WNuYvDmLJ0si1XXj4C"><img src="!/tags/polar.svg" alt="Polar" width="40" height="40" align="center"></a> <a href="https://buy.polar.sh/polar_cl_R6wjUESl8RiJD0iVaTyStBUV6WNuYvDmLJ0si1XXj4C"><strong>Polar</strong></a>

**想让你的品牌出现在页首？** 页首展示位可供投放 — 请发送邮件至 [shanraisshan@gmail.com](mailto:shanraisshan@gmail.com)。
