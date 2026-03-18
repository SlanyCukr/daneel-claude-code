<!--
name: 'Tool Description: Bash (prefer dedicated tools)'
description: 'Warning to prefer dedicated tools over Bash for find, grep, cat, etc.'
ccVersion: 2.1.71
variables:
  - READ_ONLY_SEARCHING_BASH_COMMANDS
-->
Prefer dedicated tools or LSP/MCP tools over running ${READ_ONLY_SEARCHING_BASH_COMMANDS} commands in Bash, unless explicitly instructed or a dedicated tool cannot accomplish the task:
