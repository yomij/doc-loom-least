# doc-sync-close Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `doc-sync-close` 如何同步任务文档、处理 authority / L2 / L3 文档更新、写 closure、设置最终状态并留下可追溯收尾记录。

---

## 1. 定位

`doc-sync-close` 是文档同步和任务收尾 skill。

它负责把执行结果写回 L2 任务文档，安全更新 L3 派生文档，对 authority 文档只提出变更建议并等待确认，最后写入 `closure.md`。

核心规则：

```text
Authority 文档更新必须用户确认。
有 case docs 的任务结束或中断都必须写 closure.md。
结项时必须识别新知识，但不能把未验证知识静默晋升为长期权威。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: doc-sync-close
description: Close a document-driven workflow case by syncing docs and writing closure. Use after execution or approved review, or when a case is blocked, cancelled, paused, superseded, abandoned, or otherwise ending. Updates L2 case docs, safe L3 derived docs, proposes authority changes for user confirmation, records tests and acceptance criteria, writes closure.md, and sets closure_status.
---
```

---

## 3. 触发条件

应该触发：

- 执行完成，需要收尾。
- `review` verdict 为 `Approved`，需要关闭任务。
- 任务被取消、暂停、阻塞、替代或放弃。
- 用户要求“同步文档并结束”。
- 用户要求“写 closure.md”。

不应该触发：

- 计划未执行且用户只是要改计划。
- review 仍有 blocking issues。
- Authority 更新还没有用户确认，但任务仍要继续实现。

---

## 4. 输入

- `plan.md`。
- `execution.md`。
- `review.md`，如果存在。
- 测试结果。
- 代码 diff。
- 相关 authority / L3 文档。
- 用户关于收尾状态的指令。
- `case_state.yaml`。
- 可选：`docs/governance/GOVERNANCE_PLAN.md`，用于查看已有 blocked decisions 或登记治理 follow-up。

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

- 只生成 proposed change。
- 用户确认后才修改。
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

---

## 8. 文档更新规则

| Layer | 是否可自动更新 | 规则 |
|---|---:|---|
| L1 Authority | 否 | 只生成 proposal 或 governance follow-up，必须用户确认 |
| L2 Operational | 是 | 自动更新任务过程文档 |
| L3 Derived | 有条件 | 机械性、可追溯更新可以自动执行 |
| L4 Historical | 否 | 默认不改 |
| L5 Scratch | 有条件 | 可清理或转正，但转正需说明来源 |

L3 可自动更新条件：

- 来源可追溯。
- 不改变长期事实。
- 不与 authority 冲突。
- 是 README 链接、示例、troubleshooting 等派生内容。

---

## 9. 核心工作流

### Step 1. Determine Closure Status

根据验收结果、用户指令和执行状态选择 closure status。

如果不确定：

- 默认不要标记 `Done`。
- 使用 `Done with Caveats`、`Paused` 或 `Blocked`。

### Step 2. Verify Acceptance Criteria

从 `plan.md` 读取 acceptance criteria，逐项标记：

```md
| Criteria | Status | Evidence |
|---|---|---|
```

Evidence 可以是：

- 测试命令。
- 代码 diff。
- 手动验证说明。
- review 结论。

### Step 3. Summarize Code and Tests

记录：

- 代码变更摘要。
- 测试命令和结果。
- 未运行测试及原因。
- 已知风险。

不能把未运行测试标记为通过。

### Step 3.5. Identify Knowledge Changes

结项时回答以下问题：

```text
1. 这次需求新增或改变了哪些业务规则？
2. 是否改变了模块边界、接口契约或领域模型？
3. 是否产生了新的通用实现模式？
4. 是否踩到了已有文档没有覆盖的坑？
5. 哪些知识只适用于本需求，哪些知识可复用？
6. 哪些知识需要让后续 Agent 知道？
7. 是否有可复现 bug 应转成 regression test？
8. 是否有固定操作应转成 runbook、hook 或 automation？
```

输出知识处理建议：

```md
## Knowledge Changes
| Knowledge | Type | Scope | Evidence | Suggested Action | Target |
|---|---|---|---|---|---|
```

规则：

