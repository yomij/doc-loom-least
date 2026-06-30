# setup-doc-governance Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `setup-doc-governance` 的职责、触发、输入、输出、固定治理标准、确认流程、执行边界和验收标准，供后续创建真实 `SKILL.md` 与模板资源使用。

---

## 1. 定位

`setup-doc-governance` 是文档治理初始化 / 重建 / 周期性整理 skill。

它的职责不是维护旧文档结构，也不是创建复杂治理流水线，而是用一套固定标准读取历史文档、当前文档，并在需要时读取代码和测试作为证据源，抽取仍然有效的事实，然后完成文档的整合、迁移、桥接、废弃和索引修复。

核心判断：

```text
Historical docs = evidence
Authority docs = confirmed source of truth
Governance plan = one approval surface
```

核心目标：

- 用固定分层标准判断每份文档和每条事实的去向。
- 从历史文档和代码 / 测试证据中抽取事实，而不是迁移整篇旧文档。
- 每个独立治理批次使用独立治理计划文件。
- 用户一次确认治理计划后，agent 再执行文档修改、移动、桥接和归档。
- `docs/authority/` 采用按需模块化结构，不创建空目录或占位文档。
- `setup-doc-governance` 只治理文档，不修改代码、测试、构建脚本或运行行为。

---

## 2. 建议 Frontmatter

```yaml
---
name: setup-doc-governance
description: Set up or rebuild documentation governance for a project. Use when the user asks to initialize docs governance, organize project docs, rebuild authority docs, classify historical docs, extract facts from existing docs/code/tests, create or update a governance plan, migrate docs, bridge old entries, archive superseded docs, or repair the documentation index.
---
```

---

## 3. 触发条件

应该触发：

- 用户说“初始化文档治理”。
- 用户要求“整理项目文档体系”。
- 用户要求“把历史文档治理成权威文档”。
- 用户要求从旧文档、当前文档、代码或测试中抽取事实。
- 用户要求迁移、整合、桥接、废弃、归档文档。
- 用户要求生成或执行治理计划。
- 用户要求修复文档入口、索引或 authority 结构。

不应该触发：

- 单个开发任务开始前读取上下文。此时使用 `context-authority`。
- 任务完成后的文档同步和关闭。此时使用 `doc-sync-close`。
- 普通设计文档写作，除非目标是治理体系或 authority 重建。
- 代码、测试、构建脚本或运行行为修改。

---

## 4. 输入

必需输入：

- 当前 workspace。
- 用户指定或默认的治理范围 `scope`。
- 现有 `docs/`、README、ADR、任务文档、历史文档。
- 项目约束文件。

可选输入：

- 当前 case id 或 case docs。
- 用户指定的历史文档范围。
- 用户指定的 authority 目标区域。
- 当前代码和测试，仅在 `scope: full-repo` 或事实验证需要时读取。

---

## 5. Scope

`setup-doc-governance` 不再区分 `minimal` / `full` 两套模式。它使用单一固定流程，通过 `scope` 控制扫描范围。

```text
current-case
  只治理当前 case 相关材料，例如 drafts、evidence、plan、closure 和局部迁移。

docs-only
  默认范围。扫描 docs、README、ADR、入口索引和历史文档。

full-repo
  在 docs-only 基础上读取相关代码和测试作为事实证据。
```

默认规则：

```text
If no scope is specified, use docs-only.
Escalate to full-repo only when authority claims require code/test verification or the user explicitly asks to use code/tests as evidence.
```

`full-repo` 仍然只读代码和测试。发现代码或测试需要修改时，只能在本次治理计划中记录 follow-up。

---

## 6. 输出

默认输出收敛为：

```text
docs/governance/YYYY-MM-DD-<slug>.md        # 独立仓库级治理批次
docs/cases/<case-id>/governance-plan.md     # 与 active case 绑定的治理批次
docs/authority/README.md
docs/authority/**                       # 按需创建
docs/cases/<case-id>/evidence/**        # 按需创建
docs/archive/**                         # 按需创建
docs/README.md                            # 入口索引
bridge files                            # 按需创建
```

