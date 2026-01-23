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
    │   └── built-in
    │       ├── Explore.md
    │       ├── Plan.md
    │       ├── claude-code-guide.md
    │       ├── general-purpose.md
    │       └── statusline-setup.md
    └── tools
        ├── AskUserQuestion.md
        ├── Bash.md
        ├── Edit.md
        ├── EnterPlanMode.md
        ├── ExitPlanMode.md
        ├── Glob.md
        ├── Grep.md
        ├── KillShell.md
        ├── NotebookEdit.md
        ├── Read.md
        ├── Skill.md
        ├── Task.md
        ├── TaskOutput.md
        ├── TodoWrite.md
        ├── WebFetch.md
        ├── WebSearch.md
        └── Write.md

```

## 提示词概览

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
- [`NotebookEdit.md`](prompts/tools/NotebookEdit.md)：编辑/插入/删除 Jupyter notebook cell
- [`Read.md`](prompts/tools/Read.md)：读取文件内容（支持图片/PDF/ipynb）
- [`Skill.md`](prompts/tools/Skill.md)：执行 skill（slash command，如 `/commit`）
- [`Task.md`](prompts/tools/Task.md)：启动子 agent 执行复杂多步任务
- [`TaskOutput.md`](prompts/tools/TaskOutput.md)：获取后台 task/agent/shell 的输出
- [`TodoWrite.md`](prompts/tools/TodoWrite.md)：维护结构化 TODO 列表
- [`WebFetch.md`](prompts/tools/WebFetch.md)：抓取 URL 内容并按提示抽取信息
- [`WebSearch.md`](prompts/tools/WebSearch.md)：Web 搜索（要求在答复末尾列出 Sources）
- [`Write.md`](prompts/tools/Write.md)：写入文件（覆盖写，写已存在文件前需先 Read）

## 快速链接

- [`prompts/`](prompts)
- [`prompts/agents/built-in/`](prompts/agents/built-in)
- [`prompts/tools/`](prompts/tools)
