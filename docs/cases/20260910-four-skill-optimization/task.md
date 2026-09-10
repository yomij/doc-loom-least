# Four-Skill behavior optimization

## Goal

Implement the user's Skill 系列优化执行计划 for the four installed Skills and
their local resources. Keep independent capabilities, self-contained runtime
rules, and direct development execution. README files are maintenance views,
not runtime dependencies or part of runtime cost measurements.

## Success conditions

- Four descriptions discriminate execution, read-only review, explicit
  assumption challenge, and structural documentation governance.
- Review defines three result states, severity, bounded findings, and no
  automatic subagent requirement.
- Governance separates target from evidence scope, defines authority and
  conflict decisions locally, and loads supporting detail conditionally.
- Existing authorization survives resume; only unresolved outcome or permission
  decisions interrupt work. Ordinary development requires no case or Review.
- All eight requested scenarios receive a selection and behavior walkthrough;
  cross-Skill review checks boundaries, authorization, completion, and cost.
- Validate all four Skills and local resource links; synchronize conflicting
  repository authority without changing the constitution or adding a Skill.

## Constraints and decisions

- Baseline: `3a19995f9a77ce7ab3e8e24f84c5a5059a17640f`.
- The pasted user plan authorizes these behavior changes. Necessary authority
  synchronization records that decision; no repeated approval is needed.
- Use pnpm for frontend and uv for Python dependencies. No new runtime or
  shared protocol. No subagents, installation sync, or publication is needed.
- Pre-existing untracked `.DS_Store` and `skills.zip` are outside this change.
- Preserve existing active constitution, optional single task record, legacy
  evidence, and executor ownership of fixes and durable status.

## Current state

done — four Skills and their resources updated; structural checks, eight
scenario walkthroughs, and a separate read-only consistency pass completed.
The following responsibility model was recorded before editing runtime rules.

## Current responsibility model

### docloom-workflow

| Item | Current behavior |
|---|---|
| Primary responsibility | Execute a development request through verified delivery. |
| Trigger | Normal development, status, continuation, or discovery. |
| Negative trigger | Standalone review, explicit challenge, and structural governance are helpers, but description does not exclude them. |
| Inputs | User outcome, repository instructions, authority, worktree, implementation; task record on resume. |
| Allowed actions | Authorized implementation, verification, affected documentation, optional task record. |
| Forbidden actions | Unauthorized changed outcome or irreversible action; claiming unverified completion; silent authority promotion. |
| User confirmation gates | Unestablished material facts, changed outcomes, consequential choices; resume says to reconfirm intent. |
| Completion condition | Every success condition has credible verification evidence. |
| Escalation | Review uses broad risk terms; governance for authority decisions; grill on explicit request. |
| Optional resources | Task template when durability or continuation requires it. |

### review

| Item | Current behavior |
|---|---|
| Primary responsibility | Verify an established target and return findings without changing it. |
| Trigger | Explicit review or independent check within authorized work. |
| Negative trigger | Fixes and durable records belong to executor. |
| Inputs | Target; goal, constraints, baseline, relevant delta for post-execution or high-consequence work. |
| Allowed actions | Read and assess correctness, evidence, scope, removable complexity. |
| Forbidden actions | File or task-state changes; subagents unless requested by user. |
| User confirmation gates | Missing-target behavior is implicit in insufficient_evidence. |
| Completion condition | Findings and a result, but severity and result precedence lack definitions. |
| Escalation | Executor owns corrections; transition after a fix request is implicit. |
| Optional resources | Complexity reference only on explicit focused review; it still names retired review modes. |

### grill

| Item | Current behavior |
|---|---|
| Primary responsibility | Challenge a claim through dialogue. |
| Trigger | Explicit request to challenge a claim or assumptions. |
| Negative trigger | Ordinary execution is excluded by intent, but body does not name the boundary. |
| Inputs | Claim, boundaries, user answers, discoverable facts. |
| Allowed actions | Questions, fact verification, options, conversational summary. |
| Forbidden actions | Files, workflow state, converting short agreement into durable authority. |
| User confirmation gates | One consequential question at a time, even for independent questions. |
| Completion condition | User ends discussion or important decisions converge; convergence is undefined. |
| Escalation | Workflow owner records decisions when needed. |
| Optional resources | None. |

### setup-doc-governance

