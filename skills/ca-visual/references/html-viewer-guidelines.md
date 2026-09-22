# HTML Viewer Guidelines

## Goal

The HTML Atlas is an interactive code-understanding workspace.

It is not a Markdown skin and not a giant knowledge graph.

A first-time engineer should be able to:

```text
30 seconds
→ understand what business the system does

3 minutes
→ enter one real Scenario and understand its Process Flow

10 minutes
→ drill from business to data/code and navigate back
```

Core principle:

> Default to business, reveal technology on demand.
>
> Default to local context, expand scope on demand.
>
> Every page must support business → flow → object/data → code drill-down and reverse navigation.

The visual style should feel like a modern developer intelligence console:

- technical;
- calm;
- high information density;
- interactive;
- readable for long sessions.

Avoid decorative cyberpunk effects that reduce readability.

---

## 1. Six UX Principles

### Business First

Default views show:

```text
Domain / Subdomain / Capability
→ Scenario
→ Process
→ Business Object / Lifecycle
```

Do not lead with Controller, Service, Mapper, RPC, MQ, or table lists.

### Context Preserved

Drill-down must not lose the current business context.

Selecting a Process inside a Scenario should keep the Scenario flow visible while showing Process details.

### Progressive Disclosure

Default to the minimum information needed to understand the current business question.

Expand technical detail only when requested.

Preferred depth:

```text
Business Flow
→ Key Technical Flow
→ Full Technical Flow
```

### Bidirectional Navigation

Support both:

```text
Business → Code
```

and:

```text
Code → Business
```

Examples:

```text
Scenario
→ Process
→ Execution Chain
→ Method

Method
→ Execution Chain
→ Process
→ Scenario
→ Capability
```

### Search First

Global search is a primary entry point, not an auxiliary feature.

The user must be able to search:

- business name;
- canonical ID;
- alias/source name;
- Class;
- Method;
- Table;
- Field when indexed;
- RPC;
- MQ;
- Job.

### Interactive but Calm

Use interaction to explain relationships, not to decorate the page.

Good interaction:

- hover highlight;
- upstream/downstream highlight;
- focus/spotlight mode;
- collapsible technical chains;
- evidence drawer;
- smooth local expansion;
- keyboard search.

Avoid excessive animation, particle backgrounds, 3D graphs, or large always-moving diagrams.

---

## 2. Global Layout

Use a stable three-column workspace:

```text
┌─────────────────────────────────────────────────────────────────┐
│ Code Atlas   [ Search business / class / method / table / ... ]│
│ workspace · branch · commit                         result       │
├───────────────┬────────────────────────────────┬────────────────┤
│ Navigator     │ Main Canvas                    │ Context Panel  │
│               │                                │                │
│ Business Map  │ Current interactive view       │ Current item   │
│ Flows         │                                │ Relations      │
│ Objects       │                                │ Data impact    │
│ Technical     │                                │ Implementation │
│ Data          │                                │ Evidence       │
└───────────────┴────────────────────────────────┴────────────────┘
```

Responsibilities:

```text
left
→ where can I go?

center
→ what is happening here?

right
→ what is related to the selected thing?
```

Keep Breadcrumb visible:

```text
Domain
> Subdomain
> Capability
> Scenario
> Process
> Code
```

Breadcrumb items must be clickable.

---

## 3. Primary Navigation

Prefer five user-facing entry points:

```text
Business Map
Flows
Business Objects
Technical Map
Data
```

Do not expose every internal knowledge type as a top-level menu item.

### Business Map

Primary hierarchy:

```text
Domain
→ Subdomain [optional]
→ Capability
```

### Flows

Primary hierarchy:

```text
Capability
→ Scenario
→ Process
```

### Business Objects

Show:

```text
Business Object
→ Lifecycle
→ related Capabilities / Scenarios / Processes
```

### Technical Map

Show:

```text
Project
→ technical entry
→ Execution Chain
→ RPC / MQ / Job / external boundary
```

### Data

Support:

```text
Table
→ Business Object
→ Process
→ Scenario
```

---


## 3.1 Topology Integrity

The default business hierarchy is:

```text
Domain
→ Subdomain [optional]
→ Capability
→ Scenario
→ Process
```

When Subdomains exist, render them as an explicit grouping layer.

Never flatten the primary map with:

```text
Domain → Scenario
Domain → Process
Capability → Process
```

These cross-level relationships may appear only in search, context, reverse navigation or impact views.

