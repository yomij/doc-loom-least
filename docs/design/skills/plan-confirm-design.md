# plan-confirm Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `plan-confirm` 的计划生成、风险标记、版本绑定、用户确认、计划落盘和执行前 gate。

---

## 1. 定位

`plan-confirm` 是计划生成、用户确认和任务计划文档落盘 skill。

它是进入执行前的唯一计划确认 gate。任何进入 Doc Loom Least 持久化 case workflow 的开发任务，在进入 `tdd-execute` 之前，必须有已确认的 `plan.md`。

`plan-confirm` 不生成 `case_id`，也不创建 case docs。`case_id` 与 `docs/cases/<case-id>/` 由 `docloom-workflow` 延迟创建。如果缺少 case identity，本 skill 必须返回 `needs_docloom_workflow` 或 `needs_case_initialization`。

核心规则：

```text
没有 context，不准计划。
默认没有用户确认计划，不准执行。
计划必须让一个不了解当前会话的工程师能按步骤执行。
writing-plans 只作为计划质量规则参考，不继承其运行路径、worktree 前提或执行分叉。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: plan-confirm
description: Create and confirm an implementation plan before execution in a document-driven workflow. Use after docloom-workflow has established case_id and case_docs, and after context-authority when a context gate is needed. Produces plan.md with decisions, risk level, TDD strategy, documentation impact, plan_version, base_commit, and explicit user confirmation before tdd-execute.
---
```

---

## 3. 触发条件

应该触发：

- 已完成 `context-authority`，需要写实施计划。
- 用户要求“先出计划”或“写 plan.md”。
- 用户确认的讨论决策需要写入或修改计划。
- `tdd-execute` 发现计划状态、版本或确认记录不一致，需要重新确认。
- 用户修改了任务目标、范围、风险或测试策略。

不应该触发：

- 用户已经确认计划并要求执行。此时使用 `tdd-execute`。
- 任务已完成，需要收尾。此时使用 `doc-sync-close`。

---

## 4. 输入

- `Context & Authority Brief`。
- 用户本轮需求。
- 当前 git baseline。
- skill-defined 计划确认策略。
- 相关 authority 文档。
- `docloom-workflow` 确认的 `case_id` 和 `case_docs`。
- `case_state.yaml`，如果已经创建。
- 已有 `plan.md`，如果是修改计划。
- 用户明确确认的讨论决策，如果本次计划需要吸收。
- Context Sources / route verdict，如果 `context-authority` 已生成。

---

## 5. 输出

```text
docs/cases/<case-id>/plan.md
docs/cases/<case-id>/case_state.yaml
```

条件输出：

```text
docs/cases/<case-id>/handoff.md    # 仅当存在未来恢复点
```

等待确认时：

- `plan.md` 的 `status` 为 `draft`。
- `case_state.yaml` 的 `phase` 为 `waiting_for_plan_confirmation`。

用户确认后：

- `plan.md` 的 `status` 改为 `approved`。
- 写入 `approved_by`、`approved_at` 和 `Confirmation Log`。
- `case_state.yaml` 的 `phase` 改为 `planned`。

---

## 6. 推荐资源结构

```text
plan-confirm/
  SKILL.md
  templates/
    plan.md
    handoff.md
  references/
    shared-protocol.md
```

---

## 7. case ID 与运行模式

`case_id` 归 `docloom-workflow` 生成或确认。`plan-confirm` 只消费已有 `case_id` 和 `case_docs`。

如果 case docs 不存在：

```text
return needs_docloom_workflow / needs_case_initialization
```

运行模式：

```text
isolated
branch
inline
```

`plan-confirm` 可以记录模式，但不主动创建 branch 或 worktree，除非用户明确要求或确认。

隔离策略：

- high risk、跨模块、大任务、并行任务应建议 `isolated` 模式。
- 如果使用项目内 `.worktrees/` 或 `worktrees/`，创建前必须验证该目录被 git ignore。
- 如果 baseline tests 已经失败，计划必须记录这是既有失败还是任务引入风险。
- 如果用户拒绝 worktree，记录为 `inline` 或 `branch` 模式，不阻塞低/中风险任务。

