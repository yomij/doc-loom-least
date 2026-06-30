# AI Coding 文档治理体系的渐进式与规模化方案

## 总体判断

在 AI Coding 已经深入代码生成、重构、测试、Review、运维与知识检索的前提下，文档治理不再只是“把文档写好”，而是要把**文档、代码、运行态、决策历史、Agent 指令、检索索引、长期记忆**统一成一个可治理的系统。Backstage 明确把软件目录定义为“围绕代码的元数据 YAML 与所有权系统”，并强调门户层更适合作为发现与缓存层，而 Git 中的元数据与源码才应是主要事实源；GitHub、Claude Code、Windsurf、Cursor 等则都已经把仓库级、路径级、Agent 级指令与技能/子代理/MCP 工具接入变成第一等能力，说明“AI 可消费的知识面”必须被显式设计，而不能继续依赖零散 wiki 或口口相传。citeturn26view5turn30view0turn26view0turn10view2turn10view3turn28view1turn10view6turn10view7turn35search0turn35search1turn35search3

对中大型研发团队，我最推荐的不是“单一文档平台”，而是一个**四层一环**模型：**源码事实层**负责 API/Schema/配置/代码注释等高权威信息；**治理文档层**负责 ADR、标准、Runbook、SOP、规格与 Agent 指令；**上下文索引层**负责向量索引、Repo Map、依赖图、知识图与 Context Pack 编译；**运行时上下文层**负责日志、监控、事故、发布与工单；最外侧再加一个**评估与演进闭环**，持续检测 freshness、drift、groundedness、复用率与 token 成本。这个方向与 OpenAI 的 retrieval、compaction、evals，Anthropic 的 memory / subagents / MCP，Sourcegraph 的多路检索与重排，Azure AI Search 的 agentic retrieval，Microsoft Foundry 的 groundedness / relevance / tracing 形成了相当一致的工程趋势。citeturn17view0turn17view2turn17view3turn17view4turn28view1turn28view2turn28view3turn20view0turn17view5turn17view6

如果只用一句话总结这套方案：**把“人看的文档体系”升级为“代码同源、带元数据、可验证、可检索、可压缩、可记忆、可评估”的 Knowledge OS。** 这样它既能支持人协作，也能支持 Agent Workflow，不会因为模型变强就失效，反而会随着 AI 与组织规模增长而越发重要。citeturn26view5turn19search0turn15view7turn15view8turn15view9turn33view0

下面这张“文字版架构图”是全文的总图：

```text
[源码事实层]
  src/ | openapi/ | proto/ | schemas/ | infra/ | tests/
        ↓ 代码提交 / PR / 发布

[治理文档层]
  docs/ | adr/ | runbooks/ | specs/ | standards/ | prompts/ | agents/
        ↓ lint / review / ownership / verify / build

[上下文索引层]
  catalog metadata -> repo map -> dependency graph -> embeddings -> vector store
                   -> context summaries -> context packs -> memory index
        ↓ 检索 / rerank / compress / route

[运行时上下文层]
  incidents | dashboards | deploys | alerts | tickets | changelog | release notes
        ↓ MCP / API / webhook 接入

[AI 交互流]
  user task -> intent classify -> context routing -> context pack build
            -> model / agent / subagent execution -> citations / trace / eval
            -> doc suggestion / ADR draft / runbook patch / memory write-back

[治理闭环]
  freshness -> drift -> conflict -> dead-doc -> retrieval quality -> cost -> adoption
```

## 渐进式治理成熟度模型

文档治理最常见的失败方式，不是“团队不重视文档”，而是**一开始就把治理设计得像大型企业合规体系**：要求全量模板、强制详尽设计文档、每类文档都要审批、但又没有自动生成、没有嵌入开发流程、没有 owner，也没有 AI 能稳定消费的结构。Google 的技术写作指南强调大文档要有清晰大纲、层次结构和渐进披露；Google 代码评审文档把“相关文档是否同步更新”列为 Review 项；Notion 的 verified pages 与 owners 机制、GitHub 的 CODEOWNERS 与 branch protection、Backstage 的 docs-as-code 和 CI/CD 发布模式，都说明有效治理不靠“多写”，而靠**结构、责任、触发器、自动化与可验证性**。citeturn31view0turn31view1turn18view1turn26view3turn26view4turn30view2

下表是我建议的成熟度模型。它不是按“文档多少”分级，而是按**AI 是否能稳定拿到正确上下文**、**文档是否能随代码同步演化**来分级。

