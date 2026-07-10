---
case_id: 20260710-post-execution-review-atomic-commits
requirements_version: 1
status: approved
risk_level: high
created_at: 2026-07-10T16:07:13+08:00
updated_at: 2026-07-10T16:15:52+08:00
approved_by: user
approved_at: 2026-07-10T16:15:52+08:00
---

# Requirements: Mandatory Post-Execution Review And Atomic Commits

## Confirmation Boundary

This document is a draft requirements object for owner confirmation.

Confirming requirements v1 authorizes creation of an implementation plan only.
It does not authorize implementation, authority changes, staging, committing,
pushing, rebasing, or rewriting Git history. Those actions require the later
approved implementation plan and its current execution authorization.

Preferred confirmation wording:

```text
Approve requirements v1
```

The owner may instead request concrete revisions by section or requirement ID.

Approval record: requirements v1 was approved by the owner on
2026-07-10T16:15:52+08:00 with the response `批准`.

## Decision Summary

Requirements v1 asks the owner to confirm these ten decisions as one object:

1. Mandatory post-execution review applies to every persistent code, test,
   Skill-behavior, workflow-policy, public-contract, or executable-config case.
2. The review has separate Engineering and Spec axes.
3. Axis separation is mandatory; parallel sub-agents are optional.
4. Critical and Important findings block unqualified closure; Minor findings
   are recorded and handled according to residual risk.
5. Requirements, plan, green tasks, verified refactors, material review fixes,
   and closure use semantic atomic commits.
6. A commit maps to an independently valid unit, not mechanically to every
   checkbox or intermediate attempt.
7. Plan approval authorizes case-scoped commits but not push or Git history
   rewriting.
8. A successful closure commit is required before reporting unqualified `Done`.
9. Case commits use the existing title standard plus deterministic case/step
   mapping, preferably commit trailers.
10. No new persistent review phase or mandatory `review.md` artifact is added.

## Context And Decision Status

- Case intent: workflow and skill design.
- Risk: high, because the requirements change workflow policy, review routing,
  closure semantics, Git authorization, and evidence boundaries.
- Workspace observed at drafting: branch `main`, clean worktree, commit
  `c6dfe4612c341d26ba9d125d493781a4c527025f`.
- Context verdict: `needs_user_decision` until this requirements version is
  explicitly confirmed.
- Authority status: pending proposal only. This document does not override
  current authority or current Skill behavior.

Current authority and implementation conflict with the proposed direction in
four material ways:

1. `review` is manual-only and cannot be triggered by the workflow.
2. Missing review conversation is not by itself a closure blocker.
3. `review_risk` is data only and cannot authorize or trigger review.
4. Staging and committing are disabled by default unless the user or plan
   explicitly authorizes them.

The owner has asked to reconsider those rules in favor of mandatory
post-execution review and atomic commits at semantic completion points. The
proposal must still preserve the constitutional minimum path and human-first
principles.

## Problem Statement

The current workflow can finish implementation, record passing tests, map
acceptance criteria, and close a case without an obligatory final inspection of
the accumulated change. This leaves several gaps:

- Tests may encode an incomplete or incorrect interpretation of the approved
  requirement.
- Code may pass tests while containing missed error paths, public-contract
  drift, unnecessary complexity, or unrelated scope.
- The same implementing agent may over-trust its own execution evidence.
- Review depends on the user remembering to request it.
- Optional commits leave no guaranteed, reviewable boundary for the plan,
  completed tasks, corrective fixes, and closure.
- A mixed uncommitted worktree weakens recovery, bisectability, revertability,
  and the precision of the final review target.

The workflow needs a small, deterministic quality loop after execution and a
Git history whose commits correspond to coherent, verified units of work.

## Goals

1. Require a final read-only review after implementation is complete and before
   a persistent code or behavior case can close successfully.
2. Review the final change independently along two axes so code quality cannot
   mask requirement mismatch, and requirement compliance cannot mask code
   defects.
3. Ensure the review covers the entire case delta, including committed,
   staged, unstaged, and untracked case-related changes.
4. Require atomic commits at confirmed semantic completion points: approved
   requirements when present, approved plan, independently complete tasks,
   verified refactors, material review fixes, and closure.
5. Keep every implementation commit coherent, verified, independently
   reviewable, and independently revertible.
6. Make approval of the implementation plan authorize normal case-scoped
   staging and atomic commits without repeated user interruptions.
7. Preserve unrelated user changes and refuse to package them into case
   commits.
8. Record enough commit and review evidence to support resume, audit, review,
   and closure without introducing a new mandatory artifact type.

## Non-Goals

