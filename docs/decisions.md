# LazyMate — Decision Register

Status: Initial handover draft. Accepted entries reflect product discussion. Proposed/open entries are not implementation requirements. Update this register when technical evidence resolves a choice.

| Decision | Status | Rationale / constraint |
|---|---|---|
| First user: author with existing agent configuration | Accepted | Optimize for actual recovery and progress on their work. |
| Priorities: recover context, identify attention needs, parallel work, reduce setup | Accepted | Avoid optimizing agent-fleet infrastructure before navigation is useful. |
| One project per Git repository; register existing checkout | Accepted | Simple project boundary; worktrees are not separate projects. |
| Project-local view with all-project toggle | Accepted | Focus by default; parallel work remains visible. |
| Lazygit-inspired panels and keyboard navigation | Accepted | Project selection populates active items below. |
| GitHub + Jira + local tasks at minimum release | Accepted | Both hobby and work use are required. |
| One remote provider per project initially | Accepted | Avoid multi-source configuration complexity. |
| Pi + Claude Code at minimum release | Accepted | Preserve native interfaces and approved existing setup. |
| Project harness default; stage override before launch | Accepted | Work and hobby projects may differ; no conversation conversion. |
| Separate conversation per stage | Accepted | Retain stage history and fresh review context. |
| LazyMate-owned stage instructions and signal CLI | Accepted | Avoid requiring personal skills or a new MCP server. |
| Shared item Markdown in per-user app data | Accepted | Survives upgrades/worktrees without repo pollution. |
| App-owned authoritative workflow state, agent-owned narrative | Accepted | Markdown prose cannot unlock transitions. |
| Grill → Implement → Review; Investigate → Implement → Review | Accepted | Two fixed workflows; configurable workflows deferred. |
| Grill skippable; Investigate/Implement mandatory | Accepted | Flexible clarification, explicit implementation gate. |
| In-session confirmation before Grill/Investigate readiness | Accepted | No redundant approval UI; manual start authorizes implementation. |
| All stage starts manual; Review optional | Accepted | No scheduling engine or automatic review launch. |
| Review reports first; edits only when asked | Accepted | Lightweight evidence-based refinement, not speculative audits. |
| Agent readiness signals plus manual recovery | Accepted | Exit code/prose is not completion; missing signals cannot trap work. |
| Optional worktrees created by LazyMate | Accepted | Isolation available even without a concurrency warning. |
| Warn, permit shared checkout; Grill exempt | Accepted | User controls concurrency risk. |
| No Git lifecycle automation beyond worktree creation | Accepted | No cleanup/merge/push/commit on Done. |
| Live sessions only while controller runs | Accepted | Avoid a detached daemon in the MVP. |
| macOS/Ghostty first; portable boundaries | Accepted | Test actual environment without claiming broad compatibility. |
| Return shortcut Ctrl+B, configurable | Proposed default | Reserved by LazyMate; check conflicts and forwarding behavior. |
| Terminal backend / language / TUI framework | Open: feasibility spike | Native screen/input restoration must be demonstrated first. |
| Persistence library and schema | Open | Small durable state; no database chosen yet. |
| Signal transport and execution validation | Open | Local CLI reports must be safe against stale/duplicate reports. |
| Existing Claude Jira access uses MCP | Confirmed | Specific server, deployment, auth and approved direct access remain unknown. |
| GitHub import search scope | Accepted | Search the project's configured GitHub repository initially. |
| Jira search defaults | Open | Decide concrete scope without assuming Jira project equals repo. |
| Worktree defaults | Accepted | Outside repo under LazyMate app data; suggest item-based branch; show base branch/commit for confirmation/change; never silently copy uncommitted changes. |
| Concurrency warning scope | Open detail | Define which managed sessions conflict; avoid claiming external-process discovery. |
| Human-action blocker recovery | Accepted | Persistent item note, Open session and Clear blocker actions; clearing does not resume/prompt agent automatically. |
| Concurrent shared-document edits | Accepted MVP limitation | Agents update relevant sections, not the whole file; no collaborative editor/agent-write locking. LazyMate uses safe writes; arbitrary agent edits may conflict. |
| Shutdown/crash/orphan handling | Open: spike | Orderly shutdown agreed; abnormal exits need bounded handling. |
| Native feature exceptions | Open: spike | Goal is full functionality; obtain agreement on specific losses. |
| Model/provider controls in LazyMate | Not committed | Native sessions already expose configuration. |
