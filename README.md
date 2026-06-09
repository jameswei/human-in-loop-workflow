# human-in-loop-workflow

A plain-text, human-in-the-loop workflow for small-to-medium software projects.
It defines a dual-agent collaboration protocol where one agent implements,
another reviews, and the human coordinator stays involved at review gates and
phase boundaries.

## What This Is

A set of Markdown documents that define **how** multiple LLM agents should
collaborate on a project. It is not a tool, not a framework, not a dependency —
just a protocol in plain text that any LLM coding agent can read and follow.

## Why

Coding agents are powerful, but without explicit collaboration rules they
wander, self-approve, expand scope, and leave inconsistent handoffs. This
workflow gives agents a shared contract: roles, review gates, sign-off rules,
and a live handoff surface (`docs/CURRENT.md`) for active tasks.

It was extracted from real use across 8 phases of [tiny-duo-infer][tdi], a
dual-model inference engine built entirely through this pattern.

## How It Works

```
Bootstrap workflow docs    ← fresh repo or existing/forked project
       │
       ▼
Planning discussion        ← human + agents brainstorm together
       │
       ▼
Phase 1 spec + taskboard   ← human approves scope before implementation
       │
       ▼
┌──────────────────────┐
│  Main Developer      │  implements task → writes handoff
│  (agent session 1)   │  → sets status to "review"
└──────┬───────────────┘
       │  docs/CURRENT.md (live state)
       ▼
┌──────────────────────┐
│  Reviewer            │  reviews code → writes findings
│  (agent session 2)   │  → signs off or requests fixes
└──────┬───────────────┘
       │
       ▼
   Human coordinator       looped in at every review gate
                           and phase boundary
```

- **Phases**: work is organized into scoped phases, each with a spec (what) and
  a taskboard (tasks + status)
- **Bootstrap Mode**: installs or adapts the workflow in a repo without
  changing product code
- **Planning Mode**: human and agents settle direction before any active phase
- **Dual-agent**: Main Developer implements, Reviewer signs off
- **No self-sign-off**: the agent who builds the thing cannot mark it done
- **`docs/CURRENT.md`**: the live handoff surface — current task, review
  findings, test results, blockers
- **Human-in-the-loop**: nothing ships without human awareness

## What's in the Repo

| File | Role | Audience |
|------|------|----------|
| `SKILL.md` | Bootstrap installer for agent skill systems | Agents |
| `AGENTS.md` | Entry point, routes agents to the right docs | Agents |
| `docs/agent-guidelines.md` | Full collaboration protocol: lifecycle, roles, review gates, handoff format, conflict rules | Agents |
| `docs/current-task-template.md` | Template for the live `docs/CURRENT.md` task state | Agents |
| `docs/phase-spec-template.md` | Phase spec template with fill-in guidance | Human + agents |
| `docs/taskboard-template.md` | Taskboard template with status rules | Human + agents |
| `docs/phases/README.md` | Phase index template | Human + agents |

## Getting Started

### For skill-aware LLM coding agents

Install `SKILL.md` as an agent skill, then ask the agent:

```text
Use the human-in-loop-workflow skill to bootstrap this repository.
Only install or adapt workflow docs. Do not change product code.
```

### For other LLM coding agents

Clone or copy the protocol documents into your project:

```bash
# Option A: clone the whole repo
git clone https://github.com/jameswei/human-in-loop-workflow.git /tmp/hilw
cp /tmp/hilw/AGENTS.md your-project/
cp -R /tmp/hilw/docs your-project/

# Option B: fetch individual files
curl -o AGENTS.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/AGENTS.md
mkdir -p docs/phases
curl -o docs/agent-guidelines.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/docs/agent-guidelines.md
curl -o docs/current-task-template.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/docs/current-task-template.md
curl -o docs/phase-spec-template.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/docs/phase-spec-template.md
curl -o docs/taskboard-template.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/docs/taskboard-template.md
curl -o docs/phases/README.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/docs/phases/README.md
```

Any agent that reads project files will pick up `AGENTS.md` and follow the
protocol.

### For web-based LLMs (ChatGPT, Claude, etc.)

Paste `AGENTS.md` and `docs/agent-guidelines.md` at the start of your session
as context. This is less automatic but still works.

## Bootstrap And Planning

Bootstrap works for empty repos and existing/forked projects. In both cases,
the agent should install or adapt workflow docs, inspect local conventions,
report conflicts, and stop. It should not change product code, choose a stack,
or start implementation.

After bootstrap, use Planning Mode with human and agent discussion to settle the
project direction. Only then should an agent write a phase spec and taskboard,
and only after human approval should implementation begin.

The **Project Defaults** section at the end of `docs/agent-guidelines.md` starts
unset by design. Fill it from existing project facts or confirmed planning
decisions, not guesses.

## FAQ

**Does this only work with a specific tool?** No. The protocol is plain Markdown
that any LLM coding agent can read. The `SKILL.md` is a convenience for agents
that support skill files, but the workflow itself is tool-agnostic — agents can
also read the protocol documents directly.

**Do I need two different LLM models?** No. The two agents can be the same model
running in separate sessions. What matters is that they are different
*sessions* — the Reviewer session must not have the same context/state as the
Developer session, otherwise the review gate is meaningless.

**Can I use more than two agents?** Yes. The protocol defines four roles (Main
Developer, Architecture Reviewer, Code Reviewer, Test Verifier). For small
projects, two agents splitting these roles is typical. For larger projects, you
can assign dedicated agents per role.

**Is this overkill for a solo project?** For a one-file script, yes. For
anything with multiple phases, architecture decisions, and the risk of agents
going off the rails — the overhead is about 30 minutes of initial setup and
pays back in fewer reverted commits and clearer handoffs.

[tdi]: https://github.com/jameswei/tiny-duo-infer
