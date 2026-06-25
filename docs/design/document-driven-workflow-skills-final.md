# 文档驱动开发工作流 Skill 设计（优化最终版）

> 目标：沉淀一套以文档为中心、由任务上下文驱动、通过 TDD 执行并回写文档的工作流 Skill System。
>
> 关键理念：历史文档不是直接被继续维护的对象，而是用于抽取事实的证据库；`setup-doc-governance` 的目标是用固定分层标准生成一份可确认的治理计划，再执行整合、迁移、桥接、归档和按需 authority 重建。

> 本版状态：已吸收评审优化意见，重点增强了固定治理标准、独立治理批次计划、风险分级、TDD 例外、计划确认版本绑定、机器可读任务状态、异常收尾状态和 authority 按需创建规则。

---

## 0. 本版关键优化

相比初版，本版增加以下落地机制：

```text
1. 新增 docloom-workflow 作为 public automatic entry，但保持 thin router，不做重型 orchestrator。
2. setup-doc-governance 使用单一固定流程，通过 `scope: current-case | docs-only | full-repo` 控制扫描范围。
3. TDD 是默认纪律，但允许记录化例外，避免纯文档、配置、Spike、紧急修复被流程卡死。
4. plan-confirm 增加 risk_level、plan_version、base_commit 和 `## Decisions`，确保确认对象可追溯。
5. case_state.yaml 瘦身为机器可读状态缓存，不复制 plan.md 的权威字段。
6. review 是用户显式触发的只读、对话式证据审查能力，不自动执行、不产物、不路由；风险只通过 `review_risk` 信号暴露给 `docloom-workflow`。
7. closure 状态补充 Cancelled / Superseded / Paused / Abandoned，覆盖真实任务中断场景。
8. Authority Order 拆分为 Execution Instruction Order 和 Fact Authority Order，避免把用户临时指令误写为长期事实。
9. setup-doc-governance 使用治理计划作为每个独立治理批次的确认产物，文件级和事实级都用 `promote / merge / bridge / archive / block` verdict。
10. 产物生成改为 Artifact Policy：handoff、context brief、execution 等按条件生成，不再每阶段固定铺满文件。
11. Doc Loom Least v1 不依赖 CLI backend，不调用 `.agents/doc-loom/bin/doc-loom`，只操作文档产物。
12. `tdd-execute` 只服务持久化 case workflow；普通一次性 coding 和简单纯文档任务不强制进入。
13. 执行阶段支持轻量 adaptive execution，用 `plan.md ## Plan Amendments` 记录同目标小改，避免额外审批产物。
14. 执行结果应按可原子提交的边界组织；实际 stage / commit 只有在 plan 或用户明确要求时执行。
```

---

## 1. 最终 Skill 列表

最终拆解为 **1 个入口 Skill + 5 个主流程 Skill + 2 个手动通用辅助 Skill**。其中 `docloom-workflow` 是公开自动入口和轻量路由器；`setup-doc-governance` 使用固定治理标准和一次确认流程；`review` 是只读、对话式证据审查能力；`grill` 是交互式挑战能力。二者都不属于 Doc Loom workflow 阶段，不进入 Artifact Policy：

```text
docloom-workflow       # public automatic entry / thin router
setup-doc-governance
context-authority
plan-confirm
tdd-execute
doc-sync-close

review                # 手动通用辅助 / 证据审查，不产物、不路由
grill                 # 通用手动辅助，不进入 workflow 路由或 Artifact Policy
```

| Skill | 类型 | 默认执行 | 负责什么 |
|---|---|---:|---|
| `docloom-workflow` | 入口 / 路由 | 是 | 作为 public automatic entry，读取最小 workspace / case 状态，延迟创建 case，维护 Artifact Policy，并路由到具体 skill；不替代具体 skill、不自动执行 |
| `setup-doc-governance` | 初始化 / 周期性治理 | 否 | 以固定分层标准从历史文档、当前文档、代码和测试中抽取事实，写出治理计划，经用户一次确认后执行整合、迁移、桥接、归档和按需 authority 更新 |
| `context-authority` | 主流程 | 按需 | 在需要计划前上下文 gate、恢复任务、权威判断或冲突判断时，读取最小相关上下文并输出路由 verdict |
| `plan-confirm` | 主流程 | 是 | 生成计划、标记风险等级、绑定计划版本、等待用户确认、落计划文档 |
| `tdd-execute` | 主流程 | 条件 | 按确认后的持久化 case 计划做 TDD 执行、测试、质量检查、执行记录；必要时记录 TDD 例外、Plan Amendments 和授权的原子提交 |
| `doc-sync-close` | 主流程 | 是 | 文档同步、权威文档更新确认、任务关闭 |
| `review` | 通用辅助 / 证据审查 | 否 | 用户显式触发时，对设计文档、方案、diff、测试、文档变更或 case evidence 做只读对话审查；输出 assessment、findings 和 evidence gaps；不写文件、不改状态、不路由 |
| `grill` | 通用辅助 / 交互拷问 | 否 | 用户显式触发时，通过对话逐问挑战任意主张；不写文件、不输出流程 verdict、不进入 Artifact Policy |

本版新增的跨 Skill 机制：

```text
risk_level             low | medium | high
plan_version           每次计划变更递增
plan_decisions         用户明确确认并写入 plan.md 的执行约束
base_commit            计划确认时的代码基线
tdd_applicability      是否适合严格 TDD
case_state.yaml        机器可读任务状态
artifact_policy        case 级产物生成策略
review_risk            low | medium | high，只作为 docloom-workflow 可消费的风险信号
closure_status         Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned
adaptive_execution     是否允许执行中轻量修订计划
plan_amendments        执行中同目标小改记录
governance_scope       current-case | docs-only | full-repo
governance_verdict     promote | merge | bridge | archive | block
```

---

## 2. 总体流程

### 2.1 一次性 / 周期性治理流程

```text
setup-doc-governance
  ↓
选择 scope：current-case / docs-only / full-repo
  ↓
盘点相关文档、入口、历史材料，必要时读取代码和测试作为证据
  ↓
抽取事实并按固定分层标准路由
  ↓
生成治理计划
  ↓
用户一次确认治理决策表
  ↓
执行非 block 项：按需创建 authority、merge/promote facts、bridge、archive、更新入口索引
  ↓
写回 applied result 和 blocked decisions
```

治理流程只有一套，扫描范围由 `scope` 控制：

```text
current-case
  只治理当前 case 相关材料。

docs-only
  默认范围。扫描 docs、README、ADR、入口索引和历史文档。

full-repo
  在 docs-only 基础上读取相关代码和测试作为事实证据，但不修改代码和测试。
```

### 2.2 需要文档驱动计划的开发任务默认流程

```text
docloom-workflow
  ↓
解析用户意图、workspace、已有 case 和状态
  ↓
context-authority（需要上下文 gate 时）
  ↓
plan-confirm
  ↓
用户确认计划
  ↓
tdd-execute
  ↓
doc-sync-close
```

### 2.3 通用手动辅助

`grill` 可以由用户在任意时刻显式触发，用于通过对话挑战当前主张、需求、计划、架构、测试策略或文档想法。

它不是 Doc Loom workflow 阶段：

```text
- 不由 docloom-workflow 路由。
- 不进入 Artifact Policy。
- 不生成 grill.md。
- 不修改 plan.md、case_state.yaml 或 authority 文档。
- 不输出流程 verdict 或 next owner。
```

如果用户在 `grill` 讨论中明确确认了会影响执行的决策，后续 `plan-confirm` 可以把这些决策写入 `plan.md` 的 `## Decisions`。最终 authority 文档变更由 `doc-sync-close` 或文档治理流程处理。

#### 用户手动发起 review

用户可以在任意时刻显式请求 `review`，用于对设计文档、方案、diff、测试、文档变更或 case evidence 做只读证据审查。

`review` 只在对话中输出 assessment、findings 和 evidence gaps。它不产生持久化审查文件，不修改 `case_state.yaml`，不输出 workflow route，也不决定下一阶段。所有路由仍由 `docloom-workflow` 根据用户意图、case 状态、证据和风险信号判断。

---

## 3. 全局 Workflow Protocol

保留全局协议，同时新增 `docloom-workflow` 作为 thin entry/router。它只做路由、状态推导、延迟 case 创建和 Artifact Policy 判定，不替代具体阶段 skill，不维护中心任务索引，也不成为复杂 pipeline。

