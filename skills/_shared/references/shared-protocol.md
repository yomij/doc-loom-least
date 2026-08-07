# Doc Loom Shared Protocol

Protocol version: 1. Keep only facts needed across multiple Skills; stage
procedure belongs to its owner.

## Ownership

| Contract | Owner |
|---|---|
| Human entry, case identity, status/discovery routing | `docloom-workflow` |
| Governance rebuild | `setup-doc-governance` |
| Context and authority verdict | `context-authority` |
| Outcome contract, risk/assurance metadata, baseline, confirmation | `plan-confirm` |
| TDD, conditional execution evidence, execution checks, review loop | `tdd-execute` |
| Closure, conditional completion commit, safe docs sync | `doc-sync-close` |
| Read-only evidence review | `review` |
| Interactive challenge | `grill` |

Ad-hoc `review` and `grill` require explicit user intent. Only an authorized
execution with a guarded-review trigger may invoke workflow-owned
Post-execution review.

## Authority

Execution authority descends through current-turn instructions, approved
governance, active Skills/authority, implementation/tests, case evidence,
derived docs, history, then scratch. Facts use active authority,
implementation/tests, accepted decisions/releases, current pending facts, case
evidence, derived views, history, then scratch. A request or generated/
historical text never silently rewrites durable facts.

Security, auth, permission, privacy, billing, deletion, public API/CLI, schema,
config, migration, ADR, and workflow/agent policy require context checking.
Classify consequence, reversibility, exposure, authority impact, and evidence:

- `low`: small, reversible, local, and well verified, with no meaningful
  authority, public, private-data, money, or destructive effect.
- `medium`: reversible internal behavior, Skill, API, data-flow, or broader
  local change with bounded recovery.
- `high`: security, money, private data, irreversible/destructive action,
  public compatibility, L1 critical fact, or authorization weakening.

Sensitive medium work still needs the context check.

## Proportional Path

Ask independently whether work needs a case (continuity, durable decision,
explicit request, or guarded execution) and whether it needs guarded assurance
(consequence, irreversibility, exposure, authority impact, or weak evidence).
These are behaviors, not stages or risk levels:

- **Direct:** reversible one-turn low/medium work executes, verifies, and
  reports without a case.
- **Compact persistent:** continuity, durable decision, or explicit case need
  creates a concise outcome contract and records terminal status on it.
  Execution evidence, deep review, and commits are conditional; `closure.md` is
  Guarded/legacy only.
- **Guarded:** high-risk, public/authority-sensitive, irreversible, destructive,
  or weakly verified work requires an explicit current-plan confirmation,
  exact-baseline Engineering/Spec review, and thin `closure.md` evidence.

Medium risk alone triggers neither case nor confirmation. Every path preserves
applicable TDD or a credible alternative verification.

## Case Identity And Status

Resolve identity by user-specified case, worktree/case branch, current branch,
one obvious open case, proposed slug, then `YYYYMMDD-short-slug`. Only
`docloom-workflow` creates its directory.

Artifacts own current status:

- `plan.md`: `draft | approved`, plus compact terminal fields when closed
  without `closure.md` (`final_status`, `closed_at`).
- `execution.md`: `executing | ready_to_close` when that artifact exists.
- `closure.md`: Guarded (or legacy) terminal status below.

Derive status in this order:

1. A valid `closure.md` with `Done`, `Cancelled`, `Superseded`, or `Abandoned`
   is terminal when any explicitly required commit is present (legacy and Guarded).
2. Else a valid plan `final_status` of those same values is terminal for
   Compact when any explicitly required commit is present.
3. A valid `Paused`, `Blocked`, or `Done with Caveats` on `closure.md`, else on
   plan `final_status`, remains current unless a later authorized
   `execution.md` contains required Resume evidence.
4. A terminal carrier missing a commit required by project policy or an
   approved Constraint, or a `ready_to_close` execution, is closure pending.
5. Otherwise use `executing`, approved plan, draft plan, then
   `needs_user_decision`.

