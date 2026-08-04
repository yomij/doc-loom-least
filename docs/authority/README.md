---
status: active
authority: true
layer: authority
type: operations
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-08-04
---

# Authority Index

This directory is the current authority entry for Doc Loom Least facts.
Historical designs, reference material, generated views, and indexes are
evidence or navigation only unless linked here as a source.

Authority order:

1. `docs/authority/constitution.md`.
2. Other active authority docs under this directory.
3. Current skill implementation under `skills/**`.
4. Current tests, when present.
5. Accepted ADRs, migration notes, or release notes.
6. User-provided new facts in the current task, as pending facts.
7. Case docs.
8. Derived or index docs.
9. Archive or historical docs.
10. Scratch notes.

## Current Authority

| Area | Authority |
|---|---|
| Active constitution | [constitution.md](constitution.md) |
| Product scope | [product/scope.md](product/scope.md) |
| Repository and skill architecture | [architecture/repo-and-skills.md](architecture/repo-and-skills.md) |
| Development workflow | [workflow/development-flow.md](workflow/development-flow.md) |
| Documentation governance | [workflow/doc-governance.md](workflow/doc-governance.md) |
| Agent policy | [agent/policy.md](agent/policy.md) |
| Distribution and install | [operations/distribution.md](operations/distribution.md) |
| Git commit titles | [operations/git.md](operations/git.md) |

## Workflow Policy Notes

Workflow assurance is proportional. Reversible one-turn low/medium work is
direct and creates no case. Continuity, a durable decision, or an explicit case
request uses a compact persistent case. High-risk, public/authority-sensitive,
irreversible, destructive, or weakly verified work is guarded and requires
explicit confirmation of the written current plan. Medium risk alone does not
create a case or another confirmation.
