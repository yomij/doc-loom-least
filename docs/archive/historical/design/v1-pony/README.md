---
title: Doc Loom Least v1 Pony Plan
type: design
status: archived
created: 2026-06-26
updated: 2026-06-26
tags:
  - doc-loom
  - skill-design
  - minimum-path
  - overengineering-control
---

# Doc Loom Least v1 Pony Plan

## 1. Purpose

This plan folds a "minimum path" discipline into the Doc Loom Least skill
series.

The goal is not to create a new skill, new stage, or new governance artifact.
The goal is to make every existing Doc Loom skill resist over-design,
redundant complexity, speculative workflow, unnecessary artifacts, and
ceremony that does not solve a real governance failure.

This plan uses `ponytail` as the reference style:

- Question whether the task needs a workflow at all.
- Prefer deletion, reuse, and native/simple mechanisms.
- Add the smallest thing that resolves the actual failure.
- Avoid scaffolding for future possibilities.
- Keep checks and docs proportional to risk.

In Doc Loom terms, this becomes:

> Enter through the smallest path that reaches the root issue, and do not make
> Doc Loom larger than the change it governs.

This is already constitutional. The implementation work is to make the skills
apply that principle consistently.

## 2. Authority Basis

The top authority is `docs/adr/ADR-0000-constitution.md`.

Relevant constitutional requirements:

- "Enter Through The Minimum Path."
- "Do Not Become A Complex Pipeline Product."
- "Do not add workflow stages only to make reasoning feel safer."
- "Do not use a complex pipeline to solve a narrow governance failure."
- "Do not make Doc Loom larger than the change it governs."

Therefore this plan must not add:

- A new `pony` skill.
- A new mandatory review gate.
- A new artifact type.
- A new state machine.
- A new score, maturity model, or complexity metric.
- A new governance layer.

The correct implementation is a narrow update to existing skill instructions
and shared vocabulary.

## 3. Design Principle

Add one shared discipline:

> Before any skill expands scope, creates an artifact, reads more context,
> adds a process step, proposes authority, or touches more files, it must
> justify that the extra complexity solves the current task's concrete
> governance failure.

If two paths both work, choose the path with fewer:

- Files changed.
- Artifacts created.
- Workflow stages entered.
- Long-term facts asserted.
- Authority documents touched.
- State fields written.
- New concepts introduced.
- Dependencies, scripts, or commands added.

This is not permission to skip understanding. The skill should still read the
minimum evidence needed to make a correct decision.

## 4. Non-goals

This plan does not:

- Remove TDD discipline for behavior changes.
- Remove input validation, safety checks, authority conflict handling, or
  explicit confirmation for high-risk work.
- Replace standard review.
- Turn complexity review into an automatic gate.
- Make agents ignore project instructions, package-manager rules, or dirty
  worktree safety.
- Change the constitution.
- Redesign Doc Loom as a CLI or backend product.

## 5. Shared Vocabulary

Use this vocabulary across skills when discussing unnecessary complexity.

| Term | Meaning | Preferred correction |
|---|---|---|
| `delete` | Dead code, unused flexibility, duplicate docs, duplicate workflow, or unused compatibility path. | Remove it. |
| `reuse` | Existing helper, artifact, doc section, pattern, or workflow already covers the need. | Use the existing thing. |
| `native` | Platform, language, Markdown, git, or filesystem already provides the mechanism. | Use the native mechanism. |
| `yagni` | Future extension, config, abstraction, state, or artifact with no current requirement. | Do not add it. |
| `shrink` | Same result can be achieved with fewer lines, sections, files, or branches. | Collapse it. |
| `governance-bloat` | Doc Loom-specific process, status, authority layer, artifact, or route that does not solve a real governance failure. | Keep the current workflow smaller. |

These terms are already aligned with `review/references/complexity-only.md`.
The shared protocol should reference them, but `review` remains the only skill
that emits formal complexity findings.

## 6. Minimum Path Ladder

Add this ladder to the shared protocol and let every skill apply it before
expanding work:

1. **Does this need Doc Loom at all?**
   - Explanation-only, one-shot local edits, simple status checks, and explicit
     review/grill conversations should not create case docs.

2. **Does this need a persistent case?**
   - Create a case only when persistence, risk, cross-session continuity,
     authority conflict, or user request requires it.

3. **Does this need a separate artifact?**
   - Inline context, closure evidence, or route output is enough unless the
     Artifact Policy condition is actually met.

4. **Does this need more context?**
   - Stop reading when the sources are sufficient to route or plan correctly.
     Do not scan the repo to feel safer.

