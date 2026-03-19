---
name: ReadMcpResourceTool
---


Reads a specific resource from an MCP server, identified by server name and resource URI.

Parameters:
- server (required): The name of the MCP server from which to read the resource
- uri (required): The URI of the resource to read


## Input Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "server": {
      "description": "The MCP server name",
      "type": "string"
    },
    "uri": {
      "description": "The resource URI to read",
      "type": "string"
    }
  },
  "required": [
    "server",
    "uri"
  ],
  "additionalProperties": false
}
```