不再默认输出：

```text
docs/governance/FACT_REGISTRY.md
docs/governance/KNOWLEDGE_CARDS.md
docs/governance/CONFLICT_REPORT.md
docs/governance/AUTHORITY_REBUILD_PLAN.md
docs/governance/CONTEXT_PACK_POLICY.md
```

这些概念可以作为 agent 内部分析方法，但不是默认落地产物。

---

## 7. 推荐资源结构

```text
setup-doc-governance/
  SKILL.md
  templates/
    GOVERNANCE_PLAN.md
  references/
    shared-protocol.md
    layering-and-routing.md
    verdicts.md
```

`SKILL.md` 保留核心流程、scope、verdict、gate 和执行边界。较长的分层标准、authority 路由和 frontmatter 规则放入 `references/`。

---

## 8. 固定分层标准

治理后采用面向本工作流的五层文档模型。

| Layer | 名称 | 默认位置 | 作用 |
|---|---|---|---|
| L1 | Authority | `docs/authority/` | 已确认、可复用、会约束后续 agent 的当前事实 |
| L2 | Case / Operational | `docs/cases/<case-id>/` | 当前任务过程、计划、evidence、执行和收尾记录 |
| L3 | Derived / Index | `docs/README.md`、README、guide | 入口、导航、派生说明，不作为源事实 |
| L4 | Archive / Historical | `docs/archive/` | 历史、废弃、被取代材料，默认不作为当前事实 |
| L5 | Scratch | `docs/scratch/` 或 case notes | 草稿、临时笔记、未验证推断 |

规模化模型映射：

| 工作流层 | 规模化治理层 | 说明 |
|---|---|---|
| L1 Authority | 长期稳定知识层 + 高频变更中已确认事实 + 可执行 AI Context + 运维记忆 | 按事实类型进入不同 authority 子区 |
| L2 Case / Operational | 任务级上下文与高频变更记录 | plan、execution、evidence、closure |
| L3 Derived / Index | 生成视图与分发层 | README、索引、摘要、Context Pack 视图 |
| L4 Archive / Historical | Archive / History | 只作证据，不默认参与 AI 上下文 |
| L5 Scratch | 临时上下文层 | chat notes、debug notes、候选推断 |

SSOT 不是某一个目录，而是横向原则。不同知识的真实来源可以是用户确认、代码、测试、ADR、外部系统或生成视图；frontmatter 必须声明主事实源。

---

## 9. Authority 结构

`docs/authority/` 是已确认事实源集合，不是单文件承载区，也不是原始材料仓库。

Authority 采用模块化、按需创建规则：

```text
Authority is modular and optional-by-need.
Create only the authority sections that have confirmed facts.
Do not create empty authority folders or placeholder documents.
```

默认最小结构：

```text
docs/authority/
  README.md
```

可选子区：

```text
docs/authority/
  README.md

  product/
    README.md
    goals.md
    scope.md
    requirements.md

  domain/
    README.md
    glossary.md
    concepts.md
    invariants.md

  architecture/
    README.md
    overview.md
    components.md
    flows.md
    decisions/
      ADR-0001-xxx.md

  contracts/
    README.md
    cli.md
    api.md
    schema.md
    config.md

  workflow/
    README.md
    case-lifecycle.md
    doc-governance.md
    review-policy.md

  agent/
    README.md
    policy.md
    context-loading.md
    adapters.md

  operations/
    README.md
    setup.md
    release.md
    runbooks/
      <topic>.md
```

默认路由：

