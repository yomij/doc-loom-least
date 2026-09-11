# Independent business documentation

## Goal

Deliver an independently callable business-docs Skill for business-facing
conversation synthesis, task archival, historical logic reconstruction, and
updates. Integrate it with development without requiring that entry for
standalone use.

## Success conditions

- Standalone use requires neither docloom-workflow nor task.md.
- Business rules and decisions retain sources, meaningful corrections, and
  uncertainty; implementation evidence does not invent intent or rationale.
- Historical investigation separates time/version, intended and observed
  behavior, and confirmation, delivery, and verification.
- Matching drafts update in place; later changes preserve archived history and
  link affected rules. Business navigation grants no authority.
- Development captures business changes and finalizes archival; technical-only
  tasks need no empty document.
- Scope, discovery, distribution, authority, and navigation agree on the new
  capability. Structural validation and a separate scoped review complete.

## Request and authorization

The following are excerpts and summaries of the visible user conversation on
2026-09-10. No stable external conversation URL is available; these are not an
independently verified transcript.

- U1, initial request: retain key business logic and decisions after each task
  for product, QA, and development to read without code. Content is purely
  business-facing and comes from initial requirements and later corrections
  and discussion.
- U2, explicit correction: “我希望能独立调用，有时候没有使用docloom流程的回话，
  我希望也能输出业务文档”. This requires standalone conversation use.
- U3, additional scope: “整理历史逻辑的时候，我希望也能用上”. This requires
  historical reconstruction in addition to new-task archival.
- U4, execution authorization: “执行吧”, following the concrete proposal for
  business-docs, its three input scenarios, existing-document updates, sourced
  business prose, independent storage, and existing authority governance.

## Constraints and decisions

Repository-only implementation; existing installed/global Skill copies are not
part of this change. Use pnpm for frontend dependencies and uv for Python tools.
No runtime backend, mandatory new task phase, bulk historical migration, or
constitutional amendment. Existing untracked .DS_Store is unrelated.

Applied repository governance (`target_scope: repository`, `evidence_scope:
docs-only`) under U4:

- Promote the approved business-document contract into
  docs/authority/workflow/business-docs.md; source is U1–U4 and ADR-0005.
- Merge the approved narrow product capability and fifth Skill into scope,
  architecture, development, governance, and distribution authority. Update
  adapters and derived entries accordingly.
- Preserve ADR-0004's original decision and add a pointer to ADR-0005's limited
  extension. Preserve all prior case artifacts.
- Establish business navigation as derived and business archival as sourced
  snapshots, with no automatic authority promotion. The current task's business
  document exercises the approved placement contract.

## Current state

done — Independent Skill, local template, development integration, authority
updates, and the first business archive are complete. Structural checks and
the scoped read-only review passed; evidence limits are recorded below.

## Next action

None required for repository delivery. Future real standalone sessions and
historical investigations can provide behavioral evidence beyond this change's
structural checks and manual walkthroughs.

## Verification evidence

Baseline: `7a27c6d38416884fd8a96dfcbdd57e0306ad0a77`. Delta: new Skill and template,
development integration, ADR-0005, affected authority/navigation, and this case.

- Bundled skill-creator quick_validate, run through uv with PyYAML: all five
  canonical Skills pass frontmatter/name/scaffold validation.
- Changed Markdown link and anchor check: 27 links across 29 changed/new files
  resolved at the implementation check. The new authority has all required
  metadata. Closure links are checked again after finalization.
- An isolated temporary copy of only business-docs passes validation and local
  resource resolution without sibling Skills, README, or task.md.
- `git diff --check` passes. The constitution has no diff. Active entry points
  consistently list five Skills; four-Skill references remaining in ADR-0004
  and older cases describe their historical scope.
- Actual use on this conversation produced [business.md](business.md), with
  the initial requirement, both user corrections, decision sources, and
  separate delivery/verification limits in business-facing prose.

### Manual scenario walkthroughs

These are local readings of the instructions against representative inputs,
not independent model executions or reliability measurements.

| Input scenario | Contract outcome checked |
|---|---|
| Ordinary conversation, no task record | Direct business-docs entry; default standalone output; no prerequisite development flow or manufactured task.md. |
| Initial 30-minute cancellation limit corrected to pre-shipment; agent also suggests coupon restoration without approval | Final rule follows explicit correction; meaningful change retained; the agent suggestion cannot become a confirmed rule. |
| Old document allows cancellation, current implementation rejects it, no rationale or test run is available | Intended versus observed rules and version scope remain separate; conflict and missing reason retained; test source is not verification or deployment evidence. |
| Later work changes only B1 in an archived document containing B1 and B2 | New document and reciprocal follow-up link preserve the old body and B2; no whole-topic supersession. A dated/specific filename prevents reuse of an occupied archive destination. |
| Repeated continuation of the same draft | Matching draft updates in place with sources and corrections rather than creating duplicates. |
| Technical-only refactor closure versus explicit historical summary with no new behavior | Closure creates no empty business archive; explicit historical documentation still produces a document. |
| Missing conversation context or unresolved business question at discussion-snapshot archival | Supported content persists with gaps; snapshot archival does not claim confirmation, delivery, or verification. |
| Inline-only request or missing writable workspace; workflow missing business-docs | Inline-only output does not claim a saved file; missing workflow integration preserves conclusions and discloses incomplete archival without global installation. |

### Separate read-only review

Scope: requirements, routing, evidence boundaries, archive preservation,
installation independence, authority consistency, and removable complexity in
the delta above. Used the repository review Skill in a separate local pass;
no subagent was required or used.

One destination ambiguity was found: an archived business.md in the same task
could be selected again for a later change. The executor clarified using a new
task directory or dated/specific sibling filename and rechecked that scenario.
Final result: `pass`; no unresolved Critical/Important findings or scoped
conclusion-blocking evidence gaps.

Evidence limits: no fresh-agent benchmark, independent session execution,
production business-module reconstruction, or global installation sync was
performed. Structural validation and manual walkthroughs do not prove general
model reliability. These are follow-up observations, not claimed results.

## Result and residuals

All repository delivery conditions are met. The approved governance changes
are applied; none are blocked. Source-only delivery leaves existing installed
copies to the normal separately requested distribution process.

## Business document

[业务文档整理与归档](business.md)
