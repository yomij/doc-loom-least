---
status: active
authority: true
layer: authority
type: operations
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-09
---

# Git Commit Titles

Except for Git-generated messages, commit titles use:

type: summary

The type is lowercase and one of feat, fix, docs, chore, refactor, style,
test, perf, or revert. The summary identifies the business object and action.

Commit organization is an execution choice inside the authorized outcome.
Ordinary task bookkeeping does not require a commit. A commit is a completion
gate only when user or project policy, or an explicit task constraint, requires
it. Publication, push, merge, release, and history rewriting remain separate
actions requiring authorization.
