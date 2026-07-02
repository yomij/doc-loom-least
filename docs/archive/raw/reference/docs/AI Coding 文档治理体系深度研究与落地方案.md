---
status: archived
---

# AI Coding 文档治理体系深度研究与落地方案

## 研究结论与设计原则

AI Coding 场景下，文档治理的核心问题已经不再只是“把知识写下来”，而是“把知识组织成模型可稳定消费、团队可持续维护、自动化系统可验证执行的上下文基础设施”。Anthropic 把 context window 明确定义为模型的“working memory”，并明确指出上下文越长并不天然越好，随着 token 增长会出现 accuracy 和 recall 下降，也就是 context rot；OpenAI 与 Anthropic 又分别把 conversation state、compaction、prompt caching 与 evals 做成一等能力，说明上下文治理已经从 prompt 技巧升级为工程基础设施问题。citeturn34view0turn29view5turn28view1turn28view2turn35view0turn29view4

另一个非常清晰的行业收敛点是：主流 AI Coding 工具都在把“项目级、目录级、团队级指令”产品化。GitHub Copilot 支持 repository custom instructions、path-specific instructions、prompt files，以及以 Markdown + YAML frontmatter 描述的 custom agents；Claude Code 把 `CLAUDE.md`、auto memory 和 subagents 作为持久上下文机制；Windsurf 把 `AGENTS.md`、Rules、Workflows 作为目录级 instructions 与流程资产；Cursor 也把 Rules、Subagents、Automations、team-wide rules/shared context 做成产品能力。对文档治理的启示是：团队不应该把 AI 指令散落在聊天记录里，而要把它们变成 repo 内、可版本化、可审核、可组合的资产。citeturn39view1turn39view2turn39view3turn39view4turn34view3turn34view2turn38view0turn38view1turn38view2turn6search0turn6search11turn6search15turn5search4

大团队的共识也很明确：工程知识的长期可用性依赖三个条件同时成立。第一，文档要“靠近代码”，Backstage TechDocs 和 Software Catalog 都强调 docs-like-code、metadata YAML 与 ownership 的结合；第二，知识要有 owner 与 freshness 机制，Notion 的 page owners、verified pages、到期提醒就是很强的 freshness 治理模型；第三，知识必须可检索且可过滤，OpenAI retrieval/file search 已支持 attributes、filters、expiration、chunking，Sourcegraph Cody 则把 repo、open file、`@`-mention 与 context filters 结合到代码检索里。citeturn39view5turn39view6turn40view1turn40view3turn30view0turn32view1turn32view3turn33view0turn33view3turn7search1turn23search0turn23search3

基于这些事实，我给出的总设计原则只有五条，但每一条都必须落到制度和目录上。其一，**单一事实源**：工程真相以 repo 为主，外部 wiki 只做消费层或协作层。其二，**分层上下文**：稳定知识、动态知识、运行时知识、AI 指令必须分层，而不是混在一个 `/docs` 目录里。其三，**可执行文档**：规则、工作流、agent profile、API contract、lint config 都属于“可执行文档”，应与 narrative docs 分开治理。其四，**先验证、再沉淀**：AI 生成的内容可以很多，但进入长期知识库前必须有 owner、review 与 eval。其五，**一份策略，多端适配**：不要分别维护 Copilot instructions、CLAUDE.md、AGENTS.md、Windsurf Rules，应该维护一组 canonical policy fragments，再编译到不同工具。这个方向与 GitHub、Claude Code、Windsurf、Cursor 的产品形态高度一致。citeturn39view4turn34view3turn38view0turn6search0

## 分层模型与文档类型

我推荐把 AI Coding 文档体系分成六层：**目录与元数据层、长期稳定知识层、高频变更知识层、可执行 AI Context 层、运行时上下文与运维记忆层、生成视图层**。这个模型吸收了 Backstage 的 metadata YAML + ownership、GitHub/Copilot 的 instruction hierarchy、Claude Code 的 memory、Windsurf 的 AGENTS/Rules/Workflows，以及 OpenAI retrieval 的 metadata/filter 机制。citeturn39view6turn39view4turn34view3turn38view0turn38view2turn32view1turn32view3

### 推荐层级模型

| 层级 | 目标 | 典型资产 | 稳定性 | 推荐载体 | 维护主责 |
|---|---|---|---|---|---|
| 目录与元数据层 | 定义拥有权、可发现性、可过滤性 | `catalog-info.yaml`、frontmatter、owner、tags、review SLA | 稳定 | YAML / JSON | 平台团队 + Domain Owner |
| 长期稳定知识层 | 保存团队长期不会频繁变化的“认知骨架” | 架构原则、coding standards、术语表、服务边界、ADR log | 低频变更 | Markdown + diagrams-as-code | 架构 owner / 平台 owner |
| 高频变更知识层 | 服务交付与变更管理 | Product Spec、System Design、Release Note、Changelog、API 变更说明 | 高频 | Markdown + structured frontmatter | PM / Tech Lead / Service Owner |
| 可执行 AI Context 层 | 直接驱动 AI 行为 | Prompt 资产、Agent Instructions、Workflow、技能说明、任务模板 | 高频且需要控变 | Markdown、YAML frontmatter、JSON manifest | AI Maintainer + Domain Owner |
| 运行时上下文与运维记忆层 | 面向 incident、运行状态与经验反馈 | Runbook、Postmortem、Known Issues、Runtime Snapshot、Memory | 实时或事件驱动 | Markdown + JSON snapshots | SRE / Oncall / Service Owner |
| 生成视图层 | 把源资产编译成可消费视图 | TechDocs、API 参考站点、Context Pack、RAG 索引、摘要页 | 自动生成 | HTML / JSON / embedding index | 平台自动化 |

