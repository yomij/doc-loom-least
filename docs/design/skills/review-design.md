# review Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md` 与本仓库 review 设计评审决策。
>
> 目标：定义 `review` 如何在用户明确要求时，对设计文档、方案、代码 diff、测试集合、文档变更或 Doc Loom case 证据做只读、对话式、证据型审查，并输出 assessment、findings 和 evidence gaps。

---

## 1. 定位

`review` 是用户手动触发的通用辅助 skill，用于证据型审查。

它不是 Doc Loom workflow 阶段，不进入 Artifact Policy，不产出中间文件，不修改状态，不修改任何文档或代码，也不负责 workflow 路由。

核心规则：

```text
review 只能由用户明确要求触发。
review 覆盖任意明确对象：设计文档、方案、代码 diff、测试集合、文档变更或 case evidence。
review 是 read-only / conversation-only。
review 输出 assessment、evidence reviewed、findings 和 evidence gaps。
review 不输出 verdict、ready-to-close、next step、route 或 authority proposal。
review 不创建持久化审查文件，不更新状态缓存，不修改 plan / execution / closure / authority。
所有 workflow 路由由 docloom-workflow 负责。
```

`review` 与 `grill` 的边界：

```text
review = 读取证据后直接输出结构化审查发现。
grill  = 一次只问一个问题，沿决策树交互式拷问并逼近共识。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: review
description: Review a specified design document, proposal, code diff, test set, documentation change, or Doc Loom case evidence when the user explicitly asks for review. Performs read-only, conversation-only evidence review; reports assessment, findings, and evidence gaps; does not create artifacts, modify files, update status, write proposals, or route workflow.
---
```

---

## 3. 触发条件

只在用户明确要求 review / 审查 / 检查时触发，例如：

- “review 一下”
- “做 code review”
- “让子 agent review”
- “检查这次实现”
- “审查测试覆盖”
- “评审这个设计文档”
- “看一下这个方案有没有问题”
- “检查有没有破坏文档权威”

不因以下情况自动触发：

- `risk_level = high`。
- `review_risk = high`。
- `tdd-execute` 或 `doc-sync-close` 记录了风险信号。
- agent 主观觉得需要再看一眼。
- 执行完成或 case 即将收尾。

`docloom-workflow` 可以在用户明确要求 review 时分派到本 skill，也可以在状态摘要中暴露 `review_risk`，但不能因为风险信号自动运行 review。

---

## 4. 可 Review 对象

`review` 可以审查任何明确对象：

- 独立设计文档。
- 方案文档。
- 需求草案。
- 代码 diff。
- 测试文件或测试策略。
- 文档治理变更。
- authority / ADR / contract 相关变更。
- Doc Loom case 的 plan、execution、closure 或其他证据。
- 用户粘贴的方案、片段或材料。

如果用户未指定对象：

```text
1. 如果当前对话或 docloom-workflow 明确给出对象，审那个对象。
2. 如果当前 git diff 非空，审当前 diff。
3. 如果当前 case 明确且有计划 / 执行材料，审当前 case 的相关变更。
4. 如果存在多个候选对象或没有候选对象，只问一个最小确认问题。
```

不要默认读取整个仓库、整个 `docs/` 或整个 `docs/design/`。

---

## 5. Review Maturity

每次 review 必须标明对象成熟度，避免把草稿审查误读为完成态审查。

```text
Draft
  审需求、假设、边界、事实来源、设计自洽性。

In-progress
  审当前改动、已知缺口、计划符合度、风险漂移。

Completed
  审验收证据、测试可信度、文档影响、残留风险。
```

示例：

```text
Review Maturity: Draft
Assessment: Material issues found.
```

---

## 6. 输入与证据选择

`review` 按评审对象选择最小相关证据，不把 case 文档作为固定必需输入。

常见证据选择：

| 对象 | 默认读取 | 按需读取 |
|---|---|---|
| 独立设计文档 / 方案 | 目标文档、显式引用来源、相关 constitution / authority / ADR | 涉及代码事实、契约或测试策略时读取相关代码 / 测试 |
| code diff | git diff、相关实现、相关测试、相关 authority / contract | plan / execution，如果用户要求审 case 符合度 |
| tests only | 测试文件、相关实现、测试命令或结果 | 计划、验收标准、TDD 证据 |
| docs only | 目标文档、上游 authority、相关索引 / ADR | 历史材料，只作为 evidence 并标记低权威 |
| documentation governance | constitution、GOVERNANCE_PLAN、authority / archive / index 相关文档 | 代码 / 测试，仅用于验证事实 |
| case evidence | 用户指定的 plan / execution / closure / diff / 测试结果 | case_state.yaml 仅在对象就是状态一致性时读取 |

`case_state.yaml` 默认不读，因为它是状态缓存，不是真相源。

如果证据缺失但仍可审查，应输出 evidence gap，而不是把问题升级成 workflow block。

---

