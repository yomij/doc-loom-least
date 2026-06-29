# Retrieval Routing

Read by relevance. Do not build a full context pack.

## Governance Docs

Read governance docs by priority:

1. `docs/README.md` when a docs entry or authority route is needed.
2. `docs/cases/<case-id>/governance-plan.md` when a case is active.
3. Most recent `docs/governance/*.md` with `status: approved` or
   `status: applied_with_blocks`.
4. Any `applied_with_blocks` plan when blocked-decision context is needed.
5. Legacy `docs/governance/GOVERNANCE_PLAN.md` as context only.

Use these when the task involves documentation governance, authority update,
workflow or agent policy, public contract, High-Risk Topic, new user facts that
may touch blocked decisions, or missing authority/conflict context.

If governance or authority is missing:

- Low-risk local work may continue with `proceed_to_plan_with_risk`.
- High-Risk Topic or public contract work should route to
  `run_setup_doc_governance` or `needs_user_decision`.

## Authority Filtering

Include active authority docs only by default.

Exclude:

- `status: archived`
- `status: superseded`
- `source_of_truth: generated` as a fact source

Generated docs can be consumed as views only.

## Case Docs

Read existing case docs only when resuming or when the case is explicit:

```text
case_state.yaml
plan.md
handoff.md
execution.md
closure.md
```

For `case_state.yaml` conflicts, see shared protocol `case_state.yaml`.

## Code And Tests

Read code/tests when the intent requires them. Use:

- User keywords.
- Authority frontmatter `related_code` and `related_tests`.
- `rg` for symbols, file names, errors, config keys.
- Test naming conventions and nearby tests.

Prefer exact identifiers and direct call/test paths before semantic browsing.
Every included source must have `why_included`.

## Context Source Notes

Use this shape inline or in the brief:

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

Runtime evidence, logs, CI results, or user-provided outputs need a time window
and trust note.
