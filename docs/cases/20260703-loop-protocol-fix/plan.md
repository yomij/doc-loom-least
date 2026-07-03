---
case_id: 20260703-loop-protocol-fix
plan_version: 1
status: approved
risk_level: low
approved_by: fast-path
approved_at: 2026-07-03T10:57:33+08:00
base_commit: b5bb4a3093cda0ccf7f7d54eb42680faeb591e75
---

# Implementation Plan

## Goal

Fix the broken `references/loop-protocol.md` link in `docloom-workflow` and
tighten two wording ambiguities in `loop-protocol.md` found during review: the
required-input field set vs the blocking gate, and the `Dependency` scoring sign.

## Non-goals

- Do not change discovery policy, routing, or confirmation gates.
- Do not rename or move `loop-protocol.md`.
- Do not alter authority docs or the SSOT map.
- Do not stage, commit, push, or publish.

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| `docloom-workflow/references/loop-protocol.md` resolves to the shared file. | `readlink` and `ls -L`. |
| Required-input list marks mandatory vs optional fields, matching the blocking gate. | Inspect `loop-protocol.md`. |
| Scoring term is unambiguous (positive factor clearly named). | Inspect `loop-protocol.md`. |
| No whitespace errors. | `git diff --check`. |

## Fast-Path Conditions

| Condition | Holds | Evidence |
|---|---|---|
| Risk `low` | yes | Markdown/symlink doc fix; no policy change. |
| Small change (≤ 3 files, ≤ 20 lines) | yes | 1 symlink + 2 wording edits in `loop-protocol.md`. |
| No High-Risk Topic conflict | yes | Defect fix and clarification, not workflow/agent policy change. |
| No cross-session/multi-agent/resume need | yes | Single inline turn. |

## Files to Change

- Create: `skills/development/docloom-workflow/references/loop-protocol.md` (symlink to `../../../_shared/references/loop-protocol.md`)
- Modify: `skills/_shared/references/loop-protocol.md`

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-03T10:57:33+08:00 | fast-path | 1 | User requested "Fast-Path 处理" for the three review findings. |