| Level   | 典型规模                   | 目标状态            | 最少能力                                                    | 核心产物                                                | 主要风险                  | 升级条件                         |
| ------- | ---------------------- | --------------- | ------------------------------------------------------- | --------------------------------------------------- | --------------------- | ---------------------------- |
| Level 0 | 个人项目                   | 让 AI 不迷路        | README、AGENTS/CLAUDE/Windsurf Rules、最少目录说明              | `README.md`、`AGENTS.md`、`/docs/overview.md`         | 知识只在聊天里，AI 会反复问同样问题   | 出现第二位协作者，或代码超过单一模块           |
| Level 1 | 小团队                    | 让决策可追踪          | 基础规范、API 合同、ADR、CODEOWNERS                              | `adr/`、`apis/`、`standards/`                         | 决策散落在 PR/聊天，接口演化失控    | PR 数量上升，出现多服务/多模块            |
| Level 2 | 中型团队                   | 让文档与代码同频        | 分层知识结构、Doc Review、freshness SLA、Runbook                 | `runbooks/`、`specs/`、`context/`、`catalog-info.yaml` | 文档开始陈旧，AI 检索噪声上升      | repo 变大、入职成本升高、事故增多          |
| Level 3 | 多团队组织                  | 让知识可发现、可索引      | 文档门户、软件目录、自动索引、RAG、验证机制                                 | Backstage/TechDocs、向量索引、owner/verify 面板             | 多版本冲突、孤儿服务、错误上下文被反复引用 | 多仓/Monorepo 并存，Agent 开始跨系统工作 |
| Level 4 | AI Native 团队           | 让 Agent 可执行复杂工作 | Context Pipeline、MCP、subagents、memory、evals             | Context Pack、Repo Map、Memory Index、Agent Skills     | 长上下文成本暴涨、提示漂移、自治错误扩散  | 出现持续运行 Agent、自动化 PR、自动运维     |
| Level 5 | AI Native Organization | 让知识系统自演化        | 组织级 Knowledge OS、agent memory sharing、governed autonomy | 组织知识图、经验蒸馏、跨 Agent handoff 协议                       | 局部最优、历史偏见固化、错误知识全组织传播 | 需要组织级经验复用、跨域协同、长期治理          |

这套分级不是凭空抽象出来的。它综合了 Backstage 的“YAML 元数据随代码存储、门户做发现”、GitHub 的 repo/path/agent instruction 分层、Claude/Windsurf 的本地自动记忆与持久规则分离、Cursor 的 Rules/Skills/Subagents/MCP、OpenAI 与 Anthropic 的上下文压缩与缓存、以及 Sourcegraph / Azure AI Search 的多路检索与路由式检索方向。citeturn26view5turn26view0turn28view3turn10view8turn34search0turn35search0turn35search1turn35search3turn11search2turn17view2turn28view0turn20view0turn17view5

**最小可行治理模型**我建议只包含六样东西，而且必须全部放进 repo、全部带 owner：

| 产物 | 是否必需 | 目的 |
|---|---|---|
| `README.md` | 必需 | 项目入口、快速运行、模块导航 |
| `AGENTS.md` | 必需 | 给 AI 的全局协作约束、术语、禁区、编码偏好 |
| `adr/` | 必需 | 记录关键架构决策，不让历史原因蒸发 |
| `apis/` | 必需 | 明确接口契约，优先让契约成为自动生成源 |
| `runbooks/` | 必需 | 部署/回滚/事故处理 SOP |
| `docs/index.md` | 必需 | 知识目录与权威文档入口 |

之所以从这六样开始，是因为它们分别覆盖了**入口、指令、决策、契约、操作、导航**六种最容易失效的上下文。在 AI Coding 场景里，很多失败不是因为模型“不会写代码”，而是因为仓库里没有清晰的入口、路径级约束、稳定决策记录，或者自动记忆只保存在个人机器上。Claude Code 的 auto memory 明确是 machine-local，Windsurf 的 auto-generated memories 也只在本机持久；如果团队不把这些经验显式写回 repo 的规则或知识库，它们就无法被多人共享或被另一个 Agent 可靠复用。citeturn28view3turn10view8turn34search9

## 文档知识架构与仓库结构

最关键的设计动作，是把文档先按**知识性质**而不是按“写给谁看”来分层。Google Cloud 的 ADR 指南建议把 ADR 作为贴近代码的 Markdown 决策记录；Backstage 的目录与 TechDocs 建议把元数据和文档随代码共存，并通过 CI/CD 生成只读展示层；GitHub、Cursor、Claude、Windsurf 又分别证明了面向 AI 的 instructions / rules / skills / subagents / workflows 必须成为独立资产，而不是藏在 README 的某一段里。citeturn10view14turn26view5turn30view2turn26view0turn35search0turn35search3turn28view1turn10view7

我推荐的知识分层如下：

| 层 | 知识类型 | 生命周期 | 推荐格式 | 更新方式 | 权威性 |
|---|---|---|---|---|---|
| 稳定知识层 | 术语、架构原则、系统边界、编码标准、服务责任 | 长期 | Markdown + Frontmatter | 人工主导，AI 辅助起草 | 高 |
| 变更知识层 | 产品规格、设计方案、API 合同、迁移计划、发布说明 | 中短期 | Markdown / OpenAPI / Proto / YAML | 代码变更触发同步 | 很高 |
| AI Context 层 | AGENTS、Rules、Prompt 资产、Skills、Context 摘要、Pack 模板 | 高频 | Markdown + YAML frontmatter | AI 维护建议 + 人工审核 | 中高 |
| Runtime Context 层 | Incident、Postmortem、Dashboard、告警、工单、部署状态 | 高频 | Markdown + 结构化 JSON/YAML 引用 | 事件驱动 + 自动抓取 | 很高 |
| Memory 层 | 复盘经验、常见坑、模块语义、检索摘要、历史迁移经验 | 中期 | Markdown 索引 + topic files | AI 汇总，人工确认发布 | 中 |
| Archive 层 | 过期设计、废弃 SOP、历史规格、旧版本 API | 长期 | 冻结 Markdown / JSON | 自动归档 | 低到中 |

