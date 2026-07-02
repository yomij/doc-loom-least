---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-02
---

# Documentation Governance

Documentation governance is owned by `setup-doc-governance`.

Historical docs are evidence. Authority docs are confirmed current source of
truth. Each independent governance batch gets its own plan file.

## Scope

| Scope | Meaning |
|---|---|
| `current-case` | Govern only current case materials. |
| `docs-only` | Scan docs, README, ADRs, entry indexes, and historical docs. |
| `full-repo` | Add relevant code and tests as read-only evidence. |

Default scope is `docs-only`. Escalate to `full-repo` only when authority
claims need implementation/test verification or the user explicitly asks for it.
Governance must not modify code, tests, build scripts, or runtime behavior.

## Batch Plan Path

Use the smallest path that fits the batch:

| Context | Plan path |
|---|---|
| Governance tied to an active case | `docs/cases/<case-id>/governance-plan.md` |
| Independent governance batch | `docs/governance/YYYY-MM-DD-<slug>.md` |

Do not write a new independent governance batch into a prior plan file or into
legacy `docs/governance/GOVERNANCE_PLAN.md`.

## Verdicts

Use one verdict set for files and facts:

| Verdict | Meaning |
|---|---|
| `promote` | Extract current facts into a new authority doc. |
| `merge` | Merge current facts into an existing authority doc. |
| `bridge` | Keep a thin entry pointing to current authority or archive. |
| `archive` | Move to archive; not current fact. |
| `block` | Conflict, high risk, or insufficient evidence requires owner decision. |

Authority sections are created only when they contain confirmed facts. Bridges
must be thin and must not carry old facts.

Do not create or migrate a constitution only because a conventional constitution
file is absent. If an active constitution already exists, moving it is a
confirmed authority change and must update the relevant index, SSOT map, and
decision log. Constitution content is limited to non-negotiable principles;
workflow rules and procedures belong in narrower authority docs or skills.

## Application Gate

The first governance plan write is not permission to apply changes. Application
requires user confirmation of the current plan. After confirmation, apply only
non-blocked decisions and keep blocked topics visible.

## Sources

- `skills/governance/setup-doc-governance/SKILL.md`
- `skills/governance/setup-doc-governance/references/layering-and-routing.md`
- `skills/governance/setup-doc-governance/references/verdicts.md`
- `skills/development/doc-sync-close/references/doc-update-rules.md`
