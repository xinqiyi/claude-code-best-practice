# claude-code-best-practice
从 vibe coding 到 agentic engineering - 熟能生巧，让 claude 更完美

![updated with Claude Code](https://img.shields.io/badge/updated_with_Claude_Code-v2.1.145%20(May%2023%2C%202026%208%3A46%20PM%20PKT)-white?style=flat&labelColor=555) <a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers"><img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=flat&label=%E2%98%85&labelColor=555&color=white" alt="GitHub Stars"></a><br>

[![最佳实践](!/tags/best-practice.svg)](best-practice/) [![已实现](!/tags/implemented.svg)](implementation/) [![编排工作流](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) [![Claude](!/tags/claude.svg)](https://code.claude.com/docs) [![Boris](!/tags/boris-cherny.svg)](#-tips-and-tricks) [![社区](!/tags/community.svg)](#-subscribe) ![点击下方徽章查看实际来源](!/tags/click-badges.svg)<br>
<img src="!/tags/a.svg" height="14"> = Agents · <img src="!/tags/c.svg" height="14"> = Commands · <img src="!/tags/s.svg" height="14"> = Skills

<p align="center">
  <img src="!/claude-jumping.svg" alt="Claude Code 吉祥物跳跃" width="120" height="100"><br>
  <a href="https://github.com/trending"><img src="!/root/github-trending-day.svg" alt="GitHub 每日热门 #1 仓库"></a>
</p>

<p align="center">
  <img src="!/root/boris-slider.gif" alt="Boris Cherny 谈 Claude Code" width="600"><br>
  Boris Cherny 在 X 上（<a href="https://x.com/bcherny/status/2007179832300581177">推文 1</a> · <a href="https://x.com/bcherny/status/2017742741636321619">推文 2</a> · <a href="https://x.com/bcherny/status/2021699851499798911">推文 3</a>）
</p>

> [!TIP]
> 请访问 [**如何使用**](#how-to-use) 部分以充分利用本仓库。

## 🧠 概念

| 功能 | 位置 | 描述 |
|---------|----------|-------------|
| <img src="!/tags/a.svg" height="14"> [**Subagents**](https://code.claude.com/docs/en/sub-agents) | `.claude/agents/<name>.md` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-subagents.md) [![已实现](!/tags/implemented.svg)](implementation/claude-subagents-implementation.md) |
| <img src="!/tags/c.svg" height="14"> [**Commands**](https://code.claude.com/docs/en/slash-commands) | `.claude/commands/<name>.md` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-commands.md) [![已实现](!/tags/implemented.svg)](implementation/claude-commands-implementation.md) |
| <img src="!/tags/s.svg" height="14"> [**Skills**](https://code.claude.com/docs/en/skills) | `.claude/skills/<name>/SKILL.md` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-skills.md) [![已实现](!/tags/implemented.svg)](implementation/claude-skills-implementation.md) [官方 Skills](https://github.com/anthropics/skills/tree/main/skills) · [单仓库 Skills](reports/claude-skills-for-larger-mono-repos.md) |
| [**Workflows**](https://code.claude.com/docs/en/common-workflows) | [`.claude/commands/weather-orchestrator.md`](.claude/commands/weather-orchestrator.md) | [![编排工作流](!/tags/orchestration-workflow.svg)](orchestration-workflow/orchestration-workflow.md) |
| [**Hooks**](https://code.claude.com/docs/en/hooks) | `.claude/hooks/` | [![最佳实践](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-hooks) [![已实现](!/tags/implemented.svg)](https://github.com/shanraisshan/claude-code-hooks) [指南](https://code.claude.com/docs/en/hooks-guide) |
| [**MCP 服务器**](https://code.claude.com/docs/en/mcp) | `.claude/settings.json`, `.mcp.json` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-mcp.md) [![已实现](!/tags/implemented.svg)](.mcp.json) |
| [**Plugins**](https://code.claude.com/docs/en/plugins) | 可分发的包 | [市场](https://code.claude.com/docs/en/discover-plugins) · [创建市场](https://code.claude.com/docs/en/plugin-marketplaces) |
| [**Settings**](https://code.claude.com/docs/en/settings) | `.claude/settings.json` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-settings.md) [![已实现](!/tags/implemented.svg)](.claude/settings.json) [权限](https://code.claude.com/docs/en/permissions) · [模型配置](https://code.claude.com/docs/en/model-config) · [输出样式](https://code.claude.com/docs/en/output-styles) · [沙箱](https://code.claude.com/docs/en/sandboxing) · [按键绑定](https://code.claude.com/docs/en/keybindings) · [自动模式配置](https://code.claude.com/docs/en/auto-mode-config) |
| [**状态栏**](https://code.claude.com/docs/en/statusline) | `.claude/settings.json` | [![最佳实践](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-status-line) [![已实现](!/tags/implemented.svg)](.claude/settings.json) |
| [**Memory**](https://code.claude.com/docs/en/memory) | `CLAUDE.md`, `.claude/rules/`, `~/.claude/rules/`, `~/.claude/projects/<project>/memory/` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-memory.md) [![已实现](!/tags/implemented.svg)](CLAUDE.md) [自动 Memory](https://code.claude.com/docs/en/memory) · [自动 Memory 深度解析](reports/claude-agent-memory.md) · [规则](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| [**Checkpointing**](https://code.claude.com/docs/en/checkpointing) | 自动（文件编辑跟踪） |  |
| [**CLI 启动标志**](https://code.claude.com/docs/en/cli-reference) | `claude [flags]` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-cli-startup-flags.md) [交互模式](https://code.claude.com/docs/en/interactive-mode) · [环境变量](https://code.claude.com/docs/en/env-vars) |
| **AI 术语** | | [![最佳实践](!/tags/best-practice.svg)](https://github.com/shanraisshan/claude-code-codex-cursor-gemini/blob/main/reports/ai-terms.md) |
| [**最佳实践**](https://code.claude.com/docs/en/best-practices) | | [提示工程](https://github.com/anthropics/prompt-eng-interactive-tutorial) · [扩展 Claude Code](https://code.claude.com/docs/en/features-overview) |

### 🔥 热门

| 功能 | 位置 | 描述 |
|---------|----------|-------------|
| [**Ultrareview**](https://code.claude.com/docs/en/ultrareview) ![beta](!/tags/beta.svg) | `/ultrareview`, `claude ultrareview [target]` | [任务跟踪](https://code.claude.com/docs/en/ultrareview#track-a-running-review) |
| [**Devcontainers**](https://code.claude.com/docs/en/devcontainer) | `.devcontainer/` |  |
| [**Channels**](https://code.claude.com/docs/en/channels) ![beta](!/tags/beta.svg) | `--channels`, 基于 plugin | [参考](https://code.claude.com/docs/en/channels-reference) |
| [**Ultraplan**](https://code.claude.com/docs/en/ultraplan) ![beta](!/tags/beta.svg) | `/ultraplan` |  |
| [**无闪烁模式**](https://code.claude.com/docs/en/fullscreen) ![beta](!/tags/beta.svg) | `/tui fullscreen`, `CLAUDE_CODE_NO_FLICKER=1` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2039421575422980329) |
| [**Auto 模式**](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) ![beta](!/tags/beta.svg) | `--permission-mode auto`, `Shift+Tab` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/claudeai/status/2036503582166393240) [博客](https://claude.com/blog/auto-mode) |
| [**Power-ups**](best-practice/claude-power-ups.md) | `/powerup` | [![最佳实践](!/tags/best-practice.svg)](best-practice/claude-power-ups.md) |
| [**快速模式**](https://code.claude.com/docs/en/fast-mode) ![beta](!/tags/beta.svg) | `/fast`, `"fastMode": true` |  |
| [**Computer Use**](https://code.claude.com/docs/en/computer-use) ![beta](!/tags/beta.svg) | `computer-use` MCP server | [桌面](https://code.claude.com/docs/en/desktop#let-claude-use-your-computer) |
| [**Agent SDK**](https://code.claude.com/docs/en/agent-sdk/overview) | `npm` / `pip` package | [快速入门](https://code.claude.com/docs/en/agent-sdk/quickstart) · [示例](https://github.com/anthropics/claude-agent-sdk-demos) |
| [**Ralph Wiggum 循环**](https://github.com/anthropics/claude-code/tree/main/plugins/ralph-wiggum) | plugin | [![最佳实践](!/tags/best-practice.svg)](https://github.com/ghuntley/how-to-ralph-wiggum) [![已实现](!/tags/implemented.svg)](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop) |
| [**Chrome**](https://code.claude.com/docs/en/chrome) ![beta](!/tags/beta.svg) | `--chrome`, extension | [![最佳实践](!/tags/best-practice.svg)](reports/claude-in-chrome-v-chrome-devtools-mcp.md) |
| [**Claude Code Web**](https://code.claude.com/docs/en/claude-code-on-the-web) ![beta](!/tags/beta.svg) | `claude.ai/code` | [Routines](https://code.claude.com/docs/en/routines) |
| [**Slack**](https://code.claude.com/docs/en/slack) | `@Claude` in Slack |  |
| [**Code Review**](https://code.claude.com/docs/en/code-review) ![beta](!/tags/beta.svg) | GitHub App (托管) | [![最佳实践](!/tags/best-practice.svg)](https://x.com/claudeai/status/2031088171262554195) [博客](https://claude.com/blog/code-review) |
| [**GitHub Actions**](https://code.claude.com/docs/en/github-actions) | `.github/workflows/` | [GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd) |
| [**远程控制**](https://code.claude.com/docs/en/remote-control) | `/remote-control`, `/rc` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/noahzweben/status/2032533699116355819) [无头模式](https://code.claude.com/docs/en/headless) |
| [**Deep Links**](https://code.claude.com/docs/en/deep-links) | `claude-cli://open?repo=…&q=…` |  |
| [**Agent Teams**](https://code.claude.com/docs/en/agent-teams) ![beta](!/tags/beta.svg) | 内置（环境变量） | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2019472394696683904) [![已实现](!/tags/implemented.svg)](implementation/claude-agent-teams-implementation.md) |
| [**Agent View**](https://code.claude.com/docs/en/agent-view) ![beta](!/tags/beta.svg) | `claude agents`, `--bg`, `/bg` |  |
| [**定时任务**](https://code.claude.com/docs/en/scheduled-tasks) | `/loop`, `/schedule`, cron 工具 | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2030193932404150413) [![已实现](!/tags/implemented.svg)](implementation/claude-scheduled-tasks-implementation.md) [桌面定时任务](https://code.claude.com/docs/en/desktop-scheduled-tasks) · [公告](https://x.com/noahzweben/status/2036129220959805859) |
| [**Routines**](https://code.claude.com/docs/en/routines) ![beta](!/tags/beta.svg) | `claude.ai/code/routines`, `/schedule` | [桌面任务](https://code.claude.com/docs/en/desktop-scheduled-tasks) |
| [**Tasks**](reports/claude-global-vs-project-settings.md#tasks-system) | `/tasks`, `~/.claude/tasks/` | [![最佳实践](!/tags/best-practice.svg)](reports/claude-global-vs-project-settings.md) [Ultrareview 跟踪](https://code.claude.com/docs/en/ultrareview#track-a-running-review) |
| [**Goal**](https://code.claude.com/docs/en/goal) | `/goal <condition>`, `/goal clear` | [![已实现](!/tags/implemented.svg)](implementation/claude-goal-implementation.md) |
| [**语音听写**](https://code.claude.com/docs/en/voice-dictation) ![beta](!/tags/beta.svg) | `/voice` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/trq212/status/2028628570692890800) |
| [**Simplify & Batch**](https://code.claude.com/docs/en/skills#bundled-skills) | `/simplify`, `/batch` | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2027534984534544489) |
| [**Git Worktrees**](https://code.claude.com/docs/en/worktrees) | `--worktree`/`-w`, `.worktreeinclude`, `EnterWorktree`/`ExitWorktree`, `isolation: "worktree"`, `WorktreeCreate`/`WorktreeRemove` hooks | [![最佳实践](!/tags/best-practice.svg)](https://x.com/bcherny/status/2025007393290272904) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

<a id="orchestration-workflow"></a>

## <a href="orchestration-workflow/orchestration-workflow.md"><img src="!/tags/orchestration-workflow-hd.svg" alt="编排工作流"></a>

查看 [orchestration-workflow](orchestration-workflow/orchestration-workflow.md) 了解 <img src="!/tags/c.svg" height="14"> **Command** → <img src="!/tags/a.svg" height="14"> **Agent** → <img src="!/tags/s.svg" height="14"> **Skill** 模式的实现细节。


<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.svg" alt="Command Skill Agent 架构流程图" width="100%">
</p>

<p align="center">
  <img src="orchestration-workflow/orchestration-workflow.gif" alt="编排工作流演示" width="600">
</p>

![如何使用](!/tags/how-to-use.svg)

```bash
claude
/weather-orchestrator
```

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## ⚙️ 开发工作流

所有主要工作流都遵循相同的架构模式：**研究 → 规划 → 执行 → 审查 → 发布**

| 名称 | ★ | 工作流 | <img src="!/tags/a.svg" height="14"> | <img src="!/tags/c.svg" height="14"> | <img src="!/tags/s.svg" height="14"> |
|------|---|----------|---|---|---|
| [Superpowers](https://github.com/obra/superpowers) | 200k | <img src="https://img.shields.io/badge/brainstorming-ddf4ff" alt="brainstorming" align="middle"> → <img src="https://img.shields.io/badge/using--git--worktrees-ddf4ff" alt="using-git-worktrees" align="middle"> → <img src="https://img.shields.io/badge/writing--plans-ddf4ff" alt="writing-plans" align="middle"> → <img src="https://img.shields.io/badge/subagent--driven--development-fff3b0" alt="subagent-driven-development" align="middle"> → <img src="https://img.shields.io/badge/test--driven--development-fff3b0" alt="test-driven-development" align="middle"> → <img src="https://img.shields.io/badge/requesting--code--review-fff3b0" alt="requesting-code-review" align="middle"> → <img src="https://img.shields.io/badge/verification--before--completion-fff3b0" alt="verification-before-completion" align="middle"> → <img src="https://img.shields.io/badge/finishing--a--development--branch-ddf4ff" alt="finishing-a-development-branch" align="middle"> | 0 | 0 | 14 |
| [Everything Claude Code](https://github.com/affaan-m/everything-claude-code) | 188k | <img src="https://img.shields.io/badge/%2Fecc:plan-ddf4ff" alt="/ecc:plan" align="middle"> → <img src="https://img.shields.io/badge/%2Ftdd-ddf4ff" alt="/tdd" align="middle"> → <img src="https://img.shields.io/badge/%2Fcode--review-ddf4ff" alt="/code-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fsecurity--scan-ddf4ff" alt="/security-scan" align="middle"> → <img src="https://img.shields.io/badge/%2Fe2e-ddf4ff" alt="/e2e" align="middle"> → <img src="https://img.shields.io/badge/merge-ddf4ff" alt="merge" align="middle"> | 48 | 143 | 230 |
| [Spec Kit](https://github.com/github/spec-kit) | 104k | <img src="https://img.shields.io/badge/%2Fspeckit.constitution-ddf4ff" alt="/speckit.constitution" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.specify-ddf4ff" alt="/speckit.specify" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.clarify-ddf4ff" alt="/speckit.clarify" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.plan-ddf4ff" alt="/speckit.plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.tasks-ddf4ff" alt="/speckit.tasks" align="middle"> → <img src="https://img.shields.io/badge/%2Fspeckit.implement-ddf4ff" alt="/speckit.implement" align="middle"> | 0 | 9 | 0 |
| [gstack](https://github.com/garrytan/gstack) | 100k | <img src="https://img.shields.io/badge/%2Foffice--hours-ddf4ff" alt="/office-hours" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--ceo--review-ddf4ff" alt="/plan-ceo-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--eng--review-ddf4ff" alt="/plan-eng-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan--design--review-ddf4ff" alt="/plan-design-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fdesign--shotgun-ddf4ff" alt="/design-shotgun" align="middle"> → <img src="https://img.shields.io/badge/%2Fdesign--html-ddf4ff" alt="/design-html" align="middle"> → <img src="https://img.shields.io/badge/%2Freview-ddf4ff" alt="/review" align="middle"> → <img src="https://img.shields.io/badge/%2Fcodex-ddf4ff" alt="/codex" align="middle"> → <img src="https://img.shields.io/badge/%2Fqa-ddf4ff" alt="/qa" align="middle"> → <img src="https://img.shields.io/badge/%2Fship-ddf4ff" alt="/ship" align="middle"> → <img src="https://img.shields.io/badge/%2Fland--and--deploy-ddf4ff" alt="/land-and-deploy" align="middle"> → <img src="https://img.shields.io/badge/%2Fretro-ddf4ff" alt="/retro" align="middle"> | 0 | 0 | 48 |
| [Matt Pocock Skills](https://github.com/mattpocock/skills) | 97k | <img src="https://img.shields.io/badge/%2Fgrill--with--docs-ddf4ff" alt="/grill-with-docs" align="middle"> → <img src="https://img.shields.io/badge/%2Fto--prd-ddf4ff" alt="/to-prd" align="middle"> → <img src="https://img.shields.io/badge/%2Fto--issues-ddf4ff" alt="/to-issues" align="middle"> → <img src="https://img.shields.io/badge/%2Ftriage-ddf4ff" alt="/triage" align="middle"> → <img src="https://img.shields.io/badge/%2Ftdd-fff3b0" alt="/tdd" align="middle"> → <img src="https://img.shields.io/badge/%2Fdiagnose-fff3b0" alt="/diagnose" align="middle"> → <img src="https://img.shields.io/badge/%2Fimprove--codebase--architecture-ddf4ff" alt="/improve-codebase-architecture" align="middle"> → <img src="https://img.shields.io/badge/%2Fzoom--out-ddf4ff" alt="/zoom-out" align="middle"> | 0 | 0 | 28 |
| [Get Shit Done](https://github.com/gsd-build/get-shit-done) | 63k | <img src="https://img.shields.io/badge/%2Fgsd--new--project-ddf4ff" alt="/gsd-new-project" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--discuss--phase-ddf4ff" alt="/gsd-discuss-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--plan--phase-ddf4ff" alt="/gsd-plan-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--execute--phase-ddf4ff" alt="/gsd-execute-phase" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--verify--work-fff3b0" alt="/gsd-verify-work" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--ship-ddf4ff" alt="/gsd-ship" align="middle"> → <img src="https://img.shields.io/badge/%2Fgsd--complete--milestone-ddf4ff" alt="/gsd-complete-milestone" align="middle"> | 33 | 67 | 0 |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | 50k | <img src="https://img.shields.io/badge/%2Fopsx:propose-ddf4ff" alt="/opsx:propose" align="middle"> → <img src="https://img.shields.io/badge/%2Fopsx:apply-ddf4ff" alt="/opsx:apply" align="middle"> → <img src="https://img.shields.io/badge/%2Fopsx:archive-ddf4ff" alt="/opsx:archive" align="middle"> | 0 | 11 | 0 |
| [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | 48k | <img src="https://img.shields.io/badge/bmad--product--brief-ddf4ff" alt="bmad-product-brief" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--prd-ddf4ff" alt="bmad-create-prd" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--architecture-ddf4ff" alt="bmad-create-architecture" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--epics--and--stories-ddf4ff" alt="bmad-create-epics-and-stories" align="middle"> → <img src="https://img.shields.io/badge/bmad--sprint--planning-ddf4ff" alt="bmad-sprint-planning" align="middle"> → <img src="https://img.shields.io/badge/bmad--create--story-fff3b0" alt="bmad-create-story" align="middle"> → <img src="https://img.shields.io/badge/bmad--dev--story-fff3b0" alt="bmad-dev-story" align="middle"> → <img src="https://img.shields.io/badge/bmad--code--review-fff3b0" alt="bmad-code-review" align="middle"> → <img src="https://img.shields.io/badge/bmad--retrospective-ddf4ff" alt="bmad-retrospective" align="middle"> | 0 | 0 | 42 |
| [oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) | 34k | <img src="https://img.shields.io/badge/%2Fdeep--interview-ddf4ff" alt="/deep-interview" align="middle"> → <img src="https://img.shields.io/badge/%2Fteam-ddf4ff" alt="/team" align="middle"> → <img src="https://img.shields.io/badge/team--plan-fff3b0" alt="team-plan" align="middle"> → <img src="https://img.shields.io/badge/team--prd-fff3b0" alt="team-prd" align="middle"> → <img src="https://img.shields.io/badge/team--exec-fff3b0" alt="team-exec" align="middle"> → <img src="https://img.shields.io/badge/team--verify-fff3b0" alt="team-verify" align="middle"> → <img src="https://img.shields.io/badge/team--fix-fff3b0" alt="team-fix" align="middle"> → <img src="https://img.shields.io/badge/%2Fralph-ddf4ff" alt="/ralph" align="middle"> → <img src="https://img.shields.io/badge/merge-ddf4ff" alt="merge" align="middle"> | 19 | 0 | 39 |
| [agent-skills](https://github.com/addyosmani/agent-skills) | 27k | <img src="https://img.shields.io/badge/%2Fspec-ddf4ff" alt="/spec" align="middle"> → <img src="https://img.shields.io/badge/%2Fplan-ddf4ff" alt="/plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fbuild-ddf4ff" alt="/build" align="middle"> → <img src="https://img.shields.io/badge/%2Ftest-ddf4ff" alt="/test" align="middle"> → <img src="https://img.shields.io/badge/%2Freview-ddf4ff" alt="/review" align="middle"> → <img src="https://img.shields.io/badge/%2Fship-ddf4ff" alt="/ship" align="middle"> | 3 | 7 | 21 |
| [Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin) | 17k | <img src="https://img.shields.io/badge/%2Fce--ideate-ddf4ff" alt="/ce-ideate" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--brainstorm-ddf4ff" alt="/ce-brainstorm" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--plan-ddf4ff" alt="/ce-plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--work-ddf4ff" alt="/ce-work" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--code--review-ddf4ff" alt="/ce-code-review" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--debug-fff3b0" alt="/ce-debug" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--optimize-fff3b0" alt="/ce-optimize" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--compound-ddf4ff" alt="/ce-compound" align="middle"> → <img src="https://img.shields.io/badge/%2Fce--compound--refresh-fff3b0" alt="/ce-compound-refresh" align="middle"> | 49 | 4 | 38 |
| [HumanLayer](https://github.com/humanlayer/humanlayer) | 11k | <img src="https://img.shields.io/badge/%2Fcreate__plan-ddf4ff" alt="/create_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fvalidate__plan-ddf4ff" alt="/validate_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fimplement__plan-ddf4ff" alt="/implement_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Fiterate__plan-fff3b0" alt="/iterate_plan" align="middle"> → <img src="https://img.shields.io/badge/%2Flocal__review-ddf4ff" alt="/local_review" align="middle"> → <img src="https://img.shields.io/badge/%2Fcommit-ddf4ff" alt="/commit" align="middle"> | 6 | 27 | 0 |

> *注意：黄色标签为子循环——在父步骤内重复执行的步骤（例如，每个任务、每个故事或直到验证条件通过）。*

### 其他
- [RPI](development-workflows/rpi/rpi-workflow.md) [![已实现](!/tags/implemented.svg)](development-workflows/rpi/rpi-workflow.md)
- [Ralph Wiggum 循环](https://www.youtube.com/watch?v=eAtvoGlpeRU) [![已实现](!/tags/implemented.svg)](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop)
- [Andrej Karpathy（OpenAI 创始成员）工作流](https://x.com/karpathy/status/2015883857489522876)
- [Peter Steinberger（OpenClaw 创建者）工作流](https://youtu.be/8lF7HmQ_RgY?t=2582)
- Boris Cherny（Claude Code 创建者）工作流 — [13 条提示](tips/claude-boris-13-tips-03-jan-26.md) · [10 条提示](tips/claude-boris-10-tips-01-feb-26.md) · [12 条提示](tips/claude-boris-12-tips-12-feb-26.md) · [2 条提示](tips/claude-boris-2-tips-25-mar-26.md) · [15 条提示](tips/claude-boris-15-tips-30-mar-26.md) · [6 条提示](tips/claude-boris-6-tips-16-apr-26.md) [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny)
- Thariq（Anthropic）工作流 — [Skills](tips/claude-thariq-tips-17-mar-26.md) · [会话管理](tips/claude-thariq-tips-16-apr-26.md) [![Thariq](!/tags/thariq.svg)](https://x.com/trq212)

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 🔀 跨模型工作流

将 Claude Code 与其他模型一起使用——Codex、Gemini、GPT、Kimi、DeepSeek、本地模型——通过三种机制：

- **Plugin**——另一个模型的 CLI 在 Claude Code 内运行（斜杠命令如 `/codex:review`）
- **MCP**——Claude Code 通过 Model Context Protocol 将另一个模型作为工具调用
- **Router**——Claude Code 的 API 端点被切换为不同的提供商

方法论：[跨模型（Claude Code + Codex）工作流](development-workflows/cross-model-workflow/cross-model-workflow.md) [![已实现](!/tags/implemented.svg)](development-workflows/cross-model-workflow/cross-model-workflow.md) —— 手动双终端流程，在 Claude 中进行规划，在 Codex 中进行 QA 审查。

| 名称 | ★ | 类型 | 桥接到 | 功能 |
|------|---|------|------------|--------------|
| [musistudio/claude-code-router](https://github.com/musistudio/claude-code-router) | 34k | Router | OpenRouter, DeepSeek, Ollama, Gemini, Kimi, Qwen, Groq, +更多 | 将 Claude Code 的 API 路由到任何兼容的提供商，支持按任务选择模型 |
| [router-for-me/CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI) | 32k | Router | Gemini CLI, Codex, Claude Code, Antigravity | 将每个 CLI 封装为 OpenAI/Gemini/Claude/Codex 兼容的 API 服务 |
| [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc) | 18k | Plugin | Codex / GPT-5 | OpenAI 官方 plugin：在 Claude Code 内使用 `/codex:review`、`/codex:adversarial-review`、`/codex:rescue` |
| [BeehiveInnovations/pal-mcp-server](https://github.com/BeehiveInnovations/pal-mcp-server) | 12k | MCP | Gemini, OpenAI, Azure, Grok, Ollama, OpenRouter（50+ 模型） | 多模型 MCP 服务器（原名 `zen-mcp-server`）——将其他模型作为 Claude 工具调用 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 🧰 Skill 集合

主要以 `SKILL.md` 文件的精选库形式著称的仓库（与上面完整的工作流方法论不同）。按 Stars 数降序排列。

| 名称 | ★ | <img src="!/tags/s.svg" height="14"> |
|------|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) | 138k | 17 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | 97k | 24 |
| [wshobson/agents](https://github.com/wshobson/agents) | 36k | 155 |
| [impeccable](https://github.com/pbakaus/impeccable) | 27k | 1（含 7 个设计领域参考） |
| [agent-skills](https://github.com/addyosmani/agent-skills) | 27k | 21 |
| [scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills) | 25k | 138 |
| [awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 22k | 1,100+（精选列表） |
| [claude-skills](https://github.com/alirezarezvani/claude-skills) | 15k | 246（涵盖 9 个领域） |

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 🤖 Agent 集合

主要以 subagent 定义（`.claude/agents/*.md`）的精选库形式著称的仓库。按 Stars 数降序排列。

| 名称 | ★ | <img src="!/tags/a.svg" height="14"> |
|------|---|---|
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | 104k | 144 |
| [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) | 20k | 151 |

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 💡 提示与技巧（83 条）

🚫👶 = 无需全程监督

[提示](#tips-prompting) · [规划](#tips-planning) · [上下文](#tips-context) · [会话](#tips-session) · [CLAUDE.md + .claude/rules](#tips-claudemd) · [Agents](#tips-agents) · [Commands](#tips-commands) · [Skills](#tips-skills) · [Hooks](#tips-hooks) · [工作流](#tips-workflows) · [高级](#tips-workflows-advanced) · [Git / PR](#tips-git-pr) · [调试](#tips-debugging) · [工具](#tips-utilities) · [日常](#tips-daily)

![社区](!/tags/community.svg)

<a id="tips-prompting"></a>■ **提示（3 条）**

| 提示 | 来源 |
|-----|--------|
| 挑战 Claude——"用这些变更考考我，直到我通过你的测试才让我创建 PR。"或者"向我证明这能行"，让 Claude 比较 main 分支和你当前分支的差异。🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 在平庸的修复之后——"基于你现在知道的一切，放弃这个，实现优雅的解决方案" 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| Claude 能自己修复大部分 bug——粘贴 bug 然后说"修复"，不要微观管理如何修复 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742750473720121) |

<a id="tips-planning"></a>■ **规划/规格（7 条）**

| 提示 | 来源 |
|-----|--------|
| 始终从 [plan 模式](https://code.claude.com/docs/en/common-workflows)开始 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179845336527000) |
| 从一个最小的规格或提示开始，要求 Claude 使用 [AskUserQuestion](https://code.claude.com/docs/en/cli-reference) 工具对你进行访谈，然后新建一个会话执行规格 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2005315275026260309) |
| 始终制定分阶段的门控计划，每个阶段包含多个测试（单元测试、自动化测试、集成测试） | [![Dex](!/tags/community-dex.svg)](videos/claude-dex-mlops-community-24-mar-26.md) [![视频](!/tags/video.svg)](https://youtu.be/YwZR6tc7qYg?t=1032) |
| 将 PRD 分解为垂直切片（示踪子弹），贯穿所有层级（数据库 + 服务 + UI）——AI 默认采用水平分阶段（数据库阶段、API 阶段、前端阶段），这会将端到端反馈延迟到最后一个阶段。来自《程序员修炼之道》🚫👶 | [![Matt](!/tags/community-matt.svg)](videos/claude-matt-pocock-24-apr-26.md) [![视频](!/tags/video.svg)](https://youtu.be/-QFHIoCo-Ko) |
| 启动第二个 Claude 以高级工程师身份审查你的计划，或使用[跨模型](development-workflows/cross-model-workflow/cross-model-workflow.md)进行审查 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742745365057733) |
| 在交接工作之前编写详细的规格并减少歧义——你描述得越具体，输出质量越好 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742752566632544) |
| 原型 > PRD——与其编写规格，不如构建 20-30 个版本，构建成本低，所以多尝试几次 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3630) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3630) |

<a id="tips-context"></a>■ **上下文（5 条）**

| 提示 | 来源 |
|-----|--------|
| 在 1M 上下文模型上，大约在 300-400k token 时会出现上下文退化——对智力敏感的工作不要让会话超过这个限度 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 大约在 40% 上下文时进入"愚蠢区"——"你会到达结果质量下降的临界点"。新手："尽量控制在 40% 以下，如果达到 60%，就考虑收尾了"。有经验者："强烈建议保持在 30% 以下"——仅在简单任务时可以推到 60%。切换任务时手动执行 [/compact](https://code.claude.com/docs/en/interactive-mode) 或 [/clear](https://code.claude.com/docs/en/cli-reference) 重置 | [![Dex](!/tags/community-dex.svg)](videos/claude-dex-mlops-community-24-mar-26.md) [![视频](!/tags/video.svg)](https://youtu.be/YwZR6tc7qYg?t=1541) |
| 回退 > 纠正——按两次 Esc 或使用 [/rewind](https://code.claude.com/docs/en/checkpointing) 回到失败尝试之前的状态，用你学到的新知识重新提示，而不是让失败的尝试和修正污染上下文 🚫👶 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 带提示的 [/compact](https://code.claude.com/docs/en/interactive-mode)（/compact 专注于认证重构，放弃测试调试）比让自动压缩触发更好——模型在自动压缩时处于最不智能的状态，因为上下文已退化 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 使用 subagent 进行上下文管理——问自己"我还会需要这个工具的输出吗，还是只需要结论？"——20 次文件读取 + 12 次 grep + 3 条死胡同都留在子上下文中，只有最终报告返回 🚫👶 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |

<a id="tips-session"></a>■ **会话管理（6 条）**

| 提示 | 来源 |
|-----|--------|
| 每一轮都是一个分支点——在 Claude 结束一轮后，根据你需要携带多少现有上下文选择 Continue、/rewind、/clear、/compact 或 Subagent | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 新任务 = 新会话——相关任务（例如为你刚构建的内容编写文档）可以重用上下文以提高效率，但真正的新任务值得一个全新的会话 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 在回退前使用"summarize from here"让 Claude 编写一条交接消息——就像来自未来的 Claude 给当前迭代的 Claude 的便条 | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| /compact 与 /clear 对比——compact 有信息丢失但有利于维持工作节奏（任务中间，模糊细节是可以接受的）；/clear + 简短描述需要更多工作，但你可以精确控制携带什么（高风险的下一步） | [![Thariq](!/tags/thariq.svg)](tips/claude-thariq-tips-16-apr-26.md) |
| 对于长时间运行的会话，使用 recap——Claude 做了什么以及下一步做什么的简短摘要，在几分钟或几小时后返回时很有用。在 /config 中使用 recaps 禁用 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| [/rename](https://code.claude.com/docs/en/cli-reference) 重要会话（例如 [TODO - 重构任务]）并在之后 [/resume](https://code.claude.com/docs/en/cli-reference) 它们——同时运行多个 Claude 时为每个实例打标签 | [![Cat](!/tags/cat-wu.svg)](https://every.to/podcast/how-to-use-claude-code-like-the-people-who-built-it) |

<a id="tips-claudemd"></a>■ **CLAUDE.md + .claude/rules（8 条）**

| 提示 | 来源 |
|-----|--------|
| [CLAUDE.md](https://code.claude.com/docs/en/memory) 应控制在每个文件 [200 行以下](https://code.claude.com/docs/en/memory#write-effective-instructions)。[humanlayer 中为 60 行](https://www.humanlayer.dev/blog/writing-a-good-claude-md)（[仍然不是 100% 保证](https://www.reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/)） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179840848597422) [![Dex](!/tags/community-dex.svg)](https://www.humanlayer.dev/blog/writing-a-good-claude-md) |
| .claude/rules/*.md 自动加载到每个会话中，就像 CLAUDE.md 一样——添加 `paths:` YAML frontmatter，仅在 Claude 触及匹配 glob 的文件时延迟加载它们 | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| 将领域特定的 CLAUDE.md 规则包裹在 [\<important if="..."\> 标签](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md)中，以防止 Claude 在文件变长时忽略它们 | [![Dex](!/tags/community-dex.svg)](https://www.hlyr.dev/blog/stop-claude-from-ignoring-your-claude-md) |
| 为单体仓库使用[多个 CLAUDE.md](best-practice/claude-memory.md)——祖先和后代加载 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2016339448863355206) |
| 使用 [.claude/rules/](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) 拆分大型指令 | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/memory#organize-rules-with-clauderules) |
| 任何开发者都应该能够启动 Claude，说"运行测试"，第一次就能成功——如果不能，你的 CLAUDE.md 缺少必要的设置/构建/测试命令 | [![Dex](!/tags/community-dex.svg)](https://x.com/dexhorthy/status/2034713765401551053) |
| 保持代码库整洁，完成迁移——部分迁移的框架会混淆模型，可能选择错误的模式 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=1112) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=1112) |
| 使用 [settings.json](best-practice/claude-settings.md) 实现由 harness 强制实施的行为（归属、权限、模型）——当 `attribution.commit: ""` 是确定性的时，不要将"永远不要添加 Co-Authored-By"放在 CLAUDE.md 中 | [![davila7](!/tags/community-davila7.svg)](https://x.com/dani_avila7/status/2036182734310195550) |

<a id="tips-agents"></a><img src="!/tags/a.svg" height="14"> **Agents（4 条）**

| 提示 | 来源 |
|-----|--------|
| 创建具有[技能](https://code.claude.com/docs/en/skills)（渐进式披露）的功能特定 [sub-agent](https://code.claude.com/docs/en/sub-agents)（额外上下文），而不是通用的 QA 或后端工程师 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179850139000872) |
| 说"使用 subagent"可以为问题投入更多计算能力——卸载任务以保持主上下文干净和专注 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| [agent teams with tmux](https://code.claude.com/docs/en/agent-teams) 和 [git worktrees](https://x.com/bcherny/status/2025007393290272904) 用于并行开发 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2025007393290272904) |
| 使用 [test time compute](https://code.claude.com/docs/en/sub-agents) ——分离的上下文窗口使结果更好；一个 agent 可能引入 bug，而另一个（相同模型）可以找到它们 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031151689219321886) |

<a id="tips-commands"></a><img src="!/tags/c.svg" height="14"> **Commands（3 条）**

| 提示 | 来源 |
|-----|--------|
| 使用 [commands](https://code.claude.com/docs/en/slash-commands) 作为工作流，而不是 [sub-agents](https://code.claude.com/docs/en/sub-agents) | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 对每天执行多次的每个"内部循环"工作流使用[斜杠命令](https://code.claude.com/docs/en/slash-commands)——节省重复提示，commands 存放在 `.claude/commands/` 中并提交到 git | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179847949500714) |
| 如果你每天做某事超过一次，将其变成 [skill](https://code.claude.com/docs/en/skills) 或 [command](https://code.claude.com/docs/en/slash-commands)——构建 /techdebt、context-dump 或 analytics 命令 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742748984742078) |

<a id="tips-skills"></a><img src="!/tags/s.svg" height="14"> **Skills（9 条）**

| 提示 | 来源 |
|-----|--------|
| 使用 [context: fork](https://code.claude.com/docs/en/skills) 在隔离的 subagent 中运行 skill——主上下文只看到最终结果，而不是中间工具调用。agent 字段让你设置 subagent 类型 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2033603164398883042) |
| 为单体仓库使用[子文件夹中的 skills](reports/claude-skills-for-larger-mono-repos.md) | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/skills) |
| skills 是文件夹，而不是文件——使用 references/、scripts/、examples/ 子目录实现[渐进式披露](https://code.claude.com/docs/en/skills) | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在每个 skill 中构建 Gotchas（隐患）部分——最高信号量的内容，随时间累积 Claude 的失败点 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| skill 的 description 字段是一个触发器，而不是摘要——为模型编写（"我什么时候应该触发？"） | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在 skills 中说显而易见的事情——专注于推动 Claude 超出其默认行为的内容 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 不要在 skills 中限制 Claude 的发挥空间——给出目标和约束，而不是规定性的逐步说明 🚫👶 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在 skills 中包含脚本和库，以便 Claude 进行组合而不是重新构造样板代码 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 在 SKILL.md 中嵌入 !command 以将动态 shell 输出注入提示——Claude 在调用时运行它，模型只看到结果 | [![Lydia](!/tags/lydia.svg)](https://x.com/lydiahallie/status/2034337963820327017) |

<a id="tips-hooks"></a>■ **Hooks（5 条）**

| 提示 | 来源 |
|-----|--------|
| 在 skills 中使用[按需 hooks](https://code.claude.com/docs/en/skills)——/careful 阻止破坏性命令，/freeze 阻止目录外的编辑 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 PreToolUse hook [衡量 skill 使用情况](https://code.claude.com/docs/en/skills)，以发现热门或触发不足的 skills | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 [PostToolUse hook](https://code.claude.com/docs/en/hooks) 自动格式化代码——Claude 生成格式良好的代码，hook 处理最后 10% 以避免 CI 失败 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179852047335529) |
| 通过 hook 将[权限请求](https://code.claude.com/docs/en/hooks)路由到 Opus——让它扫描攻击并自动批准安全的请求 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742755737555434) |
| 使用 [Stop hook](https://code.claude.com/docs/en/hooks) 在一轮结束时推动 Claude 继续工作或验证其工作 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701059253874861) |

<a id="tips-workflows"></a>■ **工作流（5 条）**

| 提示 | 来源 |
|-----|--------|
| 使用 [/model](https://code.claude.com/docs/en/model-config) 选择模型和推理方式，[/context](https://code.claude.com/docs/en/interactive-mode) 查看上下文使用情况，[/usage](https://code.claude.com/docs/en/costs) 检查计划限制，[/extra-usage](https://code.claude.com/docs/en/interactive-mode) 配置超额计费，[/config](https://code.claude.com/docs/en/settings) 配置设置——在 plan 模式使用 Opus，在编写代码时使用 Sonnet，以获得两方面的最佳效果 | [![Cat](!/tags/cat-wu.svg)](https://x.com/_catwu/status/1955694117264261609) |
| 始终在 [/config](https://code.claude.com/docs/en/settings) 中启用 [thinking 模式](https://code.claude.com/docs/en/model-config)（查看推理过程）和 [Output Style](https://code.claude.com/docs/en/output-styles) 为 Explanatory（查看带有 ★ Insight 框的详细输出），以便更好地理解 Claude 的决策 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179838864666847) |
| 在提示中使用 ultrathink 关键字进行[高努力推理](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#tips-and-best-practices) | [![Claude](!/tags/claude.svg)](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#tips-and-best-practices) |
| /focus 模式隐藏所有中间工作，只显示最终结果——信任模型运行正确的命令，只看结果（使用 /focus 切换） | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| 使用 Opus 4.7 的自适应思考调整努力水平——low 可提高速度并减少 token，max 可获得最强智能（滑块：low · medium · high · xhigh · max） | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |

<a id="tips-workflows-advanced"></a>■ **高级工作流（9 条）**

| 提示 | 来源 |
|-----|--------|
| 大量使用 ASCII 图表来理解你的架构 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742759218794768) |
| 使用 [/loop](https://code.claude.com/docs/en/scheduled-tasks) 进行本地循环监控（最多 7 天）· 使用 [/schedule](https://code.claude.com/docs/en/routines) 进行基于云的循环任务，即使你的电脑关机也能运行 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038454341884154269) |
| 使用 [Ralph Wiggum plugin](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop) 进行长时间运行的自主任务 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179858435281082) |
| 使用带通配符语法的 [/permissions](https://code.claude.com/docs/en/permissions)（Bash(npm run *)，Edit(/docs/**)）而不是 dangerously-skip-permissions | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2007179854077407667) |
| [/sandbox](https://code.claude.com/docs/en/sandboxing) 通过文件和网络隔离减少权限提示——内部减少 84% | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700506465579443) [![Cat](!/tags/cat-wu.svg)](https://creatoreconomy.so/p/inside-claude-code-how-an-ai-native-actually-works-cat-wu) |
| 投资打造[产品验证](https://code.claude.com/docs/en/skills) skill（signup-flow-driver、checkout-verifier）——值得花一周时间完善 | [![Thariq](!/tags/thariq.svg)](https://x.com/trq212/status/2033949937936085378) |
| 使用 [auto 模式](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) 而不是 dangerously-skip-permissions——基于模型的分类器判断每个命令是否安全并自动批准，有风险时暂停并询问。Shift+Tab 循环切换 Ask → Plan → Auto 模式 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| 使用 /less-permission-prompts skill 扫描会话历史中重复提示的安全 bash/MCP 命令，然后获取推荐的允许列表粘贴到 [settings](best-practice/claude-settings.md) 中 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |
| 构建一个 /go skill，它（1）通过 bash/browser/computer use 进行端到端测试（2）运行 /simplify（3）提交 PR——这样当你回来时，知道代码是有效的 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](tips/claude-boris-6-tips-16-apr-26.md) |

<a id="tips-git-pr"></a>■ **Git / PR（5 条）**

| 提示 | 来源 |
|-----|--------|
| 保持 PR 小而专注——[p50 为 118 行](tips/claude-boris-2-tips-25-mar-26.md)（141 个 PR，一天内改动 45K 行），每个 PR 一个功能，更容易审查和回滚 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 始终 [squash merge](tips/claude-boris-2-tips-25-mar-26.md) PR——干净的线性历史，每个功能一个 commit，易于 git revert 和 git bisect | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038552880018538749) |
| 频繁 commit——尽量至少每小时 commit 一次，任务完成后立即 commit | ![Shayan](!/tags/community-shayan.svg) |
| 在同事的 PR 上 @[claude](https://github.com/apps/claude) 以自动生成针对重复审查反馈的 lint 规则——把自己从代码审查中解放出来 🚫👶 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=2715) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=2715) |
| 使用 [/code-review](https://code.claude.com/docs/en/code-review) 进行多 agent PR 分析——在合并前捕获 bug、安全漏洞和回归问题 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2031089411820228645) |

<a id="tips-debugging"></a>■ **调试（6 条）**

| 提示 | 来源 |
|-----|--------|
| 养成遇到任何问题就截图并与 Claude 分享的习惯 | ![Shayan](!/tags/community-shayan.svg) |
| 使用 mcp（[Claude in Chrome](https://code.claude.com/docs/en/chrome)、[Playwright](https://github.com/microsoft/playwright-mcp)、[Chrome DevTools](https://developer.chrome.com/blog/chrome-devtools-mcp)）让 Claude 自行查看 Chrome 控制台日志 | [![Claude](!/tags/claude.svg)](https://code.claude.com/docs/en/chrome) |
| 始终要求 Claude 将终端（你想查看日志的）作为后台任务运行，以便更好地调试 | ![Shayan](!/tags/community-shayan.svg) |
| [/doctor](https://code.claude.com/docs/en/cli-reference) 诊断安装、认证和配置问题 | ![Shayan](!/tags/community-shayan.svg) |
| 使用[跨模型](development-workflows/cross-model-workflow/cross-model-workflow.md)进行 QA——例如 [Codex](https://github.com/shanraisshan/codex-cli-best-practice) 用于规划和实现审查 | ![Shayan](!/tags/community-shayan.svg) |
| 基于 agent 的搜索（glob + grep）优于 RAG——Claude Code 尝试并放弃了向量数据库，因为代码会不同步且权限复杂 | [![Boris](!/tags/boris-cherny.svg)](https://youtu.be/julbw1JuAz0?t=3095) [![视频](!/tags/video.svg)](https://youtu.be/julbw1JuAz0?t=3095) |

<a id="tips-utilities"></a>■ **工具（5 条）**

| 提示 | 来源 |
|-----|--------|
| 使用 [iTerm](https://iterm2.com/)/[Ghostty](https://ghostty.org/)/[tmux](https://github.com/tmux/tmux) 终端而不是 IDE（[VS Code](https://code.visualstudio.com/)/[Cursor](https://www.cursor.com/)） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2017742753971769626) |
| [/voice](https://code.claude.com/docs/en/voice-dictation) 或 [Wispr Flow](https://wisprflow.ai) 进行语音提示（10 倍效率） | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2038454362226467112) |
| [claude-code-hooks](https://github.com/shanraisshan/claude-code-hooks) 用于 Claude 反馈 | ![Shayan](!/tags/community-shayan.svg) |
| [状态栏](https://github.com/shanraisshan/claude-code-status-line)用于上下文感知和快速压缩 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021700784019452195) ![Shayan](!/tags/community-shayan.svg) |
| 探索 [settings.json](best-practice/claude-settings.md) 功能，如[计划目录](best-practice/claude-settings.md#plans-directory)、[Spinner 动词](best-practice/claude-settings.md#display--ux)以个性化体验 | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny/status/2021701145023197516) |

<a id="tips-daily"></a>■ **日常（2 条）**

| 提示 | 来源 |
|-----|--------|
| 每天[更新](https://code.claude.com/docs/en/setup) Claude Code | ![Shayan](!/tags/community-shayan.svg) |
| 每天从阅读[更新日志](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)开始 | ![Shayan](!/tags/community-shayan.svg) |

![Boris Cherny + Team](!/tags/claude.svg)

| 文章 / 推文 | 来源 |
|-----------------|--------|
| [从 Opus 4.7 获得更多价值的 6 个技巧（Boris）\| 2026年4月16日](tips/claude-boris-6-tips-16-apr-26.md) | [推文](https://x.com/bcherny) |
| [会话管理与 1M 上下文（Thariq）\| 2026年4月16日](tips/claude-thariq-tips-16-apr-26.md) | [推文](https://x.com/trq212) |
| [Claude Code 中 15 个隐藏且未被充分利用的功能（Boris）\| 2026年3月30日](tips/claude-boris-15-tips-30-mar-26.md) | [推文](https://x.com/bcherny/status/2038454336355999749) |
| [Squash 合并与 PR 大小分布（Boris）\| 2026年3月25日](tips/claude-boris-2-tips-25-mar-26.md) | [推文](https://x.com/bcherny/status/2038552880018538749) |
| [构建 Claude Code 的经验教训：我们如何使用 Skills（Thariq）\| 2026年3月17日](tips/claude-thariq-tips-17-mar-26.md) | [文章](https://x.com/trq212/status/2033949937936085378) |
| [代码审查与测试时间计算（Boris）\| 2026年3月10日](tips/claude-boris-2-tips-10-mar-26.md) | [推文](https://x.com/bcherny/status/2031089411820228645) |
| /loop —— 安排最长 3 天的循环任务（Boris）\| 2026年3月7日 | [推文](https://x.com/bcherny/status/2030193932404150413) |
| AskUserQuestion + ASCII Markdowns（Thariq）\| 2026年2月28日 | [推文](https://x.com/trq212/status/2027543858289250472) |
| 像 Agent 一样思考 —— 构建 Claude Code 的经验教训（Thariq）\| 2026年2月28日 | [文章](https://x.com/trq212/status/2027463795355095314) |
| Git Worktrees —— boris 使用的 5 种方式 \| 2026年2月21日 | [推文](https://x.com/bcherny/status/2025007393290272904) |
| 构建 Claude Code 的经验教训：提示缓存就是一切（Thariq）\| 2026年2月20日 | [文章](https://x.com/trq212/status/2024574133011673516) |
| [人们定制自己 Claude 的 12 种方式（Boris）\| 2026年2月12日](tips/claude-boris-12-tips-12-feb-26.md) | [推文](https://x.com/bcherny/status/2021699851499798911) |
| [团队分享的 10 条 Claude Code 使用技巧（Boris）\| 2026年2月1日](tips/claude-boris-10-tips-01-feb-26.md) | [推文](https://x.com/bcherny/status/2017742741636321619) |
| [我如何使用 Claude Code —— 来自我出奇简单设置的 13 条技巧（Boris）\| 2026年1月3日](tips/claude-boris-13-tips-03-jan-26.md) | [推文](https://x.com/bcherny/status/2007179832300581177) |
| 要求 Claude 使用 AskUserQuestion 工具对你进行访谈（Thariq）\| 2025年12月28日 | [推文](https://x.com/trq212/status/2005315275026260309) |
| 始终使用 plan 模式，给 Claude 验证的方式，使用 /code-review（Boris）\| 2025年12月27日 | [推文](https://x.com/bcherny/status/2004711722926616680) |

#### 来自 Claude Code CLI 二进制的提示

[Spinner 动词与提示（从 CLI 二进制 v2.1.121 中提取）](reports/claude-spinner-verbs-and-tips.md)

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 🎬 视频 / 播客

| 视频 / 播客 | 来源 | YouTube |
|-----------------|--------|--------|
| 从 Vibe Coding 到 Agentic Engineering（Andrej）\| 2026年5月2日 \| AI Engineer | [![Karpathy](!/tags/community-karpathy.svg)](https://x.com/karpathy) | [YouTube](https://www.youtube.com/watch?v=96jN2OCOfLs) |
| 完整演练：AI 编码工作流（Matt）\| 2026年4月24日 \| Matt Pocock | [![Matt](!/tags/community-matt.svg)](https://x.com/mattpocockuk) | [YouTube](https://youtu.be/-QFHIoCo-Ko) |
| 我们在"研究-规划-实现"中犯的所有错误（Dex）\| 2026年3月24日 \| MLOps Community | [![Dex](!/tags/community-dex.svg)](https://x.com/daborhyde) | [YouTube](https://youtu.be/YwZR6tc7qYg) |
| 与 Boris Cherny 一起构建 Claude Code（Boris）\| 2026年3月4日 \| The Pragmatic Engineer | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/julbw1JuAz0) |
| Claude Code 负责人：编码问题解决后会发生什么（Boris）\| 2026年2月19日 \| Lenny's Podcast | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/We7BZVKbCVw) |
| 与其创建者 Boris Cherny 一起探秘 Claude Code（Boris）\| 2026年2月17日 \| Y Combinator | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/PQU9o_5rHC4) |
| Boris Cherny（Claude Code 创建者）谈职业生涯增长（Boris）\| 2025年12月15日 \| Ryan Peterman | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/AmdLVWMdjOk) |
| 从构建它的工程师那里了解 Claude Code 的秘密（Cat）\| 2025年10月29日 \| Every | [![Boris](!/tags/boris-cherny.svg)](https://x.com/bcherny) | [YouTube](https://youtu.be/IDSAMqip6ms) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 🔔 订阅

| 来源 | 名称 | 徽章 |
|--------|------|-------|
| ![Reddit](https://img.shields.io/badge/-FF4500?style=flat&logo=reddit&logoColor=white) | [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/), [r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/), [r/Anthropic](https://www.reddit.com/r/Anthropic/) | ![Boris + Team](!/tags/claude.svg) |
| ![X](https://img.shields.io/badge/-000?style=flat&logo=x&logoColor=white) | [Claude](https://x.com/claudeai), [Claude Devs](https://x.com/ClaudeDevs), [Anthropic](https://x.com/AnthropicAI), [Boris](https://x.com/bcherny), [Thariq](https://x.com/trq212), [Cat](https://x.com/_catwu), [Lydia](https://x.com/lydiahallie), [Noah](https://x.com/noahzweben), [Anthony](https://x.com/amorriscode), [Alex](https://x.com/alexalbert__), [Kenneth](https://x.com/neilhtennek) | ![Boris + Team](!/tags/claude.svg) |
| ![X](https://img.shields.io/badge/-000?style=flat&logo=x&logoColor=white) | [Jesse Kriss](https://x.com/obra) ([Superpowers](https://github.com/obra/superpowers)), [Affaan Mustafa](https://x.com/affaanmustafa) ([ECC](https://github.com/affaan-m/everything-claude-code)), [Garry Tan](https://x.com/garrytan) ([gstack](https://github.com/garrytan/gstack)), [Dex Horthy](https://x.com/dexhorthy) ([HumanLayer](https://github.com/humanlayer/humanlayer)), [Kieran Klaassen](https://x.com/kieranklaassen) ([Compound Eng](https://github.com/EveryInc/compound-engineering-plugin)), [Tabish Gilani](https://x.com/0xTab) ([OpenSpec](https://github.com/Fission-AI/OpenSpec)), [Brian McAdams](https://x.com/BMadCode) ([BMAD](https://github.com/bmad-code-org/BMAD-METHOD)), [Lex Christopherson](https://x.com/official_taches) ([GSD](https://github.com/gsd-build/get-shit-done)), [Matt Pocock](https://x.com/mattpocockuk) ([Skills](https://github.com/mattpocock/skills)), [Dani Avila](https://x.com/dani_avila7) ([CC Templates](https://github.com/davila7/claude-code-templates)), [Dan Shipper](https://x.com/danshipper) ([Every](https://every.to/)), [Andrej Karpathy](https://x.com/karpathy) ([AutoResearch](https://x.com/karpathy/status/2015883857489522876)), [Peter Steinberger](https://x.com/steipete) ([OpenClaw](https://x.com/openclaw)), [Sigrid Jin](https://x.com/realsigridjin) ([claw-code](https://github.com/ultraworkers/claw-code)), [Yeachan Heo](https://x.com/bellman_ych) ([oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode)) | ![社区](!/tags/community.svg) |
| ![YouTube](https://img.shields.io/badge/-F00?style=flat&logo=youtube&logoColor=white) | [Anthropic](https://www.youtube.com/@anthropic-ai) | ![Boris + Team](!/tags/claude.svg) |
| ![YouTube](https://img.shields.io/badge/-F00?style=flat&logo=youtube&logoColor=white) | [Lenny's Podcast](https://www.youtube.com/@LennysPodcast), [Y Combinator](https://www.youtube.com/@ycombinator), [The Pragmatic Engineer](https://www.youtube.com/@pragmaticengineer), [Ryan Peterman](https://www.youtube.com/@ryanlpeterman), [Every](https://www.youtube.com/@every_media), [MLOps Community](https://www.youtube.com/@MLOps) | ![社区](!/tags/community.svg) |

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## ☠️ 创业 / 商业

| Claude | 被取代的 |
|-|-|
|[**Code Review**](https://code.claude.com/docs/en/code-review)|[Greptile](https://greptile.com), [CodeRabbit](https://coderabbit.ai), [Devin Review](https://devin.ai), [OpenDiff](https://opendiff.com), [Cursor BugBot](https://bugbot.dev)|
|[**语音听写**](https://code.claude.com/docs/en/voice-dictation)|[Wispr Flow](https://wisprflow.ai), [SuperWhisper](https://superwhisper.com/)|
|[**远程控制**](https://code.claude.com/docs/en/remote-control)|[OpenClaw](https://openclaw.ai/)
|[**Claude in Chrome**](https://code.claude.com/docs/en/chrome)|[Playwright MCP](https://github.com/microsoft/playwright-mcp), [Chrome DevTools MCP](https://developer.chrome.com/blog/chrome-devtools-mcp)|
|[**Computer Use**](https://docs.anthropic.com/en/docs/agents-and-tools/computer-use)|[OpenAI CUA](https://openai.com/index/computer-using-agent/)|
|[**Cowork**](https://claude.com/blog/cowork-research-preview)|[ChatGPT Agent](https://openai.com/chatgpt/agent/), [Perplexity Computer](https://www.perplexity.ai/computer/), [Manus](https://manus.im)|
|[**Tasks**](https://x.com/trq212/status/2014480496013803643)|[Beads](https://github.com/steveyegge/beads)
|[**Plan 模式**](https://code.claude.com/docs/en/common-workflows)|[Agent OS](https://github.com/buildermethods/agent-os)|
|[**设计**](https://claude.com/design)|[Figma](https://figma.com), [Framer](https://framer.com), [Sketch](https://sketch.com), [v0](https://v0.dev)|
|[**Agent SDK**](https://code.claude.com/docs/en/agent-sdk/overview)|[LangChain](https://langchain.com), [LangGraph](https://www.langchain.com/langgraph), [CrewAI](https://www.crewai.com), [AutoGen](https://github.com/microsoft/autogen), [OpenAI Assistants API](https://platform.openai.com/docs/assistants/overview)|
|[**Skills / Plugins**](https://code.claude.com/docs/en/plugins)|YC AI 封装初创公司 ([reddit](https://reddit.com/r/ClaudeAI/comments/1r6bh4d/claude_code_skills_are_basically_yc_ai_startup/))|

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

<a id="billion-dollar-questions"></a>
![十亿美元问题](!/tags/billion-dollar-questions.svg)

*如果你有答案，请通过 shanraisshan@gmail.com 告诉我*

**内存与指令（4 个）**

1. 到底应该在 CLAUDE.md 中放什么——又应该留下什么？
2. 如果你已经有 CLAUDE.md，是否还需要单独的 constitution.md 或 rules.md？
3. CLAUDE.md 应该多久更新一次，你如何知道它何时已经过时？
4. 为什么 Claude 仍然忽略 CLAUDE.md 中的指令——即使它们用大写字母写着 MUST？([reddit](https://reddit.com/r/ClaudeCode/comments/1qn9pb9/claudemd_says_must_use_agent_claude_ignores_it_80/))

**Agents、Skills 与工作流（6 个）**

1. 什么时候应该使用 command vs agent vs skill——以及什么时候普通的 Claude Code 更好？
2. 随着模型的改进，应该多久更新一次 agents、commands 和工作流？
3. 应该创建一个通才 subagent 还是功能特定/角色特定的 agent？给 subagent 一个详细的角色是否会提高质量，一个"完美的角色提示"用于研究/愿景是什么样的？
4. 应该依赖 Claude Code 内置的 plan 模式——还是构建自己的、强制执行团队工作流的规划 command/agent？
5. 如果你有个人 skill（例如，带有你编码风格的 /implement），如何整合社区 skill（例如 /simplify）而不冲突——当它们不一致时谁获胜？
6. 我们到了吗？能否将现有代码库转换为规格，删除代码，然后让 AI 仅从这些规格中重新生成完全相同的代码？

**规格与文档（3 个）**

1. 仓库中的每个功能都应该有一个 markdown 文件形式的规格吗？
2. 需要多久更新一次规格，以便它们不会在实现新功能时变得过时？
3. 在实现新功能时，如何处理对其他功能规格的连锁影响？

### 🤔 [代码重要吗？](https://github.com/shanraisshan/agentic-engineering)

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## 报告

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
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

<a id="how-to-use"></a>

## <img src="!/tags/how-to-use-hd.svg" alt="如何使用">

通过遵循以下步骤，充分利用本仓库：

1. **将此仓库作为课程阅读，而不是作为工作流或 skill。**它首先是参考资料；之后你才会实际运行。
2. **不要将 Claude 当作聊天机器人使用。**学习基本概念——agents、commands、skills、hooks——并将它们组合成自己的工作流。
3. **运行 [`/weather-orchestrator`](orchestration-workflow/orchestration-workflow.md)** 查看完整的 command → agent → skill 流程。将其作为任何开发工作流（从规划到发布）的模板。
4. **工作时聆听自定义 hook 声音。**它们的实现在专用的 [Claude Code Hooks 仓库](https://github.com/shanraisshan/claude-code-hooks)中；其他模式如 [Agent Teams](implementation/claude-agent-teams-implementation.md) 随本仓库的 `implementation/` 目录一起提供。
5. **从 [🔥 热门](#-hot) 子表格中学习高级主题及其实现**——例如，[Ralph Wiggum 自进化循环](https://github.com/shanraisshan/ralph-wiggum-self-evolving-loop)是一个完整的工作仓库，你可以克隆以端到端地查看这些模式中的一个。
6. **将 Claude 指向你自己的项目中的[提示与技巧](#-tips-and-tricks-83)部分**，并要求它建议编辑——特别是关于如何重构你的 `CLAUDE.md`。每个提示都来源于 Claude 团队或社区。
7. **订阅[订阅部分](#-subscribe)中的 Reddit 和 YouTube 频道**以跟进社区动态。

**🎬 视频**

<a href="https://www.youtube.com/watch?v=AkAhkalkRY4"><img src="!/thumbnail/video-1.png" alt="在 YouTube 上观看" width="240"></a>
<a href="https://youtu.be/lPjhM6BBK0Q"><img src="!/thumbnail/video-2.png" alt="在 YouTube 上观看" width="240"></a>

**📊 演讲**

<a href="https://github.com/shanraisshan/claude-code-best-practice/tree/main/presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini"><img src="!/thumbnail/presentation-1.png" alt="Claude Code & Gemini CLI — GDG Kolachi" width="240"></a>

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

<p align="center">
  <a href="https://github.com/trending?since=monthly"><img src="!/root/github-trending.png" alt="GitHub Trending" width="1200"></a><br>
  ✨2026年3月在 GitHub 上趋势上升✨
</p>

## Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=shanraisshan/claude-code-best-practice&type=Date&v=2)](https://star-history.com/#shanraisshan/claude-code-best-practice&Date)

<a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers"><img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=flat&label=%E2%98%85&labelColor=555&color=white" alt="GitHub Stars" align="center"></a> 颗星，持续增长中

## 其他仓库

<table>
<tr>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/claude-code-hooks"><img src="!/claude-speaking.svg" alt="Claude Code Hooks" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/claude-code-hooks"><strong>Claude Code<br>Hooks</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/codex-cli-best-practice"><img src="!/codex-jumping.svg" alt="Codex CLI Best Practice" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/codex-cli-best-practice"><strong>Codex CLI<br>最佳实践</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/codex-cli-hooks"><img src="!/codex-speaking.svg" alt="Codex CLI Hooks" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/codex-cli-hooks"><strong>Codex CLI<br>Hooks</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/gemini-cli-best-practice"><img src="!/gemini-jumping.svg" alt="Gemini CLI Best Practice" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/gemini-cli-best-practice"><strong>Gemini CLI<br>最佳实践</strong></a>
</td>
<td align="center" width="140">
  <a href="https://github.com/shanraisshan/gemini-cli-hooks"><img src="!/gemini-speaking.svg" alt="Gemini CLI Hooks" width="64" height="64"></a><br>
  <a href="https://github.com/shanraisshan/gemini-cli-hooks"><strong>Gemini CLI<br>Hooks</strong></a>
</td>
</tr>
</table>

## 由谁开发

![由谁开发](!/tags/developed-by.svg)

> | # | 工作流 | 描述 |
> |---|----------|-------------|
> | 1 | /workflows:development-workflows | 通过并行研究所有 10 个工作流仓库，更新 DEVELOPMENT WORKFLOWS 表格和跨工作流分析报告 |
> | 2 | /workflows:skill-collections | 通过并行研究所有 5 个 skill 集合仓库，更新 SKILL COLLECTIONS 表格 |
> | 3 | /workflows:agent-collections | 通过并行研究所有 agent 集合仓库，更新 AGENT COLLECTIONS 表格 |
> | 4 | /workflows:best-practice:workflow-concepts | 使用最新的 Claude Code 功能和概念更新 README CONCEPTS 部分 |
> | 5 | /workflows:best-practice:workflow-claude-settings | 跟踪 Claude Code settings 报告的变化并查找需要更新的内容 |
> | 6 | /workflows:best-practice:workflow-claude-subagents | 跟踪 Claude Code subagents 报告的变化并查找需要更新的内容 |
> | 7 | /workflows:best-practice:workflow-claude-commands | 跟踪 Claude Code commands 报告的变化并查找需要更新的内容 |
> | 8 | /workflows:best-practice:workflow-claude-skills | 跟踪 Claude Code skills 报告的变化并查找需要更新的内容 |

## 额外资源

[![Claude for OSS](!/tags/claude-for-oss.svg)](https://claude.com/contact-sales/claude-for-oss)
[![Claude Community Ambassador](!/tags/claude-community-ambassador.svg)](https://claude.com/community/ambassadors)
[![Claude Certified Architect](!/tags/claude-certified-architect.svg)](https://anthropic.skilljar.com/claude-certified-architect-foundations-access-request)
[![Anthropic Academy](!/tags/anthropic-academy.svg)](https://anthropic.skilljar.com/)
[![加入 Claude Pakistan WhatsApp 社区](!/tags/whatsapp-claude-pakistan.svg)](https://chat.whatsapp.com/BDUV2stIS0c7X5uY7RY6nS)

<p align="center">
  <img src="!/claude-jumping.svg" alt="章节分隔符" width="60" height="50">
</p>

## <img src="!/tags/sponsor-heart.svg" width="22" height="22" align="center"> 赞助我的工作

如果你喜欢我的工作，请我喝杯 doodh patti 🍵

<a href="https://buy.polar.sh/polar_cl_R6wjUESl8RiJD0iVaTyStBUV6WNuYvDmLJ0si1XXj4C"><img src="!/tags/polar.svg" alt="Polar" width="40" height="40" align="center"></a> <a href="https://buy.polar.sh/polar_cl_R6wjUESl8RiJD0iVaTyStBUV6WNuYvDmLJ0si1XXj4C"><strong>Polar</strong></a>
