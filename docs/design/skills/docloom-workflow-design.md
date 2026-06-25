# docloom-workflow Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`。
>
> 目标：定义 `docloom-workflow` 作为 Doc Loom Least 的公开自动入口、轻量路由器、case identity owner 和 Artifact Policy owner。

---

## 1. 定位

`docloom-workflow` 是 Doc Loom Least 的 public automatic entry skill。

它不是重型 `workflow-orchestrator`，不把所有任务强行塞进一条复杂流水线。它只负责识别当前请求、解析任务状态、延迟创建 case、记录最小状态缓存，并把下一步路由到正确 skill。

核心规则：

```text
Use the minimum path that safely resolves the current request.
Route; do not replace stage skills.
No CLI backend in v1.
No route artifact by default.
When intent is ambiguous, status-only is the safe fallback.
```

---

## 2. 建议 Frontmatter

```yaml
---
name: docloom-workflow
description: Public automatic entry for Doc Loom Least. Use as the default entry when a user asks for a docs-first change workflow but does not explicitly invoke a specific Doc Loom skill. Resolves current case status, routes to setup-doc-governance, context-authority, plan-confirm, tdd-execute, doc-sync-close, or explicitly requested conversation-only review, owns delayed case creation and case_id generation, consumes review_risk signals, and applies the global Artifact Policy without becoming a heavy orchestrator.
---
```

---

## 3. 触发条件

应该触发：

- 用户请求一个文档驱动开发 / 变更 / 收尾流程，但没有显式点名具体 Doc Loom skill。
- 用户说“继续”“恢复”“当前到哪了”“下一步是什么”。
- 用户要求开始一个持久化 case。
- 用户要求执行已确认计划。
- 用户要求同步文档并结束任务。
- 用户请求治理初始化、上下文判断、计划确认、执行或收尾，但入口不明确。

不应该替代：

- 用户显式触发 `review`、`grill`、`setup-doc-governance`、`context-authority`、`plan-confirm`、`tdd-execute`、`doc-sync-close`。
- 普通一次性问答、解释或局部编辑，除非用户明确要求进入 Doc Loom workflow。
- 用户显式触发流程无关的通用辅助 skill，例如 `review` 或 `grill`。

显式 skill 调用优先：

```text
If the user explicitly invokes a Doc Loom skill, honor that skill.
The invoked skill may still fail a gate and route back to docloom-workflow or another required skill.
```

---

## 4. 输入

- 用户本轮需求。
- 当前 workspace。
- 目标仓库本地 agent instructions，仅作为工作约束。
- 当前 git root、branch 和 dirty status。
- 已有 `docs/cases/<case-id>/`，如果存在或可推导。
- `case_state.yaml`，如果存在。
- `plan.md`、`execution.md`、`closure.md`，按状态推导需要读取。
- `review_risk`，如果存在于 execution / closure context。
- 可选：`context-authority` route verdict。

Agent instructions 读取规则：

```text
Read for package manager rules, command constraints, file editing boundaries, repo-specific documentation routing, and local execution constraints.
Do not treat agent instructions as Doc Loom product architecture.
Do not use agent instructions to add CLI backend behavior to Doc Loom Least v1.
Do not let agent instructions replace skill gates.
```

---

## 5. 输出

默认输出在对话中：

```text
Current phase:
Evidence:
Route decision:
Next skill:
Blocking issue:
```

允许写入：

```text
docs/cases/<case-id>/case_state.yaml
```

仅在延迟创建 case、阶段转换或当前 skill 完成时更新。

不默认生成：

```text
workflow.md
route.md
workflow-route.md
```

路由决策如果影响计划，应进入 `plan.md` 的 `Context Sources`、`Assumptions` 或 `Risks`。如果影响收尾，应进入 `closure.md`。

---

## 6. v1 非目标

```text
- 不依赖 CLI backend。
- 不执行 setup runtime。
- 不修改 package manager 文件。
- 不生成 adapter / control files。
- 不维护中心任务索引。
- 不替代 plan-confirm 写计划。
- 不替代 tdd-execute 执行代码或测试。
- 不自动运行审查对话。
- 不拥有或路由流程无关的通用辅助 skill，例如 `grill`。
- 不因为存在 approved plan 就自动执行。
```

Doc Loom Least v1 是纯文档 / skill workflow。未来如果引入 CLI backend，必须重新设计 ownership，不得静默改变本 skill 语义。

---

## 7. 固定开始动作

