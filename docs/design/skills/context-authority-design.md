# context-authority Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `context-authority` 的触发、上下文解析、权威判断、输出 brief、边界和验收标准。

---

## 1. 定位

`context-authority` 是计划前的上下文获取与事实权威判断 skill。

它负责建立计划所需的事实基础，但不写实施计划、不改代码、不解决权威冲突。

核心规则：

```text
没有 context，不准计划。
AI 只应消费最小、相关、已验证、未过期的上下文。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: context-authority
description: Gather case context and resolve authority sources before planning. Use at the start of development cases in a document-driven workflow, when resuming a case, when deciding what docs/code/tests are authoritative, or when checking conflicts between user requests, authority docs, code, tests, governance plan, and case docs.
---
```

---

## 3. 触发条件

应该触发：

- 用户提出开发任务，需要进入文档驱动工作流。
- 用户要求继续某个任务。
- 用户要求判断当前上下文、任务状态或文档权威。
- 计划生成前需要读取 authority 文档、代码和测试。
- `plan-confirm` 发现缺少 context brief。

不应该触发：

- 用户要求初始化文档治理。此时使用 `setup-doc-governance`。
- 用户明确要求 review 已完成实现。此时使用 `review`。
- 用户要求任务收尾和文档同步。此时使用 `doc-sync-close`。

---

## 4. 输入

- 用户本轮需求。
- 当前 workspace。
- 当前 branch / worktree。
- `docs/README.md` 或 `docs/DOC_INDEX.md`。
- 可选：`docs/governance/GOVERNANCE_PLAN.md`。
- `docs/authority/`。
- 相关代码和测试。
- 已有 `docs/cases/<case-id>/`。
- 可选：`context/` 下已有 Context Pack manifest。

---

## 5. 输出

默认输出：

```text
docs/cases/<case-id>/context-authority-brief.md
```

如果 case id 尚未建立，可以先在对话中输出 brief，并让 `plan-confirm` 创建 case docs。

模板：

```md
# Context & Authority Brief

## Current case
- Mode:
- Branch:
- Worktree:
- case ID:
- case Docs:
- case State File:
- Governance Scope:
- Governance Plan:
- Blocked Governance Decisions:
- case Intent:
- Intent Reason:

## User Request

## Context Pack Candidate
- Policy Pack:
- Authority Pack:
- Code Pack:
- Test Pack:
- Evidence Pack:
- Excluded:

## Relevant Authority Docs

## Relevant Code

## Relevant Tests

## Existing case Docs

## Current Behavior

## Constraints

## Conflicts
| Topic | User Request | Authority Says | Code Says | Tests Say | Resolution Needed |
|---|---|---|---|---|---|

## Unknowns

## Risks

## Recommended Next Step
```

---

## 6. 推荐资源结构

```text
context-authority/
  SKILL.md
  templates/
    context-authority-brief.md
    context-pack-manifest.yaml
  references/
    shared-protocol.md
    context-resolution.md
    retrieval-routing.md
```

---

## 7. 固定开始动作

