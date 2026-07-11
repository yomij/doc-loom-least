# Doc Loom Shared Protocol

Protocol version: 1

Use this reference for global vocabulary and cross-skill constraints. Stage
skills own their outputs.

## Skill Ownership

| Area | Owner |
|---|---|
| Case identity, delayed creation, status-only routing, artifact policy | `docloom-workflow` |
| Documentation governance plan and authority rebuild | `setup-doc-governance` |
| Minimal context, authority checks, pre-plan verdict | `context-authority` |
| Plan, risk, version, base commit, confirmation | `plan-confirm` |
| TDD execution/evidence, task/fix commits, Post-execution invocation/fix loop | `tdd-execute` |
| Closure, closure commit, L2/L3 sync, authority proposals/confirmed narrow patches | `doc-sync-close` |
| Read-only ad-hoc and workflow-owned Post-execution evidence review | `review` |
| Interactive pressure testing | `grill` |

Ad-hoc `review`/`grill` require explicit user intent. Assessment skills write no
case artifacts, routes, or status; only `tdd-execute` may invoke workflow-owned
review under an approved eligible plan.

## Execution Instruction Order

1. Current-turn user instructions.
2. Approved governance plan, for governance work.
3. Active Doc Loom skill rules.
4. Active authority docs.
5. Current production code.
6. Current tests.
7. Case docs and evidence.
8. Derived or index docs.
9. Archive or historical docs.
10. Scratch docs.

User instructions may change the current goal, but do not silently rewrite
long-term product, workflow, authority, contract, or agent facts.

## Fact Authority Order

1. Active authority docs.
2. Current production code.
3. Current tests.
4. Accepted ADR, migration, or release note.
5. Current-turn user information, as a pending fact.
6. L2 operational case docs.
7. L3 derived or index docs.
8. L4 archive or historical docs.
9. L5 scratch docs.

Generated views cannot override their source of truth; historical material is
evidence, not current authority. Risk and confirmation policy lives in
`development/plan-confirm/references/risk-levels.md`.

Loop-style discovery follows `loop-protocol.md`. It is a discovery source, not
a separate workflow, and uses these same orders, identity, artifacts,
confirmation, Post-execution review, and manual assessment gates.

## High-Risk Topics And Classification

Security, auth, permission, privacy, billing, data deletion, public API/CLI,
schema, config contract, migration, ADR-protected architecture boundaries, and
workflow or agent policy always require deliberate context checks. Topic alone
does not determine execution risk.

Classify risk from consequence, reversibility, exposure, authority impact, and
verification strength. Security, money, private data, destructive operations,
irreversible migrations, and public compatibility are presumptively high.
Reversible internal workflow or Skill text is normally medium, but remains high
when it weakens authorization, changes a public contract/L1 critical fact, or
enables irreversible action.

## Case Identity

Resolve case id in order:

1. User-specified `case_id` or case docs.
2. Current worktree plus case branch.
3. Current branch.
4. A unique, obvious, not-closed case under `docs/cases/`.
5. `context-authority` proposed slug.
6. `docloom-workflow` creates `YYYYMMDD-short-slug`.

Only `docloom-workflow` creates a case id and `docs/cases/<case-id>/`; later
stage skills consume that identity.

## Artifact-Owned Status

Case artifacts own current workflow status; there is no separate routing-state
artifact.

- `plan.md`: `status: draft | approved`.
- `execution.md`: `status: executing | ready_to_close`.
- `closure.md`: `status` uses the final set below.

Derive the current position from the first reliable signal:

1. `closure.md` with valid required closure-commit evidence -> its final status.
2. `closure.md` without required commit evidence -> closure pending.
3. `execution.md` with `status: ready_to_close` -> closure pending.
4. `execution.md` with `status: executing` -> executing.
5. Approved `plan.md` -> planned.
6. Draft `plan.md` -> waiting for plan confirmation.
7. No clear artifact -> `needs_user_decision`.

Route decisions stay in conversation output or `handoff.md`; do not persist
`next_skill`, route reason, or required input as a second status record. Existing
`case_state.yaml` files are legacy evidence only: do not rewrite closed cases,
and never let legacy state override current artifacts.