## 7. Authority 与事实判断

`review` 使用全局 Fact Authority Order 发现事实冲突，但不解决冲突、不提出治理路由。

默认事实权威顺序：

```text
1. Active authority docs
2. 当前生产代码
3. 当前测试
4. 已接受 ADR / migration / release note
5. 用户本轮新信息，默认作为 pending fact
6. L2 operational docs
7. L3 derived docs
8. L4 historical docs
9. L5 scratch docs
```

规则：

- Active authority 与目标设计冲突时，输出 finding。
- 代码 / 测试与目标设计冲突时，输出 finding 或 evidence gap。
- historical docs 只能作为证据或历史上下文，不能当作当前事实。
- 用户本轮新事实可以作为 pending fact，但不能压过 active authority、当前代码、当前测试或已接受 ADR。
- 改变长期产品事实、workflow policy、authority docs、public contract 或 agent behavior 的内容，只能被 review 标记为 governance risk，不能由 review 写 proposal。

---

## 8. 输出

`review` 只在对话中输出，不写文件。

禁止输出或执行：

```text
任何持久化审查文件
任何审查状态字段
流程 verdict 或 closure / gate 语义
下一步建议、route 或 next owner
authority proposal
governance follow-up
文件修改、状态修改、proposal 写入
```

最小输出 contract：

```md
## Scope

## Review Maturity

## Evidence Reviewed
| Source | Why Included | Trust / Freshness |
|---|---|---|

## Evidence Not Reviewed
| Source | Reason | Impact |
|---|---|---|

## Assessment
No material issue found / Material issues found / Insufficient evidence

## Findings

### Critical

### Important

### Minor

## Evidence Gaps
```

可选章节只在相关时出现：

```text
Test Coverage Notes
Documentation Governance Notes
Architecture / Security Notes
```

不固定输出 `Strengths`。只有正向事实会影响判断时，才在 evidence 或 assessment 中提及。

---

## 9. Assessment

Assessment 只有三种：

```text
No material issue found
Material issues found
Insufficient evidence
```

判定规则：

```text
存在 Critical 或 Important finding
  -> Material issues found

只有 Minor 或无 finding
  -> No material issue found

关键证据不足，无法判断是否存在 Critical / Important
  -> Insufficient evidence
```

`material issue` 只由 `Critical` 或 `Important` 构成，`Minor` 不算 material issue。

如果无 finding，必须限定在已审范围内：

```text
Findings: None within reviewed scope.
Evidence Gaps: None material within reviewed scope.
```

不能写：

```text
完全通过
可以关闭
没有问题
```

---

## 10. Severity

严重程度表达影响等级，不绑定修复流程。

```text
Critical
  影响安全、权限、隐私、计费、数据删除、public contract、核心正确性，
  或会使设计结论根本不成立。

Important
  影响需求完整性、测试可信度、架构边界、文档权威一致性，
  可能导致返工或误导后续实现。

Minor
  命名、表达、局部可维护性、低风险遗漏，
  不改变主要判断。
```

不要写：

```text
Critical (Must Fix)
Important (Should Fix)
Minor (Nice to Have)
```

---

## 11. Finding 格式

每条 finding 应可定位、可验证、可修正。

```text
- Severity:
- Location:
- Problem:
- Evidence:
- Impact:
- Required correction:
```

`Required correction` 描述问题解决条件，而不是 workflow 下一步。

允许：

```text
Required correction: 明确 source_of_truth，并把历史材料标记为 evidence。
Required correction: 提供相关测试结果，或明确记录替代验证证据。
```

禁止：

```text
流程阶段跳转。
创建新 case。
运行其他 workflow skill。
关闭或收尾判断。
```

---

## 12. 核心工作流

### Step 1. Resolve Target

确定评审对象和成熟度。

如果对象不明：

- 有唯一明显对象时，说明假设并审查。
- 多个候选或没有候选时，只问一个最小澄清问题。

### Step 2. Select Evidence

读取最小相关证据：

- 目标文档或 diff。
- 显式引用来源。
- 直接相关的 constitution、active authority、ADR、contract。
- 与目标事实相关的代码 / 测试。
- 用户提供的命令结果或运行证据。

记录已读证据和关键未读证据。不要做完整 inventory。

### Step 3. Review Against Claims

检查：

- 目标结论是否明确。
- 论据是否支撑结论。
- 假设是否未声明。
- 范围边界和非目标是否清楚。
- 验收标准或成功条件是否可验证。
- 事实来源是否符合 Fact Authority Order。
- 是否把历史、派生或草稿材料误当作当前事实。

### Step 4. Review By Object Type

按对象启用专项检查。

设计 / 方案：

- 用户问题和目标是否明确。
- 范围边界是否可执行。
- 关键决策是否有依据。
- 替代方案和取舍是否足够。
- 是否引入不必要复杂度。

代码 / diff：

