# Cases

This file is a derived dashboard for Doc Loom case discovery. Source of truth
remains each case's `case_state.yaml`, `plan.md`, `execution.md`, and
`closure.md`.

If this dashboard conflicts with a case artifact, trust the case artifact and
refresh this file.

## Active Cases

None.

## Waiting On Human

None.

## Recently Closed

| Case | Closure Status | Evidence | Caveat / Follow-up |
|---|---|---|---|
| `20260703-loop-protocol-fix` | Done | `20260703-loop-protocol-fix/closure.md` | Fast-path defect fix: `loop-protocol.md` reference + wording; no policy change. |
| `20260703-next-slice-discovery-integration` | Done with Caveats | `20260703-next-slice-discovery-integration/closure.md` | Dogfood the simple next-slice scoring rubric before promoting it into stronger authority. |
| `20260703-loop-engineering-skills-integration` | Done with Caveats | `20260703-loop-engineering-skills-integration/closure.md` | High-risk workflow-policy change; keep future loop expansion narrow and evidence-backed. |
| `20260702-fast-path-policy-dogfood` | Done with Caveats | `20260702-fast-path-policy-dogfood/closure.md` | See closure for recovery dogfood caveats. |

## Follow-up Candidates

| ID | Source Case | Candidate | Suggested Route | Reason |
|---|---|---|---|---|
| F1 | `20260703-next-slice-discovery-integration` | Dogfood next-slice discovery on a small real feature before changing the scoring rubric. | `docloom-workflow` -> `plan-confirm` | The rubric is intentionally simple and should earn authority through use. |

## Refresh Rules

- Keep this file short: dashboard rows, not detailed evidence.
- Update it from case artifacts when a case opens, changes phase, closes, or
  adds a follow-up candidate.
- Do not use this file as routing truth; use it only to decide which case
  artifacts to read next.
