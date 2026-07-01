# context-authority Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `context-authority` 的触发、上下文解析、权威判断、输出 brief、边界和验收标准。

---

## 1. 定位

`context-authority` 是计划前的条件性上下文获取与事实权威判断
preflight gate，不是默认 workflow phase。

它负责建立计划所需的最小事实基础，但不写实施计划、不改代码、不创建任务实体、不解决权威冲突。

核心规则：

```text
没有 context，不准计划。
AI 只应消费最小、相关、已验证、未过期的上下文。
context-authority 默认不落盘；只有必要时才写 context brief。
路由 verdict 优先于开放式建议。
```

`context-authority` 不是所有开发任务的固定入口。它只在任务确实需要计划前上下文 gate、恢复上下文、case 歧义判断、权威判断、高风险边界判断或冲突判断时触发。

---

## 2. 建议 Frontmatter

```yaml
---
name: context-authority
description: Check minimal context and authority before planning. Use only for resume, case ambiguity, high-risk/public contract, workflow/agent policy, or conflicts.
---
```

---

## 3. 触发条件

应该触发：

- 用户要求继续、恢复或判断已有任务状态。
- 存在多个 case 候选，或 case identity 可能影响路由。
- 用户要求判断当前上下文、任务状态或文档权威。
- 任务涉及 authority、public contract、workflow、agent policy、ADR 保护的架构边界或高风险领域。
- `plan-confirm` 发现缺少足够上下文，且没有安全的低风险 skipped-context reason。

可跳过：

- 明确的一步低风险修改。
- 纯机械文档修正。
- 用户明确只要求查询或解释，且不进入计划。
- 不需要 authority / code / tests 交叉判断的局部低风险任务。
- 已有 inline context summary 或明确低风险 skipped-context reason 的普通持久任务。

不应该触发：

- 用户要求初始化文档治理。此时使用 `setup-doc-governance`。
- 用户明确要求 review 已完成实现。此时使用 `review`。
- 用户要求任务收尾和文档同步。此时使用 `doc-sync-close`。

---

## 4. 输入

候选输入按任务意图读取，不是固定全量输入：

- 用户本轮需求。
- 当前 workspace。
- 当前 branch / worktree。
- `docs/README.md`，当需要文档入口或 authority 路由时读取。
- 治理计划，仅在任务涉及文档治理、authority 变更、workflow / agent policy、public contract、blocked decision 或高风险冲突时读取。
- `docs/authority/` 中与任务相关的 active authority 文档。
- 相关代码和测试，仅按任务意图读取。
- 已有 `docs/cases/<case-id>/`，当用户恢复任务或已有明确 case 时读取。
- 已有 runtime evidence、日志、CI 结果或用户提供材料，必须记录时间窗口和可信度。

---

## 5. 输出

默认输出是对话中的最小上下文摘要，或交给 `plan-confirm` 内嵌到 `plan.md` 的 `Context Summary / Context Sources`。

只有以下情况才写文件：

- 已有 case docs，需要恢复或接续。
- 高风险任务。
- 存在 authority / code / tests / case docs 冲突。
- 多 agent 或跨会话接续需要稳定上下文。
- 用户明确要求落盘。

条件性落盘路径：

```text
docs/cases/<case-id>/context-authority-brief.md
```

如果 case id 尚未建立，`context-authority` 只输出 `proposed_case_slug` 或 `case_intent`。正式 `case_id` 和 `docs/cases/<case-id>/` 由 `docloom-workflow` 在进入持久化 case workflow 时创建。

最小模板：

```md
# Context & Authority Brief

## User Request

## Intent
- Type:
- Reason:
- Minimum Evidence Needed:

## Workspace Snapshot
- Workspace:
- Branch:
- Worktree:
- Git Available:
- Dirty Workspace:
- Changed Files Summary:

## Case Context
- Existing Case:
- Proposed Case Slug:
- Case State:

## Sources Read
| Source | Layer / Type | Why Included | Trust / Freshness |
|---|---|---|---|

## Sources Excluded
| Source | Reason |
|---|---|

## Authority / Constraints

## Relevant Code / Tests

## Conflicts / Unknowns
| Topic | Conflict / Unknown | Risk | Required Decision |
|---|---|---|---|

## Route Verdict
- Verdict:
- Reason:
- Required Next Owner:
```

可选动态章节：

- `Current Behavior`
- `Existing Case Docs`
- `Governance Plan / Blocked Decisions`
- `Runtime Evidence`
- `Historical Evidence`

这些章节只在任务需要时加入，不为填表而读取材料。

---

## 6. 推荐资源结构

