---
name: review
description: Read-only evidence review for explicit code, docs, design, proposal, test, diff, or case-review requests, plus the approved workflow-owned Post-execution gate. Use complexity-only mode only for explicit simplification, deletion, YAGNI, or over-engineering requests.
---

# review

Enter only by explicit user review intent or `tdd-execute` invoking an approved
Post-execution gate. Plan approval never authorizes unrelated ad-hoc review.

## Modes And Boundary

- `Standard`: correctness/evidence review.
- `Complexity-only`: explicit simplification/YAGNI requests; read
  `references/complexity-only.md`; do not substitute it for Standard concerns.
- `Dual-pass`: explicitly requested Standard plus complexity.
- `Post-execution`: completed-work Engineering and Spec axes with aggregate.

Review is read-only: create/modify no files, case artifacts, authority, state,
routes, fixes, or closure. The execution owner may persist returned evidence.

## Target And Evidence

Use the user object, obvious conversation object, non-empty current diff,
explicit case, then one minimal question. Never default to the whole repository.
For Standard/Dual-pass state Draft, In-progress, or Completed; Post-execution is
Completed.

Post-execution requires:

1. project instructions, approved plan/amendments, and case evidence;
2. verified exact `base_commit` (never substitute merge-base/three-dot);
3. complete target from `git diff <base>..HEAD`, `git log <base>..HEAD`,
   `git diff --cached`, `git diff`, and relevant paths from
   `git ls-files --others --exclude-standard`;
4. explained unrelated changes and expected checks/commits;
5. trustworthy Spec authority: plan/amendments/confirmed decisions, then active
   authority/ADR/public contract/referenced requirement.

Invalid/missing baseline, empty eligible target, unexplained mixed change, or
missing material command/Spec evidence returns `insufficient_evidence`. In Git
degraded mode, state the gap and invent nothing.

Read only relevant target/diff, cited facts, active authority/contracts,
adjacent implementation/tests, and command/runtime evidence. Status metadata is
not behavioral proof. Treat missing key high-risk evidence as a gap or material
finding; never promote derived/history/scratch to fact.

## Post-Execution Axes

Finish each axis independently; neither verdict is evidence for the other.

### Engineering

Check correctness/regression/edges/errors, high-risk topics, credible tests and
commands, public/authority/ADR/repository standards, exact delta/Git isolation,
and unnecessary complexity. Omit tool-enforced issues only when the tool passed
for the target.

### Spec

Compare the complete target with Goal, Non-goals, Decisions, Acceptance,
approved file/responsibility boundaries/amendments, and directly relevant
authority/contracts/requirements. Find missing/partial/wrong/unrequested
behavior, scope creep, and unsupported acceptance; cite the governing source.

## Aggregate

| Condition | Result |
|---|---|
| Any unresolved Critical/Important | `changes_required` |
| Material judgment lacks evidence | `insufficient_evidence` |
| Both axes have only Minor/none | `pass` |

Do not let axes compensate. Return the aggregate only; execution owns fixes,
commits, readiness, and routing.

## Output

Standard/Dual-pass: lead with `No material issue found`, `Material issues
found`, or `Insufficient evidence`, then Critical/Important/Minor findings,
evidence gaps, scope, mode/maturity, reviewed/not-reviewed sources.
Include reason and trust/freshness when source authority is not obvious.

Post-execution records each axis verdict/findings/gaps/scope, then aggregate,
exact baseline, commits, and working-tree scope.

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

- Ad-hoc modes need explicit intent; Post-execution needs approved invocation.
- Missing material evidence never passes.
- Output no route, ready-to-close/closure verdict, authority proposal, or fix.
- Use review subagents only when the user explicitly requests them; the main
  reviewer owns final severity and deduplication.
