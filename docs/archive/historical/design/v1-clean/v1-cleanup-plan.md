---
status: archived
plan_version: 1
scope: skills-cleanup
created: 2026-06-23
approved_by:
approved_at:
---

# V1 Clean: Doc Loom Least Skills 精简计划

基于 8 个 skills、31 个文件、2467 行的完整审查，制定本精简计划。

目标：削减 15-20% 冗余文本（~300-400 行），不损失任何行为约束。

## 核心原则

1. **SSOT (Single Source of Truth)**：每条规则只在一处定义，其他地方引用。
2. **Description 是路由触发器**：只回答"什么时候用"，不列内部细节。
3. **Gates 只写本 skill 特有的禁令**：通用禁令集中到 shared-protocol。
4. **模板只保留一份**：多 skill 共享的模板由 owner skill 维护。

## 阶段概览

| 阶段 | 内容 | 预计削减 | 风险 |
|---|---|---|---|
| 1 | shared-protocol.md 重构 | ~80 行 | 中（影响所有 skill） |
| 2 | SKILL.md description 瘦身 | ~100 行等效 token | 低 |
| 3 | 跨文件去重 | ~60 行 | 低 |
| 4 | Gates 精简 | ~40 行 | 低 |
| 5 | 模板去重 | ~30 行 | 低 |
| 6 | References 和 review/grill 精简 | ~50 行 | 低 |

---

## 阶段 1：shared-protocol.md 重构

**文件**: `_shared/references/shared-protocol.md` (当前 325 行)

### 1.1 新增 HIGH_RISK_TOPICS 常量段

在文件顶部（Design Guardrails 之前）新增：

```md
## High-Risk Topics

The following topics are high-risk across all skills:

- Security, auth, permission, privacy, billing, data deletion.
- Public API, CLI, schema, config contract.
- ADR-protected architecture boundary.
- Workflow or agent policy change.

Any skill referencing "high-risk" uses this definition.
```

**影响**：其他 skill 中重复列举 high-risk 话题的段落可以删除，改为引用此段。

### 1.2 合并 Execution Instruction Order 和 Fact Authority Order

当前两个段各 14 行，高度重叠。合并为一个段：

```md
## Instruction And Fact Priority

When deciding **how to execute**, user instructions rank first:

1. User instructions (current turn)
2. Approved governance plan (if governance task)
3. Active Doc Loom skill rules
4-10. Authority docs → code → tests → ADR → user info → L2 → L3 → L4 → L5

When deciding **what is true**, authority docs rank first:

1. Active authority docs
2. Current production code
3. Current tests
4-9. ADR → user info (pending) → L2 → L3 → L4 → L5

User instructions can change the current task's goal. They do not silently
rewrite long-term authority, contract, or agent facts.
Generated views can be consumed but cannot override their source of truth.
```

**节省**：~14 行。

### 1.3 新增通用禁令段

新增 `## Universal Constraints` 段，集中各 skill Gates 中重复的禁令：

```md
## Universal Constraints

These apply to all Doc Loom skills. Individual skill Gates only list
skill-specific constraints.

- Do not create case id or case docs (owned by `docloom-workflow`).
- Do not modify code, tests, dependencies, scripts, lockfiles, or CI
  unless the skill explicitly authorizes it.
- Do not call a CLI backend.
- Do not auto-continue to `review`, `grill`, or `setup-doc-governance`;
  those need explicit user intent.
- Do not silently promote unconfirmed facts into authority.
- Do not treat archived, superseded, derived, or scratch docs as current facts.
```

**节省**：各 skill Gates 中约 ~40 行重复禁令可以删除。

### 1.4 合并 Case State Conflicts 和 case_state.yaml 段

当前两个段（14 + 22 行）有重叠。合并要点：

- case_state.yaml 定义字段和所有权。
- 冲突处理规则紧跟其后，不单独成段。

### 1.5 精简 Git Degraded Mode

当前 12 行。压缩为 4 行：

```md
## Git Degraded Mode

When `git_available: false`: skip base_commit (leave empty with reason),
branch/worktree operations (use inline mode), git diff (ask user),
commit/stage (record "commit deferred: git unavailable").
User-provided manual information is pending evidence, not verified git state.
```

### 1.6 精简 Session State Carry

