# tdd-execute Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `tdd-execute` 如何校验已确认计划、执行 Red-Green-Refactor、处理 TDD 例外、按 Artifact Policy 记录执行证据、更新 handoff 和提出 review 建议。

---

## 1. 定位

`tdd-execute` 是 TDD 执行和质量检查 skill。

它只适用于进入 Doc Loom 持久化 case workflow、且已有已确认 `plan.md` 的任务。

普通一次性 coding 任务不强制进入 `tdd-execute`。纯文档修改、无复杂验证的低风险任务也可以跳过 `tdd-execute`，直接由相应编辑路径和 `doc-sync-close` 收尾。

默认先写失败测试，再做最小实现和重构。无行为变化重构、TDD exception 和手动验证场景使用记录化替代验证，不强造无意义 Red。

核心规则：

```text
没有 approved plan，不准执行。
默认没有失败测试，不准实现。
如果没有亲眼看到测试因预期原因失败，就不能声称完成了 TDD。
用户明确批准计划和执行是执行授权的最高依据；不引入 checksum / digest / 指纹批准机制。
进入持久化 case workflow 后，用户批准仍必须写回 plan.md 的最小批准记录。
执行阶段可以更新 plan.md checkbox / status，但不能静默改变计划语义。
执行应按可原子提交的边界组织；实际 stage / commit 只有在 plan 或用户明确要求时执行。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: tdd-execute
description: Execute an approved persistent Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan with plan_version, risk_level, and base_commit, or after a minimal approval writeback in the same turn. Handles red-green-refactor, TDD exceptions, adaptive execution amendments, implementation, quality checks, execution evidence, handoff, case_state.yaml updates, deviations, review recommendations, and authorized atomic commits.
---
```

---

## 3. 触发条件

应该触发：

- 用户确认计划后要求执行。
- 已有 approved plan 且任务处于 `planned` / `executing`。
- `review` 要求返工，回到执行阶段。
- 用户要求继续执行某个未关闭或可恢复的持久化 case。

不应该触发：

- `plan.md` 未确认。
- `plan_version` 未确认，或 `status` 不是 `approved`。
- 用户只是要求 review。此时使用 `review`。
- 用户只是要求收尾。此时使用 `doc-sync-close`。
- 普通一次性 coding 任务，除非用户明确启动 Doc Loom case workflow。
- 纯文档修改且没有复杂验证、执行偏离或恢复需求。

---

## 4. 输入

- `docs/cases/<case-id>/plan.md`。
- `docs/cases/<case-id>/case_state.yaml`。
- `Context & Authority Brief`，仅当计划引用、高风险、恢复、冲突或 authority / public contract / workflow 相关时读取。
- 相关代码和测试。
- 项目命令规范和包管理规范。
- `plan.md ## Goal`、`## Non-goals`、`## Decisions`、`## Acceptance Criteria`、`## TDD Applicability`、checkbox / step status。
- 可选：`review.md` 中的返工项，尤其是 `Verdict: Needs Changes`。
- 可选：worktree / branch baseline 检查结果。
- 可选：dirty workspace 差异，包括 staged、unstaged 和 untracked。

---

## 5. 输出

必须更新：

```text
docs/cases/<case-id>/case_state.yaml
```

按需更新：