```text
context-authority/
  SKILL.md
  templates/
    context-authority-brief.md
  references/
    shared-protocol.md
    context-resolution.md
    retrieval-routing.md
```

`shared-protocol.md` 应承载全局 Authority Orders。`context-authority` 不应复制一份完整优先级定义，只保留本 skill 的补充规则。

---

## 7. 固定开始动作

逻辑命令：

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
```

`git diff --name-only` 只在以下情况运行：

- `git status --short` 显示 dirty workspace。
- 用户要求恢复或接续任务。
- 需要判断当前变更范围。
- 后续 `plan-confirm` 需要明确 workspace baseline。

实现时必须遵守项目本地命令规范。例如当前项目要求 shell 命令加 `rtk` 前缀，则实际命令应为：

```bash
rtk git status --short
```

如果不是 git 仓库：

- 记录 `git_available: false`。
- 使用当前工作目录作为 workspace。
- 继续读取文档和文件。
- 只记录 workspace 状态，不记录 `base_commit`。`base_commit` 属于 `plan-confirm` 的计划确认绑定职责。

---

## 8. case Context Resolution Order

按顺序解析：

```text
1. 用户本轮明确指定 case id / case docs / branch / worktree
2. 当前 worktree + case branch 推导 case id
3. 当前 branch 推导 case id
4. 当前目录下已有明确 case docs
5. 当前会话提出 proposed_case_slug，由 docloom-workflow 决定是否创建正式 case
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

- 只有在低风险、用户没有明确要求恢复特定任务、且最新未 closed 候选唯一明显相关时，才允许做明确假设。
- 其他情况必须询问用户确认。

如果 `closure.md` 已存在：

- `Done` / `Cancelled` / `Superseded` / `Abandoned` 默认不恢复。
- `Paused` / `Blocked` 可根据用户请求恢复。

---

## 9. 运行模式

| 模式 | 新 branch | 新 worktree | 适用场景 |
|---|---:|---:|---|
| `isolated` | 是 | 是 | 大任务、并行任务、高风险任务 |
| `branch` | 是 | 否 | 普通开发任务 |
| `inline` | 否 | 否 | 小任务、已有分支、临时任务 |

`context-authority` 只识别模式，不主动创建 branch、worktree 或 case docs。

---

## 10. 核心工作流

### Step 0. case Intent Routing

先判断任务类型，再决定读取哪些证据，避免把所有文档塞进上下文。

| 任务类型 | 最低证据 | 优先证据 | 默认排除 |
|---|---|---|---|
| Bug / 报错修复 | 复现或报错线索、相关代码入口、至少一个测试或验证路径 | 错误码、堆栈、相关 symbol、失败测试、runbook、known issues | 旧设计稿、无关需求 |
| Feature / 新需求 | 用户目标、acceptance criteria 草案、相关 authority / contract 或明确缺失说明、相邻实现 | spec、API contract、ADR、相邻测试策略 | 历史会议纪要 |
| Refactor | 目标边界、相关模块、测试覆盖入口、不触碰范围 | repo map、dependency graph、ADR、模块 owner、测试覆盖 | 过期 README |
| Documentation Governance | 治理范围、authority / index 状态、相关历史材料、blocked decisions | 治理计划、authority index、入口索引、case evidence、archive | 业务代码深读，除非用于验证 |
| Workflow / Skill Design | 目标 skill、上游协议、相邻 skill、constitution / authority 约束 | skill design、workflow protocol、ADR、reference skill | 无关业务代码 |
| Resume | case state、最近 handoff / plan / execution / closure 状态、当前 git 状态 | case docs、branch / worktree、changed files | 无关历史材料 |
| Incident / 运维 | 当前症状、影响范围、可回滚线索、相关 runbook | 监控说明、回滚指南、近期发布、known issues | 产品需求文档 |

`Review` 不作为 `context-authority` 的默认 intent。用户明确要求 review 时直接使用 `review`。如果 `review` skill 需要复用检索策略，可以读取本节作为辅助，但不改变触发入口。

Brief 或上下文摘要必须记录 `case Intent` 和选择该意图的理由。

### Step 1. Resolve Workspace

识别：

- repo root。
- 当前 branch。
- 当前 worktree。
- 工作区未提交变更摘要。
- git 是否可用。

失败不阻塞文档任务，但必须记录。

### Step 2. Resolve case Context

按 case Context Resolution Order 推导：

- `case_id` 或 `proposed_case_slug`。
- `case_docs` 是否存在。
- `case_state.yaml` 是否存在。
- 当前 phase。

`context-authority` 不创建新 case。正式创建由 `docloom-workflow` 负责。