- Do not require post-execution review for explanation-only, status-only, or
  read-only review conversations.
- Do not commit every edit, failed attempt, test Red state, checkbox change, or
  timestamp update.
- Do not equate an implementation-plan task mechanically with one commit when
  the task does not leave the repository in an independently valid state.
- Do not require a new `review.md` artifact or a new persistent case phase only
  to represent review.
- Do not require parallel sub-agents as a portability dependency. Separate
  review contexts are required; parallelism is an optional implementation.
- Do not automatically push, open a PR, merge, amend, rebase, squash, tag, or
  publish commits.
- Do not weaken TDD, acceptance evidence, authority confirmation, plan
  amendment, or public-contract safeguards.
- Do not turn the workflow into a daemon, CLI backend, queue, or heavy
  orchestrator.
- Do not apply authority changes merely because this requirements document is
  confirmed.

## Definitions

### Eligible Case

A persistent Doc Loom case that changes one or more of:

- production code or tests;
- Agent Skill behavior, workflow protocol, or execution semantics;
- a public API, CLI, schema, configuration contract, or migration;
- executable configuration, automation, or user-observable behavior.

Purely mechanical docs changes may use a compact inline quality check. A
one-shot change outside a persistent case is not forced into the full case
commit lifecycle by this requirement.

### Post-Execution Review

A read-only review of the final accumulated case change after planned execution
and verification are complete, but before `doc-sync-close` declares the case
complete.

### Engineering Axis

Determines whether the implementation is technically sound. It includes:

- correctness and plausible regressions;
- edge cases, error paths, and failure handling;
- security, privacy, permissions, and other triggered High-Risk Topics;
- test credibility and relevant failure-path coverage;
- public contract, authority, and ADR conformance;
- repository coding standards and verified tool results;
- unnecessary complexity, coupling, duplication, or speculative behavior.

### Spec Axis

Determines whether the final implementation faithfully satisfies the approved
task object. It compares the final change against:

- Goal and Non-goals;
- confirmed Decisions;
- Acceptance Criteria;
- approved file and responsibility boundaries;
- approved plan amendments;
- directly relevant authority, ADR, and public contracts.

It looks for missing or partial requirements, incorrect behavior, scope creep,
Non-goal violations, and unsupported claims of acceptance.

### Atomic Commit

A commit that:

1. expresses one coherent intent or root cause;
2. contains no unrelated changes;
3. includes the implementation, tests, and necessary docs for that intent;
4. leaves the relevant repository state valid and verified;
5. can be reviewed and reverted independently;
6. follows the repository commit-title standard;
7. can be mapped to the case and semantic completion point.

Atomic means coherent and independently valid, not merely small.

### Semantic Completion Point

A durable workflow boundary at which the work is meaningful and verified:

- requirements approval when a requirements artifact exists;
- plan approval;
- a green implementation task;
- an independently verified behavior-preserving refactor;
- a review-finding fix or group of findings with one root cause;
- case closure.

## Required Workflow

```text
requirements draft
  -> owner confirms requirements
  -> atomic requirements commit
  -> implementation plan draft
  -> owner approves plan
  -> atomic plan commit
  -> execute Task 1 to green
  -> atomic task commit
  -> execute Task N to green
  -> atomic task commit
  -> final verification
  -> Engineering review + Spec review
       -> changes required: fix, verify, atomic fix commit, re-review
       -> insufficient evidence: collect evidence, then re-review
       -> pass: continue
  -> write closure and closed state
  -> atomic closure commit
  -> report Done only after the closure commit succeeds
```

The review axes may run in parallel when independent reviewer contexts are
available and authorized. Otherwise they run as two separate sequential passes.
Their evidence and conclusions must remain distinguishable.

## Functional Requirements

### R1. Mandatory Review Gate

Every eligible case must complete a `Completed`-maturity Standard review after
execution and before successful closure.

The gate is workflow-authorized by the approved plan. It does not require the
user to remember to invoke `review` and does not require another confirmation
round merely to run a read-only review.

### R2. Review Placement

Review starts only after:

- every planned implementation task is complete;
- required tests and planned quality checks have run;
- acceptance criteria have preliminary execution evidence;
- all expected task commits exist;
- the case delta and unrelated workspace changes are explainable.

Review completes before closure writes a final acceptance verdict.

### R3. Deterministic Baseline

The review baseline comes from the approved plan's `base_commit` or an explicit
recorded degraded-mode reason. The workflow must validate that the baseline
resolves before reviewing.

The implementation must distinguish an exact baseline from a merge-base
comparison. It must not call a user-supplied point "fixed" and silently change
it to another merge-base.

