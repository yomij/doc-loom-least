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
- Decision question: ask one sharp question. Give 2-3 meaningful options and a
  recommendation only when the choices are genuinely discrete. For exploratory
  or definition questions, invite a free-form answer and offer one concise
  framing or example instead.

From Question 2 onward, start by restating the previous choice in one sentence.

Discrete decision format:

```md
Previous: Q<N-1> = <user choice>. This means <one-sentence restatement>.

Q<N>: <one sharp question>

A. <option>
B. <option>
C. <option>

Recommended: <A/B/C>, because <one short reason; include a condition or
alternative only if needed>.
```

Exploratory format:

```md
Previous: <omit for Q1; otherwise one-sentence restatement>.

Q<N>: <one open, high-leverage question>

Framing: <one short example, boundary, or tension that helps answer>.
```

Short answers such as "yes", "use the recommendation", or "continue" confirm the
current recommendation only when unambiguous. A/B/C answers confirm that option;
free-form answers are summarized neutrally before the next question. None become
long-term authority facts.

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

Treat security, auth, permission, privacy, billing, data deletion, public
API/CLI, schema, config contract, migration, ADR-protected architecture, and
workflow or agent policy as risk-sensitive. Classify by consequence and
reversibility; workflow or agent policy text is not automatically high unless
it weakens authorization, changes public/L1 facts, or enables irreversible
action.

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
