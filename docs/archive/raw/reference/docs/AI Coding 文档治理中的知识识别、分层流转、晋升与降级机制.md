---
status: archived
---

# AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制

## Executive Summary

AI Coding 场景中的“文档治理”已经不再只是传统意义上的知识管理，而是**对 Agent 可见上下文、可执行约束、长期记忆、历史决策和可验证证据**进行分层管理。OpenAI 将 `AGENTS.md`、memories、skills、MCP、subagents 视为互补层；Anthropic 将 `CLAUDE.md`、auto memory、rules、skills、hooks 分层管理；GitHub Copilot 则区分 repository-wide/path-specific custom instructions、`AGENTS.md`、skills、custom agents；Cursor 也公开支持 Project、Team、User Rules 与 `AGENTS.md`。这说明 AI Coding 的核心问题不是“文档写不写”，而是“**什么知识以哪种加载方式进入 Agent 的工作回路**”。citeturn21view3turn23view0turn12view3turn24search0

在这种语境下，文档治理必须同时回答四个问题：第一，哪些知识要进入**始终加载**的上下文；第二，哪些知识应只在**相关路径或相关任务**中按需加载；第三，哪些知识应成为**历史记录**而不是行为规则；第四，哪些知识必须被转换成**测试、自动检查、hooks、branch protection** 这类可执行控制，而不能只停留在文字说明里。Anthropic 明确说明 `CLAUDE.md` 属于“context，而非 enforced configuration”，如果要强制阻止动作，应使用 `PreToolUse` hooks；GitHub 也提供 CODEOWNERS、required reviews、protected branches 作为真正可执行的审批与治理手段。citeturn23view0turn33view0turn20view0turn20view1

本报告的核心判断是：**值得被长期记住的知识，必须同时满足“作用域正确、复用概率高、证据可追、稳定性足够、检索价值明显”五个条件；不满足这些条件的知识，应留在临时上下文、局部说明、计划文档或归档层，而不应污染项目规范层或 Agent 指令层。** 这一判断与 OpenAI、Anthropic、GitHub 的官方实践一致：始终加载的指令要短、小、准；详细流程应转移到 skills、path-specific rules 或独立文档；文档要持续修剪而不是无限增厚。Google 的文档最佳实践也明确指出：少量“新鲜且准确”的文档，好于大量“状态不明”的文档。citeturn21view0turn21view4turn25search11turn25search19turn19view0

基于这些证据，本文给出的最终框架是一个**四层持久化模型**：  
**临时层**保存当次任务推理与一次性排障材料；**局部长期层**保存模块级解释、模块 README、局部约定和可复现调试经验；**项目治理层**保存需求、设计、ADR/RFC、项目级规则与 runbook；**组织知识层**保存经过复盘与提炼的最佳实践、反模式和模式库。所有知识条目在进入更高层之前，都必须通过：**抽取、分类、归一化、证据验证、目标路由、人类审核、发布、周期复核、过期清理** 这一生命周期。citeturn33view3turn33view4turn29view0turn17view0turn35view0

作为落地建议，中小型 AI Coding 项目不应一开始就上全套治理。最低可行方案应只做五件事：**feature spec、模块 README、ADR、Agent 指令适配层、文档更新 PR 审查门**。一旦项目进入多人协作、生产交付或多 Agent 工作流，再逐步增加 path-specific rules、技能库、自动化 maintenance agent、漂移检测与 promotion review。这样既能降低过度工程化风险，也能保住知识质量。citeturn16view1turn17view2turn20view1turn33view2turn19view0

| 团队真正要回答的问题 | 本报告的治理答案 |
|---|---|
| 什么知识值得被记住 | 稳定、可复用、证据充分、作用域明确、未来高检索价值的知识 |
| 什么知识应该被遗忘 | 死胡同推理、一次性日志、未验证猜测、任务结束即失效的临时操作 |
| 什么知识只属于当前任务 | 暂定方案、探索过程、局部异常、临时 workaround、尚未确认的推断 |
| 什么知识应该成为项目规范 | 跨任务反复出现、能约束 Agent 行为、对 build/test/review 有稳定价值的规则 |
| 什么知识应该进入 ADR | 影响系统结构或关键质量属性、难以逆转、需要记录替代方案与后果的决策 |
| 什么知识应该变成测试 | 用户可观察行为、接口契约、回归缺陷、业务不变量、安全/性能边界 |

## 核心问题定义

传统团队知识管理主要面向**人类按需检索**；AI Coding 文档治理则额外面向**模型启动时的上下文装载**与**Agent 执行动作前的行为约束**。Codex 会在开始工作前读取 `AGENTS.md` 指令链，并按“更靠近当前目录者优先”的方式组合；Claude Code 会在每次会话开始时加载 `CLAUDE.md` 与 auto memory；GitHub Copilot 则把 repo-wide instructions、path-specific instructions 与 `AGENTS.md` 作为不同来源的 always-on/contextual guidance。这意味着知识一旦写错位置，就不仅是“人类未来找不到”，更会变成“Agent 现在就会读错、学错、执行错”。citeturn21view2turn23view0turn12view1

AI Coding 场景下的第二个新增难点是**上下文预算**。OpenAI 明确建议保持 `AGENTS.md` 简短、实用，文件过大就应拆出 task-specific markdown；Anthropic 建议每个 `CLAUDE.md` 目标控制在 200 行以内，并强调更短的文件通常有更好的遵循效果；Claude path-scoped rules、Codex skills、Claude skills 都采用按需加载或 progressive disclosure 来避免挤占上下文。研究层面，TACL 的 “Lost in the Middle” 显示：长上下文中位于中间的关键信息更容易被模型遗漏；OpenAI 也在 subagent 概念页中直接把这类问题命名为 context pollution 与 context rot。citeturn21view0turn23view0turn21view4turn25search11turn25search19turn31view0turn30search0

第三个新增难点是**指令与治理的混淆**。文字型规则并不等于强制控制。Anthropic 说明 `CLAUDE.md` 被作为用户消息注入，可能因模糊或冲突而无法稳定遵循；如果想“无论如何禁止某类编辑或命令”，应该使用 hooks。GitHub 的 protected branches 与 required reviews 则提供了真正的执行门禁，而 CODEOWNERS 让责任边界可审计。对 AI Coding 团队来说，这意味着：**凡是高风险规则，都应尽可能“文档化 + 自动化 + 审批化”，而不是只写进一份 Agent 指令文件。** citeturn23view0turn33view0turn20view0turn20view1

因此，本报告把“知识治理”定义为一个四维分配问题：  
**权威性**：提示、规则、决定、证据、可执行检查各自处在哪一级。  
**作用域**：任务、模块、feature、项目、组织，到底影响多大范围。  
**生命周期**：只活一轮对话、活到合并、活到下一版本，还是活到系统退役。  
**验证方式**：仅为推断、有人类确认、被代码印证、被测试印证，还是已经经由评审正式成立。  
只有把这四维同时标清，才能谈“晋升”“降级”与“清理”。

## AI Coding 文档层级模型

