# Doc Loom Shared Protocol

Protocol version: 1. Keep only facts needed across multiple Skills; stage
procedure belongs to its owner.

## Ownership

| Contract | Owner |
|---|---|
| Human entry, case identity, status/discovery routing | `docloom-workflow` |
| Governance rebuild | `setup-doc-governance` |
| Context and authority verdict | `context-authority` |
| Plan, risk, baseline, confirmation | `plan-confirm` |
| TDD, conditional execution evidence, execution checks, review loop | `tdd-execute` |
| Closure, conditional completion commit, safe docs sync | `doc-sync-close` |
| Read-only evidence review | `review` |
| Interactive challenge | `grill` |

Ad-hoc `review` and `grill` require explicit user intent. Only an authorized
execution with a guarded-review trigger may invoke workflow-owned
Post-execution review.

## Authority

Execution instructions follow current-turn instructions, an approved governance
plan, active Skills/authority, implementation/tests, case evidence, derived
docs, history, then scratch. Current requests do not silently rewrite durable
workflow, authority, contract, or agent facts. Factual claims prefer active
authority, implementation/tests, accepted decisions/releases, current-turn
pending facts, case evidence, derived views, history, then scratch. Generated or
historical material cannot override its source.

Security, auth, permission, privacy, billing, deletion, public API/CLI, schema,
config contract, migration, ADR boundaries, and workflow/agent policy require a
context check. Classify execution risk by consequence, reversibility, exposure,
authority impact, and verification:

- `low`: small, reversible, local, and well verified, with no meaningful
  authority, public, private-data, money, or destructive effect.
- `medium`: reversible internal behavior, Skill, API, data-flow, or broader
  local change with bounded recovery.
- `high`: security, money, private data, irreversible/destructive action,
  public compatibility, L1 critical fact, or authorization weakening.

A sensitive topic still needs context checking when final execution risk is
medium.

## Proportional Path

Answer two independent questions before entering durable workflow machinery:

1. Does the work need continuity, a durable human decision, an explicit case,
   or guarded execution?
2. Do consequence, irreversibility, exposure, authority impact, or weak
   verification require guarded assurance?

These are execution behaviors, not new stages or risk levels:

- **Direct:** reversible, one-turn low/medium work uses normal repository
  execution, verification, and final reporting without a case.
- **Compact persistent:** continuity, durable decision, or explicit case need
  creates a concise plan. Terminal status is recorded on that plan when the
  case ends. Execution evidence, deep review, commits, and a separate
  `closure.md` are not required on the compact path.
- **Guarded:** high-risk, public/authority-sensitive, irreversible, destructive,
  or weakly verified work requires an explicit current-plan confirmation,
  exact baseline, deep Engineering/Spec review, and a thin `closure.md` as
  terminal evidence.

Medium risk alone triggers neither case nor confirmation. Sensitive medium work
still gets the context check needed to verify its classification.
Every path preserves applicable project TDD; when TDD does not fit, use a
credible alternative verification rather than skipping evidence.

## Case Identity And Status

Resolve identity in order: user-specified case, current worktree/case branch,
current branch, one obvious open case, context-proposed slug, then a new
`YYYYMMDD-short-slug`. Only `docloom-workflow` creates the case directory.

Artifacts own current status:

- `plan.md`: `draft | approved`, plus compact terminal fields when closed
  without `closure.md` (`final_status`, `closed_at`).
- `execution.md`: `executing | ready_to_close` when that artifact exists.
- `closure.md`: Guarded (or legacy) terminal status below.

Derive status in this order:

1. A valid `closure.md` with `Done`, `Cancelled`, `Superseded`, or `Abandoned`
   is terminal when any commit it requires is present (legacy and Guarded).
2. Else a valid plan `final_status` of those same values is terminal for
   Compact when any commit the plan requires is present.
3. A valid `Paused`, `Blocked`, or `Done with Caveats` on `closure.md`, else on
   plan `final_status`, remains current unless a later authorized
   `execution.md` contains required Resume evidence.
4. A terminal carrier missing a commit explicitly required by its approved
   plan, or a `ready_to_close` execution, is closure pending.
5. Otherwise use `executing`, approved plan, draft plan, then
   `needs_user_decision`.

Do not persist a second route/status record; legacy `case_state.yaml` is
diagnostic only. Completion needs valid final artifact evidence and no
unexplained case-related work, plus any commit declared by the approved plan.
Existing historical `closure.md` files remain valid terminal carriers.