---

## 8. Risk Level

每个计划必须标记风险等级。

```text
low
  文档、局部文案、非关键测试、无行为变化重构。

medium
  普通功能、局部行为变化、内部 API 或局部数据流修改。

high
  权限、安全、认证、计费、隐私、数据删除、public API、schema migration、破坏性架构边界调整。
```

默认确认策略：

```yaml
plan_confirmation_policy:
  low_risk_changes: require_confirmation
  medium_risk_changes: require_confirmation
  high_risk_changes: require_explicit_confirmation
```

Doc Loom Least v1 不支持 `low_risk_changes: auto`。普通项目文档、authority 文档或入口索引不能放宽此 gate。未来如支持自动执行，必须由 skill-owned 配置机制明确设计。

---

## 9. Plan Frontmatter

`plan.md` 必须包含：

```yaml
---
case_id: 20260617-auth-refresh-flow
plan_version: 1
status: draft
risk_level: medium
approved_by:
approved_at:
base_commit:
---
```

字段含义：

| 字段 | 含义 |
|---|---|
| `case_id` | 当前任务唯一标识 |
| `plan_version` | 计划版本，实质变化后递增 |
| `status` | `draft` 或 `approved` |
| `risk_level` | `low` / `medium` / `high` |
| `approved_by` | 确认者，通常记录为 `user` |
| `approved_at` | 用户确认时间 |
| `base_commit` | 计划确认时的代码基线 |

---

## 10. Plan Version Confirmation

`plan_version` 是用户确认计划的语义版本。

确认规则：

```text
1. 用户确认时，记录当前 plan_version、approved_by、approved_at 和 base_commit。
2. Confirmation Log 必须说明用户确认的是哪个 plan_version。
3. tdd-execute 执行前必须读取当前 approved plan_version。
4. 如果计划发生实质变化，必须递增 plan_version、回到 draft 并重新确认。
5. 非实质编辑只能修正文档表达、补 Confirmation Log 或更新执行记录，不改变已确认计划含义。
```

计划确认依赖人类可读的版本、状态、确认记录和 git baseline。

---

## 11. Base Commit

理想情况记录：

```bash
git rev-parse HEAD
```

如果不是 git 仓库：

```yaml
base_commit: unavailable:not-a-git-repository
```

如果 git 命令失败：

```yaml
base_commit: unavailable:<reason>
```

如果工作区有未提交变更，计划必须记录：

```md
## Workspace Baseline

- Base Commit:
- Dirty Workspace: Yes / No
- Existing Changed Files:
  - ...
```

---

## 12. TDD Applicability

默认：

```text
TDD Required: Yes
```

允许例外：

- 纯文档修改。
- 配置调整。
- 构建脚本修复。
- 日志 / telemetry。
- UI 文案。
- 删除死代码。
- 探索性 spike。
- 紧急 hotfix。
- 无法可靠自动化验证的环境类改动。

例外必须写：

```md
## TDD Applicability

- TDD Required: No
- If No, Reason:
- Alternative Verification:
  - manual check
  - snapshot check
  - build check
  - smoke test
  - reviewer confirmation
```

---

## 13. writing-plans 参考边界

`writing-plans` 是计划质量参考，不是运行协议来源。

继承：

- 先锁定文件结构和职责。
- 任务步骤粒度保持 2-5 分钟。
- 使用精确文件路径、精确命令和预期结果。
- 禁止 TBD / TODO / later / “类似 case N”。
- 行为变化默认 TDD，测试目标和失败原因必须清楚。
- 写完 draft 后自审 spec coverage、placeholder、type/name consistency、buildability 和 TDD integrity。

不继承：

- 不保存到 `docs/superpowers/plans/**`。
- 不要求 dedicated worktree 作为前提。
- 不提供 subagent-driven / inline execution 二选一。
- 不把 `superpowers:executing-plans` 作为执行入口。

