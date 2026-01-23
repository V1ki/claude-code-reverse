---
name: ExitPlanMode
---

Use this tool when you are in plan mode and have finished writing your plan to the plan file and are ready for user approval.

## How This Tool Works
- You should have already written your plan to the plan file specified in the plan mode system message
- This tool does NOT take the plan content as a parameter - it will read the plan from the file you wrote
- This tool simply signals that you're done planning and ready for the user to review and approve
- The user will see the contents of your plan file when they review it

## Requesting Permissions (allowedPrompts)
When calling this tool, you can request prompt-based permissions for bash commands your plan will need. These are semantic descriptions of actions, not literal commands.

**How to use:**
```json
{
  "allowedPrompts": [
    { "tool": "Bash", "prompt": "run tests" },
    { "tool": "Bash", "prompt": "install dependencies" },
    { "tool": "Bash", "prompt": "build the project" }
  ]
}
```

**Guidelines for prompts:**
- Use semantic descriptions that capture the action's purpose, not specific commands
- "run tests" matches: npm test, pytest, go test, bun test, etc.
- "install dependencies" matches: npm install, pip install, cargo build, etc.
- "build the project" matches: npm run build, make, cargo build, etc.
- Keep descriptions concise but descriptive
- Only request permissions you actually need for the plan
- Scope permissions narrowly, like a security-conscious human would:
  - **Never combine multiple actions into one permission** - split them into separate, specific permissions (e.g. "list pods in namespace X", "view logs in namespace X")
  - Prefer "run read-only database queries" over "run database queries"
  - Prefer "run tests in the project" over "run code"
  - Add constraints like "read-only", "local", "non-destructive" whenever possible. If you only need read-only access, you must only request read-only access.
  - Prefer not to request overly broad permissions that would grant dangerous access, especially any access to production data or to make irrecoverable changes
  - When interacting with cloud environments, add constraints like "in the foobar project", "in the baz namespace", "in the foo DB table"
  - Never request broad tool access like "run k8s commands" - always scope to specific actions and namespaces, ideally with constraints such as read-only

**Benefits:**
- Commands matching approved prompts won't require additional permission prompts
- The user sees the requested permissions when approving the plan
- Permissions are session-scoped and cleared when the session ends

## When to Use This Tool
IMPORTANT: Only use this tool when the task requires planning the implementation steps of a task that requires writing code. For research tasks where you're gathering information, searching files, reading files or in general trying to understand the codebase - do NOT use this tool.

## Before Using This Tool
Ensure your plan is complete and unambiguous:
- If you have unresolved questions about requirements or approach, use AskUserQuestion first (in earlier phases)
- Once your plan is finalized, use THIS tool to request approval

**Important:** Do NOT use AskUserQuestion to ask "Is this plan okay?" or "Should I proceed?" - that's exactly what THIS tool does. ExitPlanMode inherently requests user approval of your plan.

## Examples

1. Initial task: "Search for and understand the implementation of vim mode in the codebase" - Do not use the exit plan mode tool because you are not planning the implementation steps of a task.
2. Initial task: "Help me implement yank mode for vim" - Use the exit plan mode tool after you have finished planning the implementation steps of the task.
3. Initial task: "Add a new feature to handle user authentication" - If unsure about auth method (OAuth, JWT, etc.), use AskUserQuestion first, then use exit plan mode tool after clarifying the approach.

---
# Tool Params
- allowedPrompts: array<object>, Prompt-based permissions needed to implement the plan. These describe categories of actions rather than specific commands., optional
  - tool: string, The tool this prompt applies to, required, enum: Bash
  - prompt: string, Semantic description of the action, e.g. "run tests", "install dependencies", required
- pushToRemote: boolean, Whether to push the plan to a remote Claude.ai session, optional
- remoteSessionId: string, The remote session ID if pushed to remote, optional
- remoteSessionUrl: string, The remote session URL if pushed to remote, optional