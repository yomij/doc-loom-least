---
status: archived
---

# grill Skill 详细设计

> 来源：早期外部挑战 skill 参考与本仓库 grill 设计评审决策；相关 raw skill 文件已移除，不再作为引用来源或当前权威。
>
> 目标：定义 `grill` 作为通用、流程无关、用户手动触发的挑战 / 拷问 skill。它只通过对话压力测试主张和决策，不生成中间产物，不修改文件，不承担 Doc Loom workflow 阶段职责。

---

## 1. 定位

`grill` 是通用手动挑战 skill。

它用于压力测试任何需求、计划、架构、实现思路、测试策略、文档变更想法、产品判断或其他待决主张。它不属于 Doc Loom workflow 的流程阶段，不由 `docloom-workflow` 路由，不进入 Artifact Policy。

核心规则：

```text
grill 只能由用户手动触发。
grill 只挑战和澄清，不生成中间产物，不修改任何文件。
grill 不输出流程 verdict，不指定 workflow next owner。
提问阶段一次只问一个问题。
每个决策问题必须给出 2-3 个真实选项，并推荐一个。
推荐理由保持一句话；成立条件并入推荐理由。
从第二问开始，先用一句话复述上一问结果。
如果问题可由代码或文档回答，先探索项目，而不是问用户。
```

`grill` 的结果只存在于对话中。后续流程如果需要使用这些结果，应由对应流程自己吸收。例如 `plan-confirm` 可以把用户明确确认的讨论决策写入 `plan.md` 的 `## Decisions`；最终 authority 文档变更由收尾或治理流程处理，不由 `grill` 处理。

---

## 2. 建议 Frontmatter

```yaml
---
name: grill
description: Interactively challenge and stress-test a requirement, plan, architecture proposal, implementation idea, test strategy, documentation change, or other claim. Use only when the user explicitly asks to grill, challenge, pressure-test, scrutinize, or aggressively question assumptions. Ask one question at a time, provide 2-3 options with a concise recommendation, verify facts from code or docs when possible, and do not create artifacts or modify files.
---
```

---

## 3. 触发条件

只在用户明确要求时触发，例如：

- “grill 一下”
- “严格拷问这个方案”
- “帮我挑战一下计划”
- “找计划漏洞”
- “压力测试这个设计”
- “审一下需求边界，但先别实现”

不因以下情况自动触发：

- 风险等级是 high。
- agent 觉得计划不完整。
- review 或执行阶段发现风险。
- workflow 路由表匹配某个状态。

高风险时 agent 可以建议用户触发 `grill`，但不能擅自执行。

---

## 4. 可 Grill 对象

`grill` 是流程无关工具，可以挑战任意清晰对象，包括：

- 用户需求。
- 计划草案或已确认计划。
- 架构方案。
- 实现策略。
- 测试策略。
- 文档更新想法。
- 产品判断。
- 权衡取舍。
- 用户在当前对话中提出的主张。

如果用户没有明确对象：

- 先用 1 个澄清问题确认要挑战什么。
- 不凭空选择 workflow 阶段或 case artifact。

---

## 5. 输入

最小输入是用户指定的 grill 对象和当前对话上下文。

可按需读取：

- 用户明确指定的文档。
- 当前项目中与问题直接相关的代码、测试或文档。
- 相关参考材料。

读取规则：

- 先基于目标对象形成问题。
- 如果问题属于事实问题，且可以由代码或文档回答，先查证再回答。
- 如果问题属于决策问题，才问用户。
- 读取材料只服务于回答当前问题，不进入流程状态，不生成 artifact。

---

## 6. 输出

`grill` 只在对话中输出，不写文件。

禁止输出：

```text
docs/cases/<case-id>/grill.md
Grill Report
Proceed / Revise Plan / Blocked
workflow next owner
artifact path
stage transition
```

提问阶段格式：

```md
已选：Q<N-1> = <用户选择>。这意味着 <一句复述>。

Q<N>：<一个尖锐问题>

A. <选项>
B. <选项>
C. <选项>

推荐：<A/B/C>，因为 <一句理由；必要时包含成立条件或替代选择>。
```

第一问省略 `已选` 行。

收束阶段格式：

```md
确认：
- <决策>

未决：
- <问题>

风险：
- <弱假设>
```

收束不是报告，不承诺落盘，不自动进入任何流程动作。

---

## 7. 推荐资源结构