### Step 3. Read Governance Docs

按相关性读取：

```text
docs/README.md
docs/authority/**/*.md
治理计划，仅在相关时
```

治理计划只在以下情况读取：

- Documentation Governance。
- authority update。
- workflow / agent policy。
- public contract。
- 高风险任务。
- 用户本轮事实可能触碰 blocked decision。
- authority 缺失或上下文冲突需要查 blocked decisions。

如果 governance / authority 不存在：

- 记录 `governance_status: missing_or_partial`。
- 低风险和局部任务可继续，但必须标记 authority 不完整。
- 中高风险、public contract、agent policy、workflow、架构边界、安全、隐私、计费、数据删除相关任务，建议或要求先运行 `setup-doc-governance`。

如果存在 metadata：

- 默认只纳入 `status: active` 的文档。
- `source_of_truth: generated` 的文档只作为消费视图，不作为事实源。
- `status: archived`、`status: superseded`、`status: needs_refresh` 的文档默认不进入当前事实上下文。
- 治理计划中的 blocked decisions 只能作为待确认风险，不作为当前事实。

### Step 4. Read Existing case Docs

仅在已有 case 或恢复任务时读取存在的文件：

```text
case_state.yaml
plan.md
handoff.md
execution.md
closure.md
```

冲突规则：

- `case_state.yaml` 是状态缓存，不是唯一真相。
- 如果它与 Markdown 冲突，必须报告并以 Markdown + git 状态重新推导。

### Step 5. Find Relevant Code and Tests

代码和测试按任务意图读取，不是必需输入。

读取依据：

- 用户需求关键词。
- 入口索引的 related code。
- Authority 文档 frontmatter 的 `related_code` / `related_tests`。
- `rg` 搜索。
- 测试目录和命名约定。

规则：

- Bug、行为变化、refactor 通常需要读取相关代码和测试。
- Feature 读取相邻实现和测试策略；纯产品或文档需求可不读代码。
- Documentation Governance 默认不深读业务代码，除非验证事实。
- Workflow / skill design 优先读权威设计和相邻 skill，代码可选。
- 只读取相关文件，不全量吞入。

检索策略：

- 先用 path/title/BM25/keyword 找精确标识符、错误码、配置键和文件名。
- 再用语义线索找设计、领域和运维文档。
- 对代码问题优先使用 symbol、调用关系、测试文件和当前 diff。
- 对候选文档按 relevance、authority、freshness、workspace proximity 排序。
- 最终进入上下文摘要的内容必须说明 `why_included`。

### Step 5.5. Build Context Selection Notes

在 brief 或计划上下文中附上任务级来源选择说明，而不是生成正式 `Context Pack Candidate`。

```yaml
context_sources:
  case:
    existing_id:
    proposed_slug:
    intent:
    risk:
  included:
    - path:
      layer:
      why_included:
      trust:
  excluded:
    - path:
      reason:
```

准入规则：

- 优先 active authority、与任务相关的文档。
- 排除 draft、superseded、archived、needs_refresh 文档，除非用户明确要求追溯历史。
- 历史文档只能作为 evidence，并标记 historical。
- case evidence 可进入任务上下文，但必须标记 unconfirmed。
- 治理计划的 blocked facts 不进入当前 fact pack。
- 运行时证据、日志、CI 结果必须带时间窗口。

### Step 6. Detect and Classify Conflicts

冲突类型：

- 用户本轮要求与 authority 冲突。
- authority 与代码冲突。
- authority 与测试冲突。
- L3 derived doc 与 authority 冲突。
- case_state 与 Markdown 冲突。
- 历史文档与当前代码冲突。
- 生成视图与源资产冲突。
- 用户本轮新事实与治理计划 blocked decisions 冲突。

阻塞级冲突：

- public API / CLI / schema / config contract。
- 安全、权限、认证、隐私、计费、数据删除。
- ADR 明确规定的架构边界。
- owner 最近确认过的事实。
- 代码和测试不一致。
- 测试缺失且影响面较高。
- workflow / agent policy 变更缺少确认。

非阻塞漂移：

- 内部实现细节。
- 非 public contract。
- 历史文档没有 active authority 状态。
- 代码和测试一致。
- 低风险局部任务。

处理规则：

- 不自动解决 authority 与代码冲突。
- 阻塞级冲突写入 brief 或对话摘要，并给出 `blocked_by_authority_conflict` 或 `needs_user_decision`。
- 非阻塞漂移可进入 `plan-confirm`，但计划必须记录 assumption、risk 和是否需要后续治理。

### Step 7. Produce Route Verdict

