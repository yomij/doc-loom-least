# Doc Loom Least

[中文版](README_CN.md)

> A minimal, document-driven personal product workflow substrate. The current slice is AI-assisted development: no CLI backend, no heavy pipeline, just skills and Markdown.

Doc Loom Least helps you and your AI agent stay aligned on three questions throughout a workflow:

- **What facts should we trust right now?**
- **What documents should we create or update?**
- **Which decisions need human confirmation first?**

It does this with a small set of Agent Skills and lightweight Markdown artifacts — nothing more.

## Why Doc Loom Least?

AI agents are powerful but forgetful. In development work, they drift from context, skip confirmation on risky changes, and leave behind no record of what happened or why.

Doc Loom Least currently solves this for development work with the smallest possible mechanism: a set of skills that gate key moments (planning, execution, closure), a shared protocol that keeps facts, case state, and artifacts consistent across sessions, and a bounded discovery path for finding the next small development slice without bypassing confirmation.

Its broader direction is a personal product workflow substrate that can later carry product, research, design, release, and operations flows. In that sense it is a repo-native, skill-based platform, not a CLI tool, daemon, app backend, or pipeline product.

## Principles

These are non-negotiable, drawn from the [Constitution](docs/authority/constitution.md):

| Principle | What it means |
|---|---|
| **Enter through the minimum path** | Use the narrowest contract, artifact, or skill that solves the real problem. No ceremony for ceremony's sake. |
| **Don't become a complex pipeline** | Doc Loom Least is a workflow, not a product. No CLI backend, no heavy orchestration. |
| **Human-semantic design** | Every document, prompt, and output must be readable, understandable, and beautiful. Meaning over mechanics. |
| **Human experience comes first** | Agents own discoverable work; users are interrupted only for real decisions, external facts, explicit authorization, or high-risk actions. |

## Lifecycle Scope

The current supported lifecycle domain is **development**. Future domains such as product, research, and design should enter only as narrow skills with clear workflow boundaries. Empty roadmap directories and speculative stages are intentionally avoided.

## What v1 Does NOT Do

These boundaries keep the project minimal by design:

- No CLI backend or daemon
- No heavy orchestrator skill
- No automatic review triggers — reviews are always manual
- No treating `review_risk` as review authorization

Simple doc edits and low-risk local changes take the minimum path. Only persistent development work enters the full case workflow with plans, execution records, and closure.

## Skills

| Skill | Role |
|---|---|
| `docloom-workflow` | Entry point & lightweight router — parses task state, reports case status, discovers next-slice candidates, and routes to the right stage skill |
| `setup-doc-governance` | Governance init & maintenance — scans docs, extracts facts, produces governance plans |
| `context-authority` | Conditional fact authority gate — reads minimal context, resolves conflicts, issues routing verdict |
| `plan-confirm` | Planning gate — generates plans with risk levels, TDD strategy, and waits for your approval |
| `tdd-execute` | Execution gate — Red-Green-Refactor cycle with evidence, or recorded TDD exceptions |
| `doc-sync-close` | Closure gate — syncs docs, records acceptance, risks, and follow-ups |
| `review` | Manual read-only review of designs, diffs, tests, docs, or case evidence |
| `grill` | Manual interactive stress-test of requirements, designs, or claims |

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
    └── assessment/            # Manual review and challenge helpers
```

## Typical Usage

### Quick doc edits (no case needed)

For one-off, low-risk documentation changes — read the [Constitution](docs/authority/constitution.md), then edit the target doc. No case, no plan, no execution record.

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

### Persistent development case

For tasks that need planning, execution, acceptance, and a closure record:

```
docloom-workflow
  → context-authority (when context gate is needed)
  → plan-confirm
  → you approve the plan
  → tdd-execute
  → doc-sync-close
```

`docloom-workflow` only routes — it never replaces a stage skill.

### Case status & next-slice discovery

When you want to know where things stand, `docloom-workflow` can read the
derived case dashboard and the relevant case artifacts, then report the current
phase, evidence, next skill, and any required input.

When you ask what to build next, it can use
[`docs/product/current-state.md`](docs/product/current-state.md), case
follow-ups, and targeted repo evidence to recommend ranked next-slice
candidates. A recommendation is not execution authorization: once you choose a
candidate, the work still goes through normal case identity, planning, approval,
execution, and closure.

### Manual review & stress-test

Use `review` and `grill` only when you explicitly ask:

- `review` — read-only assessment with findings and evidence gaps. No files written, no state changed.
- `grill` — interactive challenge of claims, one question at a time. No artifacts, no workflow routing.

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

In environments that support Agent Skills without `skillshare`, copy or symlink the desired directories that contain `SKILL.md` from the grouped `skills/` tree into your skills directory and invoke them by name:

```
Use docloom-workflow to continue the current case.
Use setup-doc-governance to organize docs-only governance.
Use review to audit this plan.
Use grill to stress-test this API design.
```

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