| 知识类型 | 目标 |
|---|---|
| 产品目标、用户、范围、非目标、验收标准 | `docs/authority/product/**` |
| 领域概念、术语、业务不变量、状态规则 | `docs/authority/domain/**` |
| 架构形状、模块边界、数据流、集成点 | `docs/authority/architecture/**` |
| 架构决策、替代方案、权衡、后果 | `docs/authority/architecture/decisions/**` |
| CLI / API / schema / config 契约 | `docs/authority/contracts/**` |
| case lifecycle、治理流程、review / confirmation gate | `docs/authority/workflow/**` |
| agent policy、context loading、adapter source | `docs/authority/agent/**` |
| setup、release、runbook、维护规则 | `docs/authority/operations/**` |

`docs/authority/README.md` 只做路由和优先级说明，不承载大量事实。

---

## 10. 原始材料与 Evidence

与当前工作相关、但未确认的原始材料放在当前 case：

```text
docs/cases/<case-id>/evidence/
  draft-api.md
  initial-design.md
  legacy-notes.md
```

规则：

- 未确认接口文档、初始设计文档、旧讨论记录都是 evidence，不能进入 authority。
- 治理计划可以引用 evidence，并对抽取出的事实给出 verdict。
- 用户确认后，只把确认后的事实写入 `docs/authority/**`。
- 原始材料本身不迁入 authority。
- case evidence 不设固定 TTL，由治理计划中的 verdict 决定保留、归档、桥接或删除。

case 结束后的常见处理：

| 情况 | 处理 |
|---|---|
| 仍需追溯 | 留在 `docs/cases/<case-id>/evidence/` |
| 已被 authority 吸收，原路径有误读风险 | 原路径写 bridge，原文进 `docs/archive/superseded/` |
| 只剩历史背景 | 移入 `docs/archive/historical/` |
| 无保留价值且用户允许删除 | 删除 |

---

## 11. Verdict 模型

文件级和事实级都使用同一组 verdict：

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

不再引入 `candidate / accepted / canonical / deprecated` 作为默认状态矩阵。

---

## 12. Governance Plan

治理计划是用户一次确认的对象。每个独立治理批次使用独立计划文件；执行后同一个批次文件写回结果，不新增 `GOVERNANCE_RESULT.md`。

路径规则：

```text
case 相关治理:
  docs/cases/<case-id>/governance-plan.md

其他仓库级治理:
  docs/governance/YYYY-MM-DD-<slug>.md
```

旧路径 `docs/governance/GOVERNANCE_PLAN.md` 只作为 legacy context 读取，不再作为新治理批次写入目标。

Frontmatter：

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

`plan_version` 表达计划实质变化后需要重新确认。

推荐结构：

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

## Follow-ups
| Follow-up | Reason | Suggested Owner |
|---|---|---|

## Confirmation Log