| Item | Current behavior |
|---|---|
| Primary responsibility | Establish or repair documentation authority and structure. |
| Trigger | Requested governance work or workflow authority escalation. |
| Negative trigger | Code changes forbidden; ordinary document-edit exclusion is missing. |
| Inputs | Constitution, authority, sources, entries, optional code/tests. |
| Allowed actions | Documentation changes, fact/file routing, indices, authority metadata. |
| Forbidden actions | Code changes, unconfirmed structural changes, rewriting old batches. |
| User confirmation gates | All structural or material decisions; changed plans require renewed approval without checking existing authorization. |
| Completion condition | Applied decisions and index refresh are implied; no explicit end condition. |
| Escalation | Owner decision for blocked conflicts; no explicit route for implementation fixes. |
| Optional resources | Governance rules always loaded; detailed plan template conditional. |

The primary responsibilities do not overlap. Boundary defects are in broad
triggers and duplicated helper instructions, so no capability split is needed.

## Next action

None required for this source optimization. Real model trials remain a separate
way to measure realized behavior and costs; no runtime success rate is claimed.

## Verification evidence

### Structural checks

- Ran the bundled skill-creator `quick_validate.py` validator against all four
  final Skill directories via uv with PyYAML: all passed.
- Checked every runtime Markdown resource link: each resolves inside its owning
  Skill; all four local resources have explicit conditional loading triggers.
- Confirmed exactly four discoverable SKILL.md files. No shared protocol,
  runtime backend, extra Skill, or cross-Skill filesystem dependency was added.
- Parsed the governance template's YAML: separate target_scope/evidence_scope
  fields replace scope. Frontmatter now begins at the first line.
- Checked changed authority metadata and `git diff --check`: passed.
- Read the final runtime delta and affected current authority/derived views
  against the baseline. Historical ADRs, cases, and constitution are unchanged.

### Requested scenario walkthroughs

These are manual simulations of selection from the four descriptions followed
by execution from the selected Skill, without relying on repository README
content. They are not live model runs or observed tool-use benchmarks.

| Case / request | Selection | Simulated behavior and result |
|---|---|---|
| 1. Fix the login button alignment issue. | docloom-workflow | Inspect UI, make the localized fix, verify alignment, finish. No case, Review, grill, governance, or phase selection by default; a login-page location alone is not an authentication-behavior change. |
| 2. Add organization-level role based access control. | docloom-workflow | Discover existing role/tenant rules, ask only for missing policy decisions affecting behavior, implement and verify allowed/denied access. Permission changes trigger the separate Review pass; no automatic subagent or phase question. Persistence depends on actual continuity or decisions. |
| 3. Review this implementation and tell me whether it is ready to merge. | review | Recover target and baseline, inspect the relevant delta and evidence without writes, return findings and exactly one result. No subagent or merge action is implied. |
| 4. Fix the issues you found. | docloom-workflow | The new request authorizes execution of the identified fixes. Leave Review's read-only context, implement and verify corrections; repeat only affected or unresolved checks. |
| 5. Replace PostgreSQL with MongoDB; challenge this decision. | grill | Establish workload and migration assumptions. Group independent questions about requirements; ask sequentially where answers determine follow-ups. Verify only facts changing the recommendation; no implementation, file edits, or tangential research. |
| 6. Update the API documentation for this endpoint. | docloom-workflow | Read endpoint behavior/contract, edit the documentation, verify examples and links. Documenting an existing contract does not itself change the public API or restructure authority, so no governance or automatic Review. |
| 7. Set up authoritative-document and knowledge-promotion rules. | setup-doc-governance | Target repository documentation, start with docs-only evidence, inspect existing ownership and prepare concrete rules. Apply existing approved decisions; unspecified binding-rule choices require owner approval after preparation. Only placement/migration or multi-decision detail loads resources. |
| 8. Continue the task from yesterday. | docloom-workflow | Read the durable record, latest instruction, and changed evidence. Resume the established outcome and authorization without reconfirmation or template reload. Ask only for an outcome-changing conflict, stale context, or new authorization. |

### Boundary walkthroughs

