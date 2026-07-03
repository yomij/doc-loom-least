# Doc Loom Least

[English](README.md)

> 一个极简的、文档驱动的个人产品工作流承接层。当前切片是 AI 辅助开发流：没有 CLI 后端，没有重型流水线，只有 Skills 和 Markdown。

Doc Loom Least 帮你和 AI 助手在工作流里始终对齐三件事：

- **现在该相信哪些事实？**
- **该创建或更新哪些文档？**
- **哪些决策需要你先拍板？**

做到这一点，靠的是一组 Agent Skills 和几份 Markdown 产物。

第一次看这个项目，先读 [START_HERE.md](START_HERE.md)。它把工作流压成三条路：
一次性小改、Fast-Path Case、完整 Case 工作流。

## 架构与使用流程

下面两张图概览当前 repo-native 架构和日常使用路径。README 中直接嵌入动态 GIF，并在每张图下方提供静态 PNG 预览。

### 系统架构

![Doc Loom Least 系统架构](docs/share/diagrams/doc-loom-architecture-cn.gif)

[PNG 预览](docs/share/diagrams/doc-loom-architecture-cn.png)

### 使用流程

![Doc Loom Least 使用流程](docs/share/diagrams/doc-loom-usage-loop-cn.gif)

[PNG 预览](docs/share/diagrams/doc-loom-usage-loop-cn.png)

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
- 不自动触发 Review，Review 始终是手动行为
- 不把 `review_risk` 当作 Review 授权

简单文档修改和低风险局部变更走最小路径即可。只有进入持久化 Case 工作流时，才需要创建计划、执行记录和收尾文档。

## Skill 列表

| Skill | 职责 |
|---|---|
| `docloom-workflow` | 入口与轻量路由。解析任务状态，报告 Case 状态，发现下一切片候选，并路由到对应阶段 Skill |
| `setup-doc-governance` | 文档治理的初始化与维护。扫描文档、抽取事实、生成治理计划 |
| `context-authority` | 按需的事实权威把关。读取最小必要上下文，解决冲突，输出路由裁决 |
| `plan-confirm` | 计划把关。生成带风险等级和 TDD 策略的计划，等你确认 |
| `tdd-execute` | 执行把关。走 Red-Green-Refactor 循环并记录证据，或记录 TDD 例外 |
| `doc-sync-close` | 收尾把关。同步文档，记录验收结果、风险项和后续事项 |
| `review` | 手动只读审查。审查设计、diff、测试、文档或 Case 证据 |
| `grill` | 手动交互式压力测试。逐问挑战需求、设计或文档主张 |

## 仓库结构

```
.
├── START_HERE.md              # 面向用户的短入口
├── INSTALL.md                 # 安装指南
├── CHANGELOG.md               # 版本历史
├── scripts/                   # 本地静态验证脚本
├── fixtures/                  # 协议回归场景
├── examples/                  # 最小项目示例
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

### 第一次使用

先读 [START_HERE.md](START_HERE.md)，然后运行：

```bash
scripts/verify
scripts/verify-fixtures
```

### 一次性文档修改（无需创建 Case）

低风险的一次性文档变更：先读[宪法文档](docs/authority/constitution.md)，再直接改目标文档就行。不用建 Case，不用写计划，也不用留执行记录。

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

想知道当前进展时，可以让 `docloom-workflow` 读取派生 Case dashboard 和相关 Case 产物，然后报告当前阶段、已有证据、下一步该用哪个 Skill，以及需要你补什么输入。

问“下一步做什么”时，它会结合 [`docs/product/current-state.md`](docs/product/current-state.md)、Case 后续项和有针对性的仓库证据，给出排序后的下一切片候选。注意，推荐不等于执行授权：你选定一个候选后，仍然要走完整的 Case 身份、计划、批准、执行和收尾流程。

### 手动审查与压力测试

`review` 和 `grill` 只在你明确要求时才用：

- `review`：只读评估，输出 findings 和 evidence gaps，不写文件、不改状态。
- `grill`：逐问挑战当前主张，不生成产物、不进入工作流路由。

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

如果只是想先评估，不需要安装；直接看
[examples/minimal-project](examples/minimal-project/)。

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
