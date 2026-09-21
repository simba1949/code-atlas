# ADR-003: Full Analysis and Full Generation by Default

## Status

Accepted

## Context

Incremental knowledge updates appear efficient, but business semantics are highly interconnected.

Changing one code path can alter:

- Scenario boundaries;
- Process boundaries;
- Business Object interpretation;
- Lifecycle transitions;
- Chain mappings;
- reverse navigation.

A partially updated Atlas can silently mix old and new semantics.

## Decision

Every normal Code Atlas run uses:

```text
Current Code
→ Full Fact Discovery
→ Full Modeling
→ Full Trace
→ Challenge
→ Converge
→ Audit
→ Full Markdown Generation
→ Full HTML Generation
```

The internal reasoning process may iterate, but publication is a coherent full snapshot.

## Rejected Alternatives

### Incremental Dirty-Knowledge Mutation

Rejected as the default because impact propagation and stale mixed-state knowledge substantially increase consistency risk.

### Patch Existing Markdown in Place

Rejected because a locally correct edit can leave reverse/cross-entity semantics stale.

## Consequences

Correctness and consistency are prioritized over runtime/token efficiency.

Future incremental analysis requires a new ADR with a proven invalidation/dependency model.
