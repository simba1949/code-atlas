# ADR-010: Analysis Workspace Root Owns the Canonical Atlas

## Status

Accepted

## Context

Code Atlas can analyze either one Project or several Projects that participate in the same business flow.

For example:

```text
settleWork/
├── emp-account/
└── site-account/
```

A cross-project Scenario or Process may synchronously move from `site-account` into `emp-account`.

If each Project receives an independent `docs/code-atlas/`, one business flow becomes physically fragmented along repository boundaries:

```text
site-account/docs/code-atlas/
emp-account/docs/code-atlas/
```

That conflicts with the principle that technical boundaries must not break business continuity.

At the same time, using a common parent directory must not imply that every sibling repository is automatically in scope.

## Decision

Every Code Atlas analysis resolves two separate concepts:

```text
Selected Project Set
= repositories/projects whose code belongs to this analysis

Analysis Workspace Root
= common publication root for the complete Canonical Atlas
```

One analysis scope publishes exactly one Canonical Atlas:

```text
<analysis-workspace-root>/docs/code-atlas/
```

Single-project example:

```text
emp-account/
├── src/
└── docs/
    └── code-atlas/
```

Multi-project example:

```text
settleWork/
├── emp-account/
├── site-account/
└── docs/
    └── code-atlas/
```

Project-specific technical knowledge remains separate inside the shared Atlas:

```text
docs/code-atlas/projects/
├── emp-account.md
└── site-account.md
```

Cross-project Scenarios, Processes, Business Objects, Lifecycles, and Execution Chains are modeled once at workspace scope.

A synchronous Project/RPC boundary does not split an Execution Chain by itself.

The Analysis Workspace Root should be resolved in this order:

1. explicit user-selected workspace root;
2. current workspace root containing the selected Projects;
3. nearest common parent of selected Project roots when it clearly represents the requested scope.

If the root is ambiguous, publication should wait for root resolution rather than silently creating separate per-Project Atlases.

## Rejected Alternatives

### One Atlas Per Project

Rejected because it fragments one business model along technical repository boundaries and creates duplicated/incomplete cross-project knowledge.

### Analyze Every Sibling Repository Under the Workspace Root

Rejected because publication root and analysis scope are different concepts.

Only selected/in-scope Projects are analyzed.

### Split Cross-Project Chains at Repository Boundaries

Rejected because Project boundaries are technical boundaries. Chain splitting depends on execution-context boundaries, not repository changes.

## Consequences

- Multi-project analysis has one coherent source of Canonical Markdown.
- Project documents remain individually navigable under `projects/`.
- Scenario/Process/Lifecycle/Chain knowledge can naturally span Projects.
- Reverse navigation can cross Projects without merging multiple independent Atlases.
- Tools must carry both `analysis_workspace_root` and the selected Project set.
