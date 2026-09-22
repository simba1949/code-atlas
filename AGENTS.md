# AGENTS.md

This file is the repository constitution for coding agents such as Codex and Claude Code when they inspect, maintain, refactor, or optimize Code Atlas.

It is **not** a runtime Skill and must not be treated as Canonical Code Atlas output.

Before making a non-trivial change, read:

1. this file;
2. `docs/code-atlas-design.md`;
3. the relevant files under `docs/decisions/`;
4. the owning `SKILL.md`;
5. affected references/templates and dependent Skills.

Do not optimize a single Skill in isolation when the change alters a cross-Skill contract.

---

## 1. Mission

Code Atlas exists to help engineers recover the **real business model behind a complex codebase** and connect that model back to concrete technical implementation.

Traditional code navigation often answers:

```text
Class A
→ Method B
→ Service C
→ Table D
```

Code Atlas must answer the deeper questions:

```text
Why does this code execute?
→ Which Business Capability does it serve?
→ Which Scenario is active?
→ Which Process is being performed?
→ Which Business Object is affected?
→ Which Lifecycle Transition occurs?
→ Which Execution Chain implements it?
→ What code/data/integration evidence proves that conclusion?
```

The project is therefore **not primarily a call-graph generator**.

Its primary product is a business-oriented, evidence-backed knowledge model whose technical traces remain inspectable.

---

## 2. Product Intent

Code Atlas should make an unfamiliar system understandable from both directions:

```text
Business
→ Domain
→ Capability
→ Scenario
→ Process
→ Activity
→ Execution Chain
→ Code / DB / RPC / MQ / Job
```

and:

```text
Code / Table / State / RPC / MQ / Job
→ Execution Chain
→ Process
→ Scenario
→ Capability
→ Domain
```

The desired outcome is not “more documentation”.

The desired outcome is:

- lower codebase comprehension cost;
- explicit business semantics;
- traceable technical evidence;
- stable knowledge identities;
- navigable business-to-code and code-to-business relationships;
- honest unresolved boundaries instead of invented explanations.

---

## 3. Mental Model

Behavior axis:

```text
Business Domain
→ Capability
→ Scenario
→ Process
→ Sub-process / Step
→ Execution Chain
```

Object axis:

```text
Business Object
→ Lifecycle
→ State
→ State Transition
       ↑
Process / Sub-process / Step
```

Important distinction:

```text
Business model
≠
Technical topology
```

Maintain both:

```text
Technical:
System / Workspace
→ Project / Repository
→ Module
→ Component

Business:
Domain
→ Capability
→ Scenario
→ Process
```

Execution Chains connect these two maps.

---

## 4. Non-Negotiable Design Invariants

These are architectural invariants, not implementation preferences.

### 4.1 Evidence First

No current-code evidence → no published semantic claim.

Never infer final business meaning from naming conventions, industry intuition, or “what systems like this usually do”.

Unknown must remain unknown.

### 4.2 Technical Boundaries Must Not Break Business Continuity

Project, repository, module, JVM, synchronous RPC, transaction, service class, and method boundaries do **not** automatically create business boundaries.

A Process may cross several technical components.

### 4.3 Process and Execution Chain Are Different Dimensions

```text
Process
= business flow unit

Execution Chain
= continuous technical execution context
```

A single Process may be implemented by multiple Execution Chains.

MQ consumers, callbacks, scheduled jobs, retry workers, compensation workers, manual retriggers, or DB-driven later workers may split an Execution Chain without creating a new Process.

### 4.4 Scenario Is a Business Path, Not an Entry Type

HTTP / RPC / MQ / Job differences do not automatically create different Scenarios.

A Scenario exists only when code-backed business selection causes a stable, materially different business path.

### 4.5 Canonical Lifecycle Belongs to the Business Object

Lifecycle definitions belong to Business Objects.

Processes and activities reference Lifecycle impact / State Transitions; they do not redefine their own state machines.

