---
title: Skills 精简计划 v1
type: plan
status: proposed
created: 2026-06-23
basis: 2026-06-23 skills/ 全量评审
---

# Skills 精简计划 v1

本计划独立于本目录已有两份计划，基于 2026-06-23 对 `skills/` 8 个 SKILL.md + `_shared/references/shared-protocol.md` 的全量评审。

## 目标与边界

**目标**：删除重复描述、压缩冗余语义、统一分散定义，不改变任何 skill 的行为约束。

**原则**：
- 每条行为规则在清理后至少保留一处；同一规则出现多处时，保留在 agent 决策时刻最可能读到的地方，其余删除或改为引用。
- 只动文本，不改路由逻辑、不改所有权划分、不改 Artifact Policy。
- 不新增规则、不新增 skill、不新增 artifact 类型。

**范围**：`skills/` 下全部 `.md`。

**预估收益**：约 250–300 行净减少，25–35% 文本量下降。

---

## 问题分类（评审结论）

评审发现 6 类冗余：

1. **高风险领域枚举重复 5 处**：`security, auth, permission, privacy, billing, data deletion, public API, CLI, schema, config contract` 在 grill / review / context-authority / tdd-execute / doc-sync-close(隐含) 各出现一次，措辞漂移。
2. **description 与正文 H1 段重复**：每个 skill 的 frontmatter description 写了功能清单，正文开头再 paraphrase 一遍。
3. **SKILL 正文复述 shared-protocol**：Closure Status、Fact Authority、case_state.yaml routing signal 等已在 shared-protocol 定义，各 skill 又重列。
4. **单 skill 内部自重复**：setup-doc-governance 同一约束出现 3 次；plan-confirm Plan Quality Standard 是 Workflow 的重述；tdd-execute 两个 review_risk 小节重叠。
5. **Gates 混入非 gate 内容 + 自路由语义模糊**：Gates 里混了操作指令；多处 `→ Route: <自身>` 实际意为「停下等用户」，写成自路由易误读。
6. **两份 authority status 值定义会漂移**：`active/draft/superseded/archived` 同时在 setup-doc-governance/SKILL.md 和 doc-update-rules.md 定义。

---

## 执行阶段

### 阶段 1：建立 shared-protocol 单一定义源（高优先级）

#### 1.1 新增 High-Risk Domains 锚点

在 `shared-protocol.md` Fact Authority Order 之后新增：

```markdown
## High-Risk Domains

High-risk domains: security, auth, permission, privacy, billing, data deletion,
public API, CLI, schema, config contract, and ADR-protected architecture
boundaries.

A task touching any of these is not low-risk, cannot use fast-path, and requires
explicit user confirmation for authority changes.
```

随后将以下位置的行内列表改为引用：

| 文件 | 位置 | 改动 |
|---|---|---|
| `shared-protocol.md` | Fast-Path 条件 | 改为 "No conflict in any High-Risk Domain" |
| `shared-protocol.md` | Small Project Degradation | 改为 "task involves any High-Risk Domain" |
| `context-authority/SKILL.md` | Conflict Handling | 改为 "See shared-protocol → High-Risk Domains" |
| `review/SKILL.md` | Review Checks 高风险段 | 改为引用 |
| `grill/SKILL.md` | High-Risk Topics | 改为引用 |
| `tdd-execute/SKILL.md` | Review Risk anchoring | 改为引用 |
| `setup-doc-governance/references/verdicts.md` | block 条件 | 保持行内但用 canonical 措辞 |
| `doc-sync-close/references/doc-update-rules.md` | Narrow Authority Patch 条件 | 条件 5 改为 "No High-Risk Domain conflict" |

> 注：grill / review 为对话型只读 skill，若担心独立调用时 shared-protocol 未加载，保留一行行内摘要而非全量列表。

#### 1.2 合并 case_state.yaml 冲突处理两处

`shared-protocol.md` line 125-129（摘要）与 line 199-213（6 步推导顺序）讲同一件事。保留一处合并版：

```markdown
When case_state.yaml conflicts with Markdown:
- Report `state_cache_conflict`.
- Derive phase from the smallest reliable signal:
  1. closure.md exists -> closed
  2. execution.md exists and acceptance complete -> doc_syncing
  3. execution.md exists -> executing
  4. plan.md status: approved -> planned
  5. plan.md status: draft -> waiting_for_plan_confirmation
  6. no clear signal -> needs_user_decision
- Route based on derived phase.
- Do not silently overwrite the cache; owning skill may update only after deriving correct phase.
```

#### 1.3 删除 Session State Carry 独立小节

line 226-232 与 case_state.yaml routing fields 段（line 108-114）语义重复。删除独立小节，其唯一增量「next skill 可用 prior output 而非重读」已在 case_state.yaml 段末尾说明（line 110-113）。若未说明则补一句到该段。

