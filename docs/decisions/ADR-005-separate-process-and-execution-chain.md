# ADR-005: Separate Process from Execution Chain

## Status

Accepted

## Context

Both Process and Execution Chain can be drawn as flows, which makes merging them attractive.

However, they answer different questions.

Process answers:

```text
What business flow is being performed?
```

Execution Chain answers:

```text
How does one technical execution context implement that business flow?
```

Their boundaries do not align.

## Decision

Keep:

```text
Process
= business flow unit with business goal, boundary, and result
```

separate from:

```text
Execution Chain
= continuous technical execution context implementing Process/Activity
```

One Process may map to several Execution Chains.

A new Execution Chain may begin at:

- MQ consumer;
- callback;
- scheduled job;
- independent retry worker;
- compensation worker;
- manual retrigger;
- DB-driven later worker.

Synchronous RPC may remain in the same Chain when provider code is visible and execution remains continuous.

## Rejected Alternatives

### Unified Flow Entity

Rejected because it mixes business semantics with technical execution mechanics.

### Split Process at Every MQ/RPC/Job Boundary

Rejected because technical boundaries do not automatically create new business goals/results.

## Consequences

Process documents stay business-centric.

Chain documents stay technical and evidence-heavy.

The mapping between them is explicit.
