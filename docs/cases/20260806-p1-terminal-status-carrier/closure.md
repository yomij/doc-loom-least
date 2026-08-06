---
case_id: 20260806-p1-terminal-status-carrier
status: Done
updated_at: 2026-08-06T09:52:30+08:00
---

# Closure Report

## Summary

- Outcome: Compact cases close on plan terminal fields; Guarded cases keep thin `closure.md`. Done Gate and residual/follow-up duties remain.
- User action needed: none. Dogfood Compact plan-only close on a later ordinary case.
- Local Git effect: implementation + this thin closure commit on `main` after baseline `6b967c4`. No push.

## Acceptance Criteria Status

| Criterion | Status | Evidence |
|---|---|---|
| Compact no required `closure.md` | met | protocol + doc-sync-close terminal table |
| Compact `final_status`/`closed_at` | met | plan template + protocol |
| Guarded thin `closure.md` | met | protocol + this carrier |
| Thin forbids full restatement | met | closure template |
| Status order legacy-first | met | shared-protocol steps 1–2 |
| Terminal field write ≠ reapproval | met | protocol + close skill |
| Historical `closure.md` valid | met | protocol compatibility |
| Done Gate retained | met | doc-sync-close Done Gate |
| Authority development-flow aligned | met | narrow patch applied |

## Remaining Risks

- Agents may still habitually create Compact `closure.md` until dogfood corrects behavior; skills forbid requiring it.
- Empty `final_status`/`closed_at` keys on open plan templates need readers to treat blank as unset.

## Follow-ups

- Dogfood one Compact case that closes only via plan `final_status` (no `closure.md`).

## Final Status

Done