| Input / condition | Required result |
|---|---|
| Important finding plus unavailable verification evidence | changes_required; also list the gap. Known defects are not hidden by insufficient_evidence. |
| No blocking finding, but target or conclusion-critical evidence is missing | insufficient_evidence, never pass. |
| Actionable Minor findings with evidence supporting the scoped conclusion | pass with Minor findings. |
| Complexity-only Review finds nothing removable | A scoped pass, without asserting overall merge readiness; no retired Dual-pass/Engineering modes. |
| current-case target with repo-readonly evidence | Read code/tests/configuration as evidence; write only case documentation. Repository authority promotion requires separate target authorization. |
| User explicitly restricts evidence to docs-only | Do not read code merely because it is useful; request expanded access only if necessary. |
| Owner already approved an exact governance change | Apply within that approval and verify; do not ask again because a plan exists or a task resumed. |
| Approved plan is revised without changing scope, binding rules, permissions, or irreversible effects | Update the current record without a fresh approval gate. |
| Vague destructive request with no identified deletion target | Make target and effects concrete, obtain the missing authorization, continue independent work. |
| Active-looking derived page conflicts with owner-designated rules | Follow declared ownership/precedence; an active marker is not independent authority. |
| Legacy document has no status but is explicitly designated by the repository owner | Inspect ownership and source evidence; do not discard it solely for missing metadata. |
| Declared precedence and existing owner decisions cannot settle a binding-rule conflict | Block the dependent decision, explain the specific missing owner decision, continue independent work. |

### Cross-Skill Review

- Scope: four runtime Skills, their four resources, and synchronization of
  conflicting current documentation; baseline is the commit named above.
- Routing: the four canonical requests map uniquely to workflow, review, grill,
  and governance. Ordinary documentation and subsequent fixes map to workflow.
- Ownership: workflow executes and verifies; review reports without mutations;
  grill stays conversational; governance owns structural documentation changes.
- Escalation: concrete triggers or explicit user intent select a capability;
  there is no fixed sequence or implicit subagent requirement.
- Authorization: existing approval persists; missing destructive targets and
  unspecified binding-rule changes retain concrete confirmation boundaries.
- Completion: workflow requires evidence and no blocking required-review issues;
  review returns a defined result; grill stops on user exit or resolved/explicitly
  deferred challenged assumptions; governance distinguishes applied from blocked.
- Findings: none remaining within this document/protocol review scope.
- Result: pass. Evidence limitation: manual scenario simulations and structural
  validation do not establish model adherence or a runtime reliability rate.

### Resource loading and cost

| Resource | Previous loading | Final category and trigger |
|---|---|---|
| workflow/templates/task.md | Conditional, with resume/update wording encouraging reload | **conditional** — create or restructure a durable record; no reload on normal update/resume. |
| review/references/complexity-only.md | Conditional | **conditional** — explicit complexity-only assessment. |
| governance/references/governance-rules.md | Always | **conditional** — new authority placement, entries, bridges, or archives. |
| governance/templates/governance-plan.md | Conditional, vague detail threshold | **conditional** — multiple decisions needing a table. |

No auxiliary resource is always loaded, and no current resource needs a separate
rare category. SKILL.md itself is loaded upon activation. No resource depends on
README or another Skill's filesystem paths.

Token counts use tiktoken o200k_base on full file contents, including frontmatter.
They are reproducible estimates, not model-specific billing. README, authority,
case documentation, and environment context are excluded from runtime totals.

| Runtime file | Baseline tokens | Final tokens |
|---|---:|---:|
| docloom-workflow/SKILL.md | 865 | 755 |
| docloom-workflow/templates/task.md | 150 | 149 |
| review/SKILL.md | 246 | 588 |
| review/references/complexity-only.md | 144 | 139 |
| grill/SKILL.md | 140 | 199 |
| setup-doc-governance/SKILL.md | 399 | 837 |
| setup-doc-governance/references/governance-rules.md | 1,038 | 284 |
| setup-doc-governance/templates/governance-plan.md | 160 | 191 |
| **Total** | **3,142** | **3,142** |

- Metadata: descriptions total 95 → 142 tokens, trading 47 tokens for explicit
  selection boundaries. These are already included in the full-file totals.
- Prompt: four entrypoints total 1,650 → 2,379 tokens; auxiliary resources fall
  1,492 → 763. Core safety/result rules move into self-contained entrypoints.
- Ordinary workflow loads 865 → 755 tokens, before common catalog/environment
  context; no supporting file is required. Ordinary governance loads
  SKILL.md plus formerly mandatory reference, 1,437 → 837 tokens.
- Standalone Review grows 246 → 588 tokens to define severities, precedence,
  evidence gaps, and read-only verification. Grill grows 140 → 199 tokens for
  explicit exclusions, bounded fact-checking, and a stopping rule.
- Review/agent cost: explicit observable triggers replace vague risk escalation;
  local fixes need no standalone Review and no capability creates a subagent by
  default. Rechecks follow changes/failures/gaps rather than a repeated loop.
