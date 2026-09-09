---
name: review
description: Review a specified target on explicit request or when an authorized task needs an independent check. Return findings without modifying files or task state.
---

# Review

Review the requested target or the target established in conversation. Use
subagents only when requested by the user. This Skill is read-only; the
executor owns fixes and records evidence that must survive.

Assess correctness, evidence, scope, and removable complexity. Read
references/complexity-only.md only when the user explicitly asks for that
focused assessment.

For a high-consequence or post-execution review, require the stated goal,
success conditions, constraints, the exact baseline when available, and the
complete relevant delta. Check implementation behavior and compliance with the
stated outcome in one finding set; do not require two reports. Account for
unrelated work.

For each finding give location, evidence, impact, correction, and severity:
Critical, Important, or Minor. Return all current findings or say that none were
found. Return changes_required for unresolved Critical or Important findings,
insufficient_evidence for a material gap or invalid target, and pass when no
material finding remains. Include the baseline, delta, and evidence gaps.
