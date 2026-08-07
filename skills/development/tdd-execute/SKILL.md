---
name: tdd-execute
description: Execute an authorized Doc Loom plan with TDD/confirmed exception, proportional evidence and commits, and triggered Post-execution review/fixes. Never execute a recommendation or unauthorized plan.
---

# tdd-execute

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared status,
  authorization, or commit rules.
- [Handoff template](./templates/handoff.md): writing a real future resume
  point only.
- [Execution template](./templates/execution.md): writing triggered execution
  evidence only.

When policy, Constraints, or evidence triggers review, invoke `review` in
`Post-execution` mode.

## Preflight

Require a non-terminal case, authorized contract/version/confirmation/baseline,
resume intent when applicable, explainable worktree, any required precondition
commit, and an exact target for triggered review. Derive the adaptive path from
policy/Constraints. A just-authorized draft may receive minimal approval
writeback; other failures return to planning.

## Workflow

1. Recheck contract/worktree; choose the adaptive path without changing plan.
2. Write `execution.md` only for resume, material deviation, meaningful
   failure/retry, deep-review evidence, or explicit request. Resume records the
   shared required evidence.
3. Observe credible Red, implement Green, then necessary refactor/checks; an
   approved exception uses its alternative evidence.
4. Run required and obvious low-risk quality checks.
5. Commit only authorized semantic points, never bookkeeping.
6. Record triggered evidence; otherwise retain a compact criteria/test/diff/
   scope check for closure.
7. Invoke Post-execution review for Guarded work, material deviation, weak
   evidence, public/authority effect, or explicit Constraint/user request; own
   evidence/fix/re-review.
8. Required review needs aggregate `pass`; otherwise the compact check suffices.
   Set existing execution evidence `ready_to_close` when ready.

## TDD And Deviations

Claim TDD only after relevant failure then pass. Tests assert observable
behavior and plausible regression through realistic, refactor-stable seams;
avoid private/call-order/coverage-only checks and production hooks. Explain an
unexpected or immediately passing Red.

A TDD exception is an approved Constraint naming category/reason/evidence
standard. Execution records its concrete verification; adding one changes plan.

| Deviation | Handling |
|---|---|
| None | Continue and record evidence. |
| Adaptive | Shared adaptive-path choice; continue, record only when useful. |
| Material | Shared protected change; stop for plan amendment. |
| Hard stop | Constraint violation, destructive/unapproved external action, or unexplained mixed work; stop for explicit confirmation/new plan. |

## Evidence And Status

Triggered evidence records actual commands, meaningful failures/retries,
deviations, hashes, findings, and resume facts; reference rather than copy the
contract. Lead with outcome/action/Git. Normal TDD alone creates no artifact.

Frontmatter is:

```yaml
case_id: <case-id>
status: executing | ready_to_close
updated_at: <timestamp>
```

Record reasoned `review_risk`: low local/well-covered; medium bounded internal/
broader; high high-consequence, weak evidence, material deviation, or pending
authority. It never authorizes ad-hoc review and clears only with evidence.
Failed artifact sync blocks fully-current status.

## Post-Execution Review

Deep-review triggers: Guarded work, material deviation, weak evidence,
public/authority effect, or explicit Constraint/user request. Other work uses
the compact check without persisted axes.

Start after implementation/checks, preliminary criteria evidence, required
commits/current delta, exact baseline, and complete target. Persist separate
Engineering/Spec verdicts/findings/gaps, target, aggregate, and re-review.

- `pass`: become ready to close.
- `insufficient_evidence`: collect evidence and rerun affected axes.
- `changes_required`: remain executing; fix the complete current finding set in
  the smallest coherent, independently valid/revertible batches, verify each
  batch, commit only when declared/authorized and semantically valuable, then
  rerun affected axes against the accumulated final delta.

Unresolved Critical/Important findings block closure; keep Minor residuals
visible.

## Commits

For each authorized semantic commit: stage explicit paths; inspect/check staged
content; use repository title and Case/Step trailers; record triggered hash
evidence. Combine inseparable changes; finding count never sets commit count.
Stop on unrelated mixed files. Authorized history rewriting invalidates hashes/
review and requires full revalidation.

## Gates

- Missing current authorization/confirmation/baseline -> planning, not execution.
- Invalid Red or unconfirmed/unverified exception -> stop.
- Material/hard-stop deviation -> amendment/confirmation.
- Missing evidence for an explicitly required commit, or a required exact
  review target -> no closure.
- Required Post-execution result other than `pass` -> remain executing.
