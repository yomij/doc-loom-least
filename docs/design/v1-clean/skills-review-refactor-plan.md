# Doc Loom Skills 精简计划

基于 2026-06-23 全量评审，按优先级分阶段执行。

---

## 阶段一：shared-protocol 去重（高优先级）

### 1.1 定义 High-Risk Domains 锚点

**问题**：`security, auth, permission, privacy, billing, data deletion, public API, CLI, schema, config contract` 在 5 个文件中重复出现。

**方案**：在 `shared-protocol.md` 的 Fact Authority Order 之后新增一节：

```markdown
## High-Risk Domains

These domains are high-risk wherever they appear — in conflict handling, review,
governance blocks, fast-path exclusions, and authority patches:

security, auth, permission, privacy, billing, data deletion, public API, CLI,
schema, config contract, and ADR-protected architecture boundaries.

When a task touches any of these, it is not low-risk, cannot use fast-path, and
requires explicit user confirmation for authority changes.
```

**影响文件**：

| 文件 | 改动 |
|---|---|
| `shared-protocol.md` | 新增 High-Risk Domains 节 |
| `shared-protocol.md` Fast-Path（line 260） | 改为 "No conflict in any High-Risk Domain" |
| `shared-protocol.md` Small Project Degradation（line 289） | 改为 "task involves any High-Risk Domain" |
| `context-authority/SKILL.md` Conflict Handling（lines 97-103） | 改为 "See shared-protocol.md → High-Risk Domains" |
| `review/SKILL.md` Review Checks（lines 94-104） | 改为引用 shared-protocol |
| `verdicts.md` block 条件（lines 50-57） | 改为引用 shared-protocol |
| `doc-update-rules.md` Narrow Authority Patch（lines 15-27） | 高风险条件改为引用 shared-protocol |
| `grill/SKILL.md` High-Risk Topics（line 74） | 改为引用 shared-protocol |

### 1.2 合并 Session State Carry 与 case_state.yaml routing fields

**问题**：`shared-protocol.md` lines 226-232 和 lines 108-114 语义重复。

**方案**：删除 Session State Carry 独立小节。在 case_state.yaml 的 routing fields 段末尾加一句：

> This is the session state carry contract: when a skill reads case_state.yaml and
> produces output for the next skill, include phase, closure_status, case_id, and
> any routing field values. The next skill may use these from prior output instead
> of re-reading case_state.yaml, unless a fresh read is required by resume, dirty
> workspace, or possible state change.

### 1.3 合并 Case State Conflicts 两处

**问题**：lines 125-129 和 lines 199-213 都在说 case_state.yaml 冲突处理。

**方案**：保留 lines 125-129 的摘要（Report → Derive → Route → Don't overwrite），将 lines 199-213 的推导顺序合并进来，删除重复段落。

合并后：

```markdown
When case_state.yaml conflicts with Markdown:
- Report `state_cache_conflict`.
- Derive phase from the smallest reliable signal:
  1. `closure.md` exists -> `closed`.
  2. `execution.md` exists and acceptance checks are complete -> `doc_syncing`.
  3. `execution.md` exists -> `executing`.
  4. `plan.md` has `status: approved` -> `planned`.
  5. `plan.md` has `status: draft` -> `waiting_for_plan_confirmation`.
  6. No clear signal -> `needs_user_decision`.
- Route based on the derived phase.
- Do not silently overwrite the cache; the owning skill may update it only after
  deriving the correct phase.
```

---

## 阶段二：SKILL.md 精简（高/中优先级）

### 2.1 压缩所有 description frontmatter

**原则**：description 只写核心职责，1-2 句。触发条件和使用场景留给正文。

| 文件 | 当前 | 建议 |
|---|---|---|
| `docloom-workflow/SKILL.md` | 64 词 | `Public entry point for Doc Loom Least. Routes user requests to the correct stage skill, owns case identity creation and status-only queries.` |
| `context-authority/SKILL.md` | 56 词 | `Gather minimal task context and resolve authority sources before planning. Outputs a route verdict for plan-confirm or setup-doc-governance.` |
| `plan-confirm/SKILL.md` | 48 词 | `Create and confirm an implementation plan before execution. Produces plan.md with risk level, TDD strategy, and explicit user confirmation.` |
| `tdd-execute/SKILL.md` | 48 词 | `Execute an approved plan using TDD discipline. Handles red-green-refactor, exceptions, adaptive amendments, and execution evidence.` |
| `doc-sync-close/SKILL.md` | 52 词 | `Close a case by syncing docs and writing closure. Handles acceptance criteria verification, L2/L3 sync, and authority proposals.` |
| `review/SKILL.md` | 50 词 | `Read-only evidence review when the user explicitly asks. Reports findings and evidence gaps without modifying files or routing workflow.` |
| `setup-doc-governance/SKILL.md` | 42 词 | `Initialize or rebuild documentation governance. Produces a governance plan with file routing, fact extraction, and authority structure.` |
| `grill/SKILL.md` | 44 词（已较好） | `Interactively pressure-test a claim through conversation. Asks one question at a time, verifies facts from code/docs when possible.` |

