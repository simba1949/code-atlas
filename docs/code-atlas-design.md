# Code Atlas Design

## 1. Goal

Code Atlas 将复杂代码库转换成可导航、可验证、可追溯的项目与业务知识 Atlas。

它需要回答：

1. 当前系统由哪些 Project / Module 构成？
2. 系统实现哪些 Business Domain 与 Capability？
3. 一个 Capability 下有哪些真实 Scenario？
4. Scenario 如何编排 Process？
5. Process 内部有哪些 Sub-process / Step？
6. 哪些 Business Object 参与？
7. Business Object Lifecycle 如何变化？
8. 哪些 Execution Chain 实现 Process / Step？
9. DB / MQ / RPC / Job / External System 如何参与？
10. 如何从业务下钻到代码、从代码反查业务？

## 2. Fundamental Rules

### 2.1 Business continuity

> 技术边界可以切分代码，但不能切断业务。

Project / Repository / Module 不得成为业务边界的默认依据。

### 2.2 Evidence first

所有正式知识必须来自当前可见代码事实。

禁止根据行业常识补：

- Business Domain
- Capability
- Scenario
- Process
- Business Object
- State Transition
- Consumer / Provider
- Retry / Callback / Compensation

### 2.3 Canonical source

> Markdown = Canonical Knowledge

> HTML = Atlas Viewer

HTML 不得产生第二套业务事实。

### 2.4 Full run

> 全量输入，迭代分析，一次定稿。

每次运行：

```text
Current Code
→ Full Fact Discovery
→ Modeling
→ Trace
→ Challenge
→ Converge
→ Audit
→ Full Markdown
→ Full HTML
```

### 2.5 Ownership

一个 Knowledge Object 只能有一个 Owner Skill。

非 Owner 只能 Reference / Discover / Challenge / Propose。

### 2.6 One-way relationship storage

只保存权威方向，反向关系派生。

## 3. Skill Architecture

| Skill | Responsibility |
|---|---|
| code-atlas | Orchestration |
| ca-code-intel | Code intelligence and Fact Inventory |
| ca-project | System / Project / Module / technical map |
| ca-business | Domain / Capability / Business Object / Lifecycle |
| ca-scenario | Scenario / Process / Sub-process / Step |
| ca-chain | Execution Chain / Common Chain |
| ca-audit | Quality gate |
| ca-knowledge | Registry / references / consistency |
| ca-visual | Markdown → HTML |

## 4. Cognitive Model

```text
Business Domain
  ↓
Capability
  ↓
Scenario
  ↓
Process
  ↓
Sub-process / Step
  ↓
Execution Chain
```

Object axis:

```text
Business Object
  ↓
Lifecycle
  ↓
State
  ↓
State Transition
       ↑
Process / Sub-process / Step
```

## 5. Fact Layer

Internal analysis model:

```text
Fact + Relation + Evidence
```

Fact categories:

- STRUCTURE
- CODE
- ENTRY
- INTEGRATION
- DATA
- STATE
- CONFIG

Core Relation types:

- CONTAINS
- DECLARES
- CALLS
- IMPLEMENTS
- TRIGGERS
- PUBLISHES
- CONSUMES
- READS
- WRITES
- USES
- DEPENDS_ON
- MAPS_TO

Fact Layer does not contain final business semantics.


## 6. Knowledge Promotion

Keep semantic reasoning simple:

```text
Fact
→ Candidate
→ Canonicalize
→ Verify
→ Confirmed Knowledge
```

Candidate knowledge stays outside the final Atlas.

No Evidence → No Semantic Change.

### 6.1 Semantic Canonicalization

Canonicalization asks:

> Is this candidate the same knowledge as something already found, or different?

Only four outcomes exist:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

Rules:

- technical boundary is not business boundary;
- technical name is not business identity;
- merge requires positive semantic-equivalence evidence;
- new identity requires positive semantic-difference evidence;
- insufficient evidence means `UNRESOLVED`;
- merge preserves aliases and evidence;
- no numeric confidence.

Use:

```text
skills/code-atlas/references/semantic-canonicalization-protocol.md
```

### 6.2 Identity Rules

Each knowledge type defines only:

```text
Identity
Ignore Differences
Promotion
```

Four common questions are usually enough:

```text
Same Meaning?
Can both independently exist?
Can both have independent lifecycle/state?
Does the behavior have an independent goal/result?
```

Scenario additionally uses the De-dimension Test.

Use:

```text
skills/code-atlas/references/semantic-identity-rules.md
```

