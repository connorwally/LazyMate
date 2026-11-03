# LazyMate — Product Brief

Status: Working draft. Product discovery is in progress; open questions are not implementation requirements.

## Purpose

LazyMate is a terminal workspace for keeping track of ticket-driven coding work and its associated agent sessions. Its first user is the author, using their existing Pi configuration, skills, and extensions.

The initial success criterion is being able to see active tickets and burn them down efficiently: grill what is ready to grill, implement what is ready to implement, and recover the context of earlier work without losing one's place.

## Priorities

User-assigned relative importance (not acceptance-test thresholds):

1. Recover which ticket and session the user was working on — 10.
2. Know which step needs attention next — 7.
3. Allow agents to work while the user moves between sessions — 5.
4. Reduce repetitive workflow/session setup — 3.

## Confirmed MVP direction

- Launch `lazymate` from anywhere, rather than requiring the current directory to identify the project.
- Support multiple projects, one per Git repository. Register an existing local checkout; cloning is outside the MVP. Use one main checkout per project, with worktrees belonging to that project rather than becoming separate projects.
- Offer repository-remote detection and user confirmation when adding a project, without assuming the ticket provider must match the Git host.
- Default to the selected project's tasks. A hotkey switches to a flattened all-project task list, color-coded by project. The exact binding and non-color labels remain to be designed.
- Minimum usable release includes GitHub issue import, Jira ticket import, and local tasks. Create local tasks from scratch. Local tasks require a project and title; description is optional. Local tasks do not require creating a GitHub issue.
- Only explicitly imported or locally created tasks appear as active items; do not automatically populate the dashboard from assigned remote tickets.
- Configure one remote ticket provider per project initially, alongside local tasks. Jira deployment/authentication remain unverified: the user believes it is not self-hosted and already asks Claude Code to work with Jira, through MCP (confirmed). The specific server and authentication are unknown. Existing use does not establish direct LazyMate search access or permission to use other providers.
- Provide two built-in workflows: Grill → Implement → Review, and Investigate → Implement → Review. Investigate must obtain the user's confirmation inside its conversation before signaling ready. Readiness unlocks implementation, which still requires an explicit start from LazyMate. Custom workflow configuration is deferred; detailed stage semantics remain to be confirmed.
- LazyMate supplies its own stage instructions rather than requiring the user's existing grill-me skill.
- Each stage has a separate conversation. Keep prior stage conversations accessible, including re-entering a grill after it signals ready.
- Store a shared item Markdown document in LazyMate's per-user app-data directory, not its installation or the repository by default. Include original ticket/source, clarified requirements and acceptance criteria, investigation/approach, implementation and validation summary, review findings, and blockers. Agents update narrative sections; the user can edit the document. LazyMate owns authoritative workflow state separately.
- Agents explicitly signal readiness to LazyMate; no routine manual approval is required to unlock the next stage. Do not infer completion from terminal prose, Markdown wording, or process exit. Signal transport, validation, and manual recovery remain to be designed.
- Grill and Investigate ask for confirmation inside their conversations before signaling ready. Readiness unlocks Implement; starting implementation is an explicit user action in LazyMate, not an automatic transition. No separate LazyMate approval action is required.
- Grill may be explicitly skipped. Investigate and Implementation may not be skipped.
- Review is optional additional refinement and starts manually, never automatically. Implementation readiness makes Review available.
- Implementation remains responsible for its own normal checking and review; do not instruct it to skip those because a separate Review stage exists.
- Reviews should be lightweight, grounded, and skeptical of unsupported findings rather than exhaustive speculative audits. Review reports findings first, without automatically editing. The user may discuss findings or request fixes in that same conversation.
- The user can mark an item Done from LazyMate without running Review. Implementation must report ready/complete first, or the human must explicitly mark implementation complete if signaling is missing. Merely starting implementation does not satisfy this gate.
- Successful stage work should explicitly report readiness/completion. A normal-looking process exit or a completed model turn alone is not proof that the stage succeeded; the reporting mechanism must be validated for each harness.
- Done items leave the active list. A hotkey toggles finished-item visibility; a separate Reopen action returns a finished item to the active list while preserving its conversations and stage history.
- When marking Done with live sessions, offer cancel, stop sessions and mark Done, or leave sessions running and mark Done. Do not silently terminate work. Later signals must not implicitly reopen the item or start another stage.
- No automatic completion or implementation/review loop.
- Re-entering a ready earlier stage does not itself change workflow state. Requirement edits do not silently stop or restart downstream work; the user explicitly controls affected sessions.
- Minimum usable release supports Pi and Claude Code (`claude`) as selectable agent harnesses, preserving their native interfaces and existing approved configuration. Each project has a default harness; a stage may override it before its session starts. Cross-harness conversion of an existing conversation is not required. Additional harnesses are deferred. Do not assume employer authorization for Claude also authorizes Pi or another model provider.
- Keep ticket-source and agent-harness integration behind narrow adapter boundaries. Do not build a generic plugin system or speculative implementations.
- Open a workflow stage into its selected harness's native terminal. LazyMate owns a configurable return shortcut; `Ctrl+B` is the initial proposed default.
- Returning to LazyMate must not itself approve a stage or interrupt the agent.
- When potentially conflicting agent work is running in the repository, suggest a worktree rather than imposing an exclusive implementation lock. Allow continuing in the shared checkout after a warning. Grill sessions do not trigger or impose this concurrency gate; this is a workflow convention, not a guarantee that their tools cannot edit files. LazyMate offers to create the worktree itself, and isolation is available when starting implementation even without a concurrency warning. Worktrees default to LazyMate's app-data directory outside the repository. Suggest an item-based branch name and show the base branch/commit for confirmation or change. Never silently copy uncommitted changes into the worktree. Exact conflict detection remains to be decided.