在具体职责边界上，可以这样划分：**长期稳定知识**包括术语、系统边界、代码规范、架构原则；**高频变更知识**包括 Specs、Roadmap 片段、发布计划、兼容性说明；**AI Context**包括 AGENTS、Rules、Prompt 资产、Workflow 模板、Skills 清单、Context Pack 模板；**Runtime Context**包括部署状态、incident timeline、告警与 SLO；**Decision Record**就是 ADR；**Memory**是经过沉淀后可复用的经验摘要，但不直接等同于事实源；**Agent Instructions**属于约束层，不属于业务知识层。MCP 规范把与模型协作的接口清晰划分成 resources、prompts、tools 三类，因此你的治理结构也应该对应区分“给模型读什么”“给模型套什么模板”“允许模型做什么”。citeturn15view7turn15view8turn15view9

我最推荐的 repo 目录结构如下：

```text
/
├─ README.md
├─ AGENTS.md
├─ .github/
│  ├─ copilot-instructions.md
│  ├─ instructions/
│  │  ├─ backend.instructions.md
│  │  ├─ frontend.instructions.md
│  │  └─ infra.instructions.md
│  └─ workflows/
├─ docs/
│  ├─ index.md
│  ├─ architecture/
│  ├─ domains/
│  ├─ onboarding/
│  └─ glossary/
├─ adr/
│  ├─ adr-20260526-auth-boundary.md
│  └─ adr-20260526-vector-index-strategy.md
├─ specs/
│  ├─ product/
│  ├─ design/
│  └─ migrations/
├─ apis/
│  ├─ openapi/
│  ├─ proto/
│  └─ schemas/
├─ standards/
│  ├─ coding/
│  ├─ testing/
│  ├─ security/
│  └─ docs/
├─ runbooks/
│  ├─ deploy/
│  ├─ rollback/
│  ├─ incident/
│  └─ ops/
├─ prompts/
│  ├─ review/
│  ├─ refactor/
│  ├─ test/
│  └─ incident/
├─ agents/
│  ├─ planner.agent.md
│  ├─ reviewer.agent.md
│  ├─ release.agent.md
│  └─ skills/
├─ context/
│  ├─ repo-map/
│  ├─ domain-summaries/
│  ├─ packs/
│  └─ generated/
├─ memory/
│  ├─ MEMORY.md
│  ├─ api-conventions.md
│  └─ migration-lessons.md
├─ incident/
│  ├─ postmortems/
│  └─ timelines/
├─ catalog-info.yaml
└─ CHANGELOG.md
```

这个结构吸收了几类已被验证的平台实践：GitHub 的 repository-wide / path-specific / agent instructions；Windsurf 的 AGENTS.md、Rules 与 Markdown workflow；Claude Code 的 `MEMORY.md`+topic files 模式；Backstage 的 `catalog-info.yaml` 与 docs-like-code；Google Cloud 的 ADR 就近存放；Stripe 的显式版本/变更日志；以及 Uber 在 incident handling 中把 runbook、annotations、ownership、shift report 收敛到统一 on-call 体验里的经验。citeturn26view0turn10view7turn10view6turn28view3turn26view5turn10view14turn24view0turn23view0

在格式选择上，我建议遵循这个原则：

| 资产 | 推荐格式 | 原因 | 是否自动生成 | 是否可由 AI 维护 | 是否必须人工审核 |
|---|---|---|---|---|---|
| README / ADR / Runbook / Spec / SOP | Markdown | 适合叙事、比较、上下文、链接 | 否 | 可初稿 | 是 |
| Catalog / Metadata / Index config / Rule frontmatter | YAML | 易被人编辑，也便于索引和过滤 | 部分 | 可 | 是 |
| API Contract / Schema / Protocol | OpenAPI / Proto / JSON Schema | 应作为机器可执行事实源 | 是 | 可补全 | 是 |
| Repo Map / Dependency Graph / Coverage 报表 | JSON / graph data | 更适合程序消费和检索 | 是 | 是 | 否 |
| Prompt 资产 / Agent profile / Skill manifest | Markdown + YAML frontmatter | 兼顾可读性与可路由性 | 部分 | 是 | 是 |
| Release Note / Postmortem 初稿 / Changelog 草稿 | Markdown | 便于人最后校订 | 是 | 是 | 是 |
| SDK / API 文档 / 类型说明 | 代码即文档 + 自动生成 | 避免手写漂移 | 是 | 否 | 是 |

其中最重要的边界是：**凡是“能从代码、合约、配置中稳定导出”的，都不应手写为主；凡是“涉及取舍、判断、责任与因果解释”的，都必须保留人工审核。** Uber 的 API gateway 实践就非常典型：API 行为由受校验的配置驱动，并自动生成客户端 SDK；Google 的 ADR、Google SRE 的 postmortem、Notion 的 verified pages / owners，又都在告诉我们，解释性与责任性文档不能完全自动化。citeturn23view1turn10view14turn31view2turn18view1

我推荐的命名规范、标签体系与 metadata schema 如下。这里的重点不是“花哨”，而是让检索、路由、freshness、权威判断有稳定信号：