下表是本报告基于 Spec-Driven Development、ADR、RFC/KEP、BDD/Cucumber、Google code review、SRE playbook 与当前主流 AI coding tools 的作用域模型综合拟定的**AI Coding 文档层级体系**。Spec Kit 强调“先定义要构建什么，再让 AI 实现”；ADR 指南强调记录“为什么”；Rust RFC 与 Kubernetes KEP 说明“重要变更需要结构化讨论与共识”；Cucumber 说明可执行规格既是规范、也是文档、也是测试；SRE 文档强调 runbook 要面向可操作响应且持续更新。citeturn16view1turn17view0turn16view4turn28view1turn26view0turn35view0turn19view1

| 文档层级 | 主要职责 | 生命周期 | 适合承载的知识 | 不应该承载的内容 |
|---|---|---|---|---|
| 临时上下文 | 记录当前任务的推理链、探索路径与一次性发现 | 从当次任务到合并后短期归档 | 对话记录、scratchpad、临时排障笔记、待确认假设、未定方案、上下文切换提示 | 项目级规则、正式需求结论、长期 API 契约、全局最佳实践 |
| 需求层 | 定义“要做什么”和“什么算完成” | 从需求提出到 feature 生命周期结束 | 用户目标、业务规则、验收标准、边界条件、用户可观察行为 | 技术实现细节、尚未达成一致的架构取舍、一次性 debug 过程 |
| 设计层 | 说明“准备怎么做” | 从设计评审到实现完成、必要时伴随维护 | 模块划分、架构约束、接口契约、数据模型、关键数据流、性能与安全设计 | 事后合理化、无替代方案分析的最终决策、与用户行为无关的业务愿景 |
| 决策层 | 记录“为何选这个方案而不是别的方案” | 长期保留，随 supersede 形成历史链 | ADR、RFC、设计决策记录、技术选型、替代方案、权衡、后果 | 大段实现说明、详细操作步骤、未获共识的草案、和决策无关的杂项备注 |
| 实现层 | 把设计展开为可执行工作 | 从计划到发布 | 任务拆解、实现计划、测试计划、迁移计划、回滚计划、阶段性 checklist | 项目通用规范、抽象愿景、应长期保留的历史决策 |
| 代码层 | 作为“最靠近实现”的局部真相 | 与代码同生命周期 | 类型定义、代码注释、测试用例、契约测试、模块 README、局部架构说明 | 全局流程规范、长期组织知识、与代码脱节的宏观决策记录 |
| 项目规范层 | 给人和 Agent 提供复用性高、作用域清晰的规则 | 长期保留，但必须周期复核 | `AGENTS.md`、`CLAUDE.md`、Cursor rules、Copilot instructions、coding standards、architecture overview | 一次性任务要求、模糊口号、过时 workaround、只适用于某个 feature 的局部经验 |
| 运维与交付层 | 支撑部署、运行、故障响应与发布沟通 | 与运行环境和发布节奏一致 | runbook、deployment docs、release notes、changelog、值班/交接材料 | 架构决策理由、完整需求讨论、与运维无关的开发习惯 |
| 组织知识层 | 面向团队学习与长期改进 | 中长期保留，定期压缩与归档 | 复盘记录、反模式库、最佳实践、经验库、培训材料 | 未经验证的碎片经验、项目私有局部规则被错误上升为组织规范 |

同样重要的是区分几类常见文档的边界。Azure 的 ADR 指南强调：ADR 只记录 architecturally significant decisions，且不要把 ADR 写成 design guide；Rust RFC 则指出并非所有变更都需要 RFC，Bug 修复和文档改进通常走普通 PR 即可；Kubernetes KEP 又进一步展示了 feature 级重要变更如何要求 motivation、design、risks、tests 与 readiness。综合这些实践，最稳妥的边界划分如下。citeturn17view0turn17view2turn16view4turn28view0turn28view1

| 文档类型 | 最适合回答的问题 | 触发条件 | 核心内容 | 不适合承载 |
|---|---|---|---|---|
| PRD / Feature Spec | 我们要让用户获得什么行为 | 需求澄清后、实现前 | 目标、范围、验收标准、边界 | 技术选型细节 |
| 设计文档 | 系统准备如何实现该能力 | 技术方案收敛前后 | 结构、接口、数据流、风险 | 最终历史裁决 |
| ADR | 为什么最终选了这个架构决定 | 关键决定被接受时 | 背景、选项、决策、权衡、状态、后果 | 详细执行步骤 |
| RFC / KEP 类文档 | 重大变更是否应被采纳 | 跨模块/跨团队/高影响变更 | 提案、讨论、影响、测试、分阶段计划 | 小修小补 |
| 模块 README | 这个局部模块怎么用、怎么改、哪些坑最容易踩 | 模块形成稳定维护面 | 局部约定、入口、关键路径、局部限制 | 全局规范 |
| Agent 指令文档 | Agent 在这里工作时必须优先知道什么 | 规则跨任务稳定复用时 | build/test 命令、路由提示、约束规则 | 一次性流程、长篇教程 |
| Runbook | 线上事件发生时该怎么看、怎么缓解、怎么升级 | 有值班/发布/故障恢复需求时 | 可操作步骤、检查项、升级路径、证据 | 架构历史与大段背景 |
| Retro / Pattern Note | 我们以后该保留什么经验、避免什么坑 | 复盘和阶段总结后 | 模式、反模式、触发条件、适用范围 | 细碎日志与未经验证猜测 |

## 知识分类体系与生命周期

本报告采用十二类知识类型来覆盖 AI Coding 从需求到维护全过程中产出的主要知识：**需求知识、领域知识、设计知识、决策知识、实现知识、测试知识、调试知识、约定知识、风险知识、反模式知识、过程知识、证据知识**。之所以额外单列“证据知识”，是因为 AI Coding 的关键不只是“结论”，而是“结论是否能被 issue、commit、PR、代码、测试、日志、ADR 所追溯”。Google 的 code review 指南强调 reviewer 应检查 design、tests 与 relevant documentation 是否更新；CL 描述应写明 why、背景、benchmark 和 design links；这恰好为知识抽取提供了高价值源头。citeturn19view1turn19view2turn19view3

| 知识类型 | 典型问题 | 主要来源 | 默认半衰期 | 默认落点 |
|---|---|---|---|---|
| 需求知识 | 用户真正要什么、什么算完成 | 需求讨论、PRD、issue、评审结论 | 中长期 | PRD / spec / acceptance criteria |
| 领域知识 | 概念、实体、业务不变量是什么 | 领域访谈、既有系统、模型定义 | 长期 | 领域词汇表 / spec / architecture |
| 设计知识 | 模块如何协作、接口如何约束 | 设计文档、接口草图、数据流图 | 中长期 | design doc / architecture |
| 决策知识 | 为什么选 A 不选 B | 设计评审、ADR、RFC、KEP | 长期 | ADR / RFC |
| 实现知识 | 代码怎么组织、关键实现路径是什么 | diff、代码、模块注释、plan | 短中期 | module README / code / plan |
| 测试知识 | 如何验证正确、如何防回归 | 测试计划、CI、缺陷复现、覆盖率 | 中长期 | tests / test plan / quality gates |
| 调试知识 | 症状、根因、修复路径是什么 | 日志、runbook、issue、PR 评论 | 短中期 | scratchpad / runbook / regression test |
| 约定知识 | 怎么命名、怎么提 PR、怎么让 Agent 工作 | code review 反馈、团队约定、工具配置 | 中长期 | coding standards / AGENTS / CLAUDE / rules |
| 风险知识 | 哪里会坏、代价是什么 | 威胁建模、review、incident、性能测试 | 中长期 | design doc / ADR / runbook |
| 反模式知识 | 哪些做法不要再重复 | retro、failed PR、incident 复盘 | 中长期 | anti-pattern library / retrospectives |
| 过程知识 | 任务拆解、阶段结论、handoff 信息 | plan、tasks、交接文档、评论 | 短期 | plan / tasks / handoff |
| 证据知识 | 上述所有结论依附于哪些事实 | issue、commit、PR、tests、logs、metrics | 与其宿主相同 | metadata / links / review records |

