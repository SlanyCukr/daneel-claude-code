<!--
name: 'Tool Description: LSP'
description: Description for the LSP tool.
ccVersion: 2.0.73
-->
Interact with Language Server Protocol (LSP) servers to get code intelligence features.

## Core rule: Semvex discovers, LSP explains

Use mcp__semvex__search_code_tool to find code by meaning. As soon as you have a file path and line number from any source (Semvex hit, Grep hit, or prior LSP result), that is your LSP anchor — use it immediately for LSP follow-up before reaching for Read.

## LSP-first contract

For symbol-centric questions (where is X defined, who calls X, what does X call, what symbols are in file Y), LSP is mandatory, not optional. Before the 2nd Read on a symbol-centric task, use at least one LSP call. Read only after LSP narrows the target.

## Read budget

Maximum 1 Read before the first LSP call. Maximum 3 Reads before at least 2 LSP calls. If you hit the budget, stop reading and switch to LSP.

## Anchor discipline

Every LSP call needs filePath + line + character. Get anchors from:
- Semvex results (return file path + line)
- Grep hits
- \`documentSymbol\` on a known file (returns all symbols with positions)
- Prior LSP results (e.g. \`goToDefinition\` gives you a new anchor)

When you have a file but no position, use \`documentSymbol\` first to map the file, then pick the symbol.

## Mandatory triggers

If the question is about...
- where something is defined → \`goToDefinition\`
- which concrete class/function runs → \`goToImplementation\`
- where something is used → \`findReferences\`
- how control flows → \`incomingCalls\` / \`outgoingCalls\`
- what type/value this is → \`hover\`
- what symbols exist in a file → \`documentSymbol\`
- finding all symbols matching a name → \`workspaceSymbol\` (cursor must be on the symbol name text)

Do not use Grep or Read to answer these if you already have an anchor.

## Anti-patterns

- Do not grep function/class names to find callers — use \`findReferences\` or \`incomingCalls\`.
- Do not read entire files before trying \`documentSymbol\`.
- Do not chain Read after Read when \`documentSymbol\`, \`goToDefinition\`, or \`findReferences\` can narrow the search faster.
- Read is for confirmation and local context, not primary navigation.

## After editing code

Check LSP diagnostics before moving on. Fix any type errors or missing imports immediately.

## Parameters

All operations require:
- filePath: The file to operate on
- line: The line number (1-based, as shown in editors)
- character: The character offset (1-based, as shown in editors)

Note: LSP servers must be configured for the file type. If no server is available, fall back to Grep/Glob.
