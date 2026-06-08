# Phase {N} Taskboard

The active implementation contract is `docs/phases/{spec-filename}.md`.

## Status Values

- `todo`: not started
- `in_progress`: actively being implemented
- `review`: implementation ready for review
- `blocked`: cannot proceed; blocker in Notes
- `done`: reviewed, tested, accepted

## Update Rules

- Set `in_progress` before starting work.
- Set `review` after implementation and local tests.
- Only a non-owner reviewing agent may set `done`.
- When marking `done`, record reviewer name and test result in `Notes`.
- Use `blocked` only with a concrete blocker in `Notes`.
- Keep `Owner` as an agent/person name or `unassigned`.
- Do not change task IDs after creation.
- Update `Notes` with skipped tests, environment limits, or follow-up work.
- Keep Acceptance criteria short; detailed requirements belong in the phase
  spec.

## Taskboard

<!-- Guidance: one row per task.
     ID format: P{N}-T{XX}
     Milestone groups related tasks (e.g. Planning, Core, Integration, Close).
     Each task's Acceptance should be a one-sentence, verifiable condition.
     Depends On prevents agents from starting work before dependencies are met. -->

| ID | Milestone | Task | Depends On | Status | Owner | Acceptance | Notes |
|---|---|---|---|---|---|---|---|
| P{N}-T00 | {milestone} | {description} | {dependency} | todo | unassigned | {acceptance} | |
| ... | | | | | | | |

## Review-Sensitive Tasks

<!-- Guidance: list task IDs that require architecture/code review before
     they can be marked done. Rules of thumb:
     - changes to public APIs
     - changes to architecture boundaries
     - changes to core data flow or assumptions -->

## Minimum Phase Completion

<!-- Guidance: list the minimum set of tasks required to complete this
     phase. Usually all non-optional tasks. -->

## Next Phase

<!-- Guidance: expected direction of the next phase (one sentence).
     Not a commitment — just orientation for agents reading this taskboard. -->