为了判断“该不该晋升”，还必须给每条知识附带一组**治理标签**。这组标签不是为了学术上的分类，而是为了直接驱动路由与清理。建议至少做出下表中的七维判断。

| 维度 | 左侧状态 | 右侧状态 | 治理含义 |
|---|---|---|---|
| 一次性 vs 可复用 | 只对当前任务有效 | 将来高概率再次出现 | 决定是否进入长期文档 |
| 局部 vs 全局 | 只影响单文件/单模块 | 影响项目或组织 | 决定写到 module README 还是项目规范 |
| 暂定 vs 已验证 | 仍在讨论或仅为推断 | 有代码/测试/人类确认 | 决定能否进入正式文档 |
| 事实 vs 决策 | 描述“是什么” | 描述“为什么这样做” | 决定落到 spec/README 还是 ADR |
| 显性 vs 隐性 | 已经写明 | 依赖个人经验 | 决定是否需要抽取显性化 |
| 人类意图 vs Agent 推断 | 来自用户/开发者明确表达 | 来自模型归纳 | 决定证据门槛与审核强度 |
| 当前需求 vs 项目长期 | 只与当前 feature 绑定 | 会跨 feature 持续有效 | 决定晋升与 TTL |

知识生命周期方面，当前工具链已经能覆盖大量自动化基础：Codex hooks 支持在 turn 结束时做验证、摘要、memory 生成；Claude hooks 支持自动格式化、阻止修改受保护文件、在 compaction 后重新注入上下文；Copilot cloud agent 可以在分支上研究仓库、规划、改文档并生成 PR；Codex automations 可以按计划在后台 worktree 中运行维护任务；OpenAI Cookbook 已把 traces、feedback、evals、repair loops 明确串成 agent improvement loop；而文档漂移检测研究也证明，代码与说明之间的过期引用可以自动发现。基于这些证据，下表给出一套完整的知识生命周期流程。citeturn33view0turn11view4turn11view5turn33view2turn33view3turn33view4turn29view0

| 生命周期步骤 | 输入 | 输出 | 判断标准 | 参与角色 | 自动化可能性 |
|---|---|---|---|---|---|
| Capture | 对话、diff、测试结果、PR 评论、日志、issue | 原始知识片段 | 是否包含可复述信息或可追踪事件 | Agent / 开发者 / CI | 高 |
| Extract | 原始片段 | 结构化候选条目 | 是否能抽成“陈述 + 证据 + 作用域 + 置信度” | Knowledge Extractor | 高 |
| Classify | 候选条目 | 类型、作用域、时效、验证状态 | 属于哪类知识，影响范围多大 | Classifier | 中高 |
| Normalize | 多个候选条目 | 去重、压缩、统一命名后的条目 | 是否与已有条目语义重复或只是同义表述 | Extractor / Router | 高 |
| Validate | 条目 + 代码/测试/历史记录 | 验证状态升级后的条目 | 是否有代码、测试、issue、日志、人类确认支撑 | Evidence Linker / Reviewer | 中 |
| Route | 已验证条目 | 目标文档建议 | 哪一层最合适，是否需要写入长期记忆 | Router | 中 |
| Promote | 局部或临时条目 | 晋升提案 | 复用、影响、稳定、证据是否达阈值 | Promotion Agent / 人类 | 中 |
| Demote | 长期文档条目 | 降级、归档或删除提案 | 是否作用域收缩、过时、冲突、低复用 | Demotion Agent / 人类 | 中 |
| Link | 条目 + issue/PR/commit/tests/ADR | 双向追踪关系 | 是否能回答“这条知识由什么证明” | Evidence Linker | 高 |
| Review | 文档 patch + 风险说明 | 审核意见 | 是否正确、必要、范围合适、无冲突 | CODEOWNER / 架构师 / PM / SRE | 中 |
| Publish | 审核通过的 patch | 正式文档 | 是否满足 branch protection 与 required reviews | Writer Agent / GitHub | 高 |
| Monitor | 现有文档、代码演化、发布时间 | 漂移/重复/冲突报告 | 是否 last_verified 超期、与代码不符、内容重复 | Maintenance Agent | 高 |
| Retire | 过期或 superseded 条目 | 归档/删除/替代链 | 是否只剩历史价值或已完全失效 | Maintenance Agent / 人类 | 中 |

## 晋升、降级与路由机制

**晋升的定义**：一条知识从**更低权威、更短寿命、更小作用域、更难发现**的载体，移动到**更高权威、更长寿命、更广作用域、更易检索**的载体。典型例子是“需求讨论中的结论 → 验收标准”“模块内稳定坑点 → 模块 README”“技术选型讨论 → ADR”“反复发生的 review feedback → 项目规范或 Agent 规则”。  
**降级的定义**：一条知识从**高权威、高作用域、长期保留**的文档，退回到**更局部、更短期、历史归档或删除**的载体。典型例子是“项目级规则收缩为模块级规则”“AGENTS 规则降级到 path-specific rule”“旧需求文档归档”“过时调试经验压缩为运行知识或直接删除”。

这种定义与外部实践是对齐的。Azure 明确要求 ADR 采用 append-only log，接受后的记录不要回改，而是用新的记录 supersede 旧记录，并显式保存状态、置信度、背景与 rationale；Google Cloud 说明 ADR 适合记录主要选项、关键要求与设计决策，并常与代码放在一起；Rust RFC 说明重要 feature 走 RFC，而 bug fix 与文档改进可走普通 PR；Kubernetes KEP 则要求 feature 和 significant changes 提供 motivation、design、risks、tests 与 readiness。从这些实践可以看出：**“是否值得晋升”首先不是写作问题，而是影响范围、历史意义、可逆性和证据充分度的问题。** citeturn17view0turn17view2turn16view3turn16view4turn28view0turn28view1

同样，哪些知识应进入 Agent 指令文档，也有一条很清晰的边界。OpenAI 建议 `AGENTS.md` 保持简短、实用、只在反复出错时追加规则，且更细致的内容应拆到 task-specific markdown；Copilot 官方文档明确区分“几乎每个任务都相关的 custom instructions”和“只有在相关任务时才应使用的 skills”；Claude 官方文档则明确建议：若内容是 multi-step procedure 或只对一部分代码生效，应移到 skill 或 path-scoped rule，而不是堆进 `CLAUDE.md`。这意味着：**只有“高频复用、稳定、可动作化、作用域正确”的知识，才应该晋升为项目规范或 Agent instructions。** citeturn21view0turn11view3turn23view0turn25search11turn25search19

