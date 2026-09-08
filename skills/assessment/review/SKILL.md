---
name: review
description: Read-only review for explicit assessment requests or triggered Post-execution checks. Complexity-only requires explicit simplification intent.
---

# review

Enter through explicit review intent or an authorized Post-execution trigger.
Write no files or state; execution owns fixes and persisted evidence.
Use subagents only when the user requests them.

## Modes

- `Standard`: correctness and evidence.
- `Complexity-only`: unnecessary complexity only; read
  `references/complexity-only.md`.
- `Dual-pass`: explicitly requested Standard plus complexity.
- `Post-execution`: separate Engineering and Spec verdicts with an aggregate.

Review the requested or clear conversation target, not the whole repo by
default. Report scope, findings with location/evidence/impact/correction,
and material evidence gaps. Classify findings Critical, Important, or Minor;
return the complete current set, including an explicit empty result.

## Post-execution Contract

Require the authorized Goal/Success Criteria/Constraints, exact `base_commit`
(not merge-base), and complete committed, staged, unstaged, and untracked delta.
Account for unrelated work and use current authority/requirements as Spec
sources after the approved contract and confirmed decisions.

- **Engineering:** correctness, regression risk, verification, and complexity.
- **Spec:** outcome and Constraint compliance. Adaptive path choices are
  evidence, not requirements unless explicitly constrained.

| Condition | Aggregate |
|---|---|
| Unresolved Critical/Important | `changes_required` |
| Missing material evidence or invalid review target | `insufficient_evidence` |
| Both axes have only Minor/none | `pass` |

Return both verdicts, findings/gaps, aggregate, and exact baseline/delta scope.
Neither axis compensates for the other; execution owns readiness and routing.
