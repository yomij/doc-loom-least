---
status: applied_with_blocks
plan_version: 1
scope: full-repo
governance_batch: 2026-07-02-full-repo-docs-governance
approved_by: user
approved_at: 2026-07-02T18:53:19+08:00
---

# Governance Plan

## Scope

This batch governs `/Users/yomi/Documents/project/doc-loom-least/docs` and uses
`full-repo` scope because the owner explicitly asked to read implementation and
documentation to verify facts.

Implementation evidence is read-only. This repository currently has no
runtime source tree, package manifest, or test suite; its executable product
surface is the tracked Agent Skills under `skills/**`, their templates,
references, symlinked shared protocol, `.skillignore`, and agent adapter files.

Workspace baseline:

| Item | Evidence |
|---|---|
| Branch | `main` |
| HEAD | `fa3a7ba` |
| Docs files scanned | 43 Markdown files under `docs/` |
| Skill implementation files scanned | 8 `SKILL.md` files, shared references, templates |
| Dirty worktree at plan time | `CLAUDE.md` is modified outside this plan; current diff changes `./AGETNO.md` to `./AGENT.md`, while the tracked repo contains `AGENTS.md` |

This plan write is not approval to create authority docs, move files, archive
documents, update indexes, or repair adapters. Application requires one explicit
confirmation of this plan.

## Evidence Inventory

| Source | Current Role | Current vs Historical Verdict |
|---|---|---|
| `docs/authority/constitution.md` | Active constitution | Current highest authority for principles, platform meaning, and conflict handling. |
| `docs/adr/ADR-0001-lifecycle-scope-and-skill-grouping.md` | Accepted ADR | Current authority for lifecycle scope and skill grouping. |
| `docs/adr/ADR-0002-human-first-agent-responsibility.md` | Accepted ADR | Current authority for human-first agent responsibility. |
| `skills/_shared/references/shared-protocol.md` | Implementation protocol | Current implementation source for cross-skill rules. |
| `skills/development/**/SKILL.md` | Implementation | Current implementation source for development workflow behavior. |
| `skills/governance/setup-doc-governance/**` | Implementation | Current implementation source for governance behavior. |
| `skills/assessment/**/SKILL.md` | Implementation | Current implementation source for manual review and grill behavior. |
| `skills/README.md` | Local map | Current derived map of skill grouping; agrees with ADR-0001 and implementation tree. |
| `.skillignore` | Distribution config | Current implementation evidence that `docs/archive/**` is excluded from published canonical skills. |
| `INSTALL.md` | Operations guide | Current derived operations guide; facts need consolidation into authority. |
| `docs/index.md` | Docs route | Current derived index; must route to authority after this batch applies. |
| `docs/ssot-map.md` | SSOT map | Current derived map; useful, but it is not L1 authority. |
| `README.md`, `README_CN.md` | Repo entries | Current onboarding entries; consistent with ADRs, but derived. |
| `AGENTS.md` | Agent adapter | Current thin adapter; consistent with the constitution and ADR-0001. |
| `CLAUDE.md` | Agent adapter | Misleading adapter: current dirty content points to missing `AGENT.md`; tracked content pointed to missing `AGETNO.md`. |
| `docs/standards/git-commit-standard.md` | Development standard | Current standard without authority layer; promote fact into operations authority. |
| `docs/share/workflow-and-design.md` | Shareable overview | Mostly current derived explanation, but must not be treated as authority and uses older "state cache" wording where implementation says "routing signal". |
| `docs/design/architecture-module-design.md` | Draft design | Historical/draft evidence; concrete behavior defers to ADRs and skill implementation. |
| `docs/design/governance-plan-batch-lifecycle-proposal.md` | Implemented proposal | Historical evidence; implemented behavior is now in `setup-doc-governance` and shared references. |
| `docs/design/skills/*.md` | Skill design drafts | Historical design evidence; current behavior is in `skills/**/SKILL.md`. |
| `docs/design/v1-clean/**` | Proposed/plan material | Historical execution/design plans; not current fact. |
| `docs/design/v1-pony/README.md` | Proposed design | Historical/proposed design; constitutional minimum-path facts already live in the constitution and current skills. |
| `docs/design/document-driven-workflow-skills-final.md` | Large final design draft | Historical design evidence; contains old artifact examples and must not override current skills. |
| `docs/reference/docs/**` | External/reference material | Raw historical/reference evidence only. |
| Historical reference skills | Removed raw evidence | Removed after application; not canonical facts and no longer retained under `docs/archive/raw/reference/`. |
| `.claude/settings.local.json` | Tool-local config | Tracked local adapter config; not a docs authority source for workflow facts. |