#### 1.4 合并 Artifact Policy 表

`closure.md` 行被拆成主表下方第二张表（line 153）。合并回主表，作为普通行。

#### 1.5 统一 authority status 值定义

`active/draft/superseded/archived` 在 `setup-doc-governance/SKILL.md:106` 与 `doc-update-rules.md:69-74` 各定义一次。保留 `doc-update-rules.md` 为唯一定义源；`setup-doc-governance/SKILL.md` 改为 "Allowed status: see doc-update-rules → Lifecycle Status"。

---

### 阶段 2：SKILL.md 精简（高/中优先级）

#### 2.1 压缩 description 为单句触发条件

description 是 skill 选择信号，应是一句「何时用」，不是功能清单。功能清单大多正文已有。

| 文件 | 建议 description（≤25 词） |
|---|---|
| docloom-workflow | Default entry for Doc Loom Least. Use when a user asks for a docs-first workflow but does not name a specific skill. |
| context-authority | Gather minimal context and resolve authority before planning. Use before plan-confirm when a case needs a context gate, on resume, or when authority conflicts may affect the plan. |
| plan-confirm | Create and confirm an implementation plan before execution. Use after case identity exists and context is gathered or explicitly skipped. |
| tdd-execute | Execute an approved Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan. |
| doc-sync-close | Sync docs and close a Doc Loom case. Use after execution, or when a case is blocked, cancelled, paused, superseded, or abandoned. |
| review | Read-only evidence review of a design doc, proposal, code diff, test set, or case evidence. Use only when the user explicitly asks for review. |
| setup-doc-governance | Set up or rebuild documentation governance for a project. Use when the user asks to initialize, organize, rebuild, or repair docs governance. |
| grill | Interactively challenge and pressure-test a claim. Use only when the user explicitly asks to grill, challenge, scrutinize, or question assumptions. |

#### 2.2 删除与 trimmed description 重复的正文开头句

| 文件 | 删除 |
|---|---|
| doc-sync-close | "Synchronize task documents and close, pause, block, cancel, supersede, or abandon an existing Doc Loom case." |
| plan-confirm | "Create the execution plan and get user confirmation before tdd-execute." |
| tdd-execute | "Execute an approved persistent Doc Loom case plan." |
| context-authority | "Gather the smallest relevant context needed before planning." |
| docloom-workflow | "Use the minimum path that safely resolves the current request."（移为 Route Table 上方一行注） |

#### 2.3 删除复述 shared-protocol 的正文

| 文件 | 位置 | 改动 |
|---|---|---|
| review/SKILL.md | Fact Authority（line 58-61） | 删 line 61 重述，保留指向引用 |
| doc-sync-close/SKILL.md | Closure Status additional guidance（line 41-45） | 删 Done/Blocked/… 重列，保留指向 |
| tdd-execute/SKILL.md | State Update 起句（line 172-174） | 删 "case_state.yaml is a routing signal…" 重述 |
| tdd-execute/SKILL.md | line 151 "Do not trigger review; only the user can" | 删，shared-protocol line 180 已定义 |
| docloom-workflow/SKILL.md | Fast-Path 条件（line 97-99） | 删，指向 shared-protocol Fast-Path |

#### 2.4 删除单 skill 内部自重复

| 文件 | 改动 |
|---|---|
| setup-doc-governance/SKILL.md | 「不写新 batch 进已有 plan 文件」出现 3 次（Outputs / Workflow step 1 / Gates），保留 Outputs 处，其余删 |
| setup-doc-governance/SKILL.md | 「只创建有确认事实的 authority section」Authority Rules 与 Gates 各一次，保留 Authority Rules |
| plan-confirm/SKILL.md | 删 Plan Quality Standard 小节（重述 Workflow step 6 + Plan Rules） |
| tdd-execute/SKILL.md | 合并 Review Risk 与 Review Risk Lifecycle 为一节，表格化（见下） |

tdd-execute 合并后：

```markdown
## Review Risk Lifecycle

| Condition | Action |
|---|---|
| 触及 High-Risk Domain / 覆盖不足 / 重大 deviation / authority proposal pending | Write review_risk to case_state.yaml |
| 范围增减或新证据化解某风险 | Update review_risk |
| 原 high-risk 条件未全部解除 | Do not clear to low |

review_risk 记 low/medium/high，作为数据非 gate。closure 消费终值，原因留 execution.md。
```

#### 2.5 Gates 节规范化

两条规则：

1. **Gates 只保留硬约束**：删除复述正文规则的条目；同一规则在正文与 Gates 都有时，留在正文。
2. **自路由语义修正**：`→ Route: <自身>`（doc-sync-close line 130/131/136、plan-confirm 多处）实际意为「停下等用户输入」，统一改为 `→ wait for user input`；只有跨 skill 路由才写 `Route: <other>`。Gate 的 `Route: X. Reason: Y. Required input: Z` 格式已由 shared-protocol line 174 定义，无需每条重拼，只写触发条件 + 目标。

