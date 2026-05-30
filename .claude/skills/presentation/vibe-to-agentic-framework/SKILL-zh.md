---
name: vibe-to-agentic-framework
description: 演讲背后的概念框架——"Vibe Coding 到 Agentic Engineering"的含义、为什么旅程以此方式构建，以及每张幻灯片如何融入叙事主线
---

# "Vibe Coding 到 Agentic Engineering"框架

本 skill 教授演讲背后的**概念模型**。每张幻灯片和每个板块的存在都是为了讲述一个单一故事：开发者如何逐步从非结构化的 "vibe coding"（低级）演进到高级的 agentic engineering（高级）。

## 核心概念

**Vibe Coding（低级）**是指开发者使用 Claude Code 时没有任何结构——没有项目上下文、没有约定、没有可复用的知识。每次 prompt 都像是抛硬币。Claude 可能创建随机的端点、忽略现有模式、跳过测试，并产生不一致的代码。代码库在每次交互中都朝着熵增的方向漂移。

**Agentic Engineering（高级）**是指 Claude Code 作为一个完全配置的工程系统运行。它了解项目架构（CLAUDE.md）、遵循作用域限定的约定（Rules）、按需加载领域专业知识（Skills）、委派给专门的工人（Agents）、编排多步骤工作流（Commands）、自动化生命周期事件（Hooks），并连接到外部工具（MCP Servers）。每次 prompt 都能产生一致、经过测试、适合生产质量的代码。

这两个极端之间的旅程是**渐进且累积的**。每一个最佳实践都建立在前一个之上，演讲按照开发者应采用的顺序来教授它们。

## 4级旅程系统

演讲使用 4 级评分系统，而非百分比条：

| 级别 | 顺序 | 颜色 | 旅程条高度 | 描述 |
|-------|-------|-------|--------------------|-------------|
| Low | 1 | 红/橙（`hsl(0, 70%, 45%)`） | 25% | Vibe coding 领地——无结构 |
| Medium | 2 | 黄（`hsl(40, 70%, 45%)`） | 50% | 结构化工作流，一定自动化 |
| High | 3 | 浅绿（`hsl(80, 70%, 45%)`） | 75% | 领域知识、skills、自定义 agents |
| Pro | 4 | 深绿（`hsl(120, 70%, 45%)`） | 100% | 完整的 agentic engineering，多 agent 团队 |

旅程条在第 1 张幻灯片（标题幻灯片）上隐藏，从第 2 张幻灯片开始出现。级别通过在关键过渡幻灯片上设置 `data-level` 属性设定，并由后续幻灯片继承，直到下一次级别变更。当级别变化时，`.level-badge` 会通过 JS 注入到幻灯片的 `h1` 上（不要在 HTML 中硬编码）。

## 贯穿示例：TodoApp 单体仓库

每项技术都在一个现实的全栈项目上演示。演讲展示了一个普通项目（vibe coding）到拥有完整 Claude Code 配置（agentic engineering）的转变：

**之前（Vibe Coding）：**
```
todoapp/
├── backend/          # FastAPI (Python)
│   ├── main.py
│   ├── routes/
│   ├── models/
│   └── tests/
└── frontend/         # Next.js (TypeScript)
    ├── components/
    ├── pages/
    └── lib/
```

**之后（Agentic Engineering）：**
```
todoapp/
├── .claude/                  # Claude Code 配置
│   ├── agents/               # 自定义 subagents
│   ├── skills/               # 领域知识
│   ├── commands/             # 斜杠命令
│   ├── hooks/                # 生命周期脚本
│   ├── rules/                # 模块化指令
│   ├── settings.json         # 团队设置
│   └── settings.local.json   # 个人设置
├── backend/
│   └── CLAUDE.md             # 后端指令
├── frontend/
│   └── CLAUDE.md             # 前端指令
├── .mcp.json                 # 托管的 MCP servers
└── CLAUDE.md                 # 项目指令
```

**为什么选择 TodoApp？** 它足够小，可以放在幻灯片上，但又足够复杂，能够演示真实问题：一个具有路由模式和测试约定的后端、一个具有组件层级结构和设计令牌的前端，以及一个单体仓库结构——在这种结构中，横切关注点（如添加新功能）需要两侧之间的协调。