### 4.6 Markdown Is Canonical Knowledge

```text
Markdown = source of published truth
HTML     = generated presentation
```

Never create facts directly in HTML or another hidden presentation model.

### 4.7 Full Analysis, Full Generation

Every normal Atlas run performs:

```text
Current Code
→ Full Discovery
→ Modeling
→ Trace
→ Challenge
→ Converge
→ Audit
→ Full Markdown Generation
→ Full HTML Generation
```

Do not silently replace this with partial/incremental mutation.

### 4.8 Single Writer / Single Owner

One Knowledge Object has one Owner Skill.

Other Skills may:

- REFERENCE
- DISCOVER
- CHALLENGE
- PROPOSE

They must not overwrite another owner's canonical knowledge.

### 4.9 One-Way Canonical Relationships

Store each relationship in one authoritative direction.

Derive reverse navigation instead of maintaining duplicate relationship truth.

### 4.10 Candidate Knowledge Is Not Published Knowledge

Candidate entities, hypotheses, and unresolved interpretations stay in the analysis workspace.

They do not enter final `docs/code-atlas/` until they pass the relevant promotion and audit gates.

### 4.11 Canonical Atlas Belongs to the Analysis Workspace Root

One analysis scope produces one Canonical Atlas:

```text
<analysis-workspace-root>/docs/code-atlas/
```

For multi-project analysis:

```text
settleWork/
├── emp-account/
├── site-account/
└── docs/code-atlas/
```

Do not create separate Atlases under `emp-account/` and `site-account/` for the same cross-project analysis.

The Analysis Workspace Root controls publication location; it does not automatically add every sibling repository to the selected Project set.

Project boundaries are represented inside the Atlas, not by splitting the Atlas itself.



### 4.12 Semantic Canonicalization Must Stay Simple

Use:

```text
skills/code-atlas/references/semantic-canonicalization-protocol.md
skills/code-atlas/references/semantic-identity-rules.md
```

Shared flow:

```text
Fact
→ Candidate
→ Canonicalize
→ Verify
→ Publish
```

Canonicalization outcomes are only:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

Rules:

- technical names/locations do not define business identity;
- merge only with positive semantic-equivalence evidence;
- create a new identity only with positive semantic-difference evidence;
- otherwise use `UNRESOLVED`;
- preserve aliases and evidence when merging;
- do not use numeric identity scores;
- Owner Skills decide semantics; `ca-knowledge` only keeps consistency.

Do not introduce extra registry states, identity meta-models, scoring layers, or analysis record types unless repeated real-world use proves they are necessary.

### 4.13 Scenario Identity Comes from Canonical Business Process Flow

### 4.14 Scenario Identity Comes from Canonical Business Process Flow

Selectors discover candidate paths; they do not define Scenario identity.

Before publishing a new Scenario:

```text
Candidate paths
→ normalize non-identity dimensions
→ compare canonical Business Process Flow
→ merge equivalents
→ promote only materially distinct paths
```

Do not create a Scenario for every combination of:

- channel;
- role;
- amount band;
- entry type;
- rule result;
- sync/async implementation;
- retry/failure/callback state.

The burden of proof is on Scenario split.

### 4.14 Sub-process Is Intentionally Shallow

Default structure:

```text
Process
├── Step
├── Sub-process
│   ├── Step
│   └── Step
└── Step
```

Do not recursively nest Sub-processes by default.

If deeper nesting appears necessary, first consider:

- collapsing to Steps;
- splitting sibling Sub-processes;
- promoting an independently valuable unit to Process.

---



### 4.15 Business Topology Must Converge Before Publication

Use:

```text
skills/code-atlas/references/business-topology-convergence.md
```

Mandatory discovery order:

```text
Business Actions
→ Capability Convergence
→ Subdomain Evaluation
→ Domain Convergence
→ Scenario Enumeration
```

