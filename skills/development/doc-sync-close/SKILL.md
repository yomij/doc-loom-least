---
name: doc-sync-close
description: Close a Doc Loom case after execution or interruption. Own final criteria/status, safe docs sync, authority proposals/approved narrow patches, and required completion commit.
---

# doc-sync-close

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared identity, status,
  artifact, or commit rules.
- [Document update rules](./references/doc-update-rules.md): L1 authority, L3
  derived sync, or an authority-change table only.
- [Handoff template](./templates/handoff.md): writing a real future resume
  point only.
- [Closure template](./templates/closure.md): writing or validating Guarded
  thin `closure.md` only.

## Inputs And Workflow

Consume contract, execution/compact checks, triggered review aggregate,
required-commit hashes, user findings, current artifacts, and relevant docs.
Missing identity returns to the doorway.

1. Choose a shared final status from evidence/user instruction.
2. Map each criterion with evidence to `met`, `partially_met`, `not_met`,
   `not_verified`, or `out_of_scope`; apply Done Gate.
3. Summarize changes, checks, unrun work, commits, and risks without repetition.
4. Sync L2 and mechanically safe L3; propose authority or apply only the exact
   approved narrow patch.
5. Write path-correct terminal evidence; refresh dashboard and evidenced state.
6. When policy/Constraint requires completion commit, stage/inspect/check/
   commit and verify title/trailers; otherwise validate artifacts without
   bookkeeping Git work.
7. Write handoff only for a real future resume point.

## Terminal Carrier

| Path | Write | Do not |
|---|---|---|
| Compact persistent | Update the existing Success Criteria Status/Evidence columns and set `plan.md` `final_status` / `closed_at` | Add another plan-body section or create `closure.md` only to close Compact |
| Guarded | Write thin `closure.md` as terminal SSOT | Restate full tests/review/commit chronicles from execution |
| Legacy case that already has `closure.md` | Keep updating that file as terminal SSOT | Force migration onto plan fields |

Compact leaves plan `approved`; shared readers use `final_status`. Criterion
evidence and terminal metadata are proof, not contract change.

Guarded closure contains Summary, criteria/pointers, Remaining Risks,
Follow-ups, and Final Status (about 40 lines). Reference execution detail; if
absent, include a compact criteria/test/diff/scope check.

## Done Gate

Unqualified `Done` requires:

- every criterion met or explained out of scope;
- triggered execution evidence, or compact terminal verification;
- confirmed deviations and resolved material user findings;
- resolved high review risk;
- triggered deep review has exact target, separate axes, aggregate `pass`, and
  finding/re-review/residual disposition;
- every explicitly required semantic/completion commit succeeded;
- no unexplained case-related worktree change;
- a complete terminal carrier for the path (plan fields or thin `closure.md`).

`not_verified` needs evidence, user caveat, or non-Done; `partially_met` needs
accepted follow-up and cannot be Done; `not_met` normally interrupts;
`out_of_scope` explains why. Missing optional execution alone does not downgrade.

Use `Done with Caveats` only for a completed main goal with accepted residual
risk or follow-up.

## Knowledge And Authority

Classify knowledge as `authority_candidate`, `ADR_candidate`,
`regression_candidate`, `runbook_candidate`, `derived_sync`, `case_local`, or
`none`; never silently promote. Unresolved authority candidate blocks Done.

Authority defaults to proposal. Apply only a confirmed, rule-compliant,
existing-boundary narrow patch; structural/high-risk/conflicting/new-area work
becomes governance/new case.

## Closure And Gates

Guarded closure frontmatter is `case_id`, final `status`, `updated_at`; no
terminal carrier predicts its own hash or contains fixes. Complete evidence is
terminal unless a required commit remains pending; failure preserves evidence
and blocks Done.

Record failed derived refresh without rolling back valid close.

- Every ended/paused/blocked/cancelled/superseded case needs a path-correct
  terminal carrier; Compact uses plan fields, Guarded uses thin `closure.md`.
- Never report final close before writing that carrier.
- No concrete authority confirmation -> proposal/wait, not patch.
- Do not change code, tests, scripts, dependencies, lockfiles, CI, Success Criteria,
  or plan semantics to make closure pass.
- A failed Done Gate condition blocks an unqualified Done report.
- Report Done after complete terminal evidence, every triggered gate and
  explicitly required commit pass, and no case-scope change remains unexplained.
