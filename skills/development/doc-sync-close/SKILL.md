---
name: doc-sync-close
description: Close or interrupt a Doc Loom case with criterion evidence, terminal status, and scoped documentation sync.
---

# doc-sync-close

Follow `references/shared-protocol.md` for status, artifacts, and required
commits. Use the authorized contract, execution or compact checks, and triggered
review evidence. Missing identity returns to `docloom-workflow`.

## Terminal Evidence

Assess each criterion as `met`, `partially_met`, `not_met`, `not_verified`, or
`out_of_scope`, with evidence. Write the final status to the existing carrier:

| Case | Carrier |
|---|---|
| Compact | Existing plan criterion Status/Evidence and `final_status` / `closed_at`; keep plan approved and add no body section. |
| Guarded | Thin `closure.md` using `templates/closure.md`. |
| Legacy with closure | Keep `closure.md` as terminal authority. |

The terminal record states the outcome, criteria result, verification
conclusion, and material residuals or follow-ups. Point to execution evidence.

`Done` needs supported criteria, resolved material findings/deviations and high
review risk, passing triggered review, complete terminal evidence, all explicitly
required commits, and no unexplained case changes. Missing optional execution
evidence alone is not a gap. `Done with Caveats` needs a completed main goal
and accepted residual risk or follow-up.

## Sync And Finish

Update operational evidence and traceable derived views. For authority or
nontrivial derived sync, read `references/doc-update-rules.md`: authority changes
remain proposals unless the exact narrow patch is confirmed. Structural or
conflicting changes belong to governance. Unresolved authority candidates remain
visible and prevent unqualified Done.

Do not change the implementation or contract to make closure pass. Commit only
when required; report failed required commits or derived refresh accurately.
Read `templates/handoff.md` only for a real resume point. Report the outcome,
remaining gaps, and next action after writing the terminal carrier.
