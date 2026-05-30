---
paths:
  - "presentation/**"
---

# 演示文稿委派

## 委派规则

任何更新、修改或修复演示文稿的请求，都**必须**由匹配的按演示文稿专属 agent 处理。**切勿直接编辑演示文稿 HTML。** 根据用户所指的演示文稿进行路由：

| 演示文稿 | 路径 | Agent |
|---|---|---|
| Vibe Coding → Agentic Engineering | `presentation/vibe-coding-to-agentic-engineering/index.html` | `presentation-vibe-coding` |
| Claude Code & Gemini CLI (GDG Kolachi 活动演示文稿) | `presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini/index.html` | `presentation-claude-gemini` |
| Claude Code 最佳实践（规范的可复用演示文稿） | `presentation/claude-code-best-practice/index.html` | `presentation-claude-code` |

通过 Agent 工具调用：

```
Agent(subagent_type="presentation-vibe-coding", description="...", prompt="...")
Agent(subagent_type="presentation-claude-gemini", description="...", prompt="...")
Agent(subagent_type="presentation-claude-code", description="...", prompt="...")
```

如果用户只说"演示文稿"而未指定具体哪一个，请在委派前先询问用户指的是哪个。注意，"主演示文稿"或"最佳实践演示文稿"通常指 `presentation-claude-code`——即规范的可复用演示文稿——但如果存在歧义，请确认。

## 原因

每个演示文稿都有自己独立的幻灯片编号、层级系统和目标受众。按演示文稿专属 agent 让每个演示文稿保持专注的知识库并自我演进，而不会相互交叉污染。vibe-coding agent 预加载了该演示文稿专属的框架/结构/样式 skills。claude-gemini agent 面向 GDG 活动的非技术受众，使用包含 6 个层级的 journey-bar。claude-code agent 拥有规范的可复用最佳实践演示文稿（于 2026-04-30 从 GDG 演示文稿 fork 而来）——相同的受众语气，更简单的结构（仅 level-badge，无 journey bar），与活动无关的标识。
