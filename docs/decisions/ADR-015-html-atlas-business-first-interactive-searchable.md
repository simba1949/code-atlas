# ADR-015: HTML Atlas Is Business-First, Interactive, and Searchable

## Status

Accepted

## Context

Canonical Markdown preserves the verified knowledge model, but a Markdown-shaped HTML site does not provide enough value for understanding a complex codebase.

Engineers need to:

- understand the system's business before reading implementation detail;
- follow one real Scenario without losing context;
- progressively drill into data and code;
- understand cross-project execution;
- search from either business or technical terminology;
- navigate back from code to the business it implements.

A giant global graph or documentation-style page hierarchy does not satisfy these needs well.

## Decision

The Code Atlas HTML viewer is an interactive business-understanding workspace derived only from Canonical Markdown.

It follows six principles:

```text
Business First
Context Preserved
Progressive Disclosure
Bidirectional Navigation
Search First
Interactive but Calm
```

### Default navigation

Primary user entry points are:

```text
Business Map
Flows
Business Objects
Technical Map
Data
```

The HTML viewer does not expose every internal Knowledge Entity as a top-level navigation item.

### Scenario is the primary flow page

Scenario shows Canonical Business Process Flow.

Selecting a Process preserves Scenario context and supports:

```text
Business
Data
Code
```

with Business selected by default.

### Technology is progressive

The default technical view shows the key Execution Chain.

Full method-level detail is expanded only on demand.

Cross-project flows prefer Project swimlanes.

### Local focus beats global graph

The viewer focuses on the selected entity and directly related context.

Large all-system graphs are not the default navigation.

### Search is mandatory

Global search covers business names and technical identifiers such as:

```text
Class
Method
Table
RPC
MQ
Job
```

A technical result should navigate into business context when canonical relations allow it.

### Reverse navigation is mandatory

Support paths such as:

```text
Method
→ Execution Chain
→ Process
→ Scenario
→ Capability
```

and:

```text
Table
→ Business Object
→ Process
→ Scenario
```

### Evidence remains secondary but reachable

Evidence is available through a detail drawer/panel rather than dominating business flow views.

### Visual style

Use a restrained modern developer-console aesthetic.

Technology-looking visuals must improve comprehension rather than merely add decoration.

### Static derived output

The viewer should work without a required backend.

Derived JSON/search indexes are allowed, but Canonical Markdown remains the only persistent source of truth.

## Rejected Alternatives

### HTML as a Markdown Skin

Rejected because it does not materially improve codebase understanding.

### Giant Global Knowledge Graph

Rejected as the default because large graphs rapidly become unreadable.

### Technology-first Navigation

Rejected because it recreates the codebase structure rather than explaining the business behind it.

### 3D / Decorative Visualizations as Core Navigation

Rejected because visual novelty is less important than long-session readability.

### Backend-dependent Viewer

Rejected as the default because a static output is easier to publish, share, and use locally.

## Consequences

- `ca-visual` becomes an interaction/presentation Skill rather than a document renderer;
- Scenario Process Flow is the central flow experience;
- search is a first-class feature;
- technical information is progressively revealed;
- business-to-code and code-to-business navigation are both required;
- visual design remains intentionally restrained;
- HTML stays derived and may always be regenerated from Canonical Markdown.