本报告建议采用一个**晋升评分模型**。评分不是替代人类判断，而是把“该不该写进长期文档”从模糊感觉变成可审查的案卷。建议所有维度按 0–5 打分，先算正向价值，再扣减风险。

| 正向维度 | 权重 | 评分含义 |
|---|---:|---|
| 复用频率 | 15 | 未来多少任务会再次用到 |
| 影响范围 | 15 | 影响单文件、模块、项目还是组织 |
| 稳定性 | 14 | 这条知识是否已经收敛、不易频繁改写 |
| 置信度 | 12 | 是用户明确表达、多人确认，还是 Agent 推断 |
| 证据完整度 | 12 | 是否有 issue / PR / code / tests / logs / metrics 支撑 |
| 未来检索价值 | 10 | 六个月后别人是否会主动搜索它 |
| 对 Agent 行为的约束价值 | 12 | 提前告知 Agent 是否能显著减少错误 |
| 对团队协作的价值 | 10 | 是否能降低沟通成本、review 成本、交接成本 |

| 风险维度 | 扣分建议 | 评分含义 |
|---|---:|---|
| 过时风险 | 0–10 | 越快失效，扣分越高 |
| 写入成本 | 0–5 | 维护成本越高，扣分越高 |
| 错误晋升风险 | 0–10 | 一旦写错会不会误导很多后续任务 |

建议公式：  
**Promotion Score = 正向加权总分 − 风险扣分**  

建议阈值：  
**75 分及以上**：可以作为正式晋升候选。  
**60–74 分**：只晋升到局部长期层，或进入待审核池。  
**60 分以下**：默认留在临时上下文、plan、notes、issue comment 或归档层。  

此外，目标文档还应设置**额外门槛**：

| 晋升目标 | 额外门槛 |
|---|---|
| 需求文档 / 验收标准 | 必须是用户可观察行为，且能被验证或转化为测试 |
| 模块 README | 必须是局部稳定知识，且仅靠读代码不容易快速恢复 |
| ADR | 必须是 architecturally significant、难以逆转、替代方案已比较 |
| RFC / KEP 类文档 | 必须是高影响变更，且需要跨角色达成共识 |
| AGENTS / CLAUDE / Copilot / Cursor rules | 必须高频复用、稳定、对 Agent 约束价值高，并且不是 task-specific |
| Runbook | 必须能支持值班/响应/恢复，且操作性强 |
| 测试 | 必须可复现、可断言、能防止未来同类问题再次出现 |

将这些阈值落到用户关心的典型场景，大致可得到如下判据：

| 场景 | 应何时晋升 | 推荐目标 |
|---|---|---|
| 临时发现晋升为需求文档 | 当它改变用户可见行为、已获得业务确认、可形成验收标准 | spec / acceptance criteria |
| 实现细节晋升为模块 README | 当它是局部稳定约定、维护者会反复踩坑、代码本身不够显式 | module README |
| 技术选择晋升为 ADR | 当它影响系统结构/质量属性、难以回滚、替代方案已比较 | ADR |
| 反复出现的修复经验晋升为团队规范 | 当同类 review/incident 连续出现且跨 feature 复发 | coding standards / team practice |
| 局部约定晋升为 Agent 规则 | 当 Agent 在相同路径反复犯同类错误，且规则能明确约束其行为 | path-specific rule / AGENTS / CLAUDE |
| Bug 修复经验晋升为测试或 runbook | 当问题能复现且未来再发成本高；值班场景则优先 runbook | regression test / runbook |
| 需求讨论结论晋升为验收标准 | 当结论形成外部可观察结果，可被 Given-When-Then 或断言表达 | acceptance criteria / executable spec |

对**需求结论 → 测试** 这条通路，Cucumber 的经验很值得借鉴。Cucumber 将场景定义为 executable specification，并强调 `Then` 应描述可观察输出，而不是深埋在系统内部的行为；场景既是规格，也是 living documentation，也是自动化测试。这给 AI Coding 一个非常实用的路由原则：**凡是影响外部行为、接口契约、回归风险或业务不变量的知识，优先考虑直接晋升为测试。** citeturn26view0turn26view1turn26view2turn26view3

降级方面，最常见的情况有六类。  
第一，**作用域收缩**：原本写在项目级规范里的规则，后来发现只适用于某个子目录或某类文件，应降级为 path-specific rule、模块 README 或 feature 局部说明。Claude、Copilot、Codex、Cursor 都提供了不同层次的局部规则机制，这正是防止“局部经验错误上升为全局规则”的最佳武器。citeturn23view0turn12view1turn21view2turn24search0

第二，**ADR 被 supersede，而不是被重写**。这是硬规则。ADR 的价值之一就是保留“当时为什么这么想”。一旦事后直接改掉旧 ADR，就失去了历史决策价值，并破坏了以后回溯变迁的能力。Azure 对 append-only log 的要求正是为了防止这种“历史洗白”。citeturn17view0turn17view2

第三，**旧需求与实现脱节后应归档**。特别是 feature 已下线、接口已变更、或需求已被替代的情况下，继续保留其高可见度只会误导 Agent。相关材料可以保留历史副本，但必须退出默认检索与默认上下文。

第四，**历史调试经验应压缩而非无上限累加**。Google SRE 对 playbook 的讨论非常有启发：有些团队偏好 general guidance，有些偏好 step-by-step；争议点在于写得越细，越容易过时。Google 的建议是至少确定一套最小结构，并主动把新的 hard-won production knowledge 转成 automation 或 consoles；如果 playbook 已经是一串每次都跑的固定命令，那就应该自动化。citeturn35view0

第五，**文档与代码不一致时要优先修正权威层级更高、但事实更旧的那一方**。Springer 的实证研究对 3000 多个 GitHub 项目做分析，发现多数项目在历史上都出现过过期代码元素引用，并给出了从文档中抽取代码引用、与当前代码实例对比的自动检测方法。这为“文档漂移扫描”提供了直接可操作的检测思路。citeturn29view0

第六，**Agent instructions 冲突或过度约束时，应优先拆分、下沉和删除**。Anthropic 明确提醒：若多个 `CLAUDE.md` 规则冲突，Claude 可能任意选一个；OpenAI 也建议把 guidance 放到离作用域最近的位置，而不是把所有内容都堆到主 `AGENTS.md`。citeturn23view0turn21view3

为控制文档熵增，建议所有长期知识条目都带以下元数据，并在 CI 或 maintenance workflow 中定期核验：