当前 8 行。合并到 case_state.yaml 段末尾，作为一句话：

> When producing output consumed by the next skill, include phase, closure_status, case_id, and any routing fields (next_skill, route_reason, required_input). The next skill may use prior output instead of re-reading case_state.yaml in the same session.

### 1.7 精简 Case Resume

当前 22 行。和 `context-resolution.md` 重叠。保留核心规则，去掉 handoff.md 写作细节（已在各 skill 模板中）：

```md
## Case Resume

On cross-session resume, read `handoff.md` first, then the minimum artifact
for the phase. Re-run `context-authority` only when authority, governance,
code, or external dependencies may have changed.

Before proceeding, verify the last completed step still holds (tests pass,
files exist, workspace matches base_commit or explains deviation).

Staleness: when `last_updated` > 7 days, report and ask user to confirm.
```

**节省**：~12 行。

### 阶段 1 总计

shared-protocol.md 预计从 325 行压缩到 ~245 行（-80 行）。

---

## 阶段 2：SKILL.md Description 瘦身

每个 description 压缩到 1-2 行。只保留触发条件，去掉内部功能列举。

### 2.1 docloom-workflow

```yaml
# Before (65 词)
description: Public automatic entry for Doc Loom Least. Use as the default entry when a user asks for a docs-first change workflow but does not explicitly invoke a specific Doc Loom skill. Resolves current case status, routes to setup-doc-governance, context-authority, plan-confirm, tdd-execute, doc-sync-close, or explicitly requested conversation-only review, owns delayed case creation and case_id generation, consumes review_risk signals, and applies the global Artifact Policy without becoming a heavy orchestrator.

# After (22 词)
description: Default entry for Doc Loom workflow. Routes to the correct stage skill, owns case identity, and applies the Artifact Policy.
```

### 2.2 context-authority

```yaml
# Before (45 词)
description: Gather minimal task context and resolve authority sources before planning. Use before plan-confirm when a case needs a context gate, when resuming a case, when deciding which docs/code/tests are authoritative, or when checking conflicts between user requests, authority docs, code, tests, governance decisions, and case docs.

# After (20 词)
description: Gather minimal context and resolve authority before planning. Use before plan-confirm or when resuming a case.
```

### 2.3 plan-confirm

```yaml
# Before (50 词)
description: Create and confirm an implementation plan before execution in a document-driven workflow. Use after docloom-workflow has established case_id and case_docs, and after context-authority when a context gate is needed. Produces plan.md with decisions, risk level, TDD strategy, optional adaptive execution policy, documentation impact, plan_version, base_commit, and explicit user confirmation before tdd-execute.

# After (18 词)
description: Create and confirm an implementation plan before tdd-execute. Use after case identity is established.
```

### 2.4 tdd-execute

```yaml
# Before (60 词)
description: Execute an approved persistent Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan with plan_version, risk_level, and base_commit, or after a minimal approval writeback in the same turn. Handles red-green-refactor, TDD exceptions, adaptive execution amendments, implementation, quality checks, execution evidence, handoff, case_state.yaml updates, deviations, review_risk signals, and authorized atomic commits.

# After (18 词)
description: Execute an approved case plan with TDD discipline. Use after plan-confirm produces an approved plan.
```

### 2.5 doc-sync-close

```yaml
# Before (55 词)
description: Close an existing document-driven workflow case by syncing docs and writing closure. Use after execution, or when a case is blocked, cancelled, paused, superseded, abandoned, or otherwise ending. Updates L2 case docs, safe mechanical L3 derived docs, proposes authority changes or applies explicitly confirmed narrow authority patches, records tests, acceptance criteria, findings, review_risk, evidence gaps, confirmations, follow-ups, writes closure.md, and sets closure_status.

# After (22 词)
description: Close, pause, or end a case by syncing docs and writing closure. Use after execution or when a case ends.
```

### 2.6 setup-doc-governance

```yaml
# Before (50 词)
description: Set up or rebuild documentation governance for a project. Use when the user asks to initialize docs governance, organize project docs, rebuild authority docs, classify historical docs, extract facts from existing docs/code/tests, create or update a governance plan, migrate docs, bridge old entries, archive superseded docs, or repair the documentation index.

# After (18 词)
description: Set up or rebuild documentation governance. Use when the user asks to organize, classify, or repair project docs.
```

