---
name: review
description: Read-only evidence review of a design doc, proposal, code diff, test set, documentation change, or case evidence. Use when the user explicitly asks for review, or when an approved Doc Loom execution invokes the workflow-owned Post-execution review gate. When the user asks for over-engineering, simplification, deletion, redundancy, YAGNI, or ponytail-style review, run complexity-only mode.
---

# review

Perform read-only evidence review through one of two entry paths:

- explicit conversation review requested by the user;
- workflow-owned `Post-execution` review invoked by `tdd-execute` under the
  current approved plan.

Only `Post-execution` may run without a separate review request. Current-plan
approval authorizes that read-only gate for the approved case; it does not make
ad-hoc review automatic.

When the user explicitly asks for over-engineering, simplification, deletion,
redundancy, YAGNI, or ponytail-style review, run `Complexity-only` mode inside
this skill. This is mode selection, not workflow routing.

Do not create files, update state, route workflow, close a case, or write
authority/governance proposals.

`tdd-execute` may persist the Post-execution verdict and findings in
`execution.md`. If the user later provides ad-hoc findings to `doc-sync-close`,
closure may consume them as user-provided findings. `review` itself still
writes no state.

## Resolve Target

If the user specifies an object, review that object.

If no object is specified:

1. In `Post-execution`, use the approved case plan, its exact `base_commit`, and
   the complete case delta defined below.
2. Otherwise use the current conversation object when obvious.
3. Else review current git diff when non-empty.
4. Else review current case materials when a case is explicit.
5. Else ask one minimal clarification question.

Do not default to reading the whole repo or all docs.

## Review Mode

Use one mode:

- `Standard`: default for ordinary review, code review, docs review, test
  review, design review, or "check this" requests.
- `Complexity-only`: use only when the user explicitly asks for
  over-engineering, simplification, deletion, redundancy, YAGNI, or
  ponytail-style review.
- `Dual-pass`: use only when the user asks for both normal review and
  complexity review.
- `Post-execution`: use only when `tdd-execute` invokes the mandatory gate for
  an eligible case under an approved plan. Run separate Engineering and Spec
  axes, then aggregate them without compensation.

`Complexity-only` is not a correctness, security, performance, or coverage
review. If the user only asks for complexity, do not report those issues as
findings; mention that a Standard review is needed if they want that scope.

## Review Maturity

For `Standard` and `Dual-pass`, state one maturity. For `Complexity-only`, omit
maturity unless it affects whether something is over-designed.

- `Draft`: review requirements, assumptions, boundaries, facts, and design
  coherence.
- `In-progress`: review current changes, known gaps, plan conformance, and risk
  drift.
- `Completed`: review acceptance evidence, test credibility, documentation
  impact, and residual risk.

`Post-execution` always uses `Completed` maturity.

## Post-Execution Preflight

Before either axis runs:

1. Read the current approved `plan.md`, approved Plan Amendments, case evidence,
   and project instructions.
2. Read `base_commit` from the approved plan. Validate that exact commit with
   `git rev-parse --verify <base_commit>^{commit}`. Do not silently replace it
   with a merge-base or three-dot comparison.
3. Build the complete target from all case-related changes:
   - committed delta: `git diff <base_commit>..HEAD` and
     `git log <base_commit>..HEAD --oneline`;
   - staged tracked changes: `git diff --cached`;
   - unstaged tracked changes: `git diff`;
   - untracked files: `git ls-files --others --exclude-standard`, followed by
     reading each case-related file.
4. Explain unrelated workspace changes and exclude them. An unresolved mixed
   file or unexplained change is an evidence gap, not a pass.
5. Verify planned tasks, required tests or alternative checks, preliminary
   acceptance evidence, and expected task commits are complete.
6. Resolve the Spec source in this order: current approved plan; approved Plan
   Amendments; confirmed current-task decisions in case evidence; relevant
   authority, ADRs, and public contracts; upstream issue or PRD referenced by
   the plan.

An unresolved baseline, empty eligible target, missing trustworthy Spec source,
unexplained case change, or missing material command evidence yields
`insufficient_evidence`. In Git degraded mode, report the evidence gap; do not
invent a baseline or commit history.

## Evidence Selection

Read the smallest relevant evidence:

- Target document, diff, or files.
- Explicitly cited sources.
- Directly related constitution, active authority, ADR, or contract.
- Code/tests relevant to the target claim.
- User-provided command output or runtime evidence.

`case_state.yaml` is a routing signal, not evidence. Read it only when the
review target is state consistency.

If evidence is missing, report an evidence gap rather than converting it into a
passing claim. In `Post-execution`, material gaps participate in the aggregate
gate result.

## Fact Authority

Follow the fact authority order in `references/shared-protocol.md`.

## Review Checks

Core checks: claim clarity, evidence support, stated assumptions/non-goals,
verifiable success criteria, appropriate fact sources, and no draft/derived/
archive/scratch material treated as authority.

Type-specific checks:
- Code/diff: requirement or plan match, scope control, edge/error handling, and
  no authority, contract, ADR, or public API violation.
- Tests/TDD: behavior and failure-path coverage, no mock/implementation-detail
  testing, realistic mock structures, and command evidence matching TDD claims.
- Documentation governance: authority, case evidence, derived, archive, and
  scratch separated; no unconfirmed promotion; `source_of_truth` correct.

For `Complexity-only`, read `references/complexity-only.md` and use only that
scope, tags, exemptions, and output contract. For `Dual-pass`, use it only for
the final complexity section.

For `Standard` and `Dual-pass` high-risk targets, check direct evidence for
relevant High-Risk Topics from `references/shared-protocol.md`. Missing key
evidence in these areas should usually be `Insufficient evidence` or an
Important/Critical finding, not a passing assessment.

