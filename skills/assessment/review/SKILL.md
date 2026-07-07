---
name: review
description: Read-only evidence review of a design doc, proposal, code diff, test set, documentation change, or case evidence. Use only when the user explicitly asks for review. When the user asks for over-engineering, simplification, deletion, redundancy, YAGNI, or ponytail-style review, run complexity-only mode.
---

# review

Perform read-only evidence review only when the user explicitly asks for review,
code review, docs review, test review, design review, or similar.

When the user explicitly asks for over-engineering, simplification, deletion,
redundancy, YAGNI, or ponytail-style review, run `Complexity-only` mode inside
this skill. This is mode selection, not workflow routing.

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

## Review Mode

Use one mode:

- `Standard`: default for ordinary review, code review, docs review, test
  review, design review, or "check this" requests.
- `Complexity-only`: use only when the user explicitly asks for
  over-engineering, simplification, deletion, redundancy, YAGNI, or
  ponytail-style review.
- `Dual-pass`: use only when the user asks for both normal review and
  complexity review.

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

For `Complexity-only`, read `references/complexity-only.md` and use only that
scope, tags, exemptions, and output contract. For `Dual-pass`, use it only for
the final complexity section.

For `Standard` and `Dual-pass` high-risk targets, check direct evidence for
relevant High-Risk Topics from `references/shared-protocol.md`. Missing key
evidence in these areas should usually be `Insufficient evidence` or an
Important/Critical finding, not a passing assessment.

## Output Contract

For `Standard` and the Standard section of `Dual-pass`, lead with the decision
a human needs, then findings, then evidence details. Keep evidence explicit, but
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

- User must explicitly request review.
- Read-only; do not write files or modify code, docs, state, or artifacts.
- Do not output workflow route, next owner, ready-to-close, closure verdict,
  authority proposal, or governance follow-up.
- Do not package missing evidence as passing.
- `Complexity-only` does not replace Standard review for correctness, security,
  performance, test credibility, or High-Risk Topics.