## 4. Home / Business Map

The home page must answer:

> What business does this system implement?

Prefer an interactive map such as:

```text
Business Domain
     │
     ├── Capability Group / Subdomain
     │       ├── Capability
     │       └── Capability
     │
     └── Capability Group / Subdomain
             ├── Capability
             └── Capability
```

Hover on a Capability may show compact information:

```text
Capability
Scenarios
Processes
Business Objects
Projects
analysis result
```

Clicking should focus into the selected Capability.

Do not make project statistics the primary visual.

---

## 5. Focus and Local Expansion

Use a Focus Mode.

When an entity is selected:

- highlight it;
- highlight direct upstream/downstream relations;
- fade unrelated content;
- keep parent business context visible.

Allow optional expansion:

```text
Current
→ 1-hop related
→ 2-hop related
→ all related
```

Do not render a huge all-system graph by default.

Prefer contextual graphs.

---

## 6. Capability View

Capability view answers:

> What does this capability do, and what real business paths implement it?

Default content:

- business meaning;
- Business Objects;
- Scenarios;
- important Business Rules;
- analysis result;
- unresolved points when present.

Do not lead with technical implementation.

---


## 6.1 Ownership vs Collaboration

The viewer must distinguish:

```text
business ownership
from
cross-domain collaboration
```

A Scenario is displayed under exactly one owning Capability/Domain.

When Processes invoke other Domains:

- keep the owning business path in the main flow;
- show collaborator Domain badges/lanes;
- do not clone the Scenario into collaborator Domains;
- do not flatten ownership hierarchy.

Example:

```text
Withdrawal [Funds Transaction owner]
   → [Funds Account] Freeze
   → [Funds Settlement] Payout
   → [Funds Account] Finalize
```

The visual should make collaboration obvious while ownership stays stable.

## 7. Scenario View — Primary Flow Page

Scenario is the main visual page of the HTML Atlas.

Show the canonical Business Process Flow prominently:

```text
Process A
   ↓
Process B
   ↓
Process C
   ↓
Process D
```

Selecting a Process must keep the full Scenario flow visible.

Right-side context may show:

- business goal;
- Business Objects;
- Business Rules;
- lifecycle/data impact;
- implementation summary;
- evidence action.

Do not expand full Java call graphs in the Scenario overview.

### Scenario node states

Keep status subtle:

```text
VERIFIED
PARTIAL
UNRESOLVED
```

Do not show numeric confidence.

---

## 8. Process View — Business / Data / Code

A selected Process should support three primary tabs:

```text
Business
Data
Code
```

Default tab:

```text
Business
```

### Business

Show:

- goal;
- start/end boundary;
- input/output;
- Business Objects;
- Business Rules;
- Sub-process/Steps when useful.

### Data

Show:

- objects/tables touched;
- important field/state effects;
- lifecycle transitions;
- read/write direction.

### Code

Show key implementation first:

```text
Entry
→ important Service
→ RPC/MQ/Job boundary
→ Repository/DB
```

Provide:

```text
Expand full chain
```

instead of rendering every method by default.

---

## 9. Execution Chain

Prefer a project swimlane for cross-project flows:

```text
project-a          project-b          project-c
   │                   │                  │
   ●──── sync RPC ────▶●                  │
                       ●---- MQ ---------▶●
```

Visually distinguish execution boundaries:

```text
sync call
────────────▶

async / MQ
- - - - - - ▶

callback
············▶
```

Do not split a synchronous business flow merely because it crosses Projects.

Support collapse/expand for Common Chains.

---

## 10. Business Object View

Use the selected Business Object as the visual center.

Show nearby business relations only.

Example:

```text
Capability
    │
    ▼
[Business Object]
    │
    ├── related Business Object
    ├── Lifecycle
    └── Processes
```

Prefer direct business wording on edges:

```text
发生交易
形成账务流水
引用原交易
```

Do not require a large generic relationship taxonomy in the UI.

---

## 11. Lifecycle View

Use an interactive state diagram as the primary visual.

Clicking or hovering a transition may show:

- triggering Process;
- Business Rule;
- state write;
- implementing Execution Chain;
- evidence.

Never draw transitions that are not in Canonical Markdown.

---

## 12. Reverse Navigation

Every technical item should provide a business route when derivable.

Examples:

```text
Method
→ Execution Chain
→ Process
→ Scenario
→ Capability
```

```text
Table
→ Business Object
→ Process
→ Scenario
```

Provide a visible action such as:

```text
Back to Business
```

This is semantic navigation, not browser history.

---

## 13. Global Search

Keep a global search entry visible at the top.

Recommended keyboard access:

```text
Ctrl/Cmd + K
/
```

Search across:

```text
business_name
canonical_id
alias
source_name
class
method
table
field
rpc
mq
job
```

Results must be grouped by meaning:

```text
Business
Flows
Objects
Code
Data
Integration
```

Example search for `emp_account` may return:

```text
Business Object
业务员钱包

Table
emp_account

Process
冻结余额

Scenario
银行卡提现

Code
EmpAccountRepository
```

Search result click should open the relevant semantic context.

Example:

```text
WithdrawService.freeze()
```

should open its Process/Scenario context and focus the Code tab when possible.

### Search index

The generated viewer may build a static search index from Canonical Markdown.

Example implementation shape:

```json
{
  "id": "PROCESS-freeze-balance",
  "type": "process",
  "businessName": "冻结余额",
  "sourceNames": ["WithdrawService.freeze"],
  "keywords": ["冻结", "余额", "withdraw", "freeze"],
  "path": ["资金交易", "提现", "银行卡提现"]
}
```

This is derived presentation data, not Canonical Knowledge.

---

## 14. Evidence Interaction

Evidence must always be reachable but should not dominate the default view.

Prefer:

```text
[View Evidence]
```

opening a side drawer/panel with:

- Project;
- file;
- symbol;
- SQL/table/config anchor;
- source context;
- analysis result.

Do not fill the main business flow with source-path noise.

---

## 15. Visual Language

Recommended visual semantics:

```text
Business
→ blue/cyan family

Business Object / Data
→ green/teal family

Technical
→ slate/neutral family

Partial / Unresolved
→ amber/warning family
```

Use red sparingly for blocking/broken information.

Do not assign a unique color to every entity type.

### Style

Preferred:

- dark developer workspace;
- restrained glow;
- subtle grid or depth;
- clear panel boundaries;
- readable contrast;
- monospace for source names/technical identifiers;
- UI font for business names.

Avoid:

- full-screen decorative animation;
- cyberpunk overload;
- unreadable neon;
- giant node clouds;
- 3D graphs as the default navigation.

---

## 16. Useful Micro-interactions

Recommended:

- selected node glow;
- hover relation highlight;
- upstream/downstream highlight;
- unrelated node fade;
- smooth expand/collapse;
- one-time pulse when locating a search result;
- evidence drawer;
- focus/spotlight mode;
- keyboard escape to close focus/detail.

Optional shortcuts:

```text
Ctrl/Cmd + K → Search
/            → Search
Esc          → Close detail / Spotlight
B            → Business tab
D            → Data tab
C            → Code tab
```

Do not require shortcuts for basic navigation.

---

## 17. Generated Output

Prefer static output without a required backend.

Recommended:

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

A single-file HTML build is also allowed when the viewer remains maintainable.

Rules:

- Canonical Markdown is still the source of truth.
- `atlas.json` and `search-index.json` are derived.
- HTML/JSON must never independently infer missing semantics.
- Rebuild derived output from the current full Markdown snapshot.

---

## 18. Desktop-first

Optimize first for software-engineering desktop use.

Primary target:

```text
desktop / large-screen workspace
```

Mobile may remain readable, but do not sacrifice graph/navigation quality to achieve mobile-first layout.

---

## 19. First-version Priority

If implementation time is limited, prioritize exactly these capabilities:

1. Business Map.
2. Scenario Process Flow.
3. Process Business/Data/Code tabs.
4. Business Object + Lifecycle navigation.
5. Execution Chain project swimlane.
6. Global search + reverse navigation.

Do not prioritize:

- 3D knowledge graphs;
- advanced analytics dashboards;
- decorative animation;
- large statistical widgets;
- complex filters.

---

## 20. Acceptance Criteria

The generated HTML is acceptable only if:

- the first page explains the system's business before technical structure;
- a user can reach a Scenario flow in a few interactions;
- selecting a Process preserves Scenario context;
- technical detail is collapsed by default;
- cross-project Chain boundaries are visually obvious;
- global search supports business and technical terms;
- a Method/Table can navigate back to business context;
- Evidence is accessible without overwhelming the main view;
- VERIFIED/PARTIAL/UNRESOLVED are visible without numeric scoring;
- no visualization invents facts not present in Canonical Markdown;
- the viewer remains useful without a backend service.
