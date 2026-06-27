# Agent Teams 实现

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar_12%2C_2026-white?style=flat&labelColor=555)

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

<a href="#time-orchestration"><img src="../!/tags/implemented-hd.svg" alt="Implemented"></a>

<p align="center">
  <img src="assets/impl-agent-teams.png" alt="Agent Teams 实战 — tmux 分屏模式" width="100%">
</p>

Agent Teams 会生成**多个独立的 Claude Code 会话**，通过共享任务列表进行协调。与 subagent（单个会话内的隔离 context fork）不同，每个 teammate 拥有自己完整的 context window，并自动加载 CLAUDE.md、MCP server 和 skill。

---

## ![如何使用](../!/tags/how-to-use.svg)

时间编排工作流完全由 agent team 构建。要运行成品：

```bash
cd agent-teams
claude
/time-orchestrator
```

这会调用 **Command → Agent → Skill** 流水线：agent 获取迪拜的当前时间，skill 将 SVG 时间卡片渲染到 `agent-teams/output/dubai-time.svg`。

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

### 3. 使用 team 结构进行提示

<a id="time-orchestration"></a>

将此 prompt 粘贴到 Claude 中，以使用 agent teams 引导完整的时间编排器工作流：

主 prompt：**[agent-teams-prompt.md](../agent-teams/agent-teams-prompt.md)**

### Team 协调流程

```
┌──────────────────────────────────────────────────────────────┐
│                         LEAD（你）                            │
│        "创建一个 agent team 来构建时间编排"                   │
└──────────────────────────┬───────────────────────────────────┘
                           │ 生成 team（全部并行）
              ┌────────────┼────────────┐
              ▼            ▼            ▼
   ┌────────────────┐ ┌──────────┐ ┌──────────────┐
   │ Command        │ │ Agent    │ │ Skill        │
   │ Architect      │ │ Engineer │ │ Designer     │
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
   │            共享任务列表                          │
   │  ☐ 商定 data contract：{time, tz, formatted}    │
   │  ☐ Command 使用 Agent 工具（非 bash）           │
   │  ☐ Agent 预加载 time-fetcher skill              │
   │  ☐ Skill 从 context 读取时间（不重新获取）      │
   │  ☐ 所有文件位于 agent-teams/.claude/ 内         │
   └──────────────────────────────────────────────────┘
                       │
                       ▼
          ┌──────────────────────────────┐
          │  cd agent-teams && claude    │
          │    /time-orchestrator        │
          │   Command → Agent → Skill    │
          └──────────────────────────────┘
```