输出明确路由 verdict，而不是开放式建议。

```text
proceed_to_plan
proceed_to_plan_with_risk
needs_user_decision
needs_case_selection
run_setup_doc_governance
blocked_by_authority_conflict
```

含义：

| Verdict | 含义 |
|---|---|
| `proceed_to_plan` | 上下文足够，未发现阻塞冲突 |
| `proceed_to_plan_with_risk` | 可计划，但 authority 缺失、低权威来源或非阻塞漂移必须进入计划风险 |
| `needs_user_decision` | 缺少关键事实或用户决策 |
| `needs_case_selection` | 多个 case 候选，不能安全假设 |
| `run_setup_doc_governance` | 当前任务依赖缺失或冲突的治理事实，需先治理 |
| `blocked_by_authority_conflict` | 存在阻塞级 authority / code / tests 冲突 |

---

## 11. Authority Orders

全局 `Execution Instruction Order` 和 `Fact Authority Order` 必须由 Workflow Protocol / shared-protocol 统一定义。`context-authority` 不维护第二份完整顺序。

本 skill 的补充规则：

- 用户本轮明确指令可以改变本次任务目标，但不能自动改写长期事实。
- 用户本轮新事实应作为 pending fact，排在 L2 operational docs 和 L3 derived docs 之前，但不能压过 active authority、当前代码、当前测试或已接受 ADR。
- 只影响当前任务执行策略、验收口径或临时约束的新事实，进入 `plan.md` confirmation。
- 改变长期产品事实、workflow policy、authority docs、public contract 或 agent behavior 的新事实，进入治理计划或 authority update proposal。
- generated view 不能压过 source_of_truth。

---

## 12. Gate

- 有阻塞级 authority / 代码 / 测试冲突时，不进入 `plan-confirm`。
- 非阻塞漂移可以进入 `plan-confirm`，但必须记录风险和假设。
- 无法定位任务且用户需要接续已有任务时，必须确认任务目录。
- 没有读到按 intent 定义的最低证据时，不得生成计划。
- 不能自动把用户本轮新事实写入 authority。
- 不修改代码、测试、构建脚本或运行行为。
- 不修改 authority、governance plan、case plan、execution、review 或 closure。
- 仅在条件满足时写 context brief；默认对话输出或交给 `plan-confirm` 内嵌。
- `Needs Refresh`、`Superseded`、`Archived` 文档不能作为当前事实进入计划，除非用户明确要求追溯历史。
- Context sources 如果只包含低权威或过期材料，必须标记为阻塞或高风险。

---

## 13. 验收标准

- 后续 agent 能知道计划前应相信哪些来源、排除哪些来源、还有哪些风险。
- 输出明确列出相关 authority、代码、测试和任务文档，或说明为何未读取。
- 输出明确 unknowns、risks、conflicts 和 route verdict。
- 输出不包含实施计划。
- 如果 governance / authority 缺失，输出清楚记录缺口和继续风险。
- 输出说明任务意图、检索路线、纳入 / 排除哪些上下文以及原因。
- 输出只包含最小高信号上下文，不全量复制文档。
- 默认不创建 case、不落盘；条件性 brief 落盘时理由清楚。

---

## 14. 失败与恢复

如果不是 git 仓库：

- 继续读取文件。
- 记录 `git_available: false` 和 workspace path。
- 不记录 `base_commit`。

如果多个任务候选：

- 列出候选任务。
- 只有低风险且唯一明显相关时可明确假设。
- 其他情况输出 `needs_case_selection`。

如果 authority 与代码冲突：

- 判断是否阻塞级。
- 阻塞级冲突停止进入计划。
- 非阻塞漂移可进入计划，但必须记录风险、assumption 和后续治理建议。

如果 governance / authority 缺失：

- 低风险局部任务可输出 `proceed_to_plan_with_risk`。
- 高风险、public contract、workflow、agent policy、架构边界、安全、隐私、计费、数据删除相关任务输出 `run_setup_doc_governance` 或 `needs_user_decision`。

---

## 15. 参考资料

- [AI Coding 场景下的文档快速定位、索引设计与任务上下文生成](<../../reference/docs/AI Coding 场景下的文档快速定位、索引设计与任务上下文生成.md>)
- [AI Coding 文档治理分层模型](<../../reference/docs/AI Coding 文档治理分层模型.md>)
- [AI Coding 文档生命周期管理与最佳实践](<../../reference/docs/AI Coding 文档生命周期管理与最佳实践.md>)
- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
- [AI Coding 文档治理知识识别与流转机制](<../../reference/docs/AI Coding 文档治理知识识别与流转机制.md>)