### R4. Complete Delta

The review target must cover all case-related changes since the baseline:

- commits reachable from the case execution;
- staged tracked changes;
- unstaged tracked changes;
- untracked case-related files.

An empty target, unresolved baseline, or unexplained change is a preflight
failure, not a passing review.

### R5. Spec Source

The Spec axis resolves its source in this order:

1. current approved `plan.md` version;
2. approved Plan Amendments;
3. confirmed current-task decisions recorded in case evidence;
4. directly relevant authority, ADR, and public contracts;
5. upstream issue or PRD explicitly referenced by the approved plan.

An eligible persistent case with no trustworthy Spec source returns
`Insufficient evidence`; it does not silently skip the Spec axis.

### R6. Standards And Engineering Sources

The Engineering axis reads the smallest relevant set of:

- project and path-specific agent instructions;
- active coding and Git standards;
- related authority, ADRs, and public contracts;
- changed code, adjacent code, and relevant tests;
- lint, typecheck, build, and test configuration;
- current command evidence.

Tool-enforced issues may be omitted from prose only when the relevant tool was
actually run and passed for the reviewed change.

### R7. Axis Isolation

Engineering and Spec reviewers must not use the other axis verdict as evidence.
Each axis produces its own verdict, findings, evidence gaps, and scope.

The implementation may use parallel sub-agents, separate context passes, or an
equivalent isolation mechanism. The core workflow must not depend on a specific
multi-agent runtime.

### R8. Review Findings

Findings use the existing severity model:

- `Critical`
- `Important`
- `Minor`

Every finding must identify a stable location or symbol, observed evidence,
impact, and the correction condition.

### R9. Review Aggregation

The two axes remain visible separately, but the gate produces one deterministic
result:

| Condition | Gate Result |
|---|---|
| Either axis has unresolved Critical or Important findings | `changes_required` |
| Either axis lacks evidence needed to judge material risk | `insufficient_evidence` |
| Both axes have only Minor findings or no findings | `pass` |

Findings may be deduplicated by root cause for handling, but an issue must not
disappear merely because both axes found it. One axis must never compensate for
failure on the other axis.

### R10. Fix And Re-Review Loop

`changes_required` returns ownership to execution for the smallest corrective
change. Every material fix must:

1. address one coherent root cause;
2. add or update regression coverage when appropriate;
3. run the relevant verification;
4. create an atomic fix commit;
5. re-run the affected axis and then the final aggregate gate against the
   accumulated final change.

An unresolved Critical or Important finding blocks unqualified `Done`.

### R11. Review Evidence Storage

Do not create a mandatory `review.md` artifact. Record compact review evidence
in existing artifacts:

- `execution.md`: review target, per-axis verdicts, findings and fixes, commands,
  re-review history, and task/fix commit hashes;
- `closure.md`: final aggregate verdict, unresolved Minor findings or evidence
  caveats, and final commit range summary.

Detailed review output may remain conversational when no future resume or audit
needs it, but the final gate verdict and material findings must be durable case
evidence.

### R12. Requirements Commit

When a case contains an explicitly requested requirements artifact, confirming
that requirements version creates an atomic requirements commit before
implementation planning begins.

The draft must not be committed as an approved requirement before confirmation.

### R13. Plan Commit

Approving the current implementation plan creates an atomic plan commit before
implementation starts. It includes the approved plan, case routing state, and
only directly related case artifacts.

The plan's `base_commit` remains the pre-execution baseline. A document must not
attempt to contain the hash of the commit that contains that same document.

### R14. Task Commits

Each independently complete implementation task creates an atomic commit after
the task is green and its relevant quality checks pass.

The task commit should normally include:

- production or Skill behavior change;
- corresponding tests or alternative verification changes;
- necessary task-local docs;
- execution evidence or plan progress needed to explain that task.

If a planned task cannot leave the repository independently valid, combine it
with the smallest dependent task or revise the plan task boundaries. Do not
create intentionally broken commits merely to preserve checkbox-to-commit
cardinality.

### R15. Refactor Commits

A behavior-preserving refactor should be separate from a behavior change when
it can remain independently verified. The refactor commit must retain passing
characterization or regression evidence.

### R16. Review-Fix Commits

Every material review fix creates an atomic commit after verification. Commit
boundaries follow root causes, not finding count: multiple findings caused by
one defect may share one fix commit.

Do not amend a prior task commit merely to hide review history unless the user
or approved plan explicitly authorizes history rewriting.

### R17. Closure Commit

Closure writes `closure.md`, updates `case_state.yaml` to the final state, and
creates one atomic closure commit. The workflow reports `Done` only after that
commit succeeds and the case-related worktree is clean.

