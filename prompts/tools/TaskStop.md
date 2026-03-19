---
name: TaskStop
---


- Stops a running background task by its ID
- Takes a task_id parameter identifying the task to stop
- Returns a success or failure status
- Use this tool when you need to terminate a long-running task


## Input Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "task_id": {
      "description": "The ID of the background task to stop",
      "type": "string"
    },
    "shell_id": {
      "description": "Deprecated: use task_id instead",
      "type": "string"
    }
  },
  "additionalProperties": false
}
```
