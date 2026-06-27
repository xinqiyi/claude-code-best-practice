# RPI 工作流

**RPI** = **R**esearch（研究）→ **P**lan（计划）→ **I**mplement（实施）

一个系统性的开发工作流，每个阶段都有验证关卡。防止在不可行的功能上浪费精力，并确保全面的文档。

<table width="100%">
<tr>
<td><a href="../../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 概述

![RPI Workflow](rpi-workflow.svg)

---

## 安装

将 `.claude` 文件夹（包含 `agents/` 和 `commands/rpi/`）复制到你的仓库根目录，然后创建 `rpi/plans` 目录。

---

## 示例工作流

### 功能：用户认证

**第 1 步：Describe**
```
User: "添加 OAuth2 认证，支持 Google 和 GitHub 提供方"

1. Claude 生成计划
   → 输出：rpi/plans/oauth2-authentication.md
2. 创建功能文件夹：rpi/oauth2-authentication/
3. 将计划复制到功能文件夹
4. 将计划重命名为 REQUEST.md
   → 最终：rpi/oauth2-authentication/REQUEST.md
```

**第 2 步：Research**
```bash
/rpi:research rpi/oauth2-authentication/REQUEST.md
```
输出：
- `research/RESEARCH.md` 包含分析
- 结论：**GO**（可行，符合战略）

**第 3 步：Plan**
```bash
/rpi:plan oauth2-authentication
```
输出：
- `plan/pm.md` — 用户故事和验收标准
- `plan/ux.md` — 登录 UI 流程
- `plan/eng.md` — 技术架构
- `plan/PLAN.md` — 3 个阶段，15 个任务

**第 4 步：Implement**
```bash
/rpi:implement oauth2-authentication
```
进度：
- Phase 1：后端基础 → PASS
- Phase 2：前端集成 → PASS
- Phase 3：测试和打磨 → PASS

结果：功能完成，准备 PR。

---

## 功能文件夹结构

所有功能工作位于 `rpi/{feature-slug}/`：

```
rpi/{feature-slug}/
├── REQUEST.md              # 第 1 步：初始功能描述
├── research/
│   └── RESEARCH.md         # 第 2 步：GO/NO-GO 分析
├── plan/
│   ├── PLAN.md             # 第 3 步：实施路线图
│   ├── pm.md               # 产品需求
│   ├── ux.md               # UX 设计
│   └── eng.md              # 技术规范
└── implement/
    └── IMPLEMENT.md        # 第 4 步：实施记录
```

---

## Agents 和 Commands

| Command | Agents Used |
|---------|-------------|
| `/rpi:research` | requirement-parser, product-manager, Explore, senior-software-engineer, technical-cto-advisor, documentation-analyst-writer |
| `/rpi:plan` | senior-software-engineer, product-manager, ux-designer, documentation-analyst-writer |
| `/rpi:implement` | Explore, senior-software-engineer, code-reviewer |