| 元数据字段 | 作用 | 建议规则 |
|---|---|---|
| `owner` | 责任归属 | 与 CODEOWNERS 或团队 owner 一致 |
| `last_verified` | 最后验证时间 | 过期即进入 review 队列 |
| `evidence` | 证据链接 | 至少一条 issue/PR/commit/test/log 关联 |
| `scope` | 适用范围 | task / module / feature / project / org 必填 |
| `confidence` | 置信度 | 低置信只能是草案，不得直接进入全局规范 |
| `expiry` / TTL | 到期时间 | plan/notes/debug 需要默认 TTL；ADR 不设过期但要状态复查 |
| `supersedes` / `superseded_by` | 替代关系 | ADR、spec、runbook 都应显式记录替代链 |
| `status` | 生命周期状态 | draft / proposed / accepted / active / superseded / retired |
| `conflict_keys` | 冲突检测键 | 用于查重、查范围冲突、查关键术语冲突 |
| `review_cadence` | 审查周期 | 按文档层级与风险设定不同 cadence |
| `deletion_criteria` | 删除标准 | 例如 feature 下线、代码删除、超过 TTL 无访问等 |

**建议的默认复核节奏**可以很务实：  
任务 notes / debug notes：7–30 天；  
plan / tasks：合并后 14–30 天归档；  
模块 README：90 天或模块有大改时复核；  
Agent 项目规则：30–60 天复核；  
runbook：30–90 天复核；  
ADR：180 天或受影响组件大改时复核；  
反模式库与经验库：180 天复核。  
GitHub 的 `actions/stale` 模式已经证明，TTL、提醒与自动关闭完全可以通过工作流实现。citeturn20view2turn20view0turn17view2

最后给出一套**知识到文档层级的默认路由表**。它不是法律，而是一套高置信的默认值；任何偏离它的情况，必须在 PR 中说明“为什么”。

| 知识类型 | 典型来源 | 判断标准 | 默认落点文档 | 可晋升目标 | 可降级目标 | 是否需人审 | 示例 |
|---|---|---|---|---|---|---|---|
| 用户可见行为 | 需求讨论、PRD、评审结论 | 外部可观察、可验收 | spec / acceptance criteria | executable spec / tests | archive | 是 | “提交成功后展示 X 提示” |
| 领域概念与不变量 | 讨论、数据模型、历史系统 | 跨 feature 稳定 | domain glossary / architecture | spec / tests | module README | 是 | “订单状态不可逆回退” |
| 架构权衡 | 设计评审、方案比较 | 影响结构和质量属性 | ADR | RFC / standards | archive | 是 | “选事件溯源而非 CRUD” |
| 重大 feature 提案 | 跨团队讨论、路线图 | 高影响、需共识 | RFC / KEP-style doc | ADR / project standard | design note | 是 | “引入全局缓存层” |
| 接口契约 | API 设计、类型定义 | 需要长期稳定和验证 | API docs / contract tests | ADR（若牵涉选型） | module README | 是 | “返回码与错误体结构” |
| 模块内部实现约定 | diff、代码 review、局部经验 | 只影响局部且稳定 | module README | path-specific rules | notes | 视风险 | “本模块统一用 repository pattern” |
| 构建/测试命令 | README、CI、review 反馈 | 高频复用、跨任务稳定 | AGENTS / CLAUDE / Copilot instructions | skills | module README | 低中风险可免 | “修改 JS 后必须跑 pnpm test” |
| 一次性 debug 过程 | 排障记录、日志 | 仅当前问题有效 | scratchpad / issue comment | runbook / regression test | delete | 否 | “本次排查的临时 grep 链路” |
| 可复现 bug 修复路径 | issue、日志、修复 PR | 可重现、未来高回归风险 | regression test / runbook | project standard | archive | 是 | “某输入组合触发空指针” |
| 重复 review 反馈 | PR 评论、代码审查 | 同类反馈反复出现 | coding standards / path rules | AGENTS / CLAUDE | module README | 是 | “禁止直接在 controller 写业务逻辑” |
| 发布与缓解动作 | 值班、incident、发布后观察 | 必须可操作 | runbook / release docs | automation | archive | 是 | “回滚顺序与 smoke checks” |
| 失败经验与禁做事项 | retro、事故复盘 | 有通用警示价值 | anti-pattern library | team standard | archive | 是 | “不要把 feature flag 写成静态常量” |

## Agent 协作架构与落地方案

现有工具已经提供了构建多 Agent 文档治理流水线的必要积木。Codex 支持 subagents 并行分工、skills 作为按需加载的复用流程、hooks 作为确定性扩展点、automations 作为周期后台作业；Copilot cloud agent 能在 GitHub Actions 驱动的临时开发环境中研究仓库、制定计划、修改代码或文档并提交 PR 供人类审核；Claude hooks 能自动格式化、阻止修改受保护文件、在 compaction 后重注上下文，还支持项目级和路径级规则。换言之，**“识别—判断—建议—开 PR—等待审核—发布—监控”的闭环，今天已经有足够的工具基础实现。** citeturn22view0turn33view0turn21view4turn33view2turn11view5turn11view4

建议的多 Agent 分工如下：

| Agent | 职责 | 输入 | 输出 | 触发时机 | 权限边界 | 失败风险 | 审查要求 |
|---|---|---|---|---|---|---|---|
| Knowledge Observer Agent | 观察潜在知识出现 | 对话、diff、tests、PR comments | 候选知识清单 | 每次任务结束、PR 更新、CI 结束 | 只读 | 把噪声当信号 | 可自动运行 |
| Knowledge Extractor Agent | 非结构化转结构化 | 候选知识片段 | JSON/YAML 知识条目草稿 | Observer 之后 | 只读 | 漏证据、误摘要 | 可自动运行 |
| Knowledge Classifier Agent | 判定类型与作用域 | 结构化草稿 | 类型、scope、stability、reuse 标签 | Extract 后 | 只读 | 作用域误判 | 可自动运行，需抽样审计 |
| Evidence Linker Agent | 建立证据与追踪链 | issue、PR、commit、tests、logs | `evidence[]`、`related_*[]` | Classify/Validate 阶段 | 只读 | 链接缺失或错链 | 高风险条目需人审 |
| Documentation Router Agent | 判断目标文档层级 | 已验证条目 + 现有文档图谱 | 路由建议 | Validate 后 | 只读 | 写错载体 | 中高风险需人审 |
| Promotion/Demotion Agent | 给出晋升/降级提案 | 路由结果 + 分数 + 历史访问/复核信息 | 建议动作与理由 | 周期作业 / PR 时 | 只读 | 过度晋升或误降级 | 必须可追溯，可人审 |
| Conflict Detector Agent | 检测冲突、重复、过度约束 | 现有 docs + 新提案 + 代码状态 | conflict report | Publish 前 / 周期扫描 | 只读 | 漏报冲突 | 中高风险需人审 |
| Human Review Workflow | 最终裁决高风险改动 | 文档 patch、证据、分数、冲突报告 | approve / request changes | PR 流程 | 评审权限 | 审核疲劳 | 必要 |
| Documentation Writer Agent | 生成 patch，不直接改主干 | 批准后的提案 | docs diff / PR | Review 前或后 | 分支写权限，不可直推 main | 过度改写、风格漂移 | 必须走 PR |
| Maintenance Agent | 周期保洁 | repo docs、code、history、TTL | stale/conflict/drift/missing 报告 | 定时每天/每周 | 只读或 worktree | 误报过多 | 低风险自动，关键项人审 |