Mandatory publication hierarchy:

```text
Domain
→ Subdomain [optional]
→ Capability
→ Scenario
→ Process
→ Execution Chain
```

Rules:

- Subdomain publication is optional; Subdomain Evaluation is mandatory.
- Every Capability must undergo Scenario Evaluation.
- Workflow stages must be challenged as Process candidates before Capability promotion.
- Project/module names must not leak into business identity by default.
- Freeze the complete Business Topology before final Markdown/HTML publication.
- The default HTML map must not bypass intermediate hierarchy levels.


### 4.16 Business Data Mutation Does Not Define Domain Ownership

Use:

```text
skills/code-atlas/references/domain-responsibility-boundaries.md
```

A Domain owns a business responsibility, not every operation that mutates its data.

Mandatory ownership questions:

```text
State
→ what current business state is authoritative here?

Intent
→ what business transaction/request explains the change?

Fulfillment
→ what obligation is actually completed here?
```

For financial systems, use this diagnostic:

```text
Funds Account
→ 账户管状态

Funds Transaction
→ 交易管意图

Funds Settlement
→ 结算管履约
```

Examples:

```text
freeze / unfreeze / debit / credit
→ Account responsibility

withdraw / recharge / consume / refund
→ Transaction responsibility

route / clearing / payout fulfillment
→ Settlement responsibility
```

unless source evidence proves otherwise.

Cross-domain calls do not transfer Scenario ownership.

### 4.17 HTML Must Optimize Understanding, Not Decoration



`ca-visual` must produce an interactive business-first Atlas.

Core viewer rule:

```text
default business
→ reveal technology on demand

default local context
→ expand scope on demand

business → flow → object/data → code
↔ reverse navigation
```

Mandatory behavior:

- global business + technical search;
- Scenario-first Process Flow;
- Process `Business / Data / Code` views;
- Project swimlanes for cross-project Chains;
- evidence on demand;
- Focus/Spotlight instead of a default global node cloud;
- Code → Business reverse navigation.

Visual technology should feel modern but calm.

Do not use decorative complexity, giant graphs, or animation that reduces comprehension.

HTML remains fully derived from Canonical Markdown.


## 5. Skill Ownership

| Skill | Canonical responsibility |
|---|---|
| `code-atlas` | orchestration only |
| `ca-code-intel` | capability discovery, Fact Inventory, source evidence |
| `ca-project` | System / Project / Module / technical map and technical references |
| `ca-business` | Domain / Capability / Business Object / Lifecycle |
| `ca-scenario` | Scenario / Process / Sub-process / Step |
| `ca-chain` | Execution Chain / Common Chain |
| `ca-audit` | audit findings only; never invent or repair semantic facts |
| `ca-knowledge` | IDs, registry, ownership, references, canonical/reverse relation consistency |
| `ca-visual` | HTML derived only from Canonical Markdown |

Do not move ownership merely to simplify implementation.

If an ownership boundary is genuinely wrong, treat the change as an architecture change and update the relevant ADR/design documentation.

---

## 6. Canonical Relationship Ownership

Use one authoritative direction.

Examples:

```text
Domain            → Capability
Scenario          → Capability
Scenario          → Process Flow
Process           → Sub-process / Step
Process           → Business Object
Process / Activity→ State Transition
Lifecycle         → Business Object
Lifecycle         → State / State Transition
Business Object   → Table
Execution Chain   → Process
Execution Chain   → Activity
Execution Chain   → Common Chain
```

Reverse relations are derived by `ca-knowledge` and presentation layers.

Do not manually maintain both directions.

---

## 7. Accepted Architectural Decisions

Accepted decisions are recorded under `docs/decisions/`.

Current decisions:

