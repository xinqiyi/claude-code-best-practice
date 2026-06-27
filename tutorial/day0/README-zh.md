# Day 0 — Claude Code 安装设置

本指南将引导你在机器上安装 Claude Code 并进行认证，以便开始使用。

## 第 1 步：安装 Claude Code

选择你的操作系统：

| OS | Guide |
|----|-------|
| Windows | [windows.md](windows.md) |
| Linux | [linux.md](linux.md) |
| macOS | [mac.md](mac.md) |

按照你操作系统的指南操作，然后返回此处进行认证。

---

## 第 2 步：验证安装

按照特定操作系统的指南操作后，确认一切工作正常：

```bash
node --version    # 应显示 v18.x 或更高版本
claude --version  # 应显示已安装的 Claude Code 版本
```

---

## 第 3 步：登录

<img src="assets/login.png" alt="Claude Code 登录界面" width="50%">

在终端中运行 `claude`。首次启动时，它会要求你选择登录方式。

### 方式 1：订阅（Claude Pro / Max）

- 选择 **Claude.ai account**
- 浏览器打开——登录并授权
- 返回终端，你已登录成功

### 方式 2a：API Key（团队邀请）

你的团队管理员从 Anthropic dashboard 邀请你。

- 你收到一封**邀请邮件**——接受邀请并创建你的 Anthropic 账户
- 在终端中运行 `claude`
- 选择 **Anthropic API Key**
- 你的 key 在 dashboard 上**自动生成**——无需手动设置
- Claude Code 立即开始工作

### 方式 2b：API Key（你已有 key）

如果有人通过 Slack、邮件等方式与你共享了 key，或你自己创建了 key：

- 在终端中运行 `claude`
- 选择 **Anthropic API Key**
- 粘贴你的 key（以 `sk-ant-` 开头）
- 该 key 将被**永久存储**——之后不会再询问