逻辑命令：

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
```

`git diff --name-only` 仅在以下情况运行：

- `git status --short` 显示 dirty workspace。
- 用户要求恢复或继续已有 case。
- 需要判断当前变更是否属于 case。
- 即将进入 `plan-confirm`，需要计划 baseline 信息。
- 即将进入 `tdd-execute`，需要判断 dirty workspace 风险。

如果不是 git 仓库：

- 记录 `git_available: false`。
- 仍可进行 status-only、文档读取和纯文档流程。
- 不写 `base_commit`；`base_commit` 属于 `plan-confirm`。

---

## 8. Status-only 模式

`status-only` 是安全查询模式。

触发：

- 用户问“状态如何”“现在到哪了”“能继续吗”。
- 用户意图不清，无法安全判断是否要创建 case、执行或修改文件。
- 发现 `case_state.yaml` 与 Markdown 冲突，需要先报告。

行为：

```text
- 不创建 case。
- 不修改文件。
- 不修复 case_state.yaml。
- 读取 git state 和必要 case docs。
- 输出当前推导状态、证据、建议下一步和一个澄清问题。
```

当 intent ambiguous：

```text
default to status-only
```

---

## 9. Case Identity 与创建规则

`docloom-workflow` 是 case identity owner。

case id 来源顺序：

```text
1. 用户显式指定的 case_id / case_docs。
2. 当前 worktree + case branch 推导出的 case_id。
3. 当前 branch 推导出的 case_id。
4. 当前目录下唯一明显相关且未 closed 的 case docs。
5. context-authority 提出的 proposed_case_slug。
6. docloom-workflow 生成 `YYYYMMDD-short-slug`。
```

`docloom-workflow` 延迟创建 case docs。

不创建 case docs：

- 用户只是查询状态、问问题或要求解释。
- 只是路由到 `setup-doc-governance` 的全局治理计划。
- 只是做一次 context/status 判断且不落盘。
- 用户显式调用的 skill 不需要持久化 case。

创建 case docs：

- route decision 进入 `plan-confirm`。
- 用户明确要求开始一个 case。
- 需要落盘 `context-authority-brief.md`，因为高风险、冲突、恢复、多 agent 或跨会话。
- 已经存在执行、review 或收尾动作，需要绑定 case。

创建时的最小产物：

```text
docs/cases/<case-id>/
docs/cases/<case-id>/case_state.yaml
```

`plan-confirm` 要求已有 `case_id` 和 `case_docs`。如果缺失，应返回 `needs_docloom_workflow` 或 `needs_case_initialization`，不自行生成 case id。

---

## 10. case_state.yaml

`case_state.yaml` 是机器可读状态缓存，不是真相源。

最小初始模板：

```yaml
case_id: 20260618-short-slug
phase: context_resolving
case_docs: docs/cases/20260618-short-slug
closure_status: open
last_updated: 2026-06-18T00:00:00+08:00
```

进入 `plan-confirm` 后：

```yaml
phase: waiting_for_plan_confirmation
plan_version: 1
```

计划确认后：

```yaml
phase: planned
plan_version: 1
```

只在需要时加入：

```yaml
mode:
branch:
worktree:
review_risk:
artifacts:
```

默认不复制：

```text
risk_level
base_commit
```

这些字段属于 `plan.md`。只有某个下游自动化确实需要缓存时，才可作为派生缓存加入，并且 Markdown 冲突时以 Markdown + git 重新推导。

冲突处理：

```text
If case_state.yaml conflicts with plan/execution/closure markdown:
- Report state_cache_conflict.
- Derive current phase from Markdown + git state.
- Route based on derived phase.
- Do not silently overwrite case_state.yaml.
```

---

## 11. Artifact Policy

`docloom-workflow` 持有全局 Artifact Policy。各阶段 skill 负责写自己的产物。

| Artifact | Required When | Optional / Skip When | Writer |
|---|---|---|---|
| `plan.md` | case enters `plan-confirm` | no persistent case workflow | `plan-confirm` |
| `case_state.yaml` | case docs are created | status-only without case creation | `docloom-workflow` initially; stage skills update |
| `context-authority-brief.md` | high risk, conflict, resume, multi-agent / cross-session, explicit request | inline `Context Sources` otherwise | `context-authority` |
| `handoff.md` | future resume point exists | continuous same-session flow | current stage skill |
| `execution.md` | TDD required, code / behavior change, plan deviation, failed/retried tests, material review_risk, resume needed | docs-only, trivial config, verification fits in closure | `tdd-execute` |
| `closure.md` | case docs exist and task ends or pauses / blocks / cancels | no case docs one-shot task | `doc-sync-close` |

`case_state.yaml` 只记录本 case 的 artifact decisions，不复制完整 policy。

`review` 和 `grill` 不在 Artifact Policy 中；它们只输出对话内容，不写 case artifact。

示例：

```yaml
artifacts:
  context_brief: inline
  handoff: not_needed
  execution: conditional
  closure: required
