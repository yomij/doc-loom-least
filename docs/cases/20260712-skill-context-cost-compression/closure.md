---
case_id: 20260712-skill-context-cost-compression
status: Done
updated_at: 2026-07-12T13:22:04+08:00
---

# Closure Report

## Summary

- Outcome: Doc Loom's common Skill loading path now uses one compact shared
  kernel, exact public/internal triggers, single semantic owners, conditional
  references, and lean artifact templates without changing workflow behavior.
- User action needed: none.
- Local Git effect: approved plan, two refactor commits, one cost-policy sync,
  and one review-fix commit exist locally; this report and final evidence are
  finalized by the declared closure commit. No push or history rewrite occurred.

Final cost:

| Surface | Before | After | Reduction | Target |
|---|---:|---:|---:|---:|
| Default doorway | 3,112 | 1,412 | 54.6% | <= 1,700 |
| Context through plan | 6,085 | 2,920 | 52.0% | <= 3,700 |
| Typical full persistent flow | 10,851 | 5,137 | 52.7% | <= 7,200 |
| All active Skill Markdown | 14,692 | 9,079 | 38.2% | <= 11,100 |

## Acceptance Criteria Status

| Criterion | Status | Evidence |
|---|---|---|
| Doorway <= 1,700 words | met | 1,412 unique-path words. |
| Context-through-plan <= 3,700 | met | 2,920 unique-path words. |
| Full flow <= 7,200 | met | 5,137 unique-path words. |
| Active Skill corpus <= 11,100 and no target grows | met | 9,079 words; all 15 compressed targets shrank. |
| No cost moved to another mandatory file | met | Required-read graph shrank; no new reference/file was introduced. |
| Trigger boundaries preserved | met | Description comparison plus 13 fresh-agent positive/negative scenarios. |
| One semantic owner per material rule | met | Ownership/scenario lock and final Spec review. |
| Status, planning, Fast-Path, resume, degraded Git, review, closure, and authority behavior intact | met | Semantic assertions, fresh-agent scenarios, and final review. |
| Artifact-owned status remains exact | met | Shared precedence/resume and all consumers validated. |
| No new Skill/reference/stage/artifact/risk/dependency/runtime/Constitution change | met | Complete tree and changed-file inspection. |
| Skills, YAML, templates, links, and symlinks valid | met | Eight validators, YAML, link/symlink, and diff checks pass. |
| Engineering/Spec aggregate passes | met | Final aggregate `pass`; no residual findings. |

## Tests

Detailed commands, failures/retries, forward-test outputs, and review history are
recorded in `execution.md`.

| Check | Result | Notes |
|---|---|---|
| Eight Skill validator runs via `uv` | pass | All canonical Skills valid. |
| YAML/frontmatter and template parsing | pass | All selected artifacts valid. |
| Route/corpus word-cost accounting | pass | Every required ceiling passed. |
| Semantic positive/stale assertions | pass | Critical trigger, authorization, status, review, and closure contracts remain. |
| Local links and shared symlinks | pass | No broken target. |
| Per-file before/after and changed-file scope | pass | All targets shrank; no unapproved source/mechanism file. |
| Git baseline/range/trailers/worktree | pass | Exact baseline and five declared pre-closure commits verified. |
| Fresh-agent forward tests | pass | 13/13 scenarios; no repository writes. |
| Final Engineering/Spec review | pass | No unresolved Critical, Important, Minor, or material gap. |

## Post-Execution Review

- Baseline: `ead85434eb6c01a8801534b92a37271899a8f055`.
- Initial aggregate: `changes_required` for missing Fast-Path condition evidence,
  ambiguous complexity-only scope, and omitted high-risk authority metadata/
  durable-promotion justification.
- Fix: `7226acf0e28261f164e641793e351e8962f1e828` restored all three edge contracts.
- Final Engineering: No material issue found.
- Final Spec: No material issue found.
- Final aggregate: `pass`.
- Residual findings/evidence gaps: none material. The repository has no runtime
  workflow interpreter; contract behavior was validated by structural,
  semantic, Git, and fresh-agent evidence.

## Commit Summary

| Step | Commit |
|---|---|
| plan | `db3474da4a913448f208d63890949dfb1fa58a75` |
| refactor:shared-loading | `64298637806110fbb5d70c9e31c671c7358faec7` |
| refactor:stage-contracts | `1cad0c1a0ae099ae7437463bea2ea75d02e58ceb` |
| task:cost-policy | `e59944abbf226a87c4e8f3f995c9dcf9dbda01a6` |
| review-fix:F1-F3 | `7226acf0e28261f164e641793e351e8962f1e828` |
| closure | Required in the commit containing this report; verify with `Doc-Loom-Step: closure`. |

## Authority Changes

| Doc | Status | Change | Evidence | Risk | Next |
|---|---|---|---|---|---|
| `docs/authority/architecture/repo-and-skills.md` | applied | Added progressive disclosure, compact shared kernel, conditional references/templates, and single-owner loading model. | Approved plan, final source, cost evidence, review pass. | high | none |
| `docs/authority/workflow/development-flow.md` | applied | Clarified that shared protocol owns cross-skill invariants while stage procedure/details remain triggered at owners. | Approved plan, final source, review pass. | high | none |

These are the exact narrow existing-boundary patches approved by plan v1. No
new authority area, conflict, lifecycle migration, Constitution change, or
agent/Git policy change was introduced. `skills/README.md` received only the
corresponding derived maintainer map and ceilings.

## Remaining Risks

None material. Word count is a stable repository proxy rather than an exact
runtime-token measurement, so future changes should continue to check both
unique required files and corpus size. The authority loading discipline keeps
that constraint durable without adding a checker or workflow stage.

## Follow-ups

- The artifact-owned status case has now been dogfooded successfully through a
  full plan/execution/review/closure cycle.
- If artifact ceremony remains costly in later real cases, evaluate
  `execution.md` + `closure.md` consolidation as a separate case; it was
  intentionally excluded here.

## Final Status

Done. Reportable only after the declared closure commit succeeds and the
case-related worktree is clean.