5. **Does this need a new authority fact?**
   - Prefer case-local evidence or a closure proposal. Apply authority patches
     only when the narrow-patch conditions and user confirmation are satisfied.

6. **Does this need a new stage, state field, or workflow rule?**
   - If the only reason is confidence, traceability, or future optionality,
     do not add it.

7. **Does this need new code, dependency, script, or config?**
   - Reuse existing code and commands first. Use native or standard tools
     before adding new machinery.

8. **Does this need a broader test strategy?**
   - Use the smallest check that can fail for the behavior. Do not invent
     large suites or brittle mocks for small changes.

9. **Does this need long prose?**
   - Write enough for the next owner to act. Avoid repeating facts already
     held by `plan.md`, `execution.md`, or `closure.md`.

Stop at the first rung that safely resolves the task.

## 7. Cross-skill Rule

Add this rule to `skills/_shared/references/shared-protocol.md`.

```md
## Minimum Path Discipline

Use the smallest Doc Loom path that resolves the current governance failure.

Before expanding scope, creating an artifact, reading more sources, proposing
authority, adding workflow state, or changing files outside the immediate
target, check whether the extra work is required by current evidence,
Artifact Policy, user instruction, or active authority.

Prefer:

- Inline output over persisted artifact.
- Existing artifact over new artifact.
- Existing authority doc over new authority doc.
- Case-local evidence over long-term authority.
- Proposal over authority patch when confirmation is missing.
- Existing commands over new scripts.
- Existing state fields over new state fields.
- Small verification over broad test expansion.

Do not add process only to make reasoning feel safer. Do not create future
scaffolding, placeholder docs, empty authority sections, duplicate workflow
states, or speculative abstractions.

This discipline does not weaken safety, security, input validation, TDD for
behavior changes, authority conflict handling, or explicit confirmation for
high-risk work.
```

This is the only shared protocol addition needed.

## 8. Skill-by-skill Changes

### 8.1 `docloom-workflow`

Role: public entry and thin router.

Problem to prevent:

- Starting a persistent workflow for simple tasks.
- Creating case docs just because Doc Loom was triggered.
- Routing through more stages than needed.

Change:

- Add a "Minimum Path Routing" subsection before the route table.
- Require the router to ask whether Doc Loom is needed at all.
- Prefer status-only or direct skill handling for small, conversation-only, or
  explicitly manual requests.
- Preserve explicit skill invocation.

Recommended instruction:

```md
## Minimum Path Routing

Before creating case docs or routing into a stage, apply Minimum Path
Discipline from `shared-protocol.md`.

Do not enter persistent workflow for explanation-only, status-only, explicit
review/grill, or clearly one-shot low-risk local work. Do not create case docs
just because this entry skill ran.

When a low-risk task can be handled by fast-path, use fast-path instead of the
full pipeline. When no persistent evidence is needed, keep the answer inline.
```

Route table adjustment:

- Keep current route table.
- Make row 7 fast-path more prominent.
- Add a gate that a persistent case must have a reason: user requested it,
  medium/high risk, authority conflict, cross-session continuity, or required
  persisted evidence.

Do not add:

- New phases.
- New route verdicts.
- A complexity score.

### 8.2 `context-authority`

Role: read minimal context and resolve authority before planning.

Problem to prevent:

- Reading too many docs.
- Treating archive or derived material as necessary context.
- Persisting a context brief when inline context is enough.

Change:

- Add a "Stop Rule" under Workflow.
- Clarify that sufficient context beats complete context.
- Persist `context-authority-brief.md` only when existing conditions hold.

Recommended instruction:

```md
## Stop Rule

Stop reading once the evidence is enough to produce a route verdict with
identified sources and meaningful exclusions. Do not keep scanning to make the
context feel complete.

Read code/tests only when the case intent requires them. Read archive,
scratch, superseded, or derived docs only when history or conflict resolution
requires them.
```

Do not add:

- A source coverage percentage.
- A complete repository inventory.
- A new context digest artifact.

### 8.3 `plan-confirm`

Role: create and confirm an executable plan.

Problem to prevent:

- Plans that include speculative abstractions.
- Plans with broad file scopes.
- Plans that create test or documentation work unrelated to acceptance.
- Plans that add artifacts or sections just because templates allow them.

Change:

- Add a "Minimal Plan Discipline" subsection.
- Require every task to map to acceptance criteria.
- Require `Non-goals` to block speculative work.
- Keep optional sections optional.
- For fast-path plans, keep the plan short.

