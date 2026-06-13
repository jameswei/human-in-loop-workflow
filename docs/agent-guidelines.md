# Agent Collaboration Guidelines

This document defines how multiple agents should collaborate on this project.

## Workflow Lifecycle

Use this lifecycle for projects that adopt this workflow:

1. Bootstrap Mode: install or adapt the workflow documents.
2. Planning Mode: human and agents brainstorm goals, risks, stack, and phase
   shape.
3. Phase Setup: move a phase proposal through Draft, Signed Off, and Active.
4. Implementation And Review: agents work task-by-task through the active
   taskboard.
5. Phase Closeout: reviewer verifies completion criteria, tests, and docs.

Phase states:

- Draft: proposed scope, spec, or taskboard for discussion only.
- Signed Off: the phase scope has been reviewed and accepted, but
  implementation still waits for activation.
- Active: `docs/phases/README.md` names the signed-off phase spec and
  taskboard; agents may claim tasks and implement.

No implementation work should begin until the phase is Signed Off and Active,
except for Bootstrap Mode changes that only install or adapt this workflow.

## Bootstrap Mode

Bootstrap Mode brings this workflow into the current repository without changing
product behavior. It may be used for a fresh repository or for an existing or
forked project.

Allowed in Bootstrap Mode:

- create missing workflow documents
- inspect existing project docs to understand local conventions
- record discovered conventions in Project Defaults
- identify conflicts with existing agent, contribution, or review docs
- propose planning questions or a future phase setup

Not allowed in Bootstrap Mode:

- change product code
- start implementation tasks
- invent product scope, stack, package manager, or test commands
- overwrite existing `AGENTS.md`, `CLAUDE.md`, `README.md`, `CONTRIBUTING.md`,
  or project governance docs without human approval
- mark a phase active without human-confirmed scope

For existing or forked projects, this workflow should wrap the upstream
contribution process, not replace it. If existing instructions disagree with
this workflow, call out the conflict and ask the human how to proceed.

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

- confirm the task belongs to the current phase
- follow the active phase spec
- keep changes narrow and intentional
- write clear docstrings and comments
- update docs when behavior, interfaces, or architecture decisions change
- run relevant tests before handoff
- produce a concise handoff note

Do not introduce major architectural changes without updating the phase spec or
project architecture docs.

### Architecture Reviewer

- verify subsystem boundaries remain clean
- check that phase scope is respected
- identify decisions that need durable documentation
- flag abstractions that are too broad, too narrow, or premature

Focus on design risks rather than formatting or small implementation style
issues.

### Code Reviewer

- verify correctness and maintainability
- check that public interfaces have useful docstrings
- check that non-obvious logic has explanatory comments
- look for unclear names, dense code, hidden assumptions, and missing edge cases
- verify tests cover the important behavior introduced by the change

Prioritize bugs, behavior regressions, and clarity.

### Test Verifier

- run the relevant tests
- run smoke tests when required hardware or artifacts are available
- record the operating system, runtime version, and hardware used
- report skipped tests with a clear reason
- report failing commands with enough output to diagnose the issue

Do not mark a phase complete if required tests were skipped without an explicit
documented reason.

## Collaboration Flow

Use this flow for normal implementation tasks:

0. If `docs/CURRENT.md` exists, read it first for live task state and any open
   review findings. Then read the phase spec and taskboard for full context.
   `docs/CURRENT.md` is a fast pointer, not a replacement for the spec and
   taskboard.
1. If starting a new active task and `docs/CURRENT.md` does not exist, create it
   from `docs/current-task-template.md`.
2. Read the relevant phase spec and existing code.
3. Confirm the task belongs to the current phase.
4. Make the smallest change that satisfies the task.
5. Keep code readable and explicit.
6. Update docs if the change affects behavior, public interfaces, architecture,
   or phase scope.
7. Run relevant tests.
8. Produce a handoff note. Update `docs/CURRENT.md` status to `review`.
9. Reviewer checks the change against the role-specific checklist. Reviewer
   writes findings directly to `docs/CURRENT.md` under "Findings From Last
   Review", tagged `[Blocking]`, `[Non-blocking]`, `[Nit]`, or `[Question]`.
   Reviewer sets `Review Result` to `changes_requested` or `signed_off` and
   updates `Last Updated` / `Updated By`.
10. Test verifier records test results and environment details in
   `docs/CURRENT.md` under "Tests Reviewed".
11. A non-owner agent records sign-off in both `docs/CURRENT.md` (`Review
    Result: signed_off`) and the taskboard `Notes` before the task is marked
    `done`.
12. After a task is committed, the agent who committed resets
    `docs/CURRENT.md` for the next task. If no next task exists (phase is fully
    closed), delete `docs/CURRENT.md` — the taskboard is the permanent record.
    A stale "done" file misleads the next session more than a missing file
    does.

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

Use these gates to avoid unclear or unverified changes:

| Gate | Required when a change... |
|---|---|
| Architecture review | changes subsystem boundaries, public APIs, data layout, core assumptions, phase scope, or major dependencies |
| Code review | changes runtime behavior, core logic, tests, test strategy, or public interfaces |
| Test verification | claims a feature works, changes behavior, changes environment-specific behavior, or closes a phase criterion |
| Documentation update | changes public API usage, architecture, phase scope, durable decisions, limitations, or environment requirements |

## Conflict Handling

Handle conflicts explicitly:

- Docs and code disagree: identify the conflict, update the stale source when
  the correct direction is clear, or ask for clarification.
- Phase scope and requested work disagree: do not silently expand the phase;
  propose a phase-spec update or record the work as out of scope.
- Agents disagree on architecture: reduce the disagreement to a concrete
  decision and record the final choice durably.
- Required hardware or environment is unavailable: skip only dependent tests,
  run all independent tests, and report the skip reason.

## Testing Expectations

Agents should run the narrowest useful test set during development and broader
tests before handoff.

Test reports should include:

- command run
- pass, fail, or skip status
- reason for skip, if skipped
- relevant environment details

Suggested environment details: operating system, CPU architecture, runtime
version, backend or framework version, and hardware availability.

## Documentation Expectations

Documentation should stay close to the code and phase roadmap.

Add or update project architecture docs for subsystem boundaries, phase specs
for phase-level scope and acceptance criteria, and decision records for durable
architecture decisions.

Docs should be concise but concrete. Avoid vague claims such as "improve
performance" or "clean up architecture" without explaining what changed and why.

## Project Defaults

Status: Unset until discovered from the existing project or decided during
planning.

Agents must not assume language, framework, package manager, test command,
deployment target, architecture, or project scope before those choices are
recorded here, in a phase spec, or in an existing project authority such as
`README.md`, `CONTRIBUTING.md`, package metadata, or upstream docs.

During Bootstrap Mode, record only facts that are already true. During Planning
Mode or Phase Setup, update this section when the human confirms a decision.

- Language: unset
- Runtime: unset
- Package manager: unset
- Test command: unset
- Lint command: unset
- Typecheck command: unset
- Main source directory: unset
- Documentation conventions: unset
- Existing contribution rules: unset
