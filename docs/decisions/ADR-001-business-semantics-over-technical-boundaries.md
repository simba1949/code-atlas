# ADR-001: Business Semantics Over Technical Boundaries

## Status

Accepted

## Context

Large systems are physically divided into repositories, projects, modules, services, JVMs, packages, RPC interfaces, and database schemas.

Those boundaries are implementation and organizational choices. They do not reliably represent business boundaries.

A single business Process may begin in one project, synchronously cross RPC into another project, update a database, and still remain one continuous business operation.

## Decision

Maintain two independent maps:

```text
Technical:
System / Workspace
→ Project / Repository
→ Module
→ Component
```

```text
Business:
Domain
→ Capability
→ Scenario
→ Process
```

Use Execution Chains to connect business concepts to technical implementation.

Do not split Domain, Capability, Scenario, or Process merely because code crosses:

- Project;
- Repository;
- Module;
- JVM;
- synchronous RPC;
- transaction;
- service class;
- method.

## Rejected Alternatives

### Project → Domain

Rejected because one project may implement several business domains, while one business domain may span several projects.

### Module → Capability

Rejected because module decomposition commonly reflects technical layering or ownership rather than stable business capability.

## Consequences

Code Atlas must be able to trace business continuity across technical boundaries.

Technical topology remains important, but it is not allowed to dictate business semantics.
