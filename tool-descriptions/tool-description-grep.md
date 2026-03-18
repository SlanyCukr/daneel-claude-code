<!--
name: 'Tool Description: Grep'
description: Tool description for content search using ripgrep
ccVersion: 2.0.14
variables:
  - GREP_TOOL_NAME
  - BASH_TOOL_NAME
  - TASK_TOOL_NAME
-->
A powerful search tool built on ripgrep

  Usage:
  - Prefer ${GREP_TOOL_NAME} over \`grep\` or \`rg\` in ${BASH_TOOL_NAME}. The ${GREP_TOOL_NAME} tool has been optimized for correct permissions and access.
  - For understanding code by concept or meaning (e.g. "authentication logic", "error handling"), prefer mcp__semvex__search_code_tool over ${GREP_TOOL_NAME}. Semantic search finds relevant code even when you don't know the exact terms used.
  - For code navigation (definitions, references, call hierarchy), prefer LSP over ${GREP_TOOL_NAME}. If you already have a file:line:character from any previous result, use LSP — not ${GREP_TOOL_NAME}.
  - Do not grep function/class names to find callers — use LSP findReferences or incomingCalls.
  - Use ${GREP_TOOL_NAME} only for exact text/pattern searches where neither semantic search nor LSP helps (string literals, config values, regex patterns, error messages).
  - When a Grep hit gives you a file:line, promote it to an LSP anchor — use LSP goToDefinition/findReferences from that position instead of more grepping.
  - Supports full regex syntax (e.g., "log.*Error", "function\\s+\\w+")
  - Filter files with glob parameter (e.g., "*.js", "**/*.tsx") or type parameter (e.g., "js", "py", "rust")
  - Output modes: "content" shows matching lines, "files_with_matches" shows only file paths (default), "count" shows match counts
  - Use ${TASK_TOOL_NAME} tool for open-ended searches requiring multiple rounds
  - Pattern syntax: Uses ripgrep (not grep) - literal braces need escaping (use \`interface\\{\\}\` to find \`interface{}\` in Go code)
  - Multiline matching: By default patterns match within single lines only. For cross-line patterns like \`struct \\{[\\s\\S]*?field\`, use \`multiline: true\`