下面这张表是我最推荐的“文档类型—职责边界—格式—审核”矩阵。它不是照搬某一家公司的目录，而是对目前主流 AI Coding 工具与大团队知识治理机制的工程化抽象。citeturn39view1turn39view3turn34view3turn38view0turn38view2turn39view5turn39view6turn40view1

| 资产类型 | 属于哪一类 | 主要职责 | 最佳载体 | 谁可自动维护 | 必须人工审核 |
|---|---|---|---|---|---|
| Product Spec | 高频变更知识 | 说明业务目标、约束、验收标准 | Markdown | AI 可草拟摘要、拆任务 | PM / EM |
| System Design | 稳定知识 + 高频变更知识 | 描述方案、边界、权衡、接口关系 | Markdown | AI 可生成初稿与图例说明 | 架构师 / Tech Lead |
| ADR | Decision Record | 记录重大架构决策、备选项、后果 | Markdown | AI 可草拟与补 history | 架构 owner |
| API Contract | API Contract | 机器可执行的接口真相 | OpenAPI / Proto / GraphQL SDL | 自动生成参考文档 | 接口 owner |
| Coding Standard | Coding Standard | 统一代码风格与 review 基线 | Markdown + lint/format config | AI 可提 PR | 平台 owner |
| SOP / Runbook | SOP | 指导重复执行的步骤 | Markdown + checklist | AI 可根据 incident 补全 | Oncall / SRE |
| Incident / Postmortem | Incident / Postmortem | 记录影响、根因、修复与 follow-up | Markdown | AI 可生成时间线与摘要 | Incident commander / owner |
| Prompt 资产 | Prompt 资产 | 复用 prompt、变量、输出约束、评估集 | Markdown / YAML | AI 可生成 variants | Prompt owner |
| Workflow | Workflow | 规定多步 agentic 轨迹 | Markdown / YAML manifest | AI 可草拟 | Workflow owner |
| Agent Instructions | Agent Instructions | 规定 AI 在 repo / 目录 / 任务中的行为边界 | `AGENTS.md` / `CLAUDE.md` / `.github/copilot-instructions.md` / agent profile | 可由 canonical policy 编译 | 平台 + Domain Owner |
| Memory | Memory | 保存经验、反复出现的问题、偏好与 lessons learned | JSONL / Markdown | AI 可写入候选记忆 | Owner 定期清理/晋升 |
| Runtime Context | Runtime Context | 提供运行中的最新状态 | 结构化 JSON / metrics snapshot | 完全自动生成 | 不允许手工改真相 |
| Context Pack | AI Context | 编译任务所需最小上下文集合 | YAML + compiled Markdown | 完全自动生成 | 高风险任务抽检 |

### 什么适合 Markdown，什么适合 YAML / JSON，什么应当代码即文档

行业实践已经把载体选择的边界画得很清楚了：Backstage 的 catalog entities 强调“人可维护的 YAML 元数据”；GitHub custom agents 用 Markdown + YAML frontmatter；Windsurf Workflows 与 AGENTS.md 是 Markdown；OpenAI retrieval/file search 又把 filters、attributes、expiration、chunking 做成结构化字段。由此推导出的最佳实践是：**叙述型知识用 Markdown，机器消费的控制面用 YAML/JSON，真正的系统真相尽量代码即文档。**citeturn17search1turn39view3turn38view0turn38view2turn32view1turn32view2turn33view0

| 载体 | 最适合的内容 | 不适合的内容 |
|---|---|---|
| Markdown | 设计 rationale、规则说明、SOP、ADR、团队约定、可读示例 | 高频变动的结构化索引、机器过滤字段、运行时状态 |
| YAML / JSON | metadata、owner、tags、review SLA、agent profile、context pack manifest、retrieval filters | 长篇叙述、复杂论证、长篇设计说明 |
| 代码即文档 | API schema、数据库 schema、IaC、lint/formatter、测试样例、CLI help、examples | 业务背景、决策权衡、事故经验 |
| 自动生成 | API reference、catalog page、dependency/service graph、release note draft、context pack、vector index | 最终决策、根因判断、对外承诺 |
| 适合 AI 维护 | 摘要、候选记忆、文档更新建议、变更影响分析、prompt variants、release note 初稿 | 版本策略、breaking change 判定、合规要求、incident root cause |
| 必须人工审核 | Product Spec、架构结论、ADR 定稿、对外 API 变更、运行手册、事故结论 | 无 |

### 推荐命名规范、标签体系与 metadata schema

