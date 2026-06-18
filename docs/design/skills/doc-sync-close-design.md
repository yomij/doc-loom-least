# doc-sync-close Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `doc-sync-close` 如何同步任务文档、处理 authority / L2 / L3 文档更新、写 closure、设置最终状态并留下可追溯收尾记录。

---

## 1. 定位

`doc-sync-close` 是文档同步和任务收尾 skill。

它负责把执行结果写回 L2 任务文档，安全更新 L3 派生文档，对 authority 文档默认只提出变更建议；仅在用户明确确认且满足窄 patch 条件时更新 authority，最后写入 `closure.md`。

核心规则：

```text
Authority 文档更新必须用户确认；窄 patch 可由本 skill 执行，结构性治理进入 `GOVERNANCE_PLAN.md`。
有 case docs 的任务结束或中断都必须写 closure.md。
结项时必须识别新知识，但不能把未验证知识静默晋升为长期权威。
`closure.md` 是收尾真相记录，`case_state.yaml` 只是状态缓存。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: doc-sync-close
description: Close an existing document-driven workflow case by syncing docs and writing closure. Use after execution, or when a case is blocked, cancelled, paused, superseded, abandoned, or otherwise ending. Updates L2 case docs, safe mechanical L3 derived docs, proposes authority changes or applies explicitly confirmed narrow authority patches, records tests, acceptance criteria, findings, review_risk, evidence gaps, confirmations, follow-ups, writes closure.md, and sets closure_status.
---
```

---

## 3. 触发条件

应该触发：

- 执行完成，需要收尾。
- 任务被取消、暂停、阻塞、替代或放弃。
- 用户提供的 findings 导致任务需要暂停、阻塞、降级或收尾。
- 用户要求“同步文档并结束”。
- 用户要求“写 closure.md”。

不应该触发：

- 计划未执行且用户只是要改计划。
- 当前任务仍要继续实现，用户并未要求收尾。
- 没有 existing case docs，且只是一次性任务；此时不强制补 closure。

---

## 4. 输入

- `plan.md`。
- `plan.md ## Decisions`，用于判断用户确认的执行决策是否需要晋升为 authority proposal。
- `execution.md`，如果 Artifact Policy 要求或文件已经存在。
- `review_risk`，如果 execution、closure 前验证或用户上下文中存在。
- 用户提供的审查 findings，如果存在。
- 测试结果。
- 代码 diff。
- 相关 authority / L3 文档。
- 用户关于收尾状态的指令。
- `case_state.yaml`，如果 case docs 存在。
- 可选：`docs/governance/GOVERNANCE_PLAN.md`，用于查看已有 blocked decisions 或登记治理 follow-up。

`doc-sync-close` 不创建 case id 或 case docs。没有 case docs 但用户要求建档收尾时，先回到 `docloom-workflow` 创建最小 case identity，再进入本 skill。

---

## 5. 输出

必须输出：

```text
docs/cases/<case-id>/closure.md
docs/cases/<case-id>/case_state.yaml
```

条件输出：

```text
docs/cases/<case-id>/handoff.md    # Paused / Blocked / 需要未来恢复时
```

可选更新：

```text
docs/derived/**
docs/README.md 或 docs/DOC_INDEX.md
```

Authority 处理：

- 默认只生成 proposed change。
- 用户明确确认 narrow patch 后才可修改已有 authority 文档。
- 结构性、高风险、冲突型或新 authority 区域相关变更进入 `GOVERNANCE_PLAN.md` 或新 case。
- 如果用户未确认，也可以 closure 为 `Done with Caveats` 或 `Paused`，视任务状态而定。

---

## 6. 推荐资源结构

```text
doc-sync-close/
  SKILL.md
  templates/
    closure.md
    handoff.md
    case_state.yaml
  references/
    shared-protocol.md
    doc-update-rules.md
```

---

## 7. Closure Status

最终状态必须是以下之一：

```text
Done
Done with Caveats
Blocked
Cancelled
Superseded
Paused
Abandoned
```

判定：

| 状态 | 含义 |
|---|---|
| `Done` | 验收标准全部满足 |
| `Done with Caveats` | 主目标完成，但有明确残留风险或后续项 |
| `Blocked` | 因依赖、权限、信息不足等无法继续 |
| `Cancelled` | 用户或项目决定取消 |
| `Superseded` | 被另一个任务或方案替代 |
| `Paused` | 暂停等待外部条件，未来可能恢复 |
| `Abandoned` | 长期未继续，不建议直接恢复 |

