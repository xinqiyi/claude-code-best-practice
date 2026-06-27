创建一个 agent team 来构建一个时间编排工作流，将当前的迪拜时间显示为可视化的 SVG 卡片。工作流遵循 Command → Agent → Skill 架构模式：

- command 编排流程并处理用户交互
- agent 使用预加载的 skill 获取迪拜的实时当前时间
- skill 从获取的数据创建可视化 SVG 时间卡片

**重要**：所有文件必须创建在 `agent-teams/.claude/` 内——而不是仓库根目录的 `.claude/` 目录。这样 agent team 的输出是自包含的，可通过 `cd agent-teams && claude` 运行。
不要引用或复制现有的天气工作流——从头构建所有内容。

分配以下团队成员：

1. **Command Architect** — 在 `agent-teams/.claude/commands/time-orchestrator.md` 中设计和实现 `/time-orchestrator` command。command 应该：
   - 通过 Agent tool（不是 bash）调用 time-agent 获取迪拜（Asia/Dubai 时区，UTC+4）的当前时间
   - 通过 Skill tool 调用 time-svg-creator skill 从获取的时间数据渲染 SVG 卡片
   - 在 frontmatter 中使用 model: haiku
   - 包含关键要求：顺序流程、正确的工具使用（Agent tool for agents、Skill tool for skills）和输出概要
   - 通过共享 task list 与其他成员协调，就组件间传递的数据契约（{time, timezone, formatted}）达成一致

2. **Agent Engineer** — 在 `agent-teams/.claude/agents/time-agent.md` 中设计和实现 `time-agent`，并在 `agent-teams/.claude/skills/time-fetcher/SKILL.md` 中设计其预加载的 `time-fetcher` skill。agent 应该：
   - 使用 Bash 执行 `TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'` 获取迪拜的当前时间
   - 将时间值、时区名称和格式化字符串返回给 command
   - 使用 frontmatter：tools (Bash)、model: haiku、color: blue、maxTurns: 3
   - 通过 `skills:` 字段预加载 time-fetcher skill
   - time-fetcher skill 应包含迪拜时间的 bash 命令、预期输出格式，并设置 user-invocable: false
   - 将商定的数据契约发布到共享 task list

3. **Skill Designer** — 在 `agent-teams/.claude/skills/time-svg-creator/SKILL.md` 中设计和实现 `time-svg-creator` skill，并附带 `reference.md`（SVG 模板 + 输出模板）和 `examples.md`（示例输入/输出对）。skill 应该：
   - 从调用 context 接收时间值、时区和格式化字符串
   - 为迪拜创建自包含的 SVG 时间卡片，显示当前时间
   - 将 SVG 写入 `agent-teams/output/dubai-time.svg`
   - 将 markdown 概要写入 `agent-teams/output/output.md`
   - 使用提供的确切时间——从不重新获取
   - 模板保留在 reference.md 中

所有三个成员应在共享 task list 中创建任务来协调数据契约：agent 返回 {time, timezone, formatted}，command 通过 context 传递它，skill 消费它。让三个成员并行启动，因为组件是独立的——它们只需要就数据接口达成一致，不需要等待对方的实现。