- Human cost: reuse existing authorization on resume and plan updates; batch
  independent grill questions; prepare concrete decisions before needed approval.
- Persistence cost: no ordinary one-turn task record, no read-only governance
  advice file, and no duplicated Review state or default plan artifact.

## Result and residuals

### Changed files and purpose

| Skill / file | Final change and problem addressed |
|---|---|
| [docloom-workflow](../../../skills/development/docloom-workflow/SKILL.md) | Default execution routing, precise Review triggers, approval reuse, bounded verification, and optional persistence replace broad escalation and resume confirmation. |
| [review](../../../skills/assessment/review/SKILL.md) | Explicit read-only protocol, severity definitions, ordered three-state results, bounded findings, and executor handoff remove ambiguous readiness and agent requirements. |
| [grill](../../../skills/assessment/grill/SKILL.md) | Explicit challenge routing, dependency-aware question grouping, relevant fact checks, and a defined stopping rule reduce interaction/research cost. |
| [setup-doc-governance](../../../skills/governance/setup-doc-governance/SKILL.md) | Independent target/evidence scopes, local authority decisions, conditional resources, and reuse of concrete approval remove hidden protocol and blanket gates. |
| [Task template](../../../skills/development/docloom-workflow/templates/task.md) | Align record triggers and avoid reload on routine updates. |
| [Complexity reference](../../../skills/assessment/review/references/complexity-only.md) | Remove retired modes and reuse Review's current result/severity protocol. |
| [Governance reference](../../../skills/governance/setup-doc-governance/references/governance-rules.md) | Retain only placement/migration detail; remove the nonexistent shared authority-order reference. |
| [Governance template](../../../skills/governance/setup-doc-governance/templates/governance-plan.md) | Separate scope fields, combine fact/file decisions, bind approval to covered effects, and put YAML frontmatter first. |

Current authority synchronization is a merge of the user's explicit plan into
development-flow.md, doc-governance.md, agent/policy.md, repo-and-skills.md, and
distribution.md. Existing locations, ownership, and precedence stay intact.
README views, product/current-state.md, and share/workflow-and-design.md now
describe those rules consistently. No authority was created, archived, or moved.

### Removed or consolidated rules

- Removed resume's unconditional reconfirmation; intent recovery and new-action
  authorization remain explicit.
- Replaced consequential/high-risk/material-trigger wording with concrete
  outcome, authorization, security, contract, permission, migration, and shared
  infrastructure conditions. Verification for every success condition remains.
- Removed workflow's duplicated grill dialogue instructions; explicit challenge
  routing remains there, while dialogue rules live in grill.
- Replaced one-question-always with answer-dependent sequencing or small
  independent groups; relevant fact verification remains.
- Replaced all-current-findings pressure with all observed Critical/Important
  findings and actionable non-duplicate Minor findings. No blocking severity
  category was removed.
- Removed Dual-pass/Post-execution/Engineering mode references and the undefined
  shared fact-authority order. The live Skill now defines the applicable rules.
- Split the mixed docs-only/current-case/full-repo scope model into target and
  evidence axes; documentation-only write limits remain explicit.
- Moved governance's core authority/layer/verdict rules into its entrypoint;
  detailed placement stays conditional. Removed duplicate layer labels and
  forced subdirectory/entry scaffolding; no new state matrix was added.
- Replaced missing-active-status rejection with ownership/source inspection.
  History, derived views, and scratch still cannot silently become authority.
- Replaced blanket plan reconfirmation with approval for changed effects outside
  prior authorization. Constitution creation/amendment/migration still needs an
  explicit covering owner decision and coordinated index/SSOT/log updates.
- Combined governance fact/file decision tables; source evidence, target,
  blocked decisions, approval, and applied verification remain captured.

### Remaining ambiguities and limits

No unresolved protocol decision blocks this implementation. Severity still
requires domain evidence: whether an exposure is serious or a maintenance burden
blocks delivery cannot be fixed by a universal numeric threshold. Review must
state concrete evidence and impact rather than infer severity from labels.

Repository-specific fact ownership must be discovered; the Skill provides a
fallback for undeclared ownership and blocks unresolved binding-rule conflicts.
The concrete shared-infrastructure trigger is deployment/storage/build behavior
used by multiple applications; other security, contract, permission, or migration
triggers remain independent. These choices are explicit, not hidden assumptions.

Real model execution, routing frequency, interaction counts, and cost savings
were not benchmarked. As required by ADR-0004, those need representative model
runs before any further removal of Review; this change retains Review and makes
its activation conditions explicit.
