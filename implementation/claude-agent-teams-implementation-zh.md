# Agent Teams 实现

![最后更新](https://img.shields.io/badge/Last_Updated-Mar_12%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#time-orchestration"><img src="../!/tags/implemented-hd.svg" alt="已实现"></a>

<p align="center">
  <img src="assets/impl-agent-teams.png" alt="Agent Teams 运行效果 — 使用 tmux 的分屏模式" width="100%">
</p>

Agent Teams 会生成**多个独立的 Claude Code 会话**，它们通过共享任务列表进行协调。与 subagent（单个会话内的隔离上下文分支）不同，每个团队成员都有自己的完整上下文窗口，自动加载 CLAUDE.md、MCP 服务器和 skills。

---

## ![如何使用](../!/tags/how-to-use.svg)

时间编排工作流完全由 agent team 构建。运行成品的方法如下：

```bash
cd agent-teams
claude
/time-orchestrator
```

这会调用 **Command → Agent → Skill** 管道：agent 获取迪拜的当前时间，skill 将 SVG 时间卡片渲染到 `agent-teams/output/dubai-time.svg`。

---

## ![如何实现](../!/tags/how-to-implement.svg)

你可以使用 agent teams 创建天气编排工作流的副本——在此示例中，时间编排工作流完全由 agent team 构建。

### 1. 安装 [iTerm2](https://iterm2.com/) 和 tmux

```bash
brew install --cask iterm2
brew install tmux
```

### 2. 启动 iTerm2 → tmux → Claude

```bash
tmux new -s dev
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 claude
```

### 3. 使用团队结构进行提示

<a id="time-orchestration"></a>

将以下提示粘贴到 Claude 中，以使用 agent teams 启动完整的时间编排工作流：

主提示：**[agent-teams-prompt.md](../agent-teams/agent-teams-prompt.md)**

### 团队协调流程

```
┌──────────────────────────────────────────────────────────────┐
│                        主导者（你）                            │
│                  "创建一个 agent team 来构建时间编排"          │
└──────────────────────────┬───────────────────────────────────┘
                           │ 生成团队（全部并行）
              ┌────────────┼────────────┐
              ▼            ▼            ▼
   ┌────────────────┐ ┌──────────┐ ┌──────────────┐
   │ Command        │ │ Agent    │ │ Skill        │
   │ 架构师         │ │ 工程师   │ │ 设计师       │
   │                │ │          │ │              │
   │ agent-teams/   │ │ agent-   │ │ agent-teams/ │
   │ .claude/       │ │ teams/   │ │ .claude/     │
   │ commands/      │ │ .claude/ │ │ skills/      │
   │ time-          │ │ agents/  │ │ time-svg-    │
   │ orchestrator.md│ │ time-    │ │ creator/     │
   │                │ │ agent.md │ │              │
   └───────┬────────┘ └────┬─────┘ └──────┬───────┘
           │               │              │
           ▼               ▼              ▼
   ┌──────────────────────────────────────────────────┐
   │                 共享任务列表                       │
   │  ☐ 约定数据契约：{time, tz, formatted}           │
   │  ☐ Command 使用 Agent 工具（非 bash）             │
   │  ☐ Agent 预加载 time-fetcher skill               │
   │  ☐ Skill 从上下文读取时间（不重新获取）           │
   │  ☐ 所有文件位于 agent-teams/.claude/ 内          │
   └──────────────────────────────────────────────────┘
                       │
                       ▼
          ┌──────────────────────────────┐
          │  cd agent-teams && claude    │
          │    /time-orchestrator        │
          │   Command → Agent → Skill    │
          └──────────────────────────────┘
```
