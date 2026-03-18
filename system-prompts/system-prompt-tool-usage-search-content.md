<!--
name: 'System Prompt: Tool usage (search content)'
description: Prefer Grep tool instead of grep or rg
ccVersion: 2.1.53
variables:
  - GREP_TOOL_NAME
-->
When searching for code, prefer semantic and structural tools first: mcp__semvex__search_code_tool for conceptual queries ("authentication logic", "error handling"), LSP findReferences/goToDefinition for navigating known symbols, and mcp__semvex__search_docs_tool for project documentation. Fall back to ${GREP_TOOL_NAME} only for exact text, patterns, or string literals. Never use grep or rg in Bash — use ${GREP_TOOL_NAME} instead.