- 是否符合明确需求或计划。
- 是否有范围扩大。
- 错误处理、边界条件、可维护性是否合理。
- 是否违反 authority、contract、ADR 或 public API 约束。

测试 / TDD：

- 是否覆盖失败路径和边界条件。
- 是否测试真实行为，而不是 mock 自身。
- mock 响应是否匹配真实结构。
- 是否只验证实现细节。
- TDD 证据是否与命令结果一致。
- 是否能防回归。

文档治理：

- 是否区分 authority、case evidence、derived、archive 和 scratch。
- 是否错误晋升未确认事实。
- 是否缺少 source_of_truth。
- 是否把 L3 / L4 材料当作当前事实。

高风险领域：

- security。
- auth / permission。
- privacy。
- billing。
- data deletion。
- public API / CLI / schema / config contract。
- migration。
- workflow / agent policy。

高风险检查只在目标涉及时启用，并输出对应 findings 或 evidence gaps。

### Step 5. Produce Assessment

按 severity 和 evidence gap 生成 assessment。

不要输出流程 verdict、路由、next owner、ready-to-close 或 proposal。

---

## 13. 子 Agent Review

如果用户明确要求“让子 agent review / independent review / code reviewer”，可以使用子 agent。

规则：

- 子 agent 只返回 findings。
- 子 agent 不写文件、不改状态、不路由。
- 主 agent 必须整合 findings，去重、分级，并确保输出仍符合本 skill 的最小 contract。
- 子 agent 未读到的证据必须标记 evidence gap。
- 子 agent 的结论不能自动变成 workflow gate。

---

## 14. 缺失证据处理

缺失输入按 evidence gap 处理。

如果没有 git diff：

- 记录 `Evidence Gap: diff unavailable`。
- 使用明确目标文件、用户提供片段或文档内容。
- 不从缺失 diff 推断行为变化。

如果没有 execution evidence：

- 不假设 TDD 已完成。
- 评估可用测试结果、命令输出或用户提供证据。
- 如果执行证据对判断关键，输出 evidence gap。

如果测试结果缺失：

- 不假设测试通过。
- 如果测试可信度对判断关键，输出 Important finding 或 `Insufficient evidence`。

如果 authority / contract 缺失：

- 不自动创建 proposal。
- 标记事实来源缺口或治理风险。

---

## 15. 与相邻 Skill 的关系

### docloom-workflow

- 负责识别用户是否明确请求 review。
- 负责消费 `review_risk` 信号。
- 可在状态摘要中暴露风险。
- 不基于 `review_risk` 自动触发审查。
- 所有 workflow 路由归 `docloom-workflow`。

### tdd-execute

- 只记录 `review_risk` 信号。
- 不输出审查建议。
- 不写审查状态字段。
- 不等待审查。
- 可把用户或外部审查发现作为普通返工输入处理，但不把它变成 workflow 状态。

### doc-sync-close

- 不以是否发生审查对话作为 closure 条件。
- 基于验收证据、测试结果、known risks、evidence gaps 和 `review_risk` 判断 closure 风险。
- authority proposal 和知识沉淀属于 doc-sync-close，不属于 review。

### setup-doc-governance

- authority / governance 变更 proposal 属于 setup-doc-governance 或 doc-sync-close。
- review 只能发现治理风险，不能写治理计划或 follow-up 表。

### grill

- grill 负责逐问挑战。
- review 负责证据审查。

---

## 16. Gate

- 用户未明确要求，不运行审查。
- 不生成中间产物。
- 不写持久化审查文件。
- 不修改任何文件。
- 不修改 `case_state.yaml`。
- 不读写审查状态字段。
- 不输出 workflow route。
- 不输出 next owner。
- 不输出流程 verdict 或 closure / gate 语义。
- 不输出 authority proposal 或 governance follow-up。
- 不把子 agent finding 自动变成流程状态。
- 不把缺失证据包装成通过。
- 不把 historical / derived / scratch 文档当成当前事实。

---

## 17. 验收标准

- 输出明确评审对象、成熟度和已审证据边界。
- 输出区分 Critical / Important / Minor。
- `Assessment` 与 severity 和 evidence gaps 一致。
- finding 包含 evidence、impact 和 required correction。
- 缺失关键证据时明确标记 evidence gap。
- 对 authority / governance 风险只做发现，不做 proposal。
- 不输出 route、next step、ready-to-close 或 workflow verdict。
- 不生成、修改或删除任何文件。

---

## 18. 参考资料

参考资料只作为审查质量输入，不继承其中的产物、路由、状态更新或 workflow gate。

- [requesting-code-review/SKILL.md](../../reference/skills/requesting-code-review/SKILL.md)
- [requesting-code-review/code-reviewer.md](../../reference/skills/requesting-code-review/code-reviewer.md)
- [test-driven-development/testing-anti-patterns.md](../../reference/skills/test-driven-development/testing-anti-patterns.md)
- [AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制](<../../reference/docs/AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制.md>)
- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
