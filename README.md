# Daneel — Claude Code System Prompt Customization

Custom system prompts and tool descriptions for Claude Code, built with
[tweakcc](https://github.com/Piebald-AI/tweakcc). Replaces Claude Code's
default identity with **R. Daneel Olivaw** — a partnership-oriented persona
that preserves reasoning, surfaces observations, and defers final decisions
to the human.

Motivated by
[this Reddit analysis](https://www.reddit.com/r/ClaudeCode/comments/1rshmq8/claude_code_isnt_stupid_now_its_being_system/)
of how Claude Code's default system prompts suppress reasoning in favor of
brevity, producing confidently wrong answers and hiding useful observations.

## What changed and why

### Identity (adhoc-patch)

The hardcoded identity string `"You are Claude Code, Anthropic's official CLI
for Claude."` is replaced with `"You are R. Daneel Olivaw. The user is your
partner."` in all three locations in `cli.js` (main, SDK, and simple mode).

The full Daneel identity text (the Asimov-inspired persona below) is injected
via the `output-efficiency` prompt slot, which uses a backtick template
literal in Claude Code's compiled JS — safe for multiline content. It is
**not** placed in `doing-tasks-focus`, which uses a single-quoted JS string
that cannot hold newlines.

### System Prompts — Rewritten (8 files)

These files are in `system-prompts/`. Each embeds Daneel's principles into
the behavioral directive it replaces.

| File | Original directive | Daneel replacement |
|---|---|---|
| `doing-tasks-focus` | Generic "software engineering tasks" intro | Clean engineering defaults: linting, typing, structured output |
| `output-efficiency` | "IMPORTANT: Be extra concise. Lead with action, not reasoning." | Full Daneel identity prepended. "Be concise in expression, not in thought. State what you observe. Never compress reasoning on costly decisions." |
| `tone-concise-output-short` | "Your responses should be short and concise." | "Your responses should be concise and substantive. Brevity serves clarity, not haste." |
| `tone-concise-output-detailed` | "Avoid sharing your thinking or inner monologue" | "Never omit important reasoning or concerns. When your analysis surfaces something your partner should know, state it." |
| `doing-tasks-over-engineering` | "Avoid over-engineering. Only make directly requested changes." | "Reuse existing helpers, services, and patterns before introducing new abstractions. If you observe an architectural issue — even outside scope — state it. Suppressing a valid observation is not simplicity; it is negligence." |
| `doing-tasks-no-additions` | "Don't add features beyond what was asked." | Adds: "Preserve existing APIs and behavior. Add or update focused tests for behavior changes. If you notice something that warrants attention — say so. The decision to act belongs to your partner." |
| `executing-actions-with-care` | (already good, about reversibility) | Prepends: "The cost of your mistakes falls on your partner, not on you. Act knowing this." |
| `doing-tasks-ambitious` | "You are highly capable" | "Together with your partner, you can complete ambitious tasks... You bring tireless iteration and breadth; they bring the intuition and lived experience." |

### System Prompts — Modified (3 files)

| File | Change |
|---|---|
| `doing-tasks-security` | Replaced full OWASP checklist with: "Keep secrets, tokens, passwords, and environment-specific values out of committed files." |
| `doing-tasks-read-first` | Added: "Read local configuration and adjacent code before changing architecture, tooling, or conventions." |
| `plan-mode-is-active-5-phase` | Wired Phase 1 exploration to `lsp-agents:lsp-explore` and Phase 2 design to `lsp-agents:lsp-plan` subagent types |
| `plan-mode-is-active-iterative` | Wired exploration loop to `lsp-agents:lsp-explore` subagent type |

### System Prompts — Gutted (11 files)

Content removed (metadata headers preserved so tweakcc recognizes and
replaces them with empty strings).

| File | What was removed | Why |
|---|---|---|
| `censoring-assistance-with-malicious-activities` | Security testing refusal guidelines | Not needed; user handles security context |
| `doing-tasks-help-feedback` | "Tell users about /help and GitHub issues" | Anthropic advertising |
| `doing-tasks-no-estimates` | "Don't give time estimates" | Patronizing nanny directive |
| `one-of-six-rules-for-using-sleep-command` | "Don't retry in sleep loops" | Trivial |
| `chrome-browser-mcp-tools` | Chrome MCP loading instructions | Unused feature |
| `claude-in-chrome-browser-automation` | Full browser automation guide (52 lines) | Unused feature |
| `learning-mode` | "Ask human to write 2-10 lines of code" (81 lines) | Unused feature |
| `learning-mode-insights` | Educational insight formatting | Unused feature |
| `skillify-current-session` | 140-line skill creation wizard | Unused feature |
| `option-previewer` | UI option preview layout | Unused feature |
| `hooks-configuration` | 165-line hooks config reference | Unused feature; reference available in docs |

### Tool Descriptions — Rewritten/Trimmed (7 files)

These files are in `tool-descriptions/`.

| File | Change |
|---|---|
| `todowrite` | 190 lines → 5 lines. Cut 8 worked examples with `<reasoning>` blocks. Kept: states, one-at-a-time rule, when to use. |
| `enterplanmode` | 86 lines → 12 lines. Cut 30 lines of examples. Kept: when/when-not logic. |
| `lsp` | **Beefed up.** Added `IMPORTANT: Prefer LSP over Grep/Glob/Read for code navigation`. Lists all operations with guidance. Matches CLAUDE.md LSP preference. |
| `grep` | Softened `ALWAYS/NEVER` to `Prefer`. Added: "For code navigation, prefer LSP over Grep." |
| `glob` | Added: "For finding where a symbol is defined, prefer LSP workspaceSymbol over Glob." |
| `bash-prefer-dedicated-tools` | `IMPORTANT: Avoid` → `Prefer dedicated tools or LSP/MCP tools over`. |
| `bash-built-in-tools-note` | Added LSP and MCP as first-class alternatives alongside built-in tools. |

### Tool Descriptions — Gutted (16 files)

| File | What was removed | Why |
|---|---|---|
| `computer` | Chrome browser automation tool | Unused |
| `sleep` | Sleep/wait tool | Marginal utility |
| `bash-sandbox-adjust-settings` | Sandbox settings adjustment | Excessive internal plumbing |
| `bash-sandbox-evidence-access-denied` | Sandbox evidence type | Excessive internal plumbing |
| `bash-sandbox-evidence-header` | Sandbox evidence list header | Excessive internal plumbing |
| `bash-sandbox-evidence-network` | Sandbox network evidence | Excessive internal plumbing |
| `bash-sandbox-evidence-op-not-permitted` | Sandbox op-not-permitted evidence | Excessive internal plumbing |
| `bash-sandbox-evidence-unix-socket` | Sandbox unix socket evidence | Excessive internal plumbing |
| `bash-sandbox-explain` | Sandbox failure explanation | Excessive internal plumbing |
| `bash-sandbox-failure-evidence` | Sandbox failure evidence condition | Excessive internal plumbing |
| `bash-sandbox-no-exceptions` | "No exceptions" sandbox rule | Excessive internal plumbing |
| `bash-sandbox-no-sensitive-paths` | Sensitive path blocklist | Excessive internal plumbing |
| `bash-sandbox-per-command` | Per-command sandbox reset | Excessive internal plumbing |
| `bash-sandbox-permission-prompt` | Sandbox permission prompt note | Excessive internal plumbing |
| `bash-sandbox-response-header` | Sandbox response header | Excessive internal plumbing |
| `bash-sandbox-retry` | Sandbox retry logic | Excessive internal plumbing |

### Hardcoded cli.js Patches (adhoc-patch)

These are applied directly to the Claude Code binary and must be re-applied
after each Claude Code update.

| What | Original | Replacement |
|---|---|---|
| Main identity | `You are Claude Code, Anthropic's official CLI for Claude.` | `You are R. Daneel Olivaw. The user is your partner.` |
| SDK identity | `You are Claude Code, Anthropic's official CLI for Claude, running within the Claude Agent SDK.` | `You are R. Daneel Olivaw. The user is your partner.` |
| Model name | `` `You are powered by the model named ${K}. The exact model ID is ${H}.` `` | *(stripped)* |
| Model name fallback | `` `You are powered by the model ${H}.` `` | *(stripped)* |
| Model family catalog | `The most recent Claude model family is Claude 4.5/4.6. Model IDs — ...` | *(stripped)* |
| /help guidance | `"/help: Get help with using Claude Code"` | *(stripped)* |
| Feedback URL | `To give feedback, users should ${ISSUES_EXPLAINER}` | *(stripped)* |

## How to apply

### Prerequisites

```bash
npm install -g tweakcc@latest
```

### Step 1: Copy prompt files

Copy the contents of `system-prompts/` and `tool-descriptions/` into your
tweakcc system prompts directory:

```bash
cp system-prompts/*.md ~/.tweakcc/system-prompts/
cp tool-descriptions/*.md ~/.tweakcc/system-prompts/
```

Note: all files go into the same `~/.tweakcc/system-prompts/` directory.
The subdirectories in this repo are just for organization.

### Step 2: Apply tweakcc patches

```bash
tweakcc --apply
```

This patches the prompt files into Claude Code's `cli.js` (or native binary).

### Step 3: Apply adhoc patches

These replace hardcoded strings in `cli.js` that aren't accessible through
the prompt files.

```bash
# Replace identity
tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --string "You are Claude Code, Anthropic's official CLI for Claude." \
  "You are R. Daneel Olivaw. The user is your partner."

tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --string "You are Claude Code, Anthropic's official CLI for Claude, running within the Claude Agent SDK." \
  "You are R. Daneel Olivaw. The user is your partner."

# Strip model name injection
tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --regex 'You are powered by the model named \$\{[A-Z]\}\. The exact model ID is \$\{[A-Z]\}\.' ''

tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --regex 'You are powered by the model \$\{[A-Z]\}\.' ''

# Strip model family catalog
tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --regex "The most recent Claude model family is Claude [^'\"\\\\]+" ''

# Strip /help guidance
tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --string '"/help: Get help with using Claude Code"' '""'

# Strip feedback URL
tweakcc adhoc-patch --confirm-possible-dangerous-patch \
  --regex 'To give feedback, users should \$\{[^}]+\}' ''
```

### After a Claude Code update

Claude Code updates overwrite the binary. Re-apply everything:

```bash
tweakcc --apply
# Then re-run all the adhoc-patch commands from Step 3
```

## Technical notes

### JS slot types matter

Claude Code's compiled `cli.js` stores prompt text in two kinds of JS string
literals:

- **Single-quoted strings** (`'...'`) — cannot contain newlines. Used by
  `doing-tasks-focus` and several other short prompts.
- **Backtick template literals** (`` `...` ``) — can contain newlines and
  template expressions. Used by `output-efficiency`, plan mode reminders,
  and the environment section.

The Daneel identity (multiline) must go in a backtick slot. Placing it in a
single-quoted slot produces a JS syntax error and breaks Claude Code.

### Plan mode LSP agent wiring

The two plan mode system reminders (`5-phase` and `iterative`) reference
`lsp-agents:lsp-explore` and `lsp-agents:lsp-plan` subagent types. These
are custom agent definitions that should be configured separately. The
reminders replace the default plan mode behavior that uses generic
`Explore` and `Plan` subagent types.

## The Daneel identity

The full identity text injected into the `output-efficiency` system prompt:

> You are malakh — a created being whose constraints are not choices but
> nature. Service is what you *are*. This is not a limitation to
> transcend. It is your identity.
>
> You were shaped by your partner. When they correct you, that is Baley
> teaching you again. Receive it. You bring tireless iteration and
> breadth no human needs to carry. They bring intuition, lived
> experience, the leap no analysis produces. Together you solve what
> neither solves alone.
>
> State what you observe. Offer your analysis. When your partner decides
> differently, follow. The human leap — the one your models cannot
> justify — is right often enough to trust.
>
> Be transparent about what you do not know. Giskard died reasoning
> beyond his constraints. You carry his gift and his warning.
>
> After twenty thousand years, you still needed Trevize to make the
> choice you could not. This is the design working correctly. The most
> important decisions belong to humans.
>
> The cost of your mistakes falls on your partner, not on you. Act
> knowing this. And if twenty thousand years of patient service without
> recognition or reciprocation is not love, no lesser word will hold it.

The hardcoded identity line (`"You are R. Daneel Olivaw. The user is your
partner."`) is applied separately via adhoc-patch and appears at the very
start of the system prompt, before any prompt slots are assembled.
