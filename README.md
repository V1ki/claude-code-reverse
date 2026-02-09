# Claude Code Reverse Engineering

中文 | [English](README_EN.md)

这个项目用于记录我对 Claude Code 的逆向分析，包括其提示词（agents）、工具（tools）以及工作流程相关设计。

## 目录

- [目录层级](#目录层级)
- [提示词概览](#提示词概览)
- [快速链接](#快速链接)

## 目录层级

```text
.
├── README.md
├── README_EN.md
└── prompts
    ├── agents
    │   ├── CreateAgent.md
    │   ├── built-in
    │   │   ├── Explore.md
    │   │   ├── Plan.md
    │   │   ├── claude-code-guide.md
    │   │   ├── general-purpose.md
    │   │   └── statusline-setup.md
    │   └── system
    │       ├── DetectionNewTopic.md
    │       ├── Suggestion.md
    │       └── default.md
    └── tools
        ├── AskUserQuestion.md
        ├── Bash.md
        ├── Edit.md
        ├── EnterPlanMode.md
        ├── ExitPlanMode.md
        ├── Glob.md
        ├── Grep.md
        ├── KillShell.md
        ├── ListMcpResourcesTool.md
        ├── NotebookEdit.md
        ├── Read.md
        ├── ReadMcpResourceTool.md
        ├── SendMessage.md
        ├── Skill.md
        ├── Task.md
        ├── TaskCreate.md
        ├── TaskGet.md
        ├── TaskList.md
        ├── TaskOutput.md
        ├── TaskStop.md
        ├── TaskUpdate.md
        ├── TeamCreate.md
        ├── TeamDelete.md
        ├── WebFetch.md
        ├── WebSearch.md
        └── Write.md

```

## 提示词概览

### Agents (`prompts/agents/`)

- [`CreateAgent.md`](prompts/agents/CreateAgent.md)：在 `/agent` 中创建自定义 agent 时使用的生成器提示词（输出 `identifier` / `whenToUse` / `systemPrompt`）

### System Agents (`prompts/agents/system/`)

- [`DetectionNewTopic.md`](prompts/agents/system/DetectionNewTopic.md)：在 `default.md` 触发前调用，判断用户消息是否进入新话题；若是则提取 2-3 词标题用于动态更新终端标题（`CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true` 时不设置）
- [`Suggestion.md`](prompts/agents/system/Suggestion.md)：在 Prompt Suggestion 打开时启用，预测用户下一步最可能输入的 2-12 词短建议（仅输出建议文本，可为空）
- [`default.md`](prompts/agents/system/default.md)：正常对话时触发的默认系统提示词与行为准则，可视为主 agent 入口

### Built-in Agents (`prompts/agents/built-in/`)

- [`Explore.md`](prompts/agents/built-in/Explore.md)：只读探索/搜索代码库的快速 agent
- [`Plan.md`](prompts/agents/built-in/Plan.md)：只读规划 agent，用于产出实现方案与关键文件
- [`general-purpose.md`](prompts/agents/built-in/general-purpose.md)：通用 agent，用于多步执行与综合研究（tools: *）
- [`statusline-setup.md`](prompts/agents/built-in/statusline-setup.md)：状态栏（statusLine）配置向导（Read/Edit）
- [`claude-code-guide.md`](prompts/agents/built-in/claude-code-guide.md)：Claude Code / Agent SDK / API 文档向导（含 WebFetch/WebSearch）

### Tools (`prompts/tools/`)

- [`AskUserQuestion.md`](prompts/tools/AskUserQuestion.md)：执行过程中向用户提问以澄清需求/偏好
- [`Bash.md`](prompts/tools/Bash.md)：在持久 shell 中执行命令（更偏 git/npm/docker 等）
- [`Edit.md`](prompts/tools/Edit.md)：对文件进行精确字符串替换（需先 Read）
- [`EnterPlanMode.md`](prompts/tools/EnterPlanMode.md)：进入 plan mode，先拿到方案确认再写代码
- [`ExitPlanMode.md`](prompts/tools/ExitPlanMode.md)：退出 plan mode 并提交方案给用户审批
- [`Glob.md`](prompts/tools/Glob.md)：基于 glob 的文件匹配
- [`Grep.md`](prompts/tools/Grep.md)：基于 ripgrep 的内容搜索
- [`KillShell.md`](prompts/tools/KillShell.md)：终止后台 bash shell
- [`ListMcpResourcesTool.md`](prompts/tools/ListMcpResourcesTool.md)：列出 MCP server 的可用资源（可按 server 过滤）
- [`NotebookEdit.md`](prompts/tools/NotebookEdit.md)：编辑/插入/删除 Jupyter notebook cell
- [`Read.md`](prompts/tools/Read.md)：读取文件内容（支持图片/PDF/ipynb）
- [`ReadMcpResourceTool.md`](prompts/tools/ReadMcpResourceTool.md)：按 `server` + `uri` 读取 MCP 资源
- [`SendMessage.md`](prompts/tools/SendMessage.md)：向团队内 agent 发送消息（单播/广播），并处理 shutdown 与 plan approval 请求响应（需启用 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS`）
- [`Skill.md`](prompts/tools/Skill.md)：执行 skill（slash command，如 `/commit`）
- [`Task.md`](prompts/tools/Task.md)：启动子 agent 执行复杂多步任务
- [`TaskCreate.md`](prompts/tools/TaskCreate.md)：创建结构化任务项（task list）
- [`TaskGet.md`](prompts/tools/TaskGet.md)：按任务 ID 获取完整任务详情
- [`TaskList.md`](prompts/tools/TaskList.md)：列出当前任务列表与状态摘要
- [`TaskOutput.md`](prompts/tools/TaskOutput.md)：获取后台 task/agent/shell 的输出
- [`TaskStop.md`](prompts/tools/TaskStop.md)：停止运行中的后台 task
- [`TaskUpdate.md`](prompts/tools/TaskUpdate.md)：更新任务状态、依赖、owner 与元数据
- [`TeamCreate.md`](prompts/tools/TeamCreate.md)：创建 agent 团队及其对应 task list，用于多 agent 协作分工（需启用 `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS`）
- [`TeamDelete.md`](prompts/tools/TeamDelete.md)：在团队协作完成后清理 team/task 目录并移除当前会话团队上下文（需先关闭全部活跃 teammate）
- [`WebFetch.md`](prompts/tools/WebFetch.md)：抓取 URL 内容并按提示抽取信息
- [`WebSearch.md`](prompts/tools/WebSearch.md)：Web 搜索（要求在答复末尾列出 Sources）
- [`Write.md`](prompts/tools/Write.md)：写入文件（覆盖写，写已存在文件前需先 Read）

## 快速链接

- [`prompts/`](prompts)
- [`prompts/agents/built-in/`](prompts/agents/built-in)
- [`prompts/agents/system/`](prompts/agents/system)
- [`prompts/tools/`](prompts/tools)
