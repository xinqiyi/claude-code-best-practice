---
name: vibe-to-agentic-framework
description: 该演示文稿背后的概念框架 — "Vibe Coding 到 Agentic Engineering"的含义、为什么 journey 以此方式构建，以及每个 slide 如何融入叙事弧线
---

# "Vibe Coding 到 Agentic Engineering"框架

该 skill 教授演示文稿背后的**概念模型**。每个 slide 和章节的存在都在讲述一个故事：开发者如何从无结构的"vibe coding"（Low level）逐步迈向高级的 agentic engineering（High level）。

## 核心概念

**Vibe Coding（Low level）**是指开发者使用 Claude Code 时没有任何结构 — 没有项目 context、没有约定、没有可复用的知识。每次 prompt 都是一次赌博。Claude 可能创建随机的 endpoint、忽略既有模式、跳过测试、产出不一致的代码。每次交互都让代码库滑向熵增。

**Agentic Engineering（High level）**是指 Claude Code 作为一个完整配置的工程系统运行。它了解项目架构（CLAUDE.md）、遵循范围化约定（Rules）、按需加载领域专业知识（Skills）、将任务委派给专门的 worker（Agents）、编排多步工作流（Commands）、自动化生命周期事件（Hooks）并连接外部工具（MCP Servers）。每次 prompt 产出的是风格一致、经过测试、达到生产级质量的代码。

这两个极端之间的旅程是**渐进且累积的**。每个最佳实践都建立在之前的实践之上，演示文稿按开发者应采用的顺序进行教授。

## 4-Level Journey 系统

该演示文稿使用 4-level 评分系统代替百分比进度条：

| Level | 顺序 | 颜色 | Journey Bar 高度 | 描述 |
|-------|-------|-------|--------------------|-------------|
| Low | 1 | 红/橙（`hsl(0, 70%, 45%)`） | 25% | Vibe coding 领域 — 无结构 |
| Medium | 2 | 黄色（`hsl(40, 70%, 45%)`） | 50% | 结构化工作流，部分自动化 |
| High | 3 | 浅绿（`hsl(80, 70%, 45%)`） | 75% | 领域知识、skills、自定义 agents |
| Pro | 4 | 深绿（`hsl(120, 70%, 45%)`） | 100% | 完全 agentic engineering，多 agent 团队 |

Journey bar 在 slide 1（标题 slide）上被隐藏，从 slide 2 开始显示。Level 通过关键转折 slide 上的 `data-level` 属性设置，并由后续 slide 继承，直到下一个 level 变化。当 level 发生变化时，`.level-badge` 由 JS 注入到 slide 的 `h1` 中（不要硬编码在 HTML 中）。

## 贯穿示例：TodoApp Monorepo

每个技巧都在一个真实的全栈项目上演示。该演示文稿展示了从普通项目（vibe coding）到具有完整 Claude Code 配置（agentic engineering）的转变：

**转变前（Vibe Coding）：**
```
todoapp/
├── backend/          # FastAPI（Python）
│   ├── main.py
│   ├── routes/
│   ├── models/
│   └── tests/
└── frontend/         # Next.js（TypeScript）
    ├── components/
    ├── pages/
    └── lib/
```

**转变后（Agentic Engineering）：**
```
todoapp/
├── .claude/                  # Claude Code 配置
│   ├── agents/               # 自定义 subagent
│   ├── skills/               # 领域知识
│   ├── commands/             # Slash 命令
│   ├── hooks/                # 生命周期脚本
│   ├── rules/                # 模块化指令
│   ├── settings.json         # 团队设置
│   └── settings.local.json   # 个人设置
├── backend/
│   └── CLAUDE.md             # 后端指令
├── frontend/
│   └── CLAUDE.md             # 前端指令
├── .mcp.json                 # 托管的 MCP server
└── CLAUDE.md                 # 项目指令
```

**为什么用 TodoApp？**它足够小，可以放在 slide 上，但足够复杂以演示真实问题：一个具有 route 模式和测试约定的后端、一个具有组件层级和 design token 的前端，以及一个 monorepo 结构，其中横切关注点（如添加新功能）需要两端协调。

TodoApp 使 vibe-coding 问题变得具体：没有结构时，让 Claude"添加一个 notes 功能"会产生一个随机的 `/api/notes` endpoint，不遵循 `routes/todos.py` 的模式，一个没有 sidebar 导航的独立页面，以及零测试。而在完整的 agentic 设置下，同样的请求会产生遵循现有模式的 route、集成到 sidebar 中的页面，以及匹配 `test_todos.py` 风格的测试。

