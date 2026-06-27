# Claude Code：Spinner 旋转词与提示

Claude Code spinner 下显示的旋转词和提示。提取自 `~/.local/share/claude/versions/2.1.121`。

<table width="100%">
<tr>
<td><a href="../">← 返回 Claude Code 最佳实践</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Spinner 旋转词

| # | Verb | # | Verb | # | Verb | # | Verb |
|---:|---|---:|---|---:|---|---:|---|
| 1 | Accomplishing | 48 | Discombobulating | 95 | Levitating | 142 | Sketching |
| 2 | Actioning | 49 | Doing | 96 | Lollygagging | 143 | Slithering |
| 3 | Actualizing | 50 | Doodling | 97 | Manifesting | 144 | Smooshing |
| 4 | Architecting | 51 | Drizzling | 98 | Marinating | 145 | Sock-hopping |
| 5 | Baking | 52 | Ebbing | 99 | Meandering | 146 | Spelunking |
| 6 | Beaming | 53 | Effecting | 100 | Metamorphosing | 147 | Spinning |
| 7 | Beboppin' | 54 | Elucidating | 101 | Misting | 148 | Sprouting |
| 8 | Befuddling | 55 | Embellishing | 102 | Moonwalking | 149 | Stewing |
| 9 | Billowing | 56 | Enchanting | 103 | Moseying | 150 | Sublimating |
| 10 | Blanching | 57 | Envisioning | 104 | Mulling | 151 | Swirling |
| 11 | Bloviating | 58 | Evaporating | 105 | Mustering | 152 | Swooping |
| 12 | Boogieing | 59 | Fermenting | 106 | Musing | 153 | Symbioting |
| 13 | Boondoggling | 60 | Fiddle-faddling | 107 | Nebulizing | 154 | Synthesizing |
| 14 | Booping | 61 | Finagling | 108 | Nesting | 155 | Tempering |
| 15 | Bootstrapping | 62 | Flambéing | 109 | Newspapering | 156 | Thinking |
| 16 | Brewing | 63 | Flibbertigibbeting | 110 | Noodling | 157 | Thundering |
| 17 | Bunning | 64 | Flowing | 111 | Nucleating | 158 | Tinkering |
| 18 | Burrowing | 65 | Flummoxing | 112 | Orbiting | 159 | Tomfoolering |
| 19 | Calculating | 66 | Fluttering | 113 | Orchestrating | 160 | Topsy-turvying |
| 20 | Canoodling | 67 | Forging | 114 | Osmosing | 161 | Transfiguring |
| 21 | Caramelizing | 68 | Forming | 115 | Perambulating | 162 | Transmuting |
| 22 | Cascading | 69 | Frolicking | 116 | Percolating | 163 | Twisting |
| 23 | Catapulting | 70 | Frosting | 117 | Perusing | 164 | Undulating |
| 24 | Cerebrating | 71 | Gallivanting | 118 | Philosophising | 165 | Unfurling |
| 25 | Channeling | 72 | Galloping | 119 | Photosynthesizing | 166 | Unravelling |
| 26 | Channelling | 73 | Garnishing | 120 | Pollinating | 167 | Vibing |
| 27 | Choreographing | 74 | Generating | 121 | Pondering | 168 | Waddling |
| 28 | Churning | 75 | Gesticulating | 122 | Pontificating | 169 | Wandering |
| 29 | Clauding | 76 | Germinating | 123 | Pouncing | 170 | Warping |
| 30 | Coalescing | 77 | Gitifying | 124 | Precipitating | 171 | Whatchamacalliting |
| 31 | Cogitating | 78 | Grooving | 125 | Prestidigitating | 172 | Whirlpooling |
| 32 | Combobulating | 79 | Gusting | 126 | Processing | 173 | Whirring |
| 33 | Composing | 80 | Harmonizing | 127 | Proofing | 174 | Whisking |
| 34 | Computing | 81 | Hashing | 128 | Propagating | 175 | Wibbling |
| 35 | Concocting | 82 | Hatching | 129 | Puttering | 176 | Working |
| 36 | Considering | 83 | Herding | 130 | Puzzling | 177 | Wrangling |
| 37 | Contemplating | 84 | Honking | 131 | Quantumizing | 178 | Zesting |
| 38 | Cooking | 85 | Hullaballooing | 132 | Razzle-dazzling | 179 | Zigzagging |
| 39 | Crafting | 86 | Hyperspacing | 133 | Razzmatazzing | | |
| 40 | Creating | 87 | Ideating | 134 | Recombobulating | | |
| 41 | Crunching | 88 | Imagining | 135 | Reticulating | | |
| 42 | Crystallizing | 89 | Improvising | 136 | Roosting | | |
| 43 | Cultivating | 90 | Incubating | 137 | Ruminating | | |
| 44 | Deciphering | 91 | Inferring | 138 | Sautéing | | |
| 45 | Deliberating | 92 | Infusing | 139 | Scampering | | |
| 46 | Determining | 93 | Ionizing | 140 | Schlepping | | |
| 47 | Dilly-dallying | 94 | Jitterbugging | 141 | Scurrying | | |

