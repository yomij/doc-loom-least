---
name: setup-doc-governance
description: Establish or adjust repository documentation authority, hierarchy, knowledge promotion, and current/historical status when requested or needed to resolve a structural governance conflict. Excludes ordinary README, API documentation, and task-note edits.
---

# Setup Doc Governance

## Scope

Infer the narrowest target from the request:

- `target_scope: current-case` governs only the identified case.
- `target_scope: repository` governs the requested repository documentation.

Evidence scope is independent:

- `evidence_scope: docs-only` reads documentation; this is the default.
- `evidence_scope: repo-readonly` also reads code, tests, and configuration when
  needed to establish facts and permitted by the user's constraints.

Evidence access grants no modification rights. Change documentation only within
the authorized target. For example, current-case plus repo-readonly permits
repository evidence without permission to restructure repository authority.
Implementation fixes belong to docloom-workflow.

## Evidence

Read the declared constitution and relevant authority entries when present.
Inventory affected facts, sources, and entries. Facts need identified sources;
inference alone cannot be promoted. Inspect available evidence before asking.
Expanding explicitly restricted evidence scope requires authorization.

## Authority Decisions

Follow declared precedence and fact ownership. Without them, preserve the
constitution, use explicit owner decisions and accepted unsuperseded decisions
for intended rules, and code/tests for implemented behavior. Disagreement does
not authorize rewriting either.

Only sourced rules designated current by the repository or owner constrain
future work. `active` alone does not establish authority.
Draft/scratch is unverified; case records are task evidence; derived documents
summarize or route; superseded/archived material is historical. Missing metadata
requires checking ownership, not automatically discarding existing rules.
Resolve conflicts using declared precedence or existing owner decisions. If
neither settles a change to binding rules, mark it blocked for owner decision.

Use `promote` for a new authority fact, `merge` for an existing authority target,
`bridge` for a thin old-entry pointer, `archive` for preserved history, and
`block` for unresolved evidence, ownership, or authorization, including code/test
disagreement.

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
authorization. Otherwise prepare changes and explain targets, binding-rule
effects, and recovery limits before asking. A general setup request permits
preparation, not unspecified authority decisions. Ask again only for scope,
binding rules, permissions, or irreversible effects outside prior approval.
Routine derived updates within authorization need no additional approval.

Preserve the constitution. Creating, amending, or moving it requires an explicit
owner decision covering that action; update its index, SSOT map, and decision
log together. Missing evidence blocks only dependent decisions.

## Verification

Finish when each decision is applied and checked, or blocked with its missing
evidence/owner decision. Check traceability, metadata, links, entry priority, and
historical/derived labels. Report applied and blocked decisions separately;
setup is incomplete while requested changes remain blocked.
