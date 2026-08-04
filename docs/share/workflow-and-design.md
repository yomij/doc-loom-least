# 文档驱动的个人产品工作流：当前开发流

> 一套用 Skills + Markdown 搭起来的最小工作流，让 AI agent 和你始终对齐三件事：
> **该信什么事实、该改什么文档、哪些决策要先确认。**

这不是某一个项目的使用说明，而是一套可以独立理解、迁移到任何仓库的工作流与设计理念。当前实现只聚焦开发流；长期上，它可以作为个人产品工作流的承接层，继续接入产品、调研、设计、发布、运营等生命周期内容。

这里的“承接层”仍然是 repo-native、skill-based、Markdown-first 的轻量系统：不是 CLI 后端，不是应用平台，也不是重型流水线。下面讲清楚它在开发流中的原理、原则和角色划分——你可以照着这套理念设计自己的实现，也可以直接复用下文描述的 skill 角色。

---

## 1. 要解决什么问题

AI agent 很强，但在开发任务中有三类通病，几乎人人都踩过：

- **上下文漂移**：聊着聊着就忘了当前事实到底是什么，把过时的旧文档当成权威照着改。
- **跳过确认**：面对高风险改动（权限、计费、数据删除、public API）直接动手，没有人为 gate。
- **无痕迹**：做完了不留记录，下次接续任务时谁也说不清"上一步到哪、为什么这么干"。

这套工作流不打算用重型平台解决这三件事。它的开发流定位很明确：

> **不是 CLI，不是流水线产品，而是一组在对话里就能驱动的角色（skill）+ 轻量 Markdown 产物。**

机制越小越好——先判断是否需要持久 Case，再判断是否需要 guarded
assurance；只在真实触发的计划、审查和收尾边界设 gate。

---

## 2. 设计理念

四条宪法原则，不可违反：

| 原则 | 含义 |
|---|---|
| **从最小路径进入** | 用最窄的契约、产物或 skill 解决真实问题，不为仪式感加流程 |
| **不成为复杂流水线** | 它是工作流不是产品，没有 CLI 后端、没有重型编排 |
| **Human-Semantic 设计** | 每份文档、prompt、输出都要可读、易懂、美观，意义优先于机械 |
| **Human Experience Comes First** | Agent 承担可发现、可执行的工作，只在真实决策或高风险边界打断用户 |

从这四条延伸出几条贯穿全局的态度，理解了它们就理解了所有具体规则为什么这么定：

- **历史文档是证据，不是权威**。旧需求、旧设计、会议记录默认不能当当前事实用——它们是抽取事实的原材料。
- **过期文档比缺失文档更危险**。一份看着像权威、实则已与代码脱节的文档，会把 agent 引向错误实现；没有文档时 agent 至少会去读代码或发问。所以治理的目标是**可信知识密度**，不是文档数量——过期的就该降级或归档。
- **确认 gate 保护真实边界**。guarded、高风险、public contract、workflow/agent policy 等边界必须显式确认；medium 风险本身不建 case，也不增加确认。
- **TDD 是默认纪律，但允许记录化例外**。纯文档、配置、spike、hotfix 不会被流程卡死，但必须记录替代验证方式。
- **不比它所治理的改动更大**。Case 只为连续性、持久决策、显式请求或 guarded 执行存在；artifact、Review 和 commit 都由各自触发条件决定。
- **生命周期扩展按真实边界发生**。未来的产品、调研、设计等领域，只有在出现清晰工作流和 skill 边界时才进入系统，不提前创建占位阶段。

```mermaid
flowchart LR
    P1["从最小路径进入"] --> C1["Direct / Compact / Guarded<br/>按后果增加保证"]
    P2["不成为复杂流水线"] --> C2["thin router 不做 orchestrator<br/>无 CLI backend"]
    P3["Human-Semantic"] --> C3["Markdown 为人类主记录<br/>yaml 只是路由信号"]
    P1 & P3 --> C4["历史文档=证据<br/>authority=治理后权威"]
    P2 & P3 --> C5["TDD 默认+记录化例外<br/>确认 gate 不放宽"]
```