The closure artifact cannot contain its own commit hash. Closure commit identity
must be discovered from Git metadata, paths, or commit trailers. Resume logic
must distinguish a merely present uncommitted `closure.md` from a successfully
committed closure.

### R18. Commit Authorization

Approval of the implementation plan authorizes normal staging and committing
for the approved case scope and commit strategy. The agent does not ask for
confirmation before every planned task or review-fix commit.

The authorization does not cover:

- unrelated files or another case;
- push, PR, merge, tag, release, rebase, squash, or amend;
- material plan deviations;
- unapproved dependency, lockfile, CI, schema, config-contract, or authority
  changes.

### R19. Commit Traceability

Commit titles continue to follow `<type>: <summary>`. Each case commit must also
be deterministically mappable to the case and completion point, preferably with
commit trailers:

```text
Doc-Loom-Case: 20260710-post-execution-review-atomic-commits
Doc-Loom-Step: requirements | plan | task:<id> | refactor:<id> | review-fix:<id> | closure
```

Actual task, refactor, and review-fix commit hashes are recorded in
`execution.md`. The final response may report the closure commit hash; the
closure document does not write a self-referential hash.

### R20. Unrelated And Mixed Changes

Before every commit, inspect staged content and confirm it is case-scoped.
Preserve unrelated user changes.

If a file mixes case changes with unrelated changes and cannot be isolated
safely, do not commit the mixed file. Prefer an isolated branch or worktree for
persistent cases. Record and surface the blocker rather than claiming an atomic
commit.

### R21. Commit Health

Every task, refactor, fix, and closure commit must leave the relevant repository
checks passing. TDD Red evidence belongs in `execution.md`; an intentionally
failing Red state is not a normal atomic commit boundary.

### R22. History Stability

Once execution evidence records commit hashes, do not rewrite those commits by
default. If the user authorizes amend, rebase, or squash, refresh affected
evidence and re-run the final review against the rewritten range.

### R23. Fast-Path

Fast-Path remains small but does not bypass quality outcomes:

- one compact implementation commit may contain the complete behavior, test,
  and necessary docs change;
- run a compact Engineering and Spec review against the final change;
- create the closure commit;
- do not manufacture separate commits for empty or purely mechanical states.

### R24. Degraded Git Mode

If Git is unavailable, the baseline is unresolved, or a required commit cannot
be created safely, an eligible case cannot receive unqualified `Done` under
this policy.

The case may pause or block. `Done with Caveats` requires an explicit owner
decision accepting the missing Git evidence; it must not be inferred by the
agent.

## Commit Boundaries

| Completion Point | Commit Required | Normal Contents | Must Not Include |
|---|---:|---|---|
| Requirements approval | Yes, when requirements artifact exists | Approved requirements and routing state | Implementation or authority patches |
| Plan approval | Yes | Approved plan and directly related state | Production changes |
| Green task | Yes | One coherent behavior plus tests and necessary docs | Unrelated tasks or failing intermediate work |
| Verified refactor | Yes, when independently meaningful | Behavior-preserving change and verification | New behavior hidden inside refactor |
| Review fix | Yes | One root-cause correction and regression evidence | Unrelated cleanup |
| Closure | Yes | Closure, closed state, justified derived sync | New implementation fixes |
| Checkbox or timestamp only | No standalone commit | Fold into nearest semantic commit | Noise-only history |

## Closure Semantics

An eligible case may be unqualified `Done` only when all are true:

1. acceptance criteria are met;
2. required execution evidence exists;
3. required plan, task, refactor, and review-fix commits exist;
4. Engineering review passes;
5. Spec review passes;
6. no unresolved Critical or Important findings remain;
7. the final closure commit succeeds;
8. no unexplained case-related working-tree changes remain.

Unresolved Minor findings do not automatically block `Done`, but they must be
recorded. A material residual risk may still require `Done with Caveats`.

## Acceptance Criteria

- [ ] Every eligible case automatically performs a post-execution review before
  closure without requiring a separate user reminder.
- [ ] The review reports Engineering and Spec verdicts separately.
- [ ] Engineering review covers correctness, errors, tests, contracts,
  standards, triggered high-risk concerns, and unnecessary complexity.
- [ ] Spec review checks Goal, Non-goals, Decisions, Acceptance Criteria, plan
  amendments, missing behavior, incorrect behavior, and scope creep.
- [ ] Review preflight validates the exact baseline and includes committed,
  staged, unstaged, and untracked case changes.
- [ ] Missing Spec or material evidence results in `insufficient_evidence`, not
  a pass or silent skip.