## Authority Structure To Create

Create only these authority sections, populated with facts confirmed by ADRs,
current skill implementation, current tracked configuration, and this plan's
approval:

| Path | Type | Source Of Truth | Owner / Verification |
|---|---|---|---|
| `docs/authority/README.md` | authority index | `user_confirmed` | Owner: user; last verified: 2026-07-02 |
| `docs/authority/product/scope.md` | product | `adr` | Constitution, ADR-0001, ADR-0002 |
| `docs/authority/architecture/repo-and-skills.md` | architecture | `code` | Current `skills/**`, `.skillignore`, symlink evidence; last verified: 2026-07-02 |
| `docs/authority/workflow/development-flow.md` | workflow | `code` | Current `SKILL.md` files and shared protocol; owner: user; last verified: 2026-07-02 |
| `docs/authority/workflow/doc-governance.md` | workflow | `code` | Current `setup-doc-governance` implementation; owner: user; last verified: 2026-07-02 |
| `docs/authority/agent/policy.md` | agent | `adr` | Constitution, ADR-0002, shared protocol; owner: user; last verified: 2026-07-02 |
| `docs/authority/operations/distribution.md` | operations | `code` | `INSTALL.md`, `.skillignore`, current symlinks; last verified: 2026-07-02 |
| `docs/authority/operations/git.md` | operations | `user_confirmed` | Existing git standard plus approval of this plan; last verified: 2026-07-02 |

High-risk authority docs are `workflow/**` and `agent/policy.md`; they must
include `owner` and `last_verified` frontmatter when applied.

## File Routing Decisions