```text
docs/cases/<case-id>/plan.md          # checkbox / status / Plan Amendments，不改变已确认计划语义
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
- 如果用户本轮说“批准并执行”但 `plan.md` 仍是 `draft`，先完成最小 `plan-confirm` 批准写回，再进入执行。
- 不做 checksum / digest / 内容指纹校验。
- 当前任务不是不可恢复的 closed 状态；`Done` 后的小范围同源 follow-up 可以继续，但必须保留 closure 历史并记录 amendment / follow-up。
- 当前工作区与 `base_commit` 差异可解释，检查范围包括 staged、unstaged 和 untracked。
- 如果 `base_commit` 不可用，用文件状态、计划版本和执行前文件摘要作为替代 baseline。
- TDD 适用性已声明。
- `Non-goals` 和 `Decisions` 已读取，并作为执行边界。
- 如果存在未关闭的 `review.md` 且 verdict 是 `Needs Changes`，本次执行优先处理返工项。
- 如果缺少 `case_state.yaml`，可从 `plan.md` 最小补齐状态缓存并记录来源；如果状态缓存与 Markdown 冲突，先报告 `state_cache_conflict`，以 Markdown + git 推导。

如果任一校验失败：

- 不执行。
- 输出阻塞原因。
- 建议回到 `plan-confirm`。

如果问题只是状态缓存缺失或 draft 计划已被用户本轮明确批准，可以在同一轮完成最小补写后继续，不需要重跑完整流程。

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

如果任务是新功能、bug fix、重构或行为变化，默认必须 TDD。例外需要用户或已确认计划允许，并记录替代验证证据。

例外记录位置按 Artifact Policy 决定。存在代码 / 行为变化、偏离、失败重试、review 建议或恢复需求时写 `execution.md`；纯文档、trivial config、UI 文案等低风险任务如果验证证据可完整写入 `closure.md`，可以不单独生成 `execution.md`。

用户可以明确要求非 TDD 执行，但必须成为已确认的 TDD exception：

```md
## TDD Applicability

- TDD Required: No
- If No, Reason: User explicitly requested non-TDD execution.
- Alternative Verification:
  - ...