Keep `base_commit` and `risk_level` in `plan.md` frontmatter. For an Atomic
Commit plan, final closure evidence is valid only when Git shows `closure.md`
with its final status in the declared closure-step commit and no unexplained
case work remains uncommitted.

## Artifact Policy

| Artifact | Required when | Optional or skipped when | Writer |
|---|---|---|---|
| `context-authority-brief.md` | Conflict, explicit request, continuity need, or high-risk/resume/recovery context too large for `plan.md` | Inline summary or `plan.md` is enough; high risk/resume/recovery alone does not force it | `context-authority` |
| `plan.md` | Case enters `plan-confirm` | No persistent case workflow | `plan-confirm` |
| `handoff.md` | A future resume point exists | Same-session flow | Current stage skill |
| `execution.md` | TDD, code/behavior change, deviation, test retry/failure, material review risk, or continuity evidence | Docs-only/recovery/trivial config whose verification fits in `closure.md` | `tdd-execute` |
| `closure.md` | A case ends, pauses, blocks, cancels, or is superseded | One-shot task without case docs | `doc-sync-close` |

Create only listed artifacts unless the user or an approved governance plan
adds a type.

`docs/cases/README.md` is an optional derived dashboard, never routing or
evidence truth. Refresh it when it conflicts with case artifacts.
`docs/product/current-state.md` is an optional discovery input and cannot
override authority, implementation, ADRs, or user-confirmed facts.

Evidence ownership: `plan.md` records expected acceptance, review/commit
strategies, and planned verification; `execution.md` records actual commands,
deviations, failures/retries, task/fix hashes, and required final axis evidence;
`closure.md` records final acceptance, aggregate review, residual findings,
commit range, and final verification (including full commands when execution is
skipped).

## Closure Status

Allowed final statuses: `Done`, `Done with Caveats`, `Blocked`, `Cancelled`,
`Superseded`, `Paused`, `Abandoned`.

Unmet acceptance cannot be `Done`. Missing ad-hoc review alone is not a
blocker; missing or non-passing required Post-execution evidence blocks
unqualified `Done` for an eligible plan that declares it.

## Route Output

When blocked or handing off, tell the user the status, reason, and decision or
input needed in plain language. This internal diagnostic form is optional:

`Route: <skill-name>. Reason: <brief reason>. Required input: <what the next skill needs>.`

This is text, not an artifact. Continue in-session only with clear intent and
inputs. Approval of the current plan implies same-turn execution unless the
user says plan-only, hold, revise, or review; an older approved plan requires
current execute/continue/reconfirm intent.

Never auto-continue to ad-hoc `review`, `grill`, or `setup-doc-governance`.
Workflow-owned Post-execution review remains available to `tdd-execute`.

## Confirmation Semantics

High-risk confirmation identifies the object, preferably `Approve plan vN` or
`Confirm authority patch for <doc>: <change>`. Version wording is not mandatory
when the current plan is unambiguous; record the approved version. Current-plan
approval authorizes same-turn execution unless the user says plan-only, hold,
revise, or review.

Short `yes`/`ok`/`continue` answers confirm only the immediate recommendation;
they approve high-risk execution or a narrow authority patch only when the
object is unambiguous, and never create long-term authority. Authority patches
must identify the document and concrete change; “sync docs” is insufficient.
Discussion decisions remain task-scoped until recorded in `plan.md`, a
governance plan, or `closure.md`.

Before asking for plan confirmation, surface a short human approval summary:
the outcome, material scope, expected local Git actions/commit count, user
interruptions, and excluded actions. Do not make users discover commit or
authority effects inside a long plan. Fast-Path records the same information in
compact form even when approval is agent-verified.

Approval of a current plan with an Atomic Commit Strategy authorizes only its
declared case-scoped staging and semantic commits. It excludes unrelated files,
another case, push, PR, merge, tag, release, amend, rebase, squash, history
rewriting, material deviations, and unlisted dependency, lockfile, CI, schema,
config-contract, or authority work.

## Atomic Commit Contract

When declared by the plan, commit each independently valid semantic completion
point: approved requirements artifact (when present), approved plan, green
task, independently meaningful verified refactor, material review-fix root
cause, and closure. Never commit Red states, failed attempts, empty stages,
checkboxes, or timestamps as completion points.