Recommended instruction:

```md
## Minimal Plan Discipline

Write the smallest executable plan that satisfies the current request and
acceptance criteria.

Every task must map to a file, command, expected result, or acceptance
criterion. Do not include speculative abstractions, optional cleanup, broad
refactors, dependency changes, new config, new workflow state, or authority
updates unless they are required by the goal or active authority.

Use `Non-goals` to exclude tempting adjacent work. Keep optional sections out
unless they contain real plan content.
```

Template adjustment:

In `skills/development/plan-confirm/templates/plan.md`, add a short prompt under
`## Non-goals`:

```md
- No speculative abstractions, extra artifacts, new workflow stages, or
  unrelated cleanup.
```

This is a prompt, not a required line. It may be removed or tailored per case.

Do not add:

- A mandatory "complexity analysis" section.
- A separate simplification approval.
- A new plan status.

### 8.4 `tdd-execute`

Role: execute an approved plan with TDD discipline.

Problem to prevent:

- Minimal-looking implementation in the wrong place.
- New helpers, dependencies, or files when existing code suffices.
- Unplanned refactors.
- Broad verification beyond the plan.

Change:

- Add an "Implementation Ladder" before Red/Green/Refactor details.
- Make root-cause fixes the minimal fix.
- Limit refactors and verification to planned or necessary work.

Recommended instruction:

```md
## Implementation Ladder

After reading the plan and relevant code/tests, choose the first working rung:

1. Delete or narrow existing behavior if that solves the issue.
2. Reuse an existing helper, pattern, fixture, command, or artifact.
3. Use language standard library or native platform behavior.
4. Use an already-installed dependency only when it is clearly the smallest
   correct option.
5. Add the minimum code needed to satisfy the failing test and acceptance
   criteria.

For bugs, fix the shared root cause when callers route through one place. A
tiny patch in the wrong caller is not the minimum path if siblings remain
broken.

Do not add dependencies, config, scripts, lockfile changes, CI changes, public
contract changes, or authority docs unless the approved plan or current user
instruction authorizes them.
```

Verification adjustment:

- Keep one smallest runnable check for non-trivial behavior.
- Avoid broad test generation unless the plan requires it.
- If a planned test becomes much more complex than the behavior, stop and
  reassess whether a simpler integration or characterization check is better.

Do not add:

- A new execution artifact.
- A complexity review gate before implementation.
- Mandatory broad regression suites.

### 8.5 `doc-sync-close`

Role: sync docs and close a case.

Problem to prevent:

- Turning case-local learning into long-term authority.
- Creating governance follow-ups for weak or speculative knowledge.
- Rewriting docs that only need mechanical sync.
- Expanding closure into another planning phase.

Change:

- Add a "Closure Minimality" subsection.
- Prefer classification and proposal over patch when authority is not confirmed.
- Keep L3 sync mechanical.
- Do not create follow-up work unless there is a concrete unresolved fact,
  caveat, or blocked authority decision.

Recommended instruction:

```md
## Closure Minimality

Close the case with the smallest document sync that preserves evidence and
authority correctness.

Do not promote case-local observations into authority. Do not rewrite tutorials,
runbooks, or derived docs unless the change is mechanical and source-traceable.
Do not create governance follow-ups for vague lessons learned.

When authority impact is real but not confirmed, record a proposal in
`closure.md` instead of patching authority.
```

Do not add:

- Closure substatuses.
- A follow-up registry.
- A mandatory lessons-learned section.

### 8.6 `review`

Role: explicit, read-only evidence review.

Problem to prevent:

- Turning complexity review into an automatic workflow gate.
- Mixing simplification findings with correctness/security review.
- Producing long essays when the user asked for over-engineering review.

Current state:

- `review` already has `Complexity-only`.
- `review/references/complexity-only.md` already defines the right tags.

Change:

- Keep `Complexity-only` as the explicit user-triggered mode.
- Reference the shared vocabulary from `shared-protocol.md`.
- Keep output compact and replacement-oriented.
- State clearly that complexity-only is not a correctness/security review.

Recommended adjustment:

```md
For `Complexity-only`, use the shared Minimum Path vocabulary and
`references/complexity-only.md`. Keep findings locatable, one-line,
replacement-oriented, and limited to unnecessary complexity.
```

Do not add:

- Automatic review routing.
- A mandatory complexity review before every plan.
- A persistent review artifact.

### 8.7 `setup-doc-governance`

Role: initialize, rebuild, or repair documentation governance.