```

如果执行中从 `TDD Required: Yes` 改为 `No`，这是测试策略实质变化，必须先更新计划或 Plan Amendment 并确认。

无行为变化重构不强行制造失败测试。计划应采用：

```text
characterize existing behavior -> refactor -> verify no regression
```

手动验证只能用于已确认的 TDD exception 或无法可靠自动化的验证点，必须记录步骤、环境、结果和限制。

---

## 9. Adaptive Execution

默认按已确认计划执行。用户明确要求“边执行边改计划”或“你看着办”时，可以进入轻量 adaptive execution。

最小记录方式是在 `plan.md` 中维护：

```md
## Plan Amendments
| When | Change | Reason | User Confirmation | Impact |
|---|---|---|---|---|
```

规则：

- 低风险、同目标、同责任边界的小改，可以记录 amendment 后继续。
- 小改不递增 `plan_version`。
- 新增相邻测试文件、同模块 helper、小型 fixture 可以作为小改。
- “你看着办”只授权低风险实现细节和同目标小改。
- 改变目标、验收标准、风险等级、TDD 策略、public contract、authority 影响、跨模块边界、依赖 / 配置或文件责任边界时，必须暂停并重新确认；必要时回到 `plan-confirm` 并递增 `plan_version`。
- adaptive execution 不引入额外审批文件、编号目录、digest 或复杂控制流。

---

## 10. 核心工作流

### Step 0. Review Plan Critically

执行前先读完整 `plan.md`，检查：

- 步骤是否清楚到可以执行。
- 文件路径和命令是否存在。
- TDD 步骤是否真实可跑。
- `Non-goals` 是否会被触碰。
- `Decisions` 是否可执行。
- acceptance criteria 是否能映射到测试、替代验证或收尾确认。
- 是否有阻塞问题或计划漏洞。

如果计划有关键缺口，不要猜测执行，回到 `plan-confirm`。

如果当前在主分支或高风险任务没有隔离 workspace，应建议用户使用 worktree / branch 隔离。skill 不擅自创建 worktree，除非用户明确确认；如果已存在 worktree，必须记录路径和 baseline。

计划步骤只需要“足够执行”，不做机械粒度审查。步骤较粗但目标、文件边界、验证命令和验收标准清楚时，可以执行；模糊到无法判断改哪里、测什么或如何验收时，回到 `plan-confirm`。

### Step 1. Red

如果 TDD required：

- 先写失败测试。
- 运行最小测试命令。
- 确认失败原因符合预期。
- 记录到 `execution.md`。

Red 记录粒度：

- 测试文件 / 测试名。
- exact command。
- 预期失败断言或错误类型。
- 实际失败摘要。
- 只在失败复杂或容易误判时保留关键日志片段，不复制长日志。

失败原因不符合预期：

- 不继续实现。
- 调整测试或回到 `plan-confirm`。

如果新增测试第一次运行就通过：

- 停止实现。
- 判断测试是否写错，或现有行为是否已经满足 acceptance criteria。
- 测试没覆盖目标时，重写测试并重新 Red。
- 现有行为已满足时，记录 Red 不成立，回到计划判断或进入收尾。

允许用现有失败测试作为 Red，但必须证明它针对当前目标，不是 unrelated baseline failure 或 flaky failure。

一个综合 failing test 可以覆盖多个紧密耦合的小行为，但必须映射到 acceptance criteria，且失败原因可定位。独立行为、不同风险或难以定位的失败应拆分 Red。

目标测试 flaky 时，必须稳定测试或回计划调整验证策略；无关 flaky 只记录 caveat 或建议新 case，不混入当前任务。

测试反模式检查：

- 不测试 mock 自身存在。
- 不为测试往生产类加 test-only 方法。
- 不在不了解依赖副作用时随意 mock。
- mock 响应必须匹配真实结构，不能只补当前断言用到的字段。
- mock 设置比测试逻辑还复杂时，优先考虑更真实的集成测试。

如果发现计划中的测试策略存在反模式：

- 同一测试目标下可改成真实行为断言的，作为 minor deviation 修正并记录。
- 需要换测试层级、引入新工具、改变验证口径或扩大环境时，回到 `plan-confirm`。

### Step 2. Green

- 写最小实现。
- 运行目标测试。
- 确认新增或修改测试通过。
- 记录实现摘要。

Green 阶段不做计划外重构。发现必须扩大范围才能通过测试时，停止并判断是否需要回 `plan-confirm`。

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

- 使用项目本地指定的包管理器、脚本入口和命令约束。
- 例如当前仓库要求前端使用 `pnpm`，Python 使用 `uv`，禁止 npm / yarn / pip / conda。

如果项目要求 shell 命令加代理前缀，例如 `rtk`，所有 shell 命令必须遵守。

命令记录必须包含：

- exact command。
- expected result。
- actual result。
- 如果未运行，说明原因和替代验证。

`Commands Run` 不需要记录普通探索命令、文件阅读或搜索。必须记录：

- 测试、lint、typecheck、build、smoke。
- 关键 baseline / status 命令。
- 失败复现命令。
- 影响执行判断或恢复的验证命令。

质量检查以 `plan.md` 为主。`tdd-execute` 可以补充本项目显然存在、低风险、只读的检查命令，并记录为 minor deviation。会新增依赖、改变环境、耗时很高、触发外部服务或有副作用的检查必须先得到用户或计划授权。

用户可以明确跳过某项质量检查，但必须记录 caveat；高风险检查、public contract、migration、安全或权限相关验证被跳过时，需要明确风险确认。

外部服务 / 网络 smoke test 不默认引入。只有计划列出、项目已有命令需要或用户明确授权时才运行，并记录环境、时间、目标、是否产生数据 / 成本和副作用风险。

依赖检查不是固定前置。命令因缺依赖失败时，记录失败并按项目包管理规范处理；安装依赖或修改 lockfile 只有计划或用户授权时才做。

生成文件、snapshot、lockfile 变化不能静默接受。计划内或工具语义预期内的变化记录后继续；非预期变化先判断来源和风险，lockfile 变化尤其需要确认，除非计划明确包含依赖更新。

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

`execution.md` 正文按最新结果维护，底部保留极简 `## History`。不要默认创建 `executions/` 编号目录，也不要纯覆盖关键 Red / Green / failure / deviation 证据。

执行阶段应更新 `plan.md` 的 checkbox / step status：

- 允许把 `[ ]` 改为 `[x]`。
- 允许添加简短状态和 `execution.md` 引用。
- 不允许改变目标、步骤语义、命令、验收标准、TDD 策略、Decisions 或文件范围。
- checkbox / status 更新不算计划实质变化，不递增 `plan_version`。

`execution.md` 是执行证据主记录；`plan.md` checkbox 是轻量进度提示。二者冲突时，以 `execution.md` + git 状态为准，并报告冲突。

### Step 6. Update Handoff

只有存在未来恢复点时才覆盖 `handoff.md`，保持短、准、适合接续。