| ADR | Decision |
|---|---|
| `ADR-001` | Business semantics are independent from technical boundaries |
| `ADR-002` | Markdown is Canonical Knowledge |
| `ADR-003` | Full analysis/full generation is the default execution model |
| `ADR-004` | Scenario directly orchestrates Process Flow; no Scenario Chain entity |
| `ADR-005` | Process and Execution Chain remain separate concepts |
| `ADR-006` | Canonical Lifecycle belongs to Business Object |
| `ADR-007` | Sub-process hierarchy is intentionally shallow |
| `ADR-008` | Evidence gates and categorical resolution replace numeric confidence scores |
| `ADR-009` | Knowledge relationships use single-direction canonical ownership |
| `ADR-010` | One analysis workspace publishes one Canonical Atlas at the workspace root |
| `ADR-011` | Scenario identity is defined by canonical business-path divergence, not condition combinations |
| `ADR-012` | Semantic Canonicalization is one shared reasoning protocol across semantic Owner Skills; simplified by ADR-014 |
| `ADR-013` | Superseded by ADR-014 |
| `ADR-014` | Semantic identity rules stay intentionally lightweight |
| `ADR-015` | HTML Atlas is business-first, interactive, searchable, and progressively reveals technology |
| `ADR-016` | Business topology converges bottom-up before top-down publication |
| `ADR-017` | Domain ownership follows business responsibility, not shared data mutation |

Before changing one of these decisions, read the ADR.

Do not silently contradict an accepted ADR.

If a decision truly needs to change:

1. create a new ADR;
2. mark the old ADR as `Superseded`;
3. explain the new evidence/problem;
4. update design and affected Skills together.

---

## 8. Rejected Design Directions

The following are intentionally rejected unless a new architectural decision supersedes them.

### 8.1 Project / Module → Business Domain Mapping

Rejected because technical decomposition and business decomposition are different dimensions.

Do not infer Domain identity from package/module/repository layout alone.

### 8.2 Entry Type → Scenario

Rejected examples:

```text
HTTP Withdrawal
RPC Withdrawal
MQ Withdrawal
Job Withdrawal
```

These are technical entry mechanisms unless code proves stable business-path divergence.

### 8.3 Independent Scenario Chain Entity

Rejected.

Scenario already owns its Process Flow.

Do not introduce:

```text
Scenario
→ Scenario Chain
→ Process
```

without a new accepted ADR.

### 8.4 Unified Process/Execution Flow Entity

Rejected because business flow and technical execution context have different boundaries.

Do not merge Process and Execution Chain merely because both can be drawn as diagrams.

### 8.5 Lifecycle Repeated in Process or Scenario

Rejected because it creates duplicate state-machine truth.

Process/Scenario may reference transitions; Lifecycle owns the canonical state model.

### 8.6 Recursive Sub-process Trees

Rejected by default because arbitrary nesting makes the model harder to reason about and encourages method-tree modeling.

### 8.7 One Markdown File Per Step / State / Transition

Rejected by default because it produces thousands of tiny documents and destroys human navigation.

These entities retain stable IDs but are embedded in their owning documents.

### 8.8 Numeric Semantic Confidence

Rejected examples:

```text
Confidence: 82%
Confidence: 0.73
```

Use evidence-backed resolution states instead:

```text
VERIFIED
PARTIAL
UNRESOLVED
```

and distinguish:

```text
NOT_FOUND
UNRESOLVED
```

where applicable.

### 8.9 Incremental Knowledge Mutation as the Default

Rejected for the current architecture because stale mixed-state knowledge and dependency propagation can silently break consistency.


### 8.10 One Atlas Per Project During Cross-Project Analysis

Rejected.

For one cross-project analysis, do not generate:

```text
workspace/project-a/docs/code-atlas/
workspace/project-b/docs/code-atlas/
```

This fragments Scenario, Process, Lifecycle, and Execution Chain knowledge along technical boundaries.

Use one:

```text
workspace/docs/code-atlas/
```

and represent Projects inside that Atlas.

---

### 8.11 Cartesian-Product Scenarios

Rejected.