### 2.7 review

```yaml
# Before (45 词)
description: Review a specified design document, proposal, code diff, test set, documentation change, or Doc Loom case evidence when the user explicitly asks for review. Performs read-only, conversation-only evidence review; reports assessment, findings, and evidence gaps; does not create artifacts, modify files, update status, write proposals, or route workflow.

# After (20 词)
description: Read-only evidence review of docs, code, tests, or designs. Use only when the user explicitly asks for review.
```

### 2.8 grill

```yaml
# Before (50 词)
description: Interactively challenge and stress-test a requirement, plan, architecture proposal, implementation idea, test strategy, documentation change, or other claim. Use only when the user explicitly asks to grill, challenge, pressure-test, scrutinize, or aggressively question assumptions. Ask one question at a time, provide a recommended answer with conditions, verify facts from code or docs when possible, and do not create artifacts or modify files.

# After (20 词)
description: Pressure-test a claim through one-question-at-a-time conversation. Use only when the user explicitly asks for grilling.
```

### 阶段 2 总计

8 个 description 从 ~420 词压缩到 ~158 词（-262 词，~100 行等效 token）。

---

## 阶段 3：跨文件去重

### 3.1 Git 命令块去重

**删除位置**：
- `docloom-workflow/SKILL.md` L16-22 的 bash 代码块
- `context-authority/SKILL.md` L25-30 的 bash 代码块

**替换为**：
```md
Run the minimal workspace checks per shared-protocol §Git And Commands.
```

**节省**：~12 行。

### 3.2 "Do not create case id" 去重

**删除位置**：
- `context-authority/SKILL.md` L78 和 L114 (Gates)
- `doc-sync-close/SKILL.md` L34
- `plan-confirm/SKILL.md` L9 和 L31

**替换为**（在首个提及处保留一句，其余引用）：
```md
Per shared-protocol §Universal Constraints, case identity is owned by `docloom-workflow`.
```

**节省**：~15 行。

### 3.3 High-risk 话题清单去重

**删除位置**（保留 shared-protocol 中的定义，其他地方引用）：
- `context-authority/SKILL.md` §Conflict Handling 中的完整清单
- `grill/SKILL.md` §High-Risk Topics 段
- `review/SKILL.md` §Review Checks 中的 high-risk 列举
- `tdd-execute/SKILL.md` §Review Risk 中的 high-risk 列举
- `setup-doc-governance/references/verdicts.md` §Use `block` when 中的清单

**替换为**：
```md
Per shared-protocol §High-Risk Topics.
```

**节省**：~30 行。

### 3.4 Closure Status 去重

`doc-sync-close/SKILL.md` §Closure Status 重述了 shared-protocol §Closure Status 的状态集。

**替换为**：
```md
Closure statuses per shared-protocol §Closure Status. Key distinctions:
- `Done`: all acceptance criteria verified as met.
- `Done with Caveats`: main goal complete, residual risk or follow-up remains.
```

**节省**：~5 行。

### 阶段 3 总计：~62 行。

---

## 阶段 4：Gates 精简

### 原则

每个 skill 的 Gates 段只保留 **本 skill 特有的** 约束。通用约束已在 shared-protocol §Universal Constraints 中定义，无需重复。

### 4.1 context-authority Gates

当前 7 条。删除 3 条通用禁令（不创建 case、不修改文件、不使用归档作当前事实），保留 4 条特有约束：

```md
## Gates

- No context, no plan.
- Blocking conflict must not enter `plan-confirm`.
- Low-authority or stale sources → mark as risk or block.
```

### 4.2 plan-confirm Gates

当前 8 条。删除 2 条通用禁令（不创建 case、不执行未确认计划），保留 6 条并合并相似项：

```md
## Gates

- No context and no skipped-context reason, no plan. → context-authority.
- Blocking conflict, no plan. → context-authority.
- Draft plans set phase to `waiting_for_plan_confirmation`.
- No user confirmation, no `tdd-execute`.
- High-risk confirmation must identify current `plan_version`.
- Discussion decisions that change constraints must enter `## Decisions`.
```

### 4.3 tdd-execute Gates

当前 10 条。删除 3 条通用禁令（不修改 authority、不修改 scripts/deps、不触碰 non-goals），合并 2 条 TDD Red 规则：

```md
## Gates

