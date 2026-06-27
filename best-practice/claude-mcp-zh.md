# MCP Servers 最佳实践

![Last Updated](https://img.shields.io/badge/Last_Updated-Mar%2002%2C%202026%2012%3A30%20PM%20PKT-white?style=flat&labelColor=555)<br>
[![Implemented](https://img.shields.io/badge/Implemented-2ea44f?style=flat)](../.mcp.json)

MCP (Model Context Protocol) server 通过连接外部工具、数据库和 API 来扩展 Claude Code。本指南涵盖推荐的日常使用 server 和配置最佳实践。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## 日常使用的 MCP Server

> *"搞了 15 个 MCP server，以为越多越好。结果每天只用 4 个。"* — [r/mcp](https://reddit.com/r/mcp/comments/1mj0fxs/) (682 赞同)

| MCP Server | 功能 | 资源 |
|------------|-------------|-----------|
| [**Context7**](https://github.com/upstash/context7) | 将最新的 library 文档获取到 context 中。防止过时 training data 导致的幻觉 API | [Reddit: "迄今为止最好的编程 MCP"](https://reddit.com/r/mcp/comments/1qarjqm/) · [npm](https://www.npmjs.com/package/@upstash/context7-mcp) |
| [**Playwright**](https://github.com/microsoft/playwright-mcp) | 浏览器自动化——自主实现、测试和验证 UI 功能。截图、导航、表单测试 | [Reddit: 前端必备](https://reddit.com/r/mcp/comments/1m59pk0/) · [Docs](https://playwright.dev/) |
| [**Claude in Chrome**](https://github.com/nicobailon/claude-code-in-chrome-mcp) | 将 Claude 连接到您的真实 Chrome 浏览器——检查 console、network、DOM。调试用户实际看到的内容 | [Reddit: 调试的 "game changer"](https://reddit.com/r/mcp/comments/1qarjqm/5_mcps_that_have_genuinely_made_me_10x_faster/nza0i7t/) · [对比报告](../reports/claude-in-chrome-v-chrome-devtools-mcp.md) |
| [**DeepWiki**](https://github.com/devanshusemwal/deepwiki-mcp) | 为任何 GitHub repo 获取结构化 wiki 风格文档——architecture、API surface、关系 | [Reddit: "与 Context7 一起放在 gateway 后面"](https://reddit.com/r/mcp/comments/1qarjqm/) |
| [**Excalidraw**](https://github.com/antonpk1/excalidraw-mcp-app) | 从 prompt 生成手绘 Excalidraw 风格的架构图、流程图和系统设计 | [GitHub](https://github.com/antonpk1/excalidraw-mcp-app) |

研究 (Context7/DeepWiki) -> 调试 (Playwright/Chrome) -> 文档 (Excalidraw)

---

## Configuration

MCP server 在项目根目录的 `.mcp.json`（project-scoped）或 `~/.claude.json`（user-scoped）中配置。

### Server 类型

| Type | Transport | Example |
|------|-----------|---------|
| **stdio** | 启动本地进程 | `npx`、`python`、二进制文件 |
| **http** | 连接到远程 URL | HTTP/SSE endpoint |

### Example `.mcp.json`

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp"]
    },
    "deepwiki": {
      "command": "npx",
      "args": ["-y", "deepwiki-mcp"]
    },
    "remote-api": {
      "type": "http",
      "url": "https://mcp.example.com/mcp"
    }
  }
}
```

使用环境变量扩展来管理 secret，而不是在 `.mcp.json` 中提交 API key：

```json
{
  "mcpServers": {
    "remote-api": {
      "type": "http",
      "url": "https://mcp.example.com/mcp?token=${MCP_API_TOKEN}"
    }
  }
}
```

### MCP Server 的 Settings

`.claude/settings.json` 中的这些设置控制 MCP server 的批准：

| Key | Type | Description |
|-----|------|-------------|
| `enableAllProjectMcpServers` | boolean | 自动批准所有 `.mcp.json` server 而无需提示 |
| `enabledMcpjsonServers` | array | 要自动批准的特定 server 名称 allowlist |
| `disabledMcpjsonServers` | array | 要拒绝的特定 server 名称 blocklist |

### MCP 工具的 Permission Rules

MCP 工具在 permission rule 中遵循 `mcp__<server>__<tool>` 的命名约定：

```json
{
  "permissions": {
    "allow": [
      "mcp__*",
      "mcp__context7__*",
      "mcp__playwright__browser_snapshot"
    ],
    "deny": [
      "mcp__dangerous-server__*"
    ]
  }
}
```

---

## MCP Scopes

MCP server 可以在三个级别定义：

| Scope | Location | Purpose |
|-------|----------|---------|
| **Project** | `.mcp.json`（repo 根目录） | 团队共享 server，提交到 git |
| **User** | `~/.claude.json`（`mcpServers` key） | 跨所有项目的个人 server |
| **Subagent** | Agent frontmatter（`mcpServers` 字段） | 作用域限定于特定 subagent 的 server |

优先级：Subagent > Project > User

---

## Sources

- [MCP Servers — Claude Code Docs](https://code.claude.com/docs/en/mcp)
- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [5 MCPs that have genuinely made me 10x faster — r/mcp](https://reddit.com/r/mcp/comments/1qarjqm/)
- [MCP Server Overload Discussion — r/mcp](https://reddit.com/r/mcp/comments/1mj0fxs/)
