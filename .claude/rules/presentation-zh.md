---
paths:
  - "presentation/**"
---

# Presentation Delegation

## Delegation Rule

任何更新、修改或修复 presentation 的请求**必须**由匹配的每个 presentation 的 agent 处理。**绝不直接编辑 presentation 的 HTML。** 根据用户所指的 presentation 进行路由：

| Presentation | Path | Agent |
|---|---|---|
| Vibe Coding → Agentic Engineering | `presentation/vibe-coding-to-agentic-engineering/index.html` | `presentation-vibe-coding` |
| Claude Code & Gemini CLI（GDG Kolachi 活动 deck） | `presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini/index.html` | `presentation-claude-gemini` |
| Claude Code Best Practice（规范化的可重用 deck） | `presentation/claude-code-best-practice/index.html` | `presentation-claude-code` |

通过 Agent 工具调用：

```
Agent(subagent_type="presentation-vibe-coding", description="...", prompt="...")
Agent(subagent_type="presentation-claude-gemini", description="...", prompt="...")
Agent(subagent_type="presentation-claude-code", description="...", prompt="...")
```

如果用户说 "the presentation" 而没有指定是哪个，在委托之前询问他们指的是哪一个。注意 "the main presentation" 或 "the best-practice deck" 通常指的是 `presentation-claude-code` — 规范化的可重用 deck — 但如果存在歧义请确认。

## 为什么

每个 presentation 有自己的幻灯片编号、级别系统和目标受众。每个 presentation 的 agent 让每个 deck 保持专注的知识库并自我进化，避免相互污染。vibe-coding agent 预加载了特定于该 deck 的 framework/structure/styling skills。claude-gemini agent 针对非技术性 GDG 活动受众，使用带有 6 个级别的旅程栏。claude-code agent 拥有规范化的可重用 best-practices deck（于 2026-04-30 从 GDG 的分支而来）— 相同的受众语调，更简单的结构（仅 level-badge，无旅程栏），活动无关的身份标识。
