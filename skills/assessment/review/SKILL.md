---
name: review
description: Run a separate read-only verification pass on code, diffs, plans, or documents and return prioritized findings with a three-state verdict. Use when the user requests a review or the development workflow triggers an independent check, without modifying files or task state.
---

# Review

Use the user's language for all output, including templates, unless another language is requested.

Independence means a separate verification pass, not another agent. Use
subagents only when explicitly requested by the user or required by the runtime.
Do not modify the target, repository files, or task state; the executor owns
fixes and durable evidence.

## Inputs

Recover target, goal, success conditions, constraints, and delta from available
context. Identify the baseline for change reviews; whole-target reviews need
none. Ask only when target or requirement ambiguity still prevents assessment
after inspection; otherwise report gaps. Do not invent a baseline or include
unrelated work.

## Checks

Assess correctness, requirements, compatibility, verification evidence, and
removable complexity within the requested scope. Follow declared repository
fact ownership; historical or derived prose does not override active rules.
Report unresolved authority conflicts rather than silently choosing a rule.

Run only read-only checks that leave repository and external state unchanged
(using isolated temporary paths if needed), or report which checks could not
run. Read [references/complexity-only.md](references/complexity-only.md)
**conditionally**, only for an explicitly requested complexity-only assessment.
Stop once the target and outcome-affecting questions are covered; do not hunt
indefinitely for minor polish.

## Findings

For each finding provide location, evidence, impact, correction, and severity:

- **Critical**: clearly wrong delivery, a serious security exposure, data
  corruption/loss, a major contract violation, or an unsafe delivery.
- **Important**: a correctness, requirement, compatibility, maintainability, or
  verification defect that needs correction before completion. Explain the
  concrete failure or maintenance burden; preference alone is not a defect.
- **Minor**: an actionable improvement that does not block completion.

Return all Critical/Important findings found within scope and only actionable,
non-duplicative Minor findings. Combine shared causes/corrections; state when
none were found.

## Result

Return exactly one verdict, applying these rules in order:

1. `changes_required`: at least one unresolved Critical or Important finding.
2. `insufficient_evidence`: no known blocking finding, but missing target,
   context, or verification evidence prevents a reliable conclusion.
3. `pass`: no unresolved Critical or Important finding and no evidence gap
   preventing the scoped conclusion. Minor findings do not prevent pass.

Always report scope, baseline/delta, executed checks, and evidence gaps
alongside the verdict. Pass confirms the scoped verification only; it grants no
merge or publication authority.

## Escalation

A request to fix findings moves execution to docloom-workflow. Structural
policy or authority decisions belong to setup-doc-governance; neither permits
mutations during this review.
