创建一个 agent team 来构建一个时间编排工作流，将当前的迪拜时间显示为可视化 SVG 卡片。该工作流遵循 Command → Agent → Skill 架构模式：

- command 负责编排流程并处理用户交互
- agent 使用预加载的 skill 获取迪拜的实时当前时间
- skill 根据获取到的数据创建可视化 SVG 时间卡片

**重要**：所有文件必须创建在 `agent-teams/.claude/` 目录下，而不是仓库根目录的 `.claude/` 目录中。这样可以使 agent team 的输出自成一体，并可通过 `cd agent-teams && claude` 直接运行。
请勿参考或复制现有的 weather 工作流——所有内容从头构建。

分配以下团队成员：

1. **Command Architect** — 在 `agent-teams/.claude/commands/time-orchestrator.md` 中设计并实现 `/time-orchestrator` command。该 command 应：
   - 通过 Agent 工具（而非 bash）调用 time-agent 获取阿联酋迪拜的当前时间（Asia/Dubai 时区，UTC+4）
   - 通过 Skill 工具调用 time-svg-creator skill 根据获取到的时间数据渲染 SVG 卡片
   - 在 frontmatter 中使用 model: haiku
   - 包含关键需求：顺序执行流程、正确的工具使用方式（agent 使用 Agent 工具，skill 使用 Skill 工具）以及输出摘要
   - 通过共享任务列表与其他团队成员协调，就组件间传递的数据契约 ({time, timezone, formatted}) 达成一致

2. **Agent Engineer** — 在 `agent-teams/.claude/agents/time-agent.md` 中设计并实现 `time-agent`，及其预加载的 `time-fetcher` skill（位于 `agent-teams/.claude/skills/time-fetcher/SKILL.md`）。该 agent 应：
   - 使用 Bash 命令 `TZ='Asia/Dubai' date '+%Y-%m-%d %H:%M:%S %Z'` 获取迪拜当前时间（Asia/Dubai，UTC+4）
   - 将时间值、时区名称和格式化字符串返回给 command
   - 使用 frontmatter：tools (Bash)、model: haiku、color: blue、maxTurns: 3
   - 通过 `skills:` 字段预加载 time-fetcher skill
   - time-fetcher skill（`agent-teams/.claude/skills/time-fetcher/SKILL.md`）应包含用于获取迪拜时间的 bash 命令、预期的输出格式，并设置 user-invocable: false，因为它仅作为 agent 的领域知识。
   - 将商定的数据契约发布到共享任务列表，以便 Command Architect 和 Skill Designer 能够就接口达成一致。

3. **Skill Designer** — 在 `agent-teams/.claude/skills/time-svg-creator/SKILL.md` 中设计并实现 `time-svg-creator` skill，以及配套文件 `reference.md`（SVG 模板 + 输出模板）和 `examples.md`（示例输入/输出对）。该 skill 应：
   - 从调用上下文中接收时间值、时区和格式化字符串
   - 创建一个显示当前时间的迪拜自包含 SVG 时间卡片
   - 将 SVG 写入 `agent-teams/output/dubai-time.svg`
   - 将 markdown 摘要写入 `agent-teams/output/output.md`
   - 使用提供的确切时间——绝不重新获取
   - 将模板保存在 reference.md 中（带占位符的 SVG 标记、markdown 输出模板），示例对保存在 examples.md 中
   - 同时创建 `agent-teams/output/` 目录用于存放输出文件。

所有三位团队成员应在共享任务列表中创建任务，以协调数据契约：agent 返回 {time, timezone, formatted}，command 通过 context 传递它，skill 消费它。
三个任务并行启动，因为各组件是独立的——它们只需就数据接口达成一致，无需等待彼此的实现。
