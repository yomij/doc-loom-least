# grill Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `grill` 如何对需求、上下文、计划、架构、测试策略和文档影响做手动压力测试，并输出可执行的挑战报告。

---

## 1. 定位

`grill` 是用户手动触发的拷问 / 压力测试 skill。

它不属于默认主流程，不自动运行。它只挑战和暴露问题，不执行实现、不改代码、不自动修改计划。

核心规则：

```text
grill 只能由用户手动触发。
grill 后如果计划变化，必须回到 plan-confirm。
每次只问一个问题；如果问题可由代码或文档回答，先探索项目而不是问用户。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: grill
description: Challenge and stress-test a requirement, context brief, implementation plan, architecture proposal, test strategy, or documentation update. Use only when the user explicitly asks to grill, challenge, pressure-test, scrutinize, or aggressively review assumptions before proceeding.
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
- `tdd-execute` 建议 review。

高风险时可以建议用户触发 `grill`，但不能擅自执行。

---

## 4. 可 Grill 对象

- 用户需求。
- `Context & Authority Brief`。
- `plan.md`。
- 架构方案。
- 文档更新方案。
- 测试策略。
- Authority 事实变更 proposal。

如果用户没有明确对象：

- 优先 grill 当前 `plan.md`。
- 如果没有 plan，grill 当前需求和 context brief。

---

## 5. 输入

- 用户指定的 grill 目标。
- 用户本轮需求。
- `context-authority-brief.md`，如果存在。
- `plan.md`，如果存在。
- 相关 authority 文档。
- 相关代码和测试，必要时读取。

---

## 6. 输出

有 case docs 时写入：

```text
docs/cases/<case-id>/grill.md
```

没有 case docs 时直接在对话中输出报告。

模板：

```md
# Grill Report

## Target

## Current Claim

## Ambiguities

## Hidden Assumptions

## Edge Cases

## Possible Regressions

## Missing Tests

## Documentation Risks

## Questions for User

## Suggested Plan Changes

## Verdict
Proceed / Revise Plan / Blocked
```

---

## 7. 推荐资源结构

```text
grill/
  SKILL.md
  templates/
    grill-report.md
  references/
    challenge-prompts.md
```

---

## 8. 核心工作流

### Step 1. Identify Target

确认 grill 对象：

```text
Requirement
Context Brief
Plan
Architecture
Test Strategy
Documentation Update
Authority Change
```

如果对象不存在：

- 不凭空假设。
- 先说明缺少对象，并基于可用输入做轻量 grill。

### Step 2. Restate Current Claim

先中性复述：

- 当前方案想达成什么。
- 依赖哪些证据。
- 会影响哪些边界。
- 当前计划假设了什么。

避免上来直接反驳导致误解目标。

### Step 3. Challenge Requirements

检查：

- 用户问题是否清楚。
- 成功标准是否可验证。
- 非目标是否明确。
- 是否存在未说出的兼容性约束。
- 是否有时间、成本、依赖团队、合规限制。

提问方式：

- 一次只问一个问题。
- 每个问题都给出推荐答案和理由。
- 如果存在分支决策，沿决策树逐一走完，不把多个未决点揉成一团。

### Step 4. Challenge Plan

检查：

- 是否解决根因，而不是症状。
- 是否范围过大或过小。
- 是否可回滚。
- 是否有计划漂移风险。
- 风险等级是否被低估。
- 文件影响是否遗漏。

### Step 5. Challenge Testing Strategy

检查：

- 是否覆盖失败路径。
- 是否覆盖边界条件。
- 是否只测 happy path。
- 是否过度依赖实现细节。
- TDD exception 是否合理。
- 替代验证是否足够。

### Step 6. Challenge Documentation Impact

检查：

- 是否需要 authority 更新。
- 用户本轮新事实是否被错误当作 accepted。
- L3 是否可能与 authority 冲突。
- historical docs 是否被误当作当前事实。
- doc sync 是否遗漏验收或风险记录。

### Step 7. Challenge Architecture and Security

检查：

- 是否引入不必要抽象。
- 是否改变 public contract。
- 是否有隐式数据迁移。
- 是否影响权限、安全、认证、计费、隐私。
- 是否缺少 rollback / observability。

### Step 8. Produce Verdict

三种 verdict：

```text
Proceed
  没有阻塞问题，可以继续；可带非阻塞建议。

Revise Plan
  计划应改版，必须回到 plan-confirm。

Blocked
  缺少关键事实或用户决策，不能继续。
```

---

## 9. 问题分级

每个关键问题建议包含：

```text
- Severity: blocking | non-blocking
- Problem:
- Evidence:
- Impact:
- Recommendation:
```

输出应聚焦高价值问题，不罗列低价值噪音。

## 9.5 交互模式

`grill` 可以有两种输出模式：

```text
interactive
  连续问用户问题，直到关键分支收敛。

report
  直接输出一次性 Grill Report。
```

默认：

- 用户说“grill 我”“拷问我”时，用 interactive。
- 用户说“出一份 grill report”或异步审查时，用 report。

Interactive 每轮格式：

```md
## Question N

<one question>

Recommended answer:
<recommended answer>

Why this matters:
<impact>
```

当所有 blocking 分支收敛后，再写 `grill.md` 总结。

---

## 10. Gate

- 不改 `plan.md`。
- 不执行实现。
- 不修改代码。
- 不把建议自动当作计划变化。
- 如果 verdict 是 `Revise Plan` 或 `Blocked`，不能继续 `tdd-execute`。
- 如果 grill 后计划变化，必须回到 `plan-confirm` 并重新确认。

---

## 11. 验收标准

- 报告能指出最关键的歧义、假设和测试缺口。
- 每个 blocking 问题都有影响和建议。
- verdict 明确。
- 没有把低价值问题堆成噪音。
- 如果建议改计划，明确说明需要回到 `plan-confirm`。

---

## 12. 失败与恢复

如果输入材料不足：

- 明确列出缺失材料。
- 基于现有材料给条件性 grill。
- 不输出过度确定结论。

如果用户只想要轻量挑战：

- 输出 3 到 5 个最高价值问题。
- 不强行套完整模板。

如果 grill 发现 authority / 代码冲突：

- 标记为 blocking。
- 建议回到 `context-authority` 或 `setup-doc-governance`。

---

## 13. 参考资料

- [grill-me/SKILL.md](../../reference/skills/grill-me/SKILL.md)
- [writing-plans/SKILL.md](../../reference/skills/writing-plans/SKILL.md)
- [AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制](<../../reference/docs/AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制.md>)
- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