### 3.1 核心原则

```text
1. 历史文档是证据，不是默认权威。
2. 标准权威文档体系是治理后的结果，不是在旧结构上修修补补。
3. setup-doc-governance 使用单一固定流程；无 scope 时默认 `docs-only`，必要时升级到 `full-repo` 读取代码和测试证据。
4. 当前任务上下文可以来自 branch/worktree，也可以来自用户指定或 case docs。
5. branch/worktree 是推荐隔离机制，不是强制前置条件。
6. agent 不应擅自创建 branch 或 worktree，除非用户明确要求或确认。
7. docloom-workflow 是默认公开自动入口；用户显式点名具体 skill 时，直接进入该 skill。
8. 没有 context，不准计划。
9. 默认没有用户确认计划，不准执行；Doc Loom Least v1 不支持 low-risk auto execution。
10. TDD 是默认纪律；无法先写失败测试时，必须记录 TDD exception 和替代验证方式。
11. 执行偏离计划，必须记录；严重偏离，必须重新确认。
12. plan-confirm 必须绑定 plan_version 和 base_commit。
13. case_state.yaml 是瘦身状态缓存，不是唯一真相。
14. docloom-workflow 持有 Artifact Policy，各阶段 skill 负责写自己的产物。
15. Authority 更新必须用户确认；窄 authority patch 可由 `doc-sync-close` 在 explicit confirmation 后执行，结构性、高风险或冲突型 authority 更新必须通过治理计划一次确认；未确认原始材料不得写入 authority。
16. 有 case docs 的任务结束或中断必须写 closure.md；无 case docs 的一次性任务不强制补 closure。
17. grill 是流程无关的通用手动辅助 skill，不进入 Artifact Policy，不由 workflow 路由，不写文件。
18. review 只能由用户手动触发或明确要求；它是只读对话审查，不产物、不改状态、不路由。
19. `tdd-execute` 只适用于持久化 case workflow；普通一次性 coding 和简单纯文档修改不强制进入。
20. 执行阶段可以更新 `plan.md` checkbox / status 和轻量 `Plan Amendments`，但不能静默改变计划语义。
21. 执行应保持改动可原子提交；实际 stage / commit 需要 plan 或用户明确授权。
22. Doc Loom Least v1 不依赖 CLI backend，不调用 `.agents/doc-loom/bin/doc-loom`。
```

### 3.2 风险等级与计划确认策略

每个任务在 `plan-confirm` 阶段必须标记风险等级：

```text
low
  小范围、低影响、易回滚的改动。
  示例：文档修正、局部文案、非关键测试补充、无行为变化的重构。

medium
  普通功能开发或局部行为变更。
  示例：新增业务逻辑、修改内部 API、更新局部数据流。

high
  高影响、难回滚、涉及外部契约或敏感领域的改动。
  示例：权限、安全、计费、数据删除、public API、schema migration、认证、隐私。
```

Doc Loom Least v1 的 skill-defined 策略：

```yaml
plan_confirmation_policy:
  low_risk_changes: require_confirmation
  medium_risk_changes: require_confirmation
  high_risk_changes: require_explicit_confirmation
```

普通项目文档、authority 文档或入口索引不能放宽这个 gate。`low_risk_changes: auto` 不进入 v1；如未来支持，必须由 skill-owned 配置机制明确设计。

### 3.3 TDD 适用性与例外机制

默认执行 Red-Green-Refactor，但以下任务可以记录 TDD exception：

```text
- 纯文档修改
- 配置调整
- 构建脚本修复
- 日志 / telemetry 改动
- UI 文案
- 删除死代码
- 探索性 spike
- 紧急 hotfix
- 无法可靠自动化验证的环境类改动
```

例外不是跳过验证，而是必须记录替代验证方式：

```md
## TDD Applicability

- TDD Required: Yes / No
- If No, Reason:
- Alternative Verification:
  - manual check
  - snapshot check
  - build check
  - smoke test
  - reviewer confirmation
```

TDD exception 的证据位置由 Artifact Policy 决定。存在代码 / 行为变化、偏离、失败重试、material review_risk 或恢复需求时写 `execution.md`；纯文档、trivial config 或低风险 UI 文案等任务，如果验证证据可完整放入 `closure.md`，可以跳过独立 `execution.md`。

无行为变化重构不强行制造失败测试，采用 characterization / existing behavior lock → refactor → verify no regression。用户明确要求非 TDD 执行时，必须作为已确认 TDD exception 或 Plan Amendment 记录，并保留替代验证。

### 3.4 计划确认绑定规则

`plan-confirm` 写入 `plan.md` 时必须包含：

```yaml
---
case_id: 20260617-auth-refresh-flow
plan_version: 1
status: draft | approved
risk_level: low | medium | high
approved_by:
approved_at:
base_commit:
---
```

规则：

```text
- 每次计划内容实质变化，plan_version 必须递增。
- 用户确认的执行决策变化属于计划内容实质变化。
- 用户确认后，记录 approved_by、approved_at 和 Confirmation Log。
- tdd-execute 必须引用被确认的 plan_version。
- 执行阶段更新 checkbox / status 不属于计划实质变化。
- adaptive execution 下低风险、同目标、同责任边界的小改进入 `Plan Amendments`，不递增 `plan_version`。
- 目标、验收标准、风险等级、TDD 策略、public contract、authority 影响或文件责任边界变化时，必须重新确认并递增 `plan_version`。
- 如果 base_commit 后出现无法解释或影响执行语义的 diff，必须重新评估上下文。
```

### 3.5 机器可读任务状态

创建 case docs 时维护：

```text
docs/cases/<case-id>/case_state.yaml
```

示例：

```yaml
case_id: 20260617-auth-refresh-flow
phase: executing
plan_version: 2
closure_status: open
last_updated: 2026-06-17T11:00:00+09:00
artifacts:
  context_brief: inline
  handoff: not_needed
  execution: conditional
  closure: required
```

定位：

```text
- Markdown 文档仍是人类可读主记录。
- case_state.yaml 是机器可读状态缓存。
- case_state.yaml 只记录本 case 的状态和产物决策，不复制完整 Artifact Policy。
- risk_level、base_commit 默认属于 plan.md。
- 如果 case_state.yaml 与 Markdown 冲突，必须报告并以 Markdown + git 状态重新推导。
```

### 3.6 Artifact Policy

`docloom-workflow` 持有全局 Artifact Policy，各阶段 skill 负责写自己的产物。

| Artifact | Required When | Optional / Skip When | Writer |
|---|---|---|---|
| `plan.md` | case enters `plan-confirm` | no persistent case workflow | `plan-confirm` writes plan; `tdd-execute` may update checkbox/status and Plan Amendments |
| `case_state.yaml` | case docs are created | status-only without case creation | `docloom-workflow` initially; stage skills update |
| `context-authority-brief.md` | high risk, conflict, resume, multi-agent / cross-session, explicit request | inline `Context Sources` otherwise | `context-authority` |
| `handoff.md` | future resume point exists | continuous same-session flow | current stage skill |
| `execution.md` | TDD required, code / behavior change, plan deviation, failed/retried tests, material review_risk, resume needed | docs-only, trivial config, verification fits in closure | `tdd-execute` |
| `closure.md` | case docs exist and task ends or pauses / blocks / cancels | no case docs one-shot task | `doc-sync-close` |

`review` 和 `grill` 不在 Artifact Policy 中。它们只输出对话内容，不写 case artifact。

### 3.7 Handoff 写入规则

```text
handoff.md
  - 只在存在未来恢复点时生成或更新。
  - 由当前阶段 skill 写入，因为当前阶段最知道下一步和注意事项。
  - 必须短、准、适合接续任务。

execution.md
  - 记录 TDD / 行为变更 / 偏离 / 复杂验证证据。
  - docs-only、trivial config 或验证证据可放入 closure 时可跳过。

closure.md
  - 保存最终结果。
  - 有 case docs 的任务结束、中断、取消、替代时必须写。
  - 正文按最新结果维护，保留 Follow-up Updates、Confirmation Log 和极简 History；默认不创建编号 closure 目录。
```

未来恢复点包括：