```text
grill/
  SKILL.md
  references/
    challenge-prompts.md
```

不提供 `templates/grill-report.md`。`challenge-prompts.md` 只能作为内部提问参考，不能定义必填维度、固定章节或报告模板。

---

## 8. 核心工作流

### Step 1. Restate Target

先用 1-3 句话中性复述：

- 正在挑战什么对象。
- 当前主张是什么。
- 已知边界是什么。

如果对象不明，只问一个澄清问题。

### Step 2. Choose Highest-Leverage Question

自由选择当前最关键问题。

不要机械覆盖固定清单。可以在内部参考目标、非目标、成功标准、证据、边界条件、失败模式、测试 / 验证和文档影响等方向，但对外不要求固定维度。

### Step 3. Separate Fact From Decision

事实问题：

- 先查代码、测试或文档。
- 能查到就给结论和依据。
- 查不到就说明证据缺口。

决策问题：

- 一次只问一个。
- 给 2-3 个真实选项。
- 推荐一个选项。
- 推荐理由只写一句话；成立条件并入理由。
- 如果条件变化，直接在推荐理由里给替代选择。

推荐答案格式：

```text
推荐：A，因为它最小且可回滚；如果这是 public API，再选 B。
```

### Step 4. Walk The Decision Tree

沿设计树逐个解决依赖决策。

从第二问开始，先用一句话复述上一问结果：

```text
已选：Q1 = A。这意味着先保持最小改动，把扩展点留到证据出现后再加。
```

用户可以用简短回答确认，例如：

```text
是
按推荐
继续
```

如果短答无歧义，视为确认当前推荐答案，并继续下一个最高杠杆问题。如果有歧义，只问一个澄清问题。

### Step 5. Handle Insufficient Facts

如果事实基础不足：

- 先说明缺少哪些事实。
- 只问一个最关键澄清问题。
- 不把 `grill` 变成需求采集、上下文补全或文档治理流程。

### Step 6. Close When Converged

结束条件：

- 用户要求结束或总结。
- agent 判断关键分支已经收敛。

结束时输出轻量收束：

- `确认`
- `未决`
- `风险`

不自动进入计划、执行、review、收尾或治理。

---

## 9. 高风险规则

高风险领域包括：

- 安全。
- 权限。
- 认证 / 授权。
- 隐私。
- 计费。
- 数据删除。
- public API / CLI / schema / config contract。
- 高影响架构边界。

在高风险领域：

- 推荐答案必须降低确定性。
- 必须明确证据来源和未验证点。
- 用户短答只确认当前讨论取舍，不等于长期事实授权。
- 不把未验证结论写成事实。

---

## 10. 与 Doc Loom Workflow 的关系

`grill` 是通用辅助 skill，不是 Doc Loom workflow 阶段。

边界：

- 不由 `docloom-workflow` 路由。
- 不进入 Artifact Policy。
- 不写 `grill.md`。
- 不更新 `case_state.yaml`。
- 不改 `plan.md`。
- 不改 authority 文档。
- 不提出 workflow verdict。
- 不指定 next owner。

后续流程可以消费用户确认过的讨论决策，但消费动作属于后续流程：

- `plan-confirm` 可以把用户明确确认的决策写入 `plan.md` 的 `## Decisions`。
- `review` 可以检查实现、测试和文档是否遵守 `plan.md ## Decisions`。
- `doc-sync-close` 可以判断哪些 decision 需要进入 authority proposal。

---

## 11. Gate

- 用户未明确要求，不执行 `grill`。
- 不生成中间产物。
- 不修改任何文件。
- 不输出固定报告模板。
- 不输出流程 verdict。
- 不把推荐答案自动当作用户决策。
- 不把用户短答升级为长期 authority。
- 不把可查证事实问题推给用户。
- 不把事实不足的场景扩展成 discovery workflow。

---

## 12. 验收标准

- 每轮只问一个问题。
- 每个决策问题都有 2-3 个真实选项和一句话推荐理由。
- 从第二问开始复述上一问结果。
- 可由代码或文档回答的问题已先查证。
- 用户确认的决策与未确认建议区分清楚。
- 高风险推荐明确证据和未验证点。
- 结束时只输出流程无关的轻量收束。
- 没有写文件、没有 artifact、没有 workflow verdict。

---

## 13. 参考资料

早期设计曾参考外部 skill 材料；相关 raw skill 文件已移除，不再作为引用来源或当前权威。

- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