未满足验收标准时不能标记 `Done`。

高风险任务不以是否发生审查对话作为 closure 条件。`doc-sync-close` 只读取 `review_risk`、验收证据、测试结果和 evidence gaps；如果 `review_risk = high` 且缺少足够验收证据，不能无条件标记 `Done`。如果 `review_risk = high` 但验收证据充分，可以记录 residual risk，但不因为缺少审查对话自动降级。

已关闭 case 可以承载小范围同源 follow-up：

- `Done` 后的补测试、文案、小边界修正可以复用原 case。
- `Paused`、`Blocked`、`Done with Caveats` 在用户明确要求且恢复条件满足时可继续。
- 必须保留原 closure 历史，并记录本次 follow-up。
- 新目标、新验收标准、新风险、新文件责任边界，或原状态为 `Cancelled` / `Superseded` / `Abandoned` 时，默认新开 case 或重新确认。

---

## 8. 文档更新规则

| Layer | 是否可自动更新 | 规则 |
|---|---:|---|
| L1 Authority | 有条件 | 默认 proposal；仅 explicit confirmation 后执行 narrow patch |
| L2 Operational | 是 | 自动更新任务过程文档 |
| L3 Derived | 有条件 | 仅 mechanical derived sync 可以自动执行 |
| L4 Historical | 否 | 默认不改 |
| L5 Scratch | 有条件 | 可清理或转正，但转正需说明来源 |

L1 narrow patch 必须同时满足：

- 目标 authority 文档已存在。
- 只在已有权威事实上做小范围同步。
- evidence 来自本 case 的 plan、execution、tests、diff 或 explicit user confirmation。
- 不创建新 authority 区域。
- 不移动、归档或 bridge 文档。
- 不涉及冲突事实、高风险 owner 裁决或生命周期迁移。
- 用户确认的是具体 patch，不是泛泛“同步一下”。

不满足 narrow patch 条件时，`doc-sync-close` 只写 closure proposal / governance follow-up，实际 authority 更新交给 `setup-doc-governance` 的 `GOVERNANCE_PLAN.md` 或新 case。

L3 自动更新仅限 mechanical derived sync：

- 来源可追溯。
- 不改变长期事实。
- 不与 authority 冲突。
- README / DOC_INDEX 的链接、路径、状态入口。
- troubleshooting 中新增已验证、可复现的短条目。
- examples 中修正已随代码变化的命令、路径或参数。
- getting-started / dev-guide 中同步已确认 contract 的机械引用。

以下 L3 变更只能提出 proposal，不能自动更新：

- 新增或改写产品目标、架构原则、workflow policy。
- 根据一次 case 经验泛化成最佳实践。
- 需要 owner 判断语义是否成立。
- 只是“看起来应该更新”的教程性重写。

---

## 9. 核心工作流

### Step 1. Determine Closure Status

根据验收结果、用户指令和执行状态选择 closure status。

如果不确定：

- 默认不要标记 `Done`。
- 使用 `Done with Caveats`、`Paused` 或 `Blocked`。

Authority proposal 未确认不阻止关闭 case；如果主目标和验收已经完成，通常标记 `Done with Caveats`。只有确认本身是继续执行的前置条件时，才使用 `Paused` 或 `Blocked`。

### Step 2. Verify Acceptance Criteria

从 `plan.md` 读取 acceptance criteria，逐项标记：

```md
| Criteria | Status | Evidence |
|---|---|---|
```

Status 必须是以下之一：

```text
met
partially_met
not_met
not_verified
out_of_scope
```

Evidence 可以是：

- 测试命令。
- 代码 diff。
- 手动验证说明。
- 用户提供的审查 findings 或确认。
- `execution.md` 中的 `met / partially met / not met / not verified` 映射。

如果 `tdd-execute` 标记某项为 `not verified`，`doc-sync-close` 不能默认为通过。必须补验证、请求用户确认、保留 caveat，或选择非 `Done` 状态。

状态映射：

