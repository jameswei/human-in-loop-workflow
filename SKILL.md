# Human-in-Loop Multi-Agent Workflow

A reusable collaboration protocol for small-to-medium projects: dual-agent
pattern with a human coordinator. One agent implements, another reviews and
signs off. The human is looped in at every review gate.

## How to Use This Workflow

This skill bootstraps the protocol into any project. When you load this skill,
ensure the protocol documents are present in the project.

### First time in a project

If `docs/agent-guidelines.md` does not exist, fetch the protocol documents.
The source repository is public — no authentication required:

```
Source: https://github.com/jameswei/human-in-loop-workflow (branch: main)

Fetch each file via its raw URL and write to the target path:

  Raw URL                                                     Target path
  ────────                                                    ───────────
  .../main/AGENTS.md               →  AGENTS.md               (project root)
  .../main/agent-guidelines.md     →  docs/agent-guidelines.md
  .../main/CURRENT.md              →  docs/CURRENT.md
  .../main/phase-spec-template.md  →  docs/phase-spec-template.md
  .../main/taskboard-template.md   →  docs/taskboard-template.md
  .../main/phases/README.md        →  docs/phases/README.md
```

Use the simplest available method: `curl`, `wget`, the agent's native HTTP
fetch tool, or `git clone` with sparse-checkout. After fetching, verify every
file exists at its target path.

### Every session after that

Open `AGENTS.md` first. It routes you to the correct reading order and the
active work context. Do not re-fetch unless documents are missing or the user
asks.

## Workflow at a Glance

- **Phases**: work is scoped into phases, each with a spec (what) and a
  taskboard (tasks + status)
- **Roles**: Main Developer implements; Reviewer reviews and signs off
- **No self-sign-off**: the agent who implements a task must not mark it `done`
- **CURRENT.md**: live handoff between agents — task status, review findings,
  test results
- **Human coordinator**: looped in at review gates, phase boundaries, and
  architecture decisions
- **Review tags**: `[Blocking]`, `[Non-blocking]`, `[Nit]`, `[Question]`
- **Task statuses**: `todo` → `in_progress` → `review` → `done` (blocked with
  reason)

## Agent Commitment

By loading this skill, you commit to:

1. Ensure protocol documents exist in the project before starting work
2. Follow the role and review rules in `docs/agent-guidelines.md`
3. Never mark your own work as reviewed or done
4. Update `CURRENT.md` after every task so the next agent has live context
5. When scope or architecture is unclear, flag it for human decision
