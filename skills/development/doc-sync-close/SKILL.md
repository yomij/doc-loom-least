---
name: doc-sync-close
description: Record a Doc Loom case's outcome and synchronize affected documentation when closing or interrupting the work.
---

# Close a Case

Read `references/shared-protocol.md` for status, artifacts, and required commits.
Use the authorized contract, execution evidence or completion checks, and any
required review results. Return missing case identity to `docloom-workflow`.

## Record the Result

Assess each criterion as `met`, `partially_met`, `not_met`, `not_verified`, or
`out_of_scope`, with evidence. Record the outcome, criterion results,
verification conclusion, and material residuals. Reference execution details
instead of repeating them.

| Case | Result location |
|---|---|
| Compact | Plan criterion Status/Evidence and frontmatter `final_status`, `closed_at`; use `verification_summary` and `residuals` when needed. Keep the plan approved; add no body sections. |
| Guarded | `closure.md`, using `templates/closure.md`. |
| Legacy with closure | Existing `closure.md` remains the terminal authority. |

Use `Done` only when criteria are met, material findings, deviations, and high
review risks are resolved, required review passes, terminal evidence is complete,
all required commits exist, and no case changes remain unexplained. An optional
`execution.md` need not exist. Use `Done with Caveats` when the main goal is
complete and residual risks or follow-ups are accepted.

## Sync Documentation and Finish

Update operational evidence and traceable derived views. For authority changes
or nontrivial derived updates, read `references/doc-update-rules.md`. Apply only
confirmed narrow authority patches; route structural changes or conflicts to
governance. Keep unresolved authority proposals visible; they prevent `Done`.

Do not alter the implementation or contract to satisfy closure. Commit when
required and report any required commit or derived update that failed. Use
`templates/handoff.md` only when a future resume point is needed. Report the
outcome, remaining gaps, and next action.