| Acceptance 状态 | Closure 处理 |
|---|---|
| 全部 `met` | 可以 `Done`；若有未确认 authority proposal 或 residual risk，则 `Done with Caveats` |
| 任一 `not_verified` | 先补验证；补不了时根据是否影响核心验收，使用 `Done with Caveats` 或 `Blocked` |
| 任一 `partially_met` | 默认不能 `Done`；用户明确接受剩余部分为 follow-up 后，可 `Done with Caveats` |
| 任一 `not_met` | 默认 `Blocked` / `Paused` / `Cancelled` / `Superseded`；只有用户明确修改目标或验收口径后，才能 `Done with Caveats` |
| `out_of_scope` | 不阻止 `Done`，但必须说明为什么不属于当前验收 |

### Step 3. Summarize Code and Tests

记录：

- 代码变更摘要。
- 测试命令和结果。
- 未运行测试及原因。
- 已知风险。

不能把未运行测试标记为通过。

`doc-sync-close` 可以补跑非破坏性验证命令：

- `plan.md` / `execution.md` 已列出的测试命令。
- 与验收直接相关的最小测试。
- lint / typecheck / build 等只读验证。
- 项目已有的文档检查命令。

`doc-sync-close` 不能新增或修改业务代码、补测试、改依赖 / 脚本 / CI / lockfile，或重写计划 / 验收标准来适配结果。需要新增测试或修复代码时，停止收尾，回到 `tdd-execute` 或 `plan-confirm`。

### Step 3.5. Identify Knowledge Changes

结项时必须判断是否存在知识变化，但不要把这一步做成九问式治理访谈。

输出知识处理建议：

```md
## Knowledge Changes
| Knowledge | Type | Scope | Evidence | Suggested Action | Target |
|---|---|---|---|---|---|
```

`Type` 必须是以下之一：

```text
authority_candidate
regression_candidate
runbook_candidate
derived_sync
case_local
none
```

规则：

- `authority_candidate`：长期事实、contract、workflow policy、agent behavior、ADR 影响；写 proposal、`GOVERNANCE_PLAN.md` follow-up 或 narrow patch。
- `regression_candidate`：可复现 bug 或边界；优先转测试 follow-up。
- `runbook_candidate`：重复操作、恢复步骤、维护动作；写 proposal，不自动写 authority。
- `derived_sync`：机械同步到 L3 的入口、示例或 troubleshooting。
- `case_local`：只适用于本 case，留在 closure。
- `none`：明确说明没有可沉淀知识。
- 不直接改 authority。
- `plan.md ## Decisions` 不是自动晋升来源；只把长期、契约、workflow、agent policy 或 authority 相关决策列入 proposal。
- 可复现 bug 优先晋升为测试。
- 固定重复操作优先晋升为 automation / runbook。
- 只适用于本任务的一次性经验留在 L2 closure，不晋升。

### Step 4. Sync L2

确保：

- 需要执行证据时，`execution.md` 已更新；如果 Artifact Policy 不要求 `execution.md`，closure 承载完整验证证据。
- 如果存在未来恢复点，`handoff.md` 指向 closure 并写清恢复条件。
- `case_state.yaml` phase 为 `closed`。

`doc-sync-close` 默认只读 `plan.md`，读取范围包括 Goal、Non-goals、Decisions、Acceptance Criteria、Documentation Impact 和 Plan Amendments。它最多向 `plan.md` 追加 closure 链接或简短 status note，不更新 checkbox，不改变计划语义。

### Step 5. Sync L3

可更新：

- getting-started。
- dev-guide。
- examples。
- troubleshooting。
- README 中机械性入口链接。

如果 L3 与 authority 冲突：

- 以 authority 为准。
- 更新 L3 或标记 stale。

### Step 6. Handle Authority Changes

如果实现改变了权威事实，写入：

```md
## Authority Changes

| Doc | Status | Proposed / Applied Change | Evidence | Risk | Next |
|---|---|---|---|---|---|
```

规则：

- 未确认前不静默改 authority。
- `Status` 只能是 `proposed` 或 `applied`。
- `applied` 只允许用于用户明确确认后的 narrow authority patch。
- `Risk` 只能是 `low`、`medium` 或 `high`。
- `Next` 只能是 `none`、`confirm_narrow_patch`、`governance_plan` 或 `new_case`。
- 用户确认后可以更新 narrow authority patch，并在 closure 记录确认。
- 用户不确认时记录 caveat。
- Authority 更新 proposal 必须带 evidence 和 source_of_truth。
- 高风险 authority 必须在说明里写 owner 和 last_verified 要求；缺少这些信息时不能写成 `active`。
- 来自 `plan.md ## Decisions` 的 proposal 必须标明该 decision 的任务作用域和为什么需要晋升为长期权威。