Google 的技术写作课程强调大型文档应按读者任务组织，并在引入陌生术语时建立 glossary；GitHub Docs 强调一致的内容模型与可扫描性；Backstage 则证明 ownership 和 metadata 必须进入源文件本身。基于这些做法，我建议命名、标签与 frontmatter 按“**稳定 ID + 受控枚举 + 可追踪 ownership**”设计，而不是纯自由文本。citeturn11search22turn11search15turn10search21turn39view6

**命名规范建议：**

| 规则 | 建议 |
|---|---|
| 目录名 | 全小写复数，如 `specs`、`runbooks`、`prompts` |
| 文件名 | `kebab-case`，避免空格与日期前缀污染稳定文档 |
| ADR | `ADR-0042-use-event-driven-invoice-sync.md` |
| Spec | `SPEC-checkout-refund-flow.md` |
| Runbook | `RUNBOOK-api-rate-limit.md` |
| Postmortem | `PM-2026-05-18-cache-stampede.md` |
| Context Pack | `ctxpack-checkout-pr-review-v3.yaml` |
| Prompt 资产 | `pr-review.security.prompt.md` |
| Workflow | `release-service.workflow.md` |

**标签体系建议：**

| 维度 | 例子 | 说明 |
|---|---|---|
| `artifact` | `spec` `adr` `runbook` `prompt` `workflow` `memory` | 资产类型 |
| `domain` | `checkout` `auth` `platform` `ml-infra` | 业务/技术域 |
| `service` | `svc-billing-api` | 绑定服务 |
| `stability` | `stable` `dynamic` `runtime` | 上下文层级 |
| `audience` | `human` `ai` `both` | 主要消费对象 |
| `risk` | `p0` `p1` `p2` `p3` | 影响等级 |
| `phase` | `plan` `build` `review` `release` `operate` | 生命周期阶段 |
| `source_of_truth` | `code` `manual` `generated` `external` | 真相来源 |

**推荐 metadata frontmatter 示例：**

```yaml
---
id: adr-auth-session-expiry
title: Use sliding session expiry for dashboard access
artifact: adr
domain: auth
service: svc-dashboard-auth
audience: both
stability: stable
status: active
owners:
  doc_owner: team-auth-platform
  domain_owner: architect-identity
reviewers:
  - team-security
review_cycle_days: 180
last_verified_at: 2026-05-20
next_review_at: 2026-11-16
source_of_truth: manual
human_review_required: true
code_refs:
  - services/auth/session.ts
contracts:
  - apis/openapi/auth.yaml
related:
  - ADR-0038
  - RUNBOOK-auth-session-incident
ai_usage:
  include_in_context_pack: true
  retrieval_priority: high
  allow_summary_generation: true
  promote_to_memory: false
---
```

这个 schema 的关键不是字段多少，而是把 **owner、freshness、status、code_refs、AI usage** 强制标准化。这样才能同时支撑 Backstage catalog、RAG filtering、Context Pack 组装、Freshness 审计与 CODEOWNERS 审查。citeturn39view6turn39view0turn32view1turn32view2

## Repo 结构与架构图

### 推荐 Repo 目录结构

我建议中大型 AI Native 团队采用下面这套结构。它兼容 GitHub/Copilot、Claude Code、Windsurf、Backstage/TechDocs，也适合后续接入 RAG、MCP 与 Agent Workflow。GitHub 已把 repository-wide instructions、path-specific instructions、agent instructions 变成 repo 资产；Windsurf 直接把 `AGENTS.md` 和 `.windsurf/workflows/` 作为上下文与流程入口；Backstage 以 `catalog-info.yaml` 和与代码同仓的 Markdown 作为 catalog 与 docs 的来源。citeturn39view1turn39view4turn38view0turn38view2turn39view5turn39view6

```text
/
├─ catalog-info.yaml
├─ mkdocs.yml
├─ AGENTS.md
├─ CLAUDE.md
├─ docs/
│  ├─ architecture/
│  ├─ standards/
│  ├─ glossary/
│  ├─ domains/
│  └─ onboarding/
├─ specs/
│  ├─ product/
│  ├─ system/
│  └─ changes/
├─ adr/
├─ apis/
│  ├─ openapi/
│  ├─ graphql/
│  ├─ protobuf/
│  └─ examples/
├─ runbooks/
├─ postmortems/
├─ prompts/
│  ├─ review/
│  ├─ test/
│  ├─ refactor/
│  └─ design/
├─ workflows/
├─ agents/
│  ├─ profiles/
│  ├─ policies/
│  └─ adapters/
├─ context/
│  ├─ manifests/
│  ├─ packs/
│  └─ snapshots/
├─ memory/
│  ├─ lessons/
│  ├─ known-issues/
│  └─ decisions/
├─ .github/
│  ├─ copilot-instructions.md
│  ├─ instructions/
│  └─ workflows/
├─ .claude/
│  └─ rules/
└─ .windsurf/
   ├─ rules/
   └─ workflows/
```

### 每层职责解释

