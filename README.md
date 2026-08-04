# Doc Loom Least

[中文版](README_CN.md)

> A minimal, document-driven personal product workflow substrate. The current slice is AI-assisted development: no CLI backend, no heavy pipeline, just skills and Markdown.

Doc Loom Least keeps you and your AI agent aligned on three questions throughout a workflow:

- **What facts should we trust right now?**
- **What documents should we create or update?**
- **Which decisions need human confirmation first?**

It does that with a small set of Agent Skills and lightweight Markdown artifacts.

## Architecture and Usage Flow

These diagrams summarize the repo-native architecture and the guarded-case
usage path. The animated GIFs are embedded for quick reading, with static PNG
previews linked below each diagram.

### Architecture

![Doc Loom Least architecture](docs/share/diagrams/doc-loom-architecture-en.gif)

[PNG preview](docs/share/diagrams/doc-loom-architecture-en.png)

### Usage flow

![Doc Loom Least usage flow](docs/share/diagrams/doc-loom-usage-loop-en.gif)

[PNG preview](docs/share/diagrams/doc-loom-usage-loop-en.png)

The usage illustration shows the guarded-case path. Reversible one-turn
low/medium work now runs directly, while compact persistent cases omit
untriggered execution, deep-review, and commit ceremony.

## Why Doc Loom Least?

AI agents are capable, but they forget. In development work they drift from context, skip confirmation on risky changes, and leave no record of what they did or why.

Doc Loom Least solves this for development work with the smallest mechanism that does the job: a set of skills that gate key moments (planning, execution, closure), a shared protocol that keeps facts, case state, and artifacts consistent across sessions, and a bounded discovery path for finding the next small development slice without bypassing confirmation.

The broader direction is a personal product workflow substrate that can later carry product, research, design, release, and operations flows. “Platform” here means a repo-native, skill-based workflow substrate — not a CLI tool, daemon, app backend, or pipeline product.

## Principles

These are non-negotiable, drawn from the [Constitution](docs/authority/constitution.md):

| Principle | What it means |
|---|---|
| **Enter through the minimum path** | Use the narrowest contract, artifact, or skill that solves the real problem. No ceremony for ceremony's sake. |
| **Don't become a complex pipeline** | Doc Loom Least is a workflow, not a product. No CLI backend, no heavy orchestration. |
| **Human-semantic design** | Every document, prompt, and output must be readable, understandable, and beautiful. Meaning over mechanics. |
| **Human experience comes first** | Agents own discoverable work; users are interrupted only for real decisions, external facts, explicit authorization, or high-risk actions. |

## Lifecycle Scope

The only lifecycle domain supported right now is **development**. Future domains like product, research, and design should enter as narrow skills with clear workflow boundaries. Empty roadmap directories and speculative stages are avoided on purpose.

## What v1 Does NOT Do

These boundaries are what keep the project minimal:

- No CLI backend or daemon
- No heavy orchestrator skill
- No automatic ad-hoc review triggers or treating `review_risk` as authorization
- No separate review phase or mandatory `review.md`; guarded, materially
  deviated, weakly verified, public/authority-sensitive, or explicitly
  requested work runs the read-only Post-execution gate inside `tdd-execute`

Workflow cost follows two independent questions: whether continuity or a
durable decision needs a case, and whether consequences or weak verification
need guarded assurance. Reversible one-turn low/medium work is direct; compact
persistent work uses a concise plan and closure; guarded work adds explicit
current-plan confirmation, exact-baseline review, and durable evidence.

## Skills

For normal use, start with `docloom-workflow` or simply ask the agent to manage
a persistent development task with Doc Loom. It chooses the internal owner;
you do not need to select a stage skill. The remaining names are stable advanced
entry points and implementation owners.

| Skill | Role |
|---|---|
| `docloom-workflow` | Recommended human-facing entry. Handles persistent feature/bug/refactor requests, status, continuation, and discovery, then routes internally. |
| `setup-doc-governance` | Governance init and maintenance. Scans docs, extracts facts, produces governance plans. |
| `context-authority` | Conditional fact authority gate. Reads minimal context, resolves conflicts, issues a routing verdict. |
| `plan-confirm` | Planning gate. Defines the outcome envelope, risk, TDD, and conditional review/commit strategy, then surfaces or records the applicable confirmation boundary. |
| `tdd-execute` | Execution gate for cases. Runs Red-Green-Refactor or a recorded exception, keeps evidence proportional, and owns triggered review/fix loops. |
| `doc-sync-close` | Closure gate. Syncs docs, records final evidence, and creates a completion commit only when declared. |
| `review` | Read-only ad-hoc review plus the workflow-owned Engineering/Spec Post-execution gate. |
| `grill` | Manual interactive stress-test of requirements, designs, or claims. |

## Repository Structure

