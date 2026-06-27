# Cross-Model（Claude Code + Codex）工作流

基于 [claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) 和 [codex-cli-best-practice](https://github.com/shanraisshan/codex-cli-best-practice)

## 工作流

```
┌─────────────────────────────────────────────────────────────────────────┐
│              跨模型 Claude Code + Codex 工作流                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  第 1 步：PLAN                                         Claude Code      │
│  ─────────────                                         Opus 4.6         │
│  在 Terminal 1 中以 plan mode 打开 Claude Code。        Plan Mode        │
│  Claude 通过 AskUserQuestion 与你进行访谈。                              │
│  生成带有测试关卡的分阶段计划。                                          │
│                                                                         │
│  输出：plans/{feature-name}.md                                           │
│                                                                         │
│                              ▼                                          │
│                                                                         │
│  第 2 步：QA REVIEW                                    Codex CLI         │
│  ──────────────────                                    GPT-5.4           │
│  在另一个终端中（Terminal 2）打开 Codex CLI。                             │
│  Codex 对照实际代码库审查计划。                                          │
│  插入中间阶段（"Phase 2.5"）                                             │
│  带有 "Codex Finding" 标题。                                             │
│  向计划追加内容——从不重写原始阶段。                                       │
│                                                                         │
│  输出：plans/{feature-name}.md（已更新）                                  │
│                                                                         │
│                              ▼                                          │
│                                                                         │
│  第 3 步：IMPLEMENT                                    Claude Code       │
│  ──────────────────                                    Opus 4.6          │
│  开始一个新的 Claude Code session（Terminal 1）。                         │
│  你逐步实现每个阶段                                                     │
│  每个阶段都有测试关卡检查。                                              │
│                                                                         │
│                              ▼                                          │
│                                                                         │
│  第 4 步：VERIFY                                       Codex CLI         │
│  ────────────────                                      GPT-5.4           │
│  开始一个新的 Codex CLI session（Terminal 2）。                           │
│  Codex 对照计划验证实现效果。                                            │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 跨模型工作流在生产环境中的实际表现

![Cross-Model Workflow](assets/cross-model-workflow.png)

*最后更新：2026-03-06*