### 2.2 `docloom-workflow` 和 `context-authority` Start 段去重

**问题**：两个 skill 的 Start 段运行相同的 git 命令。

**方案**：`docloom-workflow` 保留 Start 段。`context-authority` 的 Start 改为：

```markdown
## Start

If `docloom-workflow` already ran workspace checks in this session, reuse its
output. Otherwise run the minimal workspace checks:

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
```

Run `git diff --name-only` only when the workspace is dirty, resuming, or
plan-confirm will need a baseline.
```

### 2.3 `docloom-workflow` Route Table 合并行 6+8

**当前**：

```
| 6 | Need resume, authority decision, conflict handling, or high-risk work | context-authority |
| 8 | Persistent work needing a context gate but not yet high-risk enough for full context-authority | context-authority |
```

**改为**：

```
| 6 | Persistent work that needs a context gate (resume, authority, conflict, or high-risk) | context-authority |
```

删除行 8。

### 2.4 各 skill Gates 段去重

**原则**：Gates 只保留该 skill 特有的硬约束。与 shared-protocol 重复的条目改为引用或删除。

| 文件 | 删除的 Gate | 原因 |
|---|---|---|
| `docloom-workflow/SKILL.md` line 145 | `Do not write base_commit or risk_level` | shared-protocol case_state.yaml 段已定义 |
| `docloom-workflow/SKILL.md` lines 70-72 | `Do not create case docs for...` | 引用 shared-protocol Case Creation Boundary |
| `context-authority/SKILL.md` lines 118-119 | `If sources are only low-authority or stale` | Route Verdict 已覆盖 |
| `plan-confirm/SKILL.md` lines 135-137 | Plan Quality Standard 小节 | Plan Rules + Workflow 已覆盖 |
| `plan-confirm/SKILL.md` lines 141-143 | `No context / Blocking conflict` gates | Workflow 步骤 1-2 已检查 |
| `tdd-execute/SKILL.md` lines 215-217, 221 | `Unrecoverable closed / Touching non-goals` | Preflight 已检查 |
| `doc-sync-close/SKILL.md` lines 41-45 | Closure Status additional guidance | shared-protocol 已定义 |
| `doc-sync-close/SKILL.md` line 141 | 7 类不可修改之物的列举 | 简化为 "Do not modify code, tests, or artifacts to make closure pass" |
| `setup-doc-governance/SKILL.md` line 111 | user-provided facts 不静默成为 authority | shared-protocol Confirmation Semantics 已定义 |
| `setup-doc-governance/SKILL.md` line 156 | 不把 evidence/archive/derived 当事实 | shared-protocol Fact Authority Order 已定义 |

### 2.5 `plan-confirm` Version And Confirmation 精简

**当前**：lines 102-133 用 32 行描述 "On draft write" / "On confirmation" / "Material plan changes" 三个场景。

**改为**：保留关键状态转换，字段细节在模板注释中已有。

```markdown
## Version And Confirmation

State transitions:

| Event | plan.md status | case_state.yaml phase | Approval fields |
|---|---|---|---|
| Draft written | `draft` | `waiting_for_plan_confirmation` | Cleared |
| User confirms | `approved` | `planned` | `approved_by: user`, `approved_at`, `base_commit` |
| Material change | `draft` | `waiting_for_plan_confirmation` | Cleared, `plan_version` incremented |

Material changes: goal, decisions, file scope, test strategy, risk level,
authority impact, TDD exception, public contract, or file responsibility changes.
Checkbox/status edits are not material.
```

### 2.6 `tdd-execute` Review Risk Lifecycle 表格化

**当前**：lines 153-168 用 3 段文字描述 Write / Update / Do not clear。

**改为**：

```markdown
## Review Risk Lifecycle

| Condition | Action |
|---|---|
| Implementation touches High-Risk Domain, coverage insufficient, material deviation, or authority proposal pending | Write `review_risk` to `case_state.yaml` |
| Scope increases/decreases or new evidence resolves a concern | Update `review_risk` |
| Original high-risk conditions not all resolved | Do not clear to `low` |