```

`handoff.md` 规则：

```text
docloom-workflow decides whether a future resume point exists.
The current stage skill writes the handoff content because it knows the next step and caveats.
```

未来恢复点包括：

- 用户要求稍后继续、交给别人、交给另一个 agent 或另一个会话。
- 计划已确认但不立刻执行。
- 执行已开始但未完成。
- 等待用户确认或外部依赖。
- 任务进入 `Paused`、`Blocked`。
- 高风险或跨模块任务需要恢复摘要。

---

## 12. 路由表

| 条件 | Route |
|---|---|
| 用户明确要求初始化、整理、重建文档治理 | `setup-doc-governance` |
| `context-authority` verdict = `run_setup_doc_governance` | `setup-doc-governance` |
| 高风险 authority / workflow / public contract 缺失且无法安全计划 | `setup-doc-governance` 或 `needs_user_decision` |
| 用户显式要求 review / code review / docs review / test review | `review` |
| 用户要求状态、下一步、能否继续，且未授权修改 | `status-only` |
| 需要恢复 case、判断权威、处理冲突或计划前 context gate | `context-authority` |
| 请求进入持久化开发计划，且 context 足够 | `plan-confirm` |
| 已有 approved plan，用户明确要求执行 / 继续 / implement | `tdd-execute` |
| 用户要求收尾、同步文档、写 closure，或执行已完成 | `doc-sync-close` |
| intent ambiguous | `status-only` + one clarifying question |

`context-authority` 只在需要时路由，不作为所有任务固定前置。

`tdd-execute` 只能在用户有执行意图时路由：

```text
approved plan + user asks to continue/execute -> route to tdd-execute
approved plan alone -> status summary, no execution
```

`review` 保持手动属性。`docloom-workflow` 可以消费 `review_risk` 并在状态摘要中暴露风险，但不能基于风险信号自动触发 review。`grill` 是流程无关的通用手动辅助 skill，不属于本路由表。

---

## 13. Gate

- 不因用户进入入口 skill 就创建 case。
- 不因存在 approved plan 就自动执行。
- 不默认运行 `context-authority`。
- 不自动触发 `review`。
- 不因为 `review_risk` 自动触发 `review`。
- 不拥有或路由 `grill`。
- 不在 status-only 中写文件。
- 不静默修复 `case_state.yaml` 与 Markdown 冲突。
- 不写 `base_commit`；`plan-confirm` 在计划确认时写。
- 不生成独立 route artifact。
- 不调用 CLI backend。
- 如果 route decision 需要文件修改，但用户意图不清，停在 status-only。

---

## 14. 验收标准

- 未显式点名 skill 的 Doc Loom workflow 请求能进入 `docloom-workflow`。
- 明确点名 skill 的请求不会被入口截获。
- 能从 git、branch、case docs 和 `case_state.yaml` 推导当前状态。
- 能延迟创建 case，并写最小 `case_state.yaml`。
- `plan-confirm` 不再拥有 case id 生成职责。
- Artifact Policy 集中在本 skill，阶段 skill 不再各自固定生成所有产物。
- 意图不清时不写文件。
- 无 CLI backend 依赖。
- 输出 route decision 清楚说明证据、下一步和阻塞项。

---

## 15. 失败与恢复

如果多个 case 候选：

- 低风险且唯一明显相关时可以明确假设。
- 其他情况进入 `status-only`，输出 `needs_case_selection`。

如果缺少 git：

- 继续读取文档。
- 标记 `git_available: false`。
- 不写 `base_commit`。

如果 `case_state.yaml` 损坏：

- 尝试从 Markdown 和 git 推导。
- 报告 `state_cache_unreadable`。
- 不静默重写，除非用户明确要求修复状态缓存。

如果 route 指向的 skill gate 失败：

- 输出 gate failure。
- 路由到 required next owner，例如 `context-authority`、`plan-confirm` 或 `needs_user_decision`。

---

## 16. 参考资料

- [writing-plans/SKILL.md](../../reference/skills/writing-plans/SKILL.md)
- [executing-plans/SKILL.md](../../reference/skills/executing-plans/SKILL.md)
- [AI Coding 场景下的文档快速定位、索引设计与任务上下文生成](<../../reference/docs/AI Coding 场景下的文档快速定位、索引设计与任务上下文生成.md>)
- [AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制](<../../reference/docs/AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制.md>)