逐文件 Gates 清理：

| 文件 | 删除（已被正文/shared-protocol 覆盖） |
|---|---|
| review | "Do not write files"、"Do not modify code/docs/state/artifacts"、"Do not output workflow route"、"Do not write authority proposal" |
| doc-sync-close | 复述 Acceptance Mapping 的两条；line 141 七类列举简化为 "Do not modify code, tests, or artifacts to make closure pass" |
| grill | 4 条 "do not create/modify files" 合并为一行 |
| context-authority | "Do not modify code, tests, authority…" 简化为 "Read-only; do not modify any file" |
| plan-confirm | "Draft plans awaiting…phase" 条目（Workflow step 8 已覆盖） |
| tdd-execute | "Do not modify authority docs"、"Do not modify scripts, dependencies…"（Preflight/TDD Rules 已覆盖） |
| docloom-workflow | 复述 Route Table 行的条目（status-only / no auto-execute / no auto-review / no grill） |
| setup-doc-governance | 复述 Authority Rules 的条目（empty sections / unconfirmed raw material） |

操作指令误入 Gates 的：

| 文件 | 误置条目 | 移至 |
|---|---|---|
| setup-doc-governance/SKILL.md:157 | "Update the docs entry index after applying decisions" | Workflow |

#### 2.6 docloom-workflow Route Table 合并行 6 与 8

两行都路由到 context-authority，合并为：

```
| 6 | Persistent work needing a context gate (resume, authority, conflict, or high-risk) | context-authority |
```

删除原行 8。

#### 2.7 plan-confirm Version And Confirmation 表格化

line 102-133 用 32 行描述三个状态转换场景，压缩为一张状态转换表 + material changes 定义（保留语义，详见阶段 1 的合并范例风格）。

---

### 阶段 3：References 与 Templates 精简（中/低优先级）

- `retrieval-routing.md` governance docs 读取优先级大段文字 → 表格化。
- `context-resolution.md` Run Modes（isolated/branch/inline）移到 shared-protocol，context-authority 与 plan-confirm 引用。
- `shared-protocol.md` Design Guardrails（line 9-12 纯哲学）删除。
- `shared-protocol.md` Skill Runtime Boundary 5 条 Do not 合并为 2 条（line 24-30）。
- `setup-doc-governance/SKILL.md` line 58-61 legacy artifact 禁列：若确认无生成风险则删，否则保留一行 "Do not generate legacy artifact types from older designs."
- `tdd-execute/SKILL.md` TDD anti-patterns 5 条压为 2 条。
- `doc-sync-close/SKILL.md` Knowledge Changes 删除举例句，保留分类定义。
- 三份近同 handoff 模板合并为一份 `skills/_shared/templates/handoff.md`，plan-confirm / tdd-execute / doc-sync-close 引用。

---

## 执行顺序

| 阶段 | 内容 | 净减少 | 风险 |
|---|---|---|---|
| 1 | shared-protocol 单一定义源（High-Risk Domains + 合并重复段） | ~80 行 | 低·纯重构 |
| 2 | SKILL.md 精简（description + 正文去重 + Gates 规范化） | ~140 行 | 低·删除冗余 |
| 3 | References / Templates（表格化 + 合并模板） | ~50 行 | 低·格式调整 |

阶段 1 应先做：阶段 2/3 多处引用 High-Risk Domains 等新定义源，依赖阶段 1 落地。

---

## 验证

每阶段后：

1. `rg` 检索被删文本，确认无悬空引用。
2. 逐个读改过的 SKILL.md，确认每条行为规则仍存在至少一处。
3. 确认所有 "Read when trigger condition is met" 指向的文件仍存在。
4. 确认 docloom-workflow Route Table 在 Gates 精简后仍覆盖全部路由场景。
5. 确认自路由条目统一为 `wait for user input`，跨 skill 路由仍写 `Route: <other>`。
6. 跑已有 eval（若有）。

## 验收标准

- 清理前存在的每条行为规则，清理后至少保留一处。
- 无 SKILL.md 正文开头句与 trimmed description 重复。
- 无 Gates 条目在不增加路由信息的前提下复述正文规则。
- shared-protocol 无内部重复小节。
- 高风险领域列表在 shared-protocol 定义一次，他处引用且措辞一致。
- authority status 值仅在一处定义。
- 自路由与跨 skill 路由措辞区分清晰。
- 文本量下降约 25–35%。

## 不纳入本计划

- Execution Instruction Order 与 Fact Authority Order 合并：语义不同（执行优先级 vs 事实优先级），合并损可读性。
- review Finding Format 的 Minor 示例：偏弱但无害。
- 新增 skill、新增规则、改 Artifact Policy、改所有权划分：均超出「文本精简」范围。
