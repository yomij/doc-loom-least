# Cases

This file is a derived dashboard for Doc Loom case discovery. Current status
comes from each case's `plan.md`, `execution.md`, and `closure.md`.

If this dashboard conflicts with case artifacts, follow their documented
precedence and refresh this file.

## Active Cases

None.

## Waiting On Human

| Case | Waiting For | Evidence |
|---|---|---|
None.

## Recently Closed

| Case | Closure Status | Evidence | Caveat / Follow-up |
|---|---|---|---|
| `20260712-artifact-owned-case-state` | Done | `20260712-artifact-owned-case-state/closure.md` | Artifact-owned status shipped; dogfood it before merging execution and closure artifacts. |
| `20260712-human-first-workflow-simplification` | Done | `20260712-human-first-workflow-simplification/closure.md` | Human entry, compact Fast-Path, Git disclosure, risk, discovery, summaries, and grill improvements passed review after one atomic fix. |
| `20260710-compress-workflow-skill-text` | Done | `20260710-compress-workflow-skill-text/closure.md` | Seven contracts reduced 1,610→1,140 lines; dual-axis review passed after one atomic wording fix. |
| `20260710-post-execution-review-atomic-commits` | Done | `20260710-post-execution-review-atomic-commits/closure.md` | Mandatory Engineering/Spec review passed after two atomic review fixes; no follow-up. |
| `20260708-plan-approval-direct-execute` | Done with Caveats | `20260708-plan-approval-direct-execute/closure.md` | High-risk workflow-policy change verified by document/skill inspection; no automated workflow interpreter tests exist. |
| `20260703-next-slice-rubric-dogfood` | Done with Caveats | `20260703-next-slice-rubric-dogfood/closure.md` | Compact output applied; validate it on the next real discovery pass before promoting the rubric. |
| `20260703-loop-protocol-fix` | Done | `20260703-loop-protocol-fix/closure.md` | Fast-path defect fix: `loop-protocol.md` reference + wording; no policy change. |
| `20260703-next-slice-discovery-integration` | Done with Caveats | `20260703-next-slice-discovery-integration/closure.md` | Dogfood follow-up consumed by `20260703-next-slice-rubric-dogfood`. |
| `20260703-loop-engineering-skills-integration` | Done with Caveats | `20260703-loop-engineering-skills-integration/closure.md` | High-risk workflow-policy change; keep future loop expansion narrow and evidence-backed. |
| `20260702-fast-path-policy-dogfood` | Done with Caveats | `20260702-fast-path-policy-dogfood/closure.md` | See closure for recovery dogfood caveats. |

## Follow-up Candidates

| ID | Source Case | Candidate | Suggested Route | Reason |
|---|---|---|---|---|
| F2 | `20260703-next-slice-rubric-dogfood` | Validate compact next-slice output on the next real discovery pass before changing the rubric. | `docloom-workflow` | The first dogfood fixed output density but does not yet prove the compact format across another selection. |
| F3 | `20260712-artifact-owned-case-state` | Dogfood artifact-owned status, then evaluate `execution.md` + `closure.md` consolidation and generic-guidance deletion. | `docloom-workflow` | This case removes the duplicate state source; the next simplification should build on observed use rather than combine migrations prematurely. |

## Refresh Rules

- Keep this file short: dashboard rows, not detailed evidence.
- Update it from case artifacts when a case opens, changes status, closes, or
  adds a follow-up candidate.
- Do not use this file as routing truth; use it only to decide which case
  artifacts to read next.