| 目录 | 职责 | 说明 |
|---|---|---|
| `catalog-info.yaml` | 软件目录入口 | 绑定 owner、system、component、links，供 Backstage/catalog 消费 |
| `docs/` | 长期稳定知识 | 架构原则、标准、术语、入门资料 |
| `specs/` | 高频变更知识 | 产品需求、方案设计、变更设计，不等于长期“真相” |
| `adr/` | 决策日志 | 只记录 architecturally significant decisions |
| `apis/` | 机器真相层 | schema、样例、mock、compat notes |
| `runbooks/` | 可执行 SOP | 发布、回滚、告警处理、容量扩展 |
| `postmortems/` | 事故学习层 | 事件、时间线、根因、行动项 |
| `prompts/` | Prompt 资产 | 版本化 prompt、评估数据、模板变量 |
| `workflows/` | 跨工具流程 | 面向 agent 的多步任务脚本 |
| `agents/` | Agent 治理中心 | canonical policies、agent profiles、tool adapters |
| `context/` | AI 编译产物 | context pack manifest、临时快照、索引中间件 |
| `memory/` | 经验与候选记忆 | known issues、lessons learned、待晋升的记忆 |
| `.github/` | GitHub 适配层 | Copilot instructions、PR/CI policies、Actions |
| `.claude/` / `CLAUDE.md` | Claude 适配层 | 持久 instructions / memory rules |
| `.windsurf/` / `AGENTS.md` | Windsurf 适配层 | rules、workflows、目录级 instructions |

### 最推荐的架构图文字版

```text
作者层
人类作者 / AI 助手
    ↓
源资产层
Specs / ADR / API Contracts / Standards / Runbooks / Prompts / Agent Policies
    ↓
治理层
CODEOWNERS + Branch Protection + Doc Schema Lint + Link Check + Drift Check + Review SLA
    ↓
发布层
Git merge → TechDocs / Backstage Catalog / API Docs / Changelog / Release Note
    ↓
索引层
Chunking + Metadata enrichment + Embeddings + Lexical index + Vector DB + Graph index
    ↓
消费层
ChatGPT / Claude / Cursor / Windsurf / Copilot / Sourcegraph / Internal Agents
    ↓
上下文编译层
Context Pack Builder + Retrieval + MCP + Session State + Compaction + Prompt Cache
    ↓
反馈层
Usage telemetry + Eval results + Incident learnings + Memory proposals + Freshness reminders
    ↓
回写层
更新 Runbook / ADR / Standards / Prompt Assets / Agent Policies
```

这张图最重要的不是“有多少系统”，而是四条流必须打通：**信息流、治理流、AI 交互流、反馈回写流**。Backstage 推荐在 CI/CD 里生成 TechDocs 并把前台变成 read-only；GitHub Actions 本质上就是 repository-native 自动化控制面；OpenAI/Anthropic 的 eval、state、compaction、prompt caching 能力则让“上下文编译层”真正可工程化。citeturn17search2turn10search2turn10search5turn10search17turn29view4turn28view1turn28view2turn35view0

## 治理模型与生命周期

### 生命周期治理

文档生命周期不应只有“创建”和“更新”，而应明确为：**Draft → In Review → Active → Needs Refresh → Superseded → Archived → Deleted**。Notion verified pages 的做法非常值得借鉴：内容可以被 owner 验证并设置有效期，到期后自动提醒 re-verify；AWS ADR 最佳实践强调保留历史、以 Superseded 而不是覆盖的方式处理决策演进；GitHub 的 CODEOWNERS 与 branch protection 则提供了 review gate。citeturn40view1turn40view3turn18search7turn39view0turn10search20

我建议把生命周期规则写成下面这套制度：

| 状态 | 进入条件 | 退出条件 | 自动化动作 |
|---|---|---|---|
| Draft | 新建文档 | 提交 review | schema 校验、指派 owner |
| In Review | PR / MR 打开 | reviewer 通过 | CODEOWNERS、lint、AI diff summary |
| Active | 合并到主干 | 到期或被 supersede | 索引、发布、纳入检索 |
| Needs Refresh | 到达 `next_review_at` 或命中 drift | 重新验证或归档 | owner 提醒、生成更新建议 |
| Superseded | 新版本替代 | 长期保留 | 建立 `supersedes` 链接 |
| Archived | 不再 operational，但保留历史 | 删除 | 从默认检索降权 |
| Deleted | 法务/安全/冗余清理 | 无 | 删除索引与引用 |

### Ownership 机制

Backstage 把 ownership 和 metadata 作为软件目录的中心；GitHub CODEOWNERS 把 review ownership 下沉到路径；Notion verified pages 又证明 freshness 必须绑定 owner；AWS ADR 建议每条 ADR 都有 owner 和 change history。综合起来，最合理的角色模型如下。citeturn39view6turn39view0turn40view1turn18search7

| 角色 | 核心职责 | 是否必须存在 |
|---|---|---|
| Doc Owner | 对单篇文档的正确性与 freshness 负责 | 必须 |
| Domain Owner | 对一组文档的语义一致性负责 | 必须 |
| AI Maintainer | 负责索引、摘要、更新建议、context pack、eval 流水线 | 强烈建议 |
| Reviewer | 负责语义审核、合规审核或运行可执行性审核 | 必须 |
| Knowledge Platform Owner | 负责 schema、搜索、门户、自动化、指标 | 团队级必须 |

**关键规则：**  
Doc Owner 负责“这篇文档准不准”；Domain Owner 负责“同一主题下的多篇文档是否互相一致”；AI Maintainer 负责“AI 能不能正确找到并正确消费这些文档”；Reviewer 负责“进入长期知识库之前是否达到门槛”。没有 owner 的文档不得进入 Active 状态。