```
.
├── INSTALL.md                 # Installation guide
├── CHANGELOG.md               # Version history
├── docs/
│   ├── index.md               # Documentation routing index
│   ├── ssot-map.md            # Source-of-truth map
│   ├── authority/             # Current governed facts, including constitution
│   ├── adr/                   # Architectural decision records
│   ├── cases/                 # Per-case artifacts and derived case dashboard
│   ├── product/               # Operational product-state inputs, not authority
│   ├── governance/            # Governance batch plans
│   └── archive/               # Historical and raw evidence, not current authority
└── skills/
    ├── README.md              # Skill grouping map
    ├── _shared/               # Cross-skill shared protocol
    ├── development/           # Current development flow skills
    ├── governance/            # Documentation governance skills
    └── assessment/            # Read-only review and manual challenge helpers
```

## Typical Usage

### One-shot reversible work (no case needed)

For reversible one-turn low/medium work, read the relevant project authority
and instructions, implement and verify directly, then report the result. No
case, plan, or execution artifact is required.

### Document governance

When you need to organize, rebuild, archive, or repair the doc system:

```
setup-doc-governance
  → choose scope: current-case | docs-only | full-repo
  → inventory docs & entry points
  → extract facts & evidence
  → generate governance plan
  → confirm with you
  → execute unblocked decisions
```

Default scope is `docs-only`. Only escalate to `full-repo` when authority claims need code or test evidence.

### Proportional development paths

First decide whether the work needs a durable case, then independently decide
whether it needs guarded assurance:

```
Direct: reversible one-turn low/medium work
  → normal repository execution + verification + final report; no case

Compact persistent: continuity, durable decision, or explicit case
  → concise plan → tdd-execute → closure
  → execution.md, deep review, and commits only when triggered

Guarded: high/public/authority-sensitive/irreversible/weakly verified
  → context-authority → plan-confirm → explicit current-plan approval
  → tdd-execute → Engineering + Spec review
  → coherent finding-fix batches + re-review when needed
  → doc-sync-close with declared durable evidence/commits
```

Medium risk alone does not create a case or require another confirmation.
Approval binds the Goal, Guardrails/Non-goals, Acceptance, and Escalation
Triggers; internal file discovery, tests, and ordinary implementation choices
adapt inside that envelope.

`docloom-workflow` routes internally—it never replaces stage ownership, and
normal users do not need to invoke those stages themselves.

### Case status & next-slice discovery

When you want to know where things stand, `docloom-workflow` can read the derived case dashboard and the relevant case artifacts, then report what is complete, what can happen next, and any decision it needs from you. Internal phase and skill names stay out of the default response unless they help diagnose a problem.

When you ask what to build next, it can pull from [`docs/product/current-state.md`](docs/product/current-state.md), case follow-ups, and targeted repo evidence to recommend ranked next-slice candidates. It labels facts inferred from the repo and asks only for missing human information that could materially change the ranking. A recommendation is not execution authorization: once you pick a candidate, the work still goes through normal case identity, planning, approval, execution, and closure.

### Review & stress-test

Ad-hoc `review` and `grill` run only when you explicitly ask:

- `review`: read-only assessment with findings and evidence gaps. No files written, no state changed.
- `grill`: interactive challenge of claims, one question at a time. No artifacts, no workflow routing.

Separately, guarded, materially deviated, weakly verified, public/authority-
sensitive, or explicitly requested work runs `review`'s read-only
Post-execution mode before closure. It returns the complete current Engineering
and Spec finding set; execution fixes coherent batches and re-reviews affected
axes.

## Installation

Doc Loom Least is distributed as Agent Skills. Install with [`skillshare`](https://github.com/anthropics/skillshare):

```bash
# Public repo
skillshare install github.com/yomij/doc-loom-least --track --json
skillshare sync

# Private repo (SSH)
skillshare install git@github.com:yomij/doc-loom-least.git --track --json
skillshare sync
```

Update:

```bash
skillshare check
skillshare update --all --diff
skillshare sync
```

For project-scoped installation and detailed verification steps, see [INSTALL.md](INSTALL.md).

In environments that support Agent Skills but don't have `skillshare`, copy or symlink the directories containing `SKILL.md` from the grouped `skills/` tree into your skills directory. The normal entry is:

```
Use Doc Loom to implement this persistent feature.
Use docloom-workflow to continue the current case.
```

Explicit advanced entry points remain available, for example `review` for a
read-only audit, `grill` for interactive pressure-testing, and
`setup-doc-governance` for documentation governance.

## Fact Authority Order

When maintaining this repo's own documentation, facts are resolved in this order:

1. Active authority docs
2. Current production code
3. Current tests
4. Accepted ADRs, migrations, or release notes
5. New user-provided information (as pending facts)
6. L2 operational case docs
7. L3 derived or index docs
8. L4 archive or historical docs
9. L5 scratch docs

`README.md` is the project entry point and navigation doc — not the highest authority. If it conflicts with the constitution, active authority, or current skill implementation, fix the README, not the other way around.

## License

[MIT](LICENSE)
