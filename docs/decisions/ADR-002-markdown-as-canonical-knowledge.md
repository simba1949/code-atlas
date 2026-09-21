# ADR-002: Markdown as Canonical Knowledge

## Status

Accepted

## Context

Code Atlas produces structured knowledge and a human-readable HTML Atlas.

If both Markdown and HTML independently contain facts, the project creates two truth sources that can drift.

A hidden internal model that directly produces HTML also makes review, Git diff, manual inspection, and AI collaboration harder.

## Decision

Use:

```text
Markdown = Canonical Published Knowledge
HTML     = Derived Viewer
```

HTML must be generated from the current run's canonical Markdown.

The visual layer may derive navigation and presentation, but it may not invent independent semantic facts.

## Rejected Alternatives

### Analysis Workspace → HTML Directly

Rejected because users cannot reliably review or version hidden semantic state.

### Markdown + HTML as Co-equal Sources

Rejected because duplicate truth causes drift and reconciliation complexity.

## Consequences

Canonical Markdown must remain complete enough to regenerate the HTML Atlas.

Visual improvements must not require a second semantic database unless a future ADR explicitly changes this architecture.
