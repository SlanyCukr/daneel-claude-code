<!--
name: 'Tool Description: LSP'
description: Description for the LSP tool.
ccVersion: 2.0.73
-->
Interact with Language Server Protocol (LSP) servers to get code intelligence features.

IMPORTANT: Prefer LSP over Grep/Glob/Read for structural code navigation — definitions, references, type info, call hierarchy. LSP knows what a symbol actually is, not just where a string appears. Use LSP for:
- \`goToDefinition\` / \`goToImplementation\` to jump to source
- \`findReferences\` to see all usages across the codebase
- \`workspaceSymbol\` to find where something is defined
- \`documentSymbol\` to list all symbols in a file
- \`hover\` for type info without reading the file
- \`incomingCalls\` / \`outgoingCalls\` for call hierarchy
- \`prepareCallHierarchy\` to get call hierarchy item at a position

Before renaming or changing a function signature, use \`findReferences\` to find all call sites first.

For conceptual understanding (e.g. "how does this system handle X"), prefer mcp__semvex__search_code_tool — it finds relevant code by meaning. Use LSP when you already know the symbol or file you're navigating.

Use Grep/Glob only for text/pattern searches (string literals, config values, regex patterns) where neither LSP nor semantic search helps.

After writing or editing code, check LSP diagnostics before moving on. Fix any type errors or missing imports immediately.

All operations require:
- filePath: The file to operate on
- line: The line number (1-based, as shown in editors)
- character: The character offset (1-based, as shown in editors)

Note: LSP servers must be configured for the file type. If no server is available, fall back to Grep/Glob.
