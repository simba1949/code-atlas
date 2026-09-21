# ADR-007: Keep Sub-process Hierarchy Shallow

## Status

Accepted

## Context

Allowing arbitrary recursive Sub-process nesting makes it easy for Code Atlas to recreate method-call trees instead of a business model.

Deep hierarchical flows also become difficult for humans to navigate.

## Decision

Default to:

```text
Process
├── Step
├── Sub-process
│   ├── Step
│   └── Step
└── Step
```

Sub-process does not recursively contain Sub-process by default.

If deeper structure appears necessary:

1. collapse local detail into Steps;
2. split sibling Sub-processes;
3. promote independently valuable/cross-Scenario behavior to Process.

## Rejected Alternatives

### Unlimited Recursive Sub-process

Rejected because it encourages technical decomposition and produces unstable arbitrary depth.

### Every Method = Step/Sub-process

Rejected because technical call depth is not business semantic depth.

## Consequences

Process documents stay readable and business-oriented.

Detailed technical nesting remains in Execution Chains.