---

## 3. 为什么是"文档驱动"

很多 agent 工作流把状态放在 CLI 后端、数据库或不可见的 pipeline 里。这套工作流反过来——**文档既是事实来源，也是任务状态，也是过程证据**。这是它最根本的范式选择。

### 3.1 文档即一切

持久 Case 闭环的每个环节都落在 Markdown 上；Direct 工作不为此创建
Case artifact：

| 环节 | 驱动它的文档 |
|---|---|
| 当前进度是什么 | 持久 case 的 `plan.md` / 条件化 `execution.md` / `closure.md` |
| 事实该信什么 | L1 authority 文档 + 代码 + 测试 |
| 这次要做什么 | `plan.md`（带版本、风险、TDD 策略） |
| 做了什么、偏没偏离 | 触发时用 `execution.md`；否则 closure 保留精简验收/测试/diff/scope 证据 |
| 结果如何 | `closure.md`（最终状态、残留风险、后续） |
| 下次怎么接 | `handoff.md`（仅在有恢复点时） |

agent 进入一个 workspace，不需要查询任何外部系统——读最小一组文档 + git 状态，就能推导出当前阶段、下一步、已知问题。**状态是从文档推导出来的，不是被某个中心服务持有的。**

### 3.2 为什么选文档当载体

- **人能直接读改**：Markdown 是人类可读主记录。你不需要任何工具就能审阅计划、修正事实、补全证据——agent 写的东西你看得懂、改得动。这正是 Human-Semantic 原则的落点。
- **跨会话/跨 agent 可续**：会话会断、context 会丢，但文档留在仓库里。下一个会话、另一个 agent、另一个同事，读同一份 `handoff.md` + `plan.md` 就能接上。
- **事实可追溯**：每个权威事实都有 `source_of_truth`（user_confirmed / code / tests / adr…）和证据来源。改动为什么发生、依据是什么，都在文档里。
- **零基础设施**：不需要装 CLI、跑 daemon、维护数据库。skill + Markdown，到此为止——这是"不成为复杂流水线"原则的直接体现。

### 3.3 一条关键边界

> **Markdown artifact 同时承载人类可读证据和当前状态。**

`plan.md`、条件生成的 `execution.md`、`closure.md` 各自只拥有本阶段的状态；agent 按固定优先级读取最新可靠 artifact，并检查计划声明的完成证据。没有需要修复或同步的第二份状态缓存。

文档驱动不等于"什么都写文档"。Artifact Policy 规定产物**按需生成**：`execution.md` 只在恢复、实质偏离、重要失败/重试、深度 Review finding/证据或显式请求时写，`handoff.md` 只在存在未来恢复点时写。普通行为变化或一次正常 TDD 循环本身不强制落盘。

### 3.4 两个容易踩的坑

这两个坑来自实践中反复出现的失败模式，值得单独点出：

**坑一：以为"更长的 context window = 更好的上下文"。** 把整个仓库塞进 prompt 是本能反应，但相关研究（context rot、Lost in the Middle）表明，关键信息放在长上下文中间时模型利用率显著下降。解法不是堆 context，而是**先路由、再裁剪**：上下文 gate 只读与当前任务相关的最小 artifact。少而准，胜过多而杂。

**坑二：把经验沉淀在个人机器上。** Claude Code 的 auto memory、各 IDE 的本地 memory 都是 machine-local——你用着很顺，但同事的 agent 读不到，换台机器也丢了。这正是把治理产物写进仓库（而非本地 memory）的理由：**只有进 repo 的文档才是组织资产**，能被任何人、任何 agent 在任何机器上可靠复用。

---

## 4. 角色划分（Skill 职责）

整套开发流由若干角色化的 skill 组成，分三类——入口路由、主流程 gate、只读审查/手动挑战辅助。每个 skill 的职责是工作流的逻辑切分，你可以用任何方式实现它们（自定义 prompt、agent skill、或者干脆人工对照执行）。

