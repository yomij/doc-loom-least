---
name: review
description: Read-only evidence review of a design doc, proposal, code diff, test set, documentation change, or case evidence. Use when the user explicitly asks for review, or when an approved Doc Loom execution invokes the workflow-owned Post-execution review gate. When the user asks for over-engineering, simplification, deletion, redundancy, YAGNI, or ponytail-style review, run complexity-only mode.
---

# review

Use one entry path:

- explicit user-requested conversation review;
- workflow-owned `Post-execution` review invoked by `tdd-execute` under the
  current approved eligible plan.

Only `Post-execution` needs no separate review request. Plan approval authorizes
that case-scoped gate, never automatic ad-hoc review.

## Modes

- `Standard`: default code, docs, tests, design, proposal, or evidence review.
- `Complexity-only`: only for explicit over-engineering, simplification,
  deletion, redundancy, YAGNI, or ponytail requests.
- `Dual-pass`: explicit request for Standard plus complexity review.
- `Post-execution`: mandatory completed-work gate invoked by `tdd-execute`; run
  isolated Engineering and Spec axes and aggregate without compensation.

Complexity mode is internal mode selection, not routing. It does not assess
correctness, security, performance, coverage, or other Standard concerns. Read
`references/complexity-only.md` for its scope, tags, exemptions, and output; in
`Dual-pass`, use it only for the complexity section.

## Read-Only Gate

Do not create or modify files, code, docs, artifacts, state, authority, or
governance proposals; do not route workflow or close a case. `tdd-execute` may
persist Post-execution output in `execution.md`, and `doc-sync-close` may consume
later user-provided ad-hoc findings, but `review` writes nothing.

## Resolve Target

Use a user-specified object. Otherwise:

1. `Post-execution`: approved plan, its exact `base_commit`, and complete case
   delta.
2. Obvious current conversation object.
3. Non-empty current Git diff.
4. Explicit current case materials.
5. One minimal clarification question.

Never default to the whole repository or all docs.

## Maturity

State one for `Standard`/`Dual-pass`; omit for `Complexity-only` unless relevant:

- `Draft`: requirements, assumptions, boundaries, facts, design coherence.
- `In-progress`: changes, gaps, plan conformance, risk drift.
- `Completed`: acceptance evidence, test credibility, docs impact, residual risk.

`Post-execution` is always `Completed`.

## Post-Execution Preflight

Before either axis:

1. Read project instructions, the current approved plan and amendments, and case
   evidence.
2. Validate the plan's exact baseline with
   `git rev-parse --verify <base_commit>^{commit}`. Never substitute a merge-base
   or three-dot comparison.
3. Build the complete case target from:
   - `git diff <base_commit>..HEAD` and
     `git log <base_commit>..HEAD --oneline`;
   - `git diff --cached` and `git diff`;
   - `git ls-files --others --exclude-standard`, then read every case-related
     untracked file.
4. Explain and exclude unrelated changes. A mixed file that cannot be resolved
   is an evidence gap.
5. Verify planned tasks, tests/alternative checks, preliminary acceptance
   evidence, and expected task commits.
6. Resolve Spec authority in order: approved plan; approved amendments;
   confirmed case decisions; relevant authority/ADR/public contracts; referenced
   upstream issue or PRD.

Return `insufficient_evidence` for an unresolved/invalid baseline, empty eligible
target, missing trustworthy Spec source or material command evidence, or
unexplained case change. In Git degraded mode, report the gap and invent no
baseline or history.

## Evidence And Checks

Read only relevant target files/diffs, cited sources, active authority/ADR/
contracts, related code/tests, and user-provided command/runtime evidence.
Artifact frontmatter carries current status but does not replace behavioral
evidence. Missing evidence is a gap, never a passing claim.

Follow fact authority in `references/shared-protocol.md`. Check claim clarity,
evidence, assumptions/non-goals, verifiable success criteria, source authority,
and that draft/derived/archive/scratch material is not promoted as fact.

Type-specific checks:

- Code/diff: requirement/plan match, scope, edges/errors, authority, contract,
  ADR, and public API safety.
- Tests/TDD: behavior/failure coverage, credible Red/Green evidence, realistic
  mocks, and no private-implementation assertions.
- Documentation governance: correct separation and `source_of_truth`, with no
  unconfirmed authority promotion.

For high-risk Standard/Dual-pass targets, require direct evidence for triggered
High-Risk Topics from `references/shared-protocol.md`. Missing key evidence is
normally `Insufficient evidence` or an Important/Critical finding.