## Journey 弧线：为什么是这个顺序

该演示文稿遵循一个有意的教学顺序。每个章节解锁一个新的能力层：

### Part 0: Introduction（Slides 1–4，无 weight）
**目的：** 搭建舞台。介绍 TodoApp，定义 vibe coding，展示目的地。
- 标题 slide 确立了 journey 的隐喻
- Example Project 展示了转变：TodoApp 的前后对比 — 普通项目结构与完整 Claude Code 配置（`.claude/`、`CLAUDE.md`、`.mcp.json` 等）的对比
- "What is Vibe Coding?"创建了 0% 的基线 — 痛点所在
- Journey Map 提供了一个可点击的 TOC，展示前方的完整路径

### Part 1: Prerequisites（Slides 5–9，无 weight）
**目的：** 安装并运行 Claude Code。纯粹的后勤工作 — 还不涉及工程实践。
- 安装、认证、首次 session、界面概览
- 没有 weight，因为知道如何安装工具并不能提高代码质量
- "首次 session"本身就是 vibe coding — 这是刻意的，让开发者亲身体验 0% 的状态

### Part 2: Better Prompting（Slides 10–17，Level: Low）
**目的：** 第一个真正的改进。更好的输入产生更好的输出，即使没有任何项目配置。
- **Good vs Bad Prompts：** 具体、范围明确的 prompt 对比模糊的请求。最简单的改进。
- **Providing Context：** 使用 `@files` 为 Claude 提供所需的代码。立即减少 hallucination。
- **Context Window & /compact：** 理解有限的 context window 可防止长 session 中响应质量下降。
- **Plan Mode：** `/plan` 强制在编码前思考。避免在错误方向上浪费精力。

**为什么是 Low level：** Prompting 是基础但有限。它改进了单次交互，但不能创建持久的项目知识。每个 session 从零开始。

### Part 3: Project Memory（Slides 18–24，Level: Medium）
**目的：** 从 session 级知识跃升到项目级知识。Claude 现在可以跨 session 记忆。
- **CLAUDE.md & /init：** 项目的"给 Claude 的 README"。确立架构、技术栈和约定。这是最具影响力的单个文件。
- **What to Include：** 编写有效 CLAUDE.md 内容的实用指导（保持在 150 行以内，聚焦于 Claude 需要了解的内容）。
- **Rules：** `.claude/rules/` 中的路径范围约定。Rules 是一个倍增器 — 它们自动应用于每个匹配的文件，无需开发者付出努力即可强制一致性。一个 `backend-testing.md` rule 就能确保每个测试永远遵循相同的模式。

**为什么是 Medium level：** Project memory 将 Claude 从无状态工具转变为有 context 意识的协作者。但仅有知识并不能创建工作流。

### Part 4: Structured Workflows（Slides 25–28，Level: Medium）
**目的：** 防止浪费精力并提高执行质量的系统性方法。
- **Task Lists：** 将复杂工作分解为可追踪的步骤。防止 scope creep 并确保完整性。
- **Model Selection：** 选择合适的 model（Opus 用于架构、Sonnet 用于实现、Haiku 用于快速任务）优化成本和质量。

**为什么仍然是 Medium level：** Workflow 重要但相对简单的概念。它们建立在 Part 3 的 project memory 之上，并更系统地使用它。迈向 High 的关键一步来自领域知识。

### Part 5: Domain Knowledge（Slides 29–33，Level: High）
**目的：** 可复用、按需加载的专业知识。Skills 是静态 memory（CLAUDE.md/Rules）与动态 agent 之间的桥梁。
- **What Are Skills：** Skills 是打包的领域知识，Claude 在相关时加载。渐进式披露的概念。
- **Creating Skills：** 动手实践：为 TodoApp 构建一个 `frontend-conventions` skill，教授 Tailwind token、组件模式和 sidebar 集成。
- **Skill Frontmatter & Invocation：** 技术细节：YAML frontmatter、手动调用 vs 自动发现调用、`context: fork` 选项。

**为什么是 High level：** Skills 是第一个"倍增器"概念 — 一个 skill 定义可以改善其领域内每次后续交互。但 skills 是被动知识；它们需要 agent 才能变得主动。