```yaml
---
id: adr-auth-boundary-20260526
title: 统一认证边界与会话验证策略
doc_type: adr
domain: identity
system_id: account-platform
service: auth-gateway
owner: team-identity
reviewers: [arch-council, sre-auth]
status: active
authority: authoritative
source_of_truth: repo
freshness_tier: quarterly
review_by: 2026-08-31
version: 1.2.0
related_code:
  - services/auth-gateway/**
  - apis/openapi/auth.yaml
related_docs:
  - docs/architecture/identity-boundary.md
  - runbooks/incident/auth-token-rotation.md
api_versions:
  - 2026-04-22
tags:
  - security
  - auth
  - boundary
  - decision
confidentiality: internal
ai_summary: >
  定义认证边界、令牌验证位置和网关职责。若涉及 session、JWT、
  introspection 或 token rotation，优先检索本记录。
---
```

标签建议控制在五类以内：**domain、artifact-type、lifecycle-state、risk-level、audience**。不要把标签变成第二套目录树。OpenAI retrieval 已支持 attribute filtering；Backstage catalog 以 YAML 元数据做组织发现；Notion verified pages 也允许按 owner 与 teamspace 过滤，所以标签与 metadata 的目标不是“好看”，而是让后续的**权威路由与分层检索**可执行。citeturn15view0turn16view4turn26view5turn18view1

## 治理机制与自动化控制面

成熟的文档治理，不是靠“文档管理员”手工巡检，而是靠**ownership + lifecycle + automation + eval**共同工作。GitHub 的 CODEOWNERS 与 branch protection 已经提供了最直接的责任与合并门禁；Google 代码评审原则明确要求相关文档随代码更新；Notion 的 verified pages 用 owner 和过期提醒解决“没人知道这页是否还可信”的问题；Backstage 推荐在 CI/CD 中生成 TechDocs 并把展示层切换成只读；SRE postmortem 则通过统一模板与趋势分析，让 incident 文档从“写完即忘”变成系统改进数据。citeturn26view3turn26view4turn31view1turn18view1turn30view2turn31view2turn31view3

我建议的角色模型如下：

| 角色 | 职责 | 是否可由 AI 代替 |
|---|---|---|
| Document Owner | 对单篇权威文档负责，确保正确、完整、按 SLA 刷新 | 否 |
| Domain Owner | 对某个领域知识树负责，裁决冲突与目录演进 | 否 |
| AI Maintainer | 负责索引、摘要、拼接 Context Pack、提出更新建议 | 可部分代替 |
| Reviewer | 审核事实、风险、边界、影响范围 | 否 |
| Runtime Steward | 对 incident / ops / runbook 的运行态信息负责 | 否 |
| Knowledge Platform Owner | 负责工具链、索引、门户、评估指标与自动化平台 | 否 |

建议把 ownership 同时写进三处：**文档 frontmatter、CODEOWNERS、软件目录元数据**。这样仓库内、PR 审核流、组织发现层是统一的。Backstage 的 catalog 与 owner 体系就是为“服务发现与责任发现”而设计的；GitHub 则能把责任直接转成 Review gate。citeturn26view5turn26view3

文档生命周期建议走下面这条线：

```text
Create
  -> Classify (doc_type / domain / authority / freshness_tier)
  -> Assign Owner
  -> Draft by human or AI
  -> Review gate (CODEOWNERS + doc lint + schema lint + link check)
  -> Merge
  -> Index / embed / repo-map refresh / graph refresh
  -> Publish to portal / verify page / expose to agents
  -> Periodic freshness check
  -> Drift/conflict/dead-doc detection
  -> Archive
  -> Delete only after replacement or retention policy passes
```

在**版本治理**上，建议把文档分成三类来处理。第一类是**与代码强同步**的，例如 OpenAPI、Proto、Schema、SDK 文档、Config Reference，这类必须与代码同版本，尽量自动生成；第二类是**与决策同步**的，例如 ADR、设计方案、迁移计划，这类通过 ADR status 与 superseded 链接保持演化；第三类是**与发布/运行同步**的，例如 changelog、release note、postmortem、runbook，这类应该由事件自动触发并允许 AI 起草。Stripe 的 API versioning 和 changelog 说明了“版本本身就是产品契约”；Google Cloud ADR 建议把 ADR 存在与决策相关的代码附近；Google SRE 的 postmortem 模板又说明 incident 记录必须支持长期趋势分析。citeturn24view0turn10view14turn31view3

自动化控制面我建议至少包含下面五条流水线：

| 流水线 | 触发器 | 自动动作 | 人工关口 |
|---|---|---|---|
| Docs Gate | PR / push | lint、frontmatter 校验、broken link、owner 缺失检测、docs-needed 检测 | CODEOWNERS 审核 |
| Contract Sync | API/schema/config 变更 | 生成 API docs、SDK docs、兼容性 diff、changelog 草稿 | 合约 owner 审核 |
| Context Index | merge / nightly | chunk、embed、repo map、graph refresh、summary refresh | 平台 owner 抽检 |
| Drift Detector | nightly / release | 代码-文档差异、Prompt 漂移、重复知识、冲突知识、死文档候选 | domain owner 裁决 |
| Eval Loop | nightly / model change / prompt change | retrieval eval、groundedness、tool-call accuracy、token cost、latency | AI maintainer + domain owner |