在仓库中，这些 skill 按物理目录分组：`skills/development/` 承载当前开发流，`skills/governance/` 承载文档治理，`skills/assessment/` 承载只读审查和手动挑战辅助，`skills/_shared/` 承载共享协议。

| Skill | 角色 |
|---|---|
| `docloom-workflow` | **入口 + 轻量路由器**：解析状态、延迟创建 case、持有 Artifact Policy |
| `setup-doc-governance` | 文档治理：扫描抽取事实，生成治理计划，一次确认后执行 |
| `context-authority` | 按需上下文 gate：读最小上下文、判权威、输出路由 verdict |
| `plan-confirm` | 计划 gate：定义 outcome envelope、风险/TDD 和条件化 Review/提交策略 |
| `tdd-execute` | Case 执行 gate：红绿重构或例外验证 + 按需证据 + 触发后的 Review/修复循环 |
| `doc-sync-close` | 收尾 gate：同步文档、记录最终证据、按计划声明创建 completion commit |
| `review` | 只读审查：临时审查由用户触发；满足 guarded-review 条件时由执行流调用 Post-execution 模式 |
| `grill` | 手动交互拷问（一次一问，纯对话） |

关键设计抉择：**`docloom-workflow` 只路由，不替代任何阶段 skill。** 它读最小 git 状态、推导 case、判定该进哪个阶段，但绝不自己生成计划或执行代码。这样它才能保持"thin"，不会退化成一个无所不能的 orchestrator——那正是"不成为复杂流水线"原则要避免的。

临时 `review` 和 `grill` 被排除在 workflow 路由之外，只在用户明确点名时触发。guarded、实质偏离、弱验证、public/authority-sensitive 或显式要求的工作，由 `tdd-execute` 调用 `review` 的只读 Post-execution 模式；`review` 自身仍不写状态或产物，因此无需增加新阶段或 `review.md`。

```mermaid
flowchart TD
    User([用户意图]) --> Entry{docloom-workflow<br/>thin router}
    Entry -->|治理/重建| GOV[setup-doc-governance]
    Entry -->|需要上下文 gate| CTX[context-authority]
    Entry -->|direct| DIRECT[正常执行/验证/报告<br/>不建 case]
    Entry -->|compact / guarded| PLAN[plan-confirm]
    PLAN --> TDD[tdd-execute]
    TDD -->|guarded-review trigger| POST[review<br/>Post-execution]
    TDD -->|无 trigger| CLOSE[doc-sync-close]
    POST -->|pass| CLOSE
    POST -->|重要问题| TDD
    Entry -.临时只读审查.-> ADHOC[review<br/>ad-hoc]
    Entry -.交互拷问.-> GRILL[grill]

    classDef manual stroke-dasharray: 5 5,fill:#f9f,stroke:#333
    class ADHOC,GRILL manual
```

> 虚线 = 手动触发，不进入主流程路由。

---

## 5. 核心工作流

### 5.1 两个判断，三种执行结果

流程先回答两个互不替代的问题：是否因连续性、持久决策、显式 case 或
guarded execution 需要 case；是否因后果、不可逆性、暴露、authority 影响或
弱验证需要 guarded assurance。

```mermaid
flowchart LR
    A[用户任务] --> C{需要持久 case?}
    C -->|否| DIRECT[Direct<br/>正常执行/验证/报告]
    C -->|是| G{需要 guarded assurance?}
    G -->|否| COMPACT[Compact persistent<br/>精简 plan + closure]
    G -->|是| GUARDED[Guarded<br/>显式确认 + 精确基线深审 + 持久证据]
    COMPACT --> EXEC[执行<br/>artifact/review/commit 按触发生成]
    GUARDED --> EXEC
```

这三种是同一套现有 stage 的执行结果，不是新 stage 或 risk level。Medium
风险本身不建 case，也不要求重复确认。compact persistent 计划可记录当前
明确的“执行”请求；guarded 计划必须展示书面当前对象并获得显式确认。

### 5.2 计划确认绑定

