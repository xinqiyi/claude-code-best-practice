---
description: 为功能创建全面的规划文档
argument-hint: "<feature-slug>"
---

## 用户输入

```text
$ARGUMENTS
```

你必须解析用户输入以提取功能 slug（`rpi/` 中的文件夹名称）。

## 目的

此命令为功能请求创建全面的规划文档。在功能的 RPI 文件夹中生成详细的规范、技术设计和实施计划。

**前置条件**：
- 功能文件夹存在于 `rpi/{feature-slug}/`
- 研究已完成并建议 GO（`rpi/{feature-slug}/research/RESEARCH.md` 存在）

**输出位置**：所有文件保存到 `rpi/{feature-slug}/plan/`

**这是 RPI 工作流的第 3 步**（在研究批准 GO 之后）。

## 工作流概述

### 第 0 阶段：加载 Context
- 验证研究已完成
- 读取研究发现
- 加载项目 constitution（如果存在）

### 第 1 阶段：理解功能需求
- 解析功能描述
- 识别受影响的组件
- 研究现有模式

### 第 2 阶段：分析技术需求
- 审查组件架构
- 识别技术依赖
- 评估集成点
- 评估技术风险

### 第 3 阶段：设计功能架构
- **Agent**：senior-software-engineer
- 设计高層架构
- 定义实施方法
- 规划数据库/存储变更
- 设计 API 契约
- 规划测试策略

### 第 4 阶段：分解实施任务
- 识别实施阶段（3-5 个逻辑阶段）
- 为每个阶段创建任务分解
- 定义成功标准
- 识别并行化机会

### 第 5 阶段：生成文档

使用 documentation-analyst-writer 生成 4 个文件：

| File | Content | Agent |
|------|---------|-------|
| `pm.md` | 产品需求和用户故事 | product-manager |
| `ux.md` | UX 设计 | ux-designer |
| `eng.md` | 技术规范 | senior-software-engineer |
| `PLAN.md` | 分阶段实施路线图 | documentation-analyst-writer |

### 完成报告

成功完成后报告功能名称、目标组件、复杂性和实施阶段摘要。

## 后续步骤

1. 审查文档
2. 与利益相关者验证
3. 运行 `/rpi:implement "{feature-slug}"` 开始实施

## 最佳实践

1. **先审查研究**：确保了解可行性评估
2. **利用发现**：使用研究阶段的技术发现
3. **具体化**：详细计划带来更顺畅的实施
4. **尽早验证**：在实施之前审查文档
