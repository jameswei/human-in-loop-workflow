# Agent Collaboration Guidelines

This document defines how multiple agents should collaborate on this project.

## Source Of Truth

Agents should read the relevant planning documents before changing code.

Use this source-of-truth order:

1. `docs/phases/README.md`: active phase pointer and completed/deferred phase
   index.
2. Active phase spec and taskboard named by `docs/phases/README.md`.
3. Project architecture and planning docs in `docs/`.
4. Code and tests: implementation truth.
5. Earlier proposal and review docs: historical context, not current
   implementation contracts.

If documents and code disagree, agents should not silently choose one. The agent
should call out the conflict and either update the relevant document as part of
the task or ask for clarification if the correct direction is unclear.

## Role Definitions

Each agent should operate in one clear role for a task. A single agent may fill
multiple roles only when the task is small, but the handoff should still state
which responsibilities were covered.

### Main Developer

The main developer implements scoped changes.

Responsibilities:

- confirm the task belongs to the current phase
- follow the active phase spec
- keep changes narrow and intentional
- write clear docstrings and comments
- update docs when behavior, interfaces, or architecture decisions change
- run relevant tests before handoff
- produce a concise handoff note

The main developer should not introduce major architectural changes without
updating the phase spec or project architecture docs.

### Architecture Reviewer

The architecture reviewer checks whether a change fits the intended project
design.

Responsibilities:

- verify subsystem boundaries remain clean
- check that phase scope is respected
- identify decisions that need durable documentation
- flag abstractions that are too broad, too narrow, or premature

The architecture reviewer should focus on design risks rather than formatting or
small implementation style issues.

### Code Reviewer

The code reviewer checks implementation quality.

Responsibilities:

- verify correctness and maintainability
- check that public interfaces have useful docstrings
- check that non-obvious logic has explanatory comments
- look for unclear names, dense code, hidden assumptions, and missing edge cases
- verify tests cover the important behavior introduced by the change

The code reviewer should prioritize bugs, behavior regressions, and clarity.

### Test Verifier

The test verifier runs tests and records reproducibility information.

Responsibilities:

- run the relevant tests
- run smoke tests when required hardware or artifacts are available
- record the operating system, runtime version, and hardware used
- report skipped tests with a clear reason
- report failing commands with enough output to diagnose the issue

The test verifier should not mark a phase complete if required tests were
skipped without an explicit documented reason.

## Collaboration Flow

Use this flow for normal implementation tasks:

0. If `docs/CURRENT.md` exists, read it first for live task state and any open
   review findings. Then read the phase spec and taskboard for full context.
   `CURRENT.md` is a fast pointer, not a replacement for the spec and taskboard.
1. Read the relevant phase spec and existing code.
2. Confirm the task belongs to the current phase.
3. Make the smallest change that satisfies the task.
4. Keep code readable and explicit.
5. Update docs if the change affects behavior, public interfaces, architecture,
   or phase scope.
6. Run relevant tests.
7. Produce a handoff note. Update `CURRENT.md` status to `review`.
8. Reviewer checks the change against the role-specific checklist. Reviewer
   writes findings directly to `CURRENT.md` under "Findings From Last Review",
   tagged `[Blocking]`, `[Non-blocking]`, `[Nit]`, or `[Question]`. Reviewer
   sets `Review Result` to `changes_requested` or `signed_off` and updates
   `Last Updated` / `Updated By`.
9. Test verifier records test results and environment details in `CURRENT.md`
   under "Tests Reviewed".
10. A non-owner agent records sign-off in both `CURRENT.md` (`Review Result:
    signed_off`) and the taskboard `Notes` before the task is marked `done`.
11. After a task is committed, the agent who committed resets `CURRENT.md` for
    the next task. If no next task exists (phase is fully closed), delete
    `CURRENT.md` — the taskboard is the permanent record. A stale "done" file
    misleads the next session more than a missing file does.

For large design changes, architecture review should happen before full
implementation.

## Review And Sign-Off Requirements

Every task or code change must be reviewed and signed off by an agent other
than the owner who made the change.

The implementing owner may:

- claim a task by setting it to `in_progress`
- move a task to `review` after implementation and local tests
- record test commands, skipped tests, known gaps, and handoff notes

The implementing owner must not:

- mark their own task as `done`
- record their own implementation as reviewed or accepted
- merge or close review-sensitive work without another agent's explicit sign-off

The reviewing agent is responsible for deciding whether the task can move from
`review` to `done`. When marking a task `done`, the reviewer must record their
agent name and the review result in the taskboard `Notes`, for example:
`reviewed by {agent}; {test command}: {result}; no findings`.

If the reviewer requests fixes, the task stays in `review` until the owner
applies the fixes and the reviewer signs off on the updated change.

## Handoff Format

Every substantial implementation task should end with a handoff note.

Use this format:

```markdown
## Handoff

### Task Summary

Briefly describe what changed and why.

### Files Changed

- `path/to/file`: short purpose of change

### Design Decisions

- Decision made and reason

### Tests Run

- `command`: pass/fail/skip

### Known Gaps

- Any limitation, skipped test, missing hardware, or incomplete follow-up

### Questions For Next Agent

- Open questions, if any
```

Small documentation-only changes may use a shorter handoff, but they should
still state what changed and whether tests were run.

## Review Gates

Use these gates to avoid unclear or unverified changes.

Architecture review is required when a change:

- introduces or changes subsystem boundaries
- changes public APIs
- changes data layout or core assumptions
- changes the phase roadmap or scope
- adds a new major dependency

Code review is required when a change:

- adds or changes runtime behavior
- adds core logic
- changes tests or test strategy
- changes public interfaces

Test verification is required when a change:

- claims a feature works
- changes behavior
- changes environment-specific behavior
- closes a phase completion criterion

Documentation updates are required when a change:

- changes public API usage
- changes architecture or phase scope
- adds a decision future agents should follow
- introduces known limitations or environment requirements

## Conflict Handling

Agents should handle conflicts explicitly.

If docs and code disagree:

- identify the conflict
- decide whether the code or docs should change
- update the stale source when the correct direction is clear
- ask for clarification when the correct direction is not clear

If phase scope and a requested change disagree:

- do not silently expand the phase
- propose a phase-spec update or record the change as out of scope

If two agents disagree on architecture:

- reduce the disagreement to a concrete decision
- record the final choice in a durable location for future agents

If required hardware or environment is unavailable:

- skip only the environment-dependent tests
- run all environment-independent tests
- report the skip reason clearly

## Testing Expectations

Agents should run the narrowest useful test set during development and broader
tests before handoff.

Test reports should include:

- command run
- pass, fail, or skip status
- reason for skip, if skipped
- relevant environment details

Suggested environment details:

- operating system
- CPU architecture
- runtime version
- backend or framework version
- hardware availability

## Documentation Expectations

Documentation should stay close to the code and phase roadmap.

Agents should add or update:

- project architecture docs for subsystem boundaries
- phase specs for phase-level scope and acceptance criteria
- decision records for durable architecture decisions

Docs should be concise but concrete. Avoid vague claims such as "improve
performance" or "clean up architecture" without explaining what changed and why.

## Project Defaults

Unless documented otherwise, agents should assume:

<!-- Fill in project-specific defaults below. Examples: -->
<!--   - Python 3.11+ implementation -->
<!--   - pytest for testing -->
<!--   - ruff for linting and formatting -->
<!--   - REST API with FastAPI and uvicorn -->
<!--   - PostgreSQL as the primary database -->
<!--   - no multi-tenant support in phase 1 -->
<!--   - single-user execution in phase 1 -->

- {default 1}
- {default 2}
