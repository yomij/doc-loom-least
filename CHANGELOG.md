# Changelog

## Unreleased

### Changed
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
  plan-confirm/references/risk-levels.md with anchoring examples.
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

### Added
- plan-confirm/references/risk-levels.md (new file).

## v1 (2026-06-18)

- Initial skill set: 8 skills + shared protocol.
- skillshare installation, update, and verification support.
- Per-skill shared protocol symlinks for skillshare copy-mode compatibility.
