# ADR-004: Scenario Directly Orchestrates Process Flow

## Status

Accepted

## Context

A Scenario represents a stable business path under a Capability.

Earlier modeling could introduce an additional concept such as `Scenario Chain` between Scenario and Process.

That extra entity adds hierarchy without adding distinct semantic ownership.

## Decision

Use:

```text
Capability
→ Scenario
→ Process Flow
```

Scenario directly owns the ordered/conditional orchestration of Processes.

`Scenario Process Flow` is a view/section of Scenario, not an independent Knowledge Entity.

Scenario promotion requires evidence-backed:

1. Capability continuity;
2. real path selection;
3. material path divergence;
4. path stability/repeatability;
5. evidenced Process Flow.

## Rejected Alternatives

### Scenario → Scenario Chain → Process

Rejected because `Scenario Chain` duplicates the Scenario's orchestration role and increases cognitive load.

### Entry Type = Scenario

Rejected because HTTP/RPC/MQ/Job are technical triggers, not business variants by themselves.

### Success/Failure = Separate Scenario

Rejected by default because these are usually branches, states, or chain outcomes rather than stable business scenarios.

## Consequences

Scenario files must focus on business path selection and Process orchestration.

Technical implementation belongs in Execution Chains.