### 版本治理

Google code review 指南强调 reviewer 首先看 overall design；GitHub branch protection 可以强制通过 review 和 status checks；Stripe、Uber、Microsoft Graph 都把 API versioning、upgrade、changelog、breaking change notice 做成正式制度；ADR 领域的通用实践也强调决策历史不可抹除。对 AI Coding 文档治理来说，这意味着“文档版本”必须与“代码版本”和“接口版本”形成联动，而不是单独存在。citeturn11search11turn10search20turn14search0turn14search1turn14search5turn15search2turn15search4turn12search4turn18search4turn18search7

我建议采用这套版本规则：

| 资产 | 版本策略 |
|---|---|
| 代码同仓文档 | 跟随 Git 版本与分支，必须与代码同 PR 演进 |
| ADR | 编号不可变，内容只允许状态变更和 supersede 链接 |
| API Contract | 以 schema 版本为准，breaking change 必须强制 changelog |
| Release Note | 由 PR label、merged ADR、API diff 自动草拟，人工确认 |
| Prompt / Workflow | 语义版本化，模型升级或指令改动触发 eval 回归 |
| Runbook | 非语义版本化，但每次 incident 后都应记录 revision |

### 质量治理与指标体系

OpenAI 明确把 evals 定义为保证 LLM 应用可靠性的关键组件；Anthropic 进一步指出 agent eval 需要把工具、环境和多轮过程纳入测试；Google SRE 强调 postmortem 的书面化与持续改进；Notion verified pages 则天然适合做 freshness 度量。因此，文档治理不能只看“有没有文档”，而要看“文档是不是新鲜、可执行、可检索、可验证”。citeturn29view4turn35view1turn11search1turn40view1

我推荐团队至少跟踪下面这些指标：

| 指标 | 定义 | 目标 |
|---|---|---|
| Freshness SLA 达成率 | 处于 Active 且在 SLA 内完成验证的文档占比 | > 90% |
| Owner 完整率 | 带 `doc_owner` 与 `domain_owner` 的文档占比 | 100% |
| Doc-Change 联动率 | 涉及代码/接口变更且同时更新文档的 PR 占比 | > 80% |
| Drift Backlog | 已发现但未修复的 doc/code drift 数量 | 持续下降 |
| Dead Doc Ratio | 90 天无访问且无引用、无 owner 的文档占比 | < 10% |
| Retrieval Precision@K | 检索前 K 条中真正相关文档比例 | > 0.8 |
| Context Pack 命中率 | Agent 任务中无需再次追问关键信息的比例 | 持续提高 |
| Prompt Regression Score | Prompt/Workflow 升级后 golden tasks 通过率 | 不低于基线 |
| Postmortem Closure Rate | 事故 action items 按期关闭比例 | > 85% |
| Review Lead Time | 文档从 Draft 到 Active 的中位时长 | 团队自定并逐季降低 |

## AI Context Engineering 与 Agent Memory

### 上下文供给模型

最有效的上下文不是“把所有文档都塞给模型”，而是分层编译。Anthropic 直接指出 context 需要 curate，而不是只追求更大；OpenAI 提供 stateful conversation、`previous_response_id`、Conversations API 与 compaction，说明长会话应通过状态管理与压缩解决，而不是全文回放；Claude Code subagents 又证明应该把探索性工作放在独立 context window 里，避免主会话被搜索结果、日志和文件内容淹没。citeturn34view0turn29view5turn28view1turn34view2

因此，我建议把 AI 消费上下文分成七层，从上到下优先级递减：

| 层级 | 内容 | 典型来源 |
|---|---|---|
| 组织级策略 | 安全、合规、通用工程规范 | 平台规则、系统级 rules |
| Repo 全局上下文 | 架构总览、全局约定、构建/测试命令 | `AGENTS.md`、`CLAUDE.md`、`.github/copilot-instructions.md` |
| 路径级上下文 | 目录特定规范、局部架构、子域规则 | path instructions、目录级 `AGENTS.md` |
| 任务级上下文 | 当前需求、PR、issue、workflow、agent profile | `specs/`、`workflows/`、custom agent profile |
| 检索上下文 | 与问题直接相关的 canonicals、API、ADR、runbook | RAG / code search / vector store |
| 运行时上下文 | 日志、告警、feature flags、部署状态、失败用例 | MCP、monitoring、CI artifacts |
| 会话与记忆 | compacted state、上一轮输出、经验记忆 | Conversations、compaction、memory DB |

### Context Pack 设计

OpenAI 的 prompt caching 明确建议把 static content 放在 prompt 前部、dynamic content 放在后部；Anthropic prompt caching 也区分自动缓存与显式缓存点，并说明它适合 large context、many examples、long multi-turn conversations；OpenAI 还支持 compaction 把长窗口压缩成更短的 canonical next window。由此最自然的工程做法，就是为 AI 任务构建 **Context Pack**：一个由静态前缀、动态增量、运行时证据和输出契约组成的最小上下文包。citeturn28view2turn28view3turn35view0turn28view1

我推荐的 Context Pack 结构如下：