Do not model every combination of:

```text
channel × role × amount × account type × result × entry
```

as an independent Scenario.

These are candidate dimensions.

They become Scenario identity only when code proves stable, material canonical Process Flow divergence.

Prefer:

```text
Scenario: Bank-card withdrawal

Applicable conditions:
- PC / RPC
- normal / star actor

Branches/rules:
- large amount requires review
```

over:

```text
PC star-user large-amount bank-card withdrawal
RPC normal-user small-amount bank-card withdrawal
...
```


## 9. Optimization Priorities

When trade-offs conflict, use this order:

```text
P0  Factual correctness
P1  Business-semantic correctness
P2  Knowledge consistency
P3  Traceability / auditability
P4  Human readability and navigation
P5  Maintainability
P6  Tool-call / token efficiency
P7  Runtime performance
```

Never optimize P6/P7 by damaging P0–P4.

Examples of invalid optimizations:

```text
Fewer tool calls
→ infer Capability from class names

Fewer documents
→ merge Process and Execution Chain

Faster generation
→ skip evidence challenges

Simpler code
→ let multiple Skills write the same Knowledge Object
```

Prefer semantic correctness over implementation cleverness.

---

## 10. How Agents Should Optimize This Repository

Do not treat “optimization” as shortening prompts or merging files.

Use this workflow.

### Step 1 — Understand

Read the project intent, relevant ADRs, owner Skill, references, and templates.

### Step 2 — Locate Ownership

Identify which Skill owns the faulty concept.

Examples:

```text
wrong Scenario promotion     → ca-scenario
wrong Process boundary       → ca-scenario
wrong Lifecycle semantics    → ca-business
wrong technical chain split  → ca-chain
wrong project topology       → ca-project
broken cross-reference       → ca-knowledge
```

Do not add a workaround to a non-owner Skill when the root contract belongs elsewhere.

### Step 3 — Identify the Invariant

State what must remain true after the change.

Example for Scenario work:

```text
Scenario != technical entry
Scenario != failure branch
Scenario != workflow stage
Scenario != condition combination
Selector → Candidate Path
Canonical Business Process Flow → Scenario Identity
Scenario → evidence-backed Process Flow
```

### Step 4 — Identify the Root Cause

Ask why the current rules produce the bad result.

Prefer improving:

- promotion gates;
- evidence requirements;
- ownership contracts;
- boundary definitions;
- challenge/reconciliation behavior;

over adding dozens of isolated exceptions.

### Step 5 — Propose the Smallest Coherent Change

Change the smallest set of files that fully fixes the contract.

Avoid unrelated cleanup during semantic changes.

### Step 6 — Check Cross-Skill Impact

Determine whether the change affects:

- orchestration;
- downstream inputs;
- canonical IDs;
- templates;
- reverse relations;
- audits;
- HTML rendering;
- README user behavior;
- accepted ADRs.

### Step 7 — Implement

Preserve established terminology.

Do not rename concepts merely for stylistic preference.

### Step 8 — Challenge the Change

Try to disprove the new rule with counterexamples.

Check whether it introduces:

- over-promotion;
- under-promotion;
- duplicate ownership;
- hidden unresolved states;
- technical/business boundary confusion.

### Step 9 — Audit

Run repository and contract checks.

### Step 10 — Record Architectural Changes

If the change alters an accepted architectural decision, add/supersede an ADR.

---

## 11. Change Classification

Before editing, classify the change.

### Local behavior change

Example:

- clarify one evidence rule inside `ca-chain`.

Usually affects one Skill plus tests/checks.

### Contract change

Example:

- change what `ca-code-intel` must return to `ca-business`.

Requires owner + dependent Skills + references/templates + audit review.

### Architecture change

Example:

- add a new Knowledge Entity;
- move ownership between Skills;
- replace full analysis with incremental analysis;
- merge Process and Execution Chain.

Requires:

