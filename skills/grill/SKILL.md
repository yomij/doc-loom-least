---
name: grill
description: Interactively challenge and stress-test a requirement, plan, architecture proposal, implementation idea, test strategy, documentation change, or other claim. Use only when the user explicitly asks to grill, challenge, pressure-test, scrutinize, or aggressively question assumptions. Ask one question at a time, provide a recommended answer with conditions, verify facts from code or docs when possible, and do not create artifacts or modify files.
---

# grill

Pressure-test a claim through conversation. Use only when the user explicitly
asks to grill, challenge, scrutinize, pressure-test, or aggressively question
assumptions.

Do not create files, reports, case artifacts, workflow verdicts, or state
changes.

Read `references/challenge-prompts.md` only when you need prompt inspiration.

## Start

Restate in 1-3 neutral sentences:

- What object is being challenged.
- What the current claim is.
- What boundaries are known.

If the object is unclear, ask one clarification question.

## Question Loop

Choose the highest-leverage question. Do not mechanically cover a checklist.

Separate fact from decision:

- Fact question: check code, tests, or docs first when possible. Report evidence
  or evidence gap.
- Decision question: ask one question, give a recommended answer, and state the
  conditions that make it valid.

Question format:

```md
## Question N

<one question>

Recommended answer:
<clear recommendation, with conditions>

Why this matters:
<impact>
```

Short answers such as "yes", "use the recommendation", or "continue" confirm
the current recommendation only when unambiguous. They do not become long-term
authority facts.

## Converge

Walk the decision tree until the user ends or the key branches have converged.
Then output a light close:

```md
## Confirmed Decisions

## Open Questions

## Risks / Weak Assumptions
```

Do not promise to write these into a file. Later workflow skills may consume
explicitly confirmed decisions in their own artifacts.

## High-Risk Topics

High risk includes security, auth, permission, privacy, billing, data deletion,
public API, CLI, schema, config contract, and high-impact architecture
boundaries.

For high-risk recommendations:

- Lower certainty.
- State evidence sources and unverified points.
- Treat short user answers as current discussion decisions only.
- Do not phrase unverified conclusions as facts.

## Gates

- User must explicitly ask for grilling.
- Ask one question at a time.
- Verify answerable facts before asking the user.
- Do not generate `grill.md`, a report artifact, route, next owner, or closure
  verdict.
- Do not modify files.
- Do not turn recommendations into user decisions without confirmation.