```yaml
task:
  id: pr-review-checkout-4821
  objective: review
  risk: p1
policy_pack:
  - docs/standards/review-guidelines.md
  - docs/standards/security-baseline.md
repo_pack:
  - docs/architecture/checkout-overview.md
  - apis/openapi/checkout.yaml
delta_pack:
  - specs/changes/checkout-refund-retry.md
  - adr/ADR-0042-use-idempotency-window.md
evidence_pack:
  - ci/test-failures.json
  - observability/error-snapshot.json
memory_pack:
  - memory/known-issues/refund-duplicate-charge.md
output_contract:
  must_update:
    - runbooks/refund-retry.md
  must_not_change:
    - apis/openapi/auth.yaml
```

### 减少 Token 浪费与 Context Compression

我建议把 token 控制做成制度，而不是优化技巧：

| 策略 | 做法 | 结果 |
|---|---|---|
| 静态前缀缓存 | 组织级策略、repo 规则、长期稳定知识固定放前缀 | 提升 prompt cache hit rate |
| 分层摘要 | 每篇长文保留 full / brief / 200-token abstract 三层摘要 | 检索时按需加载 |
| 只取 canonical | AI 默认只读“已验证、未过期、Active”的文档 | 降低过时信息注入 |
| 子代理隔离 | 搜索、日志分析、依赖追踪交给 subagent | 主会话保持干净 |
| 状态压缩 | 长会话定期 compact，而非全量 replay | 保持上下文可持续 |
| 输出契约 | 明确生成目标与必须更新文件 | 减少无关游走 |

### 分层检索与 Agent Memory

OpenAI retrieval 允许给 vector store files 设置 attributes、attribute filtering、expiration policy 和 chunking；默认 chunk 是 800 token / 400 overlap，也允许自定义 chunk size/overlap；Sourcegraph Cody 则把 open file、repo、`@`-mentioned files/symbols 和 context filters 一起用于代码检索；Claude citations 建议如果希望按 RAG chunk 精确引用，就把 chunk 作为 plain text documents，并用可引用内容区分 source/title/context。最佳实践由此很清楚：**检索层必须既有语义向量，也有结构化 metadata 和 lexical/code search。**citeturn32view1turn32view2turn32view3turn33view0turn33view3turn23search3turn23search0turn35view2

我建议 Agent Memory 分三档管理：

| 记忆类型 | 内容 | TTL | 晋升条件 |
|---|---|---|---|
| Ephemeral Session Memory | 当前会话临时结论、待办、假设 | 会话级 | 默认不晋升 |
| Working Memory | 最近 7–30 天反复命中的经验、known issues、调试发现 | 短期 | 访问频繁且未被新事实推翻 |
| Durable Memory | 编码规范、架构偏好、固定流程、常见坑 | 长期 | 人工审核后写入 `CLAUDE.md` / `AGENTS.md` / standards |

Claude Code 的 auto memory 可以作为“候选记忆”机制，而不是直接真相源；Notion verified pages 和 owner/expiry 模型则非常适合做 durable memory 的 freshness 门槛。也就是说：**AI 可以记住，但只有经过 owner 验证的记忆才应该成为长期上下文。**citeturn34view3turn40view1turn40view3

### 人机协同边界

Google 的 code review 标准强调首先审 design；Google SRE 强调 postmortem 要成为学习机会；Anthropic 与 OpenAI 都把 evals 放在可靠性核心。这决定了人机边界不能模糊。citeturn11search11turn11search1turn29view4turn35view1

| 必须人工决策 | 适合 AI 自动化 | 适合 Human-in-the-loop |
|---|---|---|
| 产品范围取舍、架构定案、breaking change、合规与隐私边界、事故根因结论、记忆晋升/删除 | 摘要、索引、context pack、变更影响分析、release note 初稿、runbook diff 建议、prompt variants、死文档扫描 | 设计初稿、ADR 草稿、postmortem 时间线、重构计划、review 评论生成、runbook 更新、memory promotion |

## AI Native 工作流与自动化

### 端到端文档工作流

下面这套工作流覆盖产品需求、架构、编码、Review、测试、发布、事故、重构和 agent 协作。它的重要区别是：**每一步都同时产出代码和文档信号**，并且这些信号会被自动回流到 AI context 基础设施。OpenAI/Anthropic 的 eval 与 state 机制，GitHub Actions 的 repo-native 自动化，Windsurf 的 Workflow、Claude Code 的 MCP/subagents、Sourcegraph 的 code context，都支持这个工作流落地。citeturn29view4turn35view1turn10search2turn38view2turn34view4turn34view2turn23search3