这套自动化与现成平台能力高度契合：GitHub Actions 可以把 workflow 深入仓库内 CI/CD；OpenAI retrieval 支持 metadata/attribute filtering、batch ingest、向量过期策略与 compaction；OpenAI evals 和 agent evals 能用 traces、graders、datasets 评估工作流；Microsoft Foundry 提供 groundedness、relevance、tool-call accuracy、task completion 与 tracing/monitoring；Devin 已经把知识去重、冲突解决、从代码模式生成知识、知识建议待审这几件事做成内建能力。citeturn27search1turn27search2turn17view1turn16view3turn17view3turn17view4turn17view6turn32view0

质量指标建议至少监控以下几组：**覆盖率**（有 owner/README/ADR/runbook/API 合同的服务占比）、**新鲜度**（review_by 未过期比率）、**一致性**（同一主题重复与冲突文档数）、**漂移**（代码与 docs diff 命中率）、**检索质量**（top-k 命中、groundedness、citation coverage）、**上下文效率**（平均 pack tokens、缓存命中率、compaction 触发率）、**治理效率**（AI 建议采纳率、docs PR lead time、死文档关闭时长）。这些指标并非全部来自单一产品，但其可观测面与 OpenAI、Microsoft Foundry、GitHub Actions、Backstage、Notion 的能力边界是对齐的。citeturn17view3turn17view4turn17view6turn27search1turn30view2turn18view1

## 大规模 AI Context Architecture

文档规模一大，AI 找不到正确上下文，通常不是因为“向量库不够强”，而是因为同时出现了五个问题：**上下文窗口有限**、**噪声文档过多**、**旧知识未退场**、**权威性信号缺失**、**项目结构没有被显式建模**。Anthropic 明确指出 context window 本质上是工作记忆，随着 token 增长会出现 recall/accuracy 下降的 context rot；Sourcegraph 也强调 context fetching 一定要受 latency SLA 与 token budget 约束，检索层追求 recall，排序层再在 token 预算内做 precision；OpenAI 的 compaction 与 prompt caching、Anthropic 的 prompt caching 与 subagents，本质上都是在解决“长任务上下文保不住、又不能无限堆文本”的问题。citeturn29search0turn20view0turn17view2turn10view11turn28view0turn28view1

因此，大规模 AI Context Architecture 的核心不是“把所有文档都喂给模型”，而是做一个**先路由、后检索、再重排、再压缩、最后组包**的 Context Pipeline。现代实现上，OpenAI file search 会重写查询、把复杂问题拆成并行搜索、同时做 keyword + semantic search 并 rerank；Azure AI Search 的 agentic retrieval 会根据会话历史拆子查询、并行执行、输出带 citations 与 execution metadata 的结构化结果；Sourcegraph 则把 keyword、embedding、graph、local context 几类检索并联，再在 token 预算内做集合选择式排名。citeturn17view0turn17view5turn20view0

我建议的检索架构对比如下：

| 架构 | 机制 | 优点 | 缺点 | 适用场景 | Token 成本 | 延迟 | 工程复杂度 |
|---|---|---|---|---|---|---|---|
| 全量检索 | 全库 semantic / keyword 搜索 | 实现简单 | 噪声高、召回失真 | 小库、Level 0-1 | 中 | 低 | 低 |
| 分层检索 | 先层路由，再层内检索 | 控噪声、可做优先级 | 需要稳定 metadata | 中大型团队 | 低到中 | 低到中 | 中 |
| 路由检索 | 先判定任务类型/领域，再选检索器 | 精度高 | 路由错会漏召回 | 多领域系统 | 低 | 低 | 中 |
| Domain-based Retrieval | 以服务/领域/团队为边界检索 | 符合组织现实 | 跨域问题需要二次扩展 | 微服务、多团队 | 中低 | 低 | 中 |
| Graph Retrieval | 从依赖图/调用图/知识图扩邻域 | 适合理解结构和影响面 | 图构建昂贵 | 大型代码库、重构、根因分析 | 中 | 中 | 高 |
| Hybrid Retrieval | keyword + semantic + graph + local | 综合效果最好 | 实现最复杂 | Level 3+ 默认方案 | 中 | 中 | 高 |
| Intent-aware Retrieval | 按“写代码/查历史/排障/发布”切换策略 | 贴任务 | 需要高质量 intent classifier | Agent Workflow | 低到中 | 低 | 中高 |
| Agentic Retrieval | LLM 规划子查询、并行检索、结构化返回 | 复杂问题效果最好 | 成本和时延较高 | 跨系统、研究型、诊断型任务 | 中到高 | 中高 | 高 |

这张表是综合 OpenAI、Azure AI Search、Sourcegraph 以及 Anthropic subagents/MCP 能力之后的工程化建议：**Level 0-2 不要过度设计，Level 3 开始上 layered + hybrid，Level 4 以后再把 graph 与 agentic retrieval 纳入默认路径。**citeturn17view0turn17view5turn20view0turn28view1turn17view5

我最推荐的 **AI Context 分层模型** 如下：

