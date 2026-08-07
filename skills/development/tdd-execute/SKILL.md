---
name: tdd-execute
description: Execute a currently authorized Doc Loom plan with TDD or its confirmed exception, proportional evidence and commits, and a Post-execution review/fix loop when guarded-review triggers apply. Never execute a recommendation or unauthorized plan.
---

# tdd-execute

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared status,
  authorization, or commit rules.
- [Handoff template](./templates/handoff.md): writing a real future resume
  point only.
- [Execution template](./templates/execution.md): writing triggered execution
  evidence only.

When current policy, Constraints, or execution evidence triggers
Post-execution review, invoke the installed `review` Skill in `Post-execution`
mode.

## Preflight

Require a non-terminal case, current authorized plan/version/confirmation/
baseline, current execute intent when resuming, executable Goal/Success
Criteria/Constraints, and explainable working tree. Derive the implementation,
TDD/verification, review, and commit path from current policy and Constraints.
Also require an exact target when a guarded-review trigger applies, plus any
commit required as a precondition by user/project policy or a Constraint. A
just-authorized draft may receive minimal approval writeback first; other
failures return to planning.

## Workflow

1. Recheck Goal, Success Criteria, Constraints, and working-tree scope; choose
   and adapt the implementation path without writing it back to the plan.
2. Write/update `execution.md` only for resume evidence, material deviation,
   meaningful failure/retry history, deep-review findings, or explicit
   request. Resumed cases add the later Resume evidence required by shared
   protocol.
3. TDD: observe the smallest credible Red, implement Green, then only necessary
   refactor with related checks. For an approved exception, run its alternative
   characterization/verification.
4. Run required and obvious low-risk quality checks.
5. Create commits only when user/project intent or a Constraint requires or
   permits a semantic completion point; do not create bookkeeping-only commits.
6. Record current evidence where triggered; otherwise retain a compact final
   Success-Criteria/test/diff/scope check for closure.
7. Invoke Post-execution review for guarded work, material deviation, weak
   verification, public/authority-sensitive change, or explicit Constraint/
   user request; own every evidence/fix/re-review loop.
8. Required review needs aggregate `pass` before closure. Without that trigger,
   the compact completion check is sufficient. Set existing `execution.md` to
   `ready_to_close` when one exists.

## TDD And Deviations

Claim TDD only after observing a relevant failure and later pass. Tests should
assert observable behavior, cover a plausible regression, use realistic
boundaries, and survive refactors; avoid private/call-order/coverage-only tests
and production test hooks. An unexpected or immediately passing Red must be
explained before continuing.

A TDD exception must be an approved Constraint naming eligibility/category and
the evidence standard that replaces a meaningful Red. Execution chooses and
records the concrete alternative verification; adding an exception is a
Constraint change and returns to planning.

| Deviation | Handling |
|---|---|
| None | Continue and record evidence. |
| Adaptive | Files, tasks, commands, sequencing, run mode, tests, verification, review invocation, commit organization, or another ordinary implementation choice inside the outcome contract; continue and record only when useful. |
| Material | Goal, Success Criterion, Constraint, risk, authority/public contract, dependency/lockfile/CI/schema/config contract, external resource, irreversible action, or protected effect changed; stop for plan amendment. |
| Hard stop | Constraint violation, destructive/unapproved external action, or unexplained mixed work; stop for explicit confirmation/new plan. |

## Evidence And Status

When execution evidence is triggered, record actual commands, meaningful
failures/retries, deviations, hashes, findings, and resume-critical facts;
reference Success Criteria/Constraints instead of copying them. Lead with current human
outcome/action/Git effect. A normal behavior change or TDD cycle alone does not
force the artifact.

Frontmatter is:

```yaml
case_id: <case-id>
status: executing | ready_to_close
updated_at: <timestamp>
```

Keep `review_risk` high/medium/low with reasons; it never auto-triggers ad-hoc
review and cannot be cleared until its cause is evidenced. Failed artifact sync
blocks a claim that workflow state is fully current.

Use low for local well-covered change, medium for bounded internal/broader
change, and high for high-consequence, weakly evidenced, materially deviated,
or pending-authority work.

## Post-Execution Review

Deep-review triggers are guarded work, material deviations, weak verification,
public/authority-sensitive changes, or explicit Constraint/user request. Other work
uses the executor's compact Success-Criteria/test/diff/scope check and does not
persist dual axes.

Start only after implementation/checks, preliminary Success Criteria evidence,
required commits/current delta, exact baseline, and a complete explainable
target exist. Persist separate Engineering and Spec verdicts/findings/gaps,
exact baseline/commits/worktree, aggregate result, and re-review history.

- `pass`: become ready to close.
- `insufficient_evidence`: collect evidence and rerun affected axes.
- `changes_required`: remain executing; fix the complete current finding set in
  the smallest coherent, independently valid/revertible batches, verify each
  batch, commit only when declared/authorized and semantically valuable, then
  rerun affected axes against the accumulated final delta.

Unresolved Critical/Important findings block closure; keep Minor residuals
visible.

## Commits

Follow shared authorization and any commit requirement in user/project policy
or Constraints. For each chosen semantic commit: stage explicit paths, inspect
staged content, run staged diff/checks, use repository title plus
`Doc-Loom-Case`/`Doc-Loom-Step`, and record the resulting hash when execution
evidence exists. Combine dependent changes when separation would be invalid;
finding count never dictates commit count. Stop on an inseparable unrelated
mixed file. Separately authorized history rewriting makes hash/review evidence
stale and requires complete revalidation.

## Gates

- Missing current authorization/confirmation/baseline -> planning, not execution.
- Invalid Red or unconfirmed/unverified exception -> stop.
- Material/hard-stop deviation -> amendment/confirmation.
- Missing evidence for an explicitly required commit, or a required exact
  review target -> no closure.
- Required Post-execution result other than `pass` -> remain executing.
