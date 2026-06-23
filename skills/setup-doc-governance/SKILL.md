---
name: setup-doc-governance
description: Set up or rebuild documentation governance for a project. Use when the user asks to initialize, organize, rebuild, or repair docs governance.
---

# setup-doc-governance

Initialize, rebuild, or periodically clean a project's documentation governance.
Historical docs are evidence. Authority docs are confirmed source of truth.
Each independent governance batch gets its own plan file.

Read when trigger condition is met:

- `references/shared-protocol.md` when: checking execution instruction order,
  fact authority order, risk levels, or artifact policy.
- `references/layering-and-routing.md` when: writing authority structure, index
  rules, bridge rules, or archive rules.
- `references/verdicts.md` when: routing files or facts, writing decision
  tables, or deciding block conditions.
- `../doc-sync-close/references/doc-update-rules.md` when: checking authority
  lifecycle status values.

## Scope

Use one fixed workflow. Scope controls scanning breadth:

| Scope | Meaning |
|---|---|
| `current-case` | Govern only current case materials such as drafts, evidence, plan, closure, and local migrations. |
| `docs-only` | Default. Scan docs, README, ADRs, entry indexes, and historical docs. |
| `full-repo` | Add relevant code and tests as read-only evidence. |

Default to `docs-only`. Escalate to `full-repo` only when authority claims need
code/test verification or the user explicitly asks to use code/tests as
evidence. Never modify code, tests, build scripts, or runtime behavior.

Use a bounded scan by default: docs entry points, existing governance and
authority docs, ADRs, and explicitly relevant historical docs. Do not scan the
whole repository unless `full-repo` is selected.

## Outputs

Default:

```text
docs/governance/YYYY-MM-DD-<slug>.md   # independent governance batch
docs/cases/<case-id>/governance-plan.md  # governance tied to an active case
docs/authority/README.md
docs/authority/**                 # only sections with confirmed facts
docs/cases/<case-id>/evidence/**  # when case evidence is needed
docs/archive/**                   # when archival is applied
docs/README.md
bridge files                      # only when old entries are likely to mislead
```

Do not write a new independent governance batch into a prior plan file.
If `docs/governance/GOVERNANCE_PLAN.md` exists from an older design, read it
as legacy context only; write new batches to the date-named path.

Do not invent artifact types outside the shared Artifact Policy or a confirmed
governance plan.

## Workflow

1. Resolve scope and plan path.
   - If a case is active and governance is tied to that case:
     `docs/cases/<case-id>/governance-plan.md`.
   - Otherwise: `docs/governance/YYYY-MM-DD-<slug>.md`.
   - If the target path already exists, choose a unique slug or stop and ask.
2. Inventory relevant docs and entries.
3. Extract facts as statement, source, evidence, scope, and risk.
4. Route files and facts with `promote`, `merge`, `bridge`, `archive`, or
   `block`.
5. Write the governance plan with `status: proposed`.
6. Stop for one user confirmation of the plan.
7. After confirmation, set `status: approved` and apply non-blocked decisions.
8. Write applied result back into the same governance plan.
9. Update the docs entry index after applying decisions.

The first plan write is not permission to move, archive, bridge, or update
authority docs.

## Authority Rules

Create only authority sections that have confirmed facts. Do not create empty
folders or placeholder docs.

Minimum authority entry:

```text
docs/authority/README.md
```

Authority docs use minimum frontmatter:

```yaml
---
status: active
authority: true
layer: authority
type: product
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
---
```

Allowed status values: see `../doc-sync-close/references/doc-update-rules.md`
-> Lifecycle Status.
High-risk authority docs must include `owner` and `last_verified`.

Unconfirmed raw material belongs in case evidence or archive, not authority.
User-provided new facts enter the governance plan for confirmation; they do not
silently become authority.

## Confirmation And Application

Governance plan frontmatter:

```yaml
---
status: proposed
plan_version: 1
scope: docs-only
governance_batch: YYYY-MM-DD-short-slug
approved_by:
approved_at:
---
```

After user confirmation:

```yaml
status: approved
approved_by: user
approved_at: <timestamp>
```

If the plan materially changes, increment `plan_version` and return to
`status: proposed`.

When applying:

- Execute non-blocked decisions.
- Skip decisions that depend on blocked facts.
- Use `applied` when all applicable decisions were applied.
- Use `applied_with_blocks` when safe decisions were applied and blocks remain.
- Keep blocked decisions visible for owner decision.

## Gates

- Without a specified scope, use `docs-only`; do not block.
- Do not apply changes before user confirmation -> wait for user input.
- Do not write a new independent batch into an existing plan file. If the target path already exists, choose a unique slug or stop and ask.
- Do not write unconfirmed raw material into authority.
- Do not modify code, tests, build scripts, or runtime behavior.
- Do not make bridge files carry old facts.
- Do not treat evidence, archive, derived, or generated docs as current facts.