逻辑命令：

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
git diff --name-only
```

实现时必须遵守项目本地命令规范。例如当前项目要求 shell 命令加 `rtk` 前缀，则实际命令应为：

```bash
rtk git status --short
```

如果不是 git 仓库：

- 记录 `git_available: false`。
- 使用当前工作目录作为 workspace。
- 继续读取文档和文件。

---

## 8. case Context Resolution Order

按顺序解析：

```text
1. 用户本轮明确指定 case id / case docs / branch / worktree
2. 当前 worktree + case branch 推导 case id
3. 当前 branch 推导 case id
4. 当前目录下已有明确 case docs
5. 当前会话创建新 case id
6. 无法判断时要求用户选择或确认
```

推荐 branch 命名：

```text
case/<case-id>
```

由 branch 推导：

```text
branch: case/20260617-auth-refresh-flow
case_id: 20260617-auth-refresh-flow
case_docs: docs/cases/20260617-auth-refresh-flow/
```

如果多个候选 case docs：

- 优先最近更新且未 closed 的任务。
- 如果仍然歧义，询问用户。

---

## 9. 运行模式

| 模式 | 新 branch | 新 worktree | 适用场景 |
|---|---:|---:|---|
| `isolated` | 是 | 是 | 大任务、并行任务、高风险任务 |
| `branch` | 是 | 否 | 普通开发任务 |
| `inline` | 否 | 否 | 小任务、已有分支、临时任务 |

`context-authority` 只识别模式，不主动创建 branch 或 worktree。

---

## 10. 核心工作流

### Step 0. case Intent Routing

先判断任务类型，再决定读取哪些证据，避免把所有文档塞进上下文。

| 任务类型 | 优先证据 | 默认排除 |
|---|---|---|
| Bug / 报错修复 | 错误码、堆栈、相关 symbol、失败测试、runbook、known issues | 旧设计稿、无关需求 |
| Feature / 新需求 | spec、acceptance criteria、API contract、ADR、相邻实现、测试策略 | 历史会议纪要 |
| Refactor | repo map、dependency graph、ADR、模块 owner、测试覆盖 | 过期 README |
| Review | 当前 diff、plan、execution、测试结果、standards、相关 ADR | 无关领域文档 |
| Incident / 运维 | runbook、监控说明、回滚指南、近期发布、known issues | 产品需求文档 |
| Documentation Governance | `GOVERNANCE_PLAN.md`、authority index、入口索引、case evidence、archive | 业务代码深读，除非用于验证 |

Brief 中必须记录 `case Intent` 和选择该意图的理由。

### Step 1. Resolve Workspace

识别：

- repo root。
- 当前 branch。
- 当前 worktree。
- 工作区未提交变更。
- git 是否可用。

失败不阻塞文档任务，但必须记录。

### Step 2. Resolve case Context

按 case Context Resolution Order 推导：

- `case_id`。
- `case_docs`。
- `case_state.yaml`。
- 当前 phase。

如果 `closure.md` 已存在：

- `Done` / `Cancelled` / `Superseded` / `Abandoned` 默认不恢复。
- `Paused` / `Blocked` 可根据用户请求恢复。

### Step 3. Read Governance Docs

读取：

```text
docs/README.md 或 docs/DOC_INDEX.md
docs/governance/GOVERNANCE_PLAN.md，如存在
docs/authority/**/*.md
```

如果不存在：

- 记录 `governance_status: missing_or_partial`。
- 建议先运行 `setup-doc-governance`。
- 低风险小任务可继续，但必须标记 authority 不完整。

如果存在 metadata：

- 默认只纳入 `status: active` 的文档。
- `source_of_truth: generated` 的文档只作为消费视图，不作为事实源。
- `status: archived`、`status: superseded` 的文档默认不进入当前事实上下文。
- `GOVERNANCE_PLAN.md` 中的 blocked decisions 只能作为待确认风险，不作为当前事实。

### Step 4. Read Existing case Docs

读取存在的文件：

```text
case_state.yaml
plan.md
handoff.md
execution.md
review.md
closure.md
```

冲突规则：

- `case_state.yaml` 是状态缓存，不是唯一真相。
- 如果它与 Markdown 冲突，必须报告并以 Markdown + git 状态重新推导。

### Step 5. Find Relevant Code and Tests

查找依据：

- 用户需求关键词。
- 入口索引的 related code。
- Authority 文档 frontmatter 的 `related_code`。
- `rg` 搜索。
- 测试目录和命名约定。

只读取相关文件，不全量吞入。

检索策略：

- 先用 path/title/BM25/keyword 找精确标识符、错误码、配置键和文件名。
- 再用语义线索找设计、领域和运维文档。
- 对代码问题优先使用 symbol、调用关系、测试文件和当前 diff。
- 对候选文档按 relevance、authority、freshness、workspace proximity 排序。
- 最终进入 brief 的内容必须说明 `why_included`。

### Step 5.5. Build Context Pack Candidate

在 brief 中附上任务级 Context Pack 摘要，而不是把所有原文堆入计划。

```yaml
context_pack_candidate:
  case:
    id:
    intent:
    risk:
  policy_pack: []
  authority_pack: []
  code_pack: []
  test_pack: []
  evidence_pack: []
  excluded:
    - path:
      reason:
