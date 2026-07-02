# Context & Authority Brief

## User Request

Use a real case to dogfood the Doc Loom Least development workflow after resolving
the Fast-Path confirmation-policy conflict.

## Intent

- Type: Workflow / skill design dogfood.
- Reason: The task touches workflow and agent policy, authority docs, shared
  protocol, and stage skill gates.
- Minimum Evidence Needed: Current authority docs, shared protocol,
  risk-level policy, affected stage skills, governance batch blocker, current
  git baseline, dirty workspace diff, and verification command output.

## Workspace Snapshot

- Workspace: `/Users/yomi/Documents/project/doc-loom-least`
- Branch: `main`
- Worktree: primary workspace
- Git Available: true
- Dirty Workspace: true
- Base Commit: `e3f7c2e8d6b445ac41d6a52419a73dd45a53bbc5`
- Changed Files Summary:
  - `docs/authority/README.md`
  - `docs/authority/workflow/development-flow.md`
  - `docs/governance/2026-07-02-full-repo-docs-governance.md`
  - `docs/share/workflow-and-design.md`
  - `skills/_shared/references/shared-protocol.md`
  - `skills/development/doc-sync-close/SKILL.md`
  - `skills/development/docloom-workflow/SKILL.md`
  - `skills/development/plan-confirm/SKILL.md`
  - `skills/development/plan-confirm/references/risk-levels.md`
  - `skills/development/plan-confirm/templates/plan.md`
  - `skills/development/tdd-execute/SKILL.md`

## Case Context

- Existing Case: none
- Proposed Case Slug: `fast-path-policy-dogfood`
- Case State: new recovery dogfood case

## Sources Read

| Source | Layer / Type | Why Included | Trust / Freshness |
|---|---|---|---|
| `docs/authority/README.md` | L1 authority | Current authority index and previously blocked Fast-Path fact. | Active, modified in current diff. |
| `docs/authority/workflow/development-flow.md` | L1 authority | Current development workflow authority and confirmation policy. | Active, modified in current diff. |
| `docs/governance/2026-07-02-full-repo-docs-governance.md` | Governance batch | Records the original Fast-Path policy blocker and applied result. | Active governance record, modified in current diff. |
| `skills/_shared/references/shared-protocol.md` | Shared runtime protocol | Owns Fast-Path, artifact policy, case state, and confirmation semantics. | Current implementation, modified in current diff. |
| `skills/development/plan-confirm/references/risk-levels.md` | Runtime reference | Owns risk levels and plan confirmation policy. | Current implementation, modified in current diff. |
| `skills/development/docloom-workflow/SKILL.md` | Runtime skill | Owns case identity, routing, and Fast-Path route. | Current implementation, modified in current diff. |
| `skills/development/plan-confirm/SKILL.md` | Runtime skill | Owns plan approval, user confirmation, and Fast-Path approval handling. | Current implementation, modified in current diff. |
| `skills/development/tdd-execute/SKILL.md` | Runtime skill | Owns approved-plan preflight and execution evidence. | Current implementation, modified in current diff. |
| `skills/development/doc-sync-close/SKILL.md` | Runtime skill | Owns closure and state update behavior. | Current implementation, modified in current diff. |
| `docs/share/workflow-and-design.md` | L3 overview | Derived public explanation needed sync with authority/runtime. | Derived, modified in current diff. |

## Sources Excluded

| Source | Reason |
|---|---|
| `docs/archive/**` | Historical evidence only; not current authority. |
| Full external reference corpus | Not needed to resolve or dogfood this concrete workflow-policy case. |
| Code tests | Repository has no runtime source tree or test suite; verification is command/text based. |

## Authority / Constraints

- ADR-0000 and authority docs require the smallest useful workflow path.
- ADR-0002 requires agents to own discoverable/executable work when no real
  human decision is needed.
- High-Risk Topics include workflow or agent policy change.
- The dogfood case must not hide that the policy patch was implemented before
  this case was created.

## Relevant Code / Tests

No runtime source tree or automated test suite exists. Relevant implementation
is the skill runtime text and shared references under `skills/**`.

## Conflicts / Unknowns

| Topic | Conflict / Unknown | Risk | Required Decision |
|---|---|---|---|
| Pre-plan execution order | The Fast-Path policy patch was completed before this case was created. | Medium | Record as recovery dogfood caveat, not as a clean planned execution. |
| Independent review | No separate `review` pass was requested. | Low | Do not auto-trigger review; record residual review risk. |

## Route Verdict

- Verdict: `proceed_to_plan_with_risk`
- Reason: Context is sufficient and the previous Fast-Path blocker is resolved,
  but this case is binding an already-produced dirty diff.
- Required Next Owner: `plan-confirm`