- Execution requires: approved plan, confirmed `plan_version`, resumable case status.
- TDD required but no observed expected Red, no implementation.
- TDD exception without alternative verification, no execution.
- Serious plan deviation, stop → `plan-confirm`.
- Unrecoverable closed case, no execution without new case or reconfirmation.
```

### 4.4 doc-sync-close Gates

当前 9 条。删除 2 条通用禁令（不修改 plan semantics、不修改 code/tests），合并相似项：

```md
## Gates

- Authority update requires user confirmation.
- Unmet acceptance criteria cannot be `Done`.
- Critical/Important user-provided findings cannot be ignored.
- High `review_risk` with insufficient evidence cannot be unqualified `Done`.
- Missing required `execution.md` cannot be unqualified `Done`.
- Write `closure.md` before updating `case_state.yaml`.
```

### 4.5 review Gates

当前 6 条。删除 4 条通用禁令（不写文件、不修改状态、不输出路由、不写 authority proposal），保留 2 条：

```md
## Gates

- User must explicitly request review.
- Missing key evidence → report as finding, not passing assessment.
```

### 4.6 grill Gates

当前 5 条。删除 3 条通用禁令（不生成文件、不修改文件、不输出路由），保留 2 条：

```md
## Gates

- User must explicitly request grilling.
- One question at a time; verify answerable facts before asking.
```

### 4.7 setup-doc-governance Gates

当前 8 条。删除 2 条通用禁令（不修改 code、不创建空 authority），合并：

```md
## Gates

- Do not apply changes before user confirmation.
- Do not write a new batch into an existing plan file.
- Do not write unconfirmed material into authority.
- Do not make bridge files carry old facts.
- Update the docs entry index after applying decisions.
```

### 阶段 4 总计：~40 行。

---

## 阶段 5：模板去重

### 5.1 handoff.md 模板合并

当前 3 份几乎相同的 handoff 模板（plan-confirm、tdd-execute、doc-sync-close）。

**方案**：保留 `docloom-workflow/templates/handoff.md` 作为唯一模板（docloom-workflow 是 case identity owner，也适合做 handoff owner）。各 skill 在需要写 handoff 时引用此模板。

**删除**：
- `plan-confirm/templates/handoff.md`
- `tdd-execute/templates/handoff.md`
- `doc-sync-close/templates/handoff.md`

**新增**：
- `docloom-workflow/templates/handoff.md`（通用模板，不预设 phase 值）

```md
# Handoff

## Current Phase

<set by writing skill>

## Last Completed

## Next Step

## Resume Condition

## Known Issues

## Source of Detailed History

- plan.md
- execution.md
- closure.md
```

**节省**：~20 行（三份合一份）。

### 5.2 case_state.yaml 模板去重

当前 2 份（docloom-workflow、doc-sync-close）。

**方案**：只保留 `docloom-workflow/templates/case_state.yaml`。`doc-sync-close` 引用此模板。

**删除**：`doc-sync-close/templates/case_state.yaml`。

**节省**：~10 行。

### 阶段 5 总计：~30 行。

---

## 阶段 6：References 和 review/grill 精简

### 6.1 verdicts.md Plan Tables 去重

`verdicts.md` L58-79 的 3 个 Plan Tables 和 `GOVERNANCE_PLAN.md` 模板完全重复。

**方案**：verdicts.md 删除 Plan Tables 段，改为：
```md
## Plan Tables

Use the table structures in `templates/GOVERNANCE_PLAN.md`.
```

**节省**：~20 行。

### 6.2 context-resolution.md Case Intent 表精简

删除 "Default exclusions" 列（大多是常识），加一句总结：

```md
Exclude materials unrelated to the current intent. Historical drafts, stale
READMEs, and unrelated business code are excluded by default.
```

**节省**：~10 行。

### 6.3 review/SKILL.md Review Checks 精简

当前 "Review Checks" 段有 5 个分类、~25 个检查项。压缩为核心原则：

```md
## Review Checks

Core checks (adapt to target type):

- Is the claim clear and scoped?
- Does evidence support the conclusion?
- Are assumptions, non-goals, and success criteria stated and verifiable?
- Are draft/derived/archive materials incorrectly treated as authority?