部分执行、等待用户、等待 review、阻塞、暂停或跨会话继续时应更新短 `handoff.md`。如果 `plan.md` checkbox 和 `execution.md` 已能快速说明下一步，可跳过 `handoff.md`。

### Step 7. Review Recommendation

根据风险输出 review 建议，但不自动触发 review。

`Ready for Review` 和 `Review Recommendation` 语义分开：

- `Ready for Review` 表示实现和执行记录是否达到可审查状态。
- `Review Recommendation` 表示是否建议用户触发 review。

review recommendation 不阻塞进入 `doc-sync-close`。但高风险任务建议 review 而用户未执行时，收尾必须记录 caveat，通常不应无条件标记 `Done`。

### Step 8. Acceptance Criteria Check

完成前必须逐项核对 `plan.md ## Acceptance Criteria`：

```text
met / partially met / not met / not verified
```

每项都应有测试、替代验证、代码 diff、用户确认或 review 证据。无法由 `tdd-execute` 验证的产品、文档或体验条件，标为 `not verified`，交给用户、review 或 `doc-sync-close`；不能默认为通过。

### Step 9. Authorized Atomic Commit

默认不自动 `git add`、`git commit` 或创建 checkpoint。

只有当 `plan.md` 明确要求提交，或用户本轮明确要求提交时，才执行 stage / commit。执行时：

- 按原子边界组织提交。
- 只 stage 当前 case 相关文件。
- 不纳入用户无关 dirty diff。
- 不 stash / revert 用户改动，除非用户明确要求。
- 每个原子 commit 前至少运行相关最小测试；整体验证可在全部 commit 后运行。
- commit message 遵守项目规范；Doc Loom 不定义格式。
- 如果实际创建 commit，在 `execution.md` 记录 commit hash。

原子边界优先对齐 plan checklist 或 TDD cycle，但以“可独立理解、可独立验证、可回滚”为准。多个 TDD cycle 如果共同完成一个不可分割行为，可以 squash 成一个 commit；独立行为应拆开。

大范围格式化和行为改动通常分开提交。只影响本次改动附近的局部格式化可以随原子行为提交。

commit hook / pre-commit 失败时，只在计划范围内修复。若 hook 修改大量无关文件、要求安装依赖、暴露计划外问题或触发外部服务失败，停止并记录，必要时回计划或询问用户。

---

## 11. execution.md 模板

仅在 Artifact Policy 或触发条件要求时生成。

```md
# Execution Report

## Plan Reference
- plan_version:
- base_commit:
- adaptive_execution: Yes / No

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

## Acceptance Criteria Status
| Criteria | Status | Evidence |
|---|---|---|

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

## Checkpoints / Commits
| Commit | Scope | Verification |
|---|---|---|

## History
| When | Previous Status | Reason Updated |
|---|---|---|
```

---

## 12. handoff.md 模板

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

## 13. case_state.yaml 更新

执行中：

```yaml
phase: executing
closure_status: open
```

执行完成，准备收尾：

```yaml
phase: doc_syncing
closure_status: open
```

等待 review 或返工时才写 review 状态：

```yaml
phase: executing
review_status: recommended
```

review 返工完成但未复审：

```yaml
phase: executing
review_status: addressed
```

`review_status` 只在影响路由时进入 `case_state.yaml`。普通 recommendation 留在 `execution.md` 或 `closure.md`。

如果 `case_state.yaml` 更新失败，不回滚已完成实现，但不能宣称 workflow 完成；必须记录失败原因并提示修复。

---

## 14. Deviation 规则

轻微偏离：

- 文件名变化但目标不变。
- 测试命令调整。
- 小范围实现路径变化。
- 同目标内新增相邻测试、fixture 或 helper。
- 同目标内修正测试反模式。
- adaptive execution 下的低风险 Plan Amendment。

处理：

- 记录到 `execution.md`。
- 可继续。
- 不递增 `plan_version`。

严重偏离：

- 目标变化。
- 风险等级升高。
- 触及未计划的 public API / auth / data migration。
- 新增或改变 authority / workflow / public contract / agent behavior 的长期事实需求。
- TDD exception 从 No 变 Yes 或从 Yes 变 No。
- 计划步骤无法执行，需要重写任务分解。
- 触及 `Non-goals`。
- 违反 `Decisions`。
- 新增跨模块边界、依赖 / 配置、migration 或高风险文件范围。

