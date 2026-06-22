---
name: setup-doc-governance
description: Set up or rebuild documentation governance for a project. Use when the user asks to initialize docs governance, organize project docs, rebuild authority docs, classify historical docs, extract facts from existing docs/code/tests, create or update a governance plan, migrate docs, bridge old entries, archive superseded docs, or repair the documentation index.
---

# setup-doc-governance

Initialize, rebuild, or periodically clean a project's documentation governance.
Historical docs are evidence. Authority docs are confirmed source of truth.
`GOVERNANCE_PLAN.md` is the single default approval surface.

Read when trigger condition is met:

- `../_shared/references/shared-protocol.md` when: checking execution instruction order,
  fact authority order, risk levels, or artifact policy.
- `references/layering-and-routing.md` when: writing authority structure, index
  rules, bridge rules, or archive rules.
- `references/verdicts.md` when: routing files or facts, writing decision
  tables, or deciding block conditions.

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
docs/governance/GOVERNANCE_PLAN.md
docs/authority/README.md
docs/authority/**                 # only sections with confirmed facts
docs/cases/<case-id>/evidence/**  # when case evidence is needed
docs/archive/**                   # when archival is applied
docs/README.md or docs/DOC_INDEX.md
bridge files                      # only when old entries are likely to mislead
```

Do not generate default `FACT_REGISTRY.md`, `KNOWLEDGE_CARDS.md`,
`CONFLICT_REPORT.md`, `AUTHORITY_REBUILD_PLAN.md`, or
`CONTEXT_PACK_POLICY.md`.

## Workflow

1. Resolve scope.
2. Inventory relevant docs and entries.
3. Extract facts as statement, source, evidence, scope, and risk.
4. Route files and facts with `promote`, `merge`, `bridge`, `archive`, or
   `block`.
5. Write `docs/governance/GOVERNANCE_PLAN.md` with `status: proposed`.
6. Stop for one user confirmation of the plan.
7. After confirmation, set `status: approved` and apply non-blocked decisions.
8. Write applied result back into the same governance plan.

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

Allowed `status`: `active`, `draft`, `superseded`, `archived`.
High-risk authority docs must include `owner` and `last_verified`.

Unconfirmed raw material belongs in case evidence or archive, not authority.
User-provided new facts enter the governance plan for confirmation; they do not
silently become authority.

## Confirmation And Application

`GOVERNANCE_PLAN.md` frontmatter:

```yaml
---
status: proposed
plan_version: 1
scope: docs-only
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
- Do not apply changes before user confirmation. → Output: "Governance plan written as proposed. Present plan to user and await confirmation."
- Do not create empty authority sections. → Output: "No confirmed facts for this section. Skip section creation."
- Do not write unconfirmed raw material into authority.
- Do not modify code, tests, build scripts, or runtime behavior.
- Do not make bridge files carry old facts.
- Do not treat evidence, archive, derived, or generated docs as current facts.
- Update the docs entry index after applying decisions.