| Source | Current Role | Verdict | Target | Reason | Requires Confirmation |
|---|---|---|---|---|---|
| `docs/authority/constitution.md` | Active constitution | merge | `docs/authority/README.md`, `docs/authority/product/scope.md`, `docs/authority/agent/policy.md` | Expose constitutional facts through authority routing. | yes |
| `docs/adr/ADR-0001-lifecycle-scope-and-skill-grouping.md` | Accepted ADR | merge | `docs/authority/product/scope.md`, `docs/authority/architecture/repo-and-skills.md` | ADR owns lifecycle scope and skill grouping facts. | yes |
| `docs/adr/ADR-0002-human-first-agent-responsibility.md` | Accepted ADR | merge | `docs/authority/agent/policy.md`, `docs/authority/product/scope.md` | ADR records the accepted human-first amendment. | yes |
| `skills/_shared/references/shared-protocol.md` | Implementation protocol | promote | `docs/authority/workflow/development-flow.md`, `docs/authority/agent/policy.md` | Current cross-skill behavior should be findable as L1 workflow authority. | yes |
| `skills/development/**/SKILL.md` | Implementation | promote | `docs/authority/workflow/development-flow.md` | Current development-flow facts must come from implementation, not draft designs. | yes |
| `skills/governance/setup-doc-governance/**` | Implementation | promote | `docs/authority/workflow/doc-governance.md` | Current governance behavior should become L1 workflow authority. | yes |
| `skills/assessment/**/SKILL.md` | Implementation | merge | `docs/authority/workflow/development-flow.md` | Manual review/grill boundaries are current workflow facts. | yes |
| `skills/README.md` | Local skill map | merge | `docs/authority/architecture/repo-and-skills.md` | Derived map agrees with ADR and implementation; keep as local map after authority exists. | yes |
| `INSTALL.md` | Operations guide | merge | `docs/authority/operations/distribution.md` | Installation facts should be consolidated into operations authority while `INSTALL.md` stays onboarding. | yes |
| `.skillignore` | Distribution config | promote | `docs/authority/operations/distribution.md` | Confirms archived material is excluded from canonical publishing. | yes |
| `docs/standards/git-commit-standard.md` | Development standard | promote | `docs/authority/operations/git.md` | Active commit-title rule needs an authority home. | yes |
| `README.md`, `README_CN.md` | Repo entries | merge | Update after authority creation | Keep as L3 onboarding; route to authority and avoid independent source-of-truth claims. | yes |
| `docs/index.md` | Docs route | merge | Update after authority creation | Add authority routes and mark design/reference/archive as non-authority. | yes |
| `docs/ssot-map.md` | SSOT map | merge | Update after authority creation | Convert to derived map pointing at `docs/authority/**`. | yes |
| `AGENTS.md` | Agent adapter | merge | `docs/authority/agent/policy.md` | Keep as thin derived pointer and conflict-handling reminder. | yes |
| `CLAUDE.md` | Agent adapter | merge | Replace with thin pointer to `AGENTS.md` and/or `docs/authority/agent/policy.md` | Current dirty and tracked versions both point to missing files; repair only after approval. | yes |
| `docs/share/workflow-and-design.md` | Shareable overview | merge | Update header and stale wording after authority creation | Keep as L3 overview, not authority; align "state cache" language with implementation "routing signal". | yes |
| `docs/design/architecture-module-design.md` | Draft design | archive | `docs/archive/historical/design/architecture-module-design.md` | Valid current facts will be promoted; the source remains draft/historical. | yes |
| `docs/design/governance-plan-batch-lifecycle-proposal.md` | Implemented proposal | archive | `docs/archive/historical/design/governance-plan-batch-lifecycle-proposal.md` | Implemented behavior is now in skills and authority; proposal is historical evidence. | yes |
| `docs/design/document-driven-workflow-skills-final.md` | Large design draft | archive | `docs/archive/historical/design/document-driven-workflow-skills-final.md` | It mixes old design examples with current concepts; keep as evidence only. | yes |
| `docs/design/skills/*.md` | Skill design drafts | archive | `docs/archive/historical/design/skills/` | Current behavior is implemented in `skills/**`. | yes |
| `docs/design/v1-clean/**` | Historical plans | archive | `docs/archive/historical/design/v1-clean/` | Proposed/plan material should not be current context. | yes |
| `docs/design/v1-pony/README.md` | Proposed design | archive | `docs/archive/historical/design/v1-pony/README.md` | Minimum-path facts are already constitutional and implementation-backed. | yes |
| `docs/reference/docs/**` | External/reference docs | archive | `docs/archive/raw/reference/docs/` | External/raw evidence should not live beside current docs routing as current-looking material. | yes |
| Historical reference skills | Historical reference skills | remove | none | They are not canonical facts and were removed from the raw archive after application. | yes |

## Fact Decisions