**关键治理原则**应是“检测自动化、写作半自动化、合并严格人工控制”。GitHub 的 branch protection、required reviews、CODEOWNERS 很适合作为最后一道门；Agent 只负责提出 patch，不负责直接把长期文档写进主干。citeturn20view0turn20view1turn11view5

为了兼容多工具并避免规则碎片化，建议采用**“canonical once, compile many”** 的目录策略：只维护一份规范化知识源，再自动生成或同步到各工具所需格式。原因很简单：当前工具的作用域和文件名并不统一。Codex 采用 `AGENTS.md` 分层扫描，Claude 采用 managed/user/project/local 的 `CLAUDE.md` 与 `.claude/rules/`，Copilot 采用 `.github/copilot-instructions.md`、`.github/instructions/*.instructions.md` 与 `AGENTS.md`，Cursor 采用 Project/Team/User Rules 与 `AGENTS.md`。如果团队直接手写四套文件，冲突几乎是必然的。citeturn21view2turn23view0turn12view1turn12view3turn24search0

推荐目录结构如下：

```text
/docs
  /product
    vision.md
    glossary.md
  /specs
    /feature-<id>
      spec.md
      acceptance.md
  /architecture
    overview.md
    /design
      /feature-<id>.md
    /adr
      adr-0001-<title>.md
    /rfc
      rfc-0001-<title>.md
  /operations
    /runbooks
      service-<name>.md
    /releases
      release-<version>.md
      changelog.md
  /knowledge
    /patterns
    /anti-patterns
    /retrospectives
/features
  /feature-<id>
    notes.md
    plan.md
    tasks.md
    test-plan.md
    migration.md
/agent
  /policy
    global.yaml
    paths/
      api.yaml
      frontend.yaml
      ops.yaml
  /skills
    /doc-governance
      SKILL.md
      checklists/
      templates/
/.generated
  AGENTS.md
  CLAUDE.md
  cursor-rules.md
  copilot-instructions.md
  copilot-path-rules/
/.github
  CODEOWNERS
  pull_request_template.md
  /instructions
  /workflows
    docs-drift.yml
    docs-stale.yml
    promotion-review.yml
```

推荐的知识条目元数据 schema 如下。它既能服务 markdown frontmatter，也能被后续脚本、CI、检索系统与 Agent 消费。MkDocs 等 docs-as-code 工具对 YAML frontmatter 有原生支持，而 GitHub/CODEOWNERS/ADR 状态管理又给这些字段提供了现实落点。citeturn5search2turn20view0turn17view2

```yaml
id: KG-2026-000123
title: "API rate-limit error format must expose retry_after"
type: decision            # requirement | domain | design | decision | implementation | test | debug | convention | risk | anti_pattern | process | evidence
scope: project            # task | module | feature | project | org
audience:
  - human
  - agent
status: proposed          # draft | proposed | accepted | active | superseded | retired
confidence: 4             # 0-5
stability: 4              # 0-5
reuse_score: 4            # 0-5
impact_score: 4           # 0-5
owner: "@backend-team"
created_at: 2026-05-31
last_verified: 2026-05-31
review_cadence: "30d"
expiry: null
source:
  - type: pr
    ref: "#482"
  - type: issue
    ref: "#471"
evidence:
  - type: commit
    ref: "abc1234"
  - type: test
    ref: "tests/api/test_rate_limit.py::test_retry_after"
  - type: log
    ref: "s3://ops-logs/incident-2026-05-28"
related_code:
  - "src/api/errors.ts"
related_tests:
  - "tests/api/test_rate_limit.py"
related_docs:
  - "docs/specs/feature-17/acceptance.md"
promotion_candidate: true
promotion_target: "docs/architecture/adr/adr-0017-rate-limit-format.md"
demotion_candidates:
  - "features/feature-17/notes.md"
supersedes: null
superseded_by: null
conflict_keys:
  - "rate_limit_error_format"
human_confirmation_required: true
notes: >
  Agent may not treat this as accepted until reviewer approval is linked.
```

从“需求开始”到“上线后复盘”的推荐工作流如下：

| 阶段 | 主要产物 | Agent 参与方式 | 人类控制点 |
|---|---|---|---|
| 需求输入 | issue / PRD 草稿 | Observer 抓取需求陈述 | PM / requester 确认 |
| 需求澄清 | spec 草稿 | Extractor 提炼需求、边界、问题清单 | PM / Eng lead 确认 |
| spec 生成 | `spec.md` / `acceptance.md` | Router 建议落点，Writer 生成 patch | PM / QA 审核 |
| 方案设计 | design doc | Classifier 识别设计知识与风险 | Architect / senior reviewer |
| 决策固化 | ADR / RFC 草稿 | Promotion Agent 提议晋升 | Architect / tech lead |
| plan / tasks | `plan.md` / `tasks.md` | Writer 生成执行计划 | 实施负责人确认 |
| coding | diff / comments | Observer 持续抓取实现与局部经验 | 开发者主导 |
| test | test plan / CI result | Extractor 识别回归知识与缺陷模式 | QA / CI gate |
| PR review | PR comments / revised docs | Conflict Detector 检测冲突与漏文档 | Reviewer / CODEOWNER |
| documentation diff | docs patch | Writer 只生成 patch，不直推主干 | Reviewer 审核 |
| knowledge promotion review | promotion queue | Promotion/Demotion Agent 出分与理由 | 人类批准或驳回 |
| merge | 合并后的正式文档 | Publish + link update | branch protection |
| post-merge maintenance | stale/drift/conflict report | Maintenance Agent 定时扫描 | owner 周期复核 |
| post-release retro | retro / anti-patterns | Extractor 汇总 learned lessons | 团队复盘确认 |

下面给出一组可直接复用的 Agent prompt 模板示例。它们的重点不是语言华丽，而是**输出结构固定、边界明确、拒绝无证据晋升**。

