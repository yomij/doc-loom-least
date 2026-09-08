---
name: setup-doc-governance
description: Initialize, rebuild, or repair documentation governance when requested.
---

# setup-doc-governance

Read `references/governance-rules.md` for layers, authority, routing, and entry
rules; use `references/shared-protocol.md` for authorization and artifact policy.
Govern docs only. Code and tests may serve as read-only evidence.

## Scope And Plan

Default to `docs-only`; use `current-case` for case-local governance and
`full-repo` when code/test evidence is needed or requested.

Each independent batch uses a new `docs/governance/YYYY-MM-DD-<slug>.md`.
Case-bound governance uses `docs/cases/<case-id>/governance-plan.md`. Preserve
existing batches; choose a unique slug for a new one.

Inventory relevant facts, sources, and entries, then write
`templates/governance-plan.md` with `promote`, `merge`, `bridge`, `archive`, or
`block` decisions. Include evidence, target, and material risk; omit empty
sections and unsupported authority areas.

## Apply

The proposed plan does not authorize application. After user confirmation,
record approver/time/version and apply non-blocked decisions, excluding their
blocked dependents. Write results into the same plan and refresh the docs index.
Use `applied` or `applied_with_blocks`; keep unresolved decisions visible.
Material plan changes increment the version and return to `proposed`.

Preserve the declared active constitution. Creation or migration requires a
real need and confirmation; an amendment or migration updates index, SSOT map,
and decision log. Keep agent adapters as pointers to active authority.

Authority frontmatter includes `status`, `authority: true`, `layer: authority`,
`type`, `source_of_truth`, `supersedes`, and `superseded_by`. Lifecycle statuses
are `active`, `draft`, `superseded`, `archived`; high-risk authority also needs
`owner` and `last_verified`.