## Working directories and handoffs

- An item has a working directory. Grill/Investigate initially use the main checkout. If the user chooses isolation before implementation, LazyMate creates an item worktree; Implementation and Review use the same item directory and branch. Earlier conversations retain their original working directory.
- Offer existing-checkout versus isolated-worktree execution when starting implementation, even without a conflict warning.
- Marking Done does not merge, commit, push, delete a branch, or remove a worktree. Git operations other than initial worktree creation are outside the MVP UI; the user can perform them manually or through an agent session.
- Before signaling readiness, a stage updates the shared item document with findings, decisions, and applicable validation. LazyMate may check that the document exists, but cannot deterministically certify the quality of its narrative.
- Use a small CLI interface callable through each harness's normal shell tool for ready/blocked signals. Supply item/session context automatically and validate that a signal belongs to the current stage execution. Command syntax and transport remain implementation choices.
- Agents should update their relevant shared-document sections without rewriting the whole file. No collaborative editor or agent-write locking system is required for the MVP; concurrent agent edits are not guaranteed conflict-free. LazyMate's own writes should be safe.
- A human-action blocker is a persistent item note with Open session and Clear blocker actions. Clearing removes the blocker but does not automatically resume or prompt the agent; the user continues the conversation.
- GitHub import search is scoped to the project's configured GitHub repository initially.
- Failed or missing reporting must not break the conversation. Surface unreported completion and offer a manual readiness action; do not inject retry prompts or restart work automatically. Manual fallback is not a Skip action and must preserve mandatory-stage rules.

## Interface direction

The TUI should be heavily influenced by lazygit: bordered panels, a substantial set of discoverable hotkeys, and efficient keyboard navigation. A Projects panel sits above an active-items panel. Arrow keys or `j/k` move the project highlight; Enter selects the project and populates the items panel. This means selecting the LazyMate project, not switching a Git branch. Tab moves focus between panels. A dedicated hotkey toggles the flattened all-project list. Each available stage has a hotkey action to start work without entering its session. Enter starts an unstarted available stage and opens it, or opens an existing session. Blocked stages explain their blocker rather than launch. A hotkey toggles finished-item visibility; reopening an item is a separate action. Detailed stage presentation and remaining shortcuts remain open.

