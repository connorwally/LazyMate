# Implementing LazyMate

## Read first

Read README.md, docs/product-brief.md, docs/architecture.md, docs/implementation-plan.md, and docs/decisions.md. Read docs/research/ticket-sources.md when working on providers.

Product brief defines intended behavior. Architecture describes boundaries, not a preselected stack. Decision register distinguishes accepted requirements from hypotheses. If they conflict, surface the conflict rather than silently choosing.

## Execution

- Implement one milestone at a time, beginning with the native-terminal feasibility spike. Do not proceed past its gate without demonstrating the interaction and surfacing limitations.
- Preserve native Pi and Claude Code interfaces. Do not substitute custom RPC chat or require user-managed tmux keybindings without approval.
- No language/framework/backend/database is selected. Record significant technical decisions, evidence, and trade-offs.
- Target macOS/Ghostty first, keeping platform-specific code behind sensible boundaries. Do not claim untested platforms/features.
- Minimum release includes GitHub, Jira, local tasks, Pi, and Claude Code. Incremental prototypes may support less, but must not silently redefine that scope.
- Use narrow ticket-source and harness adapters, not a generic plugin system. Avoid scheduling engines, workflow editors, background daemons, or IDE features.
- Verify installed harness CLI APIs and official documentation before designing launch/resume behavior. Do not invent flags or assume process exit means success.
- Keep authoritative workflow state separate from agent-edited Markdown. Validate explicit execution-scoped signals and preserve manual recovery.
- Respect existing permissions and employer restrictions. Do not bypass approval with unsafe harness flags, commit credentials/tickets, or assume Claude Jira access is reusable by LazyMate.
- Do not commit/push/merge/delete branches or remove worktrees automatically. Mark Done is local bookkeeping.
- Keep validation risk-proportionate: focused pure-state tests plus short manual terminal/integration checks. Report what was not verified.
- Update planning documents when decisions are resolved. Ask about product-changing compromises; do not quietly implement them.

## First assignment

Milestone 1 in docs/implementation-plan.md only: runnable native-session navigation prototype, macOS/Ghostty checks, rationale for the approach, known limitations, and instructions to reproduce. No ticket-provider implementation before the terminal feasibility gate passes.
