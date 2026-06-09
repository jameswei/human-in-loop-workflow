# human-in-loop-workflow

A plain-text, human-in-the-loop workflow for small-to-medium software projects.
One agent implements, another reviews, and the human coordinator stays involved
at every review gate and phase boundary. No tools, no frameworks — just a
protocol in Markdown that any LLM coding agent can read and follow.

## Why

Coding agents are powerful, but without explicit collaboration rules they
wander, self-approve, expand scope, and leave inconsistent handoffs. This
workflow gives agents a shared contract: roles, review gates, sign-off rules,
and a live handoff surface (`docs/CURRENT.md`) for active tasks.

## How It Works

```
Bootstrap workflow         ← fresh repo or existing/forked project
       │
       ▼
Planning brainstorm        ← human + agents brainstorm together
       │
       ▼
Phase N spec + taskboard   ← human approves scope before implementation
       │
       ▼
┌──────────────────────┐
│  Main Developer      │   implements task → writes handoff → sets status to "review"
│  (agent session 1)   │
└──────┬───────────────┘
       │  docs/CURRENT.md (live state)
       ▼
┌──────────────────────┐
│  Reviewer            │   reviews code → writes findings → signs off or requests fixes
│  (agent session 2)   │
└──────┬───────────────┘
       │
       ▼
   Human as coordinator    looped in at every review gate and phase boundary
```

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

### For Claude Code

Either install from plugin marketplace:

```text
/plugin marketplace add jameswei/human-in-loop-workflow
/plugin install human-in-loop-workflow@human-in-loop-workflow
```

or simply fetch single skill file:

```bash
mkdir -p ~/.claude/skills/human-in-loop-workflow && \
  curl -o ~/.claude/skills/human-in-loop-workflow/SKILL.md \
  https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/SKILL.md
```

Then prompt like following:

```text
Use the human-in-loop-workflow skill to bootstrap this repository.
Only install or adapt workflow docs. Do not change product code.
```

### For Codex

```text
$skill-installer install https://github.com/jameswei/human-in-loop-workflow/tree/{tag}
```

Then prompt like following:

```text
Use the human-in-loop-workflow skill to bootstrap this repository.
Only install or adapt workflow docs. Do not change product code.
```

### For other LLM coding agents

Copy the protocol docs into your project, then any agent that reads project
files will pick up `AGENTS.md` and follow the protocol:

```bash
curl -o AGENTS.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/AGENTS.md
mkdir -p docs/phases
curl -o docs/agent-guidelines.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/docs/agent-guidelines.md
curl -o docs/current-task-template.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/docs/current-task-template.md
curl -o docs/phase-spec-template.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/docs/phase-spec-template.md
curl -o docs/taskboard-template.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/docs/taskboard-template.md
curl -o docs/phases/README.md https://raw.githubusercontent.com/jameswei/human-in-loop-workflow/refs/tags/{tag}/docs/phases/README.md
```

## FAQ

**Do I need two different LLM models?** No. The two agents can be the same model
running in separate sessions. What matters is separate sessions — the Reviewer
must not share context with the Developer, otherwise the review gate is
meaningless.

**Can I use more than two agents?** Yes. The protocol defines four roles (Main
Developer, Architecture Reviewer, Code Reviewer, Test Verifier). Two agents
splitting these roles is typical for small projects; larger projects can assign
one per role.

**Is this overkill for a solo project?** For a one-file script, yes. For
anything with multiple phases and the risk of agents going off the rails — the
overhead is about 30 minutes of initial setup and pays back in fewer reverted
commits and clearer handoffs.
