# review Skill 详细设计

> 来源：`docs/design/document-driven-workflow-skills-final.md`
>
> 目标：定义 `review` 如何在用户明确要求时审查实现、测试、文档、架构和安全风险，并输出 blocking / non-blocking findings 与 verdict。

---

## 1. 定位

`review` 是用户手动触发 / 可选的审查 skill。

它不属于默认强制流程。agent 可以基于风险建议 review，但不能把建议等同于用户授权。

核心规则：

```text
review 只能由用户手动触发或明确要求。
review 发现 blocking issue 后，必须回到 tdd-execute。
review 必须基于工作产物和证据，不基于当前会话印象。
```

---

## 2. 建议 Frontmatter

```yaml
---
name: review
description: Review completed or in-progress work in the document-driven workflow. Use only when the user explicitly asks for review, code review, subagent review, test review, documentation review, or implementation inspection. Checks plan conformance, tests, authority docs, architecture, security, scope creep, and outputs blocking/non-blocking findings plus a verdict.
---
```

---

## 3. 触发条件

只在用户明确要求时触发，例如：

- “review 一下”
- “做 code review”
- “让子 agent review”
- “检查这次实现”
- “审查测试覆盖”
- “看一下有没有破坏文档权威”

不应该因为以下情况自动触发：

- `risk_level = high`。
- `tdd-execute` 给出 review recommendation。
- agent 主观觉得需要再看一眼。

这些情况只能建议用户触发 review。

---

## 4. 输入

- `plan.md`。
- git diff 或变更文件列表。
- 测试结果。
- 相关 authority 文档。
- `case_state.yaml`。
- 用户指定的 review 范围。
- 可选：base/head commit 或 plan 的 `base_commit`。

可选输入：

- `execution.md`，如果 Artifact Policy 生成了执行报告。
- `context-authority-brief.md`。
- `closure.md` 中的验证证据，如果 review 发生在收尾后。
- 已有 `review.md`。

---

## 5. 输出

```text
docs/cases/<case-id>/review.md
```

模板：

```md
# Review Report

## Scope

## Inputs Reviewed
- plan.md
- execution.md, if present
- git diff
- test results
- authority docs

## Strengths

## Issues

### Critical (Must Fix)

### Important (Should Fix)

### Minor (Nice to Have)

## Test Coverage Notes

## Documentation Notes

## Documentation / Knowledge Governance Notes

## Architecture / Security Notes

## Recommendations

## Assessment
- Ready to close: Yes / No / With fixes
- Reasoning:

## Verdict
Approved / Needs Changes
```

---

## 6. 推荐资源结构

```text
review/
  SKILL.md
  templates/
    review.md
  references/
    review-rubric.md
```

---

## 7. Review Scope

如果用户指定范围，按用户范围审查。

常见范围：

```text
full case
code diff
tests only
docs only
security
architecture
public API
plan conformance
```

如果用户未指定，默认审查当前任务全部变更：

- 计划符合度。
- 代码变更。
- 测试覆盖。
- 文档影响。
- 架构 / 安全风险。

如果 git 可用，应优先用明确范围审查：

```bash
git diff --stat <base>..<head>
git diff <base>..<head>
```

如果不是 git 仓库，使用变更文件列表、执行记录和测试结果替代，并在报告中说明证据限制。

---

## 8. 核心工作流

### Step 1. Load Inputs

读取：

- `plan.md`。
- `execution.md`。
- 变更文件和 diff。
- 测试结果。
- 相关 authority 文档。

如果缺少关键输入：

- 记录缺失项。
- 能审多少审多少。
- 不输出过度确定结论。

### Step 2. Compare Against Plan

检查：

- 是否实现了 acceptance criteria。
- 是否遵守 `plan.md ## Decisions` 中用户确认的决策。
- 是否修改了未计划文件。
- 是否绕过 TDD。
- 是否存在未记录偏离。
- 风险等级是否被低估。
- 是否产生新的 authority 文档影响。

### Step 3. Inspect Tests

检查：

- 是否有失败路径。
- 是否覆盖边界条件。
- 是否只验证实现细节。
- 是否能防回归。
- 是否覆盖关键用户决策对应的行为或风险。
- TDD exception 的替代验证是否足够。
- 命令结果是否真实记录。

### Step 4. Inspect Authority Docs

检查：

- 实现是否违反 authority。
- 是否应提出 authority 更新。
- 是否错误更新了 authority。
- L3 派生文档是否 stale。
- historical docs 是否被误用为当前事实。

### Step 5. Inspect Architecture and Security

按风险聚焦：

- public contract。
- auth / permission。
- data deletion。
- privacy。
- billing。
- migration。
- error handling。
- observability。
- rollback。

### Step 6. Produce Findings

问题分级：

```text
Critical
  数据丢失、安全漏洞、权限/隐私/计费错误、破坏核心功能，必须立即修。

Important
  架构问题、缺失需求、错误处理不足、明显测试缺口，继续前应修。

Minor
  可维护性、命名、文档补充、轻微优化，不阻塞但应记录。
```