### Step 6.5. Lifecycle Transitions

根据任务结果提出文档生命周期变化：

| 情况 | 建议状态 |
|---|---|
| 新建但未评审的文档 | `draft` |
| 已确认且 metadata 完整 | `active` |
| 被新文档或新方案替代 | `superseded` |
| 功能下线或历史保留 | `archived` |

实际文档状态只使用 `active`、`draft`、`superseded`、`archived`。`in_review` 和 `needs_refresh` 只能作为 proposal reason 或 freshness issue，不能写入 authority frontmatter。状态迁移必须记录在 closure 中；高影响文档不得由 agent 单独标记为 `active`。

### Step 7. Write Closure

写入 `closure.md`。

`closure.md` 写入失败时，不更新 `case_state.yaml`，不能宣称 case 已关闭。

### Step 8. Update case State

`closure.md` 写入并校验后，再更新 `case_state.yaml`。

示例：

```yaml
phase: closed
closure_status: Done with Caveats
closure_path: docs/cases/<case-id>/closure.md
last_updated: 2026-06-17T00:00:00+08:00
```

如果 `case_state.yaml` 更新失败，`closure.md` 仍是事实记录，但不能宣称 workflow fully synced；后续可用 `closure.md` 修复状态缓存。若 `case_state.yaml` 与 `closure.md` 冲突，以 `closure.md` + git state 重新推导，不静默覆盖。

---

## 10. closure.md 模板

`closure.md` 正文按最新结果维护，底部保留极简 `## History`。不要默认创建 `closures/` 编号目录，也不要纯覆盖旧 closure 关键结论。

```md
# Closure Report

## Summary

## Acceptance Criteria Status
| Criteria | Status | Evidence |
|---|---|---|

## Code Changes

## Tests
| Command | Result | Notes |
|---|---|---|

## Docs Updated
| Doc | Layer | Change Type | Evidence | Auto Updated |
|---|---|---|---|---|

## Knowledge Changes
| Knowledge | Type | Scope | Evidence | Suggested Action | Target |
|---|---|---|---|---|---|

## Decisions Reviewed
| Decision | Scope | Closure Handling |
|---|---|---|

## Findings Disposition
| Finding | Severity | Affects Acceptance | Disposition | Evidence |
|---|---|---:|---|---|

## Authority Changes
| Doc | Status | Proposed / Applied Change | Evidence | Risk | Next |
|---|---|---|---|---|---|

## Lifecycle Changes Proposed
| Doc | From | To | Reason | Requires Approval |
|---|---|---|---|---|

## Follow-up Updates
| When | Scope | Evidence | Effect on Final Status |
|---|---|---|---|

## Confirmation Log
| When | Confirmed By | Scope | Confirmation |
|---|---|---|---|

## Remaining Risks

## Follow-ups

## Final Status
Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned

## History
| When | Previous Status | Reason Updated |
|---|---|---|
```

---

## 11. handoff.md 收尾模板

仅在 `Paused`、`Blocked` 或存在未来恢复点时生成。

```md
# Handoff

## Current Phase
closed

## Closure Status
Paused / Blocked / Done with Caveats

## Last Completed
- Closure written.

## Resume Condition
- ...

## Next Step
- ...

## Known Issues

## Source of Detailed History
- plan.md
- execution.md  # if present
- closure.md
```

如果 closure status 是 `Paused` 或 `Blocked`，`Next Step` 必须写清恢复条件。

Handoff 规则：

- 不重复 `plan.md`、`execution.md`、`closure.md` 已有长内容。
- 只引用路径和下一步恢复条件。
- 对 fresh agent 只保留必要状态、阻塞项和应使用的后续 skill。
- `Paused` / `Blocked` 必须生成 handoff，且 `Resume Condition` 必填。
- `Done with Caveats` 只有 caveat 需要未来接续时才生成 handoff。
- `Done` / `Cancelled` / `Superseded` / `Abandoned` 通常不生成 handoff，除非用户明确要求未来恢复点。