## Artifacts

| Artifact | Required when | Owner |
|---|---|---|
| `context-authority-brief.md` | Conflict, explicit request, or continuity context too large for the plan | `context-authority` |
| `plan.md` | A persistent case enters planning; Compact terminal status when the case ends without Guarded thin closure | `plan-confirm` writes plan; `doc-sync-close` writes Compact terminal fields |
| `handoff.md` | A future resume point exists | Producing `tdd-execute` or `doc-sync-close` stage |
| `execution.md` | Resume evidence, material deviation, meaningful failure/retry history, deep-review findings, or explicit request | `tdd-execute` |
| `closure.md` | Guarded case ends, pauses, blocks, cancels, or is superseded; also valid for legacy cases | `doc-sync-close` |

Create no other case artifact without user or approved-governance authority.
Dashboards/product-state views are derived inputs, never routing or fact
authority. Plan records expectations and, on Compact close, the terminal
verdict. Execution records triggered actual evidence. Thin `closure.md` is the
Guarded terminal verdict (acceptance summary, residuals, follow-ups); it must
not restate full command or review narratives already in execution.

Final statuses are `Done`, `Done with Caveats`, `Blocked`, `Cancelled`,
`Superseded`, `Paused`, and `Abandoned`. Unmet acceptance or a required
non-passing Post-execution review cannot be unqualified `Done`. Writing only
Compact terminal fields (`final_status`, `closed_at`, optional Final status
body) is not an escalation and does not require plan reapproval.

## Confirmation And Git Scope

Compact persistent work may record a current unambiguous execute request as
approval. Guarded work requires explicit confirmation of the written current
plan. Older plans need current execute/continue intent; guarded approval names
the current object/version.

A short `yes`/`ok`/`continue` confirms only the immediate unambiguous
recommendation and never creates durable authority.

Approval binds an outcome envelope: Goal, Guardrails/Non-goals, Acceptance,
Escalation Triggers, and any specifically protected boundary. Exact file lists,
added tests, and ordinary internal implementation choices are planning
evidence; they may adapt inside that envelope without reapproval.

Before guarded confirmation, summarize outcome, material scope, local Git
effects, interruptions, and exclusions. Authorization excludes unrelated work,
another case, publication/history rewriting, and escalation triggers. Reconfirm
for outcome/non-goal change, risk escalation, authority/public contract,
dependency/lockfile/CI/schema/config-contract effect, external resource,
irreversible action, or another protected boundary. A narrow authority patch
names the document and concrete change.

Declared commits contain independently valid/revertible completion points:
approved requirements/plan, green task, verified refactor, material review-fix
batch, or terminal completion. Boundaries follow semantics, not
artifact/finding count. Never commit failed attempts or bookkeeping alone. Use
repository title rules, explicit staging, passing checks, and `Doc-Loom-Case` /
`Doc-Loom-Step` trailers. Execution records actual hashes when present;
otherwise the terminal carrier does. Terminal carriers never predict their own
commit hash.

Ordinary cases do not require standalone plan or closure bookkeeping commits.
A guarded plan may declare them when durable approval/auditability justifies
the cost; only a declared commit gates closure and unqualified `Done`.

If Git is unavailable, continue safe document/assessment work but omit invented
baselines, staging, and commits. A guarded case missing required Git evidence
can become `Done with Caveats` only by explicit owner decision.

Run modes are `isolated` (new branch/worktree for large, parallel, or high-risk
work), `branch` (normal development), and `inline` (small existing-branch work).

## Boundaries And Resume

Do not create a case for explanation/status-only, standalone review/grill, or
reversible one-turn low/medium work; the proportional path owns positive
triggers.

Across sessions, read `handoff.md` when present, then the minimum current
artifact. Recheck changed authority, implementation, dependencies, risk, or
conflict. Handoffs older than seven days are stale.
Terminal closures do not resume in the same case by default. A resumable
closure requires current authorization, satisfied resume conditions, and a
later `execution.md` Resume section naming prior status/evidence and reason.

`skills/_shared/templates/handoff.md` owns the handoff shape. The producing
stage owns a concrete file's content; `docloom-workflow` and
`context-authority` may consume it for resume routing/revalidation. A handoff
never owns or overrides case status.

Follow project-local command, package-manager, and editing instructions.
