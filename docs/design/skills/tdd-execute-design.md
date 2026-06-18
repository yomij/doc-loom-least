# tdd-execute Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `tdd-execute` 如何校验已确认计划、执行 Red-Green-Refactor、处理 TDD 例外、按 Artifact Policy 记录执行证据、更新 handoff 和提出 review 建议。

---

## 1. 定位

`tdd-execute` 是 TDD 执行和质量检查 skill。

它只在 `plan-confirm` 生成已确认计划后执行，默认先写失败测试，再做最小实现和重构。

核心规则：

```text
没有 approved plan，不准执行。
默认没有失败测试，不准实现。
如果没有亲眼看到测试因预期原因失败，就不能声称完成了 TDD。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: tdd-execute
description: Execute an approved document-driven implementation plan using TDD discipline. Use only after plan-confirm has produced an approved plan with plan_version, risk_level, and base_commit. Handles red-green-refactor, TDD exceptions, implementation, quality checks, execution.md, handoff.md, case_state.yaml updates, deviations, and review recommendations.
---
```

---

## 3. 触发条件

应该触发：

- 用户确认计划后要求执行。
- 已有 approved plan 且任务处于 `planned` / `executing`。
- `review` 要求返工，回到执行阶段。

不应该触发：

- `plan.md` 未确认。
- `plan_version` 未确认，或 `status` 不是 `approved`。
- 用户只是要求 review。此时使用 `review`。
- 用户只是要求收尾。此时使用 `doc-sync-close`。

---

## 4. 输入

- `docs/cases/<case-id>/plan.md`。
- `docs/cases/<case-id>/case_state.yaml`。
- `Context & Authority Brief`。
- 相关代码和测试。
- 项目命令规范和包管理规范。
- 可选：`review.md` 中的返工项。
- 可选：worktree / branch baseline 检查结果。

---

## 5. 输出

必须更新：

```text
docs/cases/<case-id>/case_state.yaml
```

条件输出：

```text
docs/cases/<case-id>/execution.md    # TDD / 行为变化 / 偏离 / 复杂验证 / 需要恢复时
docs/cases/<case-id>/handoff.md      # 仅当存在未来恢复点
```

---

## 6. 推荐资源结构

```text
tdd-execute/
  SKILL.md
  templates/
    execution.md
    handoff.md
  references/
    shared-protocol.md
    tdd-exceptions.md
```

---

## 7. 执行前校验

必须检查：

- `plan.md` 存在。
- `status: approved`。
- `plan_version` 存在。
- `approved_by`、`approved_at` 和 `Confirmation Log` 存在，并引用当前 `plan_version`。
- 当前任务未 closed。
- 当前工作区与 `base_commit` 差异可解释。
- TDD 适用性已声明。

如果任一校验失败：

- 不执行。
- 输出阻塞原因。
- 建议回到 `plan-confirm`。

---

## 8. TDD Applicability

默认：

```text
TDD Required: Yes
```

例外必须已在 `plan.md` 记录：

```md
## TDD Applicability

- TDD Required: No
- If No, Reason:
- Alternative Verification:
  - ...
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

例外不是跳过验证，必须执行替代验证。

如果任务是新功能、bug fix、重构或行为变化，默认必须 TDD。例外需要用户或已确认计划允许，并在 `execution.md` 中记录。

---

## 9. 核心工作流

### Step 0. Review Plan Critically

执行前先读完整 `plan.md`，检查：

- 步骤是否清楚到可以执行。
- 文件路径和命令是否存在。
- TDD 步骤是否真实可跑。
- 是否有阻塞问题或计划漏洞。

如果计划有关键缺口，不要猜测执行，回到 `plan-confirm`。

如果当前在主分支或高风险任务没有隔离 workspace，应建议用户使用 worktree / branch 隔离。skill 不擅自创建 worktree，除非用户明确确认；如果已存在 worktree，必须记录路径和 baseline。

### Step 1. Red

如果 TDD required：

- 先写失败测试。
- 运行最小测试命令。
- 确认失败原因符合预期。
- 记录到 `execution.md`。

失败原因不符合预期：

- 不继续实现。
- 调整测试或回到 `plan-confirm`。

测试反模式检查：

- 不测试 mock 自身存在。
- 不为测试往生产类加 test-only 方法。
- 不在不了解依赖副作用时随意 mock。
- mock 响应必须匹配真实结构，不能只补当前断言用到的字段。
- mock 设置比测试逻辑还复杂时，优先考虑更真实的集成测试。

### Step 2. Green

- 写最小实现。
- 运行目标测试。
- 确认新增或修改测试通过。
- 记录实现摘要。

### Step 3. Refactor

- 只做计划范围内必要重构。
- 不引入无关抽象。
- 每次重构后跑相关测试。
- 记录 refactor summary。

### Step 4. Quality Check

根据项目类型运行：

- 单元测试。
- 集成测试。
- lint。
- typecheck。
- build。
- smoke test。

必须遵守项目包管理约束：

- 前端依赖和脚本使用 `pnpm`。
- Python 依赖和脚本使用 `uv`。
- 不使用 npm / yarn / pip / conda。

如果项目要求 shell 命令加代理前缀，例如 `rtk`，所有 shell 命令必须遵守。

命令记录必须包含：

- exact command。
- expected result。
- actual result。
- 如果未运行，说明原因和替代验证。

### Step 5. Update Execution Docs

按 Artifact Policy 追加或更新 `execution.md`。

必须写 `execution.md` 的情况：

- TDD required。
- 存在代码或行为变化。
- 执行中发生计划偏离。
- 测试失败、重试或需要记录失败原因。
- review recommended / requested。
- 当前任务可能需要恢复。

纯文档、trivial config 或替代验证证据可完整写入 `closure.md` 时，可以不写独立 `execution.md`。

### Step 6. Update Handoff

只有存在未来恢复点时才覆盖 `handoff.md`，保持短、准、适合接续。

### Step 7. Review Recommendation

根据风险输出 review 建议，但不自动触发 review。

---

## 10. execution.md 模板

仅在 Artifact Policy 或触发条件要求时生成。

```md
# Execution Report

