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
| TDD, execution evidence, task/fix commits, review loop | `tdd-execute` |
| Closure, closure commit, safe docs sync | `doc-sync-close` |
| Read-only evidence review | `review` |
| Interactive challenge | `grill` |

Ad-hoc `review` and `grill` require explicit user intent. Only an approved
eligible execution may invoke workflow-owned Post-execution review.

## Authority

Follow current-turn instructions, then an approved governance plan when one
exists, active Skill rules, active authority, implementation, tests, case
evidence, derived docs, history, and scratch material. User instructions may
change the current task but do not silently rewrite durable workflow, authority,
contract, or agent facts.

For factual claims prefer active authority, implementation, tests, accepted
ADR/migration/release evidence, current-turn pending facts, case evidence,
derived views, history, then scratch. Generated and historical material cannot
override its source.

Security, auth, permission, privacy, billing, deletion, public API/CLI, schema,
config contract, migration, ADR boundaries, and workflow/agent policy require a
context check. Classify execution risk by consequence, reversibility, exposure,
authority impact, and verification. Reversible internal Skill text is normally
medium; authorization weakening, public compatibility, L1 critical facts, or
irreversible impact is high.

## Case Identity And Status

Resolve identity in order: user-specified case, current worktree/case branch,
current branch, one obvious open case, context-proposed slug, then a new
`YYYYMMDD-short-slug`. Only `docloom-workflow` creates the case directory.

Artifacts own current status:

- `plan.md`: `draft | approved`.
- `execution.md`: `executing | ready_to_close`.
- `closure.md`: a final status below.

Derive status in this order:

1. A committed `Done`, `Cancelled`, `Superseded`, or `Abandoned` closure is
   terminal.
2. A committed `Paused`, `Blocked`, or `Done with Caveats` closure remains
   current unless a later authorized `execution.md` contains the required
   Resume evidence.
3. An uncommitted closure, or `ready_to_close` execution, is closure pending.
4. Otherwise use `executing`, approved plan, draft plan, then
   `needs_user_decision`.

Do not persist route output as a second status record. Legacy
`case_state.yaml` is diagnostic history only. Atomic completion requires the
final closure artifact/status in its declared commit and no unexplained
case-related work.

## Artifacts

| Artifact | Required when | Owner |
|---|---|---|
| `context-authority-brief.md` | Conflict, explicit request, or continuity context too large for the plan | `context-authority` |
| `plan.md` | A persistent case enters planning | `plan-confirm` |
| `handoff.md` | A future resume point exists | Current stage |
| `execution.md` | TDD/behavior change, failure/retry/deviation, material review risk, or continuity evidence | `tdd-execute` |
| `closure.md` | A case ends, pauses, blocks, cancels, or is superseded | `doc-sync-close` |

Create no other case artifact without user or approved-governance authority.
Dashboards and product-state views are optional derived inputs, never routing
or fact authority. Plan records expected evidence; execution records actual
commands, deviations, hashes, and review; closure records final acceptance,
residuals, commit range, and final verification.

Final statuses are `Done`, `Done with Caveats`, `Blocked`, `Cancelled`,
`Superseded`, `Paused`, and `Abandoned`. Unmet acceptance or a required
non-passing Post-execution review cannot be unqualified `Done`.

## Confirmation And Git Scope

Approval of the current plan normally authorizes same-turn execution unless the
user says hold, revise, or review first. An older plan needs current
execute/continue intent. High-risk approval must identify the current object;
record its version even when the user does not name it.

A short `yes`/`ok`/`continue` confirms only the immediate unambiguous
recommendation and never creates durable authority.

Before confirmation, summarize outcome, material scope, expected local Git
actions/commit count, interruptions, and excluded actions. Approval authorizes
only declared case paths and semantic commits. It excludes unrelated work,
another case, push, PR, merge, tag, release, amend, rebase, squash, history
rewrite, material deviation, and unlisted dependency, lockfile, CI, schema,
config-contract, or authority changes. A narrow authority patch must name the
document and concrete change.

When a plan declares atomic commits, commit independently valid completion
points only: approved requirements/plan, green task, verified refactor,
material review fix, and closure. Never commit failed attempts or bookkeeping
alone. Use repository title rules, explicit staging, passing checks, and
`Doc-Loom-Case` / `Doc-Loom-Step` trailers. Execution records actual task/fix
hashes; closure never predicts its own hash.

Fast-Path is the low-risk exception: at most three files/twenty lines, no
high-risk conflict, and no continuity need. It uses a minimal approved plan,
green verification, compact Engineering/Spec review, and one coherent
`Doc-Loom-Step: closure` completion commit containing plan, change, evidence,
closure, and necessary dashboard sync. Any failed condition uses the full flow.

If Git is unavailable, continue safe document/assessment work but omit invented
baselines, staging, and commits. An eligible atomic case without required Git
evidence can become `Done with Caveats` only by explicit owner decision.

Run modes are `isolated` (new branch/worktree for large, parallel, or high-risk
work), `branch` (normal development), and `inline` (small existing-branch work).

## Boundaries And Resume

Create a case for persistent workflow, medium/high-risk execution, or required
continuity. Do not create one for explanation/status-only, standalone
review/grill, or clearly one-shot low-risk work.

Across sessions, read `handoff.md` when present, then the minimum current
artifact. Recheck context when authority, implementation, dependencies, risk,
or conflict may have changed. Handoffs older than seven days are stale.
Terminal closures do not resume in the same case by default. A resumable
closure requires current authorization, satisfied resume conditions, and a
later `execution.md` Resume section naming prior status/evidence and reason.

Follow project-local command, package-manager, and editing instructions.
