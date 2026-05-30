# Day 0 — Claude Code 设置

本指南将引导你在自己的机器上安装 Claude Code 并完成认证，以便你可以开始使用它。

## 步骤 1：安装 Claude Code

选择你的操作系统：

| 操作系统 | 指南 |
|----|-------|
| Windows | [windows.md](windows.md) |
| Linux | [linux.md](linux.md) |
| macOS | [mac.md](mac.md) |

按照适用于你操作系统的指南操作，然后返回此处进行认证。

---

## 步骤 2：验证安装

按照特定于你操作系统的指南操作后，确认一切正常工作：

```bash
node --version    # Should show v18.x or higher
claude --version  # Should show the installed Claude Code version
```

---

## 步骤 3：登录

<img src="assets/login.png" alt="Claude Code 登录界面" width="50%">

在终端中运行 `claude`。首次启动时，它会要求你选择一种登录方法。

### 方式 1：订阅（Claude Pro / Max）

- 选择 **Claude.ai 账户**
- 浏览器打开 — 登录并授权
- 返回终端，即已登录

### 方式 2a：API Key（团队邀请）

你的团队管理员会从 Anthropic 控制台邀请你。

- 你会收到一封**邀请邮件** — 接受邀请并创建你的 Anthropic 账户
- 在终端中运行 `claude`
- 选择 **Anthropic API Key**
- 你的密钥会在控制台上**自动生成** — 无需手动设置
- Claude Code 立即开始工作

### 方式 2b：API Key（已有密钥）

如果有人与你分享了密钥（通过 Slack、电子邮件等）或你自己创建了密钥：

- 在终端中运行 `claude`
- 选择 **Anthropic API Key**
- 粘贴你的密钥（以 `sk-ant-` 开头）
- 密钥会被**永久存储** — 你不会再被询问

---
