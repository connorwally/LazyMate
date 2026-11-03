# LazyMate — Implementation Plan

Status: Initial handover draft. Build one milestone at a time. The product's minimum release includes GitHub, Jira, local tasks, Pi, and Claude Code; earlier milestones are prototypes, not permission to drop release requirements.

Read [product-brief.md](product-brief.md), [architecture.md](architecture.md), and [decisions.md](decisions.md) first.

## Milestone 1 — Prove native terminal hosting

Use two fake items and real Pi/Claude Code sessions on macOS/Ghostty. Select the language and minimum dependencies needed for the spike, recording reasons. Do not scaffold unrelated application infrastructure.

Demonstrate:

- Enter starts/opens native sessions; a separate action starts without entering.
- LazyMate owns the return shortcut. No user tmux key configuration is required.
- Return leaves agent work running; switching back restores the live screen, including background output.
- Sessions remain distinct, resize correctly, and do not corrupt dashboard/outer-terminal state.
- Launch instructions and shared-document access work through normal harness permissions.
- Exact conversation identity can be captured and resumed for both harnesses.
- Interrupt, close, and warned controller shutdown behave as documented.
- Check relevant native keyboard/UI features, clipboard/images, and external editor behavior; disclose unsupported or untested behavior.

**Gate:** record demonstrated behavior, limitations, and chosen approach. Obtain agreement on meaningful native-functionality losses. If the interaction cannot be achieved, present alternatives; do not silently replace the native UI with RPC chat or proceed to ticket integration.

## Milestone 2 — Local projects, tasks, and durable navigation

- Register existing Git repositories; confirm project/source identity separately.
- Projects panel, active-items panel, stage navigation, focus/key hints, all-project toggle, finished visibility.
- Local task creation: project and title required, description optional.
- Choose a small durable store and app-data location; stable project/item/stage IDs.
- Persist conversation references and an initial shared item document.
- Project default harness; stage override before first launch.

Exit: two projects and several local items survive restart, stage conversations resume by exact identity, and failure to resume does not lose task data. Project names identify items even without colors.

## Milestone 3 — Workflows and reliable handoffs

- Implement the two fixed workflows and built-in stage instructions.
- Grill may be skipped; Investigate and Implement may not.
- Grill/Investigate require in-session confirmation before signaling ready.
- All starts are manual; implementation completion unlocks optional Review.
- Implementation retains normal self-checking. Review reports evidence-based findings first and edits only when asked.
- Shared-document update precedes explicit CLI ready/blocked signals. Agents update relevant sections without whole-file rewrites; no collaborative editor/agent-write locking in MVP. LazyMate uses safe writes.
- Human-action blockers persist with Open session and Clear blocker actions. Clearing does not automatically resume or prompt the agent.
- Validate execution-scoped signals, duplicate/stale reports, missing reports, manual readiness recovery, and mandatory-stage gates.
- Done hides an item; show-finished and reopen are distinct. Handle live-session choices and signals arriving after Done.
- Re-enter earlier stages without silently resetting/stopping downstream work.

Exit: complete both workflows on local tasks, including skipped Grill, mandatory Investigation, missing-signal recovery, optional Review, and manual Done. No process-exit or terminal-text completion inference.

## Milestone 4 — Optional worktree execution

- Offer existing checkout or creation of an item worktree before implementation.
- Warn on conflicting managed work; allow continuing after the warning; Grill is exempt.
- LazyMate creates requested worktrees outside the repo under app data by default, suggests an item-based branch name, and shows the base branch/commit for confirmation or change. Never silently copy uncommitted changes; handle creation failures safely.
- Implementation and Review use the same item working directory. Earlier sessions retain their directory.
- Do not merge, push, commit, delete branches, or clean up on Done.

Exit: two isolated items can work independently, shared-checkout override is explicit, and worktree errors do not launch into the wrong directory or lose task state.

## Milestone 5 — GitHub and Jira import

Investigate approved Jira deployment/access early enough to reveal blockers, but do not build full source UI before the terminal feasibility gate passes.

- One configured remote provider per project, plus local tasks.
- Verify Jira Cloud versus Data Center, existing approved integration, auth, and local/AI data permissions with the user. Do not assume Claude MCP credentials are directly usable.
- Implement read-only search/select/fetch/import for both GitHub and Jira through narrow adapters. GitHub search initially targets the project's configured GitHub repository. Existing Claude Jira access is confirmed MCP-based; determine the specific integration and approved access separately.
- Retain source identity, URL and snapshot; duplicate import within the project opens the existing item.
- Handle missing tools, login/permissions failures, empty results, and unreachable sources with actionable errors.
- Do not add writeback, scheduled sync, assigned-issue auto-import, or extra providers.

Exit: import authorized examples from both sources and run their tasks through native sessions. If Jira access is unavailable, document the blocker; do not label GitHub-only functionality the completed minimum release.

## Milestone 6 — Recovery and real use

- Bound shutdown and restore terminal settings after errors.
- Handle missing executable/history/worktree and stale session references.
- Validate data remains available after controller restart; document crash/orphan limitations.
- Make hotkeys discoverable and avoid accidental destructive actions.
- Use several real tickets to confirm recovery of prior work and clear next-step visibility.
- Finish installation, usage, requirements, known limitations, and a short manual checklist.

Exit: macOS/Ghostty behavior is honestly documented and the agreed minimum release works with both harnesses and ticket sources. Other platforms remain unverified unless actually tested.

## Proportionate validation

Use focused automated tests for workflow transitions, identity/deduplication, signal validation, and persistence where practical. Use short manual terminal smoke checks for native UI/input/resize/re-entry and approved provider integration. Do not build broad fake-terminal or multi-platform test infrastructure before the spike establishes the approach. Never use employer content as committed fixtures.

## First implementation-agent assignment

Implement Milestone 1 only. Deliver a runnable native-session navigation prototype, launch instructions, evidence from macOS/Ghostty checks, the selected approach and rationale, and an explicit limitations list. Ask about product-changing compromises rather than deciding them silently. Do not begin ticket integration until the terminal-hosting gate is satisfied.