Never persist a second route/status record; legacy `case_state.yaml` is
diagnostic. Completion needs valid terminal evidence, no unexplained case work,
and every explicitly required commit. Historical closures remain valid.

## Artifacts

| Artifact | Required when | Owner |
|---|---|---|
| `context-authority-brief.md` | Conflict, explicit request, continuity, or supporting context that must survive independently | `context-authority` |
| `plan.md` | A persistent case enters planning; Compact terminal status when the case ends without Guarded thin closure | `plan-confirm` writes Goal/Success Criteria/Constraints; `doc-sync-close` writes Compact criterion evidence and terminal fields |
| `handoff.md` | A future resume point exists | Producing `tdd-execute` or `doc-sync-close` stage |
| `execution.md` | Resume evidence, material deviation, meaningful failure/retry history, deep-review findings, or explicit request | `tdd-execute` |
| `closure.md` | Guarded case ends, pauses, blocks, cancels, or is superseded; also valid for legacy cases | `doc-sync-close` |

Create no other case artifact without user or governance authority. Dashboards
and product-state views are derived inputs only. Plan records the contract and
Compact verdict; execution records triggered path evidence; thin Guarded
closure records criteria, residuals, and follow-ups without repeating execution.

Final statuses: `Done`, `Done with Caveats`, `Blocked`, `Cancelled`,
`Superseded`, `Paused`, `Abandoned`. Unmet criteria or required non-passing
review blocks `Done`. Compact criterion evidence and terminal fields are not a
plan change.

## Confirmation And Git Scope

The **outcome contract** is Goal, Success Criteria, and Constraints. Constraints
hold guardrails, non-goals, protected effects, reapproval triggers, exclusions,
and owner mandates. The **adaptive path**—context use, files, tasks, commands,
sequence, run mode, tests/TDD, review, and commit organization—belongs to
execution, never the plan body.

Compact may record a current unambiguous execute request as approval. Guarded
requires explicit confirmation of the written current plan. Older plans need
current intent; guarded approval names object/version. A short
`yes`/`ok`/`continue` confirms only the immediate unambiguous recommendation,
not durable authority.

Before guarded confirmation summarize outcome, material scope, local Git,
interruptions, and exclusions. Authorization excludes unrelated/other-case
work, publication/history rewriting, and Constraint violations. A **protected
change**—contract semantics, risk escalation, authority/public contract,
dependency/lockfile/CI/schema/config, external resource, irreversible action,
or another protected effect—requires reconfirmation. A narrow authority patch
names its document and change.

When policy or a Constraint requires a commit, use a valid/revertible semantic
point: approved requirements/plan, green behavior, verified refactor, review-fix
batch, or terminal completion. Never commit failed attempts/bookkeeping. Use
explicit staging, passing checks, repository titles, and `Doc-Loom-Case` /
`Doc-Loom-Step` trailers. Execution chooses boundaries and records triggered
hash evidence; terminal carriers never predict their own hash.

Ordinary cases need no plan/closure bookkeeping commits. Only an explicitly
required commit gates closure and `Done`.

Without Git, continue safe docs/assessment but invent no baseline or commit.
Missing required guarded Git evidence needs an owner-approved caveat.

Run modes are `isolated` (new branch/worktree for large, parallel, or high-risk
work), `branch` (normal development), and `inline` (small existing-branch work).

## Boundaries And Resume

Do not create cases for explanation/status, standalone review/grill, or
reversible one-turn low/medium work.

Across sessions read `handoff.md`, then minimum current evidence; recheck changed
authority, implementation, dependencies, risk, or conflict. Handoffs older than
seven days are stale.
Terminal closures do not resume in the same case by default. A resumable
closure requires current authorization, satisfied resume conditions, and a
later `execution.md` Resume section naming prior status/evidence and reason.

The shared template owns handoff shape; its producer owns content. Router and
context may consume it, but handoff never owns status.

Follow project-local command, package-manager, and editing instructions.