```text
- 用户要求稍后继续、交给别人、交给另一个 agent 或另一个会话。
- 计划已确认但不立刻执行。
- 执行已开始但未完成。
- 等待用户确认或外部依赖。
- 任务进入 Paused、Blocked。
- 高风险或跨模块任务需要恢复摘要。
```

### 3.8 Review Risk 信号

`review` 不自动执行。阶段 skill 只记录 `review_risk` 信号，供 `docloom-workflow` 在状态摘要和路由判断中消费：

```text
- risk_level = high
- 修改 public API
- 修改权限、安全、认证、计费、隐私逻辑
- 修改数据模型或 migration
- 测试覆盖不足
- 存在 authority 文档更新
- 执行中出现严重计划偏离
```

输出格式：

```md
## Review Risk

- Level: low / medium / high
- Reasons:
  - ...
- Evidence:
  - ...
```

`review_risk` 不是 review 授权，也不是 workflow gate。是否运行 `review` 只能来自用户明确请求。

### 3.9 异常收尾状态

`closure.md` 的最终状态支持：

```text
Done
Done with Caveats
Blocked
Cancelled
Superseded
Paused
Abandoned
```

含义：

```text
Done                 验收标准满足。
Done with Caveats    主目标完成，但有明确残留风险或后续项。
Blocked              因依赖、权限、信息不足等无法继续。
Cancelled            用户或项目决定取消。
Superseded           被另一个任务或方案替代。
Paused               暂停等待外部条件，未来可能恢复。
Abandoned            长期未继续，不建议直接恢复。
```

---

## 4. 任务上下文定位协议

### 4.1 三种运行模式

branch / worktree 映射是推荐机制，不是必选机制。

| 模式 | 是否需要新 branch | 是否需要新 worktree | 适用场景 |
|---|---:|---:|---|
| Isolated case Mode | 是 | 是 | 大任务、并行任务、高风险任务 |
| Branch case Mode | 是 | 否 | 普通开发任务 |
| Inline case Mode | 否 | 否 | 小改动、临时任务、已有分支、用户不想切分支 |

### 4.2 Mode A：Isolated case Mode

```text
dedicated worktree + dedicated case branch
```

示例：

```text
branch: case/20260617-auth-refresh-flow
worktree: ../worktrees/my-app/20260617-auth-refresh-flow/
case docs: docs/cases/20260617-auth-refresh-flow/
```

适合：

```text
- 中大型任务
- 多任务并行
- 风险较高的改动
- 需要长时间接续的任务
```

### 4.3 Mode B：Branch case Mode

```text
当前 repo + dedicated case branch
```

示例：

```text
branch: case/20260617-auth-refresh-flow
case docs: docs/cases/20260617-auth-refresh-flow/
```

适合：

```text
- 普通开发任务
- 不需要 worktree 隔离
- 但希望 branch 能清晰表达 case id
```

### 4.4 Mode C：Inline case Mode

```text
当前 workspace + 当前 branch
不要求 branch 符合 case/<case-id>
```

适合：

```text
- 小任务
- 临时任务
- hotfix
- 已有 feature branch
- 用户不想切分支或创建 worktree
```

此时 case id 可以来自：

```text
1. 用户明确指定的 case id
2. 用户明确指定的 case docs 目录
3. 已存在的 docs/cases/<case-id>/plan.md
4. docloom-workflow 在进入持久化 case workflow 时创建的新 case id
```

### 4.5 case Context Resolution Order

```text
1. 用户本轮明确指定的 case id / case docs / branch / worktree
2. 当前 worktree + case branch 可推导出的 case id
3. 当前 branch 可推导出的 case id
4. 当前目录下已有明确 case docs
5. 当前会话提出 proposed_case_slug，由 docloom-workflow 决定是否创建正式 case
6. 仍无法判断时，要求用户选择或确认
```

### 4.6 推荐 branch 命名

```text
case/<case-id>
```

示例：

```text
case/20260617-auth-refresh-flow
case/20260618-payment-retry-tdd
case/20260619-doc-governance-setup
```

### 4.7 由 branch 推导 case id

```text
branch: case/20260617-auth-refresh-flow
case_id: 20260617-auth-refresh-flow
case_docs: docs/cases/20260617-auth-refresh-flow/
```

### 4.8 推荐 worktree 路径

推荐放在 repo 外部：

```text
../worktrees/<repo-name>/<case-id>/
```

示例：

```text
../worktrees/my-app/20260617-auth-refresh-flow/
```

### 4.9 接续任务协议

当 agent 接续任务时，优先执行：

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
git diff --name-only
```

然后：

```text
1. 识别当前 workspace。
2. 读取当前 branch。
3. 尝试根据 case Context Resolution Order 解析 case_id。
4. 推导或确认任务目录：docs/cases/<case-id>/。
5. 读取任务文档：
   - plan.md
   - execution.md
   - handoff.md
   - closure.md，如存在
6. 检查 git status 和 git diff。
7. 判断下一步 workflow state。
8. 从最新未完成步骤继续。
```

---

## 5. 文档治理的核心理念

`setup-doc-governance` 的目标不是在历史文档结构上做修补，也不是搭建复杂治理流水线，而是用固定分层标准完成一次可审查的文档治理变更。

### 5.1 历史文档的定位

历史文档包括：

```text
- 旧需求文档
- 旧设计文档
- 会议记录
- 临时方案
- ADR 草稿
- 旧 README
- 旧接口说明
- 旧任务执行文档
- 旧 issue / PR 描述
- 旧调研文档
```

它们默认不是权威文档，而是证据材料。

```text
Historical docs = evidence
Authority docs = confirmed source of truth
Governance plan = one approval surface
```

### 5.2 setup-doc-governance 的真正产出

不是：

```text
给旧文档加几个 metadata
把旧目录整理一下
在原有结构上修修补补
```

而是：

```text
从历史材料中抽取仍然有效的事实
按固定分层标准为文件和事实给出 verdict
生成治理计划作为每个独立治理批次的确认产物
用户一次确认后执行整合、迁移、桥接、归档和 authority 更新
把未确认原始材料留在 case evidence 或 archive
把当前事实写入按需创建的 authority 文档
```

### 5.3 治理后的结构应该服务未来任务

治理后的文档体系应该让后续 agent 可以快速回答：

```text
当前产品事实是什么？
当前架构事实是什么？
当前 API 契约是什么？
当前数据模型是什么？
当前被接受的技术决策是什么？
哪些内容只是历史背景？
哪些内容仍待确认？
哪些文档修改需要用户确认？
哪些旧入口需要 bridge，哪些旧材料应 archive？
```

---

## 6. 标准权威文档体系

`setup-doc-governance` 应该建立一套按事实类型路由的 authority 体系，而不是沿用混乱的历史目录，也不是创建空目录或占位文档。

规则：

```text
Authority is modular and optional-by-need.
Create only the authority sections that have confirmed facts.
Do not create empty authority folders or placeholder documents.
```

推荐标准结构：

```text
docs/
  README.md

  governance/
    YYYY-MM-DD-<slug>.md

  authority/
    README.md

    product/
      goals.md
      scope.md
      requirements.md

    domain/
      glossary.md
      concepts.md
      invariants.md

    architecture/
      README.md
      overview.md
      components.md
      flows.md
      decisions/
        ADR-0001-<decision>.md

    contracts/
      cli.md
      api.md
      schema.md
      config.md

    workflow/
      case-lifecycle.md
      doc-governance.md
      review-policy.md

    agent/
      policy.md
      context-loading.md
      adapters.md

    operations/
      setup.md
      release.md
      runbooks/

  cases/
    <case-id>/
      evidence/
      case_state.yaml
      plan.md
      execution.md
      handoff.md
      closure.md

  derived/
    getting-started.md
    dev-guide.md
    examples.md
    troubleshooting.md

  archive/
    historical/
    superseded/
    raw/
