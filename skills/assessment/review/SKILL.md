---
name: review
description: Review a specified target on explicit request or after an authorized execution trigger. Return findings without modifying files or workflow state.
---

# Review

Review the requested target or the target established in conversation. Use
subagents only when requested by the user. Execution owns fixes and records
review evidence.

## Select a Mode

- **Standard:** assess correctness and supporting evidence.
- **Complexity-only:** assess removable complexity when simplification is
  explicitly requested; read `references/complexity-only.md`.
- **Dual-pass:** combine Standard and Complexity-only when explicitly requested.
- **Post-execution:** assess Engineering and Spec separately, then combine the
  verdicts.

## Report Findings

State the scope and material evidence gaps. For each finding, give its location,
evidence, impact, correction, and severity: Critical, Important, or Minor.
Return all current findings, or state that none were found.

## Post-execution Review

Require the authorized Goal, Success Criteria, and Constraints; the exact
`base_commit` rather than merge-base; and the complete committed, staged,
unstaged, and untracked delta. Account for unrelated work. For Spec, use the
approved contract and confirmed decisions first, then current authority and
requirements.

- **Engineering:** correctness, regression risk, verification, and complexity.
- **Spec:** outcome and Constraint compliance. Execution choices are evidence,
  not requirements unless explicitly constrained.

| Condition | Aggregate verdict |
|---|---|
| Unresolved Critical or Important findings | `changes_required` |
| Missing material evidence or invalid review target | `insufficient_evidence` |
| Both axes have only Minor findings or none | `pass` |

Return both verdicts, the aggregate, findings and gaps, and the exact baseline
and delta reviewed. Neither axis can compensate for the other.
