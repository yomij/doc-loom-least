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
| `20260703-next-slice-rubric-dogfood` | Done with Caveats | `20260703-next-slice-rubric-dogfood/closure.md` | Compact output applied; validate it on the next real discovery pass before promoting the rubric. |
| `20260703-loop-protocol-fix` | Done | `20260703-loop-protocol-fix/closure.md` | Fast-path defect fix: `loop-protocol.md` reference + wording; no policy change. |
| `20260703-next-slice-discovery-integration` | Done with Caveats | `20260703-next-slice-discovery-integration/closure.md` | Dogfood follow-up consumed by `20260703-next-slice-rubric-dogfood`. |
| `20260703-loop-engineering-skills-integration` | Done with Caveats | `20260703-loop-engineering-skills-integration/closure.md` | High-risk workflow-policy change; keep future loop expansion narrow and evidence-backed. |
| `20260702-fast-path-policy-dogfood` | Done with Caveats | `20260702-fast-path-policy-dogfood/closure.md` | See closure for recovery dogfood caveats. |

## Follow-up Candidates

| ID | Source Case | Candidate | Suggested Route | Reason |
|---|---|---|---|---|
| F2 | `20260703-next-slice-rubric-dogfood` | Validate compact next-slice output on the next real discovery pass before changing the rubric. | `docloom-workflow` | The first dogfood fixed output density but does not yet prove the compact format across another selection. |

## Refresh Rules

- Keep this file short: dashboard rows, not detailed evidence.
- Update it from case artifacts when a case opens, changes phase, closes, or
  adds a follow-up candidate.
- Do not use this file as routing truth; use it only to decide which case
  artifacts to read next.