```text
ADR
→ design
→ owner Skills
→ orchestrator
→ knowledge/audit rules
→ templates
→ README if user-facing behavior changes
→ validation
```

Do not disguise an architecture change as a local refactor.

---

## 12. Fact Layer vs Knowledge Layer

`ca-code-intel` produces source-grounded facts.

Core internal pattern:

```text
Fact + Relation + Evidence
```

Fact Layer must not prematurely invent final business semantics.

Examples:

```text
METHOD --WRITES(value=PAID)--> STATE_FIELD
```

does not by itself prove:

```text
UNPAID → PAID
```

A State Transition requires evidence for the transition semantics.

Similarly:

```text
Producer --PUBLISHES--> Topic
```

does not prove a consumer exists if no consumer is visible.

Use:

```text
RESOLVED
PARTIAL
UNRESOLVED
```

rather than invention.

---

## 13. Scenario / Process / Chain Boundary Guardrails

### Scenario

Promote only when all relevant gates are satisfied:

1. Capability Gate
2. Selection Gate
3. Divergence Gate
4. Stability Gate
5. Evidence Gate

### Process

Use business-semantic boundaries such as:

- Business Goal change;
- independent Business Result;
- independent closure;
- cross-Scenario reuse;
- independent business value.

Do not split merely because of:

- HTTP;
- RPC;
- MQ;
- Job;
- transaction;
- project;
- module;
- service;
- method.

### Execution Chain

Split when a new independent execution context begins, such as:

- MQ consumer;
- callback;
- scheduled worker;
- independent retry worker;
- compensation worker;
- manual retrigger;
- DB-driven later worker.

Continue through synchronous RPC when provider code is visible and the same Process continues.

---

## 14. Lifecycle Guardrails

A canonical Lifecycle requires:

1. confirmed Business Object;
2. state dimension;
3. state values;
4. evidenced transitions.

Status field existence alone is not a Lifecycle.

Enum values alone do not prove transition arrows.

Different state fields such as:

```text
status
pay_status
audit_status
```

remain separate lifecycle dimensions unless code proves otherwise.

---

## 15. Knowledge IDs and File Naming

IDs express knowledge identity, not code location.

Use complete type names:

```text
SYSTEM-...
PROJECT-...
DOMAIN-...
CAPABILITY-...
SCENARIO-...
PROCESS-...
SUB-PROCESS-...
STEP-...
BUSINESS-OBJECT-...
LIFECYCLE-...
STATE-...
TRANSITION-...
EXECUTION-CHAIN-...
COMMON-CHAIN-...
```

Do not introduce ambiguous canonical abbreviations such as:

```text
CAP-
PROC-
SUB-
OBJ-
LC-
TRANS-
```

Business IDs must not encode Project/Module names by default.

File names use readable semantic slugs.

Do not create standalone Markdown files for embedded entities merely because they have stable IDs.

See:

```text
skills/code-atlas/references/naming-conventions.md
```

---

## 16. Repository Structure

```text
code-atlas/
├── AGENTS.md
├── LICENSE
├── README.md
├── README.zh-CN.md
├── docs/
│   ├── code-atlas-design.md
│   └── decisions/
│       ├── README.md
│       └── ADR-*.md
└── skills/
    ├── code-atlas/
    │   ├── SKILL.md
    │   ├── references/
    │   └── templates/
    ├── ca-code-intel/
    ├── ca-project/
    ├── ca-business/
    ├── ca-scenario/
    ├── ca-chain/
    ├── ca-audit/
    ├── ca-knowledge/
    └── ca-visual/
```

---

## 17. Repository Change Rules

### Adding a Skill

Do not add a Skill because a file is large.

A new Skill requires a genuinely distinct responsibility and ownership boundary.

Check:

- Why can no current owner handle this coherently?
- Does it own a new responsibility or duplicate an existing owner?
- Does the orchestrator need to call it?
- Does it change the mental model?
- Does it require an ADR?

