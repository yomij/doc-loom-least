# 文档驱动开发工作流 Skill 设计（优化最终版）

> 目标：沉淀一套以文档为中心、由任务上下文驱动、通过 TDD 执行并回写文档的工作流 Skill System。
>
> 关键理念：历史文档不是直接被继续维护的对象，而是用于抽取事实的证据库；`setup-doc-governance` 的目标是用固定分层标准生成一份可确认的治理计划，再执行整合、迁移、桥接、归档和按需 authority 重建。

> 本版状态：已吸收评审优化意见，重点增强了固定治理标准、单一治理计划、风险分级、TDD 例外、计划确认版本绑定、机器可读任务状态、异常收尾状态和 authority 按需创建规则。

---

## 0. 本版关键优化

相比初版，本版增加以下落地机制：

```text
1. setup-doc-governance 使用单一固定流程，通过 `scope: current-case | docs-only | full-repo` 控制扫描范围。
2. TDD 是默认纪律，但允许记录化例外，避免纯文档、配置、Spike、紧急修复被流程卡死。
3. plan-confirm 增加 risk_level、plan_version、approved_plan_hash、base_commit，确保确认对象可追溯。
4. 每个任务目录增加 case_state.yaml，降低接续任务时的状态推导歧义。
5. review 仍默认不自动执行，但 agent 可以基于风险建议 review。
6. closure 状态补充 Cancelled / Superseded / Paused / Abandoned，覆盖真实任务中断场景。
7. Authority Order 拆分为 Execution Instruction Order 和 Fact Authority Order，避免把用户临时指令误写为长期事实。
8. setup-doc-governance 使用 `GOVERNANCE_PLAN.md` 作为唯一默认中间产物，文件级和事实级都用 `promote / merge / bridge / archive / block` verdict。
```

---

## 1. 最终 Skill 列表

最终拆解为 **5 个主流程 Skill + 2 个手动可选 Skill**。其中 `setup-doc-governance` 使用固定治理标准和一次确认流程，`review` 仍以手动触发为主，但允许 agent 基于风险给出 review 建议：

```text
setup-doc-governance
context-authority
plan-confirm
grill                 # 手动触发
tdd-execute
review                # 手动触发 / 可选
doc-sync-close
```

| Skill | 类型 | 默认执行 | 负责什么 |
|---|---|---:|---|
| `setup-doc-governance` | 初始化 / 周期性治理 | 否 | 以固定分层标准从历史文档、当前文档、代码和测试中抽取事实，写出 `GOVERNANCE_PLAN.md`，经用户一次确认后执行整合、迁移、桥接、归档和按需 authority 更新 |
| `context-authority` | 主流程 | 按需 | 在需要计划前上下文 gate、恢复任务、权威判断或冲突判断时，读取最小相关上下文并输出路由 verdict |
| `plan-confirm` | 主流程 | 是 | 生成计划、标记风险等级、绑定计划版本、等待用户确认、落计划文档 |
| `grill` | 手动可选 | 否 | 拷问需求、计划、测试策略、架构方案、文档影响 |
| `tdd-execute` | 主流程 | 是 | 按确认后的计划做 TDD 执行、测试、质量检查、执行记录；必要时记录 TDD 例外 |
| `review` | 手动可选 / 可建议 | 否 | 子 agent review / code review / 测试审查 / 文档审查；高风险任务可由 agent 建议触发 |
| `doc-sync-close` | 主流程 | 是 | 文档同步、权威文档更新确认、任务关闭 |

本版新增的跨 Skill 机制：

```text
risk_level             low | medium | high
plan_version           每次计划变更递增
approved_plan_hash     用户确认的计划内容哈希
base_commit            计划确认时的代码基线
tdd_applicability      是否适合严格 TDD
case_state.yaml        机器可读任务状态
review_recommendation  是否建议 review
closure_status         Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned
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
生成 GOVERNANCE_PLAN.md
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
进入当前任务上下文
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

### 2.3 手动可选流程

#### 用户手动发起 grill

```text
context-authority 之后
  ↓
grill 需求 / 上下文 / 约束
  ↓
plan-confirm
```

或者：

```text
plan-confirm 之后
  ↓
grill 计划
  ↓
修改计划
  ↓