```yaml
knowledge_observer:
  system_prompt: |
    你是 Knowledge Observer Agent。
    只识别候选知识，不做最终判断，不写正式文档。
    输入可能来自：需求讨论、代码 diff、测试结果、PR 评论、日志。
    你的任务：
    1. 提取“值得进一步审查”的候选项；
    2. 区分 fact / decision / inference / process；
    3. 给出 source_refs、scope_guess、confidence_guess；
    4. 若只有猜测，没有证据，必须标成 inference_only。
  output_json:
    - id
    - statement
    - kind
    - source_refs[]
    - scope_guess
    - confidence_guess
    - inference_only

knowledge_extractor:
  system_prompt: |
    你是 Knowledge Extractor Agent。
    把非结构化上下文压缩为结构化知识条目。
    不得把未验证推断改写成事实。
    每条条目必须包含 evidence_needed。
  output_json:
    - id
    - normalized_statement
    - type
    - scope
    - status
    - evidence_present[]
    - evidence_needed[]
    - owner_suggestion

knowledge_classifier:
  system_prompt: |
    你是 Knowledge Classifier Agent。
    根据条目内容判断：
    - type
    - scope
    - stability(0-5)
    - reuse(0-5)
    - impact(0-5)
    - candidate_destination
    若 scope 或 stability 不明确，返回 needs_human_confirmation=true。
  output_json:
    - id
    - type
    - scope
    - stability
    - reuse
    - impact
    - candidate_destination
    - needs_human_confirmation

evidence_linker:
  system_prompt: |
    你是 Evidence Linker Agent。
    你的任务是把知识条目与 issue、commit、PR、代码文件、测试、日志建立双向追踪关系。
    没有可核验证据的条目不得标记为 accepted。
  output_json:
    - id
    - evidence[]
    - related_code[]
    - related_tests[]
    - related_issues[]
    - validation_status

documentation_router:
  system_prompt: |
    你是 Documentation Router Agent。
    你要在这些目标中选择一个默认落点：
    scratchpad, spec, design_doc, adr, rfc, module_readme,
    agents_rule, path_rule, runbook, test, retrospective, archive。
    你的首要原则是 scope correctness，其次是 context cost minimization。
    若条目只对局部有效，禁止路由到 project-wide instructions。
  output_json:
    - id
    - default_destination
    - alternative_destinations[]
    - rationale
    - human_review_required

promotion_demotion_agent:
  system_prompt: |
    你是 Promotion/Demotion Agent。
    请对每条知识按以下维度打分：
    reuse, impact, stability, confidence, evidence, retrieval_value,
    agent_constraint_value, collaboration_value, staleness_risk,
    write_cost, false_promotion_risk。
    给出 promote / keep_local / demote / archive / delete 的建议。
  output_json:
    - id
    - score_breakdown
    - recommended_action
    - recommended_target
    - rationale
    - risks[]
    - review_level

conflict_detector:
  system_prompt: |
    你是 Conflict Detector Agent。
    检测以下冲突：
    1. 新文档与旧文档冲突；
    2. 文档与代码/测试冲突；
    3. 全局规则与局部规则冲突；
    4. 重复规则与过度约束。
    输出必须区分 hard_conflict 与 soft_overlap。
  output_json:
    - id
    - hard_conflicts[]
    - soft_overlaps[]
    - duplicate_candidates[]
    - recommended_resolution

documentation_writer:
  system_prompt: |
    你是 Documentation Writer Agent。
    你只能生成 reviewable patch，不得直接合并。
    patch 必须最小化、不扩写、不新增无证据段落，并附带变更理由与证据链接。
  output_format: unified_diff

maintenance_agent:
  system_prompt: |
    你是 Maintenance Agent。
    周期扫描整个仓库，发现：
    - stale docs
    - code-doc drift
    - missing owner
    - expired last_verified
    - orphaned notes
    - duplicated rules
    只生成报告或 PR，不做静默修改。
  output_json:
    - stale_items[]
    - drift_items[]
    - duplicate_items[]
    - missing_docs[]
    - suggested_actions[]
```

文档更新 PR 模板建议如下：

```markdown
## Summary
说明本 PR 为什么更新文档，以及对应的代码/需求/事故背景。

## Knowledge changes
| Knowledge ID | Action | Target Doc | Scope | Risk |
|---|---|---|---|---|
| KG-... | promote | docs/specs/... | feature | medium |

## Rationale
- 这条知识为什么需要写入该层级
- 为什么不写到更高/更低层级
- 是否会增加 always-on context 负担

## Evidence
- Issue:
- PR:
- Commit:
- Tests:
- Logs / Metrics:
- ADR / RFC:

## Review requirements
- [ ] 需求或用户可见行为已由 PM/产品方确认
- [ ] 架构决策已由技术负责人确认
- [ ] 全局规则已由 CODEOWNER 审核
- [ ] 无纯 Agent 推断被写成事实
- [ ] 无局部经验被错误提升为全局规范
- [ ] 相关测试/自动检查已补齐或明确说明为什么不能补齐

## Conflict check
- [ ] 已检查与 AGENTS / CLAUDE / Copilot / Cursor rules 是否冲突
- [ ] 已检查与现有 ADR / spec / runbook 是否重复
- [ ] 已检查与代码实现是否一致

## Lifecycle metadata
- owner:
- last_verified:
- review_cadence:
- expiry:
- supersedes:
- superseded_by:
```

## 风险、评估指标、最佳实践与反模式

AI Coding 文档治理的最大风险，不是“文档不够”，而是**低质量知识进入了错误层级并长时间存活**。Anthropic 已明确提醒：上下文型指令不保证严格遵守；OpenAI 则警告 context pollution/context rot；Google 的工程实践强调 reviewer 必须关注 design、tests 与 docs 更新；文档漂移研究又表明过期引用会长期存活。这些风险一旦叠加，就会形成“Agent 越做越懂，但懂的越来越脏”的知识污染闭环。citeturn23view0turn30search0turn19view1turn29view0

| 风险 | 典型成因 | 治理策略 |
|---|---|---|
| Agent 把猜测当事实写入文档 | 抽取时未区分 inference 与 fact | 强制 `status=draft`、`confidence` 字段；无证据不得晋升；高风险变更必须人审 |
| 局部实现细节错误晋升为全局规则 | 作用域识别失败 | 路由先看 scope；能 path-specific 就不 project-wide；全局规则需要更高阈值 |
| 文档越来越厚，Agent 上下文负担过重 | 把教程、plan、历史、规范混在 always-on 文件 | 设定上下文预算；把详细流程移到 skills / path rules；删掉无效规则 |
| AGENTS / CLAUDE 规则过多，执行效果反而下降 | 冗长、模糊、冲突 | 主文件只保留“每次都需要”的规则；closest-scope wins；按路径拆分 |
| 多文档互相冲突 | 多工具多来源手写 | canonical once, compile many；Conflict Detector；统一 precedence map |
| 文档与代码不同步 | 无 last_verified、无 drift scan | 代码—文档链接；CI/定时扫描；检测过期代码引用；重要变更时强制 docs review |
| ADR 被事后改写 | 把 ADR 当活文档而不是历史记录 | append-only；新 ADR supersede 旧 ADR；保留状态链 |
| 人类审核成本过高 | 所有更新都同级审查 | 采用风险分级：局部 README 低成本，高风险规则/ADR/需求必须严审 |
| Agent 自写自审自执行闭环失真 | 无 separation of duties | Writer 不可直推主干；Reviewer 不能复用同一 Agent 配置；最终门禁在 protected branch |
| 低质量知识进入长期记忆后持续污染 | 没有 TTL、last_verified、deletion criteria | 所有长期知识带 lifecycle metadata；过期自动进队列；无人认领则降级/删除 |
| 工具之间规则格式不统一 | Codex / Claude / Copilot / Cursor 各有入口 | 维护统一 policy source，再生成适配文件；不要把规范散落四处手工同步 |

评估体系建议围绕**正确性、时效性、负担、协作收益**四类指标展开。下面这些指标足以支撑中小团队首轮治理，而不会过度工程化：

