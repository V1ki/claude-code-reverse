---
name: Grep
---

A powerful search tool built on ripgrep

  Usage:
  - ALWAYS use Grep for search tasks. NEVER invoke `grep` or `rg` as a Bash command. The Grep tool has been optimized for correct permissions and access.
  - Supports full regex syntax (e.g., "log.*Error", "function\s+\w+")
  - Filter files with glob parameter (e.g., "*.js", "**/*.tsx") or type parameter (e.g., "js", "py", "rust")
  - Output modes: "content" shows matching lines, "files_with_matches" shows only file paths (default), "count" shows match counts
  - Use Task tool for open-ended searches requiring multiple rounds
  - Pattern syntax: Uses ripgrep (not grep) - literal braces need escaping (use `interface\{\}` to find `interface{}` in Go code)
  - Multiline matching: By default patterns match within single lines only. For cross-line patterns like `struct \{[\s\S]*?field`, use `multiline: true`


---
# Tool Params
- pattern: string, The regular expression pattern to search for in file contents, required
- path: string, File or directory to search in (rg PATH). Defaults to current working directory., optional
- glob: string, Glob pattern to filter files (e.g. "*.js", "*.{ts,tsx}") - maps to rg --glob, optional
- output_mode: string, Output mode: "content" shows matching lines (supports -A/-B/-C context, -n line numbers, head_limit), "files_with_matches" shows file paths (supports head_limit), "count" shows match counts (supports head_limit). Defaults to "files_with_matches"., optional, enum: content | files_with_matches | count
- -B: number, Number of lines to show before each match (rg -B). Requires output_mode: "content", ignored otherwise., optional
- -A: number, Number of lines to show after each match (rg -A). Requires output_mode: "content", ignored otherwise., optional
- -C: number, Number of lines to show before and after each match (rg -C). Requires output_mode: "content", ignored otherwise., optional
- -n: boolean, Show line numbers in output (rg -n). Requires output_mode: "content", ignored otherwise. Defaults to true., optional
- -i: boolean, Case insensitive search (rg -i), optional
- type: string, File type to search (rg --type). Common types: js, py, rust, go, java, etc. More efficient than include for standard file types., optional
- head_limit: number, Limit output to first N lines/entries, equivalent to "| head -N". Works across all output modes: content (limits output lines), files_with_matches (limits file paths), count (limits count entries). Defaults to 0 (unlimited)., optional
- offset: number, Skip first N lines/entries before applying head_limit, equivalent to "| tail -n +N | head -N". Works across all output modes. Defaults to 0., optional
- multiline: boolean, Enable multiline mode where . matches newlines and patterns can span lines (rg -U --multiline-dotall). Default: false., optional