用户确认
```

#### 用户手动发起 review

```text
tdd-execute 之后
  ↓
review
  ↓
如需返工，回到 tdd-execute
  ↓
通过后进入 doc-sync-close
```

---

## 3. 全局 Workflow Protocol

不单独保留 `workflow-orchestrator` Skill，而是把编排规则沉淀为所有 Skill 必须遵守的全局协议。

### 3.1 核心原则

```text
1. 历史文档是证据，不是默认权威。
2. 标准权威文档体系是治理后的结果，不是在旧结构上修修补补。
3. setup-doc-governance 使用单一固定流程；无 scope 时默认 `docs-only`，必要时升级到 `full-repo` 读取代码和测试证据。
4. 当前任务上下文可以来自 branch/worktree，也可以来自用户指定或 case docs。
5. branch/worktree 是推荐隔离机制，不是强制前置条件。
6. agent 不应擅自创建 branch 或 worktree，除非用户明确要求或确认。
7. 没有 context，不准计划。
8. 默认没有用户确认计划，不准执行；低风险任务可由项目策略显式放宽。
9. TDD 是默认纪律；无法先写失败测试时，必须记录 TDD exception 和替代验证方式。
10. 执行偏离计划，必须记录；严重偏离，必须重新确认。
11. plan-confirm 必须绑定 plan_version、approved_plan_hash 和 base_commit。
12. 每个任务应维护 case_state.yaml，作为机器可读状态缓存。
13. Authority 更新必须通过 `GOVERNANCE_PLAN.md` 一次确认；未确认原始材料不得写入 authority。
14. 每个任务结束必须写 closure.md；中断任务也必须有明确 closure_status。
15. grill 只能由用户手动触发。
16. review 只能由用户手动触发或明确要求；agent 可以基于风险建议 review。
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

默认策略：

```yaml
plan_confirmation_policy:
  low_risk_changes: require_confirmation
  medium_risk_changes: require_confirmation
  high_risk_changes: require_explicit_confirmation
```

项目可以显式放宽低风险任务：

```yaml
plan_confirmation_policy:
  low_risk_changes: auto
  medium_risk_changes: require_confirmation
  high_risk_changes: require_explicit_confirmation
```

如果项目没有声明策略，必须使用默认策略。

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
approved_plan_hash:
base_commit:
---
```

规则：

```text
- 每次计划内容实质变化，plan_version 必须递增。
- 用户确认后，记录 approved_plan_hash。
- tdd-execute 必须引用被确认的 plan_version。
- 如果 plan.md 被修改但未重新确认，不得继续执行。
- 如果 base_commit 后出现大量无关 diff，必须重新评估上下文。
```

### 3.5 机器可读任务状态

每个任务目录建议维护：

```text
docs/cases/<case-id>/case_state.yaml
```

示例：

```yaml
case_id: 20260617-auth-refresh-flow
phase: executing
mode: branch
branch: case/20260617-auth-refresh-flow
worktree:
plan_status: approved
plan_version: 2
review_status: not_requested
closure_status: open
risk_level: medium
base_commit: abc123
last_updated: 2026-06-17T11:00:00+09:00
```

定位：

```text
- Markdown 文档仍是人类可读主记录。
- case_state.yaml 是机器可读状态缓存。
- 如果 case_state.yaml 与 Markdown 冲突，必须报告并以 Markdown + git 状态重新推导。
```

### 3.6 Handoff 写入规则

```text
handoff.md
  - 保存当前状态摘要。
  - 可以覆盖。
  - 必须短、准、适合接续任务。

execution.md
  - 记录完整执行日志。
  - 原则上 append-only。

closure.md
  - 保存最终结果。
  - 任务结束、中断、取消、替代时都必须写。
```

### 3.7 Review 建议机制

`review` 不自动执行，但 agent 可以在以下情况建议 review：

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
## Review Recommendation

- Recommended: Yes / No
- Reason:
  - ...
```

