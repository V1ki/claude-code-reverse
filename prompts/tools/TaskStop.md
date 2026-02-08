---
name: TaskStop
---

- Stops a running background task by its ID
- Takes a task_id parameter identifying the task to stop
- Returns a success or failure status
- Use this tool when you need to terminate a long-running task

--- 
# Tool Params
- task_id: string, The ID of the background task to stop, optional
- shell_id: string, Deprecated: use task_id instead, optional