| Layer | 内容 | 生命周期 | 获取方式 | 压缩方式 | Token 优先级 |
|---|---|---|---|---|---|
| L0 当前任务上下文 | 当前问题、diff、branch、活跃文件、报错、目标输出 | 临时 | IDE / CLI / PR / trace | 不压缩或极轻压缩 | 最高 |
| L1 Workspace Context | Repo root rules、AGENTS、路径规则、当前服务 README、Repo Map、局部符号图 | 高频 | 本地 + catalog | 摘要 + 去重 | 很高 |
| L2 Domain Knowledge | ADR、Design、API Contract、Coding Standard、Spec | 中期 | metadata filter + hybrid retrieval | 结构化摘要 | 高 |
| L3 Runtime Context | 监控、发布、incident、工单、最近变更 | 高频 | MCP / API / webhook | 时间窗口摘要 | 中高 |
| L4 Durable Memory | 经验教训、常见坑、迁移经验、经验证的摘要 | 中期到长期 | memory index | topic summary | 中 |
| L5 Archive / History | 旧版本文档、过期设计、历史 postmortem | 长期 | on-demand 检索 | 强压缩 + recency 标记 | 低 |

这套模型吸收了几类现实产品做法：Sourcegraph 默认会把打开文件和当前仓库作为上下文起点，并允许 `@` 更多文件/符号/仓库；Claude Code 的 `MEMORY.md` 只加载前 200 行或 25KB，topic files 按需读；Windsurf 的根目录 `AGENTS.md` 会全局注入，而子目录 `AGENTS.md` 自动按路径生效；GitHub Copilot 与 Cursor 也都支持仓库级、路径级和 Agent 级的分层指令。也就是说，**好的 Context Layer 不是概念分层，而是与真实工具的生效边界一一对应。**citeturn10view15turn28view3turn7search17turn26view0turn35search0

Context Pack 的构建，我推荐采用下面这个流水线：

```text
Intent classify
  -> choose route (code / design / api / incident / release / refactor)
  -> collect L0 local context
  -> apply authority filters (owner, status, freshness, domain)
  -> run hybrid retrieval
  -> expand graph neighbors if task needs impact analysis
  -> rerank
  -> deduplicate by canonical doc id + semantic cluster
  -> conflict check (prefer authoritative + fresher + in-scope doc)
  -> compress into layered pack
  -> cache prefix / save pack id / attach citations
```

Context Pack 的排序分数建议至少包含六个因子：**relevance、authority、freshness、locality、dependency proximity、cost**。一个实用的工程公式可以是：

```text
final_score =
  0.35 * relevance
+ 0.20 * authority
+ 0.15 * freshness
+ 0.10 * locality
+ 0.10 * dependency_proximity
+ 0.10 * cost_efficiency
```

其中 authority 依赖 owner / verified / source_of_truth / status；freshness 依赖 review_by / last_changed / release proximity；locality 取决于当前 diff / file / service；cost_efficiency 用来限制长文档无脑挤占上下文。这个公式不是来源于某个单一厂商，而是把 OpenAI 的 metadata filtering、Sourcegraph 的 retrieval-then-ranking、Azure 的 semantic ranking、Notion 的 verified pages、Backstage 的 owner metadata 合并成了一个统一的工程评分框架。citeturn16view4turn20view0turn17view5turn18view1turn26view5

在**减少 token 浪费**上，最有效的不是“压缩得更狠”，而是“不要把本该缓存、分治、局部加载、按需读取的东西一次性塞进来”。OpenAI prompt caching 要求静态前缀放前面、动态部分放后面，并说明缓存命中需要 exact prefix；Anthropic 同样把 system/tools/messages 的固定前缀缓存起来，并支持自动前移的 cache breakpoint；OpenAI compaction 和 Anthropic 的 context compaction/automatic caching 则更适合长会话；Claude subagents 还能把侧任务放到独立窗口里，仅回传摘要。基于这些机制，建议把 Context Pack 拆成**固定前缀包**、**任务局部包**、**运行态增量包**三段。citeturn10view11turn28view0turn17view2turn29search20turn28view1

当文档库和代码库都很大时，AI 要真正“理解系统结构”，必须把 repo 变成可导航图，而不能只是一堆 chunk。Backstage catalog graph 已经明确支持 ownership、grouping、API relationship 等实体关系；Sourcegraph 说明其上下文理解会同时用 keyword、embedding、graph 和 local context；Uber 的 on-call dashboard 说明 runbook、annotations、ownership、alerts 这些运行态关系也必须被建模；Uber 的 API gateway 又说明契约、验证、观测与生成 SDK 这些结构最好由配置统一驱动。对 AI Coding 来说，这意味着你至少需要四张图：**Repo Map、Dependency Graph、Architecture Graph、Decision Graph**；大型组织再加一张 **Runtime Relationship Graph**。citeturn30view1turn20view0turn23view0turn23view1

## 项目类型适配与 AI 驱动演进

治理不应该“一把尺子量到底”。不同项目类型，文档重点完全不同。Google 技术写作指南强调短文档适合新手与 how-to，长文档适合深入教程和 reference；Backstage 适合以服务、组件、API、团队为中心组织知识；Uber monorepo 的远程开发痛点表明大仓场景需要更强的环境/构建/上下文治理；Stripe 风格的 API 版本治理则适合外部接口导向系统。citeturn31view0turn26view5turn23view2turn24view0

下面是我对不同项目类型的适配建议：

