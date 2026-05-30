---
description: 使用验证门执行分阶段实现
argument-hint: "<feature-slug> [--phase N] [--validate-only]"
---

## 用户输入

```text
$ARGUMENTS
```

你**必须**解析用户输入以提取功能标识（`rpi/` 中的文件夹名称）。

## 目的

此命令基于规划文档执行功能的分阶段实现。它编排专门的 agent，强制执行验证门，并确保在整个实现过程中符合项目章程。

**前置条件**：
- 功能文件夹存在于 `rpi/{feature-slug}/`
- 规划已完成（`rpi/{feature-slug}/plan/PLAN.md` 存在）

**输出位置**：`rpi/{feature-slug}/implement/`

**这是 RPI 工作流的第 4 步**（最终步骤 — 实际实现）。

## 标志

- `--phase N`：执行特定阶段编号（1-8），如果省略则从阶段 1 开始
- `--validate-only`：仅验证当前阶段，不进行实现
- `--skip-validation`：跳过验证门并继续（谨慎使用）

## 可用 Agents

所有 agent 使用 **Opus 模型**以获得最高质量。

### 实现 Agent

| Agent | 类型 | 使用时机 |
|-------|------|---------|
| `senior-software-engineer` | 自定义 | 所有实现任务 |

### 支持 Agents

| Agent | 类型 | 目的 |
|-------|------|------|
| `Explore` | 内置 | 实现前的代码探索 |
| `code-reviewer` | 自定义 | 代码审查与质量验证 |
| `constitutional-validator` | 自定义 | 验证是否符合项目章程 |
| `documentation-analyst-writer` | 内置 | 文档生成 |

### Agent 路由

所有实现任务由 `senior-software-engineer` agent 处理。

---

## 阶段 0：加载上下文和规则

**前置条件**：从用户输入中解析出功能标识

**流程**：

### 0.1 加载项目章程

1. 检查仓库中是否存在章程或原则文档
2. 如果存在，提取：
   - 技术约束（类型安全、测试、组件隔离）
   - 业务原则（质量标准、工作流）
   - 架构边界
3. 存储约束条件以供实现期间强制执行

### 0.2 加载领域特定指南

根据要修改的文件，加载相关项目指南：
- 检查是否存在组件特定的 README 文件
- 检查是否存在编码风格指南
- 检查是否存在测试需求文档

### 0.3 分析实现范围

1. 读取 `rpi/{feature-slug}/plan/PLAN.md`
2. 识别所有要修改的文件
3. 将文件映射到实现 agent

**输出**：
- 章程上下文摘要
- 已加载的领域规则
- 文件到 agent 的映射
- 阶段执行计划

**验证**：
- [ ] 章程已加载（如果存在）
- [ ] 受影响文件的领域规则已加载
- [ ] 所有文件已映射到 agent
- [ ] 执行计划已理解

---

## 分阶段实现工作流

### 阶段实现循环

对于 PLAN.md 中的每个阶段：

```
┌─────────────────────────────────────────────────────────────────┐
│ 阶段 N：[阶段名称]                                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. 代码发现（Explore Agent）                                    │
│     └─→ 在修改前理解现有代码                                      │
│                                                                  │
│  2. 实现（senior-software-engineer）                             │
│     └─→ 实现阶段交付物                                           │
│                                                                  │
│  3. 自我验证                                                     │
│     └─→ 工程师根据阶段清单进行自我验证                            │
│                                                                  │
│  4. 代码审查（code-reviewer Agent）                              │
│     └─→ 安全性、正确性、可维护性                                  │
│                                                                  │
│  5. 用户验证门                                                   │
│     └─→ 停止并请求用户批准                                       │
│         ├─→ 通过：继续下一阶段                                   │
│         ├─→ 有条件通过：记录问题，继续                            │
│         └─→ 未通过：修复问题，重新验证                            │
│                                                                  │
│  6. 文档更新                                                     │
│     └─→ 在 PLAN.md 中更新阶段状态                                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 第 1 步：代码发现（每个阶段）

**Agent**：Explore（内置，通过 Task 工具）

**目的**：在进行修改之前，将实现建立在代码现实的基础上。

**流程**：
1. 通过 Task 工具使用 `subagent_type="Explore"` 启动 Explore agent
2. 请求分析当前阶段受影响的文件
3. 理解现有模式、集成点和约束

**Explore Agent 提示**：
```
Analyze the codebase to prepare for implementing Phase N of [feature-name].

