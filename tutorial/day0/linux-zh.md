# Linux 环境配置

[返回第 0 天](README.md)

## 前置要求

你需要 **Node.js v18 或更高版本** 以及 **npm**。

## 第 1 步：安装 Node.js

### 选项 A：通过 nodejs.org 下载页面配合 fnm（推荐）

**fnm**（Fast Node Manager）是 Node.js 官方推荐的工具。它快速、轻量，并且如果以后需要，可以轻松切换 Node 版本。

1. 打开浏览器，访问 [nodejs.org/en/download](https://nodejs.org/en/download)。

2. 你会看到一行下拉菜单，显示：**"Get Node.js® vXX.XX.X (LTS) for __ using __ with __"**。按下述方式设置下拉菜单：

   | 下拉菜单 | 选择 |
   |----------|------|
   | Version | **vXX.XX.X (LTS)** — 保持默认的 LTS 版本，不要更改 |
   | OS | **Linux** |
   | Package Manager | **fnm**（在 "Recommended (Official)" 下） |
   | Package Format | **npm** — 保持默认值 |

3. 页面会显示你要运行的确切命令。打开终端并复制粘贴它们。内容大致如下：

   ```bash
   # Step 1 — Install fnm
   curl -fsSL https://fnm.vercel.app/install | bash

   # Step 2 — Restart your terminal or reload your shell profile
   source ~/.bashrc   # or: source ~/.zshrc (if you use zsh)

   # Step 3 — Install Node.js
   fnm install 24   # The page will show the exact version number
   ```

   > 版本号可能与上面不同 —— 始终使用网站显示的具体版本。

4. **关闭并重新打开终端**（或运行上述 `source` 命令），以便 `fnm`、`node` 和 `npm` 可用。

> **为什么选择 fnm？** 它位于 Node.js 下载页面的 "Recommended (Official)" 类别中。与 nvm 一样，它将 Node 安装到你的主目录中，这样你就不需要为 npm 全局安装使用 `sudo` —— 但 fnm 速度明显更快（用 Rust 编写），并且在 Windows、macOS 和 Linux 上工作方式相同。

### 选项 B：使用你的发行版包管理器

这样更快，但可能安装较旧版本的 Node.js。**安装后检查版本** —— 如果低于 v18，请改用选项 A。

**Ubuntu / Debian：**

```bash
sudo apt update
sudo apt install -y nodejs npm

# Check the version
node --version   # Must be v18 or higher
```

**Fedora：**

```bash
sudo dnf install -y nodejs npm
```

**Arch Linux：**

```bash
sudo pacman -S nodejs npm
```

### 选项 C：NodeSource（通过 apt 获取最新 LTS，无需 nvm）

适用于希望在不使用 nvm 的情况下获取最新 LTS 的 Ubuntu/Debian 用户：

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

## 第 2 步：验证 Node.js

```bash
node --version
npm --version
```

两者都应打印版本号。`node --version` 必须显示 v18.x 或更高版本。

## 第 3 步：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

> **权限错误？**
> - 如果你使用了 **fnm** 或 **nvm**：这不应该发生。检查它是否已激活（`which node` 应指向主目录内的路径，而不是 `/usr/...`）。
> - 如果你使用了系统安装：要么使用 `sudo npm install -g @anthropic-ai/claude-code`，要么修复 npm 的全局目录权限：
>   ```bash
>   mkdir -p ~/.npm-global
>   npm config set prefix '~/.npm-global'
>   echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
>   source ~/.bashrc
>   ```

## 第 4 步：验证 Claude Code

```bash
claude --version
```

你应该能看到 Claude Code 版本号的输出。现在返回 [README.md](README.md) 进行身份验证设置。

---

## 说明

- **WSL（Windows Subsystem for Linux）：** 本指南同样适用于 WSL。只需在你的 WSL 终端中按照这些步骤操作即可。
- **PATH 问题：** 如果安装后找不到 `claude`，请确保 npm 的全局 bin 目录在你的 PATH 中。运行 `npm config get prefix` —— 该路径下的 `bin/` 子目录需要位于你的 PATH 中。
