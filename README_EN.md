# Claude Code Reverse Engineering

English | [中文](README.md)

This repository records my reverse-engineering notes on Claude Code, including built-in agent prompts, tool prompts, and workflow conventions.

## Contents

- [Repository Structure](#repository-structure)
- [Prompt Overview](#prompt-overview)
- [Quick Links](#quick-links)

## Repository Structure

```text
.
├── README.md
├── README_EN.md
└── prompts
    ├── agents
    │   ├── CreateAgent.md
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
        ├── ListMcpResourcesTool.md
        ├── NotebookEdit.md
        ├── Read.md
        ├── ReadMcpResourceTool.md
        ├── Skill.md
        ├── Task.md
        ├── TaskCreate.md
        ├── TaskGet.md
        ├── TaskList.md
        ├── TaskOutput.md
        ├── TaskStop.md
        ├── TaskUpdate.md
        ├── WebFetch.md
        ├── WebSearch.md
        └── Write.md
```

## Prompt Overview

### Agents (`prompts/agents/`)

- [`CreateAgent.md`](prompts/agents/CreateAgent.md) - Generator prompt used when creating a custom agent from `/agent` (outputs `identifier` / `whenToUse` / `systemPrompt`)

### Built-in Agents (`prompts/agents/built-in/`)

- [`Explore.md`](prompts/agents/built-in/Explore.md) - Read-only codebase exploration/search agent
- [`Plan.md`](prompts/agents/built-in/Plan.md) - Read-only planning agent for implementation strategy and critical files
- [`general-purpose.md`](prompts/agents/built-in/general-purpose.md) - General-purpose agent for multi-step execution and research (tools: `*`)
- [`statusline-setup.md`](prompts/agents/built-in/statusline-setup.md) - Status line (`statusLine`) setup guide (Read/Edit)
- [`claude-code-guide.md`](prompts/agents/built-in/claude-code-guide.md) - Docs guide for Claude Code / Agent SDK / API (includes WebFetch/WebSearch)

### Tools (`prompts/tools/`)

- [`AskUserQuestion.md`](prompts/tools/AskUserQuestion.md) - Ask the user clarifying questions during execution
- [`Bash.md`](prompts/tools/Bash.md) - Run commands in a persistent shell session
- [`Edit.md`](prompts/tools/Edit.md) - Exact string replacements in files (requires Read first)
- [`EnterPlanMode.md`](prompts/tools/EnterPlanMode.md) - Enter plan mode to get approach approval
- [`ExitPlanMode.md`](prompts/tools/ExitPlanMode.md) - Exit plan mode and request approval
- [`Glob.md`](prompts/tools/Glob.md) - Fast file pattern matching via glob
- [`Grep.md`](prompts/tools/Grep.md) - Content search powered by ripgrep
- [`KillShell.md`](prompts/tools/KillShell.md) - Terminate a background shell session
- [`ListMcpResourcesTool.md`](prompts/tools/ListMcpResourcesTool.md) - List available resources from configured MCP servers (optionally filtered by server)
- [`NotebookEdit.md`](prompts/tools/NotebookEdit.md) - Edit/insert/delete Jupyter notebook cells
- [`Read.md`](prompts/tools/Read.md) - Read files (supports images/PDF/ipynb)
- [`ReadMcpResourceTool.md`](prompts/tools/ReadMcpResourceTool.md) - Read a specific MCP resource by `server` and `uri`
- [`Skill.md`](prompts/tools/Skill.md) - Invoke a skill (slash command)
- [`Task.md`](prompts/tools/Task.md) - Launch subagents for complex multi-step work
- [`TaskCreate.md`](prompts/tools/TaskCreate.md) - Create structured tasks in the task list
- [`TaskGet.md`](prompts/tools/TaskGet.md) - Retrieve full task details by task ID
- [`TaskList.md`](prompts/tools/TaskList.md) - List task summaries and statuses
- [`TaskOutput.md`](prompts/tools/TaskOutput.md) - Retrieve output for a task/agent/shell
- [`TaskStop.md`](prompts/tools/TaskStop.md) - Stop a running background task
- [`TaskUpdate.md`](prompts/tools/TaskUpdate.md) - Update task status, dependencies, owner, and metadata
- [`WebFetch.md`](prompts/tools/WebFetch.md) - Fetch a URL and extract info with a prompt
- [`WebSearch.md`](prompts/tools/WebSearch.md) - Web search (requires listing Sources)
- [`Write.md`](prompts/tools/Write.md) - Write files to disk (overwrites; Read required for existing files)

## Quick Links

- [`prompts/`](prompts)
- [`prompts/agents/built-in/`](prompts/agents/built-in)
- [`prompts/tools/`](prompts/tools)
