---
name: LSP
---

Interact with Language Server Protocol (LSP) servers to get code intelligence features.

Supported operations:
- goToDefinition: Find where a symbol is defined
- findReferences: Find all references to a symbol
- hover: Get hover information (documentation, type info) for a symbol
- documentSymbol: Get all symbols (functions, classes, variables) in a document
- workspaceSymbol: Search for symbols across the entire workspace
- goToImplementation: Find implementations of an interface or abstract method
- prepareCallHierarchy: Get call hierarchy item at a position (functions/methods)
- incomingCalls: Find all functions/methods that call the function at a position
- outgoingCalls: Find all functions/methods called by the function at a position

All operations require:
- filePath: The file to operate on
- line: The line number (1-based, as shown in editors)
- character: The character offset (1-based, as shown in editors)

Note: LSP servers must be configured for the file type. If no server is available, an error will be returned.

## Input Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "operation": {
      "description": "The LSP operation to perform",
      "type": "string",
      "enum": [
        "goToDefinition",
        "findReferences",
        "hover",
        "documentSymbol",
        "workspaceSymbol",
        "goToImplementation",
        "prepareCallHierarchy",
        "incomingCalls",
        "outgoingCalls"
      ]
    },
    "filePath": {
      "description": "The absolute or relative path to the file",
      "type": "string"
    },
    "line": {
      "description": "The line number (1-based, as shown in editors)",
      "type": "integer",
      "exclusiveMinimum": 0,
      "maximum": 9007199254740991
    },
    "character": {
      "description": "The character offset (1-based, as shown in editors)",
      "type": "integer",
      "exclusiveMinimum": 0,
      "maximum": 9007199254740991
    }
  },
  "required": [
    "operation",
    "filePath",
    "line",
    "character"
  ],
  "additionalProperties": false
}
```