`plan-confirm` 绑定的是 outcome envelope，而不是冻结实现清单：

- Goal
- Guardrails / Non-goals
- Acceptance Criteria
- Escalation Triggers
- 明确保护的特殊边界

`risk_level`、`plan_version` 和 `base_commit` 继续提供追溯。文件清单、补充测试和
普通实现选择是计划证据，可在 envelope 内自适应，不触发升版。只有 outcome/
non-goal/acceptance 变化、风险升级、authority/public contract 变化、依赖/
lockfile/CI/schema/config effect、外部资源或不可逆动作、其他受保护边界变化时，
才停止并重新确认。

### 5.3 TDD 执行与例外

默认走 Red → Green → Refactor → Quality Check。但 TDD 不是教条——以下任务可记录例外：纯文档、配置、构建脚本、UI 文案、删除死代码、探索 spike、紧急 hotfix。

例外 ≠ 跳过验证，必须记录替代验证方式（manual / snapshot / build / smoke / reviewer）。无行为变化的重构不强行造失败测试，改用 characterization 锁定现有行为 → 重构 → 验证无回归。

guarded、实质偏离、弱验证、public/authority-sensitive 或显式要求的工作，
必须进行两次相互独立的只读检查。Engineering 关注正确性、错误路径、测试
可信度、标准/契约、高风险项和不必要复杂度；Spec 对照 outcome envelope、
确认决策和权威检查遗漏、错误实现和 scope creep。一次 Review 返回当前完整
finding set；执行按独立有效性和可回滚性组成最小 coherent batch，验证后再
复审受影响轴。finding 数量不决定 commit 数量，缺少关键证据不能当作通过。

其他工作由执行者完成精简的 acceptance/test/diff/scope check。提交遵循用户/
项目意图和语义价值；普通 case 不强制独立 plan/closure bookkeeping commit。
只有计划明确声明的提交才是完成门禁。授权始终不包含无关文件、push、发布、
amend/rebase/squash 或 escalation trigger。

### 5.4 收尾与状态

`doc-sync-close` 把执行结果回写文档体系，最终状态必须是以下之一：

| 状态 | 含义 |
|---|---|
| `Done` | 验收标准满足；触发的 Review 通过；closure 证据完整；声明的提交成功 |
| `Done with Caveats` | 主目标完成，有明确残留风险 |
| `Blocked` | 依赖/权限/信息不足，无法继续 |
| `Cancelled` / `Superseded` | 取消 / 被另一方案替代 |
| `Paused` / `Abandoned` | 暂停待恢复 / 长期未续 |

验收未满足、触发的双轴 Review 未通过、closure 证据不完整，或计划声明的提交
未成功，都不能报告无保留 `Done`。没有声明 completion commit 时，完整
closure artifact 本身即为终态证据。Authority 文档更新默认只生成 proposal，
必须用户明确确认后才执行窄 patch；结构性、高风险或冲突型更新走治理计划。

### 5.5 case 状态机

case 的当前阶段由 Markdown artifact + git 状态直接推导。终止 closure 优先；`Paused`、`Blocked`、`Done with Caveats` 在明确恢复后，由带有更新 `updated_at` 和 Resume 证据的 `execution.md` 重新取得当前状态：

```mermaid
stateDiagram-v2
    [*] --> Draft: plan.md status draft
    Draft --> Approved: 用户确认
    Approved --> Executing: 触发 execution.md 时
    Approved --> Final: 无 execution trigger，直接 closure
    Executing --> ReadyToClose: 触发的完成 gate 通过
    ReadyToClose --> Final: closure.md + 计划声明的完成证据
    Executing --> Paused: closure.md status Paused
    Executing --> Blocked: closure.md status Blocked
    Final --> [*]
    Paused --> Executing: 明确恢复 + 新 execution Resume 证据
    Blocked --> Executing: 条件满足 + 新 execution Resume 证据
```

### 5.6 为什么不用数字捷径