| 项目类型 | 文档重点 | 必须先做 | 可延后 | 适合自动化 | 必须严格 Review |
|---|---|---|---|---|---|
| MVP / 快速验证 | 目标、范围、技术假设、快速运行 | README、AGENTS、关键假设 ADR | 完整知识图、门户 | README/脚手架/测试说明生成 | 产品边界、数据风险 |
| SaaS 系统 | API、租户边界、发布、运维 | API Contract、Runbook、ADR、Incident 模板 | 全量历史归档 | Release note、SDK docs、变更摘要 | 安全、计费、兼容性 |
| Monorepo | Repo Map、模块边界、构建与依赖 | Repo Map、workspace rules、构建手册 | 全量领域知识库 | 依赖图、影响分析、context pack | cross-cutting architecture |
| 微服务 | 服务边界、所有权、调用关系 | catalog metadata、API/事件契约、runbook | 深度设计文档 | 服务目录、接口 diff、owner 路由 | 契约变更、SLO/SLA |
| Infra / Platform | SOP、标准、迁移、安全 | ADR、标准、回滚手册、变更策略 | 产品化文档 | 变更单摘要、兼容性检查 | 安全、稳定性、权限 |
| AI Agent 系统 | tools/prompts/memory/evals | Agent profile、prompt assets、eval sets、tool policy | 组织级记忆共享 | prompt drift 检测、trace summarization | autonomy boundary、tool access |
| 数据平台 | schema、血缘、质量规则 | schema contract、pipeline runbook、数据字典 | 全量业务文档 | 血缘图、指标定义同步 | 口径、权限、保留策略 |
| SDK / Library | API reference、版本、升级指南 | changelog、semver、examples、compat notes | 复杂内部设计文档 | API docs、example tests | 破坏性变更 |
| Open Source | onboarding、贡献规范、公开 API | README、CONTRIBUTING、versioning、examples | 内部流程文档 | site build、doc preview、lint | 公共接口、社区规则 |
| 企业级系统 | 合规、审批、变更、审计 | owner、review、freshness、incident、archive policy | 个性化 agent memory | 审计报表、到期提醒、知识去重 | 合规、安全、关键决策 |

避免“治理过度”的关键，是把每个项目的治理动作都挂到**真实失败模式**上。例如 AI Agent 系统里，最容易失效的是 prompt 漂移、tool 权限失控、memory 污染，所以优先做 prompt 资产、tool policy、eval 和 trace；而 SDK / Library 更容易踩的是破坏性变更和版本混乱，所以优先做 changelog、compatibility notes、example tests。不要要求 MVP 一开始就建设完整 portal、知识图和多级 archive。citeturn17view3turn17view4turn17view6turn24view0

AI 也应该帮助治理体系自身演进，但自治级别必须分层。我建议这样切分：

| 自治级别 | 适合事项 |
|---|---|
| Fully Autonomous | 索引、摘要、chunk/embed、死文档候选、重复知识候选、pack 编译、缓存、到期提醒、变更摘要 |
| Human-in-the-loop | ADR 起草、Prompt 优化建议、目录结构升级建议、知识热点分析、runbook 更新建议、冲突知识合并建议 |
| 必须人工治理 | 架构决策定稿、删除权威文档、Incident 根因定责与结论、合规/安全策略、产品范围与承诺 |

Devin 的知识管理能力已经证明，AI 很适合做**重复知识发现、冲突指导、从代码模式生成知识、待审建议队列**；OpenAI 与 Microsoft Foundry 的 eval / trace 能让你对 AI 的治理行为本身做回归与质量控制；GitHub / Anthropic / MCP 则提醒你对 tools 的授权必须默认最小化、Prefer read-only allowlist。也就是说，AI 很适合做“候选生成器”和“守门自动化”，不适合做“最终裁决者”。citeturn32view0turn17view3turn17view6turn36view0turn28view2

## 最推荐的 Best Practice 架构

如果让我为一个**中大型、AI Native、面向未来 Agentic Workflow**的组织只推荐一套方案，我会推荐下面这套 **Repo-centric Knowledge OS**：

```text
Git repos as source of truth
  + contracts as executable truth
  + docs as code
  + metadata YAML on every service / library / API
  + AGENTS / rules / prompts / workflows as first-class assets

Backstage / internal portal as discovery layer
  + owner lookup
  + graph navigation
  + verified pages
  + read-only TechDocs publishing

Context compiler as AI-facing layer
  + intent router
  + layered hybrid retrieval
  + repo map + dependency / architecture / decision graph
  + authority / freshness / trust scoring
  + context pack build
  + prompt caching + compaction + subagents

Runtime + tool layer
  + MCP to issues / incidents / observability / deploy / wiki / tickets
  + read-only by default, write tools allowlisted

Governance loop
  + CODEOWNERS + branch protection
  + docs gates in CI/CD
  + drift / dead-doc / duplicate / conflict detection
  + evals + traces + groundedness + cost dashboards

Memory layer
  + durable human-approved memory
  + machine-local scratchpads remain scratchpads
  + organization memory only through reviewed write-back
```

这套方案有几个明确的 trade-off。它**优先正确性和可扩展性**，而不是最小初始投入；它把 portal 定位为发现层，而不是事实源，因此需要团队接受“知识必须回 repo”的纪律；它假设你愿意维护 metadata 与 owner，但回报是后续的 AI routing、RAG quality、incident learning、组织发现和授权都能复用这些同一套信号。Backstage、Notion、GitHub、Stripe、Google、Sourcegraph、Anthropic、OpenAI 在各自产品中，实际上已经把这些原则分别跑通了不同部分。citeturn26view5turn18view1turn26view3turn24view0turn31view2turn20view0turn28view3turn17view0turn17view3

下面这张表，是我对业内实践最有借鉴价值部分的压缩总结：