- [ ] Critical or Important findings return to execution, produce verified
  atomic fix commits, and trigger re-review.
- [ ] Requirements approval, plan approval, green tasks, verified refactors,
  material review fixes, and closure follow the defined commit policy.
- [ ] No commit includes unrelated user or other-case changes.
- [ ] TDD Red states and failed attempts are not committed as normal task
  completion points.
- [ ] Every task/fix commit leaves its relevant checks passing and is
  independently revertible.
- [ ] Plan approval authorizes scoped task and fix commits without repeated
  confirmation, but not push, history rewriting, or material deviations.
- [ ] Execution evidence maps semantic steps to actual commit hashes without
  requiring self-referential document hashes.
- [ ] Closure distinguishes an uncommitted closure file from a successful
  closure commit and does not report `Done` before commit success.
- [ ] Fast-Path retains compact review and commit evidence without manufacturing
  empty stages.
- [ ] Git-degraded or unsafe mixed-worktree conditions cannot silently receive
  unqualified `Done`.
- [ ] No new mandatory review artifact, daemon, CLI backend, or heavy
  orchestration layer is introduced.
- [ ] Current authority, implementation Skills, templates, README guidance, and
  any semantic guard checks are synchronized only after an approved
  implementation plan executes successfully.

## Expected Authority And Implementation Impact

If requirements v1 is confirmed and a later implementation plan is approved,
the plan is expected to evaluate changes to at least:

- `docs/authority/workflow/development-flow.md`
- `docs/authority/operations/git.md`
- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/assessment/review/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- related templates, references, semantic checks, and derived README guidance

No constitution amendment is currently proposed. The implementation design must
show that review and commit gates solve concrete traceability and quality
failures through the smallest path, rather than adding ceremonial stages.

## Risks And Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Review becomes ritual | False confidence and extra cost | Evidence-based findings, deterministic axes, fail on missing evidence |
| Standards axis misses real bugs | Passing style review with defective code | Use broad Engineering axis rather than style-only Standards |
| Commit-per-checkbox noise | Fragmented or broken history | Commit only independently valid semantic units |
| Same-agent confirmation bias | Defects repeated across implementation and review | Separate axis contexts; prefer independent reviewer for high risk when available |
| Mixed worktree contaminates commits | User changes are accidentally committed | Isolated mode or case-scoped staging with pre-commit inspection |
| Closure claims success before Git succeeds | State and history disagree | Require successful closure commit before reporting Done |
| Commit hashes become stale after rebase | Evidence points to missing commits | No history rewrite by default; refresh evidence and re-review when authorized |
| Workflow becomes too heavy | Violates minimum-path constitution | Compact Fast-Path; no new phase or artifact; no repeated user confirmation |

## Context Sources

| Source | Layer | Why Included | Trust |
|---|---|---|---|
| `docs/authority/constitution.md` | Constitutional authority | Minimum path, no complex pipeline, human-first constraints | Highest |
| `docs/authority/workflow/development-flow.md` | Active workflow authority | Current manual review and confirmation behavior | High |
| `docs/authority/agent/policy.md` | Active agent authority | High-risk workflow-policy classification and human-first responsibility | High |
| `docs/authority/operations/git.md` | Active operations authority | Current commit-title contract | High |
| `skills/_shared/references/shared-protocol.md` | Current implementation protocol | Artifact, closure, routing, Git degraded-mode, and manual review rules | High |
| `skills/development/plan-confirm/SKILL.md` | Current Skill implementation | Approved plan, risk, acceptance, and base-commit contract | High |
| `skills/development/tdd-execute/SKILL.md` | Current Skill implementation | Execution evidence, review risk, and optional commit behavior | High |
| `skills/assessment/review/SKILL.md` | Current Skill implementation | Completed review scope, findings, severity, and read-only boundary | High |
| `skills/development/doc-sync-close/SKILL.md` | Current Skill implementation | Acceptance and closure gate behavior | High |
| Owner discussion, 2026-07-10 | Pending owner fact | Mandatory post-execution review and semantic atomic commit direction | Pending confirmation |
| User-provided two-axis code-review Skill | External design evidence | Two-axis separation, fixed baseline, Spec resolution, and independent contexts | Reference only |

## Excluded Sources

| Source | Reason |
|---|---|
| Archived historical designs | Current authority and Skills are sufficient; archive cannot override them |
| External issue tracker integration | Doc Loom has a repo-native approved plan and no confirmed issue-tracker dependency |
| Runtime code and tests | This repository's executable surface is the Skill and Markdown workflow; implementation planning has not started |
