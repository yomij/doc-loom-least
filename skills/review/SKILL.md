---
name: review
description: Review a specified design document, proposal, code diff, test set, documentation change, or Doc Loom case evidence when the user explicitly asks for review. Performs read-only, conversation-only evidence review; reports assessment, findings, and evidence gaps; does not create artifacts, modify files, update status, write proposals, or route workflow.
---

# review

Perform read-only evidence review only when the user explicitly asks for review,
code review, docs review, test review, design review, or similar.

Do not create files, update state, route workflow, close a case, or write
authority/governance proposals.

## Resolve Target

If the user specifies an object, review that object.

If no object is specified:

1. Use the current conversation object when obvious.
2. Else review current git diff when non-empty.
3. Else review current case materials when a case is explicit.
4. Else ask one minimal clarification question.

Do not default to reading the whole repo or all docs.

## Review Maturity

State one maturity:

- `Draft`: review requirements, assumptions, boundaries, facts, and design
  coherence.
- `In-progress`: review current changes, known gaps, plan conformance, and risk
  drift.
- `Completed`: review acceptance evidence, test credibility, documentation
  impact, and residual risk.

## Evidence Selection

Read the smallest relevant evidence:

- Target document, diff, or files.
- Explicitly cited sources.
- Directly related constitution, active authority, ADR, or contract.
- Code/tests relevant to the target claim.
- User-provided command output or runtime evidence.

`case_state.yaml` is not read by default because it is a cache. Read it only
when the review target is state consistency.

If evidence is missing, report an evidence gap rather than converting it into a
workflow block.

## Fact Authority

Use this order for fact conflicts:

1. Active authority docs.
2. Current production code.
3. Current tests.
4. Accepted ADR, migration, or release note.
5. User-provided new information as pending fact.
6. L2 operational docs.
7. L3 derived docs.
8. L4 historical docs.
9. L5 scratch docs.

Historical, derived, and scratch docs are not current facts by default.

## Review Checks

For any target:

- Is the target claim clear?
- Does evidence support the conclusion?
- Are assumptions and non-goals stated?
- Are success criteria verifiable?
- Are fact sources appropriate?
- Are draft, derived, archive, or scratch materials being treated as authority?

For code/diff:

- Does it match the explicit requirement or plan?
- Is scope expanded?
- Are edge cases and errors handled?
- Does it violate authority, contract, ADR, or public API constraints?

For tests/TDD:

- Do tests cover behavior, failure paths, and boundaries?
- Do they avoid testing mocks or implementation details?
- Do mock responses match real structures?
- Does TDD evidence match command results?

For documentation governance:

- Are authority, case evidence, derived, archive, and scratch separated?
- Are unconfirmed facts incorrectly promoted?
- Is `source_of_truth` missing or wrong?

## Output Contract

Use this structure:

```md
## Scope

## Review Maturity

## Evidence Reviewed
| Source | Why Included | Trust / Freshness |
|---|---|---|

## Evidence Not Reviewed
| Source | Reason | Impact |
|---|---|---|

## Assessment
No material issue found / Material issues found / Insufficient evidence

## Findings

### Critical

### Important

### Minor

## Evidence Gaps
```

Assessment rules:

- Critical or Important finding -> `Material issues found`.
- Only Minor or no finding -> `No material issue found`.
- Missing key evidence prevents judging Critical/Important -> `Insufficient evidence`.

If there are no findings, say `None within reviewed scope`.

## Finding Format

Each finding must be locatable and verifiable:

```text
- Severity:
- Location:
- Problem:
- Evidence:
- Impact:
- Required correction:
```

`Required correction` describes the condition for resolving the issue. It must
not assign workflow next owner or route.

## Subagent Review

If the user explicitly requests independent or subagent review, a subagent may
return findings. The main agent must merge, deduplicate, grade severity, and
keep this output contract. Subagent findings do not become workflow state.

## Gates

- User must explicitly request review.
- Do not write files.
- Do not modify code, docs, state, or artifacts.
- Do not output workflow route, next owner, ready-to-close, or closure verdict.
- Do not write authority proposal or governance follow-up.
- Do not package missing evidence as passing.