Do not introduce a larger identity meta-model unless real analysis repeatedly proves it is necessary.


## 7. Capability

> 一个 Business Domain 长期稳定提供的业务功能。

Capability 不是：

- technical entry
- method
- endpoint
- workflow stage

## 8. Scenario

> 同一 Capability 下，经过候选路径归一化后，具有稳定且实质不同 Business Process Flow 的真实业务路径。

核心原则：

> **Selector discovers candidate paths; Canonical Business Process Flow defines Scenario identity.**

Selector 只能发现 Scenario Candidate，不能直接定义 Scenario。

### Scenario Promotion Gates

1. Capability Gate
2. Selection Gate
3. Divergence Gate
4. Stability Gate
5. Evidence Gate
6. Canonicalization Gate

### Scenario Canonicalization

```text
Discover selectors
→ Trace candidate paths
→ Build Candidate Signatures
→ Normalize technical/parameter differences
→ Canonicalize Business Process Flow
→ Merge equivalent candidates
→ Evaluate remaining divergence
→ Scenario Promotion Gate
→ Publish Scenario
```

在创建新 Scenario 前，必须执行 De-dimension Test：

> 去掉渠道、角色、金额区间、入口类型、普通规则结果等维度标签后，两条候选路径是否仍具有实质不同的 Canonical Business Process Flow？

如果没有：

```text
MERGE
```

差异进入：

- Path Selection
- Applicable Conditions
- Business Rules
- Branches

如果仍然不同，才继续 Scenario Promotion。

以下不能单独证明 Scenario：

- HTTP / RPC / MQ / Job entry
- Project / Module
- role/channel 差异
- amount band
- fee/rate/deposit calculation
- success / failure / timeout
- retry / callback / compensation
- sync / async
- different Execution Chain
- technical implementation difference
- one conditional branch

Rule difference 只有在导致稳定、实质性的 Business Process Flow divergence 时，才可能成为 Scenario split 的依据。

Scenario 的命名应描述稳定业务路径，而不是把所有条件组合编码进名称。

Scenario Candidate Signature 属于分析过程对象，不是正式 Knowledge Entity。

## 9. Process

> Scenario 中具有相对独立业务目标、边界和业务结果的业务流程单元。

强边界：

- Business Goal 变化
- 独立 Business Result / closure
- 独立业务价值
- 跨 Scenario 复用

强辅助：

- stable Lifecycle stage
- business responsibility handoff

以下不能单独切 Process：

- MQ
- Job
- Callback
- RPC
- HTTP
- Transaction
- Thread
- Project
- Module
- Service
- Method

## 10. Sub-process / Step

Sub-process:

> Process 内部具有局部业务目标、由多个 Step 组成的可展开局部流程。

跨场景复用或具有独立业务价值时升级为 Process。

Sub-process 默认不嵌套 Sub-process。

Step:

> 不可再拆出有意义业务流程的关键业务动作。

技术动作不是 Step。

## 11. Business Object / Lifecycle

Business Object != Table.

Business Object 可以跨多个表、多个服务，并拥有多个状态维度。

Canonical Lifecycle 只由 Business Object 拥有。

Process 只引用 Lifecycle Transition。

State existence != State Transition.

Enum alone cannot prove transition.

## 12. Execution Chain

Execution Chain 绑定 Process / Activity，描述技术实现。

一个 Process 可以有多个 Chain：

- ENTRY
- ASYNC_CONTINUATION
- CALLBACK
- SCHEDULED
- RETRY
- COMPENSATION
- MANUAL

同步 RPC / 跨 Project 不自动切 Chain。

MQ Consumer / Callback / Job / independent Retry / Compensation / Manual retrigger 形成新的 Chain。

Termination:

- RETURN
- ASYNC_BOUNDARY
- EXTERNAL_BOUNDARY
- PROCESS_BOUNDARY
- DATA_HANDOFF
- SOURCE_UNAVAILABLE

Common Chain 必须是真实共享代码，不是语义相似。

## 13. Knowledge Entity

Independent Markdown:

- System
- Project
- Business Domain
- Business Object
- Lifecycle
- Scenario
- Process
- Execution Chain
- Common Chain
- Core Business Table（按需）

Embedded Entity:

- Capability
- Sub-process
- Step
- State
- State Transition
- Business Rule
- Trigger / Condition

Knowledge Entity != Markdown File.

## 14. Canonical Relationship Direction

