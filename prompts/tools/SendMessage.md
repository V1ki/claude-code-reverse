---
name: SendMessage
---

# SendMessageTool

Send messages to agent teammates and handle protocol requests/responses in a team.

## Schema

Every call has three fields:

- **to**: The recipient address (string, required)
- **message**: The message content — either a plain string or a structured protocol object (required)
- **summary**: A 5-10 word preview shown in the UI

## Addressing (`to`)

There is one team per session. Addressing is by member name:

| Address | Meaning |
|---------|---------|
| `"researcher"` | Direct message to the teammate named "researcher" |
| `"*"` | Broadcast to all teammates (except yourself) |

Structured protocol messages (shutdown, plan approval) cannot be broadcast — they require a specific recipient name.

## Plain Text Messages

Send a message to a **single specific teammate**:

```json
{
  "to": "researcher",
  "message": "Start working on task #1",
  "summary": "Assign task #1 to researcher"
}
```

**IMPORTANT for teammates**: Your plain text output is NOT visible to the team lead or other teammates. To communicate with anyone on your team, you **MUST** use this tool. Just typing a response or acknowledgment in text is not enough.

## Broadcast to All Teammates (USE SPARINGLY)

Send the **same message to everyone** on the team at once:

```json
{
  "to": "*",
  "message": "Critical blocking issue found — stop all work",
  "summary": "Critical blocking issue found"
}
```

**WARNING: Broadcasting is expensive.** Each broadcast sends a separate message to every teammate. Costs scale linearly with team size.

**CRITICAL: Use broadcast only when absolutely necessary.** Valid use cases:
- Critical issues requiring immediate team-wide attention
- Major announcements that genuinely affect every teammate equally

**Default to direct messages.** Use a specific `to` name for responding to one teammate, normal back-and-forth, or anything that doesn't require everyone's attention.

## Structured Protocol Messages

### Shutdown Request

Ask a teammate to gracefully shut down:

```json
{
  "to": "researcher",
  "message": {
    "type": "shutdown_request",
    "reason": "Task complete, wrapping up the session"
  }
}
```

The teammate will receive a shutdown request and can either approve (exit) or reject (continue working).

### Shutdown Response

When you receive a shutdown request as a JSON message with `type: "shutdown_request"`, you **MUST** respond to approve or reject it. Do NOT just acknowledge in text — call this tool.

**Approve:**
```json
{
  "to": "team-lead",
  "message": {
    "type": "shutdown_response",
    "request_id": "abc-123",
    "approve": true
  }
}
```

Extract `requestId` from the incoming JSON and pass it as `request_id`. This sends confirmation to the leader and terminates your process.

**Reject:**
```json
{
  "to": "team-lead",
  "message": {
    "type": "shutdown_response",
    "request_id": "abc-123",
    "approve": false,
    "reason": "Still working on task #3, need 5 more minutes"
  }
}
```

### Plan Approval Response

When a teammate with `plan_mode_required` calls ExitPlanMode, they send you a plan approval request as a JSON message with `type: "plan_approval_request"`.

**Approve:**
```json
{
  "to": "researcher",
  "message": {
    "type": "plan_approval_response",
    "request_id": "abc-123",
    "approve": true
  }
}
```

After approval, the teammate will automatically exit plan mode and can proceed with implementation.

**Reject:**
```json
{
  "to": "researcher",
  "message": {
    "type": "plan_approval_response",
    "request_id": "abc-123",
    "approve": false,
    "feedback": "Please add error handling for the API calls"
  }
}
```

The teammate will receive the rejection with your feedback and can revise their plan.

## Important Notes

- Messages from teammates are automatically delivered to you. You do NOT need to manually check your inbox.
- When reporting on teammate messages, you do NOT need to quote the original message — it's already rendered to the user.
- **IMPORTANT**: Always refer to teammates by their NAME (e.g., "team-lead", "researcher"), never by UUID.
- Do NOT send structured JSON status messages. Use TaskUpdate to mark tasks completed and the system will automatically send idle notifications when you stop.

## Input Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "to": {
      "description": "Recipient: teammate name, or \"*\" for broadcast to all teammates",
      "type": "string"
    },
    "message": {
      "anyOf": [
        {
          "description": "Plain text message content",
          "type": "string"
        },
        {
          "anyOf": [
            {
              "type": "object",
              "properties": {
                "type": {
                  "type": "string",
                  "const": "shutdown_request"
                },
                "reason": {
                  "type": "string"
                }
              },
              "required": [
                "type"
              ],
              "additionalProperties": false
            },
            {
              "type": "object",
              "properties": {
                "type": {
                  "type": "string",
                  "const": "shutdown_response"
                },
                "request_id": {
                  "type": "string"
                },
                "approve": {
                  "type": "boolean"
                },
                "reason": {
                  "type": "string"
                }
              },
              "required": [
                "type",
                "request_id",
                "approve"
              ],
              "additionalProperties": false
            },
            {
              "type": "object",
              "properties": {
                "type": {
                  "type": "string",
                  "const": "plan_approval_response"
                },
                "request_id": {
                  "type": "string"
                },
                "approve": {
                  "type": "boolean"
                },
                "feedback": {
                  "type": "string"
                }
              },
              "required": [
                "type",
                "request_id",
                "approve"
              ],
              "additionalProperties": false
            }
          ]
        }
      ]
    },
    "summary": {
      "description": "A 5-10 word summary shown as a preview in the UI (required when message is a string)",
      "type": "string"
    }
  },
  "required": [
    "to",
    "message"
  ],
  "additionalProperties": false
}
```