Type-specific:
- Code/diff: matches plan, scope controlled, edge cases handled, no contract violation.
- Tests: cover behavior and failure paths, avoid testing implementation details.
- Docs governance: layers separated, no unconfirmed promotion, source_of_truth correct.
- High-risk: direct evidence required; missing evidence → finding or gap, not passing.
```

**节省**：~20 行。

### 6.4 grill/SKILL.md High-Risk Topics 段

删除整个段（已在 shared-protocol §High-Risk Topics 定义）。替换为引用：

```md
## High-Risk Topics

Per shared-protocol §High-Risk Topics. For high-risk recommendations:
lower certainty, state evidence gaps, treat short answers as discussion only.
```

**节省**：~8 行。

### 阶段 6 总计：~58 行。

---

## 变更影响矩阵

| 文件 | 阶段 | 变更类型 | 预计行数变化 |
|---|---|---|---|
| `_shared/references/shared-protocol.md` | 1 | 重构 | -80 |
| `docloom-workflow/SKILL.md` | 2,3,4 | 精简 | -25 |
| `context-authority/SKILL.md` | 2,3,4 | 精简 | -25 |
| `plan-confirm/SKILL.md` | 2,3,4 | 精简 | -20 |
| `tdd-execute/SKILL.md` | 2,3,4 | 精简 | -25 |
| `doc-sync-close/SKILL.md` | 2,3,4,5 | 精简 | -20 |
| `setup-doc-governance/SKILL.md` | 2,4 | 精简 | -15 |
| `review/SKILL.md` | 2,4,6 | 精简 | -30 |
| `grill/SKILL.md` | 2,4,6 | 精简 | -20 |
| `context-authority/references/context-resolution.md` | 6 | 精简 | -10 |
| `context-authority/references/retrieval-routing.md` | 3 | 去重 | -5 |
| `setup-doc-governance/references/verdicts.md` | 3,6 | 去重 | -25 |
| `plan-confirm/templates/handoff.md` | 5 | 删除 | -13 |
| `tdd-execute/templates/handoff.md` | 5 | 删除 | -14 |
| `doc-sync-close/templates/handoff.md` | 5 | 删除 | -15 |
| `doc-sync-close/templates/case_state.yaml` | 5 | 删除 | -10 |
| `docloom-workflow/templates/handoff.md` | 5 | 新增 | +12 |

**净变化**：约 -326 行 (从 2467 行 → ~2141 行，削减 ~13%)。

加上 description 瘦身的 token 节省（~262 词 ≈ ~100 行等效），**总有效削减 ~426 行 / ~17%**。

---

## 执行顺序和依赖

```
阶段 1 (shared-protocol)
  ↓ 必须先完成，其他阶段依赖新增的 §Universal Constraints 和 §High-Risk Topics
阶段 2 (description) — 无依赖，可并行
阶段 3 (跨文件去重) — 依赖阶段 1
阶段 4 (Gates) — 依赖阶段 1
阶段 5 (模板去重) — 无依赖，可并行
阶段 6 (references/grill/review) — 依赖阶段 1
```

建议：阶段 1 → (阶段 2 + 5 并行) → (阶段 3 + 4 + 6 并行) → 验证。

## 验证标准

1. 所有 skill 的 `references/shared-protocol.md` 引用路径正确。
2. 每个 SKILL.md 的 Gates 段只包含本 skill 特有约束 + 引用 shared-protocol §Universal Constraints。
3. 模板文件只存在于 owner skill 的 templates/ 目录。
4. 全文搜索 "security, auth, permission, privacy, billing" 只在 shared-protocol 中出现。
5. 全文搜索 "git rev-parse" 只在 shared-protocol 中出现。
6. 全文搜索 "Do not create case" 只在 shared-protocol 中出现。
7. 每个 description ≤ 25 词。
8. 总行数 ≤ 2200 行。

## 不在范围内

- 不改变任何 skill 的行为语义。
- 不改变 skill 之间的路由逻辑。
- 不改变 case_state.yaml 的字段定义。
- 不改变 Artifact Policy 表。
- 不改变 Fast-Path 条件。
- 不新增 skill 或合并现有 skill。
- 不修改模板的内容结构（仅去重）。