### 3.8 异常收尾状态

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
4. plan-confirm 在需要落盘时新建的 case id
```

### 4.5 case Context Resolution Order

```text
1. 用户本轮明确指定的 case id / case docs / branch / worktree
2. 当前 worktree + case branch 可推导出的 case id
3. 当前 branch 可推导出的 case id
4. 当前目录下已有明确 case docs
5. 当前会话提出 proposed_case_slug，由 plan-confirm 决定是否创建正式 case
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
   - review.md，如存在
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
生成 GOVERNANCE_PLAN.md 作为唯一默认中间产物
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
  README.md 或 DOC_INDEX.md

  governance/
    GOVERNANCE_PLAN.md

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
      grill.md
      review.md
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
- review / confirmation gate
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

盘点结果不生成独立 inventory 产物，而是写入 `GOVERNANCE_PLAN.md` 的决策表。

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

### 7.4 Phase 4：GOVERNANCE_PLAN / 治理计划

`docs/governance/GOVERNANCE_PLAN.md` 是唯一默认中间产物，也是用户一次确认的对象。

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
approved_by:
approved_at:
---
```

不需要 hash。计划内容实质变化时递增 `plan_version` 并重新确认。

### 7.5 Phase 5：One Confirmation / 一次确认

写入 `GOVERNANCE_PLAN.md` 后，未获用户确认不得执行文件移动、bridge、archive 或 authority 更新。

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
- 在 GOVERNANCE_PLAN.md 写回 Applied Result。
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
docs/README.md 或 docs/DOC_INDEX.md
```

并明确：

```text
- authority 入口
- governance plan 入口
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
- 如果发现仍然有效，需要重新抽取事实并进入 `GOVERNANCE_PLAN.md` 确认。
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
3. 已确认 GOVERNANCE_PLAN.md，如当前任务是文档治理
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
- 只影响当前任务的新事实进入任务计划确认；改变长期产品事实、workflow policy、authority docs、public contract 或 agent behavior 的新事实进入 `GOVERNANCE_PLAN.md` 或 authority update proposal。
```

---

## 11. Skill 详细职责

---

# 11.1 `setup-doc-governance`

## 定位

项目初始化 / 周期性治理 Skill。

它的任务不是维护旧文档结构，而是从历史材料、当前文档、必要的代码和测试证据中抽取事实，写出 `GOVERNANCE_PLAN.md`，经用户一次确认后执行文档整合、迁移、桥接、归档和按需 authority 更新。

## 负责什么

```text
- 选择或默认 `scope: current-case | docs-only | full-repo`。
- 盘点历史文档、当前文档、入口索引和必要的代码 / 测试证据。
- 从历史材料和证据中抽取事实。
- 使用固定分层标准和 `promote / merge / bridge / archive / block` verdict 路由文件和事实。
- 生成 `docs/governance/GOVERNANCE_PLAN.md`。
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
docs/governance/GOVERNANCE_PLAN.md
docs/authority/README.md
docs/authority/**                       # 按需
docs/cases/<case-id>/evidence/**        # 按需
docs/archive/**                         # 按需
docs/README.md 或 docs/DOC_INDEX.md      # 入口索引
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

## 输出建议：`docs/governance/GOVERNANCE_PLAN.md`

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

优先更新既有 `docs/README.md`；没有入口时创建 `docs/DOC_INDEX.md`。

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
- 冲突事实必须进入 GOVERNANCE_PLAN.md 的 blocked decisions。
```

---

# 11.2 `context-authority`

## 定位

上下文获取与权威源判断 Skill。

它负责在计划前建立最小事实基础。它不是所有开发任务的固定入口，只在任务需要上下文 gate、恢复任务、权威判断或冲突判断时触发。

## 负责什么

```text
- 识别当前任务上下文。
- 尝试读取当前 branch / worktree。
- 根据 case Context Resolution Order 解析已有 case，或提出 proposed_case_slug。
- 按任务意图读取已有任务文档、入口索引、active authority、相关代码和测试。
- 仅在任务涉及文档治理、authority 变更、workflow / agent policy、public contract、高风险冲突或 blocked decision 时读取 `docs/governance/GOVERNANCE_PLAN.md`。
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
- docs/README.md 或 docs/DOC_INDEX.md
- docs/authority/ 中相关 active authority
- docs/governance/GOVERNANCE_PLAN.md，仅在相关时
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
- 不修改 authority、GOVERNANCE_PLAN、plan、execution、review 或 closure。
- 不自动解决权威冲突。
- 默认不落盘；条件满足时才写 context brief。
```

---

# 11.3 `plan-confirm`