| 来源 | 直接可借鉴的做法 | 你应该落地成什么 |
|---|---|---|
| OpenAI | retrieval 支持 metadata filter、batch ingest、expiration；file search 做 query rewrite、并行搜索、hybrid search、rerank；prompt caching、compaction、agent evals 已产品化。citeturn17view0turn17view1turn17view2turn17view3turn17view4 | 建 Context Compiler、metadata schema、eval loop |
| Anthropic | Claude Code 的 `MEMORY.md`、topic files、subagents、MCP、prompt caching 都在强调“上下文要分层、可缓存、可独立工作”。citeturn28view0turn28view1turn28view2turn28view3 | 把 durable memory 与 scratch memory 分开；对侧任务使用 subagent |
| GitHub | repo/path/agent instructions、custom agents、agent skills、MCP、CODEOWNERS、branch protection 已形成完整治理面。citeturn26view0turn10view2turn10view3turn26view2turn26view3turn26view4 | 把 AI 指令纳入 repo，并像代码一样 gate |
| Cursor | Rules、Skills、Subagents、MCP、CLI + GitHub Actions 说明 AI Coding 已从 IDE 扩展到 headless automation。citeturn35search0turn35search1turn35search3turn11search2turn35search12 | 不只在 IDE 中治理，也要治理 CI 中的 Agent |
| Windsurf | AGENTS.md 的目录作用域、Rules/Workflows/Knowledge Base、Memories 的共享边界非常清楚。citeturn7search17turn10view7turn34search7turn34search9 | 用目录级 Agent 指令替代一份庞大全局指令 |
| Sourcegraph | 默认 workspace context、`@` context、keyword+embedding+graph+local 的多路检索、严格 token 预算。citeturn10view15turn20view0turn20view2 | 构建 layered hybrid retrieval，而不是单一向量检索 |
| Backstage | catalog metadata + owner + TechDocs + graph + GitOps。citeturn26view5turn30view0turn30view1turn30view2 | 把文档治理接到软件目录和开发门户 |
| Notion | verified pages + page owners + 到期提醒。citeturn18view1 | 给“权威知识”做验证状态与 refresh SLA |
| Google Engineering / SRE | code review 要求文档同步；ADR 贴近代码；postmortem 模板化并做趋势分析；blameless culture。citeturn31view1turn10view14turn31view2turn31view3 | 把 docs update、ADR、postmortem 纳入硬流程 |
| Stripe / Airbnb / Uber / Microsoft | Stripe 式版本/变更管理；Airbnb 式可执行注释约定；Uber 式 runbook/ownership/on-call context 与 contract-driven API；Microsoft 式 groundedness / relevance / tracing / monitoring。citeturn24view0turn25view2turn23view0turn23view1turn17view6 | 建立版本契约、运行态知识、AI 质量可观测性 |

把这些实践合在一起后，我给出的**最推荐 Best Practice**可以概括成八条执行原则：

1. **代码与契约是事实源，门户不是事实源。**  
2. **每个服务/库/API/运行手册都必须有 owner、authority、freshness。**  
3. **AI 指令、Prompt、Workflow、Skill 必须作为 repo 资产治理。**  
4. **默认采用 layered hybrid retrieval，不做“全库一把搜”。**  
5. **任何可从合约/配置/图谱生成的文档，都尽量自动生成。**  
6. **任何涉及决策、事故结论、合规与策略的内容，都必须人工审核。**  
7. **本地记忆不等于组织记忆；组织记忆只能通过 reviewed write-back 形成。**  
8. **用 evals、traces、groundedness 与 token 指标治理 AI，而不是只看“感觉好不好用”。** citeturn17view3turn17view4turn17view6turn28view3turn10view8turn26view5

## 适用边界与开放问题

这份方案的适用边界很明确：它最适合**中大型研发团队、AI Native 团队、需要长期知识沉淀和 Agent 协作的组织**。如果是 1 人项目或生命周期很短的验证项目，直接上全套 catalog、graph、vector、portal、eval loop 会过重。此时用 README + AGENTS + ADR + API Contract + Runbook 模板启动更合适，等到协作者数量、模块数量、事故频率或 AI 自动化深度上升，再逐层升级。citeturn31view0turn26view5turn26view0

仍然存在的开放问题主要有三类。第一，**跨工具兼容性**仍在快速变化，尤其是 Cursor、GitHub、Windsurf、Claude 对 instructions / skills / agents / MCP 的支持面和行为差异；第二，**组织级长期记忆写回**还缺乏统一标准，MCP 提供了 resources/prompts/tools 的协议骨架，但“什么能升格为组织记忆”仍需要公司内部治理；第三，**大模型与检索系统的评估**虽然已有 groundedness、relevance、tool-call accuracy、trace grading 等机制，但对“工程决策质量”本身的量化仍然不成熟。citeturn15view7turn15view8turn15view9turn17view3turn17view6

**最终结论**：真正适用于 AI Coding、AI Native Engineering、Agentic Software Development、长周期知识演化的，不是传统 wiki，也不是单纯的 RAG，而是**一套可渐进演化、可规模化、以 repo 为事实源、以 metadata 和 owner 为治理骨架、以 Context Pipeline 为 AI 入口、以 eval loop 为质量闭环的文档治理体系**。这就是我最推荐的一套 AI Coding 文档治理架构。
