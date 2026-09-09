---
name: setup-doc-governance
description: Establish, rebuild, or repair documentation governance on request, with an evidence-based plan and confirmed changes.
---

# Set Up Documentation Governance

Read references/governance-rules.md for layers, authority, routing, and entry
rules. Change documentation only; use code and tests as read-only evidence.

## Scope and record

Default to docs-only. Use current-case for case-local governance or full-repo
when code and test evidence is needed or requested.

For an independent batch, create docs/governance/YYYY-MM-DD-short-slug.md. For
case-bound work, keep the decision in the current task record or use
docs/cases/<case-id>/governance-plan.md when detailed evidence is necessary.
Do not rewrite existing batches.

Inventory relevant facts, sources, and entry points. Use the governance-plan
template only when its detail is needed. Record promote, merge, bridge, archive,
or block decisions with evidence, target, and material risk. Omit empty sections.

## Apply decisions

Obtain user confirmation before applying structural, authority, or otherwise
material changes. Routine derived updates within an already authorized task can
be applied by the agent. When a governance plan is used, applying its
structural or authority decisions still requires owner confirmation. Record the
decision and result in the same record, refresh the
documentation index, and keep unresolved decisions visible. Material changes
need a new version and renewed confirmation.

Preserve the active constitution. Create or migrate it only for a demonstrated
need with confirmation. Amendments and migrations update the index, SSOT map,
and decision log. Keep agent adapters as thin pointers to active authority.

Include authority frontmatter when creating an authority document: status,
authority, layer, type, source_of_truth, supersedes, and superseded_by. Use
active, draft, superseded, or archived; high-risk authority also names owner
and last_verified.
