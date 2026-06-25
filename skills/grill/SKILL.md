---
name: grill
description: Interactively pressure-test a claim. Use only when the user explicitly asks to grill, challenge, scrutinize, or question assumptions.
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
- Decision question: ask one sharp question, give 2-3 meaningful options, and
  recommend one with a short reason. Include conditions inline when they matter.

From Question 2 onward, start by restating the previous choice in one sentence.

Question format:

```md
Previous: Q<N-1> = <user choice>. This means <one-sentence restatement>.

Q<N>: <one sharp question>

A. <option>
B. <option>
C. <option>

Recommended: <A/B/C>, because <one short reason; include a condition or
alternative only if needed>.
```

Short answers such as "yes", "use the recommendation", or "continue" confirm the
current recommendation only when unambiguous. A/B/C answers confirm that option.
They do not become long-term authority facts.

## Converge

Walk the decision tree until the user ends or the key branches have converged.
Then output a light close:

```md
Confirmed:
- <decision>

Open:
- <question>

Risks:
- <weak assumption>
```

Do not promise to write these into a file. Later workflow skills may consume
explicitly confirmed decisions in their own artifacts.

## High-Risk Topics

High risk includes security, auth, permission, privacy, billing, data deletion,
public API, CLI, schema, config contract, migration, ADR-protected architecture
boundaries, and workflow or agent policy change.

For high-risk recommendations:

- Lower certainty; state evidence sources and unverified points.
- Treat short user answers as current discussion decisions only, and do not
  phrase unverified conclusions as facts.

## Gates

- User must explicitly ask for grilling.
- Ask one question at a time.
- Verify answerable facts before asking the user.
- Do not modify files or state.
- Do not turn recommendations into user decisions without confirmation.
