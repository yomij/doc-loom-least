# Doc Loom Least

[English](README.md)

> 一个极简的、文档驱动的个人产品工作流承接层。当前切片是 AI 辅助开发流：没有 CLI 后端，没有重型流水线，只有 Skills 和 Markdown。

Doc Loom Least 帮你和 AI 助手在工作流里始终对齐三件事：

- **现在该相信哪些事实？**
- **该创建或更新哪些文档？**
- **哪些决策需要你先拍板？**

做到这一点，靠的是一组 Agent Skills 和几份 Markdown 产物。

## 架构与使用流程

下面两张图概览当前 repo-native 架构和日常使用路径。README 中直接嵌入动态 GIF，并在每张图下方提供静态 PNG 预览。

### 系统架构

![Doc Loom Least 系统架构](docs/share/diagrams/doc-loom-architecture-cn.gif)

[PNG 预览](docs/share/diagrams/doc-loom-architecture-cn.png)

### 使用流程

![Doc Loom Least 使用流程](docs/share/diagrams/doc-loom-usage-loop-cn.gif)

[PNG 预览](docs/share/diagrams/doc-loom-usage-loop-cn.png)

这张静态图展示的是 guarded case 路径。可逆、单轮的 low/medium 工作直接
执行；compact persistent case 不承担未触发的执行记录、深度审查和提交仪式。

## 为什么需要 Doc Loom Least？

AI 助手能力很强，但容易忘事。做开发时，它们会偏离上下文，跳过风险变更的确认，也不留记录说明做了什么、为什么这么做。

Doc Loom Least 现阶段用尽可能小的机制来解决这些问题：一组在关键节点（计划、执行、收尾）把关的 Skills，一份让事实、案例状态和产物在跨会话间保持一致的共享协议，再加上一条受控的下一切片发现路径，帮你找到下一个该做的小切片，又不绕过确认门。

更远的方向是承接完整的个人产品工作流，后续逐步纳入产品、调研、设计、发布、运营等环节。所谓“平台”，指的是 repo-native、skill-based 的工作流承接层，不是 CLI 工具、守护进程、应用后端或流水线产品。

## 核心原则

以下原则来自[宪法文档](docs/authority/constitution.md)，不可违反：

| 原则 | 含义 |
|---|---|
| **走最小路径** | 用最窄的 contract、artifact 或 skill 解决真正的治理问题。别为了仪式感而加流程。 |
| **不变成复杂流水线产品** | Doc Loom Least 是工作流，不是产品。没有 CLI 后端，没有重型编排。 |
| **人类语义优先** | 每个文档、prompt 和输出都必须可读、可理解、优美。意义优先于机制。 |
| **人类体验优先** | Agent 承担可发现、可执行的工作；只在真实决策、外部事实、明确授权或高风险动作前打断用户。 |

## 生命周期范围

当前唯一明确支持的生命周期领域是**开发流**。未来的产品、调研、设计等领域，只有在出现真实工作流边界时才新增对应 skill。空目录、占位阶段，以及单纯为了让路线图看起来完整的流程，都不进入系统。

## v1 明确不做什么

为了保持项目极简，v1 刻意划了这些边界：

- 不依赖 CLI 后端或守护进程
- 不把入口 Skill 做成重型编排器
- 不自动触发临时 Review，也不把 `review_risk` 当作 Review 授权
- 不新增 Review 阶段或强制 `review.md`；guarded、实质偏离、弱验证、
  public/authority-sensitive 或显式要求的工作，由 `tdd-execute` 在内部
  执行只读 Post-execution Review

流程成本由两个独立问题决定：是否因连续性或持久决策需要 Case，以及是否因
后果或弱验证需要 guarded assurance。可逆、单轮的 low/medium 工作直接
执行；compact persistent 工作只保留精简计划和收尾；guarded 工作才增加
当前计划显式确认、精确基线审查和持久证据。

## Skill 列表

| Skill | 职责 |
|---|---|
| `docloom-workflow` | 入口与轻量路由。解析任务状态，报告 Case 状态，发现下一切片候选，并路由到对应阶段 Skill |
| `setup-doc-governance` | 文档治理的初始化与维护。扫描文档、抽取事实、生成治理计划 |
| `context-authority` | 按需的事实权威把关。读取最小必要上下文，解决冲突，输出路由裁决 |
| `plan-confirm` | 计划把关。定义 outcome envelope、风险、TDD 和条件化 Review/提交策略，在适用的边界记录或请求确认 |
| `tdd-execute` | Case 执行把关。执行 Red-Green-Refactor 或已记录的例外，按需留证，并负责触发后的 Review/修复循环 |
| `doc-sync-close` | 收尾把关。同步文档、记录最终证据，仅在计划声明时创建 completion commit |
| `review` | 只读临时审查，以及工作流内部的 Engineering/Spec Post-execution gate |
| `grill` | 手动交互式压力测试。逐问挑战需求、设计或文档主张 |

## 仓库结构