这些运行职责由 `docloom-workflow`、`plan-confirm` 和 `tdd-execute` 承担。

---

## 14. 核心工作流

### Step 1. Validate Context

检查：

- 是否有足够 context。
- 如果有 `Context & Authority Brief`，读取 brief。
- 如果没有独立 brief，读取 `Context Sources` / route verdict。
- 是否有阻塞冲突。
- 是否有足够文件和文档依据。

如果缺少足够 context，回到 `context-authority`。

### Step 2. Determine case ID and Mode

解析：

- `case_id`。
- `docs/cases/<case-id>/`。
- `case_state.yaml`。
- mode：`isolated` / `branch` / `inline`。

如果缺少 `case_id` 或 `case_docs`，停止并返回 `docloom-workflow` 初始化 case。

### Step 3. Classify Risk

写入风险等级和理由。

高风险触发条件：

- public API。
- auth / permission / security。
- billing。
- privacy / compliance。
- destructive migration。
- data deletion。
- L1 critical facts。

### Step 4. Decide TDD Applicability

写明：

- 是否 TDD required。
- 如果不是，原因和替代验证方式。
- 如果是，Red-Green-Refactor 的测试顺序。

### Step 5. Lock File Structure

写任务步骤前，先列出文件结构和职责，防止计划只描述抽象动作。

必须说明：

- 创建哪些文件。
- 修改哪些文件，必要时标明行号或现有符号。
- 每个文件负责什么。
- 哪些文件不应触碰。
- 相关测试文件在哪里。

文件结构原则：

- 文件边界要清晰，单文件只承担一个主要责任。
- 现有代码库优先遵循本地模式。
- 如果必须拆分过大的文件，只能把拆分写成计划内的明确任务，不做顺手重构。

### Step 6. Write Bite-Sized Plan Draft

计划任务必须可执行，粒度尽量是 2-5 分钟一个动作。

每个任务建议结构：

````md
### case N: <component or behavior>

**Files:**
- Create:
- Modify:
- Test:

- [ ] **Step 1: Write the failing test**

```language
完整测试代码或关键断言
```

- [ ] **Step 2: Run test to verify it fails**

Run: `<exact command>`
Expected: FAIL with `<expected reason>`

- [ ] **Step 3: Write minimal implementation**

```language
实现要点或完整小片段
```

- [ ] **Step 4: Run test to verify it passes**

Run: `<exact command>`
Expected: PASS

- [ ] **Step 5: Commit or checkpoint**

Run: `<exact command or checkpoint instruction>`
Expected: clean case-level checkpoint
````

禁止：

- `TBD`、`TODO`、`later`。
- “添加适当错误处理”但不说明具体错误。
- “写测试”但不给测试目标、命令和预期失败。
- “类似 case N”。
- 引用尚未定义的类型、函数或方法。

### Step 7. Write Plan Document

模板：

```md
---
case_id:
plan_version: 1
status: draft
risk_level:
approved_by:
approved_at:
base_commit:
---

# Implementation Plan

## Goal

## Non-goals

## Decisions
| Decision | Source | Scope | Rationale |
|---|---|---|---|

## Assumptions

## Context Summary

## Context Sources
- Context Brief:
- Route Verdict:
- Included Authority Docs:
- Excluded Docs and Reasons:

## Workspace Baseline

## Risk Level

## TDD Applicability

## TDD Plan

## File Structure

## Files to Change

## Tests to Add / Update

## Acceptance Criteria

## Risks

## Documentation Impact
| Doc | Layer | Change Needed | Requires Confirmation |
|---|---|---|---|

## Confirmation Log
```

### Step 8. Self-Review Plan

写完 draft 后必须自审：

1. Spec / context coverage：每条需求、约束和验收标准是否有任务覆盖。
2. Placeholder scan：是否存在 TBD、TODO、later、模糊步骤。
3. Type and name consistency：后续步骤使用的类型、函数、文件名是否与前文一致。
4. Buildability：一个新 agent 是否能不读当前会话也开始执行。
5. TDD integrity：是否每个行为变更都有失败测试或记录化 TDD exception。

