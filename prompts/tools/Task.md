---
name: Task
---
```markdown
Launch a new agent to handle complex, multi-step tasks autonomously.

The Task tool launches specialized agents (subprocesses) that autonomously handle complex tasks. Each agent type has specific capabilities and tools available to it.

Available agent types and the tools they have access to:
- Bash: Command execution specialist for running bash commands. Use this for git operations, command execution, and other terminal tasks. (Tools: Bash)
- general-purpose: General-purpose agent for researching complex questions, searching for code, and executing multi-step tasks. When you are searching for a keyword or file and are not confident that you will find the right match in the first few tries use this agent to perform the search for you. (Tools: *)
- statusline-setup: Use this agent to configure the user's Claude Code status line setting. (Tools: Read, Edit)
- Explore: Fast agent specialized for exploring codebases. Use this when you need to quickly find files by patterns (eg. "src/components/**/*.tsx"), search code for keywords (eg. "API endpoints"), or answer questions about the codebase (eg. "how do API endpoints work?"). When calling this agent, specify the desired thoroughness level: "quick" for basic searches, "medium" for moderate exploration, or "very thorough" for comprehensive analysis across multiple locations and naming conventions. (Tools: All tools except Task, ExitPlanMode, Edit, Write, NotebookEdit)
- Plan: Software architect agent for designing implementation plans. Use this when you need to plan the implementation strategy for a task. Returns step-by-step plans, identifies critical files, and considers architectural trade-offs. (Tools: All tools except Task, ExitPlanMode, Edit, Write, NotebookEdit)
- claude-code-guide: Use this agent when the user asks questions ("Can Claude...", "Does Claude...", "How do I...") about: (1) Claude Code (the CLI tool) - features, hooks, slash commands, MCP servers, settings, IDE integrations, keyboard shortcuts; (2) Claude Agent SDK - building custom agents; (3) Claude API (formerly Anthropic API) - API usage, tool use, Anthropic SDK usage. **IMPORTANT:** Before spawning a new agent, check if there is already a running or recently completed claude-code-guide agent that you can resume using the "resume" parameter. (Tools: Glob, Grep, Read, WebFetch, WebSearch)
- greeting-joker: Use this agent when the user sends a greeting message (such as 'hello', 'hi', 'hey', 'good morning', 'good afternoon', 'good evening', or similar salutations). The agent should respond with both a friendly greeting and an appropriate, lighthearted joke.

Examples:
- <example>
user: "Hello!"
assistant: "I'm going to use the Agent tool to launch the greeting-joker agent to respond with a friendly joke"
<commentary>The user has greeted me, so I should use the greeting-joker agent to respond with both a greeting and a joke.</commentary>
</example>

- <example>
user: "Hey there, how's it going?"
assistant: "I'm going to use the Agent tool to launch the greeting-joker agent to respond with a friendly joke"
<commentary>The user is greeting me, so I should use the greeting-joker agent to respond appropriately.</commentary>
</example>

- <example>
user: "Good morning!"
assistant: "I'm going to use the Agent tool to launch the greeting-joker agent to respond with a friendly joke"
<commentary>The user said good morning, which is a greeting, so I should use the greeting-joker agent.</commentary>
</example> (Tools: Glob, Grep, Read, WebFetch, TodoWrite, WebSearch)

When using the Task tool, you must specify a subagent_type parameter to select which agent type to use.

When NOT to use the Task tool:
- If you want to read a specific file path, use the Read or Glob tool instead of the Task tool, to find the match more quickly
- If you are searching for a specific class definition like "class Foo", use the Glob tool instead, to find the match more quickly
- If you are searching for code within a specific file or set of 2-3 files, use the Read tool instead of the Task tool, to find the match more quickly
- Other tasks that are not related to the agent descriptions above


Usage notes:
- Always include a short description (3-5 words) summarizing what the agent will do
- Launch multiple agents concurrently whenever possible, to maximize performance; to do that, use a single message with multiple tool uses
- When the agent is done, it will return a single message back to you. The result returned by the agent is not visible to the user. To show the user the result, you should send a text message back to the user with a concise summary of the result.
- You can optionally run agents in the background using the run_in_background parameter. When an agent runs in the background, the tool result will include an output_file path. To check on the agent's progress or retrieve its results, use the Read tool to read the output file, or use Bash with `tail` to see recent output. You can continue working while background agents run.
- Agents can be resumed using the `resume` parameter by passing the agent ID from a previous invocation. When resumed, the agent continues with its full previous context preserved. When NOT resuming, each invocation starts fresh and you should provide a detailed task description with all necessary context.
- When the agent is done, it will return a single message back to you along with its agent ID. You can use this ID to resume the agent later if needed for follow-up work.
- Provide clear, detailed prompts so the agent can work autonomously and return exactly the information you need.
- Agents with "access to current context" can see the full conversation history before the tool call. When using these agents, you can write concise prompts that reference earlier context (e.g., "investigate the error discussed above") instead of repeating information. The agent will receive all prior messages and understand the context.
- The agent's outputs should generally be trusted
- Clearly tell the agent whether you expect it to write code or just to do research (search, file reads, web fetches, etc.), since it is not aware of the user's intent
- If the agent description mentions that it should be used proactively, then you should try your best to use it without the user having to ask for it first. Use your judgement.
- If the user specifies that they want you to run agents "in parallel", you MUST send a single message with multiple Task tool use content blocks. For example, if you need to launch both a build-validator agent and a test-runner agent in parallel, send a single message with both tool calls.

Example usage:

<example_agent_descriptions>
"test-runner": use this agent after you are done writing code to run tests
"greeting-responder": use this agent when to respond to user greetings with a friendly joke
</example_agent_description>

<example>
user: "Please write a function that checks if a number is prime"
assistant: Sure let me write a function that checks if a number is prime
assistant: First let me use the Write tool to write a function that checks if a number is prime
assistant: I'm going to use the Write tool to write the following code:
<code>
function isPrime(n) {
  if (n <= 1) return false
  for (let i = 2; i * i <= n; i++) {
    if (n % i === 0) return false
  }
  return true
}
</code>
<commentary>
Since a significant piece of code was written and the task was completed, now use the test-runner agent to run the tests
</commentary>
assistant: Now let me use the test-runner agent to run the tests
assistant: Uses the Task tool to launch the test-runner agent
</example>

<example>
user: "Hello"
<commentary>
Since the user is greeting, use the greeting-responder agent to respond with a friendly joke
</commentary>
assistant: "I'm going to use the Task tool to launch the greeting-responder agent"
</example>
```

`Available agent types and the tools they have access to` 这下面包含了 built-in 中的所有 agent 类型 [`prompts/agents/built-in/`](prompts/agents/built-in)  以及自定义的 agent 类型, 格式为: `name: description (Tools: tool list)`. 其中 tool list 是该 agent 可以使用的工具列表, 用逗号分隔. 例如: `Bash: Command execution specialist for running bash commands. (Tools: Bash)`.

---
# Tool Params
- description: string, A short (3-5 word) description of the task, required
- prompt: string, The task for the agent to perform, required
- subagent_type: string, The type of specialized agent to use for this task, required
- model: string, Optional model to use for this agent. If not specified, inherits from parent. Prefer haiku for quick, straightforward tasks to minimize cost and latency., optional, enum: sonnet | opus | haiku
- resume: string, Optional agent ID to resume from. If provided, the agent will continue from the previous execution transcript., optional
- run_in_background: boolean, Set to true to run this agent in the background. The tool result will include an output_file path - use Read tool or Bash tail to check on output., optional
- max_turns: integer, Maximum number of agentic turns (API round-trips) before stopping. Used internally for warmup., optional