---

## 12. Gate

- Authority 更新必须用户确认。
- 未满足 acceptance criteria 不能标记 `Done`。
- 用户提供的未处理 Critical / Important findings 影响当前验收时，不能标记 `Done`，但仍可以写 closure。
- `review_risk = high` 且验收证据不足时，不能无条件标记 `Done`。
- 有 case docs 但没有 `closure.md` 的任务不能算结束。
- 任务取消、阻塞、暂停、替代也必须写 closure。
- follow-up 更新不能覆盖旧 closure 关键证据，必须维护 `Follow-up Updates` 和极简 History。
- 未评审的候选知识不能写成 active / canonical。
- 生命周期状态迁移必须遵守 owner / confirmation / freshness 规则。
- `execution.md` 是条件输入；缺少非必需 `execution.md` 不自动降级。
- `doc-sync-close` 不创建 case id 或 case docs。
- `closure.md` 写入成功前不得更新 `case_state.yaml` 为 closed。
- `doc-sync-close` 不修改计划语义，最多追加 closure 引用。

---

## 13. 验收标准

- `closure.md` 清楚说明结果、测试、文档和风险。
- closure_status 明确。
- `case_state.yaml` 与 closure 一致。
- Authority changes 清晰可确认，且区分 `proposed` / `applied`。
- 后续项可独立创建新任务。
- 未完成或未验证内容没有被包装成 Done。
- closure 中包含知识变化识别，或明确说明本任务没有可沉淀知识。
- Authority / runbook / ADR / agent rule 的更新均以 proposal 形式呈现，等待人工确认。
- follow-up closure 保留旧状态摘要、更新原因和 follow-up evidence。
- findings 已映射到 disposition。
- 影响收尾或 authority 的用户确认已记录到 `Confirmation Log`。
- handoff 只在有未来恢复点时生成，并包含恢复条件。

---

## 14. 失败与恢复

如果测试未全部通过：

- 不标记 `Done`。
- 根据情况标记 `Done with Caveats` 或 `Blocked`。

如果 Artifact Policy 要求 `execution.md` 但不存在：

- 不能直接 `Done`。
- 先补 execution evidence，或在 closure 记录 evidence gap。
- 根据严重程度标记 `Done with Caveats` 或 `Blocked`。

如果 authority 需要更新但用户未确认：

- 保留 proposal。
- 记录 caveat。
- 不直接改 authority。
- 如果主目标和验收已完成，通常标记 `Done with Caveats`。

如果用户提供 Critical / Important findings：

- 如果影响当前验收且未处理，closure_status 只能是 `Blocked`、`Paused` 或 `Done with Caveats`。
- 如果已处理，closure 必须记录 finding、修复证据和验证结果。
- 如果不影响当前验收，closure 记录 out-of-scope / follow-up，不阻止 `Done`。
- Minor findings 不阻止 `Done`，但应记录 remaining risk / follow-up。

如果 closed case 有小范围同源 follow-up：

- 更新 `closure.md` 正文为最新结果。
- 在 `Follow-up Updates` 中记录 scope、evidence 和对最终状态的影响。
- 在 `History` 中保留旧 closure status、时间和更新原因。
- 若 follow-up 改变目标、风险或验收标准，停止并回到 `plan-confirm` 或新 case。

需要记录到 `Confirmation Log` 的收尾确认：

- authority narrow patch approval。
- 用户接受 `partially_met` 作为 follow-up。
- 用户接受 `not_verified` 作为 caveat。
- 用户选择带 residual risk 的 `Done with Caveats`。
- 用户确认复用 closed case 做 follow-up。
- 用户确认 cancellation / supersession / abandonment。

如果任务被用户取消：

- 写 `closure_status: Cancelled`。
- 记录已完成和未完成内容。

---

## 15. 参考资料

- [handoff/SKILL.md](../../reference/skills/handoff/SKILL.md)
- [AI Coding 文档生命周期管理与最佳实践](<../../reference/docs/AI Coding 文档生命周期管理与最佳实践.md>)
- [AI Coding 文档治理知识识别与流转机制](<../../reference/docs/AI Coding 文档治理知识识别与流转机制.md>)
- [AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制](<../../reference/docs/AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制.md>)
- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