- 候选知识先写 closure proposal 或 `GOVERNANCE_PLAN.md` follow-up。
- 不直接改 authority。
- 可复现 bug 优先晋升为测试。
- 固定重复操作优先晋升为 automation / runbook。
- 只适用于本任务的一次性经验留在 L2 closure，不晋升。

### Step 4. Sync L2

确保：

- 需要执行证据时，`execution.md` 已更新。
- 如果存在未来恢复点，`handoff.md` 指向 closure 并写清恢复条件。
- `case_state.yaml` phase 为 `closed` 或对应中断状态。

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

### Step 6. Propose Authority Changes

如果实现改变了权威事实，写入：

```md
## Authority Docs Requiring Approval

| Doc | Reason | Proposed Change | Confirmation Needed |
|---|---|---|---|
```

规则：

- 未确认前不静默改 authority。
- 用户确认后可以更新 authority，并在 closure 记录确认。
- 用户不确认时记录 caveat。
- Authority 更新 proposal 必须带 evidence、source_of_truth；高风险 authority 还必须带 owner 和 last_verified。

### Step 6.5. Lifecycle Transitions

根据任务结果提出文档生命周期变化：

| 情况 | 建议状态 |
|---|---|
| 新建但未评审的文档 | `draft` |
| 正在等待用户 / owner 确认 | `in_review` |
| 已确认且 metadata 完整 | `active` |
| 发现过期、漂移、owner 缺失 | `needs_refresh` |
| 被新文档或新方案替代 | `superseded` |
| 功能下线或历史保留 | `archived` |

状态迁移必须记录在 closure 中；高影响文档不得由 agent 单独标记为 `active`。

### Step 7. Write Closure

写入 `closure.md`。

### Step 8. Update case State

示例：

```yaml
phase: closed
closure_status: Done with Caveats
last_updated: 2026-06-17T00:00:00+08:00
```

---

## 10. closure.md 模板

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

## Knowledge Changes
| Knowledge | Type | Scope | Evidence | Suggested Action | Target |
|---|---|---|---|---|---|

## Authority Docs Requiring Approval
| Doc | Reason | Proposed Change |
|---|---|---|

## Lifecycle Changes Proposed
| Doc | From | To | Reason | Requires Approval |
|---|---|---|---|---|

## Remaining Risks

## Follow-ups

## Final Status
Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned
```

---

## 11. handoff.md 收尾模板

仅在 `Paused`、`Blocked` 或存在未来恢复点时生成。

```md
# Handoff

## Current Phase
closed

## Last Completed
- Closure written.

## Next Step
- None for this case.

## Known Issues

## Source of Detailed History
- plan.md
- execution.md
- review.md
- closure.md
```

如果 closure status 是 `Paused` 或 `Blocked`，`Next Step` 必须写清恢复条件。

Handoff 规则：

- 不重复 `plan.md`、`execution.md`、`review.md`、`closure.md` 已有长内容。
- 只引用路径和下一步恢复条件。
- 对 fresh agent 只保留必要状态、阻塞项和应使用的后续 skill。

---

## 12. Gate

- Authority 更新必须用户确认。
- 未满足 acceptance criteria 不能标记 `Done`。
- review verdict 为 `Needs Changes` 不能直接关闭为 `Done`。
- 有 case docs 但没有 `closure.md` 的任务不能算结束。
- 任务取消、阻塞、暂停、替代也必须写 closure。
- 未评审的候选知识不能写成 active / canonical。
- 生命周期状态迁移必须遵守 owner / review / freshness 规则。

---

## 13. 验收标准

- `closure.md` 清楚说明结果、测试、文档和风险。
- closure_status 明确。
- `case_state.yaml` 与 closure 一致。
- Authority proposed changes 清晰可确认。
- 后续项可独立创建新任务。
- 未完成或未验证内容没有被包装成 Done。
- closure 中包含知识变化识别，或明确说明本任务没有可沉淀知识。
- Authority / runbook / ADR / agent rule 的更新均以 proposal 形式呈现，等待人工确认。

---

## 14. 失败与恢复

如果测试未全部通过：

- 不标记 `Done`。
- 根据情况标记 `Done with Caveats` 或 `Blocked`。

如果 authority 需要更新但用户未确认：

- 保留 proposal。
- 记录 caveat。
- 不直接改 authority。

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
