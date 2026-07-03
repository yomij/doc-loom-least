# Doc Loom Least

[English](README.md)

> 一套极简的文档驱动个人产品工作流承接层。当前切片是 AI 辅助开发流：没有 CLI 后端，没有重型流水线，只有 Skills 和 Markdown。

Doc Loom Least 帮助你和 AI 助手在工作流中始终对齐三件事：

- **当前应该相信哪些事实？**
- **应该创建或更新哪些文档？**
- **哪些决策必须先经过你确认？**

它用最小的一组 Agent Skills 和轻量 Markdown 产物来实现这一切——仅此而已。

## 为什么需要 Doc Loom Least？

AI 助手强大但健忘。在开发任务中，它们会偏离上下文、跳过风险变更的确认、不留任何记录说明发生了什么以及为什么。

Doc Loom Least 当前先用尽可能小的机制解决开发流问题：一组在关键节点（计划、执行、收尾）进行把关的 Skills，一份跨会话保持事实、案例状态和产物一致性的共享协议，以及一条受控的下一切片发现路径，用来寻找下一个小的开发切片，同时不绕过确认门。

它的长期方向是承接个人产品工作流，后续可以纳入产品、调研、设计、发布、运营等生命周期内容。这里的“平台”指 repo-native、skill-based 的工作流承接层，不指 CLI 工具、守护进程、应用后端或流水线产品。

## 核心原则

以下原则来自[宪法文档](docs/authority/constitution.md)，不可违反：

| 原则 | 含义 |
|---|---|
| **走最小路径** | 用最窄的 contract、artifact 或 skill 解决真正的治理问题。不为仪式感而仪式感。 |
| **不变成复杂流水线产品** | Doc Loom Least 是工作流，不是产品。没有 CLI 后端，没有重型编排。 |
| **人类语义优先** | 每个文档、prompt 和输出都必须可读、可理解、优美。意义优先于机制。 |
| **人类体验优先** | Agent 承担可发现、可执行的工作；只在真实决策、外部事实、明确授权或高风险动作前打断用户。 |

## 生命周期范围

当前唯一明确支持的生命周期领域是**开发流**。未来的产品、调研、设计等领域，只有在出现真实工作流边界时才新增对应 skill。空目录、占位阶段和为了路线图完整而添加的流程都不进入系统。

## v1 明确不做什么

这些边界是刻意保持项目极简的设计决策：

- 不依赖 CLI 后端或守护进程
- 不把入口 Skill 做成重型编排器
- 不自动触发 Review——Review 始终是手动行为
- 不把 `review_risk` 当作 Review 授权

简单文档修改和低风险局部变更走最小路径即可。只有进入持久化 Case 工作流时，才需要创建计划、执行记录和收尾文档。

## Skill 列表

| Skill | 职责 |
|---|---|
| `docloom-workflow` | 入口与轻量路由——解析任务状态，报告 Case 状态，发现下一切片候选，并路由到对应阶段 Skill |
| `setup-doc-governance` | 文档治理初始化与维护——扫描文档、抽取事实、生成治理计划 |
| `context-authority` | 按需事实权威把关——读取最小必要上下文，解决冲突，输出路由裁决 |
| `plan-confirm` | 计划把关——生成含风险等级和 TDD 策略的计划，等待你确认 |
| `tdd-execute` | 执行把关——Red-Green-Refactor 循环并记录证据，或记录 TDD 例外 |
| `doc-sync-close` | 收尾把关——同步文档，记录验收结果、风险项和后续事项 |
| `review` | 手动只读审查——审查设计、diff、测试、文档或 Case 证据 |
| `grill` | 手动交互式压力测试——逐问挑战需求、设计或文档主张 |

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
    └── assessment/            # 手动审查与挑战辅助
```

## 典型使用路径

### 一次性文档修改（无需创建 Case）

低风险的一次性文档变更——读取[宪法文档](docs/authority/constitution.md)，然后直接修改目标文档。不需要创建 Case、不需要计划、不需要执行记录。

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

### 持久化开发 Case

需要计划、执行、验收和收尾记录的开发任务：

```
docloom-workflow
  → context-authority（需要上下文把关时）
  → plan-confirm
  → 你确认计划
  → tdd-execute
  → doc-sync-close
```

`docloom-workflow` 只做路由，不替代任何阶段 Skill。

### Case 状态与下一切片发现

当你想知道当前进展时，`docloom-workflow` 可以读取派生 Case dashboard 和
相关 Case 产物，然后报告当前阶段、证据、下一步 Skill，以及需要你提供的
输入。

当你问下一步该做什么时，它可以使用
[`docs/product/current-state.md`](docs/product/current-state.md)、Case 后续项和
有针对性的仓库证据，推荐排序后的下一切片候选。推荐不等于执行授权：你选择
候选后，工作仍然必须经过正常的 Case 身份、计划、批准、执行和收尾流程。

### 手动审查与压力测试

`review` 和 `grill` 仅在你明确要求时使用：

- `review`——只读评估，输出 findings 和 evidence gaps。不写文件、不改状态。
- `grill`——逐问挑战当前主张。不生成产物、不进入工作流路由。

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

在不使用 `skillshare` 但支持 Agent Skills 的环境中，将分组后的 `skills/` 树中包含 `SKILL.md` 的目标目录复制或链接到对应 Skills 目录后，按名称调用：

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