TodoApp 让 vibe-coding 问题变得具体：在没有结构的情况下，要求 Claude "添加一个 notes 功能"会产生一个不遵循 `routes/todos.py` 模式的随机 `/api/notes` 端点、一个没有侧边栏导航的独立页面，以及零测试。而在完整的 agentic 设置下，同样的请求会产生遵循现有模式的路由、集成到侧边栏的页面，以及与 `test_todos.py` 风格匹配的测试。

## 旅程主线：为什么是这个顺序

演讲遵循一个精心设计的教学序列。每个部分解锁一个新的能力层：

### 第 0 部分：引言（幻灯片 1–4，无权重）
**目的：** 奠定基础。介绍 TodoApp，定义 vibe coding，并展示目的地。
- 标题幻灯片建立旅程隐喻
- 示例项目展示转变：TodoApp 的前后对比——普通项目结构与完整 Claude Code 配置（.claude/、CLAUDE.md、.mcp.json 等）的对比
- "什么是 Vibe Coding？"创建 0% 基线——痛点
- 旅程地图提供可点击的目录，展示前方的完整路径

### 第 1 部分：前置准备（幻灯片 5–9，无权重）
**目的：** 安装并运行 Claude Code。这纯粹是后勤性的——还没有工程实践。
- 安装、认证、首次会话、界面概览
- 无权重，因为知道如何安装工具并不能提高代码质量
- "首次会话"就是 vibe coding——这是有意为之，让开发者亲身体验 0% 状态

### 第 2 部分：更好的 Prompting（幻灯片 10–17，级别：Low）
**目的：** 第一个真正的改进。更好的输入产生更好的输出，即使没有任何项目配置。
- **好的 vs 差的 Prompts：** 具体、限定范围的 prompts vs 模糊的请求。最简单的改进。
- **提供上下文：** 使用 `@files` 向 Claude 提供它需要的代码。立即减少幻觉。
- **上下文窗口与 /compact：** 理解有限的上下文窗口可以防止长时间会话中的回答质量下降。
- **Plan 模式：** `/plan` 强制在编码之前先思考。防止在错误方法上浪费精力。

**为什么是 Low 级别：** Prompting 是基础性的但有限。它改善了个别交互，但不会创建持久性的项目知识。每个会话从零开始。

### 第 3 部分：项目记忆（幻灯片 18–24，级别：Medium）
**目的：** 从会话级别跃升至项目级别的知识。Claude 现在能跨会话记忆。
- **CLAUDE.md 与 /init：** 项目的"给 Claude 的 README"。建立架构、技术栈和约定。这是最有影响力的单个文件。
- **包含什么内容：** 编写有效 CLAUDE.md 内容的实用指南（保持在 150 行以下，专注于 Claude 需要知道的内容）。
- **Rules：** `.claude/rules/` 中按路径限定的约定。Rules 是倍增器——它们自动应用于每个匹配的文件，无需开发者付出努力即可强制执行一致性。一个 `backend-testing.md` 规则可确保每个测试永远遵循相同的模式。

**为什么是 Medium 级别：** 项目记忆将 Claude 从一个无状态工具转变为一个了解上下文的协作者。但仅有知识并不能创建工作流。

### 第 4 部分：结构化工作流（幻灯片 25–28，级别：Medium）
**目的：** 防止浪费精力并提高执行质量的系统性方法。
- **Task Lists：** 将复杂工作分解为可追踪的步骤。防止范围蔓延并确保完整性。
- **模型选择：** 选择合适的模型（Opus 用于架构，Sonnet 用于实现，Haiku 用于快速任务）优化成本和质量。

**为什么仍然是 Medium 级别：** 工作流很重要但相对简单。它们建立在第 3 部分的项目记忆之上，并更系统地使用它。迈入 High 级别需要领域知识。

### 第 5 部分：领域知识（幻灯片 29–33，级别：High）
**目的：** 可复用的按需专业知识。Skills 是静态记忆（CLAUDE.md/Rules）和动态 agents 之间的桥梁。
- **什么是 Skills：** Skills 作为打包的领域知识，Claude 在相关时加载。渐进式披露的概念。
- **创建 Skills：** 动手实践：为 TodoApp 构建一个 `frontend-conventions` skill，教授 Tailwind 令牌、组件模式和侧边栏集成。
- **Skill 前置元数据与调用：** 技术细节：YAML 前置元数据、手动与自动发现调用、`context: fork` 选项。

**为什么是 High 级别：** Skills 是第一个"倍增器"概念——一个 skill 定义能改善其领域内所有未来的交互。但 skills 是被动知识；它们需要 agents 才能变得活跃。