| Relation | Owner |
|---|---|
| Domain → Capability | Domain |
| Scenario → Capability | Scenario |
| Scenario → Process Flow | Scenario |
| Process → Sub-process / Step | Process |
| Process → Business Object | Process |
| Process / Activity → Transition | Process |
| Lifecycle → Business Object | Lifecycle |
| Lifecycle → State / Transition | Lifecycle |
| Business Object → Table | Business Object |
| Execution Chain → Process | Execution Chain |
| Execution Chain → Activity | Execution Chain |
| Execution Chain → Common Chain | Execution Chain |

Reverse relations are derived.

## 15. ID Rules

使用完整类型名，不使用容易歧义的缩写：

```text
SYSTEM-<system>
PROJECT-<project>
DOMAIN-<domain>
CAPABILITY-<domain>-<capability>
SCENARIO-<domain>-<capability>-<scenario>
PROCESS-<process>
SUB-PROCESS-<process>-<sub-process>
STEP-<process>-<step>
BUSINESS-OBJECT-<object>
LIFECYCLE-<object>-<dimension>
STATE-<object>-<dimension>-<state>
TRANSITION-<object>-<dimension>-<from>-TO-<to>
EXECUTION-CHAIN-<process>-<role>
COMMON-CHAIN-<chain>
```

业务 Entity ID 默认禁止包含 Project / Module 名。

ID 表示知识身份，不表示代码位置。

## 16. File Naming

文件名使用完整、可读 semantic slug，不使用类型缩写，也不重复目录已经表达的类型。

Examples:

```text
business/domains/courier-wallet.md
scenarios/courier-wallet-withdraw-bank-card.md
processes/site-withdraw.md
objects/withdraw-order.md
lifecycles/withdraw-order-main.md
chains/site-withdraw/entry.md
chains/common-chains/account-validation.md
```

## 17. Analysis Workspace Root

Code Atlas 必须先区分两个概念：

```text
Selected Project Set
= 本次实际分析哪些 Project / Repository

Analysis Workspace Root
= 本次完整 Atlas 的统一发布根目录
```

单项目：

```text
emp-account/
├── src/
└── docs/
    └── code-atlas/
```

多项目：

```text
settleWork/
├── emp-account/
├── site-account/
└── docs/
    └── code-atlas/
```

对于一次跨项目分析，禁止分别生成：

```text
settleWork/emp-account/docs/code-atlas/
settleWork/site-account/docs/code-atlas/
```

Project 边界应该在 Atlas 内部表达：

```text
docs/code-atlas/projects/
├── emp-account.md
└── site-account.md
```

Scenario / Process / Business Object / Lifecycle / Execution Chain 可以自然跨越多个 Project。

同步 RPC / Project 切换本身不能切断 Execution Chain。

Analysis Workspace Root 只决定发布位置，不代表其下所有 sibling repository 自动进入分析范围。

Root 解析优先级：

1. 用户显式指定的 workspace root；
2. 包含已选 Projects 的当前 workspace root；
3. 能明确代表当前分析范围的 selected Project 最近公共父目录。

如果无法可靠确定 Root，不应退化为“每个 Project 各生成一套 Atlas”。

详见 `ADR-010`。

## 18. Canonical Output

所有路径均相对于 Analysis Workspace Root：

```text
docs/code-atlas/
├── README.md
├── system/
├── projects/
├── business/
│   └── domains/
├── objects/
├── lifecycles/
├── scenarios/
├── processes/
├── chains/
│   ├── <process>/
│   └── common-chains/
├── tables/
└── references/
    ├── entries.md
    ├── tables.md
    ├── rpc.md
    ├── mq.md
    └── jobs.md
```

## 19. HTML Atlas Experience

HTML is not a Markdown skin.

It is the interactive view of Canonical Markdown.

Core principle:

> 默认展示业务，按需展开技术；默认展示局部，按需扩大范围；业务与代码双向可达。

### 19.1 Viewer Principles

Use:

```text
Business First
Context Preserved
Progressive Disclosure
Bidirectional Navigation
Search First
Interactive but Calm
```

### 19.2 Primary Navigation

Prefer:

```text
Business Map
Flows
Business Objects
Technical Map
Data
```

Do not expose every internal Knowledge Entity as a top-level navigation item.

### 19.3 Core Drill-down

Primary business route:

```text
Domain / Subdomain
→ Capability
→ Scenario
→ Process
→ Business Object / Data
→ Execution Chain
→ Code
```

Reverse route:

```text
Method / Table / RPC / MQ / Job
→ Execution Chain / Business Object
→ Process
→ Scenario
→ Capability
```

### 19.4 Scenario as the Main Flow Page

Scenario Process Flow is the main interactive visual.

Selecting a Process must preserve the whole Scenario context.

Process detail supports:

```text
Business
Data
Code
```

and defaults to `Business`.

Technical chains use progressive expansion rather than default full call graphs.

### 19.5 Local Context First

Use Focus/Spotlight behavior:

```text
selected entity
+ direct upstream/downstream
+ direct related objects/data/technical nodes
```

Fade unrelated information.

Expand farther only on user request.

Do not use a giant global knowledge graph as the default view.

### 19.6 Search

Global search is mandatory.

Index both semantic and technical identifiers:

```text
business_name
canonical_id
alias/source_name
class
method
table
field
rpc
mq
job
```

Group results into meaningful categories such as:

```text
Business
Flows
Objects
Code
Data
Integration
```

A technical search result should navigate into its business context when canonical relations allow it.

### 19.7 Execution Chain Visualization

Cross-project flows should prefer Project swimlanes.

Distinguish synchronous and asynchronous boundaries visually.

Do not confuse Project boundaries with Process boundaries.

### 19.8 Evidence

Evidence stays reachable but secondary.

Prefer an evidence drawer/panel rather than placing source paths directly throughout the main business flow.

### 19.9 Visual Style

Prefer a calm developer intelligence console:

- dark workspace;
- restrained blue/cyan business emphasis;
- green/teal object/data emphasis;
- neutral technical nodes;
- amber PARTIAL/UNRESOLVED state;
- subtle grid/glow;
- readable contrast;
- monospace technical identifiers.

Avoid decorative cyberpunk effects, 3D graphs, or continuous motion that reduces readability.

### 19.10 Static Derived Output

HTML should work without a required backend.

Recommended derived output:

```text
docs/code-atlas/html/
├── index.html
├── assets/
│   ├── app.js
│   └── app.css
└── data/
    ├── atlas.json
    └── search-index.json
```

Canonical Markdown remains the only persistent knowledge truth.

The complete viewer behavior is defined in:

```text
skills/ca-visual/references/html-viewer-guidelines.md
```

## 20. Analysis Result

- VERIFIED
- PARTIAL
- UNRESOLVED

另外严格区分：

- NOT_FOUND：当前范围没有发现
- UNRESOLVED：已知存在但无法解析

## 21. Final Philosophy

> Discover → Model → Trace → Challenge → Converge → Audit → Publish

> 从业务可以下钻到代码。

> 从代码可以反查业务。

> 从状态变化可以找到 Process。

> 所有结论均可追溯到当前代码事实。

## 22. Design Decisions

Important architectural choices are recorded as ADRs under:

```text
docs/decisions/
```

Current accepted decisions include:

- business semantics independent from technical boundaries;
- Markdown as Canonical Knowledge;
- full analysis/full generation;
- Scenario directly owns Process Flow;
- Process and Execution Chain remain separate;
- Business Object owns canonical Lifecycle;
- shallow Sub-process hierarchy;
- categorical evidence resolution instead of numeric confidence;
- one-way canonical relationship ownership;
- one analysis workspace publishes one Canonical Atlas at the workspace root;
- Scenario canonicalization prevents condition-combination explosion;
- Semantic Canonicalization is a unified repository-wide reasoning protocol.
- Semantic identity rules stay intentionally lightweight.
- HTML Atlas is business-first, interactive, searchable, locally focused, and derived only from Canonical Markdown.

Agents must read the relevant ADR before changing one of these architectural boundaries.

## 23. Optimization Philosophy

Optimization priority:

```text
Factual correctness
→ Business-semantic correctness
→ Knowledge consistency
→ Traceability
→ Human readability
→ Maintainability
→ Tool/token efficiency
→ Runtime performance
```

Do not optimize runtime, prompt length, or tool-call count by weakening semantic gates or evidence requirements.

Prefer improving promotion criteria, evidence contracts, ownership boundaries, and reconciliation behavior over accumulating isolated exception rules.

## 24. Documentation Roles

```text
README.md / README.zh-CN.md
→ user-facing introduction and install/update/remove/output usage

AGENTS.md
→ repository constitution and agent optimization guidance

docs/code-atlas-design.md
→ current architecture specification

docs/decisions/
→ why important architecture decisions exist

skills/**/SKILL.md
→ runtime instructions for each Skill
```