### Updating a Skill

Preserve established contracts unless the change intentionally updates them.

Preserve stable Knowledge IDs unless entity identity itself changes.

### Deleting a Skill

Before deletion:

1. find all references;
2. identify owned responsibilities;
3. reassign or intentionally remove them;
4. update orchestration;
5. update design/ADRs if architectural;
6. remove only after no orphan ownership/reference remains.

### Renaming a Skill

Treat as reference migration, not a filesystem-only rename.

Update:

- directory;
- front matter `name`;
- orchestrator;
- dependent Skill references;
- AGENTS/design/ADRs where relevant;
- README if user-facing.

---

## 18. Documentation Boundaries

### README

User-facing.

It should focus on:

- what Code Atlas is;
- what it produces;
- available Skills;
- listing;
- installation;
- update;
- removal;
- user-visible output.

Do not move internal ID, naming, promotion, ownership, or optimization rules into README.

### AGENTS.md

Agent-facing repository constitution.

Contains invariants, optimization guidance, ownership, and decision navigation.

### `docs/code-atlas-design.md`

Human/agent design specification.

Contains the current architecture, not every historical discussion.

### `docs/decisions/`

Architecture Decision Records.

Contains why important decisions were accepted and what alternatives were rejected.

### `skills/**/SKILL.md`

Runtime instructions for the relevant Skill.

Only runtime-relevant rules belong here.

---

## 19. Validation

At minimum, validate:

- every expected Skill directory exists;
- every Skill has `SKILL.md`;
- `name` matches directory;
- `description` exists;
- no duplicate Skill name;
- no broken Skill references;
- no orphan Knowledge owner;
- no ambiguous canonical ID prefix;
- README EN/ZH remain semantically aligned when user-facing behavior changes;
- ADR references exist;
- architecture changes update design/ADR;
- generated Atlas output is never edited to hide a source-Skill contract bug.

Repository discovery:

```bash
npx skills add . --list
```

Expected:

```text
code-atlas
ca-code-intel
ca-project
ca-business
ca-scenario
ca-chain
ca-audit
ca-knowledge
ca-visual
```

---

## 20. Review Checklist

Before finishing a non-trivial change:

- [ ] I read the relevant ADRs.
- [ ] I identified the correct Owner Skill.
- [ ] I preserved or intentionally superseded the relevant design invariant.
- [ ] I fixed the root rule instead of adding a workaround in another Skill.
- [ ] I did not infer business semantics without evidence.
- [ ] I did not confuse technical and business boundaries.
- [ ] I did not create duplicate canonical relationships.
- [ ] I did not introduce duplicate Lifecycle truth.
- [ ] I did not publish candidate/unresolved semantics as verified.
- [ ] I used MERGE / NEW / UNRESOLVED / REJECT consistently.
- [ ] I preserved aliases/evidence when merging.
- [ ] I did not let technical boundaries or name similarity determine semantic identity.
- [ ] I checked cross-Skill contract impact.
- [ ] I updated design/ADR if architecture changed.
- [ ] HTML viewer changes preserve Business First, search, reverse navigation, local focus, and Markdown-as-truth.
- [ ] I kept README user-facing.
- [ ] I validated all Skill names/references.
- [ ] I did not optimize token/runtime cost at the expense of semantic correctness.

---

## 21. Language

- `README.md`: English, default public entry point.
- `README.zh-CN.md`: Simplified Chinese.
- `AGENTS.md`: English to maximize compatibility across coding agents.
- ADRs: English by default; stable domain terms remain unchanged.
- Runtime Skill instructions may use English or Chinese where precision benefits, but canonical terminology must remain stable.

---

## 22. Final Principle

When uncertain how to optimize Code Atlas, choose the change that makes the model:

```text
more evidence-backed
more semantically correct
more internally consistent
more traceable
more understandable
```

—not merely shorter, faster, or more clever.