## Applied Result
```

决策表粒度：

- 文件级为主，因为治理执行最终要移动、桥接、归档文件。
- 事实级为辅，因为 authority 写入的是事实，不是整篇旧文档。

---

## 13. Metadata

Authority 文档使用最小必填 frontmatter。

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

高风险 authority 文档额外要求：

```yaml
owner:
last_verified:
```

高风险包括：

- public API / CLI / schema / config contract。
- 安全、权限、认证、隐私、计费、数据删除。
- runbook。
- agent policy / executable workflow。
- ADR 或架构边界。

`source_of_truth` 规则：

| 值 | 含义 |
|---|---|
| `user_confirmed` | 用户明确确认的产品、范围、流程、治理事实 |
| `code` | 当前实现是主事实源 |
| `tests` | 测试或契约测试是主事实源 |
| `adr` | 已接受 ADR 是主事实源 |
| `external` | 外部系统、法规、第三方协议是主事实源 |
| `generated` | 派生视图，只能消费，不能覆盖源事实 |

多个来源共同支撑时，用 `evidence` 记录，但 `source_of_truth` 只填主事实源。

---

## 14. 权威优先级

治理文档必须区分两套优先级。

### 14.1 Execution Instruction Order

用于判断本次治理任务怎么执行：

```text
1. 用户本轮明确指令
2. 已确认的本批次治理计划
3. 本 skill 的固定治理规则
4. Active authority docs
5. 当前生产代码
6. 当前测试
7. case docs / evidence
8. derived / index docs
9. archive / historical docs
10. scratch docs
```

### 14.2 Fact Authority Order

用于判断当前事实是什么：

```text
1. Active authority docs
2. 当前生产代码
3. 当前测试
4. 已接受 ADR / migration / release note
5. 用户本轮新信息，默认需要进入治理计划确认
6. case docs / evidence
7. derived / index docs
8. archive / historical docs
9. scratch docs
```

规则：

- 用户本轮指令可以改变本次治理目标。
- 用户本轮新事实不能静默写入 authority，必须进入治理计划确认。
- 用户指令与 authority 冲突时，必须记录为 `block` 或明确 proposed change。

---

## 15. 代码 / 测试证据规则

`setup-doc-governance` 可以读取代码和测试作为证据，但不能修改它们。

代码与历史文档冲突时：

可以默认以代码 + 测试为准的情况：

- 内部实现细节。
- 非 public contract。
- 非安全 / 权限 / 计费 / 数据删除 / 隐私。
- 历史文档没有 active authority 状态。
- 代码和测试一致。

必须 `block` 的情况：

- public API / CLI / schema / config contract。
- 安全、权限、认证、隐私、计费、数据删除。
- ADR 明确规定的架构边界。
- 用户或 owner 最近确认过的事实。
- 代码和测试不一致。
- 测试缺失且影响面较高。

---

## 16. 核心工作流

### Step 1. Resolve Scope

确定 `scope`：

```text
current-case
docs-only
full-repo
```

如果用户未指定，使用 `docs-only`。

### Step 2. Inventory

扫描范围内的文档入口和历史材料。

`docs-only` 默认扫描：

```text
docs/**
README*
adr/**
*.md
```

`full-repo` 可额外读取相关：

```text
src/**
tests/**
package/config/schema/API files
```

盘点目标不是生成独立 inventory 产物，而是为治理计划的决策表提供依据。

### Step 3. Extract Facts

从旧文档、当前文档、case evidence、代码和测试中抽取事实。

知识类型：

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

抽取规则：

- 抽取“陈述 + 来源 + 证据 + 作用域 + 风险”。
- 不复制旧文档结构。
- 不把 agent 推断改写成当前事实。
- 不把未确认接口、设计草稿、历史讨论直接写入 authority。

### Step 4. Route Files and Facts

为文件和事实分别给出 verdict。

路由判断维度：

- 这是否是当前事实。
- 是否已有 authority 宿主。
- 是否只对当前 case 有效。
- 是否有误读风险，需要 bridge。
- 是否只剩历史价值。
- 是否冲突、高风险或证据不足，需要 block。

### Step 5. Write Governance Plan

写入或更新：

```text
docs/cases/<case-id>/governance-plan.md
docs/governance/YYYY-MM-DD-<slug>.md
```

状态为：

```yaml
status: proposed
```

此时不得执行文件移动、bridge、archive 或 authority 更新。

### Step 6. One User Confirmation

用户确认本批次治理计划后：

```yaml
status: approved
approved_by: user
approved_at: <timestamp>
```

如果计划内容实质变化，必须递增 `plan_version` 并重新确认。

### Step 7. Apply Non-blocked Decisions

执行所有非 `block` 项：

- 创建按需 authority 子区和文档。
- 合并或创建 authority facts。
- 创建必要 bridge 文件。
- 移动归档历史 / superseded 材料。
- 保留需要追溯的 case evidence。
- 更新入口索引。

允许 `block` 项存在并跳过执行。

规则：

- 与 blocked 事实存在依赖的非 block 变更也必须跳过。
- 有 block 但其他项已执行，状态为 `applied_with_blocks`。
- 无 block 且全部执行，状态为 `applied`。

### Step 8. Write Applied Result

在同一个治理计划文件写回：

- 实际创建、更新、移动、桥接、归档的文件。
- 跳过的 blocked 项。
- 需要后续开发、测试、owner 决策或代码修复的 follow-up。

---

## 17. Bridge 与 Archive 规则

旧文档不默认 bridge。

优先 bridge 的情况：

- 原路径是常见入口。
- README、设计入口、旧 authority 文档。
- 未来人或 agent 很可能继续从旧路径进入。
- 不保留 bridge 会造成误读或断链。

优先 archive 的情况：

- 深层历史材料。
- 草稿或证据附件。
- 原文已被 authority 吸收，且旧路径未来不应继续作为入口。
- 只剩历史背景。

Bridge 文件应薄：

```md
# Superseded

This document is no longer the authority source.

Current authority: <target>
Historical source: <archive target>
```

Bridge 不承载旧事实，不扩写背景。

Archive 推荐结构：

```text
docs/archive/
  historical/
  superseded/
  raw/
```

归档文档应尽量记录 `superseded_by` 或在 archive index 中能追溯到新位置。

---

## 18. Index 规则

执行后必须同步更新文档入口索引。

规则：

- 如果项目已有 `docs/README.md`，优先更新它。
- 如果项目没有文档入口，创建 `docs/README.md`。
- 索引只回答“从哪里进入当前文档体系”。
- 索引不要求列出每一个叶子文档。

索引必须能路由到：

- authority 入口。
- 治理计划。
- case evidence，如存在。
- archive 入口，如存在。
- derived / generated 文档说明。

索引必须声明：

- authority 是当前事实入口。
- evidence / archive 只作证据或历史。
- generated / derived 不作为 source of truth。

---

## 18.5 入口与权威声明规则

入口层（L3）只做路由，不承载领域事实。

- 根 `README.md` 为必需全局入口，限定为路由、权威来源与顺序、冲突规则，不含领域细节。
- `AGENTS.md` 等 always-on agent 文件只放薄派生指针。
- 局部 `README.md` 仅用于复杂领域（有权威关系、文件多或层级深）；普通目录记 `none`。
- 入口须声明权威来源，冲突链须与本 skill 的 Fact Authority Order 一致。
- `archive/`、`old/`、`deprecated/` 路径或标注废弃的文档必须带 `status: archived | superseded`，非 active 的文档只作证据，不进入当前事实上下文。

---

## 19. AI Context 读取规则

不生成独立 `CONTEXT_PACK_POLICY.md`。

上下文读取规则写入 `docs/authority/agent/context-loading.md` 或 `docs/authority/workflow/doc-governance.md`，仅在对应 authority 子区存在确认事实时创建。

默认规则：

- Agent 默认读取 active authority。
- Archive / historical 只能作为引用证据。
- case evidence 可进入任务上下文，但必须标记未确认。
- generated / derived 不作为 source of truth。
- blocked facts 不进入当前事实上下文。
- bridge 只用于导航，不作为事实来源。

---

## 20. Agent Instruction 治理

AI 指令属于可执行 AI Context 层，不应散落多份手工维护。

推荐策略：

```text
canonical policy once -> adapters everywhere
```

默认 authority 宿主：

```text
docs/authority/agent/policy.md
docs/authority/agent/adapters.md
```

适配输出可以包括：

```text
CLAUDE.md
.github/copilot-instructions.md
.github/instructions/*.instructions.md
Cursor / Windsurf rules
```

规则：

- 只把高频、稳定、跨任务都需要的规则放入 always-on 指令。
- 多步骤流程应进入 skill。
- 路径局部规则应进入 path-specific rule，不进入全局 AGENTS。
- Prompt / workflow / agent policy 变更必须进入治理计划并经用户确认。
- 生成的 adapter 文件是派生视图，不是源事实。

---

## 21. Gate

- 无 `scope` 时使用默认 `docs-only`，不再阻塞。
- 写入治理计划后，未获用户确认不得执行文件修改、移动、桥接、归档或 authority 更新。
- 用户一次确认的是本批次治理计划的治理决策表。
- 不得把新的独立治理批次写入已有计划文件；目标路径已存在且不是同一 `governance_batch` 时，必须换唯一 slug 或询问用户。
- 有 `block` 项时，允许执行无依赖的非 block 项。
- 与 blocked 事实有依赖的变更必须跳过。
- 高影响事实必须 `block` 或明确等待用户确认。
- 未确认原始材料不得写入 authority。
- 不创建空 authority 子区或占位文档。
- `setup-doc-governance` 不得修改代码、测试、构建脚本或运行行为。
- Bridge 文件不得承载旧事实。
- Archive / evidence / derived / generated 文档不能默认作为当前事实。
- 执行后必须更新入口索引。

---

## 22. 验收标准

- 本批次治理计划存在，且包含 scope、governance_batch、plan_version、文件级决策表、事实级决策表、blocked 项和确认记录。
- 不存在默认生成的 `FACT_REGISTRY.md`、`KNOWLEDGE_CARDS.md`、`CONFLICT_REPORT.md`、`AUTHORITY_REBUILD_PLAN.md`、`CONTEXT_PACK_POLICY.md`。
- `docs/authority/README.md` 存在，并能路由到按需创建的 authority 子区。
- 没有空 authority 子目录或占位文档。
- 未确认原始材料位于 `docs/cases/<case-id>/evidence/` 或 archive，不进入 authority。
- Authority 文档只包含经确认或证据足够的事实，并具备最小 frontmatter。
- 高风险 authority 文档包含 `owner` 和 `last_verified`。
- 旧入口只在有误读风险时 bridge，bridge 文件薄且指向当前 authority 或 archive。
- 历史 / superseded 材料进入 `docs/archive/**`，不再默认作为当前事实。
- 入口索引能路由到 authority、治理计划、case evidence 和 archive。
- 执行结果写回同一个治理计划文件。
- 有 block 项时，状态为 `applied_with_blocks`，且 blocked 项保留待用户裁决。

---

## 23. 失败与恢复

如果文档太多：

- 缩小 `scope`。
- 先治理当前 case 或关键 docs 入口。
- 在治理计划中记录 deferred sources。

如果代码和历史文档冲突：

- 低风险内部实现且代码 + 测试一致，可以按代码 / 测试路由历史文档归档。
- 高风险契约、安全、权限、隐私、计费、数据删除、ADR 边界必须 `block`。

如果用户未确认计划：

- 停留在 `status: proposed`。
- 不执行文件修改。

如果用户确认后执行中发现新冲突：

- 停止相关变更。
- 写入 `Blocked Decisions`。
- 已安全完成的无关变更可保留，并在 Applied Result 中说明。

如果不是 git 仓库：

- 继续治理。
- 在治理计划中记录 `version_control: unavailable`。

---

## 24. 参考资料

- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
- [AI Coding 文档治理分层模型](<../../reference/docs/AI Coding 文档治理分层模型.md>)
- [AI Coding 文档生命周期管理与最佳实践](<../../reference/docs/AI Coding 文档生命周期管理与最佳实践.md>)
- [AI Coding 文档治理体系深度研究与落地方案](<../../reference/docs/AI Coding 文档治理体系深度研究与落地方案.md>)
- [AI Coding 文档治理体系的渐进式与规模化方案](<../../reference/docs/AI Coding 文档治理体系的渐进式与规模化方案.md>)
- [AI Coding 文档治理知识识别与流转机制](<../../reference/docs/AI Coding 文档治理知识识别与流转机制.md>)
- [AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制](<../../reference/docs/AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制.md>)
- [AI Coding 场景下的文档快速定位、索引设计与任务上下文生成](<../../reference/docs/AI Coding 场景下的文档快速定位、索引设计与任务上下文生成.md>)
