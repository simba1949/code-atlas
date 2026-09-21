# ADR-006: Business Object Owns the Canonical Lifecycle

## Status

Accepted

## Context

Process, Scenario, database status fields, and Business Objects can all expose state-related information.

If every Process or Scenario defines its own state model, transition truth becomes duplicated and contradictory.

A table is also not equivalent to a Business Object: one object can span several tables/services and several state dimensions.

## Decision

Canonical Lifecycle belongs to the Business Object.

Use:

```text
Business Object
→ Lifecycle
→ State
→ State Transition
```

Processes and activities reference Lifecycle impact / State Transitions.

Lifecycle promotion requires:

1. confirmed Business Object;
2. state dimension;
3. state values;
4. evidenced transitions.

A status field or enum alone does not prove transition arrows.

Different fields such as:

```text
status
pay_status
audit_status
```

remain separate dimensions unless code proves otherwise.

## Rejected Alternatives

### Process-Owned State Machines

Rejected because the same object may be affected by many Processes.

### Table = Business Object

Rejected because physical persistence does not define business identity.

### Infer Every Enum Transition

Rejected because declared values do not prove allowed transition paths.

## Consequences

Lifecycle knowledge is centralized, reusable, and auditable.

Process documents only describe verified Lifecycle impact.
