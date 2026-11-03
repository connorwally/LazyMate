# LazyMate

Your coding companion in the terminal.

LazyMate is a terminal workspace for ticket-driven coding-agent workflows. Import a GitHub issue or Jira ticket, or create a local task; apply a workflow and jump into native Pi or Claude Code sessions without losing your place.

**Status:** Planning and technical feasibility. No application is implemented yet. First release targets macOS, initially validated with Ghostty; other platforms are future targets, not claimed support.

## Intended experience

- Select a repository in a lazygit-inspired Projects panel.
- See active items and their workflow stages.
- Start a stage in the background, or press Enter to start/open its native agent session.
- Use a LazyMate-owned return shortcut to return without interrupting work.
- Carry requirements, findings, and review notes across separate stage conversations through a shared item document.
- Mark work Done and revisit its history later.

## Planning documents

1. [Product brief](docs/product-brief.md) — purpose, requirements, scope, and open questions.
2. [Architecture guide](docs/architecture.md) — boundaries, diagrams, and technical risks.
3. [Implementation plan](docs/implementation-plan.md) — feasibility-first milestones and exit criteria.
4. [Decision register](docs/decisions.md) — accepted choices versus unresolved implementation details.
5. [Ticket-source research](docs/research/ticket-sources.md) — GitHub/Jira options and authentication constraints.

Implementing agents should read [AGENTS.md](AGENTS.md) first. The first assignment is the native-terminal feasibility spike, not ticket integration or a custom coding IDE.