文件数和行数不能稳定代表后果、连续性或验证强度；一个两行权限改动可能需要
guarded assurance，一个跨多个内部文件的机械修改也可能是 direct。流程不再用
数字阈值判断“够不够简单”，而是直接判断是否需要持久性与 guarded assurance。

真实 Git 历史也显示，问题不在“提交太多”本身，而在提交形状：27 个带标准
trailer 的样本 Case 共 117 个提交，其中 plan/closure 占 65 个（55.6%），
55 个提交只改 `docs/cases/*.md`；17 个 review-fix 则没有纯 Case 文档提交，
约 94% 的 churn 在 Case 文档之外。因此优化目标是让 commit 表示可独立验证、
审查和回滚的交付单元，而不是工作流状态跳转。55.6% 是历史样本的理论压缩
上限，不是新的 KPI 或门槛；guarded evidence 仍然允许承担必要成本。

---

## 6. 文档治理：把历史文档变成权威体系

这是整套工作流最有特色的部分。

### 6.1 核心理念

`setup-doc-governance` 的目标**不是给旧文档加 metadata、在旧目录上修修补补**，而是：

> 从历史材料中抽取仍有效的事实 → 按固定分层标准给文件和事实下 verdict → 生成治理计划 → 用户一次确认后执行整合、迁移、归档和按需重建 authority。

一句话：**历史文档是证据库，标准权威文档体系是治理后的结果。**

### 6.2 五层文档模型

| 层级 | 角色 | 规则 |
|---|---|---|
| **L1 Authority** | 当前必须遵守的事实/规范/决策 | 更新必须用户确认，与代码冲突要显式报告 |
| **L2 Operational** | 某次任务的计划/执行/收尾记录 | 可自动创建更新，不直接当长期规范 |
| **L3 Derived** | 从 authority + 代码派生的易读文档 | 机械可追溯更新可自动，与 authority 冲突以 authority 为准 |
| **L4 Historical** | 不代表当前事实的历史记录 | 默认不能当依据，引用须标记 historical |
| **L5 Scratch** | 草稿/实验记录 | 默认不可信，可定期清理 |

分层的根本动机不是"按主题分目录"，而是**按消费对象和稳定性切分**：同一份知识要同时服务人（可读可信）、工程系统（可校验可版本化）、agent（最小相关已验证）、治理（有 owner、有状态、有时效）。分错层比不分层更糟。判断一篇文档放错了层，看反模式比看正面定义更直接：

- L1 不该出现"agent 推断但未经用户确认"的内容。
- L4 历史文档不该仍被当作当前依据引用而不打标记。
- L5 草稿不该未经治理直接晋升成长期规范。

这也带出一条反直觉但重要的判断：**过期的文档比缺失的文档更危险**。一份写着旧接口、旧行为的"看起来还权威"的文档，会误导 agent 生成错误代码；而缺失文档顶多让 agent 去问。所以治理的目标不是"文档数量多"，而是**可信知识的密度**——过期内容要么更新、要么标记为 historical 降权，绝不能默认还留在 AI 的可消费范围里。

### 6.3 治理流程

单一固定流程，通过 `scope`（`current-case` / `docs-only` / `full-repo`）控制扫描范围，默认 `docs-only`。

```mermaid
flowchart TD
    S[选择 scope] --> I[盘点文档/入口/历史材料]
    I --> F[抽取事实并按分层标准路由]
    F --> V[给文件和事实下 verdict]
    V --> P[生成治理计划]
    P --> CONF{{🛑 用户一次确认}}
    CONF -->|确认| A[执行非 block 项:<br/>创建 authority/merge/bridge/archive]
    CONF -->|block 项| SKIP[跳过冲突/高风险项]
    A --> R[写回 Applied Result + Blocked Decisions]
```

统一的 verdict 适用于文件级和事实级：

| Verdict | 含义 |
|---|---|
| `promote` | 抽取到新的 authority 文档 |
| `merge` | 合并进已有 authority 文档 |
| `bridge` | 保留薄入口指向新权威，防误读 |
| `archive` | 移入归档，不再当当前事实 |
| `block` | 冲突/高风险/证据不足，等用户裁决 |

