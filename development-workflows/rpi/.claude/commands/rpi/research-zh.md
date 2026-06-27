---
description: 研究和分析功能可行性 — GO/NO-GO 决策关卡
argument-hint: "<feature-slug>"
---

## 用户输入

```text
$ARGUMENTS
```

你必须解析用户输入以提取功能 slug（`rpi/` 中的文件夹名称）。

**预期输入格式**：`rpi/{feature-slug}/REQUEST.md`

## 目的

此命令在规划阶段开始**之前**对功能请求进行全面研究和分析。它作为一个关键的 GO/NO-GO 关卡，决定功能想法是否应进入详细规划。

**关键目标**：
- 评估产品市场契合度和用户价值
- 评估技术可行性和复杂性
- 识别风险和潜在阻碍
- 确定正确的方法（构建、购买、合作或拒绝）
- 给出明确的 go/no-go 建议及清晰理由

**前置条件**：
- 功能文件夹存在于 `rpi/{feature-slug}/`
- 功能请求文件存在于 `rpi/{feature-slug}/REQUEST.md`

**输出位置**：`rpi/{feature-slug}/research/RESEARCH.md`

**这是 RPI 工作流的第 2 步**（在第 1 步初始功能描述之后）。

## 工作流概述

### 第 0 阶段：加载 Context
- 读取功能描述
- 检查项目 constitution（如果存在）
- 创建研究 context

### 第 1 阶段：解析功能请求
- **Agent**：requirement-parser
- 提取功能名称、类型、目标组件、需求和约束

### 第 2 阶段：产品分析与 Constitution 一致性
- **Agent**：product-manager
- 评估用户价值、市场契合度、产品愿景和 constitution 一致性

### 第 2.5 阶段：技术发现（代码探索）
- **Agent**：Explore（通过 Task tool）
- **关键阶段**：在做出技术可行性评估之前深入分析现有代码库
- 分析现有实现、集成点、数据模型、依赖项和现有模式

### 第 3 阶段：技术可行性评估
- **Agent**：senior-software-engineer
- 评估技术方法、复杂性、依赖项、技术债务和风险

### 第 4 阶段：战略评估
- **Agent**：technical-cto-advisor
- 综合产品和技术的观点，给出 GO/NO-GO 建议

### 第 5 阶段：生成研究报告
- **Agent**：documentation-analyst-writer
- 生成 `rpi/{feature-slug}/research/RESEARCH.md`

## 使用到的 Agents

| Phase | Agent | Type |
|-------|-------|------|
| Phase 1 | requirement-parser | Custom |
| Phase 2 | product-manager | Custom |
| Phase 2.5 | Explore | Built-in |
| Phase 3 | senior-software-engineer | Custom |
| Phase 4 | technical-cto-advisor | Custom |
| Phase 5 | documentation-analyst-writer | Built-in |

## 后续步骤

根据建议：
- **GO** → 运行 `/rpi:plan "{feature-slug}"`
- **CONDITIONAL GO** → 先解决条件
- **NO-GO** → 审查理由并考虑替代方案
