---
name: review
description: Read-only evidence review of a design doc, proposal, code diff, test set, documentation change, or case evidence. Use only when the user explicitly asks for review.
---

# review

Perform read-only evidence review only when the user explicitly asks for review,
code review, docs review, test review, design review, or similar.

Do not create files, update state, route workflow, close a case, or write
authority/governance proposals.

If the user later provides review findings to `doc-sync-close`, closure may
consume them as user-provided findings. `review` itself still writes no state.

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

`case_state.yaml` is a routing signal, not evidence. Read it only when the
review target is state consistency.

If evidence is missing, report an evidence gap rather than converting it into a
workflow block.

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

For high-risk targets, check direct evidence for relevant High-Risk Topics from
`references/shared-protocol.md`. Missing key evidence in these areas should
usually be `Insufficient evidence` or an Important/Critical finding, not a
passing assessment.

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

- User must explicitly request review.
- Read-only; do not write files or modify code, docs, state, or artifacts.
- Do not output workflow route, next owner, ready-to-close, closure verdict,
  authority proposal, or governance follow-up.
- Do not package missing evidence as passing.
