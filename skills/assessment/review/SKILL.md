---
name: review
description: Perform a separate read-only verification pass on existing work when review is requested or a development change requires it. Return findings and a readiness result; do not implement fixes or require another agent.
---

# Review

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
Run checks only without changing repository or external state; use isolated
temporary outputs where needed, or report the unavailable check.

Read [references/complexity-only.md](references/complexity-only.md)
**conditionally**, only for an explicitly requested complexity-only assessment.
Stop after covering the target and questions affecting the result; do not keep
searching for minor issues.

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

Return exactly one result, applying these rules in order:

1. `changes_required`: at least one unresolved Critical or Important finding.
2. `insufficient_evidence`: no known blocking finding, but missing target,
   context, or verification evidence prevents a reliable conclusion.
3. `pass`: no unresolved Critical or Important finding and no evidence gap
   preventing the scoped conclusion. Minor findings do not prevent pass.

Include scope, baseline/delta, checks, and gaps even when findings determine the
result. These complete the review; pass grants no merge/publication authority.

## Escalation

A request to fix findings moves execution to docloom-workflow. Structural
policy or authority decisions belong to setup-doc-governance; neither permits
mutations during this review.