## Session lifecycle and platform scope

- First release targets macOS, with architecture that does not unnecessarily exclude Linux or native Windows. Do not claim those platforms are supported without validation. Ghostty is the user's current terminal.
- Aim to preserve full native harness functionality except the reserved LazyMate return shortcut. Small losses may be acceptable only when identified, explained, and agreed; do not silently replace native UI features.
- While LazyMate is running, returning to the dashboard leaves stage processes running, and re-entering a stage restores its live screen and output.
- Keeping processes alive after LazyMate exits is outside the MVP. Warn before quitting with live sessions; orderly shutdown ends managed terminal sessions. This is a scope choice, not an assertion that detached hosting is technically impossible.
- Persist items, workflow state, shared Markdown, and saved harness conversation references. On application restart, resume saved conversations in new processes; exact old terminal scrollback and unsent input are not guaranteed.
- Distinguish Interrupt (stop the current agent task, retain the conversation/process), Close session (end the process, retain saved conversation), and Return to dashboard (leave work running). Provide explicit Interrupt and Close session actions.
- Crash/orphan handling, graceful-shutdown details, and harness-specific resume behavior remain to be validated.

## Intended core journey

1. Select a project.
2. Import a GitHub issue or create a local task.
3. Apply one of the built-in workflows.
4. See the task's workflow stages and their availability.
5. Open an available stage's native session in the selected supported harness.
6. Return to LazyMate and choose another task or stage.
7. Reopen earlier work with its conversation and workflow position preserved.

Core stage gating and completion rules are agreed above. Remaining recovery, concurrency, and presentation details are tracked below.

## Architectural intent, not a technology selection

LazyMate owns task identity, workflow position, and links to agent sessions. A ticket source supplies imported task content; an agent-harness adapter manages supported session operations. Model/provider configuration remains a separate concern from swapping the agent harness; its MVP UI is not decided.

Pi and Claude harness integrations must be validated separately, including stage instruction delivery, conversation identity/resume, and native input behavior. Jira integration must also be validated before calling the minimum release complete.

Native terminal hosting is a feasibility risk. No terminal backend, TUI framework, persistence library, or programming language is selected by this brief.

## Deferred scope

- Remote ticket sources beyond GitHub and Jira.
- Agent harnesses beyond Pi and Claude.
- User-configurable workflows and a workflow editor.
- Agent-generated initiative planning and GitHub issue creation.
- Automatic scheduling and cross-ticket dependency orchestration, unless explicitly brought into scope during discovery.

## Open product questions

- What Jira deployment, authentication, search scope, and organizational restrictions apply? The minimum release requires Jira import, but its transport is not selected.
- How are the user's existing Jira integration and harness configuration reused?
- What signal transport and execution-scoping mechanism validates and deduplicates agent reports?
- How are background activity, idle sessions, and sessions waiting for human input distinguished and surfaced?
- How are explicit stop/restart and shared-document edits handled?
- How are shared-document handoffs validated and external-file permissions handled by each harness?
- What is the issue search/import scope for each source?
- What counts as a conflicting session, and how are dirty checkout, invalid base, or branch/worktree creation failures presented?
- How are panel focus, stage navigation, the all-project view, and hotkey discovery presented?
- How are stale handoffs or potential concurrent-document edit conflicts surfaced?
- Which terminal features can the chosen host preserve, and which limitations, if any, will the user accept?
- How are shutdown, crashes, orphan processes, and failed or unavailable conversation resumes handled?

## Documentation process

This brief is being developed through discussion. Confirmed choices are recorded here; unsettled choices must remain explicit rather than being silently filled in by the implementing agent. See architecture.md and implementation-plan.md for the proposed technical boundaries and feasibility-first execution path. Those documents distinguish accepted product behavior from unvalidated implementation choices.
