---
name: setup-doc-governance
description: Establish, rebuild, or repair documentation governance on request, with an evidence-based plan and confirmed changes.
---

# Set Up Documentation Governance

Read `references/governance-rules.md` for layers, authority, routing, and entry
rules, and `references/shared-protocol.md` for authorization and artifacts.
Change documentation only; use code and tests as read-only evidence.

## Scope and Plan

Default to `docs-only`. Use `current-case` for case-local governance or
`full-repo` when code and test evidence is needed or requested.

For an independent batch, create `docs/governance/YYYY-MM-DD-<slug>.md` with a
unique slug. For case-bound work, use `docs/cases/<case-id>/governance-plan.md`.
Preserve existing batches.

Inventory relevant facts, sources, and entry points. Use
`templates/governance-plan.md` to record `promote`, `merge`, `bridge`, `archive`,
or `block` decisions with evidence, target, and material risk. Omit empty
sections and authority areas without supporting evidence.

## Apply Confirmed Decisions

Obtain user confirmation before applying the plan; record approver, time, and
version. Apply non-blocked decisions and exclude dependents of blocked decisions.
Record results in the same plan, refresh the documentation index, and set
`applied` or `applied_with_blocks`. Keep unresolved decisions visible. Material
plan changes require a new version and return to `proposed`.

Preserve the active constitution. Create or migrate it only for a demonstrated
need with confirmation. Amendments and migrations must update the index, SSOT
map, and decision log. Keep agent adapters as pointers to active authority.

Include authority frontmatter: `status`, `authority: true`, `layer: authority`,
`type`, `source_of_truth`, `supersedes`, and `superseded_by`. Use lifecycle status
`active`, `draft`, `superseded`, or `archived`; high-risk authority also requires
`owner` and `last_verified`.