| 阶段 | 人类主动作 | AI 消费什么 | AI 产出什么 | 必须更新的文档 |
|---|---|---|---|---|
| 需求 | PM/EM 明确目标与边界 | 既有 spec、glossary、历史 incidents | spec 初稿、验收标准建议、影响分析 | `specs/product` |
| 架构设计 | Tech Lead 做 tradeoff | architecture、ADR、API contracts | design 初稿、ADR 草案、风险清单 | `specs/system`、`adr` |
| 编码 | 开发实现 | repo/global/path rules、API schema、workflow | 代码、测试、文档补丁建议 | `apis`、`docs/domains` |
| Code Review | reviewer 看 design/风险 | PR diff、standards、ADR、runbook | review comments、checklist | `prompts/review`、必要文档联动 |
| 测试 | QA/Dev 验证 | acceptance criteria、failure corpus | test plan、case 生成、缺陷摘要 | `specs/changes`、`runbooks` |
| 发布 | release owner 执行 | workflow、runbook、release notes | release note draft、变更摘要 | `runbooks`、`specs/changes` |
| Incident | oncall 处置 | runbook、runtime snapshot、known issues | timeline、行动项、doc updates | `postmortems`、`runbooks`、`memory` |
| 重构 | owner 规划债务偿还 | code graph、usage、ADR、tests | refactor plan、风险评估、迁移文档 | `adr`、`docs/architecture` |
| Agent 协作 | orchestrator 分工 | agent profiles、workflow、context packs、MCP | 子任务结果、汇总报告、更新建议 | `agents`、`workflows`、`context` |

### 自动化治理栈

#### GitHub Actions 与 CI/CD

GitHub 明确说明 Actions 是 repository-native 的 CI/CD 平台，工作流由 YAML 定义，且 branch protection 可强制 review 和状态检查；CODEOWNERS 可把 review ownership 绑定到路径。对文档治理来说，这意味着 **治理必须下沉到 CI，而不是靠人记忆**。citeturn10search2turn10search5turn10search17turn10search20turn39view0

我推荐在 CI 里最少做九类检查：

| 自动化任务 | 触发时机 | 实现方式 |
|---|---|---|
| metadata schema 校验 | PR | frontmatter lint / JSON Schema |
| CODEOWNERS 路径审查 | PR | GitHub 原生 |
| 文档-代码联动检查 | PR | path rule：改 `apis/` 或 `src/contracts/` 必须有 docs label 或 docs diff |
| OpenAPI / Proto diff | PR | schema diff tool |
| dead link / broken anchor | PR + nightly | docs lint |
| Freshness 扫描 | nightly | 根据 `next_review_at` 提 issue |
| drift 检测 | merge + nightly | code/schema/IaC 与 docs 比较 |
| 生成 TechDocs / API docs | merge | CI build + publish |
| 重新索引与 context pack 编译 | merge | embedding/index pipeline |

#### MCP、RAG、Embedding 与 Vector DB

MCP 的定位已经非常清晰：它是 AI 连接外部工具和数据源的开放标准，Claude Code、GitHub Copilot、Windsurf 都支持 MCP 方向的扩展；但不同表面能力并不等价，GitHub Copilot cloud agent 当前只支持 MCP 的 tools，不支持 resources 或 prompts，而 Claude Code 则能使用 tools、resources 与 MCP prompts。结论是：**你需要把“检索文档”和“调用工具”设计成两条可独立退化的通道**，不能假设所有 agent surface 能吃同一种上下文接口。citeturn19search3turn19search7turn34view4turn21search1turn21search10turn21search15turn38view3

我推荐的检索架构是：

| 层 | 建议 |
|---|---|
| 词法检索 | 代码与配置走 lexical / symbol / grep，如 Sourcegraph 式代码检索 |
| 向量检索 | narrative docs、postmortem、spec 走 embeddings |
| 元数据过滤 | 必须支持 `artifact/domain/service/status/verified/owner/date/risk` |
| 图关系 | service → API → ADR → runbook → postmortem 建关系边 |
| 运行时连接 | 通过 MCP 接 issue、监控、CI、feature flag、设计系统 |
| 过期控制 | runtime snapshot 和临时 contexts 必须 TTL 化 |

OpenAI retrieval 的 attributes/filtering/expiration/chunking 机制非常适合做这个设计的最小实现；Notion Q&A 与 Claude citations 则提醒我们，回答必须尽量带来源并在没有答案时显式失败，而不是编造。citeturn32view1turn32view2turn32view3turn33view3turn39view8turn35view2turn34view5

#### 自动摘要、自动更新建议、drift 检测与 eval

OpenAI 认为 evals 是构建可靠 LLM 应用的关键；Anthropic 进一步把 agent eval 定义为输入、工具、环境、grader 的组合；Microsoft prompt flow / Foundry 也把 evaluation、variants、monitoring、RAG-specific metrics、agent-specific metrics 做成正式能力。对文档治理而言，这意味着“文档自动化”也必须有评估与监控，而不是只生成文件。citeturn29view4turn35view1turn12search0turn12search12turn12search14turn12search21

我建议至少建立四类评估：

| Eval 类型 | 评估什么 | 示例 |
|---|---|---|
| Retrieval Eval | 检索是否拿到正确文档 | P@5、MRR、答案引用是否来自 verified docs |
| Prompt Eval | prompt/workflow 升级是否回归 | golden tasks、style compliance、token cost |
| Agent Eval | 多步任务是否完成 | PR review 是否发现关键风险、incident workflow 是否正确 |
| Governance Eval | 自动化治理是否有效 | drift false positive rate、stale reminder 命中率 |

**drift 检测** 我建议分三层做：  
第一层是 **code/doc drift**，比较 API schema、CLI help、config schema、test names、directory structure 与文档描述。  
第二层是 **runtime/doc drift**，比较 runbook 中的操作步骤与真实部署脚本、监控字段、feature flags。  
第三层是 **prompt drift**，当模型升级、指令改动、工具集变化时，自动重跑 golden eval，并将失败 prompt 标记为 `needs_refresh`。  

