# Linux 安装设置

[返回 Day 0](README.md)

## 前提条件

你需要 **Node.js v18 或更高版本**以及 **npm**。

## 第 1 步：安装 Node.js

### 选项 A：通过 nodejs.org 下载页面使用 fnm（推荐）

**fnm**（Fast Node Manager）是 Node.js 官方推荐的工具。它快速、轻量，如果日后需要，可以轻松切换 Node 版本。

1. 打开浏览器并访问 [nodejs.org/en/download](https://nodejs.org/en/download)

2. 你会看到一行下拉菜单："Get Node.js® vXX.XX.X (LTS) for __ using __ with __"。按下述设置下拉菜单：

   | 下拉菜单 | 选择 |
   |----------|------|
   | Version | **vXX.XX.X (LTS)** — 保持默认 LTS 版本 |
   | OS | **Linux** |
   | Package Manager | **fnm**（在"Recommended (Official)"下） |
   | Package Format | **npm** — 保持默认 |

3. 页面会显示要运行的确切命令。打开终端并复制粘贴：

   ```bash
   # 第 1 步 — 安装 fnm
   curl -fsSL https://fnm.vercel.app/install | bash

   # 第 2 步 — 重启终端或重新加载 shell profile
   source ~/.bashrc   # 或：source ~/.zshrc（如果你使用 zsh）

   # 第 3 步 — 安装 Node.js
   fnm install 24   # 页面会显示确切版本号
   ```

4. **关闭并重新打开终端**（或运行上面的 `source` 命令），以便 `fnm`、`node` 和 `npm` 可用。

### 选项 B：使用发行版的包管理器

速度更快但可能安装较旧版本的 Node.js。**安装后检查版本**——如果低于 v18，请改用选项 A。

**Ubuntu / Debian：**

```bash
sudo apt update
sudo apt install -y nodejs npm
node --version   # 必须是 v18 或更高
```

**Fedora：**

```bash
sudo dnf install -y nodejs npm
```

**Arch Linux：**

```bash
sudo pacman -S nodejs npm
```

### 选项 C：NodeSource（通过 apt 获取最新 LTS）

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

## 第 2 步：验证 Node.js

```bash
node --version
npm --version
```

两者都应打印版本号。`node --version` 必须显示 v18.x 或更高。

## 第 3 步：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

**权限错误？**
- 如果使用 fnm 或 nvm：不应出现此问题。检查它是否已激活
- 如果使用系统安装：使用 `sudo npm install -g @anthropic-ai/claude-code` 或修复 npm 的全局目录权限

## 第 4 步：验证 Claude Code

```bash
claude --version
```

应显示 Claude Code 版本号。现在返回 [README.md](README.md) 进行认证设置。
