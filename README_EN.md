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

## Prompt Overview

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
- [`NotebookEdit.md`](prompts/tools/NotebookEdit.md) - Edit/insert/delete Jupyter notebook cells
- [`Read.md`](prompts/tools/Read.md) - Read files (supports images/PDF/ipynb)
- [`Skill.md`](prompts/tools/Skill.md) - Invoke a skill (slash command)
- [`Task.md`](prompts/tools/Task.md) - Launch subagents for complex multi-step work
- [`TaskOutput.md`](prompts/tools/TaskOutput.md) - Retrieve output for a task/agent/shell
- [`TodoWrite.md`](prompts/tools/TodoWrite.md) - Manage a structured TODO list
- [`WebFetch.md`](prompts/tools/WebFetch.md) - Fetch a URL and extract info with a prompt
- [`WebSearch.md`](prompts/tools/WebSearch.md) - Web search (requires listing Sources)
- [`Write.md`](prompts/tools/Write.md) - Write files to disk (overwrites; Read required for existing files)

## Quick Links

- [`prompts/`](prompts)
- [`prompts/agents/built-in/`](prompts/agents/built-in)
- [`prompts/tools/`](prompts/tools)
