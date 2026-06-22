# Doc Loom Least

Doc Loom Least 是一套文档驱动的 AI Coding 工作流。

它的目标不是提供复杂平台、CLI 后端或重型流水线，而是用最小的一组 Agent Skills 和 Markdown 产物，帮助 AI Coding 任务在开始、计划、执行和收尾时明确三件事：

- 当前应该相信哪些事实。
- 当前应该生成或更新哪些文档。
- 哪些决策必须先让用户确认。

## 核心原则

本仓库的最高规则是 [ADR-0000 Constitution](docs/adr/ADR-0000-constitution.md)。

- **最小路径**：用最窄的 contract、artifact、checker 或 skill 解决真实治理问题。
- **不要变成复杂流水线产品**：Doc Loom Least v1 是文档和 skill workflow，不依赖 CLI backend。
- **人类语义优先**：文档、workflow、prompt 和解释必须清晰、可读、可理解。
- **历史文档是证据，不是默认权威**：未确认的历史材料不能直接变成当前事实。

## 当前边界

Doc Loom Least v1 明确不做这些事：

- 不依赖 CLI backend。
- 不调用 `.agents/doc-loom/bin/doc-loom`。
- 不创建或维护 `.agents/doc-loom` control files。
- 不把入口 skill 做成重型 orchestrator。
- 不自动触发 review，也不把 `review_risk` 当作 review 授权。

普通一次性文档修改和低风险局部修改可以走最小路径；只有进入持久化 case workflow 时，才需要创建 case docs、计划、执行记录和 closure。

## 仓库结构

```text
.
├── AGENTS.md                         # 本仓库的 Agent 工作约束
├── docs/
│   ├── adr/                          # 根本原则和决策记录
│   ├── design/                       # Doc Loom Least 与各 skill 的设计文档
│   └── reference/                    # 外部/历史参考材料，默认不是当前权威
└── skills/
    ├── _shared/                      # 跨 skill 共享协议
    ├── docloom-workflow/             # 入口与轻量路由
    ├── setup-doc-governance/         # 文档治理初始化与重建
    ├── context-authority/            # 上下文与权威来源判断
    ├── plan-confirm/                 # 计划生成与用户确认
    ├── tdd-execute/                  # 已确认计划的 TDD 执行
    ├── doc-sync-close/               # 文档同步与 case 收尾
    ├── review/                       # 手动触发的只读证据审查
    └── grill/                        # 手动触发的交互式压力测试
```

## Skill 列表

| Skill | 类型 | 作用 |
|---|---|---|
| `docloom-workflow` | 入口 / 轻量路由 | 解析当前任务和 case 状态，延迟创建 case，路由到具体阶段 skill。 |
| `setup-doc-governance` | 治理初始化 / 周期治理 | 扫描文档，抽取事实，生成 `GOVERNANCE_PLAN.md`，经确认后整合、桥接、归档或更新 authority。 |
| `context-authority` | 主流程 | 在计划前读取最小必要上下文，判断事实来源、冲突和路由 verdict。 |
| `plan-confirm` | 主流程 | 生成带风险等级、TDD 策略、版本和基线的计划，并等待用户确认。 |
| `tdd-execute` | 主流程 | 按已确认计划执行 Red-Green-Refactor 或记录 TDD exception 与替代验证。 |
| `doc-sync-close` | 主流程 | 同步任务文档，记录验收、风险、后续项和 closure status。 |
| `review` | 手动辅助 | 用户明确要求时，只读审查设计、diff、测试、文档或 case evidence。 |
| `grill` | 手动辅助 | 用户明确要求时，通过对话压力测试需求、方案、架构或文档主张。 |

## 典型使用路径

### 一次性低风险文档任务

读取 `AGENTS.md` 和 `docs/adr/ADR-0000-constitution.md`，再按最小路径修改目标文档。

这类任务通常不需要创建 case、不需要 `plan.md`，也不需要 `execution.md`。

### 文档治理

当需要整理、重建、归档或修复文档体系时，使用：

```text
setup-doc-governance
  -> 选择 scope: current-case | docs-only | full-repo
  -> 盘点文档和入口
  -> 抽取事实与证据
  -> 生成 GOVERNANCE_PLAN.md
  -> 用户确认
  -> 执行非 blocked 的治理决策
```

默认 scope 是 `docs-only`。只有用户明确要求，或 authority claim 需要代码 / 测试证据时，才升级到 `full-repo`。

### 持久化开发 case

需要计划、执行、验收和收尾记录的开发任务使用：

```text
docloom-workflow
  -> context-authority（需要上下文 gate 时）
  -> plan-confirm
  -> 用户确认计划
  -> tdd-execute
  -> doc-sync-close
```

`docloom-workflow` 只做入口和路由，不替代阶段 skill。`plan-confirm` 负责计划与确认，`tdd-execute` 负责执行证据，`doc-sync-close` 负责收尾。

### 手动审查和压力测试

`review` 和 `grill` 只在用户明确要求时使用：

- `review` 输出 assessment、findings 和 evidence gaps，不写文件、不改状态。
- `grill` 逐问挑战当前主张，不生成 artifact、不进入 workflow route。

## 使用方式

本仓库提供的是 skill 源文件，不是可执行程序。每个 skill 目录都包含一个 `SKILL.md`，并可包含 `templates/`、`references/` 和 `agents/`。

推荐使用 `skillshare` 安装和更新：

```bash
skillshare install github.com/yomij/doc-loom-least --track --json
skillshare sync
```

私有仓库可使用 SSH：

```bash
skillshare install git@github.com:yomij/doc-loom-least.git --track --json
skillshare sync
```

更新：

```bash
skillshare check
skillshare update --all --diff
skillshare sync
```

更完整的安装、项目级安装和验证说明见 [INSTALL.md](INSTALL.md)。

在支持 Agent Skills 但不使用 `skillshare` 的环境中，也可以将 `skills/` 下需要的目录复制或链接到对应的 skills 目录后，通过 skill 名称调用。例如：

```text
用 docloom-workflow 继续当前 case。
用 setup-doc-governance 整理 docs-only 范围的文档治理。
用 review 审查这个计划。
用 grill 压力测试这个 API 设计。
```

本仓库当前没有运行时依赖安装步骤。若后续任务需要安装依赖，请遵守 `AGENTS.md`：

- JavaScript / Node 依赖优先使用 `pnpm`。
- Python 包管理和虚拟环境始终使用 `uv`。

## 文档维护规则

维护本仓库文档时，先读取 [ADR-0000 Constitution](docs/adr/ADR-0000-constitution.md)。

默认事实权威顺序：

1. Active authority docs。
2. Current production code。
3. Current tests。
4. Accepted ADR、migration 或 release note。
5. 当前用户提供的新信息，作为 pending fact。
6. L2 operational case docs。
7. L3 derived 或 index docs。
8. L4 archive 或 historical docs。
9. L5 scratch docs。

`README.md` 是项目入口和导航文档，不是最高权威。若 README 与 constitution、active authority、当前 skill 或设计文档冲突，应修正 README，而不是用 README 覆盖上游事实。

## 许可证

本仓库当前未声明许可证。
