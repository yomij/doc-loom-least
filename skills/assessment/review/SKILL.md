---
name: review
description: Read-only evidence review for explicit code/docs/design/proposal/test/diff/case requests and triggered Post-execution review. Complexity-only requires explicit simplification, deletion, YAGNI, or over-engineering intent.
---

# review

Enter by explicit review intent or authorized Post-execution invocation for a
shared trigger. Plan approval never authorizes unrelated ad-hoc review.

Read when trigger condition is met:

- [Complexity-only rules](./references/complexity-only.md): `Complexity-only`,
  the complexity section of `Dual-pass`, or the unnecessary-complexity check
  inside Post-execution Engineering.

## Modes And Boundary

- `Standard`: correctness/evidence.
- `Complexity-only`: explicit simplification/YAGNI; assess only complexity and
  never substitute it for Standard.
- `Dual-pass`: explicitly requested Standard plus complexity.
- `Post-execution`: completed-work Engineering and Spec axes with aggregate.

Review writes nothing; execution may persist its returned evidence.

## Target And Evidence

Target user object, obvious conversation object, non-empty diff, explicit case,
then one minimal question—never the whole repo by default. State maturity for
ad-hoc modes; Post-execution is Completed.

Post-execution requires:

1. instructions, authorized contract/amendments, case evidence;
2. exact `base_commit`, never merge-base/three-dot;
3. complete committed, staged, unstaged, and untracked delta;
4. explained unrelated scope and expected checks/commits;
5. trustworthy Spec sources: contract/confirmed decisions, then active
   authority/ADR/public contract/requirements.

Invalid baseline, empty target, unexplained mixed work, or missing material
command/Spec evidence returns `insufficient_evidence`; invent nothing without Git.

Read only relevant delta, cited facts, active contracts, adjacent code/tests,
and command/runtime evidence. Metadata is not proof; missing high-risk evidence
is a gap/finding; derived/history/scratch never becomes fact.

## Post-Execution Axes

Finish each axis independently; neither verdict is evidence for the other.

### Engineering

Check correctness, regression/edges/errors, high-risk topics, credible tests/
commands, public/authority/ADR/repository standards, exact Git isolation, and
unnecessary complexity. Omit tool-enforced issues only after a target pass.

### Spec

Compare complete target with approved contract/amendments and relevant
authority/requirements. Shared adaptive-path choices are evidence unless made a
Constraint. Find missing/partial/wrong/unrequested behavior, scope creep,
Constraint violations, and unsupported success; cite its source.

## Aggregate

| Condition | Result |
|---|---|
| Any unresolved Critical/Important | `changes_required` |
| Material judgment lacks evidence | `insufficient_evidence` |
| Both axes have only Minor/none | `pass` |

Axes never compensate. Return the complete current set and aggregate;
execution owns fixes, commits, readiness, and routing.

## Output

Ad-hoc output leads with result, then Critical/Important/Minor findings, gaps,
scope, mode/maturity, and reviewed/unreviewed sources. Explain non-obvious trust.

Post-execution records each axis, aggregate, exact baseline/commits/delta, and
worktree scope.

Finding format:

```md
- `path:line-or-heading`: Problem.
  Evidence: Observation or contradiction.
  Impact: Consequence.
  Required correction: Resolution condition.
```

Critical covers security/data/auth/public break; Important covers significant
logic/error/contract/authority defects; Minor covers cosmetic, naming, stale
links, or redundancy. Use `None within reviewed scope` when empty.

## Gates

- Ad-hoc modes need explicit intent; Post-execution needs an authorized
  triggered invocation.
- Missing material evidence never passes.
- Output no route, ready-to-close/closure verdict, authority proposal, or fix.
- Use review subagents only when the user explicitly requests them; the main
  reviewer owns final severity and deduplication.
