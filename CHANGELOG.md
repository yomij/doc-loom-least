# Changelog

## Unreleased

### Changed
- Added workflow-owned Post-execution review with separate Engineering and Spec
  axes, material finding fix/re-review, and no new review phase or artifact.
- Added approved case-scoped semantic atomic commits for plans, green tasks,
  review fixes, and closure; unqualified `Done` now waits for closure-commit
  success in cases that declare the policy.
- Repositioned Doc Loom Least as a repo-native personal product workflow
  substrate whose current supported lifecycle domain is development.
- Grouped canonical skills under `skills/development/`, `skills/governance/`,
  and `skills/assessment/` while keeping invocation names stable.
- Replaced "cache not truth" framing with "routing signal" hierarchy for
  case_state.yaml in shared-protocol and tdd-execute/review.
- Replaced "Read when needed" with explicit trigger conditions in 5 skill
  SKILL.md files (context-authority, plan-confirm, tdd-execute, doc-sync-close,
  setup-doc-governance).
- Added protocol version: 1 to shared-protocol.md.
- Removed duplicated closure status table from doc-sync-close/SKILL.md;
  replaced with reference to shared-protocol.
- Removed duplicated fact authority order from review/SKILL.md; replaced with
  reference to shared-protocol.
- Moved risk levels and plan confirmation policy from shared-protocol to
  development/plan-confirm/references/risk-levels.md with anchoring examples.
- Removed duplicated case identity resolution from
  context-authority/references/context-resolution.md; replaced with reference
  to shared-protocol plus resume guidance.
- Removed duplicated TDD exception list from plan-confirm/SKILL.md; kept only
  plan-level constraints.
- Added fallback behavior (→ Output: …) to every Gate in all 8 SKILL.md files.
- Added Route Output format to shared-protocol.md.
- Added severity anchoring examples to review/SKILL.md Finding Format.
- Added review_risk anchoring examples and lifecycle to tdd-execute/SKILL.md.
- Added Git Degraded Mode section to shared-protocol.md.
- Added Session State Carry section to shared-protocol.md.
- Added Case Creation Boundary section to shared-protocol.md.
- Added Small Project Degradation section to shared-protocol.md.
- Slimmed case_state.yaml section in shared-protocol.md (removed 5 YAML
  examples, added phase owner list).
- Added minimal variant comments to plan.md, closure.md, execution.md
  templates.
- Replaced writing-plans external reference in plan-confirm/SKILL.md with
  self-contained Plan Quality Standard.
- Removed 8 openai.yaml files (no functional purpose, violates Skill Runtime
  Boundary).
- Added Fast-Path section to shared-protocol.md (low-risk, small-change
  shortcut that skips context-authority and simplifies plan/execution/closure).
- Added fast-path route (row 7) to docloom-workflow Route Table with priority
  ordering.
- Upgraded next_skill, route_reason, required_input from optional to required
  routing fields in shared-protocol.md (must be written when routing occurs).
- Reordered docloom-workflow Route Table to priority-sorted numbered list.
- Unified all Gate fallback outputs across 8 SKILL.md files to Route Output
  format (Route: <skill>. Reason: <…>. Required input: <…>).
- Replaced "Do not create X, Y, Z" negative enumeration in shared-protocol
  Artifact Policy with positive principle: "Only create artifacts listed in
  the Artifact Policy table."
- Added conditional anchoring examples for execution.md (3 required, 3 skipped).
- Added staleness threshold (7 days) and resume re-validation rule to
  shared-protocol Case Resume.
- Allowed empty base_commit with git_available: false reason in plan-confirm.
- Replaced generic "triggered sections" in templates (plan.md, execution.md,
  closure.md) with per-section trigger conditions in HTML comments.

### Added
- Added ADR-0001 for lifecycle scope and skill grouping.
- Added docs index, SSOT map, and skills layout map.
- development/plan-confirm/references/risk-levels.md (new file).

## v1 (2026-06-18)

- Initial skill set: 8 skills + shared protocol.
- skillshare installation, update, and verification support.
- Per-skill shared protocol symlinks for skillshare copy-mode compatibility.