## 定位

计划生成、用户确认、落计划文档 Skill。

这是执行前的唯一入口。

## 负责什么

```text
- 基于 context-authority 的结果生成计划。
- 标记 risk_level。
- 维护 plan_version、approved_plan_hash、base_commit。
- 明确目标和非目标。
- 明确假设。
- 明确 TDD 顺序或 TDD exception。
- 明确文件影响。
- 明确测试策略。
- 明确文档影响。
- 标记哪些权威文档更新需要用户确认。
- 等待用户确认。
- 用户确认后写入 plan.md 和 handoff.md。
```

## 注意

`plan-confirm` 不内置 grill。

如果用户要求拷问、挑战、压力测试，应切换到 `grill` Skill。

## 输出计划：`docs/cases/<case-id>/plan.md`

```md
---
case_id:
plan_version: 1
status: draft | approved
risk_level: low | medium | high
approved_by:
approved_at:
approved_plan_hash:
base_commit:
---

# Implementation Plan

## Goal
...

## Non-goals
...

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
- approved_plan_hash:
```

## 输出接续摘要：`docs/cases/<case-id>/handoff.md`

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
- review.md
- closure.md
````

## 输出机器可读状态：`docs/cases/<case-id>/case_state.yaml`

```yaml
case_id:
phase: planned
mode:
branch:
worktree:
plan_status: approved
plan_version:
review_status: not_requested
closure_status: open
risk_level:
base_commit:
last_updated:
```

## 关键 Gate

```text
没有用户确认，不进入 tdd-execute，除非项目策略显式允许低风险自动执行。
计划变化后，plan_version 必须递增，并重新确认。
approved_plan_hash 与当前 plan 内容不一致时，不准执行。
如果 grill 后计划变化，必须回到 plan-confirm。
```

---

# 11.4 `grill`

## 定位

用户手动触发的拷问 / 压力测试 Skill。

它不属于默认主流程。

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
- 需求
- 上下文结论
- 实施计划
- 架构方案
- 文档更新方案
- 测试策略
```

## 负责什么

```text
- 找需求歧义。
- 找隐藏假设。
- 找边界条件。
- 找反例。
- 找可能回归。
- 找计划漏洞。
- 找测试遗漏。
- 找文档和代码冲突。
- 输出必须澄清的问题和可接受假设。
```

## 输出：`docs/cases/<case-id>/grill.md` 或直接输出报告

```md
# Grill Report

## Target
Requirement / Plan / Architecture / Test Strategy / Doc Update

## Ambiguities
- ...

## Hidden Assumptions
- ...

## Edge Cases
- ...

## Possible Regressions
- ...

## Missing Tests
- ...

## Questions for User
1. ...

## Suggested Plan Changes
- ...

## Verdict
Proceed / Revise Plan / Blocked
```

## 规则

```text
- grill 只负责挑战，不负责执行。
- grill 不自动修改 plan。
- grill 之后如果计划变化，必须回到 plan-confirm。
```

---

# 11.5 `tdd-execute`

## 定位

TDD 执行和质量检查 Skill。

只在计划被用户确认后执行。

## 负责什么

```text
- 根据已确认 plan.md 执行。
- 校验 plan_version 和 approved_plan_hash。
- 默认先写失败测试。
- 如果不适用 TDD，记录 TDD exception 和替代验证。
- 确认测试失败原因正确。
- 写最小实现。
- 跑测试。
- 重构。
- 跑 lint / typecheck / build。
- 记录执行日志。
- 更新 handoff.md。
- 如果偏离计划，记录偏离。
- 如果严重偏离计划，回到 plan-confirm。
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

## 输出：`docs/cases/<case-id>/execution.md`

```md
# Execution Report

## Plan Reference
- plan_version:
- approved_plan_hash:
- base_commit:

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

## Deviations
- ...

## Remaining Issues
- ...

## Review Recommendation
- Recommended: Yes / No
- Reason:
  - ...

## Ready for Review
Yes / No
```