### Part 6: Agentic Engineering（Slides 34–46，Level: High）
**目的：** 本演示文稿涵盖的终点。自主的、专门化的 agent，端到端协调构建功能。
- **What Are Agents：** 具有受限 tools 和预加载 skills 的专门化 subagent 的概念。
- **Frontend Engineer Agent：** 一个具体的 agent，使用 TodoApp 的前端约定，将 route 添加到 sidebar，遵循 design token。前后对比展示了转变。
- **Backend Engineer Agent：** 后端的并行 agent — 遵循 FastAPI route 模式、SQLAlchemy model，编写匹配现有风格的测试。
- **Commands & Orchestration：** 顶石模式：Command → Agent → Skills。一个 `/add-feature` 命令协调前端 + 后端 agent，每个都有各自的 skills，以交付完整的功能。这是架构的顶峰。
- **Hooks & MCP：** 生命周期自动化（pre-commit 检查、声音通知）和外部工具集成。最终的自动化层。
- **Command → Agent → Skills：** 完整架构图。展示所有部分如何连接：commands 调用 agents，agents 加载 skills，skills 提供知识。这是"High level"理解 slide。

**为什么是 High level：** 本章节涵盖本演示文稿所教授的最高价值实践。之前的一切都是为此铺垫。编排和 agentic workflow 代表了本课程涵盖的天花板 — 完全的 Pro（多 agent 团队、高级编排模式）超出了本演示文稿的范围。

### High Level Slide（Slide 44）
庆祝时刻。展示完整的 TodoApp 配置：
- CLAUDE.md 用于项目 context
- Rules 用于路径范围约定
- Skills 用于领域知识
- Agents 用于一致的执行
- Commands 用于编排的工作流
- Hooks 用于生命周期自动化
- MCP server 用于外部工具

### Appendix（Slides 47+，无 weight）
**目的：** 参考资料。每个命令、设置和配置选项。无 weight，因为这些是参考查阅，而非 journey 里程碑。包括：工具使用、所有 slash 命令、commit/PR 工作流、自定义选项、调试技巧和黄金法则。

## 编辑 Slide 时如何使用此框架

创建或修改 slide 时，请考虑：

1. **这个概念在 journey 的哪个位置？**关于"prompt 中更好的错误信息"的 slide 属于 Part 2（prompting，Low level）。关于"agent memory scope"的 slide 属于 Part 6（agentic，High level）。

2. **前后对比是什么？**每个重要 slide 都应隐含或明确地展示对比：在 Low level（vibe coding）下会发生什么 vs 使用此技巧后会发生什么。用 TodoApp 使其具体化。

3. **Level 分配感觉对吗？**Level 转折发生在 Part 章节边界。章节内的单个 slide 继承该章节的 level。

4. **它是建立在前面内容的基础上吗？**Skills 假定开发者已经了解 CLAUDE.md 和 Rules。Agents 假定他们了解 Skills。Commands 假定他们了解 Agents。绝不要在其章节之前引用一个概念。

5. **使用 TodoApp。**抽象的解释会失去观众。展示实际的 `routes/todos.py` 代码、实际的 `Sidebar.tsx` 组件、实际的 `CLAUDE.md` 内容。贯穿示例使框架变得具体。

## Level 转折参考表

| Slide | Slide 名称 | data-level | Level 标签 |
|-------|-----------|------------|-------------|
| 10 | Better Prompting（章节分隔页） | `data-level="low"` | Low |
| 18 | Project Memory（章节分隔页） | `data-level="medium"` | Medium |
| 29 | Domain Knowledge（章节分隔页） | `data-level="high"` | High |
| 34 | Agentic Engineering（章节分隔页） | `data-level="high"` | High |

所有其他 slide 继承其前最后一个 `data-level` 属性设置的 level。Slides 1–9（Intro + Prerequisites）没有 level，bar 保持隐藏直到 slide 2，显示"Low"之前的空状态（slides 2–9 在 slide 10 的第一个 level 转折之前，因此 bar 显示为空/零直到 slide 10）。

**注意：** 主演示文稿（`presentation/index.html`）的上限是 **High** level — 不使用 `data-level="pro"`。Pro 刻度标记在 journey bar 上保持可见作为理论天花板，但填充从未达到。视频演示文稿（`1-video-workflow.html`）的上限是 **Medium** level。