## Plan Reference
- plan_version:
- base_commit:

## Plan Followed
Yes / No

## Changes Made

## TDD Applicability

## TDD Log

### Red
- Test added:
- Expected failure:
- Actual failure:

### Green
- Implementation:
- Passing result:

### Refactor
- Refactor summary:
- Verification:

## Commands Run
| Command | Result | Notes |
|---|---|---|

## Deviations

## Remaining Issues

## Review Recommendation
- Recommended: Yes / No
- Reason:
  - ...

## Ready for Review
Yes / No
```

---

## 11. handoff.md 模板

仅在存在未来恢复点时生成或更新。

```md
# Handoff

## Current Phase
executing

## Last Completed

## Next Step

## Do Not Touch

## Commands Last Run

## Known Issues

## Source of Detailed History
- execution.md
- review.md
- closure.md
```

---

## 12. case_state.yaml 更新

执行中：

```yaml
phase: executing
review_status: not_requested
closure_status: open
```

执行完成但未 review：

```yaml
phase: executing
review_status: recommended
```

准备收尾：

```yaml
phase: doc_syncing
```

---

## 13. Deviation 规则

轻微偏离：

- 文件名变化但目标不变。
- 测试命令调整。
- 小范围实现路径变化。

处理：

- 记录到 `execution.md`。
- 可继续。

严重偏离：

- 目标变化。
- 风险等级升高。
- 触及未计划的 public API / auth / data migration。
- 新增 authority 更新需求。
- TDD exception 从 No 变 Yes 或从 Yes 变 No。
- 计划步骤无法执行，需要重写任务分解。

处理：

- 停止执行。
- 回到 `plan-confirm`。

---

## 14. Review Recommendation

建议 review 的条件：

- `risk_level = high`。
- 修改 public API。
- 修改权限、安全、认证、计费、隐私。
- 修改数据模型或 migration。
- 测试覆盖不足。
- 有 authority 更新 proposal。
- 执行中出现严重计划偏离。

输出格式：

```md
## Review Recommendation

- Recommended: Yes / No
- Reason:
  - ...
```

注意：这只是建议，不是用户授权。

---

## 15. Gate

- 没有 approved plan，不执行。
- 当前 `plan_version` 未被确认，不执行。
- TDD required 但未写失败测试，不实现。
- 新测试如果第一次运行就通过，必须重写测试或说明已有行为已经满足，并回到计划判断。
- 测试失败原因不是预期原因，不进入实现。
- TDD exception 没有替代验证，不执行。
- 严重偏离计划，不继续执行。
- 未记录命令结果，不宣称验证完成。
- 不能通过断言 mock 存在来证明真实行为。

---

## 16. 验收标准

- 测试按计划新增或更新。
- 至少相关测试通过。
- 质量检查结果记录完整。
- 当 Artifact Policy 要求时，`execution.md` 有 Red / Green / Refactor 或明确 TDD exception。
- 当存在未来恢复点时，`handoff.md` 能说明下一步。
- `case_state.yaml` 与执行状态一致。
- review recommendation 明确。
- 每个行为变化都有先失败后通过的测试证据，或有确认过的 TDD exception 和替代验证证据。

---

## 17. 失败与恢复

测试无法先失败：

- 记录实际原因。
- 判断测试是否写错或现有功能已满足。
- 必要时回到 `plan-confirm` 修改计划。

实现后测试仍失败：

- 继续在计划范围内修复。
- 如果发现计划错误或范围扩大，停止并回到 `plan-confirm`。

质量检查命令不可用：

- 记录命令和失败原因。
- 使用项目可用的替代验证。
- 不把未运行检查标记为通过。

---

## 18. 参考资料

- [test-driven-development/SKILL.md](../../reference/skills/test-driven-development/SKILL.md)
- [test-driven-development/testing-anti-patterns.md](../../reference/skills/test-driven-development/testing-anti-patterns.md)
- [executing-plans/SKILL.md](../../reference/skills/executing-plans/SKILL.md)
- [using-git-worktrees/SKILL.md](../../reference/skills/using-git-worktrees/SKILL.md)