| Fact | Source | Type | Verdict | Target Authority Doc | Evidence | Risk |
|---|---|---|---|---|---|---|
| Doc Loom Least is a repo-native, skill-based, Markdown-first personal workflow substrate; "platform" does not mean CLI backend, daemon, app backend, or pipeline product. | Constitution, ADR-0001, README, AGENTS | product | promote | `docs/authority/product/scope.md` | Constitution Scope Interpretation; ADR-0001 Decision; README positioning. | medium |
| The current supported lifecycle domain is development; future domains require real workflow boundaries and clear skill contracts. | Constitution, ADR-0001, skills tree | product | promote | `docs/authority/product/scope.md` | ADR-0001 Decision; existing `skills/` directories. | medium |
| Constitutional principles are minimum path, no complex pipeline product, human-semantic design, and human experience first. | Constitution, ADR-0002 | product/agent | promote | `docs/authority/product/scope.md`, `docs/authority/agent/policy.md` | Active constitution; ADR-0002 accepted. | high |
| The canonical skill groups are `development`, `governance`, `assessment`, and `_shared`; `_shared` is not directly invoked. | ADR-0001, `skills/README.md`, file tree | architecture | promote | `docs/authority/architecture/repo-and-skills.md` | 8 canonical `SKILL.md` files and current grouped directories. | low |
| Canonical invocation names are `docloom-workflow`, `setup-doc-governance`, `context-authority`, `plan-confirm`, `tdd-execute`, `doc-sync-close`, `review`, and `grill`. | `skills/**/SKILL.md`, INSTALL | architecture | promote | `docs/authority/architecture/repo-and-skills.md` | Parsed frontmatter from all 8 `SKILL.md` files. | low |
| `docloom-workflow` is a thin router and does not replace stage skills, write plans, execute code, trigger review, or call a CLI backend. | `skills/development/docloom-workflow/SKILL.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Route table and gates in current implementation. | high |
| `context-authority` is conditional, not a default phase, and is used for resume, ambiguity, high-risk/public contract, workflow/agent policy, or conflicts. | `skills/development/context-authority/SKILL.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Trigger Boundary and Gates. | high |
| `plan-confirm` consumes an existing case identity, writes a draft plan, waits for confirmation, and does not create case docs. | `skills/development/plan-confirm/SKILL.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Required Inputs, Workflow, Gates. | high |
| `tdd-execute` requires an approved plan and defaults to TDD, with documented exceptions and alternative verification. | `skills/development/tdd-execute/SKILL.md`, `tdd-exceptions.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Preflight, TDD Rules, TDD Exceptions. | high |
| `doc-sync-close` writes closure before closing state, syncs L2/L3 docs, and only applies authority patches after concrete narrow confirmation. | `skills/development/doc-sync-close/SKILL.md`, `doc-update-rules.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Workflow, Authority Handling, Gates. | high |
| `review` and `grill` are manual, explicit, conversation/read-only helpers that do not write state, artifacts, or workflow routes. | `skills/assessment/review/SKILL.md`, `skills/assessment/grill/SKILL.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Gates and Output Contracts. | medium |
| Fact authority order is active authority docs, production code, tests, accepted ADR/release/migration, pending user info, L2 case docs, L3 derived/index, L4 archive/historical, L5 scratch. | `skills/_shared/references/shared-protocol.md` | workflow | promote | `docs/authority/workflow/development-flow.md` | Shared Protocol Fact Authority Order. | high |
| Governance batches use one date-named plan file unless tied to an active case; no governance changes apply before confirmation. | `setup-doc-governance/SKILL.md`, lifecycle proposal, current skill template | workflow | promote | `docs/authority/workflow/doc-governance.md` | Current Skill Workflow and Gates. | high |
| Authority docs are created only with confirmed facts; bridge files are thin and must not carry old facts. | `setup-doc-governance/SKILL.md`, layering reference | workflow | promote | `docs/authority/workflow/doc-governance.md` | Authority Rules, Bridge Rules. | high |
| Published canonical skills come from `skills/`; archive material is excluded by `.skillignore`, and archived reference skills are not retained. | `.skillignore`, INSTALL, skills tree | operations | promote | `docs/authority/operations/distribution.md` | `.skillignore` content and current tracked paths. | low |
| Shared protocol is exposed into per-skill references by symlink for merge mode and copy-compatible distribution. | symlink listing, INSTALL | operations | promote | `docs/authority/operations/distribution.md` | `find skills -type l -ls`; INSTALL Sync Mode. | low |
| Commit titles must use `<type>: <summary>` with the listed lowercase types. | `docs/standards/git-commit-standard.md` | operations | promote | `docs/authority/operations/git.md` | Existing active standard document. | low |
| Agent adapters should be thin derived pointers; canonical agent policy belongs in authority, not duplicated in adapters. | layering reference, AGENTS, current `CLAUDE.md` breakage | agent | promote | `docs/authority/agent/policy.md` | Agent Instruction Governance; AGENTS content; CLAUDE diff. | high |

## Bridge Decisions

