# Agent Entry Point

If `docs/CURRENT.md` exists, read it first — it has the live state of the
active task, any open review findings, and the reviewer's last result. Then
continue with the full doc reading order below.

Before changing code, read the project docs in this order:

1. `docs/agent-guidelines.md`
2. `docs/phases/README.md`

If `docs/phases/README.md` names an active phase, also read that phase's spec
and taskboard before changing code.

If no phase is active, do not claim or start implementation work until the next
phase scope is confirmed and a phase spec/taskboard exists.

Use the active phase taskboard to claim tasks, update status, record blockers,
and mark review/done state.

Read additional project architecture or planning docs when a task changes
architecture, roadmap, public interfaces, or phase scope.

`AGENTS.md` is only an entrypoint. Detailed rules, status, scope, review gates,
and handoff expectations belong in `docs/agent-guidelines.md`.