## 提示

| ID | Text |
|---|---|
| new-user-warmup | 从小功能或 bug 修复开始，让 Claude 提出计划，然后验证其建议的编辑 |
| default-permission-mode-config | 使用 /config 更改默认权限模式（包括 Plan Mode） |
| git-worktrees | 使用 git worktree 并行运行多个 Claude session |
| color-when-multi-clauding | 运行多个 Claude session？使用 /color 和 /rename 一目了然地区分它们 |
| memory-command | 使用 /memory 查看和管理 Claude memory |
| theme-command | 使用 /theme 更换颜色主题 |
| colorterm-truecolor | 尝试设置环境变量 COLORTERM=truecolor 获得更丰富的颜色 |
| powershell-tool-env | 设置 CLAUDE_CODE_USE_POWERSHELL_TOOL=1 启用 PowerShell 工具（preview） |
| status-line | 使用 /statusline 设置自定义状态行，显示在输入框下方 |
| prompt-queue | 在 Claude 工作时按 Enter 排队发送额外消息 |
| enter-to-steer-in-relatime | Claude 工作时发送消息以实时引导 Claude |
| todo-list | 处理复杂任务时让 Claude 创建 todo list 来跟踪进度 |
| ide-upsell-external-terminal | 将 Claude 连接到你的 IDE · /ide |
| install-github-app | 运行 /install-github-app 在 GitHub issues 和 PR 中 @claude |
| install-slack-app | 运行 /install-slack-app 在 Slack 中使用 Claude |
| permissions | 使用 /permissions 预批准和预拒绝 bash、edit 和 MCP 工具 |
| drag-and-drop-images | 你知道吗？可以拖拽图片文件到终端 |
| paste-images-mac | 使用 control+v（不是 cmd+v！）将图片粘贴到 Claude Code |
| double-esc | 双击 esc 将对话回退到之前的某个时间点 |
| double-esc-code-restore | 双击 esc 将代码和/或对话回退到之前的某个时间点 |
| continue | 运行 claude --continue 或 claude --resume 恢复对话 |
| rename-conversation | 使用 /rename 为对话命名，方便后续在 /resume 中找到 |
| custom-commands | 通过在项目的 .claude/skills/ 中添加 .md 文件创建 skills，或在 ~/.claude/skills/ 中创建全局 skills |
| custom-agents | 使用 /agents 优化特定任务，如 Software Architect、Code Writer、Code Reviewer |
| agent-flag | 使用 --agent <agent_name> 直接启动与 subagent 的对话 |
| desktop-app | 使用 Claude 桌面应用在本地或远程运行 Claude Code：clau.de/desktop |
| web-app | 在云端运行任务的同时继续本地编码 · clau.de/web |
| voice-mode | 使用 /voice 启用按键说话听写 |
| no-flicker | 尝试无闪烁渲染，现已支持鼠标 · /tui fullscreen |
| team-artifacts | 从 team-discovery 状态中展示团队 artifact 建议 |
| plan-mode-for-complex-tasks | 在进行更改前使用 Plan Mode 为复杂请求做准备。按 &lt;cycle-mode key&gt; |
| terminal-setup | 运行 /terminal-setup 启用便捷的终端集成，如 Option+Enter 换行等 |
| shift-enter | 按 Option+Enter（Apple Terminal）或 Shift+Enter 发送多行消息 |
| shift-enter-setup | 运行 /terminal-setup 启用 Option+Enter（Apple Terminal）或 Shift+Enter 换行 |
| vscode-command-install | 打开命令面板（Cmd+Shift+P）运行 "Shell Command: Install '&lt;editor&gt;' command in PATH" 启用 IDE 集成 |
| shift-tab | 按 &lt;cycle-mode key&gt; 切换聊天模式 |
| image-paste | 使用 &lt;image-paste key&gt; 粘贴图片 |
| desktop-shortcut | 使用 &lt;suggested shortcut&gt; 在 Claude Code Desktop 中继续 session |
| remote-control | 通过 remote control 将此 session 配对到手机 |
| push-notif | 长时间任务完成时在手机上收到通知 — 在 &lt;settings menu&gt; 中启用 push notification |
| opusplan-mode-reminder | 你的默认模型设置为 Opus Plan Mode。按 &lt;cycle-mode key&gt; |
| frontend-design-plugin | 在使用 HTML/CSS？安装 frontend-design plugin |