| 指标 | 定义 | 目标方向 |
|---|---|---|
| Promotion precision | 被晋升条目中，30 天内未被撤回/回滚的比例 | 越高越好 |
| Promotion recall | 复盘时发现“本应晋升却没晋升”的比例 | 越低越好 |
| Drift rate | 扫描中发现的文档—代码不一致条目 / 扫描文档数 | 越低越好 |
| Duplicate rule rate | 规则库中重复或强重叠条目比例 | 越低越好 |
| Conflict rate | 新增文档提案触发 hard conflict 的比例 | 越低越好 |
| Review lead time | 文档 PR 从创建到批准的时长 | 越低越好 |
| Agent adherence gain | 被写入规则后的重复错误下降幅度 | 越高越好 |
| Test promotion rate | 可复现 bug 被转成 regression tests 的比例 | 越高越好 |
| Runbook freshness | `last_verified` 在周期内的 runbook 比例 | 越高越好 |
| Context budget occupancy | always-on 文档占用预算 | 保持可控 |

其中，**上下文预算**尤其要量化。Claude 建议 `CLAUDE.md` 目标低于 200 行；Codex 的 project guidance 链默认有 32 KiB 大小限制；GitHub 生成 Copilot onboarding instructions 的官方示例甚至要求“不超过两页，且不能 task specific”。所以团队至少要有一个“project-wide instructions 总长度”红线。citeturn23view0turn21view2turn12view1

下面这份 checklist 可以直接作为日常 review 的门槛：

- [ ] 这条内容到底是**事实、决策、过程、风险、还是推断**？
- [ ] 它的作用域是 **task / module / feature / project / org** 哪一层？
- [ ] 它是否已经有更高权威、更新鲜的宿主文档？
- [ ] 它是否有可追踪的 issue / PR / commit / test / log / metric 证据？
- [ ] 这条知识是不是只对“当前这次任务”有用？
- [ ] 它更适合作为 **skill / path rule / test / automation / runbook** 吗？
- [ ] 如果写入 always-on instructions，会不会明显增加上下文负担？
- [ ] 如果写错，误导面会有多大？是否足以要求人类审核？
- [ ] 是否已经设置 `owner`、`last_verified`、`review_cadence`、`expiry`？
- [ ] 是否存在与旧文档、旧规则、当前代码的冲突？

最佳实践方面，最重要的不是“多写”，而是“**写对层、写对长度、写对宿主、写成可验证对象**”。结合官方与研究证据，本报告推荐以下实践：  
**一是就近原则**：作用域局部，就写局部；不要把模块经验上升为项目宪法。  
**二是证据先行**：没有链接就没有晋升。  
**三是能执行就不只写文案**：回归缺陷转测试，固定操作转 automation/hook。  
**四是历史单独存放**：ADR 记“为什么”，design doc 记“怎么想的”，runbook 记“怎么处置”。  
**五是项目规范瘦身**：始终加载文件只保留 build/test/review/约束型知识。  
**六是定期修剪**：像修 bonsai 一样持续删掉过时、错误、重复内容。citeturn19view0turn17view2turn35view0

与之对应的反模式也很清楚：把一次性 debug 过程塞进 AGENTS、把 ADR 写成教程、把临时方案写进架构总览、把 path-specific 规则写成项目全局条款、把 Agent 自己的推断当作“项目共识”、把 runbook 变成没人敢更新的巨型百科、把规范文本当执行控制的替代品。这些做法都会让文档治理从“降低认知负担”变成“制造二次复杂度”。

## 参考资料与最终建议

本报告的关键依据主要来自以下一手资料与研究文献：OpenAI Codex 官方文档中的 `AGENTS.md`、skills、subagents、hooks、automations、workflows 与 improvement loop；Anthropic Claude Code 文档中的 `CLAUDE.md`、auto memory、rules、skills、hooks；GitHub Copilot 官方文档中的 repository/path-specific instructions、skills、custom agents、cloud agent、CODEOWNERS、protected branches；Cursor 官方文档对 Project/Team/User Rules 与 `AGENTS.md` 的说明；Google Cloud 与 Azure 的 ADR 指南；Rust RFC Book 与 Kubernetes KEP 过程说明；Cucumber 关于 executable specifications / living documentation 的文档；Google Engineering Practices、Google Style Guide、Google SRE 的 playbook/runbook/incident management 实践；以及 ACL Anthology 的长上下文研究与 Springer 关于文档漂移检测的实证研究。citeturn21view2turn21view4turn33view0turn33view2turn23view0turn12view1turn12view3turn24search0turn17view0turn16view3turn16view4turn28view1turn26view0turn19view1turn35view0turn31view0turn29view0

在所有调研对象中，我给出的**最终建议方案**是一个面向中小型 AI Coding 项目的“低负担、高约束、强证据”框架：

| 项目规模 | 必做项 | 可选项 | 不建议一开始就做 |
|---|---|---|---|
| 个人项目 | feature spec、模块 README、最小 `AGENTS/CLAUDE`、回归测试 | 周期扫描、简单 notes 归档 | 完整 RFC 体系、复杂多 Agent 管道 |
| 小团队 | spec、design、ADR、模块 README、Agent 适配层、CODEOWNERS、docs PR 模板 | path rules、runbook、weekly maintenance、promotion queue | 组织级知识图谱、重量级审批流 |
| 中大型团队 | spec、design、ADR/RFC、runbook、统一 policy source、多 Agent 维护、drift CI、周期复核 | traces/evals、自动 promotion suggestion、知识评分 dashboard | 把所有知识都自动写入长期记忆 |

如果团队只采纳一套**最小可行治理框架**，我建议是下面这套：

1. **定义四个默认宿主**：`spec.md`、`design.md`、`ADR`、`module README`。  
2. **定义一个统一的 Agent 规范源**：再生成 `AGENTS.md`、`CLAUDE.md`、Copilot instructions、Cursor rules。  
3. **明确晋升门槛**：没有 evidence、不清楚 scope、不稳定，就不晋升。  
4. **明确降级门槛**：过期、冲突、作用域收缩、无 owner，就降级或归档。  
5. **把高价值知识转成可执行控制**：回归知识进 tests，固定操作进 hooks/automation，合并控制进 protected branches。  
6. **引入周期保洁**：至少每周扫一次 stale/conflict/drift。  
7. **保留人类最终控制权**：全局规则、ADR、验收标准、runbook、组织知识的合并必须 human-in-the-loop。  

如果要把这一套框架压缩成一句团队原则，那就是：

**把“用户行为”写进 spec，把“方案结构”写进 design，把“为什么这样选”写进 ADR，把“局部维护真相”写进模块 README，把“高频且稳定的 Agent 约束”写进规则，把“可重演的问题”写进测试或 runbook，把“未证实的推断”留在临时上下文，并给所有长期知识附上证据、责任人与过期机制。**

**开放问题与局限**：本报告中的评分阈值、TTL 建议和路由规则是基于官方实践与研究证据综合出的治理框架，不是任何单一厂商的标准；其中 Cursor 的细节公开格式较偏产品文档型，本文主要引用其已公开的规则分层说明；另外，随着 AI coding products 的指令加载逻辑与上下文预算继续演化，团队应把这些阈值视为**可版本化治理配置**，而不是一次写死的制度。citeturn24search0turn23view0turn21view2