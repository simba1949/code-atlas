# Code Atlas Knowledge Model

## Canonical model

```text
Entity + Relation + Evidence
```

## Business hierarchy

```text
Business Domain
  ↓
Business Subdomain [optional]
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

When no meaningful Subdomain exists:

```text
Domain
→ Capability
```

Important:

> Subdomain publication is optional, but Subdomain Evaluation is mandatory for every Domain.

## Business Object model

```text
Business Object
  ↓
Lifecycle
  ↓
State
  ↓
State Transition
```

Business Objects may relate to Domain / Subdomain / Capability / Scenario / Process without becoming children in the main hierarchy.

## Independent Markdown entities

Use standalone Markdown for:

- System
- Project
- Business Domain
- Business Object
- Lifecycle
- Scenario
- Process
- Execution Chain
- Common Chain
- Core Business Table when needed

## Embedded stable-ID knowledge

Keep these embedded by default:

- Business Subdomain
- Capability
- Sub-process
- Step
- State
- State Transition
- Business Rule

Knowledge Entity does not imply Markdown file.

## Main ownership

- System / Project / Module → `ca-project`
- Domain / Subdomain / Capability / Business Object / Lifecycle → `ca-business`
- Scenario / Process / Sub-process / Step → `ca-scenario`
- Execution Chain / Common Chain → `ca-chain`

## Topology convergence

Before publication use:

```text
business-topology-convergence.md
```

Required discovery direction:

```text
Business Actions
→ Capabilities
→ Subdomains
→ Domains
→ Scenario Enumeration
```

Required publication direction:

```text
Domain
→ Subdomain [optional]
→ Capability
→ Scenario
→ Process
→ Execution Chain
```

Do not flatten the default business topology with convenience edges.

## Shared semantic rules

Use:

```text
semantic-canonicalization-protocol.md
semantic-identity-rules.md
```

Canonicalization outcomes remain:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

Markdown remains Canonical Knowledge.
