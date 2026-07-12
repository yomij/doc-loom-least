---
status: active
authority: true
layer: authority
type: architecture
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-12
---

# Repository And Skills

Doc Loom Least currently ships as Agent Skills and Markdown documentation. This
repository has no runtime source tree, package manifest, or test suite at the
time of verification.

Canonical skill groups:

| Directory | Role |
|---|---|
| `skills/development/` | Current development case workflow. |
| `skills/governance/` | Documentation and knowledge governance. |
| `skills/assessment/` | Read-only review and manual challenge helpers. |
| `skills/_shared/` | Shared protocol and templates; not directly invoked. |

Canonical skill invocation names:

| Skill | Source |
|---|---|
| `docloom-workflow` | `skills/development/docloom-workflow/SKILL.md` |
| `context-authority` | `skills/development/context-authority/SKILL.md` |
| `plan-confirm` | `skills/development/plan-confirm/SKILL.md` |
| `tdd-execute` | `skills/development/tdd-execute/SKILL.md` |
| `doc-sync-close` | `skills/development/doc-sync-close/SKILL.md` |
| `setup-doc-governance` | `skills/governance/setup-doc-governance/SKILL.md` |
| `review` | `skills/assessment/review/SKILL.md` |
| `grill` | `skills/assessment/grill/SKILL.md` |

`docloom-workflow` is the recommended human-facing doorway for persistent
development work, status/continuation, and discovery. The other invocation
names remain stable for explicit expert use and internal ownership, but normal
users should not need to choose a stage skill.

`review` supports explicit ad-hoc assessment and the workflow-owned read-only
Post-execution gate invoked by `tdd-execute` under an approved eligible plan.
`grill` remains explicit and manual. Assessment skills do not own case state,
workflow routing, implementation fixes, or closure.

## Loading Model

Skill design uses progressive disclosure: frontmatter descriptions define exact
triggers, each `SKILL.md` keeps only its owned procedure, the shared protocol
keeps cross-skill invariants, and references/templates load only for named
conditions. A material workflow rule has one complete semantic owner; consumers
retain only the local trigger, input, action, result, and gate they need.

Future lifecycle groups such as `product/`, `research/`, or `design/` must not
be created as empty roadmap placeholders. Add them only when real skills with
clear boundaries exist.

## Sources

- [ADR-0001](../../adr/ADR-0001-lifecycle-scope-and-skill-grouping.md)
- Current tracked `skills/**/SKILL.md` frontmatter.
- [skills/README.md](../../../skills/README.md)
