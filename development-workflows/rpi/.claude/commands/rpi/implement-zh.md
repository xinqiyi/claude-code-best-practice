---
description: 执行带验证关卡的分阶段实施
argument-hint: "<feature-slug> [--phase N] [--validate-only]"
---

## 用户输入

```text
$ARGUMENTS
```

你必须**解析用户输入以提取功能 slug（`rpi/` 中的文件夹名称）。

## 目的

此命令基于规划文档执行功能的分阶段实施。它编排专门的 agents，强制执行验证关卡，并确保整个实施过程符合 constitutional compliance。

**前置条件**：
- 功能文件夹存在于 `rpi/{feature-slug}/`
- 规划已完成（`rpi/{feature-slug}/plan/PLAN.md` 存在）

**输出位置**：`rpi/{feature-slug}/implement/`

**这是 RPI 工作流的第 4 步**（最后一步——实际实施）。

## 标志

- `--phase N`：执行特定阶段编号（1-8），如省略则从阶段 1 开始
- `--validate-only`：仅验证当前阶段，不实施
- `--skip-validation`：跳过验证关卡并继续（谨慎使用）

## 可用 Agents

所有 agents 使用 **Opus 模型**以获得最高质量。

### 实施 Agent

| Agent | Type | 何时使用 |
|-------|------|---------|
| `senior-software-engineer` | Custom | 所有实施任务 |

### 支持 Agents

| Agent | Type | Purpose |
|-------|------|---------|
| `Explore` | Built-in | 实施前代码探索 |
| `code-reviewer` | Custom | 代码审查和质量验证 |
| `constitutional-validator` | Custom | 针对项目 constitution 验证 |
| `documentation-analyst-writer` | Built-in | 文档生成 |

## 工作流概述

### 第 0 阶段：加载 Context 和规则
- 加载项目 constitution
- 加载领域特定指南
- 分析实施范围

### 分阶段实施循环

每个在 PLAN.md 中的阶段：

```
┌─────────────────────────────────────────────────────────────────┐
│ Phase N：[阶段名称]                                                │
├─────────────────────────────────────────────────────────────────┤
│ 1. 代码发现（Explore Agent）→ 了解现有代码                          │
│ 2. 实施（senior-software-engineer）→ 实现阶段交付物                  │
│ 3. 自我验证 → Engineer 对照阶段 checklist 验证                       │
│ 4. 代码审查（code-reviewer）→ 安全、正确性、可维护性                    │
│ 5. 用户验证关卡 → STOP 并请求用户批准                                │
│ 6. 文档更新 → 更新 PLAN.md 中的阶段状态                              │
└─────────────────────────────────────────────────────────────────┘
```

### 错误处理

- **实施失败**：尝试替代方案（最多 2 次），然后 STOP 并询问用户
- **测试失败**：分析原因并修复，持续失败则上报
- **构建失败**：检查类型错误、缺失导入、语法错误
- **Agent 失败**：重试一次，仍失败则跳过该 agent

### 完成报告

所有阶段完成后，生成包含以下内容的完成报告：
- 功能摘要和已执行阶段
- 修改的文件和添加的测试
- 代码审查摘要
- Constitutional Compliance 检查
- 创建的 Artifacts
- 后续步骤和 PR 说明

## 质量关卡

### 每阶段质量关卡
- [ ] 所有交付物已实施
- [ ] Linting 通过
- [ ] 测试通过
- [ ] 构建成功
- [ ] 代码审查通过
- [ ] 用户验证已收到
- [ ] 文档已更新

### 最终质量关卡
- [ ] 所有阶段已验证
- [ ] 无失败的测试
- [ ] 完整构建成功
- [ ] Constitutional compliance 已验证
- [ ] 领域规则已遵循
- [ ] PR 说明已生成