## 最推荐的 Best Practice 架构

如果只能给出一套最推荐的架构，我会选：

### Repo-Native Canonical Knowledge 加 Catalog 加 Compiled Agent Instructions 加 Context Pack Pipeline

它的核心不是“把所有知识都塞进一个知识库”，而是：

1. **工程真相以 repo 为主**：API、架构、规范、runbook、ADR、prompt、workflow、agent policy 都版本化进 Git。  
2. **知识发现以 Catalog/Portal 为主**：用 Backstage / TechDocs 或同类平台做统一发现、owner 浏览、服务地图与入口导航。citeturn39view5turn39view6  
3. **AI 指令采用“canonical → adapter” 编译模式**：  
   - canonical policy 只维护一份，放在 `agents/policies/`  
   - 自动生成 `AGENTS.md`、`CLAUDE.md`、`.github/copilot-instructions.md`、Windsurf Rules、Cursor Rules/Team Rules 等适配文件  
   - 避免同一条规则在多个 AI 工具里手工复制而漂移。GitHub、Claude Code、Windsurf 都已经把这些文件当成正式产品接口，这使“编译适配”比“人工复制”更合理。citeturn39view1turn39view4turn34view3turn38view0turn38view1turn6search0  
4. **Context Pack 作为 AI 任务入口**：每个 agentic 任务先编译一个最小上下文包，而不是直接把 repo 全量灌给模型。  
5. **Freshness 与 Owner 强制化**：借鉴 Notion verified pages，把关键文档做 owner、过期时间、re-verify reminder。citeturn40view1turn40view3  
6. **所有 prompt / workflow 都必须配 eval**：否则它们不是资产，只是猜测。citeturn29view4turn35view1  
7. **API/Schema/Style/IaC 走代码即文档**：借鉴 Stripe/Uber/Microsoft 的 versioning/changelog 思路，以及 Airbnb style guide 对 autocorrect / lint / best practice 的层次区分，把“规范”尽量落到可执行配置。citeturn14search0turn14search1turn15search2turn15search4turn12search1turn16search9  
8. **事故后必须回写**：postmortem 不只写在 `postmortems/`，还必须决定是否更新 `runbooks/`、`memory/`、`adr/` 和 `standards/`。Google SRE 的 blameless postmortem 文化本质上就是把失败转成系统知识。citeturn11search1turn11search13

### 这套架构的 tradeoff

| 选项 | 优点 | 代价 | 我的判断 |
|---|---|---|---|
| 全部放外部 wiki | 易写易协作 | 与代码脱节、权限/版本/检索不稳定 | 不建议作为工程真相层 |
| 全部放 repo | 与代码强绑定、可审核、可自动化 | 产品/运营类知识协作较弱 | 适合作为 engineering source of truth |
| Repo + Portal | 同时兼顾真相与可发现性 | 需要平台建设成本 | **最推荐** |
| 多工具各维护一份 instruction | 上手快 | prompt 漂移极快 | 不可扩展 |
| canonical policy + adapters | 一致性强、便于治理 | 需要编译与同步工具 | **长期收益最高** |

### 落地节奏建议

| 阶段 | 目标 | 必做项 |
|---|---|---|
| 基础期 | 建立最小治理骨架 | repo 结构、metadata schema、CODEOWNERS、ADR 模板、root instructions |
| 规范期 | 建立 freshness 与自动发布 | TechDocs/catalog、review SLA、verified docs、changelog / API diff |
| 智能期 | 建立 AI consumption pipeline | vector index、metadata filters、context pack builder、memory flow |
| Agent 期 | 建立 agentic workflow | MCP、workflow library、subagents、prompt/workflow evals、drift loops |

**一句话总结这套 Best Practice：**  
**把文档体系设计成“面向人阅读的知识层 + 面向机器检索的元数据层 + 面向 Agent 执行的指令层 + 面向运行时反馈的记忆层”，并用 CI、Catalog、RAG、MCP、Evals 把四层串成闭环。** 这比单纯做“文档平台”，更接近真正的 AI Native Engineering。citeturn39view6turn39view5turn32view1turn29view4turn19search3turn34view4

## 开放问题与边界

这份方案已经足够落地，但仍有几个边界需要你在实施前明确。第一，公开资料能清楚看到 GitHub、Anthropic、OpenAI、Windsurf、Backstage、Notion、Stripe、Uber、Microsoft 等平台的机制，但 Cursor、Google、Airbnb、Uber 等公司内部“文档治理细则”公开程度并不一致，因此文中关于大团队治理细节有一部分是基于公开能力和行业模式的工程化综合，而不是逐字复刻某家公司内部流程。第二，不同 AI surface 对 MCP 的支持面并不完全一致，所以你需要在“检索文档”“调用工具”“注入 prompt”三类上下文之间做兼容层设计。第三，如果你们的工程知识仍大量留在 Confluence / Notion / Jira / Slack，真正的难点不是建新目录，而是先把 canonical truth 收拢到 repo 与 catalog。这个迁移成本需要在项目排期里单独计入。citeturn21search1turn21search10turn21search15turn34view4