| Old Entry | Bridge Target | Reason |
|---|---|---|
| `docs/design/README.md` | `docs/authority/README.md` and `docs/archive/historical/design/` | `docs/design/` is currently linked from `docs/index.md` and can mislead future agents after design drafts are archived. |
| `docs/reference/README.md` | `docs/archive/raw/reference/` | Preserve a thin pointer for old reference paths after raw material moves to archive. |

No per-file bridge is planned for every archived design/reference file. The
directory-level bridge plus updated docs index is the minimum path.

Deviation at application: the owner said removable material may be removed, so
no bridge files were created; the empty `docs/design/` and `docs/reference/`
directories were removed instead. The entry points were updated directly (see
Applied Result and Entry Point Decisions). The bridge targets above remain the
authority route if a future agent re-encounters these old paths.

## Archive Decisions

| Source | Archive Target | Reason |
|---|---|---|
| `docs/design/architecture-module-design.md` | `docs/archive/historical/design/architecture-module-design.md` | Draft design, not authority. |
| `docs/design/governance-plan-batch-lifecycle-proposal.md` | `docs/archive/historical/design/governance-plan-batch-lifecycle-proposal.md` | Implemented proposal; current behavior lives in implementation and new authority. |
| `docs/design/document-driven-workflow-skills-final.md` | `docs/archive/historical/design/document-driven-workflow-skills-final.md` | Historical composite design; too broad to consume as current context. |
| `docs/design/skills/` | `docs/archive/historical/design/skills/` | Current skill behavior is in `skills/**`. |
| `docs/design/v1-clean/` | `docs/archive/historical/design/v1-clean/` | Historical cleanup plans. |
| `docs/design/v1-pony/` | `docs/archive/historical/design/v1-pony/` | Historical proposed design; current facts promoted elsewhere. |
| `docs/reference/docs/` | `docs/archive/raw/reference/docs/` | External/raw evidence only. |
| Historical reference skills | removed | Historical skill references; not canonical skills and not retained after follow-up cleanup. |

Archived Markdown files must receive `status: archived` or `status:
superseded` frontmatter when moved. Existing source content should otherwise be
preserved.

## Entry Point Decisions

| Path | Decision | Type (repo/docs-route/local/agent-summary/none) | Authority / Conflict | Reason |
|---|---|---|---|---|
| `README.md` | Update links and authority wording after authority docs exist. | repo | Derived from `docs/authority/**` and ADRs. | Repo onboarding remains useful, but it is not source of truth. |
| `README_CN.md` | Mirror relevant route/authority wording from `README.md`. | repo | Derived from `docs/authority/**` and ADRs. | Keep bilingual onboarding consistent. |
| `docs/index.md` | Keep as docs route and update to authority/archive routes. | docs-route | Derived from `docs/authority/README.md`. | Existing docs index is already the docs entry. |
| `docs/ssot-map.md` | Update as derived SSOT map after authority docs exist. | docs-route | Derived from `docs/authority/**`, ADRs, and implementation. | Useful map, but not L1 source. |
| `skills/README.md` | Keep as local skill layout map. | local | Derived from ADR-0001 and current skills tree. | Local map for a complex domain. |
| `AGENTS.md` | Keep as thin agent-summary pointer to constitution and authority. | agent-summary | Derived from `docs/authority/agent/policy.md` and the constitution. | Agent adapter should stay short. |
| `CLAUDE.md` | Repair to thin pointer to the valid agent instruction entry. | agent-summary | Current dirty/tracked content points to missing files. | Prevent Claude adapter from misleading future agents. |
| `docs/design/README.md` | none (directory removed). | local | Directory removed; no bridge created. | Owner said removable material may be removed; see Applied Result. |
| `docs/reference/README.md` | none (directory removed). | local | Directory removed; no bridge created. | Owner said removable material may be removed; see Applied Result. |
| `docs/adr/README.md` | none | none | ADR directory is small and routed by `docs/index.md`. | Avoid unnecessary local entries. |
| Plain archive subdirectories | none unless needed during application | none | Archive index can be added only if traceability becomes hard. | Avoid placeholder indexes. |

## Blocked Decisions