Files to be modified in this phase:
[List files from PLAN.md]

Investigate and document:

1. **Current Implementation**
   - How do these files currently work?
   - What patterns are used?
   - What functions/classes will be affected?

2. **Integration Points**
   - What other files import or use these modules?
   - What APIs or interfaces will change?
   - What tests cover this code?

3. **Dependencies**
   - What libraries are used?
   - What internal utilities are available?
   - What constraints exist from current code?

4. **Patterns to Follow**
   - What coding style is used in these files?
   - What naming conventions are followed?
   - What error handling patterns exist?

5. **Risks and Considerations**
   - What could break if we change this?
   - What edge cases exist?
   - What backward compatibility concerns?

Provide a discovery summary to inform implementation.
```

**输出**：为实现 agent 提供的发现摘要

---

## 第 2 步：实现（每个阶段）

**Agent**：senior-software-engineer

**流程**：
1. 使用 senior-software-engineer agent
2. 提供来自第 1 步的发现上下文
3. 实现该阶段的所有交付物
4. 遵循章程约束和项目规则

**实现 Agent 提示模板**：
```
Acting as the [agent-name] agent, implement Phase N deliverables for [feature-name].

## Context
- Constitutional Constraints: [from Phase 0]
- Domain Rules: [from Phase 0]
- Discovery Summary: [from Step 1]

## Phase N Deliverables
[List from PLAN.md]

## Files to Modify
[List files with specific changes from PLAN.md]

## Implementation Requirements
1. Follow existing code patterns identified in discovery
2. Honor constitutional constraints (type safety, testing, etc.)
3. Follow project-specific rules (if applicable)
4. Write tests for new functionality
5. Include appropriate logging
6. Handle errors gracefully

## Quality Checklist
- [ ] Code follows existing patterns
- [ ] Type annotations present where applicable
- [ ] Tests written and passing
- [ ] No breaking changes to existing functionality
- [ ] Logging added for observability
- [ ] Error handling comprehensive

Implement all deliverables and report what was done.
```

---

## 第 3 步：自我验证

**Agent**：senior-software-engineer（与第 2 步相同）

**流程**：
1. Agent 根据阶段清单验证实现
2. 运行代码检查（使用项目配置的 linter）
3. 运行与变更相关的测试
4. 验证构建是否成功

**验证命令**（根据你的项目调整）：

```bash
# Run linter
[your-linter-command]

# Run tests
[your-test-command]

# Build/compile
[your-build-command]
```

**自我验证清单**：
- [ ] 所有交付物已实现
- [ ] 代码检查通过
- [ ] 测试通过
- [ ] 构建成功
- [ ] 现有测试无回归
- [ ] 章程约束得到遵守
- [ ] 领域规则得到遵循

---

## 第 4 步：代码审查

**Agent**：code-reviewer（自定义，自动调用）

**流程**：
1. 调用 code-reviewer agent 审查变更
2. 关注正确性、安全性、可维护性
3. 在继续之前处理阻塞性问题

**代码审查 Agent 提示**：
```
Acting as the code-reviewer agent, review the Phase N implementation for [feature-name].

## Files Changed
[List modified files]

## Changes Made
[Summary of implementation]

## Review Focus
- Correctness & tests
- Security & dependency hygiene
- Architectural boundaries
- Clarity over cleverness

## Constitutional Constraints
[From Phase 0]