```

### 6.1 `docs/authority/product/`

负责产品事实：

```text
- 产品目标、用户和成功标准
- 产品范围和非目标
- 已确认需求
- 验收标准
```

### 6.2 `docs/authority/domain/`

负责领域事实：

```text
- 领域概念
- 术语表
- 业务规则
- 不变量
- 状态流转
```

### 6.3 `docs/authority/architecture/`

负责架构事实：

```text
- 系统边界、组件和模块划分
- 关键数据流 / 控制流
- 集成点
- 已接受 ADR
```

### 6.4 `docs/authority/contracts/`

负责契约事实：

```text
- CLI 命令、参数和退出语义
- API / 协议契约
- schema 契约
- config / env 约束
```

### 6.5 `docs/authority/workflow/`

负责工作流事实：

```text
- case lifecycle
- 文档治理固定规则
- review boundary / confirmation gate
- 任务状态流转
```

### 6.6 `docs/authority/agent/`

负责 Agent 可执行上下文事实：

```text
- canonical agent policy
- context loading
- adapter source
- AGENTS / CLAUDE / Copilot / Cursor 派生关系
```

### 6.7 `docs/authority/operations/`

负责运行事实：

```text
- setup
- release
- runbook
- 维护规则
```

---

## 7. setup-doc-governance 的治理流程

### 7.0 Scope Selection / 治理范围选择

`setup-doc-governance` 不再区分 `minimal` / `full` 两种模式。它使用单一固定流程，通过 `scope` 控制扫描范围：

```text
current-case
  只治理当前 case 相关材料，例如 drafts、evidence、plan、closure 和局部迁移。

docs-only
  默认范围。扫描 docs、README、ADR、入口索引和历史文档。

full-repo
  在 docs-only 基础上读取相关代码和测试作为事实证据。
```

如果用户未指定 scope，默认使用 `docs-only`。只有用户明确要求，或 authority claim 需要代码 / 测试验证时，才升级读取相关代码和测试。

### 7.1 Phase 1：Inventory / 盘点

扫描并列出范围内的历史文档、当前文档、入口索引和 evidence。`full-repo` 可以额外读取相关代码和测试。

盘点结果不生成独立 inventory 产物，而是写入治理计划的决策表。

```md
## File Routing Decisions
| Source | Current Role | Verdict | Target | Reason | Requires Confirmation |
|---|---|---|---|---|---|
```

### 7.2 Phase 2：Fact Extraction / 事实抽取

从历史文档、当前文档、case evidence、代码和测试中抽取事实，而不是复制旧文档结构。

事实类型包括：

```text
requirement
domain
design
decision
implementation
test
debug
convention
risk
anti_pattern
process
evidence
```

事实记录格式：

```md
## Fact: Auth token refresh uses rotating refresh tokens

- type: design
- source:
  - docs/old/auth-design.md
  - src/auth/refresh.ts
  - tests/auth/refresh.test.ts
- evidence:
  - Historical design describes rotating refresh tokens.
  - Current code contains refresh token rotation logic.
- conflicts:
  - docs/api/auth.md still describes static refresh tokens.
- verdict: merge
- target_authority_doc:
  - docs/authority/architecture/flows.md
  - docs/authority/contracts/api.md
```

### 7.3 Phase 3：Route Files and Facts / 路由

对文件和事实分别给出 verdict。

统一 verdict：

```text
promote
merge
bridge
archive
block
```

文件级含义：

| Verdict | 含义 |
|---|---|
| `promote` | 源文件中的当前事实将被抽取到新 authority 文档 |
| `merge` | 源文件中的当前事实将合并到已有 authority 文档 |
| `bridge` | 保留薄入口，指向新 authority 或 archive，防止未来误读 |
| `archive` | 移入 archive，不再作为当前事实 |
| `block` | 存在冲突、高风险或证据不足，等待用户裁决 |

事实级含义：

| Verdict | 含义 |
|---|---|
| `promote` | 事实写入新的 authority 文档 |
| `merge` | 事实合并进已有 authority 文档 |
| `bridge` | 事实本身不晋升，但源文档需要保留跳转说明 |
| `archive` | 事实只保留历史价值，不作为当前事实 |
| `block` | 事实冲突、证据不足或高风险，需要用户裁决 |

必须 `block` 的事实：

```text
- 安全策略
- 权限模型
- 认证 / 授权流程
- 计费逻辑
- 数据删除策略
- 隐私和合规规则
- public API breaking change
- schema migration 的破坏性变更
- 高影响架构边界调整
```

### 7.4 Phase 4：Governance Plan / 治理计划

`docs/governance/YYYY-MM-DD-<slug>.md` 是每个独立仓库级治理批次的确认产物，也是用户一次确认的对象。case 相关治理使用 `docs/cases/<case-id>/governance-plan.md`。

```md
# Governance Plan

## Scope

## Layering Standard

## Authority Structure To Create

## File Routing Decisions
| Source | Current Role | Verdict | Target | Reason | Requires Confirmation |
|---|---|---|---|---|---|

## Fact Decisions
| Fact | Source | Type | Verdict | Target Authority Doc | Evidence | Risk |
|---|---|---|---|---|---|---|

## Bridge Decisions
| Old Entry | Bridge Target | Reason |
|---|---|---|

## Archive Decisions
| Source | Archive Target | Reason |
|---|---|---|

## Blocked Decisions
| Topic | Sources | Why Blocked | Needed Decision |
|---|---|---|---|

## Confirmation Log

