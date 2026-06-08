# human-in-loop-workflow

A human-in-loop, simplified enough, well-behaviored workflow for
small-to-medium projects. It defines a dual-agent collaboration protocol where
one agent implements and another reviews, with the human coordinator looped in
at every review gate.

## What This Is

A set of Markdown documents that define **how** multiple LLM agents should
collaborate on a project. It is not a tool, not a framework, not a dependency —
just a protocol in plain text that any LLM coding agent can read and follow.

## Why

Coding agents are powerful, but without explicit collaboration rules they
wander, self-approve, expand scope, and leave inconsistent handoffs. This
workflow gives agents a shared contract: roles, review gates, sign-off rules,
and a live handoff surface (`CURRENT.md`) that keeps state visible between
sessions.

It was extracted from real use across 8 phases of [tiny-duo-infer][tdi], a
dual-model inference engine built entirely through this pattern.

## How It Works

```
Phase 1 spec + taskboard   ← human + agents brainstorm together
       │
       ▼
┌──────────────────────┐
│  Main Developer      │  implements task → writes handoff
│  (agent session 1)   │  → sets status to "review"
└──────┬───────────────┘
       │  CURRENT.md (live state)
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
- **Dual-agent**: Main Developer implements, Reviewer signs off
- **No self-sign-off**: the agent who builds the thing cannot mark it done
- **`CURRENT.md`**: the live handoff surface — current task, review findings,
  test results, blockers
- **Human-in-the-loop**: nothing ships without human awareness

## What's in the Repo

| File | Role | Audience |
|------|------|----------|
| `SKILL.md` | Bootstrap installer for agent skill systems | Agents |
| `AGENTS.md` | Entry point, routes agents to the right docs | Agents |
| `agent-guidelines.md` | Full collaboration protocol: roles, review gates, handoff format, conflict rules | Agents |
| `CURRENT.md` | Live task state template | Agents |
| `phase-spec-template.md` | Phase spec template with fill-in guidance | Human + agents |
| `taskboard-template.md` | Taskboard template with status rules | Human + agents |
| `phases/README.md` | Phase index template | Human + agents |

## Getting Started

### For skill-aware LLM coding agents

If your agent supports skill files, install `SKILL.md` from this repo. The
skill will bootstrap the protocol documents into your project's `docs/`
directory automatically.

### For other LLM coding agents

Clone or copy the protocol documents into your project:

```bash
# Option A: clone the whole repo
git clone https://github.com/jameswei/human-in-loop-workflow.git /tmp/hilw
cp /tmp/hilw/{AGENTS.md,docs/*} your-project/

# Option B: fetch individual files
curl -o AGENTS.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/AGENTS.md
curl -o docs/agent-guidelines.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/main/agent-guidelines.md
# ... repeat for each file
```

Any agent that reads project files will pick up `AGENTS.md` and follow the
protocol.

### For web-based LLMs (ChatGPT, Claude, etc.)

Paste `AGENTS.md` and `agent-guidelines.md` at the start of your session as
context. This is less automatic but still works.

## Project Defaults

After copying the protocol documents, edit the **Project Defaults** section at
the end of `docs/agent-guidelines.md` to match your project's conventions:
language, framework, testing tools, phase-1 constraints, etc. This is the only
customization step — everything else is ready to go.

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