Each case commit follows the repository title standard, excludes unrelated
changes, leaves relevant checks passing, and maps to its case and step,
preferably through `Doc-Loom-Case` and `Doc-Loom-Step` trailers. Record actual
task/refactor/fix hashes in `execution.md`; closure never predicts its own hash.

These review/commit gates apply prospectively to eligible cases whose current
approved plan declares them; legacy cases remain valid.

Fast-Path is the compact exception: it does not require separate plan,
implementation, and closure commits. After the green change and compact review,
one `Doc-Loom-Step: closure` commit may contain the minimal approved plan,
implementation/verification, closure, and necessary derived
dashboard sync. Its title describes the shipped change. It must still be
coherent, independently reviewable and revertible, and contain no unrelated
work.

## Git Degraded Mode

When `git_available: false`, continue document work and assessment conversation,
but skip `base_commit`, branch/worktree, diff, stage, and commit operations.
Record why or `commit deferred: git unavailable`; user-supplied changed-file or
commit data remains pending evidence.

An eligible Atomic Commit case needs a Git baseline and required commit evidence
for unqualified `Done`; missing Git evidence permits `Done with Caveats` only by
explicit owner decision.

## Run Modes

| Mode | New branch | New worktree | Use when |
|---|---:|---:|---|
| `isolated` | Yes | Yes | Large, parallel, or high-risk work |
| `branch` | Yes | No | Normal development work |
| `inline` | No | No | Small work, existing branch, temporary work |

Skills may select a mode; project instructions and confirmation rules still
govern branch/worktree creation.

## Case Creation Boundary

Create a case for a requested persistent workflow; medium/high-risk code, test,
or public-contract execution; continuity requiring a context brief; or an
explicit case-doc request.

Do not create one for a one-shot explanation/question/review, low-risk local
edit, status-only request, or standalone conversation-only review/grill.

## Fast-Path

A persistent case may use Fast-Path only when all hold:

- Risk is `low` (docs, local copy, non-critical tests, behavior-preserving
  refactor).
- Change is small (at most 3 files and 20 diff lines).
- No High-Risk Topic conflict.
- No cross-session, multi-agent, or resume continuity need.

Follow `development/plan-confirm/references/risk-levels.md`. `approved_by:
fast-path` records verified conditions under the current request, not unrelated
background permission.

Compact flow: `docloom-workflow` creates only case identity; `plan-confirm` writes a
minimal plan with `status: approved`, `risk_level: low`, `plan_version: 1`,
`approved_by: fast-path`, `approved_at`, `base_commit` or its unavailable
reason, Confirmation Log and Fast-Path Conditions evidence, goal/non-goals,
acceptance/files, a human approval summary, compact review/commit strategies,
and no task-level TDD breakdown or separate plan commit. `tdd-execute` produces
the green change and compact Engineering/Spec review with characterization or
build verification; `execution.md` remains optional. `doc-sync-close` writes a
short `closure.md`, records final status, and creates one combined local completion
commit containing the approved plan, green change, review evidence, closure,
and necessary dashboard sync. Any failed condition uses the full flow.

## Small Project Degradation

Missing paths do not block; create `docs/cases/` with the first case. Missing
authority yields `proceed_to_plan_with_risk`; record `no authority docs found`
or `authority: none, default risk: medium`. Missing governance routes to
`setup-doc-governance` only for authority, public-contract, or High-Risk Topic
work; otherwise proceed with risk.

## Case Resume

Across sessions, read `handoff.md` first when present, then the minimum current
artifact. Re-run `context-authority` only if authority, governance, code, or
external dependencies may have changed, or the case is high risk,
conflict-related, or resumed after a break.

A handoff records phase, last completed step, next step, resume condition,
known issues, and source artifacts. If older than seven days, report staleness
and ask continue/adjust/close. Verify the last step before resuming; route on
mismatch instead of trusting stale handoff state.

## Git And Commands

Read project-local instructions and obey their package manager, shell wrapper,
and editing constraints. If Git is unavailable, record it and never invent a
base commit.