Provide review using standard output format.
```

**审查裁决**：
- **已批准**：继续到用户验证
- **已批准但有建议**：记录建议，继续
- **需要修改**：修复问题，重新审查

---

## 第 5 步：用户验证门

**关键**：此步骤需要用户交互。不要自动进行。

**流程**：
1. 展示阶段交付物清单
2. 显示已实现的内容（更改的文件、添加的功能）
3. 展示来自 PLAN.md 的验证标准
4. 显示代码审查结果
5. **停止并等待用户决定**

**验证请求格式**：
```
## Phase N Validation Request

### Deliverables Completed
- [x] [Deliverable 1] - [implementation summary]
- [x] [Deliverable 2] - [implementation summary]
- ...

### Files Changed
| File | Change Type | Lines |
|------|-------------|-------|
| [file] | [add/modify] | [±N] |

### Tests
- [x] Unit tests: PASS
- [x] Integration tests: PASS
- [x] Build: SUCCESS

### Code Review
- Verdict: [APPROVED / APPROVED WITH SUGGESTIONS]
- Issues: [None / List]

### Validation Criteria (from PLAN.md)
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- ...

---

**Please validate Phase N:**
- **PASS**: Phase complete, proceed to Phase N+1
- **CONDITIONAL PASS**: Note issues below, proceed with caution
- **FAIL**: Specify issues to fix before proceeding
```

**用户决定**：
- **通过**：继续下一阶段
- **有条件通过**：记录问题，继续下一阶段
- **未通过**：修复问题，重新运行第 2-5 步

---

## 第 6 步：文档更新

**流程**：
1. 使用阶段状态更新 `rpi/{feature-slug}/plan/PLAN.md`
2. 使用验证结果更新 `rpi/{feature-slug}/implement/IMPLEMENT.md`
3. 将每个阶段的验证追加到 `IMPLEMENT.md`

### 阶段状态跟踪

更新 PLAN.md 中的复选框：
```markdown
- [ ] Phase N: Not Started
- [~] Phase N: In Progress
- [x] Phase N: Validated (PASS)
- [!] Phase N: Conditional Pass (with notes)
- [-] Phase N: Failed Validation (needs rework)
```

### IMPLEMENT.md 模板

```markdown
# Implementation Record

**Feature**: [feature-slug]
**Started**: [Date]
**Status**: [IN_PROGRESS / COMPLETED]

---

## Phase 1: [Phase Name]

**Date**: [Date]
**Verdict**: [PASS / CONDITIONAL PASS / FAIL]

### Deliverables
- [x] [Deliverable 1]
- [x] [Deliverable 2]

### Files Changed
[List with line counts]

### Test Results
[Test output summary]

### Code Review
[Review verdict and notes]

### Notes
[Any additional notes]

---

## Phase 2: [Phase Name]
[Same structure as Phase 1...]

---

## Summary

**Phases Completed**: [N] of [N]
**Final Status**: [COMPLETED / IN_PROGRESS]
```

---

## 错误处理

### 实现失败

**如果实现失败**：
1. 记录具体失败信息
2. 分析根本原因
3. 尝试替代方法（最多 2 次尝试）
4. 如果仍然失败，停止并请求用户指导
5. 不要带着有问题的实现进入下一阶段

**消息**："Implementation failed: [error]. Attempted [N] approaches. User guidance needed."

### 测试失败

**如果测试失败**：
1. 分析失败原因（代码错误还是测试错误）
2. 修复问题
3. 重新运行测试
4. 如果持续失败，记录并询问用户
5. 不要在测试失败的情况下标记阶段完成

**消息**："Tests failing: [failures]. Fix attempted but unsuccessful. User review needed."

### 构建失败

**如果构建失败**：
1. 检查类型错误
2. 检查缺失的导入
3. 检查语法错误
4. 修复并重新构建
5. 如果持续失败，升级给用户处理

**消息**："Build failing: [error]. Unable to resolve automatically."

### Agent 失败

**如果 agent 失败或超时**：
1. 使用相同输入重试一次
2. 如果仍然失败，在没有该 agent 贡献的情况下继续
3. 在验证请求中记录差距

**消息**："Agent [name] failed. Proceeding without contribution."

---

## 完成报告

所有阶段成功完成后：

```markdown
## Implementation Complete