## Post-Execution Axes

Finish and record each evidence pass separately; neither may use the other's
verdict as evidence. Parallel subagents are optional. If one reviewer performs
both, finish one axis before loading the other brief.

### Engineering

Check the smallest relevant set of instructions, standards, authority, ADRs,
contracts, changed/adjacent implementation, tests, config, and command evidence:

- correctness, regressions, edges, errors, and failure handling;
- triggered High-Risk Topics;
- test credibility, failure paths, and claimed tool execution;
- public contracts, repository standards, authority/ADR, and Git health;
- complete-delta/commit evidence and unrelated-change isolation;
- unnecessary complexity, coupling, duplication, speculative behavior, and
  applicable complexity tags/exemptions.

Omit tool-enforced issues only when that tool ran and passed for the target.

### Spec

Compare the complete target with Goal, Non-goals, Decisions, Acceptance
Criteria, approved file/responsibility boundaries and amendments, plus directly
relevant authority, ADR, contract, issue, or PRD requirements. Find missing,
partial, wrong, or unrequested behavior; scope creep; Non-goal violations; and
unsupported acceptance claims. Cite governing Spec evidence for every finding.
A trustworthy Spec source is mandatory for an eligible case.

## Post-Execution Gate

Keep both reports visible, then compute exactly one aggregate result:

| Condition | Result |
|---|---|
| Either axis has unresolved Critical or Important findings | `changes_required` |
| Either axis lacks evidence needed to judge material risk | `insufficient_evidence` |
| Both axes have only Minor findings or none | `pass` |

Deduplicate handling by root cause when useful, but never remove a finding or
let axes compensate. Return only this gate result to `tdd-execute`; fixes,
commits, state, routing, and closure remain outside `review`.

## Output Contract

For `Standard` and the Standard part of `Dual-pass`, lead with the human
decision, then findings and evidence. Use:

```md
## Verdict
No material issue found / Material issues found / Insufficient evidence
One-sentence result.

## Findings
### Critical
### Important
### Minor

## Evidence Gaps
## Scope
## Review Context
- Mode: Standard / Dual-pass
- Maturity: Draft / In-progress / Completed
- Reviewed: `source`; reason; trust/freshness when relevant.
- Not reviewed: `source`; reason; impact.
```

Critical/Important means `Material issues found`; Minor-only/none means `No
material issue found`; missing key evidence that prevents material judgment
means `Insufficient evidence`. Write `None within reviewed scope` when empty.
For many or contested sources, use `Source | Why Included | Trust / Freshness`
as a compact context table.

For each Post-execution axis, persist this compact contract, then the aggregate:

```md
## Engineering
- Verdict: No material issue found / Material issues found / Insufficient evidence
- Findings: Critical / Important / Minor, using the finding format
- Evidence Gaps:
- Scope:

## Spec
- Verdict: No material issue found / Material issues found / Insufficient evidence
- Findings: Critical / Important / Minor, using the finding format
- Evidence Gaps:
- Scope:

## Aggregate
- Result: pass / changes_required / insufficient_evidence
- Reviewed baseline:
- Reviewed commits and working-tree scope:
```

## Finding Format

```md
- `path/file.md:42`: Problem in one sentence.
  Evidence: Specific observation or contradiction.
  Impact: Effect on correctness, governance, risk, or user outcome.
  Required correction: Condition that resolves the issue.
```

Use a stable heading, symbol, test, command, or path when no line exists. The
section supplies severity:

- Critical: security vulnerability, data loss, auth bypass, or public-contract
  break without workaround.
- Important: significant logic/error-handling defect, public-contract test gap,
  or authority fact treated as derived.
- Minor: cosmetic/naming issue, stale link, or redundant code.

`Required correction` states a resolution condition, never a route or owner.

## Subagent Review

Use subagents only when the user explicitly requests independent/subagent
review. The main agent merges/deduplicates findings, assigns severity, and keeps
this contract; subagent output never becomes workflow state.

## Gates

- Ad-hoc modes require explicit user intent; `Post-execution` requires an
  approved eligible plan and `tdd-execute` invocation.
- Do not present missing evidence as a pass.
- Output no workflow route, next owner, ready-to-close/closure verdict, authority
  proposal, or governance follow-up. Post-execution may output only its aggregate
  result for `tdd-execute`.
- Complexity-only never replaces Standard review for non-complexity concerns.