```

准入规则：

- 优先 active authority、与任务相关的文档。
- 排除 draft、superseded、archived 文档，除非用户明确要求追溯历史。
- 历史文档只能进入 `evidence_pack`，并标记 historical。
- case evidence 可进入任务上下文，但必须标记 unconfirmed。
- `GOVERNANCE_PLAN.md` 的 blocked facts 不进入当前 fact pack。
- 运行时证据、日志、CI 结果必须带时间窗口。

### Step 6. Detect Conflicts

冲突类型：

- 用户本轮要求与 authority 冲突。
- authority 与代码冲突。
- authority 与测试冲突。
- L3 derived doc 与 authority 冲突。
- case_state 与 Markdown 冲突。
- 历史文档与当前代码冲突。
- 生成视图与源资产冲突。
- 用户本轮新事实与 `GOVERNANCE_PLAN.md` blocked decisions 冲突。

处理规则：

- 不自动解决 authority 与代码冲突。
- 把冲突写进 brief。
- 推荐下一步：确认事实、更新计划或运行治理。

### Step 7. Produce Brief

Brief 必须回答：

- 当前任务是什么。
- 哪些文档是权威。
- 哪些代码和测试相关。
- 当前约束是什么。
- 是否存在冲突。
- 哪些问题阻塞计划。

---

## 11. Authority Orders

### Execution Instruction Order

用于判断本次任务应该怎么执行：

```text
1. 用户本轮明确指令
2. 已确认 plan.md
3. 已确认 GOVERNANCE_PLAN.md，如当前任务是文档治理
4. Workflow Protocol
5. Active authority docs
6. 当前生产代码
7. 当前测试
8. L2 operational docs
9. L3 derived docs
10. L4 historical docs
11. L5 scratch docs
```

### Fact Authority Order

用于判断当前事实是什么：

```text
1. Active authority docs
2. 当前生产代码
3. 当前测试
4. 已接受 ADR / migration / release note
5. L2 operational docs
6. L3 derived docs
7. 用户本轮新信息，默认需要进入计划或治理确认
8. L4 historical docs
9. L5 scratch docs
```

---

## 12. Gate

- 有阻塞级 authority / 代码冲突时，不进入 `plan-confirm`。
- 无法定位任务但用户需要接续已有任务时，必须确认任务目录。
- 没有读到足够上下文时，不得生成计划。
- 不能自动把用户本轮新事实写入 authority。
- 不能自动修改代码或文档，除非只是写 brief。
- `Needs Refresh`、`Superseded`、`Archived` 文档不能作为当前事实进入计划，除非用户明确要求追溯历史。
- 生成视图不能压过 source_of_truth。
- Context Pack 候选如果只包含低权威或过期材料，必须标记为阻塞或高风险。

---

## 13. 验收标准

- brief 能让后续 agent 不重新猜上下文。
- brief 明确列出相关 authority、代码、测试和任务文档。
- brief 明确 unknowns、risks 和 conflicts。
- brief 不包含实施计划。
- 如果治理文档缺失，brief 清楚记录缺口和继续风险。
- brief 说明任务意图、检索路线、纳入/排除哪些上下文以及原因。
- brief 只包含最小高信号上下文，不全量复制文档。

---

## 14. 失败与恢复

如果不是 git 仓库：

- 继续读取文件。
- 在 brief 中记录 `base_commit: unavailable:not-a-git-repository`。

如果多个任务候选：

- 列出候选任务。
- 要求用户选择，或基于最新未 closed 任务做明确假设。

如果 authority 与代码冲突：

- 停止进入计划。
- 在 brief 中写明冲突和需要确认的问题。

---

## 15. 参考资料

- [AI Coding 场景下的文档快速定位、索引设计与任务上下文生成](<../../reference/docs/AI Coding 场景下的文档快速定位、索引设计与任务上下文生成.md>)
- [AI Coding 文档治理分层模型](<../../reference/docs/AI Coding 文档治理分层模型.md>)
- [AI Coding 文档生命周期管理与最佳实践](<../../reference/docs/AI Coding 文档生命周期管理与最佳实践.md>)
- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
- [AI Coding 文档治理知识识别与流转机制](<../../reference/docs/AI Coding 文档治理知识识别与流转机制.md>)