安全、权限、认证、计费、数据删除、public API breaking change 等**必须 block**，绝不自动写入 authority。

值得强调的是治理在确认前会做两件事：**去重归一**和**冲突检测前置**。同一事实在多份旧文档里有不同表述时，先归一成一条；两份文档对同一主题结论不同时，在生成治理计划阶段就标记冲突（hard conflict 进 block，软重叠只去重），而不是把自相矛盾的内容一起塞进 authority。这样确认对象才是干净的。

治理后的文档体系让后续 agent 能快速回答：当前产品事实是什么？架构事实是什么？哪些只是历史背景？哪些待确认？——这才是治理的真正产出。

---

## 7. 关键机制速览

以下几个机制是支撑上述流程的"暗线"，这里只点名不展开：

- **Fact Authority Order**：判断"当前事实是什么"的优先级（active authority > 生产代码 > 测试 > 已接受 ADR > 用户本轮新信息 > L2…L5）。代码与 authority 冲突必须停下来报告。
- **Execution Instruction Order**：判断"本次任务怎么执行"的优先级，与上面分开，避免把用户临时指令误当长期事实。
- **Artifact Policy**：产物按需生成而非每阶段铺满。`execution.md` 只在恢复、实质偏离、重要失败/重试、深度 Review finding/证据或显式请求时写；`handoff.md` 只在存在未来恢复点时写。
- **Direct vs Case 双上下文**：可逆单轮工作在正常仓库执行中保留验证和最终报告；只有连续性、持久决策、显式请求或 guarded 执行需要 Case。
- **三种运行模式**：isolated（worktree+branch，大/并行/高风险）、branch（普通开发）、inline（小改动，不切分支）。branch/worktree 是推荐机制不是强制前置。
- **Canonical Once, Adapt Everywhere**：多 AI 工具（Claude / Codex / Copilot / Cursor）的指令文件名和作用域各不相同，手写四套必然漂移。正确做法是只维护一份规范源，再编译到各工具的适配层——和 authority 体系同一套逻辑：单一真相源 + 多个派生视图。
- **能机器校验的不要只写 prose**：API 用 OpenAPI、DB 用 migration、规范用 lint、流程用 workflow、AI 行为用 prompt/agent policy。能落到可执行资产的，就不要只留在散文里。

---

## 8. 渐进式落地：小项目怎么起步

不要一上来就铺全套。治理成熟度的衡量标准不是"文档数量多"，而是两条客观轴：**AI 能否稳定拿到正确上下文**、**文档能否随代码同步演化**。升级也是事件驱动而非规模驱动——出现第二位协作者、代码跨模块、事故增多、或开始用持续运行的 agent 时，才需要加层。

最小可行起步只需要六样，且全进 repo：

| 起步产物 | 覆盖哪种最易失效的上下文 |
|---|---|
| `README.md` | 入口与导航 |
| `AGENTS.md` / `CLAUDE.md` | agent 指令 |
| `adr/` | 已接受的技术决策 |
| `contracts/`（API/schema） | 当前契约 |
| `runbooks/` | 操作与回滚 |
| `docs/index.md` | 文档路由 |

有了这套骨架，再按需补 case 闭环（plan/execution/closure）和 authority 子区。小项目用模板启动，等协作者、模块、事故、自动化深度上升再逐层升级——这本身就是"从最小路径进入"原则在落地层面的体现。

---

## 9. 总结

这套工作流可以浓缩为六句话：

> **严格，但不僵硬。**
> **可追溯，但不繁琐。**
> **文档优先，但不脱离代码。**
> **TDD 默认，但允许记录化例外。**
> **确认 gate 保护真实决策边界，比例化 assurance 保护最小路径。**
> **历史文档作为证据，authority 文档作为治理后的权威。**

它不追求覆盖所有场景，而是用最小的机制在关键节点设 gate：让 agent 该停时停、该记时记、该问时问。剩下的事，交给人和 agent 在对话里解决。
