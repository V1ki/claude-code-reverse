---
name: TaskOutput
---

- Retrieves output from a running or completed task (background shell, agent, or remote session)
- Takes a task_id parameter identifying the task
- Returns the task output along with status information
- Use block=true (default) to wait for task completion
- Use block=false for non-blocking check of current status
- Task IDs can be found using the /tasks command
- Works with all task types: background shells, async agents, and remote sessions

---
# Tool Params
- task_id: string, The task ID to get output from, required
- block: boolean, Whether to wait for completion, required
- timeout: number, Max wait time in ms, required