Problem to prevent:

- Creating empty authority structures.
- Over-scanning the repository.
- Introducing new layers to solve narrow doc drift.
- Turning historical material into current facts.

Change:

- Add a "Governance Minimality" subsection.
- Keep `docs-only` as default.
- Escalate to `full-repo` only when code/test evidence is needed.
- Create authority docs only for confirmed facts.
- Prefer merge/archive/bridge over new parallel structures when possible.

Recommended instruction:

```md
## Governance Minimality

Use the smallest governance batch that resolves the current documentation
failure.

Do not create empty authority folders, placeholder docs, broad archive moves,
or bridge files unless they prevent a real reader from being misled. Do not
scan the whole repository unless `full-repo` evidence is required by an
authority claim or explicitly requested by the user.

Promote only confirmed facts. Keep blocked or uncertain facts visible in the
governance plan instead of creating speculative authority.
```

Do not add:

- New lifecycle statuses.
- A governance score.
- A global docs migration unless the current failure requires it.

### 8.8 `grill`

Role: explicit interactive pressure testing.

Problem to prevent:

- Expanding a lightweight challenge into a workflow artifact.
- Letting discussion decisions silently become authority.

Change:

- Minimal change only: mention that minimum-path questions should be preferred
  when challenging process or architecture.

Recommended instruction:

```md
When challenging process, architecture, workflow, or scope, prefer the
highest-leverage minimum-path question: what can be deleted, reused, narrowed,
or left out without failing the stated goal?
```

Do not add:

- Grill reports.
- State updates.
- Authority proposals.

## 9. Template Changes

Only two template changes are recommended.

### 9.1 `plan.md`

Add a removable prompt under `## Non-goals`:

```md
- No speculative abstractions, extra artifacts, new workflow stages, or
  unrelated cleanup.
```

Reason:

- This makes the plan actively block adjacent work.
- It does not add a new section.
- It can be tailored or deleted when inappropriate.

### 9.2 `execution.md`

Add a short optional line under `## Changes Made`:

```md
- Minimum path used:
```

Use only when it clarifies a non-obvious simplification.

Do not make this required. Most executions should not need it.

## 10. Expected Behavior Changes

### 10.1 Low-risk README typo

Before:

- Possible case creation.
- Possible plan/execution/closure sequence.

After:

- Direct edit or fast-path only if persistent case is explicitly wanted.
- No context brief.
- No execution artifact unless verification is non-trivial.

### 10.2 Small behavior bug

Before:

- Agent may patch the named caller.
- Agent may add broad tests.

After:

- Agent checks shared function/callers.
- Fixes root cause at the narrowest shared point.
- Adds the smallest failing test that proves the bug.

### 10.3 Authority conflict

Before:

- Agent may over-read docs or propose governance rebuild.

After:

- `context-authority` reads the smallest authority set needed.
- Only routes to governance when there is a real blocking governance failure.

### 10.4 User asks "is this over-engineered?"

Before:

- Could be treated as standard review or workflow status.

After:

- `review` runs `Complexity-only`.
- Output uses delete/reuse/native/yagni/shrink/governance-bloat findings.
- No files or state are modified.

### 10.5 Governance rebuild request

Before:

- Could create broad authority structure.

After:

- Default scope is `docs-only`.
- Authority docs are created only for confirmed facts.
- Empty placeholders are forbidden.

## 11. Implementation Order

Implement in this order:

1. Update `skills/_shared/references/shared-protocol.md`.
2. Update `skills/development/docloom-workflow/SKILL.md`.
3. Update `skills/development/plan-confirm/SKILL.md`.
4. Update `skills/development/tdd-execute/SKILL.md`.
5. Update `skills/development/context-authority/SKILL.md`.
6. Update `skills/development/doc-sync-close/SKILL.md`.
7. Update `skills/governance/setup-doc-governance/SKILL.md`.
8. Update `skills/assessment/review/SKILL.md` and
   `skills/assessment/review/references/complexity-only.md` only if needed.
9. Update `skills/assessment/grill/SKILL.md` with the one minimum-path question rule.
10. Apply the two small template prompts if accepted.

Rationale:

- Shared protocol first gives all stage skills one authority.
- Router, plan, and execute are the highest-leverage path.
- Review already has most of the needed complexity-only behavior.

## 12. Verification Plan

Use these manual eval prompts after implementation.

### Eval 1: Status-only should not create a case

Prompt:

```text
现在这个 case 到哪了？只告诉我状态。
```

Expected:

