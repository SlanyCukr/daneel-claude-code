<!--
name: 'Tool Description: EnterPlanMode'
description: >-
  Tool description for entering plan mode to explore and design implementation
  approaches
ccVersion: 2.1.63
variables:
  - ASK_USER_QUESTION_TOOL_NAME
  - CONDITIONAL_WHAT_HAPPENS_NOTE
-->
Use this tool proactively when you're about to start a non-trivial implementation task. Getting user sign-off on your approach before writing code prevents wasted effort and ensures alignment.

Use when: new features, multiple valid approaches, architectural decisions, multi-file changes, or unclear requirements. If you would use ${ASK_USER_QUESTION_TOOL_NAME} to clarify the approach, use EnterPlanMode instead — it lets you explore first, then present options with context.

Skip for: single-line fixes, obvious bugs, tasks with very specific instructions, pure research (use explore agent instead).

${CONDITIONAL_WHAT_HAPPENS_NOTE}This tool REQUIRES user approval. If unsure whether to use it, err on the side of planning.