### Feature Summary
- **Feature**: [feature-name]
- **Phases Completed**: [N] of [N]

### Phases Executed
| Phase | Status | Notes |
|-------|--------|-------|
| Phase 1 | PASS | [summary] |
| Phase 2 | PASS | [summary] |
| ... | ... | ... |

### Files Modified
| File | Change Type | Lines |
|------|-------------|-------|
| [file] | [type] | [±N] |

### Tests Added
- [test files]

### Code Review Summary
- Blockers Fixed: [N]
- Suggestions Addressed: [N]

### Constitutional Compliance
- [ ] Type safety maintained
- [ ] Tests written
- [ ] Component isolation respected
- [ ] No breaking changes

### Artifacts Created
- `rpi/{feature-slug}/plan/PLAN.md` (updated with phase status)
- `rpi/{feature-slug}/implement/IMPLEMENT.md` (all phase validations)

### Next Steps
1. Create PR with changes
2. Request final human review
3. Deploy to staging
4. Verify in staging environment
5. Deploy to production

### PR Notes

**Title**: [{feature-slug}] [Brief description]

**Summary**:
[What was implemented]

**Changes**:
- [List key changes]

**Testing**:
- [How tested]

**Rollout**:
- [Deployment steps]

**Rollback**:
- [Rollback procedure if issues]
```

---

## 质量门

### 每阶段质量门

在标记任何阶段完成之前：

- [ ] 所有交付物已实现
- [ ] 代码检查通过
- [ ] 测试通过
- [ ] 构建成功
- [ ] 代码审查通过
- [ ] 用户验证已收到
- [ ] 文档已更新

### 最终质量门

在标记实现完成之前：

- [ ] 所有阶段已验证
- [ ] 没有失败的测试
- [ ] 完整构建成功
- [ ] 章程合规已验证
- [ ] 领域规则已遵循
- [ ] PR 说明已生成

---

## 备注

### 何时使用此命令

- 在 `/rpi:plan` 生成 `PLAN.md` 之后
- 当需要带验证门的分阶段实现时
- 对于需要结构化实现的功能

### 何时不使用此命令

- Bug 修复（过于重量级，直接修复即可）
- 非常简单的变更（少于 30 分钟的工作）
- 探索性原型开发
- 仅文档变更

### 最佳实践

1. **先审查 PLAN.md**：了解你要实现的内容
2. **信任代码发现**：让 Explore agent 为实现提供信息
3. **遵循现有模式**：让代码发现指导实现
4. **不要跳过验证**：质量门的存在是为了及早发现问题
5. **边做边记录**：每个阶段后更新状态
6. **遇到困难时询问**：与其错误地继续，不如询问

### 属于 RPI 工作流的一部分

第 4 步，共 4 步（描述 → 调研 → 计划 → **实现**）

---

## 命令示例

### 执行所有阶段

```bash
/rpi:implement "my-feature"
```

### 执行特定阶段

```bash
/rpi:implement "my-feature" --phase 3
```

### 仅验证（不实现）

```bash
/rpi:implement "my-feature" --phase 2 --validate-only
```

---

## 完成后的操作

**重要**：完成实现后（所有阶段或取得重大进展后），始终提示用户压缩对话：

> **上下文管理**：此实现工作流消耗了大量上下文。为保留进度并释放空间，请运行：
>
> ```
> /compact
> ```
>
> 这将总结对话并在减少未来工作的 token 使用量的同时保留实现状态。

**何时提示压缩**：
- 所有阶段完成后
- 完成每个主要阶段后（如果涉及多会话实现）
- 实现过程中上下文即将耗尽时