### 第 6 部分：Agentic Engineering（幻灯片 34–46，级别：High）
**目的：** 本次演讲覆盖的目的地。自主的、专门的 agents，它们相互协调以端到端地构建功能。
- **什么是 Agents：** 具有受限工具和预加载 skills 的专门化 subagents 的概念。
- **前端工程师 Agent：** 一个具体的 agent，它使用 TodoApp 的前端约定，将路由添加到侧边栏，遵循设计令牌。前后对比展示了转变。
- **后端工程师 Agent：** 后端的并行 agent——遵循 FastAPI 路由模式、SQLAlchemy 模型，编写与现有风格匹配的测试。
- **Commands 与编排：** 压轴模式：Command → Agent → Skills。一个 `/add-feature` 命令协调前端和后端 agents，每个 agents 都有自己的 skills，以交付一个完整功能。这是架构的巅峰。
- **Hooks 与 MCP：** 生命周期自动化（提交前检查、声音通知）和外部工具集成。最终的自动化层。
- **Command → Agent → Skills：** 完整架构图。展示所有部分如何连接：commands 调用 agents，agents 加载 skills，skills 提供知识。这是"High 级别"理解的幻灯片。

**为什么是 High 级别：** 本节涵盖了本次演讲中教授的最高价值实践。之前的所有内容都在为此铺路。编排和 agentic 工作流代表了本课程覆盖的上限——完整的 Pro（多 agent 团队、高级编排模式）超出了本次演讲的范围。

### High 级别幻灯片（第 44 张幻灯片）
庆祝时刻。展示完整的 TodoApp 配置：
- CLAUDE.md 用于项目上下文
- Rules 用于按路径限定的约定
- Skills 用于领域知识
- Agents 用于一致的执行
- Commands 用于编排的工作流
- Hooks 用于生命周期自动化
- MCP servers 用于外部工具

### 附录（幻灯片 47+，无权重）
**目的：** 参考资料。每个命令、设置和配置选项。无权重，因为这些是参考查询，而非旅程里程碑。包括：工具使用、所有斜杠命令、提交/PR 工作流、自定义选项、调试技巧和黄金法则。

## 如何在编辑幻灯片时使用此框架

在创建或修改幻灯片时，请考虑：

1. **这个概念在旅程中位于何处？** 关于"prompts 中更好的错误消息"的幻灯片属于第 2 部分（prompting，Low 级别）。关于"agent 记忆作用域"的幻灯片属于第 6 部分（agentic，High 级别）。

2. **之前/之后的对比是什么？** 每张重要的幻灯片都应隐式或显式地展示对比：在 Low 级别（vibe coding）下会发生什么，以及使用此技术后会发生什么。使用 TodoApp 使其具体化。

3. **级别分配感觉对吗？** 级别转换发生在各部分的分界处。部分内的各个幻灯片继承该部分的级别。

4. **它是否建立在之前的内容之上？** Skills 假设开发者已经了解 CLAUDE.md 和 Rules。Agents 假设他们了解 Skills。Commands 假设他们了解 Agents。永远不要在其所属部分之前引用某个概念。

5. **使用 TodoApp。** 抽象的解释会让听众迷失。展示实际的 `routes/todos.py` 代码、实际的 `Sidebar.tsx` 组件、实际的 `CLAUDE.md` 内容。贯穿示例就是让框架变得具体可感的东西。

## 级别转换参考表

| 幻灯片 | 幻灯片名称 | data-level | 级别标签 |
|-------|-----------|------------|-------------|
| 10 | 更好的 Prompting（部分分隔页） | `data-level="low"` | Low |
| 18 | 项目记忆（部分分隔页） | `data-level="medium"` | Medium |
| 29 | 领域知识（部分分隔页） | `data-level="high"` | High |
| 34 | Agentic Engineering（部分分隔页） | `data-level="high"` | High |

所有其他幻灯片继承其之前最后一个 `data-level` 属性设置的级别。幻灯片 1-9（引言 + 前置准备）没有级别，并保持条隐藏，直到第 2 张幻灯片显示 "Low"（幻灯片 2-9 处于第 10 张幻灯片的第一次级别转换之前，因此条显示为空/零，直到第 10 张幻灯片）。

**注意：** 主演讲（`presentation/index.html`）的上限是 **High** 级别——不使用 `data-level="pro"`。Pro 标记在旅程条上保持可见，作为理论上的上限，但填充条永远不会到达它。视频演讲（`1-video-workflow.html`）的上限是 **Medium** 级别。
