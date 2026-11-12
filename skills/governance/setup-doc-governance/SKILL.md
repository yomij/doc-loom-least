---
name: setup-doc-governance
description: Establish or update documentation authority, SSOT precedence, knowledge promotion, archival/bridge routing, and current vs. historical status. Use for structural documentation governance or authority conflicts, not ordinary doc edits or business-rule archives.
---

# Setup Doc Governance

Use the user's language for all output, including templates, unless another language is requested.

## Scope

Infer the narrowest target scope from the request:

- `target_scope: current-case` governs only the identified case.
- `target_scope: repository` governs the requested repository documentation.

Select evidence scope independently:

- `evidence_scope: docs-only` (default) reads documentation only.
- `evidence_scope: repo-readonly` also reads code, tests, and configuration when
  needed to establish facts and permitted by user constraints.

Evidence access grants no write permission. Modify documentation only within
the authorized target (for example, current-case plus repo-readonly permits
repository evidence without permission to restructure repository authority).
Implementation fixes belong to docloom-workflow.

## Evidence

Read the declared constitution and relevant authority entries when present.
Inventory affected facts, sources, and entries. Promoted facts need identified
sources; inference alone cannot become authority. Inspect available evidence
before asking, and obtain authorization before expanding an explicitly
restricted evidence scope.

## Authority Decisions

- **Fact ownership and roles:** Follow declared precedence and fact ownership.
  When undeclared, preserve the constitution, use explicit owner decisions and
  unsuperseded ADRs for intended rules, and use code/tests for implemented
  behavior. Only sourced rules designated current by the repository or owner
  constrain future work: an `active` label alone does not create authority, and
  missing metadata requires checking ownership rather than automatically
  discarding existing rules. Draft/scratch is unverified; case records are task
  evidence; derived documents summarize or route; superseded/archived material
  is historical.
- **Conflict handling:** Resolve conflicts using declared precedence or existing
  owner decisions. Disagreement between intended rules and code/tests does not
  authorize rewriting either; if neither settles a binding-rule conflict, mark
  it blocked for owner decision.
- **Verdicts:** Use `promote` for a new authority fact, `merge` for an existing
  authority target, `bridge` for a thin old-entry pointer, `archive` for
  preserved history, and `block` for unresolved evidence, ownership, or
  authorization, including code/test disagreement.

## Changes

Record source, decision, target, and evidence in the current task record, or
`docs/governance/YYYY-MM-DD-short-slug.md` for standalone applied governance.
Read [templates/governance-plan.md](templates/governance-plan.md)
**conditionally**, when multiple decisions need a table; case detail may use
`docs/cases/<case-id>/governance-plan.md`. Update the current record; preserve
completed batches. Read-only advice needs no file.

New authority documents declare `status`, `authority`, `layer`, `type`,
`source_of_truth`, `supersedes`, `superseded_by`, `owner`, and `last_verified`.
Use active, draft, superseded, or archived status. Refresh affected indices
and keep adapters as thin pointers. Do not create empty authority sections.

Read [references/governance-rules.md](references/governance-rules.md)
**conditionally**, for new authority locations, entries, bridges, or archives.

## Confirmation

Apply structural, authority, archive, or lifecycle changes covered by owner
authorization. A general setup request authorizes investigation and preparation,
not unspecified binding-rule changes; for unapproved changes, explain targets,
binding-rule effects, and recovery limits before asking. Ask again only when
scope, binding rules, permissions, or irreversible effects exceed prior
approval; routine derived updates within authorized work need no additional
approval.

Preserve the constitution. Creating, amending, or moving it requires an explicit
owner decision covering that action; update its index, SSOT map, and decision
log together. Missing evidence blocks only dependent decisions.

## Verification

Finish when each decision is applied and checked, or blocked with its missing
evidence/owner decision. Check traceability, metadata, links, entry priority,
and historical/derived labels. Report applied and blocked decisions separately;
setup is incomplete while requested changes remain blocked.