处理：

- 停止执行。
- 回到 `plan-confirm`。

如果只是执行后自然产生 authority update proposal，不改变当前执行语义，则继续执行，把 proposal 留给 `doc-sync-close`。

---

## 15. Review Recommendation

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

如果 `review.md` verdict 为 `Needs Changes`，`tdd-execute` 应优先处理 blocking / important findings，并在执行记录中映射 finding 与修复。返工完成后可以把 `review_status` 标为 `addressed`，不能标为 `approved`；是否 approved 只能由新的 review 或用户明确确认决定。

---

## 16. Gate

- 没有 approved plan，不执行。
- 当前 `plan_version` 未被确认，不执行。
- TDD required 但未写失败测试，不实现。
- 新测试如果第一次运行就通过，必须重写测试或说明已有行为已经满足，并回到计划判断。
- 测试失败原因不是预期原因，不进入实现。
- TDD exception 没有替代验证，不执行。
- 严重偏离计划，不继续执行。
- 未记录命令结果，不宣称验证完成。
- 不能通过断言 mock 存在来证明真实行为。
- 触及 `Non-goals` 或违反 `Decisions` 时，不继续执行。
- 缺少 `TDD Applicability` 时，不由执行阶段无记录推断；先做最小计划修订。
- 修改项目脚本、依赖、lockfile、CI 或测试命令配置，必须在计划或用户授权范围内。
- `tdd-execute` 不修改 authority 文档。
- 默认不修改 L3 derived / index 文档，除非计划明确包含。
- 默认不 stage / commit；只有计划或用户明确要求时才按原子边界提交。

---

## 17. 验收标准

- 测试按计划新增或更新。
- 至少相关测试通过。
- 质量检查结果记录完整。
- 当 Artifact Policy 要求时，`execution.md` 有 Red / Green / Refactor 或明确 TDD exception。
- 当存在未来恢复点时，`handoff.md` 能说明下一步。
- `case_state.yaml` 与执行状态一致。
- review recommendation 明确。
- 每个行为变化都有先失败后通过的测试证据，或有确认过的 TDD exception 和替代验证证据。
- `plan.md` checkbox / status 反映最新执行进度，且不改变计划语义。
- acceptance criteria 已逐项标记 `met / partially met / not met / not verified`。
- adaptive execution 的小改记录在 `Plan Amendments`。
- 如果创建 commit，commit 按原子边界组织，且 `execution.md` 记录 commit hash。

---

## 18. 失败与恢复

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

计划范围内允许多次尝试，但需要停止条件：

- 同一个 Red / Green 问题连续两三次失败仍无法解释。
- 失败原因指向计划外模块、风险或边界。
- 修复需要改变目标、测试策略、依赖、配置或 public contract。

此时停止并回到 `plan-confirm` 或请求用户确认。

目标测试通过但质量检查失败时：

- 可以记录 implementation complete。
- 不能记录 verification complete。
- 收尾时通常只能是 `Done with Caveats` 或 `Blocked`，不能无条件 `Done`。

baseline tests 已失败时：

- 与本任务无关、已有且可解释的失败，记录后可继续目标最小测试。
- 影响目标测试、相关模块或质量检查可信度，或无法判断来源时，停止并确认风险。

closed case follow-up：

- `Done` 后的小范围同源修改可以复用原 case，记录 amendment / follow-up。
- `Cancelled`、`Superseded`、`Abandoned` 默认不恢复，应新开 case 或重新确认。
- `Paused`、`Blocked`、`Done with Caveats` 在用户明确要求且恢复条件满足时可继续。

---

## 19. 参考资料

- [test-driven-development/SKILL.md](../../reference/skills/test-driven-development/SKILL.md)
- [test-driven-development/testing-anti-patterns.md](../../reference/skills/test-driven-development/testing-anti-patterns.md)
- [executing-plans/SKILL.md](../../reference/skills/executing-plans/SKILL.md)
- [using-git-worktrees/SKILL.md](../../reference/skills/using-git-worktrees/SKILL.md)