发现问题直接修正 draft。

高风险或跨模块计划应建议用户触发计划 review，或使用独立 reviewer 检查 plan 是否可执行。review 只能建议，不能替代用户确认。

### Step 9. Request User Confirmation

向用户输出：

- 计划摘要。
- 风险等级。
- TDD 是否适用。
- 涉及的文档影响。
- 是否有 L1 变更需要确认。
- 计划引用了哪些 Context Sources，以及排除了哪些过期/低权威文档。
- 请求确认。

High risk 必须明确确认，不能把沉默当作确认。

### Step 10. Approve Plan

用户确认后：

- 更新 frontmatter。
- 更新 `Confirmation Log`。
- 更新 `case_state.yaml`。
- 如果存在未来恢复点，更新 `handoff.md`。

### Step 11. Plan Revision

计划实质变化包括：

- 目标变化。
- 用户确认的决策变化。
- 文件范围变化。
- 测试策略变化。
- 风险等级变化。
- Authority 文档影响变化。
- TDD exception 变化。

发生实质变化：

- `plan_version += 1`。
- status 回到 `draft`。
- 重新确认。

---

## 15. Handoff 模板

只有存在未来恢复点时生成或更新。

```md
# Handoff

## Current Phase
planned

## Last Completed
- Plan approved.

## Next Step
- Start TDD execution from the first Red step.

## Do Not Touch

## Commands Last Run

## Known Issues

## Source of Detailed History
- plan.md
- execution.md
- review.md
- closure.md
```

---

## 16. case_state.yaml 更新

`case_state.yaml` 是由 `docloom-workflow` 创建的瘦身状态缓存。`plan-confirm` 只更新计划阶段相关字段。

```yaml
case_id:
phase: planned
case_docs:
plan_version:
closure_status: open
last_updated:
```

---

## 17. Gate

- 没有 context，不写计划。
- 没有 `case_id` / `case_docs`，不写计划，返回 `docloom-workflow`。
- 有阻塞冲突，不写执行计划。
- 没有用户确认，不进入 `tdd-execute`。
- 计划变化后必须递增 `plan_version` 并重新确认。
- 当前 `plan_version` 未被确认，或 `status` 不是 `approved`，不准执行。
- 用户确认的讨论决策改变计划时，必须更新 `## Decisions`、递增 `plan_version` 并重新确认。

---

## 18. 验收标准

- `plan.md` 可独立说明任务目标、范围、风险和测试策略。
- `plan.md` 的 `## Decisions` 只包含用户明确确认的决策，不包含未确认建议。
- `plan.md` 包含 `plan_version`、`risk_level`、`base_commit`。
- 用户确认后包含 `approved_by`、`approved_at` 和 `Confirmation Log`。
- 如果存在未来恢复点，`handoff.md` 能让执行阶段直接接续。
- `case_state.yaml` 与当前 phase 一致，且不复制 `plan.md` 的权威字段。
- 高风险计划有明确用户确认记录。

---

## 19. 失败与恢复

如果用户拒绝计划：

- 保留 `draft`。
- 根据反馈修改。
- 实质变化递增版本。

如果无法计算 git baseline：

- 记录 unavailable reason。
- 不因此阻塞纯文档或低风险任务。

如果是否属于计划实质变化不清：

- 默认按实质变化处理。
- 递增 `plan_version`，回到 `draft`，重新确认。

---

## 20. 参考资料

- [writing-plans/SKILL.md](../../reference/skills/writing-plans/SKILL.md)
- [writing-plans/plan-document-reviewer-prompt.md](../../reference/skills/writing-plans/plan-document-reviewer-prompt.md)
- [executing-plans/SKILL.md](../../reference/skills/executing-plans/SKILL.md)
- [using-git-worktrees/SKILL.md](../../reference/skills/using-git-worktrees/SKILL.md)
- [AI Coding 场景下的文档快速定位、索引设计与任务上下文生成](<../../reference/docs/AI Coding 场景下的文档快速定位、索引设计与任务上下文生成.md>)