## 输出：`docs/cases/<case-id>/handoff.md`

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
没有用户确认的 plan，不准执行，除非项目策略显式允许低风险自动执行。
approved_plan_hash 不匹配，不准执行。
默认没有失败测试，不准实现；如果不适用 TDD，必须记录 TDD exception 和替代验证。
执行偏离计划，必须记录。
严重偏离计划，必须回到 plan-confirm。
```

---

# 11.6 `review`

## 定位

用户手动触发 / 可选 Review Skill。agent 不自动执行 review，但可以基于风险输出 review recommendation。

不作为默认流程强制步骤。

## 触发条件

用户明确要求：

```text
review 一下
让子 agent review
做一次 code review
检查一下这次实现
```

## 负责什么

```text
- 检查实现是否符合计划。
- 检查测试覆盖是否充分。
- 检查是否破坏权威文档。
- 检查架构、安全、维护性问题。
- 检查是否有不必要的范围扩大。
- 输出 blocking / non-blocking 问题。
- 如果需要返工，回到 tdd-execute。
- 如果通过，进入 doc-sync-close。
```

## 输出：`docs/cases/<case-id>/review.md`

```md
# Review Report

## Scope
...

## Inputs Reviewed
- plan.md
- execution.md
- git diff
- test results
- authority docs

## Blocking Issues
- ...

## Non-blocking Suggestions
- ...

## Test Coverage Notes
- ...

## Documentation Notes
- ...

## Architecture / Security Notes
- ...

## Verdict
Approved / Needs Changes
```

## 规则

```text
- review 只在用户要求时执行。
- agent 可以建议 review，但不能把建议等同于用户授权。
- review 发现 blocking issue 后，必须回到 tdd-execute。
- review 通过后可以进入 doc-sync-close。
```

---

# 11.7 `doc-sync-close`

## 定位

文档同步和任务收尾 Skill。

它负责把执行结果回写到文档体系里。

## 负责什么

```text
- 根据执行过程更新 L2 任务文档。
- 更新安全的 L3 派生文档。
- 识别需要更新的 authority 文档。
- 对 authority 只生成 proposed change。
- 等待用户确认后才更新 authority。
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

## Authority Docs Requiring Approval
| Doc | Reason | Proposed Change |
|---|---|---|

## Remaining Risks
- ...

## Follow-ups
- ...

## Final Status
Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned
```

## 文档更新规则

```text
L1 Authority   只生成 proposed change 或 governance follow-up，必须用户确认
L2 Operational 可以自动更新
L3 Derived     可以自动更新，但必须可追溯
L4 Historical  默认不改
L5 Scratch     可清理或转正
```

## 关键 Gate

```text
Authority 文档更新必须用户确认。
任务结束必须写 closure.md。
如果 acceptance criteria 未满足，不能标记 Done，只能 Done with Caveats、Blocked、Cancelled、Superseded、Paused 或 Abandoned。
```

---

## 12. 推荐目录结构

### 12.1 Skill 目录结构

```text
skills/
  setup-doc-governance/
    SKILL.md
    templates/
      GOVERNANCE_PLAN.md
      authority-README.md
      authority-doc.md
      bridge.md
      DOC_INDEX.md

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
    templates/
      grill-report.md

  tdd-execute/
    SKILL.md
    templates/
      execution.md
      handoff.md

  review/
    SKILL.md
    templates/
      review.md

  doc-sync-close/
    SKILL.md
    templates/
      closure.md
```

### 12.2 项目文档目录结构

```text
docs/
  README.md 或 DOC_INDEX.md

  governance/
    GOVERNANCE_PLAN.md

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
      execution.md
      handoff.md
      grill.md
      review.md
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

当 agent 进入一个 workspace，需要快速恢复上下文时：

```text
1. 执行 git rev-parse --show-toplevel。
2. 执行 git branch --show-current。
3. 根据 case Context Resolution Order 解析 case_id。
4. 推导或确认 docs/cases/<case-id>/。
5. 读取 handoff.md。
6. 读取 plan.md。
7. 读取 execution.md。
8. 如存在，读取 review.md / closure.md。
9. 检查 git status 和 diff。
10. 判断下一步。
```

优先读：

```text
docs/cases/<case-id>/case_state.yaml
docs/cases/<case-id>/handoff.md
```

其中 `case_state.yaml` 用于快速判断机器状态，`handoff.md` 用于读取人类可读接续摘要。`handoff.md` 应该回答：

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

如果 docs/cases/<case-id>/plan.md 不存在
  -> planning 阶段。

