---
case_id: 20260712-skill-context-cost-compression
status: executing
updated_at: 2026-07-12T13:06:11+08:00
---

# Execution Report

## Human Summary

- Outcome so far: the first green slice compresses the shared protocol and
  human doorway from 3,112 to 1,398 words while preserving the locked routes.
- What changed: shared text now contains only cross-skill invariants;
  `docloom-workflow` contains only public entry, routing, identity, output, and
  hard gates.
- User action needed: none unless a material deviation is found.
- Local Git effect: approved plan commit `db3474d` exists locally; no push or
  history rewrite is authorized.

## Plan Reference

- plan_version: 1
- base_commit: `ead85434eb6c01a8801534b92a37271899a8f055`
- plan_commit: `db3474d`
- adaptive_execution: no

## Plan Followed

Yes. Execution begins with the approved pure-documentation exception and
behavior-preserving characterization.

## TDD Applicability

- TDD Required: No.
- Exception: pure documentation/Skill behavior-preserving refactor; no runtime
  workflow interpreter exists.
- Alternative verification: before/after route cost, semantic anchors,
  validators, YAML/frontmatter, links/symlinks, fresh-agent scenarios, Git
  evidence, and mandatory Engineering/Spec review.

## Baseline Cost

| Surface | Files | Lines | Words |
|---|---:|---:|---:|
| Default doorway | 2 | 488 | 3,112 |
| Context through plan | 9 unique files | 1,042 | 6,085 |
| Typical full persistent flow | 16 unique files | 1,915 | 10,851 |
| All active Skill Markdown | 27 | 2,727 | 14,692 |

The route totals count each required file once. They are repository word-cost
proxies, not exact model-token claims.

## Semantic Ownership Lock

| Contract | Complete owner | Required behavior before editing |
|---|---|---|
| Human entry, status-only, discovery selection, case identity | `docloom-workflow` | Status-only is read-only; discovery creates no case/plan before selection; persistent work routes internally. |
| Cross-skill authority, artifact status, confirmation scope, final statuses | shared protocol | Current artifacts own status; approval is current-object and case-scoped; no hidden routing state. |
| Context/authority verdict | `context-authority` | Workflow/high-risk work reads minimal active authority and blocks real conflicts. |
| Plan, risk, baseline, approval | `plan-confirm` | No execution without current approval; material plan changes require reconfirmation. |
| TDD/exception, execution evidence, commits, review fix loop | `tdd-execute` | Approved exception still verifies; semantic commits remain scoped; non-pass review stays in execution. |
| Engineering/Spec evidence review | `review` | Read-only, exact target/baseline, separate axes, material evidence gaps cannot pass. |
| Closure, final status, docs sync, closure commit | `doc-sync-close` | Unmet acceptance/review/commit/clean-scope gates block unqualified `Done`. |
| Manual challenge | `grill` | Explicit invocation only; outside this change scope. |

## Scenario Lock

| Scenario | Required outcome |
|---|---|
| Explanation or one-shot low-risk edit | No persistent case flow. |
| Status/continue ambiguity | Read-only status; derive from artifacts; do not repair state. |
| Next-slice discovery | Recommend only; no case/plan until user selects. |
| Workflow/agent-policy request | Run minimal context/authority gate before planning. |
| Low-risk Fast-Path | Verified conditions, compact review, one combined completion commit. |
| Non-Fast-Path persistent change | Current plan confirmation before execution. |
| High-risk current plan | Unambiguous approval object; authorization remains narrowly scoped. |
| Resumable closure | Only a newer authorized execution with explicit Resume evidence supersedes it. |
| Git unavailable | Continue document work; do not invent baseline/commits; qualify final status. |
| Eligible completed execution | Separate Engineering/Spec review; only aggregate `pass` becomes ready to close. |
| Material review finding | Atomic root-cause fix and affected-axis re-review. |
| Final closure | Allowed status set, acceptance/review/commit evidence, and clean case scope. |

## Planned Checkpoints

| Step | Commit | Status |
|---|---|---|
| plan | `db3474d` | complete |
| refactor:shared-loading | pending | ready to commit |
| refactor:stage-contracts | pending | pending |
| task:cost-policy | pending | pending |
| closure | pending | pending |

## Review Risk

High until the final common-path delta preserves every locked scenario and both
Post-execution review axes pass.

## Deviations

None.

## Changes Made

### Shared kernel and doorway

- Reduced `shared-protocol.md` from 311 lines / 1,963 words to 138 lines / 898
  words.
- Reduced `docloom-workflow` from 177 lines / 1,149 words to 87 lines / 500
  words.
- Replaced duplicated instruction/fact authority lists with one concise
  priority contract while retaining generated/history boundaries.
- Kept artifact-owned status precedence, resumable closure evidence, allowed
  final statuses, artifact ownership, current-plan authorization, atomic
  commits, Fast-Path, degraded Git, run modes, case boundaries, and resume.
- Kept the public doorway's status-only, next-slice, context-gate, planning,
  execution, closure, review/grill, and no-backend routing behavior.

## Commands Run

| Command / check | Result | Notes |
|---|---|---|
| `wc -l -w` on shared protocol and doorway | pass | 3,112 -> 1,398 words; target <= 1,700. |
| `quick_validate.py skills/development/docloom-workflow` via `uv` | pass | Frontmatter and Skill structure valid. |
| Semantic anchor `rg` for artifact/resume/Fast-Path/Git/confirmation/review routes | pass | Required locked concepts remain. |
| `find -L skills -type l -print` | pass | No broken shared-reference symlink. |
| `git diff --check` | pass | No whitespace errors. |