## Post-Execution Axes

Run the axes as separate evidence passes. Parallel sub-agents are optional, not
a dependency. When one reviewer performs both passes, finish and record one
axis before loading the other axis brief. Neither axis may use the other axis's
verdict as evidence.

### Engineering Axis

Review technical and workflow soundness against the smallest relevant set of
project instructions, active standards, authority, ADRs, contracts, changed
and adjacent implementation, tests, configuration, and current command
evidence. Check:

- correctness, plausible regressions, edge cases, errors, and failure handling;
- triggered security, privacy, permission, and other High-Risk Topics;
- test credibility, relevant failure paths, and whether reported tools ran;
- public contracts, authority, ADRs, repository standards, and Git health;
- complete-delta and commit evidence, including unrelated-change isolation;
- unnecessary complexity, coupling, duplication, speculative behavior, and
  the applicable tags/exemptions in `references/complexity-only.md`.

Tool-enforced issues may be omitted from prose only when the relevant tool
actually ran and passed for the reviewed target.

### Spec Axis

Compare the complete target with the approved task object. Check:

- Goal, Non-goals, confirmed Decisions, and Acceptance Criteria;
- approved file/responsibility boundaries and Plan Amendments;
- missing, partial, or incorrectly implemented behavior;
- scope creep, Non-goal violations, and unsupported acceptance claims;
- directly relevant authority, ADR, contract, issue, or PRD requirements.

Quote or point to the governing plan/spec evidence for each finding. A
trustworthy Spec source is mandatory for an eligible persistent case.

## Post-Execution Gate

Keep Engineering and Spec reports visible separately, then compute exactly one
aggregate result:

| Condition | Result |
|---|---|
| Either axis has unresolved Critical or Important findings | `changes_required` |
| Either axis lacks evidence needed to judge material risk | `insufficient_evidence` |
| Both axes have only Minor findings or no findings | `pass` |

Deduplicate handling by root cause when useful, but never remove a finding from
an axis or let one axis compensate for the other. Return the result to
`tdd-execute`; `review` remains read-only and does not perform fixes, commits,
state updates, routing, or closure.

## Output Contract

For `Standard` and the Standard section of `Dual-pass`, lead with the decision a
human needs, then findings, then evidence details. Keep evidence explicit, but
do not front-load audit tables unless the source set is large or contested.

Use this structure:

```md
## Verdict
No material issue found / Material issues found / Insufficient evidence

One-sentence summary of the review result.

## Findings

### Critical

### Important

### Minor

## Evidence Gaps

## Scope

## Review Context
- Mode: Standard / Dual-pass
- Maturity: Draft / In-progress / Completed
- Reviewed: `source`; why included; trust/freshness when relevant.
- Not reviewed: `source`; reason; impact.
```

If there are many sources, contested sources, or trust/freshness matters, use a
compact table inside `Review Context`:

```md
| Source | Why Included | Trust / Freshness |
|---|---|---|
```

Verdict rules:

- Critical or Important finding -> `Material issues found`.
- Only Minor or no finding -> `No material issue found`.
- Missing key evidence prevents judging Critical/Important -> `Insufficient evidence`.

If there are no findings, say `None within reviewed scope` under `Findings`.

For `Post-execution`, use this compact durable contract for each axis, followed
by the aggregate gate:

```md
## Engineering
- Verdict: No material issue found / Material issues found / Insufficient evidence
- Findings: Critical / Important / Minor, using the standard finding format
- Evidence Gaps:
- Scope:

## Spec
- Verdict: No material issue found / Material issues found / Insufficient evidence
- Findings: Critical / Important / Minor, using the standard finding format
- Evidence Gaps:
- Scope:

## Aggregate
- Result: pass / changes_required / insufficient_evidence
- Reviewed baseline:
- Reviewed commits and working-tree scope:
```

## Finding Format

Each finding must be locatable and verifiable:

```md
- `path/file.md:42`: Problem in one sentence.
  Evidence: Specific observed evidence or contradiction.
  Impact: Why this matters to correctness, governance, risk, or user outcome.
  Required correction: Condition that resolves the issue.
```

The severity is supplied by the section heading. Do not repeat `Severity:` per
finding. If a line number is not available, use the nearest stable heading,
symbol, test name, command, or artifact path.

Severity anchoring:
- Critical: security vulnerability, data loss path, auth bypass, public
  contract violation with no workaround. Example: API endpoint allows
  unauthenticated data deletion.
- Important: significant logic error, missing error handling, test gap for
  public contract, authority fact treated as derived. Example: a public API
  endpoint lacks error handling for malformed input.
- Minor: cosmetic issue, naming inconsistency, stale link, redundant code.
  Example: a doc references a moved file path.

`Required correction` describes the condition for resolving the issue. It must
not assign workflow next owner or route.

## Subagent Review

If the user explicitly requests independent or subagent review, a subagent may
return findings. The main agent must merge, deduplicate, grade severity, and
keep this output contract. Subagent findings do not become workflow state.

## Gates

- Ad-hoc `Standard`, `Complexity-only`, and `Dual-pass` review require explicit
  user intent. `Post-execution` requires an approved eligible plan and
  invocation by `tdd-execute`.
- Read-only; do not write files or modify code, docs, state, or artifacts.
- Do not output a workflow route, next owner, ready-to-close claim, closure
  verdict, authority proposal, or governance follow-up. `Post-execution` may
  output only its aggregate gate result for `tdd-execute` to consume.
- Do not package missing evidence as passing.
- `Complexity-only` does not replace Standard review for correctness, security,
  performance, test credibility, or High-Risk Topics.