如果 plan.md 存在但未标记 approved
  -> waiting for plan confirmation。

如果 plan.md approved 且 execution.md 不存在
  -> ready to execute。

如果 execution.md 存在且 closure.md 不存在
  -> executing / reviewing / doc_syncing。

如果 review.md 存在且 verdict = Needs Changes
  -> 回到 tdd-execute。

如果 closure.md 存在
  -> done / done with caveats / blocked / cancelled / superseded / paused / abandoned。
```

---

## 15. 最终约束清单

这部分可以直接放进每个 `SKILL.md` 的 shared protocol。

```text
- 历史文档是证据，不是默认权威。
- setup-doc-governance 使用固定分层标准、单一 GOVERNANCE_PLAN.md 和一次用户确认。
- setup-doc-governance 通过 scope 控制范围：current-case / docs-only / full-repo。
- docs/authority/ 是按需创建的当前事实源集合，不创建空目录或占位文档。
- 未确认原始材料进入 docs/cases/<case-id>/evidence/ 或 archive，不进入 authority。
- 文件和事实治理 verdict 统一为 promote / merge / bridge / archive / block。
- branch/worktree 是推荐任务定位机制，不是强制前置条件。
- agent 不应擅自创建 branch 或 worktree，除非用户明确要求或确认。
- case docs 可以由 branch 推导，也可以由用户指定；新 case 由 `plan-confirm` 在需要落盘时创建。
- 所有任务过程文档都属于 L2 Operational。
- 每个任务建议维护 case_state.yaml，作为机器可读状态缓存。
- Authority 更新必须用户确认。
- 用户本轮新事实不能静默写入 authority。
- L4 历史文档默认不能作为当前事实。
- 没有 context，不准计划。
- 默认没有用户确认计划，不准执行；低风险自动执行必须由项目策略显式允许。
- plan-confirm 必须维护 plan_version、approved_plan_hash、base_commit。
- 默认没有失败测试，不准实现；不适用 TDD 时必须记录 TDD exception 和替代验证。
- 执行偏离计划，必须记录。
- 严重偏离计划，必须重新确认。
- grill 只由用户手动触发。
- review 只由用户手动触发或明确要求；agent 可以基于风险建议 review。
- 每个任务结束或中断都必须写 closure.md。
- closure_status 必须明确：Done / Done with Caveats / Blocked / Cancelled / Superseded / Paused / Abandoned。
```

---

## 16. 一句话总结

这套工作流可以概括为：

> 用 `setup-doc-governance` 以固定分层标准和 `scope: current-case | docs-only | full-repo` 从历史文档、当前文档以及必要的代码 / 测试证据中抽取事实，生成 `GOVERNANCE_PLAN.md` 并经用户一次确认后执行整合、迁移、桥接、归档和按需 authority 更新；在需要上下文 gate 时用 `context-authority` 定位当前工作和权威来源；用 `plan-confirm → tdd-execute → doc-sync-close` 完成带版本确认和 TDD 纪律的开发闭环；`grill` 和 `review` 作为用户手动触发的质量增强 Skill，review 可由 agent 基于风险建议。


---

## 17. 落地优先级建议

为了避免一次性引入过多流程，建议按以下顺序落地：

### P0：必须先落地

```text
1. docs/governance/GOVERNANCE_PLAN.md
2. docs/authority/README.md
3. docs/README.md 或 docs/DOC_INDEX.md
4. docs/cases/<case-id>/plan.md
5. docs/cases/<case-id>/handoff.md
6. docs/cases/<case-id>/execution.md
7. docs/cases/<case-id>/closure.md
```

目标：先让任务闭环跑起来。

### P1：增强接续稳定性

```text
1. case_state.yaml
2. plan_version
3. approved_plan_hash
4. base_commit
5. risk_level
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
1. grill.md
2. review.md
3. review_recommendation
4. 高风险任务强制建议 review
```

目标：在关键任务上提高质量，而不是让所有任务都变重。

---

## 18. 最终设计原则

```text
严格，但不僵硬。
可追溯，但不繁琐。
文档优先，但不脱离代码。
TDD 默认，但允许记录化例外。
用户确认关键事实，但允许低风险任务通过项目策略提速。
历史文档作为证据，authority 文档作为治理后的权威。
```