```
.
├── INSTALL.md                 # 安装指南
├── CHANGELOG.md               # 版本历史
├── docs/
│   ├── index.md               # 文档路由索引
│   ├── ssot-map.md            # 单一事实源地图
│   ├── authority/             # 当前已治理事实，包含宪法
│   ├── adr/                   # 架构决策记录
│   ├── cases/                 # 单个 Case 产物与派生 Case dashboard
│   ├── product/               # 运营级产品状态输入，非权威
│   ├── governance/            # 治理批次计划
│   └── archive/               # 历史与原始证据，非当前权威
└── skills/
    ├── README.md              # Skill 分组地图
    ├── _shared/               # 跨 Skill 共享协议
    ├── development/           # 当前开发流 skills
    ├── governance/            # 文档治理 skills
    └── assessment/            # 只读审查与手动挑战辅助
```

## 典型使用路径

### 一次性可逆工作（无需创建 Case）

可逆、单轮的 low/medium 工作：读取相关项目权威和指令后直接实现、验证并报告即可。不用建 Case，不用写计划，也不用留执行记录。

### 文档治理

当需要整理、重建、归档或修复文档体系时：

```
setup-doc-governance
  → 选择范围: current-case | docs-only | full-repo
  → 盘点文档与入口
  → 抽取事实与证据
  → 生成治理计划
  → 你确认
  → 执行未被阻塞的治理决策
```

默认范围是 `docs-only`。只有当权威声明需要代码或测试证据时，才升级到 `full-repo`。

### 按比例选择开发路径

先判断是否需要持久 Case，再独立判断是否需要 guarded assurance：

```
Direct：可逆、单轮的 low/medium 工作
  → 正常执行 + 验证 + 最终报告；不建 Case

Compact persistent：需要连续性、持久决策或显式 Case
  → 精简 plan → tdd-execute → closure
  → execution.md、深度 Review 和提交按触发条件生成

Guarded：high/public/authority-sensitive/不可逆/弱验证
  → context-authority → plan-confirm → 显式确认当前计划
  → tdd-execute → Engineering + Spec Review
  → 按 coherent batch 修复 findings 并复审
  → doc-sync-close，保留计划声明的持久证据/提交
```

Medium 风险本身不建 Case，也不要求再次确认。批准绑定 Goal、
Guardrails/Non-goals、Acceptance 和 Escalation Triggers；内部文件发现、
补测试和普通实现选择可在这个 envelope 内自适应。

`docloom-workflow` 只做路由，不替代任何阶段 Skill。

### Case 状态与下一切片发现

想知道当前进展时，可以让 `docloom-workflow` 读取派生 Case dashboard 和相关 Case 产物，然后报告当前阶段、已有证据、下一步该用哪个 Skill，以及需要你补什么输入。

问“下一步做什么”时，它会结合 [`docs/product/current-state.md`](docs/product/current-state.md)、Case 后续项和有针对性的仓库证据，给出排序后的下一切片候选。注意，推荐不等于执行授权：选定候选后仍要应用上述两个判断；direct 工作不为候选强制创建 Case。

### 审查与压力测试

临时 `review` 和 `grill` 只在你明确要求时使用：

- `review`：只读评估，输出 findings 和 evidence gaps，不写文件、不改状态。
- `grill`：逐问挑战当前主张，不生成产物、不进入工作流路由。

另外，guarded、实质偏离、弱验证、public/authority-sensitive 或显式要求的
工作会在收尾前运行 `review` 的只读 Post-execution 模式，一次返回当前完整的
Engineering 和 Spec finding set；执行阶段按 coherent batch 修复并复审。

## 安装

Doc Loom Least 以 Agent Skills 形式分发。使用 [`skillshare`](https://github.com/anthropics/skillshare) 安装：

```bash
# 公开仓库
skillshare install github.com/yomij/doc-loom-least --track --json
skillshare sync

# 私有仓库（SSH）
skillshare install git@github.com:yomij/doc-loom-least.git --track --json
skillshare sync
```

更新：

```bash
skillshare check
skillshare update --all --diff
skillshare sync
```

项目级安装和详细验证步骤见 [INSTALL.md](INSTALL.md)。

如果环境支持 Agent Skills 但没有 `skillshare`，可以把分组后的 `skills/` 树里含 `SKILL.md` 的目录复制或软链到你的 Skills 目录，然后按名称调用：

```
用 docloom-workflow 继续当前 case。
用 setup-doc-governance 整理 docs-only 范围的文档治理。
用 review 审查这个计划。
用 grill 压力测试这个 API 设计。
```

## 事实权威顺序

维护本仓库自身文档时，事实按以下顺序解析：

1. Active 权威文档
2. 当前生产代码
3. 当前测试
4. 已接受的 ADR、迁移记录或 Release Note
5. 用户新提供的信息（作为待确认事实）
6. L2 运营级 Case 文档
7. L3 派生或索引文档
8. L4 归档或历史文档
9. L5 草稿文档

`README.md` 是项目入口和导航文档，不是最高权威。若它与宪法、Active 权威或当前 Skill 实现冲突，应修正 README，而不是用 README 覆盖上游事实。

## 许可证

[MIT](LICENSE)