## Applied Result
```

计划 frontmatter：

```yaml
---
status: proposed | approved | applied | applied_with_blocks | blocked
plan_version: 1
scope: current-case | docs-only | full-repo
governance_batch: YYYY-MM-DD-<slug>
approved_by:
approved_at:
---
```

计划内容实质变化时递增 `plan_version` 并重新确认。

### 7.5 Phase 5：One Confirmation / 一次确认

写入治理计划后，未获用户确认不得执行文件移动、bridge、archive 或 authority 更新。

用户确认后，agent 执行所有无依赖的非 `block` 项。允许 `block` 项存在并跳过执行；与 blocked 事实有依赖的非 block 变更也必须跳过。

状态规则：

```text
applied              无 block 且全部执行
applied_with_blocks  存在 block，但无依赖的非 block 项已执行
blocked              治理计划整体无法执行
```

### 7.6 Phase 6：Apply / 执行

执行内容：

```text
- 创建按需 authority 子区和文档。
- 合并或创建 authority facts。
- 创建必要 bridge 文件。
- 移动归档 historical / superseded / raw 材料。
- 保留需要追溯的 case evidence。
- 更新入口索引。
- 在治理计划写回 Applied Result。
```

Authority 文档最小 frontmatter：

```yaml
---
status: active | draft | superseded | archived
authority: true
layer: authority
type: product | domain | architecture | decision | contract | workflow | agent | operations
source_of_truth: user_confirmed | code | tests | adr | external | generated
supersedes: []
superseded_by: []
---
```

### 7.7 Phase 7：Archive / 历史文档归档

历史文档不作为当前事实继续散落在主文档区。

归档规则：

```text
- 原始历史材料移入 docs/archive/raw/ 或 docs/archive/historical/
- 被新权威文档取代的文档移入 docs/archive/superseded/
- 不删除历史文档，除非用户明确要求
- 归档文档应标记 superseded_by
```

### 7.8 Phase 8：Index / 入口索引

执行后必须更新入口索引：

```text
docs/README.md
```

并明确：

```text
- authority 入口
- 治理计划入口
- case evidence 入口，如存在
- archive 入口，如存在
- derived / generated 文档不是 source of truth
```

---

## 8. 文档分层模型

由 `setup-doc-governance` 建立并维护。

### L1：Authority / 权威文档

代表当前必须遵守的事实、规范和决策。

推荐位置：

```text
docs/authority/
```

规则：

```text
- 当前有效。
- 后续开发必须遵守。
- 更新必须用户确认。
- 和代码冲突时，必须显式报告。
- 不能被 agent 自动静默修改。
```

### L2：Operational / 工作流文档

代表某次任务的计划、执行和收尾记录。

推荐位置：

```text
docs/cases/<case-id>/
```

规则：

```text
- 可以自动创建和更新。
- 记录任务过程。
- 不直接作为长期规范。
- 可以为未来决策提供上下文。
```

### L3：Derived / 派生文档

从权威文档和代码派生出来，方便人阅读。

推荐位置：

```text
docs/derived/
README.md
```

规则：

```text
- 可以由 agent 建议更新。
- 机械性、可追溯的更新可以自动执行。
- 通常不作为最高权威。
- 如果和 authority 冲突，以 authority 为准。
```

### L4：Historical / 历史文档

已经不代表当前事实，但保留作为历史记录。

推荐位置：

```text
docs/archive/
```

规则：

```text
- 默认不能作为当前依据。
- 只能作为历史参考。
- 引用时必须标记 historical。
- 如果发现仍然有效，需要重新抽取事实并进入治理计划确认。
```

### L5：Scratch / 草稿文档

临时思考、草稿、实验记录。

推荐位置：

```text
docs/scratch/
```

规则：

```text
- 默认不可信。
- 不参与正式决策。
- 可以整理后升级为 L2 / L3 / L1。
- 可以定期清理。
```

---

## 9. 文档 Metadata 规范

Authority 文档使用最小必填 frontmatter：

```yaml
---
status: active | draft | superseded | archived
authority: true
layer: authority
type: product | domain | architecture | decision | contract | workflow | agent | operations
source_of_truth: user_confirmed | code | tests | adr | external | generated
supersedes: []
superseded_by: []
---
```

可选字段：

```yaml
owner:
last_verified:
review_cadence:
evidence: []
related_code: []
related_tests: []
related_docs: []
```

高风险 authority 文档额外要求 `owner` 和 `last_verified`。高风险包括 public API / CLI / schema / config contract、安全、权限、认证、隐私、计费、数据删除、runbook、agent policy、executable workflow、ADR 或架构边界。

---

## 10. Authority Order

本版将优先级拆成两套，避免把“用户本轮临时指令”误当成长期事实。

### 10.1 Execution Instruction Order

用于判断本次任务应该怎么执行：

```text
1. 用户本轮明确指令
2. 已确认 plan.md
3. 已确认治理计划，如当前任务是文档治理
4. Workflow Protocol
5. Active authority docs
6. 当前生产代码
7. 当前测试
8. L2 operational docs
9. L3 derived docs
10. L4 historical docs
11. L5 scratch docs
```

规则：

```text
- 用户本轮指令可以改变本次任务目标。
- 用户本轮指令不能自动改写长期事实。
- 如果用户指令与 authority 冲突，必须报告冲突，并等待用户确认是否提出 authority 变更。
```

### 10.2 Fact Authority Order

用于判断当前事实是什么：

```text
1. Active authority docs
2. 当前生产代码
3. 当前测试
4. 已接受 ADR / migration / release note
5. 用户本轮新信息，默认进入计划或治理确认，不能静默写入 authority
6. L2 operational docs
7. L3 derived docs
8. L4 historical docs
9. L5 scratch docs
```

规则：

```text
- 如果代码和 authority 文档冲突，必须停下来报告。
- 如果 L3 文档和 authority 文档冲突，更新或标记 L3 stale。
- 如果 historical docs 看起来相关，只能作为证据或历史上下文。
- 没有 metadata 的文档默认 unclassified，降低信任等级。
- 用户本轮提供的新事实作为 pending fact 高于 L2 / L3，但不能压过 active authority、当前代码、当前测试或已接受 ADR。
- 只影响当前任务的新事实进入任务计划确认；改变长期产品事实、workflow policy、authority docs、public contract 或 agent behavior 的新事实进入治理计划、authority update proposal，或由 `doc-sync-close` 在 explicit confirmation 后执行 narrow authority patch。
```

---

## 11. Skill 详细职责

---

# 11.1 `docloom-workflow`

## 定位

public automatic entry / thin router。

它负责识别用户意图、读取最小 workspace 和 case 状态、延迟创建 case、维护 Artifact Policy，并路由到具体阶段 skill。它不生成详细计划、不执行代码、不自动 review，也不维护中心任务索引。`review` 和 `grill` 是流程无关通用辅助 skill，不属于 Artifact Policy。

## 负责什么

```text
- 作为默认公开入口；用户显式点名 skill 时尊重用户点名。
- 读取目标仓库 AGENTS.md 作为本地工作约束，但不从中引入 CLI backend 架构。
- 固定读取最小 git 状态：root、branch、status。
- 按需读取 git diff --name-only。
- 解析现有 case 或生成正式 case_id。
- 延迟创建 docs/cases/<case-id>/ 和最小 case_state.yaml。
- 执行 status-only 查询。
- 判定本 case 的 Artifact Policy。
- 根据状态和用户意图路由到 setup-doc-governance、context-authority、plan-confirm、tdd-execute、doc-sync-close，或显式触发的 review。
- 消费 `review_risk` 信号，并在状态摘要中暴露风险。
```

## 不是做什么

```text
- 不依赖 CLI backend。
- 不调用 `.agents/doc-loom/bin/doc-loom`。
- 不创建 `.agents/doc-loom` control zone。
- 不生成 workflow.md / route.md。
- 不因为存在 approved plan 就自动执行。
- 不默认运行 context-authority。
- 不自动触发 review。
- 不因为 `review_risk` 自动运行 review。
- 不拥有或路由 `grill`。
- 不静默修复 case_state.yaml 与 Markdown 的冲突。
```

## 输出

默认对话输出：

```text
Current phase:
Evidence:
Route decision:
Next skill:
Blocking issue:
```

允许写入：

```text
docs/cases/<case-id>/case_state.yaml
```

只在创建 case、阶段转换或当前 skill 完成时更新。

## 路由原则

```text
- 用户明确要求治理初始化 / 重建 -> setup-doc-governance。
- 需要上下文 gate / 恢复 / 权威判断 / 冲突判断 -> context-authority。
- 进入持久化开发计划且 context 足够 -> plan-confirm。
- approved plan + 用户明确要求执行 / 继续 -> tdd-execute。
- 用户要求收尾 / 同步文档 / closure -> doc-sync-close。
- 用户显式要求 review -> review。
- 意图不清 -> status-only + 一个澄清问题。
```

详见 `docs/design/skills/docloom-workflow-design.md`。

---

# 11.2 `setup-doc-governance`

## 定位

项目初始化 / 周期性治理 Skill。

它的任务不是维护旧文档结构，而是从历史材料、当前文档、必要的代码和测试证据中抽取事实，写出治理计划，经用户一次确认后执行文档整合、迁移、桥接、归档和按需 authority 更新。

## 负责什么

```text
- 选择或默认 `scope: current-case | docs-only | full-repo`。
- 盘点历史文档、当前文档、入口索引和必要的代码 / 测试证据。
- 从历史材料和证据中抽取事实。
- 使用固定分层标准和 `promote / merge / bridge / archive / block` verdict 路由文件和事实。
- 生成 `docs/governance/YYYY-MM-DD-<slug>.md`。
- 等待用户一次确认治理决策表。
- 执行无依赖的非 block 项。
- 按需创建 `docs/authority/**` 文档。
- 将未确认原始材料保留在 `docs/cases/<case-id>/evidence/` 或归档。
- 创建必要 bridge，归档 historical / superseded / raw 材料。
- 更新入口索引。
```

## 不是做什么

```text
- 不是给旧文档简单加 metadata。
- 不是在原有目录上修修补补。
- 不是把所有历史内容直接迁移为权威内容。
- 不是自动把有冲突的事实写入 authority。
- 不是修改代码、测试、构建脚本或运行行为。
```

## 核心产物

```text
docs/governance/YYYY-MM-DD-<slug>.md
docs/authority/README.md
docs/authority/**                       # 按需
docs/cases/<case-id>/evidence/**        # 按需
docs/archive/**                         # 按需
docs/README.md      # 入口索引
bridge files                            # 按需
```

## 不再默认生成

```text
docs/governance/FACT_REGISTRY.md
docs/governance/KNOWLEDGE_CARDS.md
docs/governance/CONFLICT_REPORT.md
docs/governance/AUTHORITY_REBUILD_PLAN.md
docs/governance/CONTEXT_PACK_POLICY.md
```

## 输出建议：`docs/governance/YYYY-MM-DD-<slug>.md`

```md
# Governance Plan

## Scope

## Layering Standard

## Authority Structure To Create

## File Routing Decisions
| Source | Current Role | Verdict | Target | Reason | Requires Confirmation |
|---|---|---|---|---|---|

## Fact Decisions
| Fact | Source | Type | Verdict | Target Authority Doc | Evidence | Risk |
|---|---|---|---|---|---|---|

## Blocked Decisions
| Topic | Sources | Why Blocked | Needed Decision |
|---|---|---|---|

## Confirmation Log

## Applied Result
```

## 输出建议：入口索引

优先更新既有 `docs/README.md`。

索引只做路由，不列出每一个叶子文档：

```md
# Documentation Index

## Authority

## Governance Plan

## Cases and Evidence

## Archive

## Derived / Generated Views
```

索引必须声明 authority 是当前事实入口，evidence / archive 只作证据或历史，generated / derived 不作为 source of truth。

## 边界

```text
- 不处理具体开发任务。
- 不做业务代码修改。
- 不替用户自动决定高影响权威事实。
- 不创建空 authority 子区或占位文档。
- 未确认原始材料不得写入 authority。
- 冲突事实必须进入治理计划的 blocked decisions。
```

---

# 11.3 `context-authority`

## 定位

上下文获取与权威源判断 Skill。

它负责在计划前建立最小事实基础。它不是所有开发任务的固定入口，只在任务需要上下文 gate、恢复任务、权威判断或冲突判断时触发。

## 负责什么

```text
- 识别当前任务上下文。
- 尝试读取当前 branch / worktree。
- 根据 case Context Resolution Order 解析已有 case，或提出 proposed_case_slug。
- 按任务意图读取已有任务文档、入口索引、active authority、相关代码和测试。
- 仅在任务涉及文档治理、authority 变更、workflow / agent policy、public contract、高风险冲突或 blocked decision 时读取 `docs/governance/YYYY-MM-DD-<slug>.md`。
- 识别并分级 authority / code / tests / case docs 冲突。
- 标记不确定点和风险。
- 输出 route verdict，给 `plan-confirm` 判断是否能计划。
```

## 固定开始动作

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
```

`git diff --name-only` 仅在 dirty workspace、恢复任务、需要判断当前变更范围或计划 baseline 时运行。

## 输入

```text
- 用户需求
- 当前 workspace
- 当前 branch
- docs/README.md
- docs/authority/ 中相关 active authority
- docs/governance/YYYY-MM-DD-<slug>.md，仅在相关时
- 相关代码和测试，按 intent 决定
- 已有 case docs，仅在已有 case 或恢复任务时
```

## 输出

默认输出是对话中的最小上下文摘要，或交给 `plan-confirm` 内嵌到 `plan.md` 的 `Context Summary / Context Sources`。

只有已有 case、恢复任务、高风险任务、存在冲突、多 agent / 跨会话接续或用户明确要求时，才写：

```text
docs/cases/<case-id>/context-authority-brief.md
```

最小内容：

```md
## User Request

## Intent
- Type:
- Reason:
- Minimum Evidence Needed:

## Workspace Snapshot

## Case Context

## Sources Read
| Source | Layer / Type | Why Included | Trust / Freshness |
|---|---|---|---|

## Sources Excluded
| Source | Reason |
|---|---|

## Authority / Constraints

## Relevant Code / Tests

## Conflicts / Unknowns
| Topic | Conflict / Unknown | Risk | Required Decision |
|---|---|---|---|

## Route Verdict
- Verdict:
- Reason:
- Required Next Owner:
```

Route verdict：

```text
proceed_to_plan
proceed_to_plan_with_risk
needs_user_decision
needs_case_selection
run_setup_doc_governance
blocked_by_authority_conflict
```

## 边界

```text
- 只收集事实。
- 不写实现计划。
- 不改代码。
- 不创建 branch、worktree 或 case docs。
- 不修改 authority、governance plan、plan、execution、review 或 closure。
- 不自动解决权威冲突。
- 默认不落盘；条件满足时才写 context brief。
```

---

# 11.4 `plan-confirm`

## 定位

计划生成、用户确认、落计划文档 Skill。

这是执行前的唯一入口。
它要求 `docloom-workflow` 已经确定 `case_id` 和 `case_docs`。如果缺少 case identity，应返回 `needs_docloom_workflow` 或 `needs_case_initialization`，不自行生成 case。

## 负责什么

```text
- 基于 context-authority 的结果生成计划。
- 标记 risk_level。
- 维护 plan_version 和 base_commit。
- 明确目标和非目标。
- 记录用户明确确认的执行决策。
- 明确假设。
- 明确 TDD 顺序或 TDD exception。
- 明确文件影响。
- 明确测试策略。
- 明确文档影响。
- 标记哪些权威文档更新需要用户确认。
- 等待用户确认。
- 确认前写入 `status: draft` 的 plan.md。
- 用户确认后更新 approval metadata、Confirmation Log 和 case_state.yaml。
- 只有存在未来恢复点时才写 handoff.md。
```

## 注意

`plan-confirm` 不内置 `grill`。如果用户在独立讨论中明确确认了会影响执行的决策，`plan-confirm` 负责把这些决策写入 `plan.md` 的 `## Decisions`。

## 输出计划：`docs/cases/<case-id>/plan.md`

```md
---
case_id:
plan_version: 1
status: draft | approved
risk_level: low | medium | high
approved_by:
approved_at:
base_commit:
---

# Implementation Plan

## Goal
...

## Non-goals
...

## Decisions
| Decision | Source | Scope | Rationale |
|---|---|---|---|

## Assumptions
...

## Context Summary
...

## Risk Level
low | medium | high

Reason:
- ...

## TDD Applicability

- TDD Required: Yes / No
- If No, Reason:
- Alternative Verification:
  - ...

## TDD Plan
1. Add failing test for ...
2. Confirm failure reason.
3. Implement minimal change.
4. Refactor.
5. Run quality checks.

## Files to Change
- ...

## Tests to Add / Update
- ...

## Acceptance Criteria
- ...

## Risks
- ...

## Documentation Impact
| Doc | Layer | Change Needed | Requires Confirmation |
|---|---|---|---|

## Confirmation Log
- User approved plan_version:
- User approved at:
```

## 条件输出接续摘要：`docs/cases/<case-id>/handoff.md`

只有存在未来恢复点时才写，例如计划已确认但不立刻执行、需要跨会话接续、等待用户或外部依赖。

````md
# Handoff

## Current Phase
planned

## Last Completed
- Plan approved.

## Next Step
- Start TDD execution from the first Red step.

## Do Not Touch
- ...

## Commands Last Run
```bash
...
```

## Known Issues
- ...

## Source of Detailed History
- execution.md
- closure.md
````

## 输出机器可读状态：`docs/cases/<case-id>/case_state.yaml`

```yaml
case_id:
phase: planned
plan_version:
closure_status: open
last_updated:
```

## 关键 Gate

```text
没有用户确认，不进入 tdd-execute。
计划变化后，plan_version 必须递增，并重新确认。
当前 plan_version 未被确认，或 status 不是 approved，不准执行。
如果用户确认的讨论决策改变目标、非目标、文件范围、测试策略、风险等级、TDD exception 或文档影响，必须更新 `plan.md ## Decisions` 并重新确认计划。
```

---

# 11.5 `grill`

## 定位

通用、流程无关、用户手动触发的挑战 / 拷问 Skill。

它不属于 Doc Loom workflow 阶段，不由 `docloom-workflow` 路由，不进入 Artifact Policy。

## 触发条件

只有用户明确要求时才执行，例如：

```text
grill 一下
严格 grill 这个方案
帮我挑战一下这个计划
拷问一下需求边界
```

## 可以 grill 的对象

```text
- 任意需求、计划、架构、实现思路、测试策略、文档想法或产品判断。
- 用户在当前对话中提出的任何待决主张。
```

## 负责什么

```text
- 先中性复述正在挑战的目标、主张和边界。
- 沿设计树逐个解决最高杠杆问题。
- 对事实问题先查代码、测试或文档。
- 对决策问题一次只问一个，给 2-3 个选项，并用一句话推荐一个。
- 从第二问开始，先用一句话复述上一问结果。
- 在关键分支收敛或用户要求结束时，输出轻量收束。
```

## 输出

只输出对话内容，不写文件。

提问阶段每轮只输出一个问题、2-3 个选项和一句话推荐。第二问开始先复述上一问结果。结束时只输出：

```text
确认
未决
风险
```

不输出 `Grill Report`、workflow verdict、next owner 或 artifact path。

## 规则

```text
- grill 只负责挑战，不负责执行。
- grill 不修改任何文件。
- grill 不生成中间产物。
- grill 不把推荐答案自动当作用户决策。
- 用户明确确认的讨论决策可由后续 plan-confirm 写入 plan.md 的 ## Decisions。
- 最终 authority 文档变更由 doc-sync-close 或文档治理流程处理。
```

---

# 11.6 `tdd-execute`

## 定位

TDD 执行和质量检查 Skill。

只在持久化 case 的计划被用户确认后执行。普通一次性 coding、简单纯文档修改或无复杂验证的低风险任务不强制进入本 skill。

## 负责什么

```text
- 根据已确认 plan.md 执行。
- 校验 plan_version、approval metadata 和 base_commit。
- 读取 `Non-goals` 和 `Decisions` 作为执行边界。
- 默认先写失败测试。
- 如果不适用 TDD，记录 TDD exception 和替代验证。
- 支持轻量 adaptive execution，通过 `plan.md ## Plan Amendments` 记录低风险同目标小改。
- 确认测试失败原因正确。
- 写最小实现。
- 跑测试。
- 重构。
- 跑 lint / typecheck / build。
- 逐项核对 acceptance criteria。
- 记录执行证据。
- 更新 plan checkbox / status，不改变计划语义。
- 按 Artifact Policy 写 execution.md。
- 只有存在未来恢复点时更新 handoff.md。
- 如果偏离计划，记录偏离。
- 如果严重偏离计划，回到 plan-confirm。
- 如果 plan 或用户要求提交，按原子边界 stage / commit，并记录 commit hash。
```

## TDD Loop

```text
Red
  ↓
Green
  ↓
Refactor
  ↓
Quality Check
  ↓
Update Execution Docs
```

## 条件输出：`docs/cases/<case-id>/execution.md`

以下情况必须写：

```text
- TDD Required: Yes。
- 存在代码或行为变化。
- 出现计划偏离。
- 测试失败、重试或需要记录失败原因。
- 存在 material review_risk。
- 任务可能需要恢复。
```

纯文档、trivial config 或替代验证证据能完整放入 `closure.md` 时，可以跳过独立 `execution.md`。

`execution.md` 正文按最新结果维护，底部保留极简 `History`。不默认创建编号 execution 目录。

```md
# Execution Report

## Plan Reference
- plan_version:
- base_commit:
- adaptive_execution:

## Plan Followed
Yes / No

## Changes Made
- ...

## TDD Applicability

- TDD Required: Yes / No
- If No, Reason:
- Alternative Verification:
  - ...

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

## Acceptance Criteria Status
| Criteria | Status | Evidence |
|---|---|---|

## Deviations
- ...

## Remaining Issues
- ...

## Review Risk
- Level: low / medium / high
- Reasons:
  - ...
- Evidence:
  - ...

## Checkpoints / Commits
| Commit | Scope | Verification |
|---|---|---|
```

## 条件输出：`docs/cases/<case-id>/handoff.md`

只有存在未来恢复点时写。

````md
# Handoff

## Current Phase
executing

## Last Completed
- ...

## Next Step
- ...

## Do Not Touch
- ...

## Commands Last Run
```bash
...
```

## Known Issues
- ...
````

## 关键 Gate

```text
没有用户确认的 plan，不准执行。
当前 plan_version 未被确认，不准执行。
默认没有失败测试，不准实现；如果不适用 TDD，必须记录 TDD exception 和替代验证。
执行偏离计划，必须记录。
严重偏离计划，必须回到 plan-confirm。
```

---

# 11.7 `review`

## 定位

用户手动触发的通用辅助 Skill，用于只读、对话式证据审查。

`review` 不作为 workflow 阶段，不进入 Artifact Policy，不生成持久化审查文件，不修改状态，不写 proposal，不输出路由。

## 触发条件

用户明确要求：

```text
review 一下
让子 agent review
做一次 code review
检查一下这次实现
评审这个设计文档
检查这个方案有没有问题
```

## 负责什么

```text
- 审查指定设计文档、方案、diff、测试、文档变更或 case evidence。
- 按最小相关证据读取目标对象、引用来源、authority / ADR / contract、必要代码和测试。
- 使用 Fact Authority Order 发现事实冲突。
- 输出 assessment、evidence reviewed、findings 和 evidence gaps。
- 对 Critical / Important / Minor 按影响等级分级。
- 给出 finding 的 required correction，但不输出流程下一步。
```

## 对话输出

```md
## Scope

## Review Maturity
Draft / In-progress / Completed

## Evidence Reviewed
| Source | Why Included | Trust / Freshness |
|---|---|---|

## Evidence Not Reviewed
| Source | Reason | Impact |
|---|---|---|

## Assessment
No material issue found / Material issues found / Insufficient evidence

## Findings

### Critical

### Important

### Minor

## Evidence Gaps
```

## 规则

```text
- review 只在用户要求时执行。
- review 默认直接输出审查结果，只有无法开始时才问一个最小澄清问题。
- review 可以使用子 agent，但子 agent 只返回 findings。
- review 不产物、不改状态、不路由、不写 proposal、不修改文件。
```

详细规则以 `docs/design/skills/review-design.md` 为 SSOT。

---

# 11.8 `doc-sync-close`

## 定位

文档同步和任务收尾 Skill。

它负责把执行结果回写到文档体系里。

## 负责什么

```text
- 根据执行过程更新 L2 任务文档。
- 更新安全的 mechanical L3 派生文档。
- 识别需要更新的 authority 文档。
- 对 authority 默认只生成 proposed change。
- 等待用户明确确认后，才可执行 narrow authority patch；结构性、高风险或冲突型 authority 更新进入治理计划。
- 生成任务完成报告。
- 写入 closure.md。
- 标记任务完成。
```

## 输出：`docs/cases/<case-id>/closure.md`

```md
# Closure Report

## Summary
...

## Acceptance Criteria Status
| Criteria | Status |
|---|---|

## Code Changes
- ...

## Tests
| Command | Result |
|---|---|

## Docs Updated
- ...

## Findings Disposition
| Finding | Severity | Affects Acceptance | Disposition | Evidence |
|---|---|---:|---|---|

## Authority Changes
| Doc | Status | Proposed / Applied Change | Evidence | Risk | Next |
|---|---|---|---|---|---|

## Follow-up Updates
| When | Scope | Evidence | Effect on Final Status |
|---|---|---|---|

## Confirmation Log
| When | Confirmed By | Scope | Confirmation |
|---|---|---|---|

## Remaining Risks
- ...

## Follow-ups
- ...

## Final Status
Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned
```

## 文档更新规则

```text
L1 Authority   默认 proposal；explicit confirmation 后可执行 narrow patch；结构性治理进治理计划
L2 Operational 可以自动更新
L3 Derived     仅 mechanical derived sync 可自动更新，且必须可追溯
L4 Historical  默认不改
L5 Scratch     可清理或转正
```

## 关键 Gate

```text
Authority 文档更新必须用户确认；`doc-sync-close` 只可执行 explicit confirmation 后的 narrow authority patch。
有 case docs 的任务结束必须写 closure.md；无 case docs 的一次性任务不强制补 closure。
如果 acceptance criteria 未满足，不能标记 Done；`not_met` 默认 Blocked / Paused / Cancelled / Superseded，`partially_met` 或 `not_verified` 只有在用户明确接受剩余项或证据缺口时才能 Done with Caveats。
```

---

## 12. 推荐目录结构

### 12.1 Skill 目录结构

```text
skills/
  docloom-workflow/
    SKILL.md
    templates/
      case_state.yaml

  setup-doc-governance/
    SKILL.md
    templates/
      GOVERNANCE_PLAN.md
      authority-README.md
      authority-doc.md
      bridge.md

  context-authority/
    SKILL.md
    templates/
      context-authority-brief.md
      context-sources.md

  plan-confirm/
    SKILL.md
    templates/
      plan.md
      handoff.md

  grill/
    SKILL.md
    references/
      challenge-prompts.md

  tdd-execute/
    SKILL.md
    templates/
      execution.md
      handoff.md

  review/
    SKILL.md
    references/
      review-rubric.md

  doc-sync-close/
    SKILL.md
    templates/
      closure.md
      handoff.md
      case_state.yaml
```

### 12.2 项目文档目录结构

```text
docs/
  README.md

  governance/
    YYYY-MM-DD-<slug>.md

  authority/
    README.md
    product/
    domain/
    architecture/
      decisions/
    contracts/
    workflow/
    agent/
    operations/

  cases/
    <case-id>/
      evidence/
      case_state.yaml
      plan.md
      execution.md       # conditional
      handoff.md         # only when a future resume point exists
      closure.md

  derived/
    getting-started.md
    dev-guide.md
    examples.md
    troubleshooting.md

  archive/
    raw/
    historical/
    superseded/

  scratch/
```

---

## 13. Agent 接续任务的最短路径

当 agent 进入一个 workspace，需要快速恢复上下文时，优先走 `docloom-workflow` status-only：

```text
1. 执行 git rev-parse --show-toplevel。
2. 执行 git branch --show-current。
3. 根据 case Context Resolution Order 解析 case_id。
4. 推导或确认 docs/cases/<case-id>/。
5. 读取 case_state.yaml。
6. 如存在且需要恢复，读取 handoff.md。
7. 读取 plan.md。
8. 如存在，读取 execution.md / closure.md。
9. 检查 git status 和 diff。
10. 判断下一步。
```

优先读：

```text
docs/cases/<case-id>/case_state.yaml
docs/cases/<case-id>/handoff.md（仅当存在未来恢复点）
```

其中 `case_state.yaml` 用于快速判断机器状态；`handoff.md` 不是固定产物，只在存在未来恢复点时读取。`handoff.md` 应该回答：

```text
- 当前阶段是什么？
- 上一步完成了什么？
- 下一步是什么？
- 哪些文件不能碰？
- 最近跑过什么命令？
- 已知问题是什么？
```

---

## 14. 状态推导规则

不维护中心任务索引，状态从任务上下文、case_state.yaml、任务文档和 git 状态推导。`case_state.yaml` 是状态缓存，不是唯一真相；冲突时必须重新读取 Markdown 和 git 状态。

```text
如果用户明确指定 case docs
  -> 使用用户指定的任务上下文。

如果当前 branch 符合 case/<case-id>
  -> 从 branch 推导任务上下文。

如果 case docs 不存在且用户只是查询
  -> status-only。

如果 docs/cases/<case-id>/plan.md 不存在
  -> planning 阶段。

如果 plan.md 存在但未标记 approved
  -> waiting for plan confirmation。

如果 plan.md approved 且用户明确要求执行 / 继续
  -> ready to execute。

如果 plan.md approved 但用户只是查询
  -> status-only，报告 ready to execute。

如果 plan.md approved 且任务是纯文档 / trivial config / 低风险 UI 文案并无复杂验证
  -> 可以跳过 tdd-execute，直接进入相应编辑或 doc-sync-close。

如果 execution.md 存在或 git 状态显示 case 范围内有执行中改动，且 closure.md 不存在
  -> executing / doc_syncing。

如果 closure.md 存在
  -> done / done with caveats / blocked / cancelled / superseded / paused / abandoned。

如果 closure.md 存在但用户要求小范围同源 follow-up
  -> 可复用原 case，记录 amendment / follow-up；新目标、新风险或已 Cancelled / Superseded / Abandoned 的 case 默认新开或重新确认。
```

---

## 15. 最终约束清单

这部分可以直接放进每个 `SKILL.md` 的 shared protocol。

```text
- 历史文档是证据，不是默认权威。
- setup-doc-governance 使用固定分层标准、独立治理批次计划和一次用户确认。
- setup-doc-governance 通过 scope 控制范围：current-case / docs-only / full-repo。
- docs/authority/ 是按需创建的当前事实源集合，不创建空目录或占位文档。
- 未确认原始材料进入 docs/cases/<case-id>/evidence/ 或 archive，不进入 authority。
- 文件和事实治理 verdict 统一为 promote / merge / bridge / archive / block。
- docloom-workflow 是 public automatic entry 和 thin router；用户显式点名 skill 时尊重用户点名。
- docloom-workflow 拥有 case_id 生成和延迟 case 创建职责。
- docloom-workflow 持有 Artifact Policy；阶段 skill 按 policy 写自己的产物。
- docloom-workflow 消费 review_risk 信号；review_risk 不是 review 授权，也不是 workflow gate。
- branch/worktree 是推荐任务定位机制，不是强制前置条件。
- agent 不应擅自创建 branch 或 worktree，除非用户明确要求或确认。
- case docs 可以由 branch 推导，也可以由用户指定；新 case 由 `docloom-workflow` 在进入持久化 case workflow 时创建。
- 所有任务过程文档都属于 L2 Operational。
- case docs 创建后维护瘦身 case_state.yaml，作为机器可读状态缓存。
- Authority 更新必须用户确认；`doc-sync-close` 只可执行 explicit confirmation 后的 narrow patch。
- 用户本轮新事实不能静默写入 authority。
- L4 历史文档默认不能作为当前事实。
- 没有 context，不准计划。
- 默认没有用户确认计划，不准执行；Doc Loom Least v1 不支持低风险自动执行。
- plan-confirm 必须维护 plan_version 和 base_commit。
- 默认没有失败测试，不准实现；不适用 TDD 时必须记录 TDD exception 和替代验证。
- 无行为变化重构可用 characterization / existing behavior lock 替代 Red。
- tdd-execute 只服务持久化 case workflow；普通一次性 coding 不强制进入。
- 执行阶段可更新 plan checkbox / status，不算 plan_version 实质变化。
- adaptive execution 下低风险同目标小改记录到 Plan Amendments；实质变化必须重新确认。
- 执行偏离计划，必须记录。
- 严重偏离计划，必须重新确认。
- 实际 stage / commit 只有 plan 或用户明确要求时执行；提交必须按原子边界组织。
- review 是通用手动辅助 skill，不进入 Artifact Policy；只由用户明确请求触发，只读对话输出，不产物、不改状态、不路由。
- grill 是通用手动辅助 skill，不进入 workflow 路由或 Artifact Policy。
- 有 case docs 的任务结束或中断必须写 closure.md；无 case docs 的一次性任务不强制补 closure。
- closure_status 必须明确：Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned。
- `closure.md` 是收尾真相记录，`case_state.yaml` 是状态缓存；收尾时 phase 统一为 `closed`，具体结果写入 closure_status。
- Doc Loom Least v1 不依赖 CLI backend，不调用 `.agents/doc-loom/bin/doc-loom`。
```

---

## 16. 一句话总结

这套工作流可以概括为：

> 用 `docloom-workflow` 作为 public automatic entry 和 thin router，按最小路径解析状态、延迟创建 case、应用 Artifact Policy 并消费 `review_risk` 信号；用 `setup-doc-governance` 以固定分层标准和 `scope: current-case | docs-only | full-repo` 从历史文档、当前文档以及必要的代码 / 测试证据中抽取事实，生成治理计划并经用户一次确认后执行整合、迁移、桥接、归档和按需 authority 更新；在需要上下文 gate 时用 `context-authority` 定位当前工作和权威来源；用 `plan-confirm → tdd-execute → doc-sync-close` 完成带版本确认、TDD 纪律、轻量 adaptive execution 和授权原子提交边界的开发闭环；`review` 作为用户手动触发的只读对话审查 Skill，不产物、不改状态、不路由；`grill` 作为流程无关的通用手动挑战 Skill，只通过对话压力测试主张。


---

## 17. 落地优先级建议

为了避免一次性引入过多流程，建议按以下顺序落地：

### P0：必须先落地

```text
1. docs/governance/YYYY-MM-DD-<slug>.md
2. docs/authority/README.md
3. docs/README.md
4. docs/cases/<case-id>/plan.md
5. docs/cases/<case-id>/case_state.yaml
6. docs/cases/<case-id>/closure.md
```

目标：先让任务闭环跑起来。

### P1：增强接续稳定性

```text
1. plan_version
2. base_commit
3. risk_level
4. conditional handoff.md
5. conditional execution.md
```

目标：降低 agent 续跑、多人协作和计划漂移风险。

### P2：增强文档治理质量

```text
1. 文件级和事实级 verdict 表
2. bridge / archive 决策表
3. blocked decisions
4. 按需 authority 子区
5. historical / superseded / raw 归档规则
```

目标：把历史文档治理成可维护的 authority 体系。

### P3：质量增强

```text
1. conversation-only review skill
2. review_risk signals consumed by docloom-workflow
3. high-risk focused review checks
4. 通用 grill skill 的交互规则
```

目标：在关键任务上提高质量，而不是让所有任务都变重。

---

## 18. 最终设计原则

```text
严格，但不僵硬。
可追溯，但不繁琐。
文档优先，但不脱离代码。
TDD 默认，但允许记录化例外。
用户确认计划 gate 不由普通项目文档或 authority 文档放宽。
历史文档作为证据，authority 文档作为治理后的权威。
```