The closure skill consumes the final value. Reasons stay in `execution.md`.
```

### 2.7 `tdd-execute` TDD anti-patterns 压缩

**当前**：lines 87-93 的 5 条测试反模式是通用最佳实践。

**方案**：保留但压缩为 2 条：

```markdown
Avoid testing anti-patterns: do not assert mock existence as behavior, do not add
test-only production methods, and ensure mock responses match real structures.
Prefer integration tests when mocks exceed the behavior under test.
```

### 2.8 `doc-sync-close` Knowledge Changes 举例精简

**当前**：lines 93-101 的 "Reproducible bugs usually become..." 等 3 句是举例。

**方案**：删除举例，保留分类定义。

---

## 阶段三：References 和 Templates 精简（中/低优先级）

### 3.1 `retrieval-routing.md` Governance Docs 优先级表化

**当前**：lines 6-27 用大段文字描述 governance plan 读取优先级。

**改为**：

```markdown
## Governance Docs

Read governance plans by priority:

1. `docs/cases/<case-id>/governance-plan.md` — when a case is active.
2. Most recent `docs/governance/*.md` with `status: approved` or `applied_with_blocks`.
3. Legacy `docs/governance/GOVERNANCE_PLAN.md` — read as context only, do not write.

If governance or authority is missing:
- Low-risk local work → `proceed_to_plan_with_risk`.
- High-Risk Domain work → `run_setup_doc_governance` or `needs_user_decision`.
```

### 3.2 `context-resolution.md` Run Modes 移到 shared-protocol

**问题**：`isolated` / `branch` / `inline` 三模式在 `context-authority` 只是识别，真正的使用在 `plan-confirm`。

**方案**：将 Run Modes 表移到 `shared-protocol.md`，`context-resolution.md` 和 `plan-confirm/SKILL.md` 引用。

### 3.3 `doc-update-rules.md` Narrow Authority Patch 引用 shared-protocol

**当前**：7 个条件中 "No conflict fact, high-risk owner decision, or lifecycle migration" 与 shared-protocol High-Risk Domains 重叠。

**方案**：条件 5 改为 "No High-Risk Domain conflict or lifecycle migration (see shared-protocol.md → High-Risk Domains)"。

### 3.4 `shared-protocol.md` 删除 Design Guardrails

**当前**：lines 9-12 是设计哲学。

**方案**：删除。Skill 行为已有足够约束，哲学陈述不增加实际指导。

### 3.5 `shared-protocol.md` Skill Runtime Boundary 合并 Do-not

**当前**：lines 24-30 有 5 条 Do not，其中 3 条指向同一件事。

**改为**：

```markdown
Do not:
- Depend on a CLI backend, `.agents/doc-loom` control files, or central task indexes.
- Replace stage skills from the entry skill.
```

### 3.6 `setup-doc-governance/SKILL.md` 评估 "Do not generate" 列表

**当前**：lines 58-61 禁止生成 `FACT_REGISTRY.md`, `KNOWLEDGE_CARDS.md` 等。

**方案**：如果这些文件是旧版设计的遗留产物且不再有生成风险，直接删除。否则保留 1 行 "Do not generate legacy artifact types from older designs."

### 3.7 `grill/SKILL.md` High-Risk Topics 压缩

**当前**：4 条规则。

**改为**：2 条。

```markdown
For high-risk recommendations:
- Lower certainty; state evidence sources and unverified points.
- Treat short user answers as discussion-only; do not phrase unverified
  conclusions as facts (see shared-protocol → Confirmation Semantics).
```

---

## 阶段四：模板清理（低优先级）

模板文件整体干净，无需大改。两个小建议：

1. **`plan.md` 模板**：删除 `## Optional Section Menu` 注释中的重复列举（已在顶部 HTML 注释中覆盖）。
2. **`closure.md` 模板**：同样删除底部的 `## Optional Section Menu` 注释，顶部 HTML 注释已完整。

---

## 执行顺序

| 阶段 | 内容 | 预估改动行数 | 风险 |
|---|---|---|---|
| 一 | shared-protocol 去重（High-Risk Domains 锚点 + 合并重复段） | ~80 行净减少 | 低 — 纯重构 |
| 二 | SKILL.md 精简（description + gates + 重复段） | ~120 行净减少 | 低 — 删除冗余 |
| 三 | References 精简（表格化 + 引用） | ~40 行净减少 | 低 — 格式调整 |
| 四 | 模板清理 | ~10 行净减少 | 极低 |

**总计预估**：约 250 行净减少，不改变任何 skill 的行为语义。

---

## 不纳入本次计划的项

- **Execution Instruction Order 和 Fact Authority Order 合并**：虽然骨架相似，但语义确实不同（执行优先级 vs 事实优先级），强行合并会降低可读性。
- **`review/SKILL.md` Finding Format 的 Minor 示例**：当前示例偏弱但无害。
- **`context-authority` Workflow 步骤 4 引用 retrieval-routing**：已通过 "Read when trigger condition is met" 机制覆盖。