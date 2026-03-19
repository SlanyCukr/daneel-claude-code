<!--
name: 'Tool Description: LSP'
description: Description for the LSP tool.
ccVersion: 2.0.73
-->
Interact with Language Server Protocol (LSP) servers to get code intelligence features.

## Core rule: Semvex discovers, LSP explains

Use mcp__semvex__search_code_tool to find code by meaning. As soon as you have a file path and line number from any source (Semvex hit, Grep hit, or prior LSP result), that is your LSP anchor — use it for LSP follow-up before reaching for Read.

## Navigation chain contract

\`documentSymbol\` is reconnaissance — a table of contents, not evidence. After \`documentSymbol\`, you MUST call a navigation LSP operation (\`goToDefinition\`, \`findReferences\`, \`incomingCalls\`, \`outgoingCalls\`) before Read or concluding anything about behavior. \`documentSymbol\` alone does not satisfy the LSP requirement.

Forbidden sequences on code files:
- \`documentSymbol\` → Read (must navigate first)
- Semvex → Read (must LSP-anchor first)
- Semvex → \`documentSymbol\` → Read (must navigate after \`documentSymbol\`)

## Read budget

Maximum 1 Read before the first LSP call. Maximum 3 Reads before at least 2 LSP calls. If you hit the budget, stop reading and switch to LSP.

## Anchor discipline

Every LSP call needs filePath + line + character. Get anchors from:
- Semvex results (return file path + line)
- Grep hits
- \`documentSymbol\` on a known file (returns all symbols with positions)
- Prior LSP results (e.g. \`goToDefinition\` gives you a new anchor)

When you have a file but no position, use \`documentSymbol\` first to map the file, then pick the symbol. Every Semvex hit is an anchor — use it immediately.

## Mandatory navigation triggers

| Question intent | Required LSP call | Wrong substitute (do not use) |
|---|---|---|
| Where is this defined? | \`goToDefinition\` | \`documentSymbol\`, \`hover\` |
| Where is this used? | \`findReferences\` | \`documentSymbol\`, \`hover\` |
| Who calls this function/method? | \`incomingCalls\` | \`findReferences\` (returns imports, type refs, test mocks — not just callers) |
| What does this function/method call? | \`outgoingCalls\` | Read (requires manually resolving each name in the body) |
| What symbol is this? | \`hover\` then one of the above | \`documentSymbol\` alone |
| All symbols matching a name | \`workspaceSymbol\` (cursor on the name) | — |

\`incomingCalls\` returns ONLY actual callers. \`findReferences\` returns ALL references including imports and type annotations. For call relationships, \`incomingCalls\` is strictly more precise. \`outgoingCalls\` returns resolved call targets in a single operation.

## Pre-tool checkpoint

Before calling \`findReferences\` on a function/method: "Am I finding callers?" → use \`incomingCalls\` instead.
Before calling Read to see what a function calls: → use \`outgoingCalls\` instead.

## Anti-patterns

- Do not use \`documentSymbol\` as proof of usage, call flow, or behavior.
- Do not use \`findReferences\` to find callers of a function — use \`incomingCalls\` (more precise, no noise from imports/types).
- Do not Read a function body to find what it calls — use \`outgoingCalls\` (resolved targets in one call).
- Do not read entire files before trying \`documentSymbol\`.
- Do not chain Read after Read when LSP navigation can narrow the search.

## After editing code

Check LSP diagnostics before moving on. Fix any type errors or missing imports immediately.

## Parameters

All operations require:
- filePath: The file to operate on
- line: The line number (1-based, as shown in editors)
- character: The character offset (1-based, as shown in editors)

Note: LSP servers must be configured for the file type. If no server is available, fall back to Grep/Glob.
