---
name: tdd-execute
description: Execute a currently authorized approved Doc Loom plan with TDD or its confirmed exception, scoped semantic commits, execution evidence, and the required Post-execution review/fix loop. Never execute a recommendation or unapproved plan.
---

# tdd-execute

Read shared status/authorization/commit rules. Read `references/tdd-exceptions.md`
only for `TDD Required: No`; read `../../assessment/review/SKILL.md` only when
the approved plan requires Post-execution review.

## Preflight

Require a non-terminal case, current approved plan/version/log/baseline,
current execute authorization, executable scope/acceptance/TDD strategy,
explainable working tree, eligible review/commit strategies, and the required
full-flow plan commit. Fast-Path instead requires verified conditions and its
combined completion strategy. A just-approved draft may receive minimal
approval writeback first; other failures return to planning.

## Workflow

1. Recheck plan, commands, non-goals, acceptance, and deviations.
2. Write/update `execution.md` as `executing`; resumed cases add later Resume
   evidence required by shared protocol.
3. TDD: observe the smallest credible Red, implement Green, then only necessary
   refactor with related checks. For an approved exception, run its alternative
   characterization/verification.
4. Run planned and obvious low-risk quality checks.
5. In full flow, commit each independently valid green task/refactor. Fast-Path
   keeps its passing delta for the combined completion unit.
6. Record current evidence, hashes, review risk, acceptance status, and any
   future-resume handoff.
7. Invoke Post-execution review when eligible; own every evidence/fix/re-review
   loop.
8. Only aggregate `pass` may set `execution.md` to `ready_to_close` and route to
   closure.

## TDD And Deviations

Claim TDD only after observing a relevant failure and later pass. Tests should
assert observable behavior, cover a plausible regression, use realistic
boundaries, and survive refactors; avoid private/call-order/coverage-only tests
and production test hooks. An unexpected or immediately passing Red must be
explained before continuing.

The approved plan owns exceptions; changing Yes to No is material.

| Deviation | Handling |
|---|---|
| None | Continue and record evidence. |
| Minor | Same goal/responsibility and low risk; record it, plus an amendment only when adaptive execution allows. |
| Material | Goal, acceptance, files, risk, TDD, authority/public contract, dependency/config, or execution constraints changed; stop for plan amendment. |
| Hard stop | Non-goal/decision violation or unapproved authority/script/dependency/lockfile/CI/config change; stop for explicit confirmation/new plan. |

## Evidence And Status

Use `templates/execution.md` when execution evidence is required. Record actual
commands, failures/retries, deviations, hashes, findings, and resume-critical
facts; reference plan expectations instead of copying them. Lead with current
human outcome/action/Git effect.

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

Eligible changes include production/tests, Skill behavior, workflow policy,
public contracts, executable config, or user-visible behavior. Fast-Path uses a
compact Engineering/Spec check; material findings leave the compact path.

Start only after implementation/checks, preliminary acceptance evidence,
expected commits/combined delta, exact baseline, and complete explainable target
exist. Persist separate Engineering and Spec verdicts/findings/gaps, exact
baseline/commits/worktree, aggregate result, and re-review history.

- `pass`: become ready to close.
- `insufficient_evidence`: collect evidence and rerun affected axes.
- `changes_required`: remain executing; fix one root cause, verify it, commit a
  scoped `review-fix:<id>`, then rerun affected axes against the accumulated
  final delta.

Unresolved Critical/Important findings block closure; keep Minor residuals
visible.

## Commits

Follow shared authorization and the plan's semantic boundaries. For each
task/refactor/fix: stage explicit case paths, inspect staged content, run staged
diff/checks, use repository title plus `Doc-Loom-Case`/`Doc-Loom-Step`, and
record the resulting hash. Combine dependent changes when separation would be
invalid; stop on an inseparable unrelated mixed file.

Fast-Path creates no task commit. Separately authorized history rewriting makes
hash/review evidence stale and requires complete revalidation.

## Gates

- Missing current approval/log/baseline/authorization -> planning, not execution.
- Invalid Red or unconfirmed/unverified exception -> stop.
- Material/hard-stop deviation -> amendment/confirmation.
- Missing required plan/task strategy or exact review target -> no closure.
- Fast-Path without green verification and compact pass -> no completion unit.
- Required Post-execution result other than `pass` -> remain executing.
