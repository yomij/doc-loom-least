# Doc Loom Least

[English](README.md)

> 一套极简的文档驱动 AI 编程工作流——没有 CLI 后端，没有重型流水线，只有 Skills 和 Markdown。

Doc Loom Least 帮助你和 AI 编程助手在任何任务中始终对齐三件事：

- **当前应该相信哪些事实？**
- **应该创建或更新哪些文档？**
- **哪些决策必须先经过你确认？**

它用最小的一组 Agent Skills 和轻量 Markdown 产物来实现这一切——仅此而已。

## 为什么需要 Doc Loom Least？

AI 编程助手强大但健忘。它们会偏离上下文、跳过风险变更的确认、不留任何记录说明发生了什么以及为什么。

Doc Loom Least 用尽可能小的机制解决这个问题：一组在关键节点（计划、执行、收尾）进行把关的 Skills，以及一份跨会话保持事实、案例状态和产物一致性的共享协议。

它**不是**一个平台、CLI 工具或流水线产品。它是一个以 Skills 形式安装、在对话中调用的工作流。

## 核心原则

以下原则来自[宪法文档](docs/adr/ADR-0000-constitution.md)，不可违反：

| 原则 | 含义 |
|---|---|
| **走最小路径** | 用最窄的 contract、artifact 或 skill 解决真正的治理问题。不为仪式感而仪式感。 |
| **不变成复杂流水线产品** | Doc Loom Least 是工作流，不是产品。没有 CLI 后端，没有重型编排。 |
| **人类语义优先** | 每个文档、prompt 和输出都必须可读、可理解、优美。意义优先于机制。 |

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
| `docloom-workflow` | 入口与轻量路由——解析任务状态，路由到对应阶段 Skill |
| `setup-doc-governance` | 文档治理初始化与维护——扫描文档、抽取事实、生成治理计划 |
| `context-authority` | 事实权威把关——读取最小必要上下文，解决冲突，输出路由裁决 |
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
│   ├── adr/                   # 宪法与架构决策记录
│   ├── design/                # Doc Loom Least 及各 Skill 的设计文档
│   └── reference/             # 外部/历史参考材料（非当前权威）
└── skills/
    ├── _shared/               # 跨 Skill 共享协议
    ├── docloom-workflow/      # 入口与路由
    ├── setup-doc-governance/  # 治理初始化与重建
    ├── context-authority/     # 上下文与权威来源判断
    ├── plan-confirm/          # 计划生成与确认
    ├── tdd-execute/           # TDD 执行
    ├── doc-sync-close/        # 文档同步与 Case 收尾
    ├── review/                # 手动审查
    └── grill/                 # 手动压力测试
```

## 典型使用路径

### 一次性文档修改（无需创建 Case）

低风险的一次性文档变更——读取[宪法文档](docs/adr/ADR-0000-constitution.md)，然后直接修改目标文档。不需要创建 Case、不需要计划、不需要执行记录。

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

在不使用 `skillshare` 但支持 Agent Skills 的环境中，将 `skills/` 下需要的目录复制或链接到对应 Skills 目录后，按名称调用：

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

`README.md` 是项目入口和导航文档，不是最高权威。若它与宪法、Active 权威、当前 Skill 或设计文档冲突，应修正 README，而不是用 README 覆盖上游事实。

## 许可证

[MIT](LICENSE)
