# Human-in-Loop Multi-Agent Workflow

A reusable collaboration protocol for small-to-medium projects: dual-agent
pattern with a human coordinator. One agent implements, another reviews and
signs off. The human is looped in at every review gate.

## How to Use This Workflow

This skill bootstraps the protocol into any project. Bootstrap Mode may be used
for either a fresh repository or an existing/forked repository.

### Bootstrap

When the user asks to initialize, bootstrap, install, or adapt this workflow:

1. Check whether `AGENTS.md` and `docs/agent-guidelines.md` already exist.
2. If workflow docs are missing, create the missing target directories and fetch
   the protocol documents.
3. If existing project instructions are present, such as `AGENTS.md`,
   `CLAUDE.md`, `README.md`, or `CONTRIBUTING.md`, inspect them before changing
   anything. Do not overwrite them without explicit human approval.
4. For existing or forked projects, preserve upstream contribution rules and use
   this workflow as a coordination layer around them.
5. Leave `docs/phases/README.md` with no active phase unless the human has
   already approved a phase scope. Do not create `docs/CURRENT.md` until an
   implementation task is active; use `docs/current-task-template.md` when that
   live file is needed.
6. Report which files were created, skipped, or need human approval. Then stop.

Do not change product code, choose a stack, create implementation tasks, or start
implementation during Bootstrap Mode.

The source repository is public, so no authentication is required when fetching
the protocol:

```
Source: https://github.com/jameswei/human-in-loop-workflow (branch: main)

Fetch each file via its raw URL and write to the target path:

  Raw URL                                                     Target path
  ────────                                                    ───────────
  .../main/AGENTS.md               →  AGENTS.md               (project root)
  .../main/docs/agent-guidelines.md     →  docs/agent-guidelines.md
  .../main/docs/current-task-template.md  →  docs/current-task-template.md
  .../main/docs/phase-spec-template.md    →  docs/phase-spec-template.md
  .../main/docs/taskboard-template.md     →  docs/taskboard-template.md
  .../main/docs/phases/README.md          →  docs/phases/README.md
```

Use the simplest available method: `curl`, `wget`, the agent's native HTTP
fetch tool, or `git clone` with sparse-checkout. After fetching, verify every
file exists at its target path.

### After Bootstrap

Open `AGENTS.md` first. It routes you to the correct reading order and the
active work context. Do not re-fetch unless documents are missing or the user
asks.

If no phase is active, stay in Planning Mode or Phase Setup. Do not begin
implementation until the human approves a phase spec and taskboard.

## Core Rules

- Follow `docs/agent-guidelines.md`.
- Never mark your own work as reviewed or done.
- Update `docs/CURRENT.md` for active implementation tasks.
- Flag unclear scope, architecture, or upstream-process conflicts for human
  decision.
