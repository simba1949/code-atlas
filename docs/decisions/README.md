# Architecture Decision Records

This directory records architectural decisions that future maintainers and coding agents must understand before changing Code Atlas.

An ADR captures:

- context;
- accepted decision;
- why the decision exists;
- rejected alternatives;
- consequences.

Accepted ADRs are architectural constraints until explicitly superseded by a newer ADR.

## Current ADRs

| ADR | Status | Decision |
|---|---|---|
| [ADR-001](./ADR-001-business-semantics-over-technical-boundaries.md) | Accepted | Business semantics are independent from technical boundaries |
| [ADR-002](./ADR-002-markdown-as-canonical-knowledge.md) | Accepted | Markdown is Canonical Knowledge |
| [ADR-003](./ADR-003-full-analysis-full-generation.md) | Accepted | Full analysis/full generation is the default execution model |
| [ADR-004](./ADR-004-scenario-directly-orchestrates-process-flow.md) | Accepted | Scenario directly orchestrates Process Flow |
| [ADR-005](./ADR-005-separate-process-and-execution-chain.md) | Accepted | Process and Execution Chain remain separate concepts |
| [ADR-006](./ADR-006-business-object-owns-lifecycle.md) | Accepted | Canonical Lifecycle belongs to Business Object |
| [ADR-007](./ADR-007-shallow-sub-process-hierarchy.md) | Accepted | Sub-process hierarchy is intentionally shallow |
| [ADR-008](./ADR-008-evidence-resolution-not-numeric-confidence.md) | Accepted | Evidence gates and categorical resolution replace numeric confidence |
| [ADR-009](./ADR-009-single-direction-canonical-relations.md) | Accepted | Knowledge relationships use one canonical storage direction |
| [ADR-010](./ADR-010-analysis-workspace-root-owns-canonical-atlas.md) | Accepted | One analysis workspace publishes one Canonical Atlas at the workspace root |
| [ADR-011](./ADR-011-scenario-canonicalization-prevents-combinatorial-explosion.md) | Accepted | Scenario identity comes from canonical business-path divergence, not condition combinations |
| [ADR-012](./ADR-012-unified-semantic-canonicalization-protocol.md) | Accepted; simplified by ADR-014 | Semantic Canonicalization is repository-wide |
| [ADR-013](./ADR-013-semantic-identity-contracts-govern-merge-split.md) | Superseded by ADR-014 | Earlier detailed Semantic Identity Contract model |
| [ADR-014](./ADR-014-simplify-semantic-identity-rules.md) | Accepted | Semantic identity rules stay intentionally lightweight |
| [ADR-015](./ADR-015-html-atlas-business-first-interactive-searchable.md) | Accepted | HTML Atlas is business-first, interactive, searchable, and progressively reveals technology |
| [ADR-016](./ADR-016-business-topology-convergence-before-publication.md) | Accepted | Business topology converges bottom-up before top-down publication |
| [ADR-017](./ADR-017-domain-ownership-follows-business-responsibility.md) | Accepted | Domain ownership follows business responsibility, not shared data mutation |

## Changing an Accepted Decision

Do not edit an accepted ADR to make history disappear.

If architecture changes:

1. create a new ADR;
2. reference the ADR being superseded;
3. change the old ADR status to `Superseded by ADR-XXX`;
4. explain the new context/evidence;
5. update `docs/code-atlas-design.md`;
6. update affected Skills together.
