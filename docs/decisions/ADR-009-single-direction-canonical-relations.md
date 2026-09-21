# ADR-009: Single-Direction Canonical Relationship Ownership

## Status

Accepted

## Context

Many knowledge relationships are naturally navigable in both directions.

If both directions are manually stored, updates can leave them inconsistent.

Example:

```text
Scenario → Process
Process → Scenario
```

stored independently can drift.

## Decision

Each relationship has one canonical storage direction.

Examples:

```text
Domain             → Capability
Scenario           → Capability
Scenario           → Process Flow
Process            → Sub-process / Step
Process            → Business Object
Lifecycle          → Business Object
Lifecycle          → State / Transition
Business Object    → Table
Execution Chain    → Process
Execution Chain    → Activity
Execution Chain    → Common Chain
```

Reverse relations are derived by `ca-knowledge` / rendering/navigation layers.

## Rejected Alternatives

### Dual Manual Storage

Rejected because it creates duplicate truth and synchronization burden.

### Let Every Skill Write Both Directions

Rejected because ownership becomes ambiguous.

## Consequences

Every new relation must define its canonical owner/direction before publication.

Reverse navigation remains rich without duplicating semantic state.