| Topic | Sources | Why Blocked | Needed Decision |
|---|---|---|---|
| Whether external AI Coding reference material contains facts that should become Doc Loom authority. | `docs/reference/docs/**` | These docs are raw/reference material and not verified against current implementation beyond high-level influence. | Owner must explicitly identify any external reference fact to promote. |
| Whether the implemented governance batch lifecycle proposal should become an ADR. | `docs/design/governance-plan-batch-lifecycle-proposal.md`, current skills | Implementation agrees with the proposal, but ADR promotion changes decision history semantics. | Owner decision if this should be recorded as an ADR rather than authority extracted from implementation. |
| Whether to add future lifecycle domains such as product/research/design now. | Constitution, ADR-0001, README | Constitution and ADR allow future domains only when real boundaries exist; no current implementing skills exist. | Owner provides a concrete lifecycle boundary and skill contract in a separate case. |

## Follow-ups

| Follow-up | Reason | Suggested Owner |
|---|---|---|
| After applying, run link checks with `rg` for old `docs/design` and `docs/reference` paths. | Archive moves will require route cleanup in README/index/share docs. | agent |
| Consider a later ADR for governance batch lifecycle only if the owner wants decision-history permanence. | Current implementation can be governed by authority without adding an ADR now. | user |
| Consider adding a small docs/reference import note if raw references are kept elsewhere by external tooling. | This batch assumes repo-local archive is the current evidence store. | user |

## Applied Result

| When | Change | Result | Evidence |
|---|---|---|---|
| 2026-07-02T18:53:19+08:00 | Approved plan v1. | Governance plan moved from `proposed` to `approved`. | User confirmed `Approve plan v1`. |
| 2026-07-02 | Created authority docs. | Added `docs/authority/README.md`, product scope, repo/skills architecture, development workflow, doc governance, agent policy, distribution, and git commit-title authority docs. | ADRs, current `skills/**`, `.skillignore`, symlink evidence, and approved plan. |
| 2026-07-02 | Archived historical and raw docs. | Moved `docs/design/**` to `docs/archive/historical/design/` and `docs/reference/**` to `docs/archive/raw/reference/`; all 36 archived Markdown files have `status: archived`. | Archive frontmatter verification script. |
| 2026-07-02 | Removed archived reference skills. | Deleted historical skill references from the raw archive, removed direct historical-design citations to the deleted skill files, and kept only a non-authority historical note; `docs/archive/raw/reference/` now keeps raw reference docs only. | Current file tree, historical design reference sections, `.skillignore`, `INSTALL.md`, `skills/README.md`, and distribution authority. |
| 2026-07-02 | Removed empty old entry directories. | Removed empty `docs/design` and `docs/reference` instead of creating bridge README files after user said removable material may be removed. | `find docs -maxdepth 2 -type d`; current entry docs no longer link old paths. |
| 2026-07-02 | Updated entry points and adapters. | Updated `README.md`, `README_CN.md`, `docs/index.md`, `docs/ssot-map.md`, `docs/share/workflow-and-design.md`, `AGENTS.md`, `INSTALL.md`, `skills/README.md`, and `.skillignore`; `CLAUDE.md` now points to `./AGENTS.md`. | `rg` found no current-entry links to old `docs/design` or `docs/reference` paths. |
| 2026-07-02 | Kept blocked topics visible. | Applied safe decisions and left external-reference promotion, ADR promotion, and future lifecycle domains unresolved. | This plan has `status: applied_with_blocks`. |
| 2026-07-02 | Resolved Fast-Path confirmation policy. | Aligned risk policy, shared protocol, development workflow skills, plan template, and authority docs around one-shot low-risk, Fast-Path persistent, full-pipeline, and high-risk confirmation paths. | Owner requested execution of the synthesized improvement plan; see current skill and authority diffs. |
| 2026-07-02 | Moved constitution into authority. | Added `docs/authority/constitution.md`, removed the old ADR-0000 constitution file, and recorded the location decision in ADR-0003. | Owner requested direct replacement with `constitution.md`; current docs index and SSOT map point to the authority constitution. |
