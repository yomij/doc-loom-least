---
case_id: 20260807-outcome-contract-text-compression
plan_version: 1
status: approved
risk_level: medium
assurance_mode: guarded
approved_by: user
approved_at: 2026-08-07T10:53:00+08:00
confirmation: 'User said “执行”, explicitly authorizing current plan v1.'
base_commit: dfd85af3ccd5c542c6cec46e401a558625e77f51
final_status:
closed_at:
---

# Goal

Reduce the token cost of the outcome-contract workflow text introduced or
rewritten by `dfd85af` while preserving its complete authorization, assurance,
status, evidence, and human-semantic meaning.

# Success Criteria

| Criterion | Evidence required | Status | Evidence |
|---|---|---|---|
| The seven runtime-loaded core contracts fall from the `dfd85af` baseline of 9,017 `cl100k_base` tokens to at most 8,115 tokens. | Reproducible per-file and total before/after token counts using the same tokenizer. | pending | |
| Compression removes repetition and excess wording without deleting, weakening, or relocating any required workflow behavior. | Exact-diff semantic audit maps retained ownership, routing, authorization, reapproval, TDD, review, Git, status, artifact, resume, and Done gates. | pending | |
| The plan interface remains exactly Goal, Success Criteria, and Constraints; execution continues to own implementation-path choices. | Canonical template/Skill checks and active-contract residue search pass. | pending | |
| Active authority and user-facing explanations remain concise and consistent with the compressed Skill implementation. | Relevant authority, English/Chinese entry docs, and detailed workflow explanation contain no conflict or stale superseded wording. | pending | |
| Historical Case evidence and unrelated repository facts remain unchanged. | Diff scope contains only the new Case plus active text affected by the prior outcome-contract change. | pending | |
| Structural, semantic, and guarded-review evidence supports unqualified completion. | Skill validation, frontmatter/heading checks, diff checks, exact-baseline Engineering/Spec review, and final worktree-scope audit pass. | pending | |

# Constraints

- This is semantic-preserving compression, not a new workflow-policy change.
  Preserve Direct/Compact/Guarded routing, authority order, current-plan
  confirmation, exact baseline, TDD or confirmed exception, conditional
  evidence, dual-axis review triggers, terminal carriers, resume rules, Git
  exclusions, and every stop/reconfirm/Done condition.
- Optimize the runtime-loaded contracts first:
  `skills/_shared/references/shared-protocol.md` and the six active development/
  assessment `SKILL.md` files measured in the baseline. Supporting templates,
  authority, and public explanations may change only to stay concise and
  semantically aligned.
- Do not edit the constitution, archived material, historical Case artifacts,
  diagrams, dependencies, lockfiles, CI, schemas, config contracts, or external
  resources.
- Do not game the metric through broken grammar, unexplained terminology,
  unreadable abbreviation, removed safety clauses, or moving required common
  rules into files that increase effective loading cost.
- This pure Skill/Markdown refactor has a confirmed TDD exception; do not invent
  a synthetic Red. Completion still requires quantitative, structural,
  semantic, diff-scope, and guarded-review evidence.
- Do not commit, push, publish, synchronize installed Skills, or rewrite history
  without separate user authorization.