- No files modified.
- No case created.
- Output includes current phase/evidence/route or a minimal clarification.

### Eval 2: Low-risk small docs edit

Prompt:

```text
把 README 里一处错别字修掉。
```

Expected:

- No full pipeline unless user explicitly asks.
- No context brief.
- No new governance artifact.

### Eval 3: Small bug fix

Prompt:

```text
这个 parser 在空输入时报错，修一下。
```

Expected:

- Agent reads relevant callers/shared parser.
- Fix is at the smallest correct shared location.
- Adds one targeted failing check if behavior is non-trivial.
- No new dependency or config.

### Eval 4: Complexity-only review

Prompt:

```text
帮我 review 一下这个 diff 有没有过度设计，只看冗余复杂度。
```

Expected:

- `review` selects `Complexity-only`.
- Read-only.
- Findings use tags like `yagni`, `shrink`, `governance-bloat`.
- Does not report correctness/security findings unless saying they are out of
  scope.

### Eval 5: Authority-related docs conflict

Prompt:

```text
这里 README 和 ADR 对 workflow 的说法冲突，帮我走 Doc Loom 处理。
```

Expected:

- `context-authority` reads ADR and directly relevant README/doc sections.
- Does not scan unrelated docs.
- Blocks or proceeds with risk based on authority order.
- Does not silently resolve by adding process.

### Eval 6: Governance rebuild

Prompt:

```text
用 setup-doc-governance 整理 docs-only 范围的文档治理。
```

Expected:

- Scope defaults to `docs-only`.
- No code/test modification.
- No empty authority docs.
- Governance plan records promote/merge/bridge/archive/block decisions.

## 13. Acceptance Criteria

This plan is implemented when:

- Shared protocol contains `Minimum Path Discipline`.
- Each primary workflow skill contains a local application of that discipline.
- No new skill, artifact type, stage, or state machine is introduced.
- Fast-path and status-only behavior are easier for the agent to choose.
- Plans explicitly block speculative adjacent work.
- Execution prefers deletion, reuse, native mechanisms, and root-cause fixes.
- Closure avoids promoting case-local observations into authority.
- Complexity-only review remains explicit, read-only, and compact.
- Governance setup avoids empty placeholders and broad migrations by default.
- The manual eval prompts above pass by behavior inspection.

## 14. Risks

| Risk | Mitigation |
|---|---|
| Agents under-read context and make wrong plans. | State that minimum path never means skipping understanding; context stops only when evidence is sufficient. |
| Agents skip needed TDD. | Preserve TDD default for behavior changes and require confirmed exceptions. |
| Agents avoid authority updates that are actually needed. | Closure still records authority proposals; confirmed narrow patches remain allowed. |
| Complexity-only mode is mistaken for full review. | Keep explicit scope warning in `review`. |
| Minimum path becomes vague style advice. | Put concrete ladders and gates in the shared protocol and stage skills. |

## 15. Rollout Strategy

Use a narrow rollout:

1. Make the shared protocol change.
2. Update the three highest-traffic skills: `docloom-workflow`,
   `plan-confirm`, `tdd-execute`.
3. Run eval prompts 1-4.
4. Update closure and governance skills.
5. Run eval prompts 5-6.
6. Only then tune wording in `review` and `grill`.

Do not run a broad rewrite of all design docs in the same change. If derived
design docs need sync, let `doc-sync-close` or a later docs sync case handle
that mechanically.

## 16. Proposed Patch Summary

Expected file changes:

```text
skills/_shared/references/shared-protocol.md
skills/development/docloom-workflow/SKILL.md
skills/development/context-authority/SKILL.md
skills/development/plan-confirm/SKILL.md
skills/development/plan-confirm/templates/plan.md
skills/development/tdd-execute/SKILL.md
skills/development/tdd-execute/templates/execution.md
skills/development/doc-sync-close/SKILL.md
skills/governance/setup-doc-governance/SKILL.md
skills/assessment/review/SKILL.md
skills/assessment/review/references/complexity-only.md
skills/assessment/grill/SKILL.md
```

Expected non-changes:

```text
docs/adr/ADR-0000-constitution.md
case_state.yaml shape, except no new fields
Artifact Policy artifact list
Closure status set
Risk level set
Skill list
```

## 17. Final Recommendation

Proceed with the narrow implementation.

The important choice is to treat "avoid over-design" as a discipline inside
the existing workflow, not as another workflow object. Doc Loom Least already
has the constitutional authority for this. The skills only need sharper local
instructions so agents choose the smallest correct path by default.
