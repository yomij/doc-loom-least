# Context & Authority Brief

## User Request

Integrate loop-engineering semantics into Doc Loom skills themselves, rather
than adding an external root-level loop scaffold.

## Intent

- Type: Workflow / Agent Skill design.
- Reason: The task changes how Doc Loom skills route, resume, and record
  repeated work.
- Minimum Evidence Needed: constitution, product scope, development workflow,
  agent policy, shared protocol, current skill implementation, and existing case
  artifact policy.

## Workspace Snapshot

- Workspace: `/Users/yomi/Documents/project/doc-loom-least`
- Branch: `main`
- Git Available: true
- Dirty Workspace: no, before this case was created
- Base Commit: `b5bb4a3093cda0ccf7f7d54eb42680faeb591e75`

## Sources Read

| Source | Layer / Type | Why Included |
|---|---|---|
| `docs/authority/constitution.md` | authority / constitution | Defines minimum path, no complex pipeline, human-first rules. |
| `docs/authority/product/scope.md` | authority / product | Defines Doc Loom as repo-native, skill-based, Markdown-first substrate. |
| `docs/authority/workflow/development-flow.md` | authority / workflow | Owns current development skill workflow and confirmation policy. |
| `docs/authority/agent/policy.md` | authority / agent | Defines high-risk topics and agent responsibility. |
| `docs/authority/architecture/repo-and-skills.md` | authority / architecture | Defines canonical skill groups and invocation names. |
| `skills/_shared/references/shared-protocol.md` | implementation protocol | Owns case state, artifact policy, routing, Fast-Path, and confirmation semantics. |
| `skills/development/docloom-workflow/SKILL.md` | skill implementation | Owns routing and case identity. |
| `skills/development/context-authority/SKILL.md` | skill implementation | Owns conditional context gate. |
| `skills/development/plan-confirm/SKILL.md` | skill implementation | Owns planning and approval gate. |
| `skills/development/tdd-execute/SKILL.md` | skill implementation | Owns approved execution and evidence. |
| `skills/development/doc-sync-close/SKILL.md` | skill implementation | Owns closure and sync. |
| `skills/assessment/review/SKILL.md`, `skills/assessment/grill/SKILL.md` | skill implementation | Manual-only assessment helpers that must not become automatic loop stages. |

## Sources Excluded

| Source | Reason |
|---|---|
| `docs/archive/**` | Historical evidence only; not current authority. |
| Runtime tests | Repository has no runtime source tree or test suite. |

## Authority Constraints

- Do not add a CLI backend, daemon, centralized orchestrator, or complex
  pipeline.
- Do not create empty future lifecycle domains.
- Do not make users do bookkeeping the agent can perform.
- Interrupt users only for real decisions, external facts, explicit
  authorization, preferences, or high-risk irreversible actions.
- `review` and `grill` remain manual-only.
- High-risk workflow/agent policy changes require explicit confirmation.

## Route Verdict

- Verdict: `proceed_to_plan_with_risk`
- Reason: Context is sufficient, but the change touches workflow/agent policy
  and shared skill semantics.
- Required Next Owner: `plan-confirm`