每条 finding 包含：

```text
- Severity:
- File / Section / Line:
- Problem:
- Evidence:
- Impact:
- Recommendation:
```

### Step 7. Produce Verdict

两种 verdict：

```text
Approved
Needs Changes
```

如果 `Needs Changes`：

- 更新 `case_state.yaml` 的 `review_status` 为 `needs_changes`。
- 下一步回到 `tdd-execute`。

如果 `Approved`：

- 更新 `review_status` 为 `approved`。
- 下一步进入 `doc-sync-close`。

---

## 9. Findings 输出规范

优先级：

1. 数据丢失、安全、权限、隐私、计费。
2. public API / schema migration / breaking change。
3. 验收标准未满足。
4. 测试缺口导致明显回归风险。
5. 文档权威冲突。
6. 可维护性建议。

不要把所有建议都写成 blocking。

示例：

```md
### Blocking

- Severity: High
- File / Section: src/auth/refresh.ts
- Problem: Refresh token reuse is accepted after rotation.
- Evidence: The new test only covers successful refresh and does not assert old token invalidation.
- Impact: This weakens the documented auth invariant.
- Recommendation: Add a failing test for old-token reuse and reject reused refresh tokens.
```

报告格式应使用：

```md
## Strengths

## Issues

### Critical (Must Fix)

### Important (Should Fix)

### Minor (Nice to Have)

## Recommendations

## Assessment

**Ready to close?** Yes / No / With fixes

**Reasoning:**
```

`Verdict` 与 `Ready to close` 的关系：

- 有 Critical 或 Important：`Needs Changes`。
- 只有 Minor：可以 `Approved` 或 `Approved with Caveats`，由用户决定是否返工。
- 无问题且验收满足：`Approved`。

---

## 10. 文档与知识治理审查

除了代码审查，必须检查：

- 是否有用户可见行为变化但未更新 acceptance criteria 或测试。
- 是否有 `plan.md ## Decisions` 中的长期或契约相关决策未进入文档影响审查。
- 是否有 API/schema/错误码变化但未更新 contract / changelog / examples。
- 是否有新知识被直接写入 authority，但缺少 evidence 或必要确认。
- 是否有局部经验被错误提升为全局规则。
- 是否有需要进入 `GOVERNANCE_PLAN.md` follow-up 的规则、反模式、runbook 或 regression test。
- 是否有 authority 更新 proposal 未确认。

如果发现知识晋升/降级问题：

- 不直接改文档。
- 在 review 中输出 `Documentation / Knowledge Governance Notes`。
- 建议回到 `doc-sync-close` 或 `plan-confirm`，视问题发生阶段决定。

---

## 11. 与 Review Recommendation 的关系

`tdd-execute` 可以输出：

```md
## Review Recommendation

- Recommended: Yes
- Reason:
  - risk_level = high
```

但这不是授权。

只有用户明确说要 review 时，才运行 `review`。

---

## 12. Gate

- 用户未明确要求，不执行 review。
- review 不自动修改代码。
- review 不自动修改 plan。
- review 发现 blocking issue 后，不进入 `doc-sync-close`。
- verdict 为 `Needs Changes` 时，必须回到 `tdd-execute`。
- 不审查未读取的 diff、文件或测试结果；证据缺失必须写明。
- Critical / Important issue 不能被降级为 follow-up。

---

## 13. 验收标准

- Findings 以高风险问题优先。
- 每条 blocking issue 可执行、可定位、可验证。
- verdict 明确。
- 不把无关重构建议当作阻塞。
- 明确说明测试覆盖和文档影响。
- 报告包含 Strengths、Issues、Recommendations 和 Assessment。
- 明确 `Ready to close?` 判断。

---

## 14. 失败与恢复

如果没有 git diff：

- 使用变更文件列表、`execution.md` 或 `closure.md` 中的验证证据。
- 记录 diff 不可用。

如果没有 `execution.md`：

- 判断它是否按 Artifact Policy 可以省略。
- 使用 git diff、测试结果、closure evidence 或用户提供的命令结果替代。
- 不能假设 Red / Green / Refactor 已完成；证据不足时必须标记 review risk。

如果测试结果缺失：

- 标记为 review risk。
- 不假设测试通过。

如果发现计划与实现严重不一致：

- verdict 为 `Needs Changes`。
- 建议回到 `plan-confirm` 或 `tdd-execute`，视偏离性质决定。

---

## 15. 参考资料

- [requesting-code-review/SKILL.md](../../reference/skills/requesting-code-review/SKILL.md)
- [requesting-code-review/code-reviewer.md](../../reference/skills/requesting-code-review/code-reviewer.md)
- [test-driven-development/testing-anti-patterns.md](../../reference/skills/test-driven-development/testing-anti-patterns.md)
- [AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制](<../../reference/docs/AI Coding 文档治理中的知识识别、分层流转、晋升与降级机制.md>)
- [AI Coding 文档治理原则](<../../reference/docs/AI Coding 文档治理原则.md>)
