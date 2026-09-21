# ADR-013: Semantic Identity Contracts Govern Merge/Split Decisions

## Status

Superseded by ADR-014

## Context

ADR-012 established Semantic Canonicalization as a repository-wide reasoning protocol.

A shared process is not enough unless every semantic knowledge type also defines what its identity means. Without a common identity contract, Owner Skills can still drift in how they classify differences, normalize evidence, merge candidates, split candidates, or handle ambiguity.

## Decision

Introduce the repository-wide Semantic Identity Contract:

```text
skills/code-atlas/references/semantic-identity-contract.md
```

It defines:

- Identity Kernel;
- Context Dimensions;
- Representation Dimensions;
- Decisive and Supporting Merge Evidence;
- Decisive and Supporting Split Evidence;
- reusable Identity Tests;
- unresolved conditions;
- the global Merge/Split decision algorithm.

### Difference Classification

Every observed difference must first be classified as:

```text
IDENTITY
CONTEXT
REPRESENTATION
UNKNOWN
```

Only proven `CONTEXT` and `REPRESENTATION` differences may be normalized.

`UNKNOWN` must not be silently discarded.

### Qualitative Identity Tests

Identity Tests return:

```text
DECISIVE_MERGE
SUPPORTS_MERGE
NEUTRAL
SUPPORTS_SPLIT
DECISIVE_SPLIT
CONFLICT
INSUFFICIENT
```

No numeric score or percentage determines semantic identity.

### No Default Merge or Split

```text
absence of split evidence
!= merge evidence

absence of merge evidence
!= split evidence
```

Insufficient evidence resolves to:

```text
UNRESOLVED
```

### Owner Authority Remains

The shared Identity Contract does not replace Owner Skills.

Each Owner applies the contract for its owned knowledge type and still owns:

- semantic identity resolution;
- Promotion Gate;
- acceptance/rejection of semantic challenges.

`ca-knowledge` may detect duplicates and maintain the canonical registry, but it does not become the business-semantic decision maker.

### Internal Resolution Records

Important Merge/Split decisions may create analysis-only Identity Resolution Records.

These records improve auditability but are not Canonical Knowledge Entities and are not automatically published into Markdown.

## Rejected Alternatives

### Numeric Merge/Split Scoring

Rejected because a single decisive semantic contradiction may invalidate many weak similarity signals.

### Merge Unless Proven Different

Rejected because false merge contaminates downstream knowledge.

### Split Unless Proven Equal

Rejected because Context and Representation differences would create duplicate semantic identities.

### Put Identity Reasoning Directly in Every Skill

Rejected because the same rules would drift.

### Expose Full Identity Resolution Records in Canonical Markdown

Rejected because canonical documents should remain user-facing knowledge rather than internal reasoning dumps.

## Consequences

- Merge/Split decisions become reproducible and auditable;
- `UNRESOLVED` is a first-class correctness outcome;
- technical/context differences are separated from identity differences;
- Owner Skills share one Test Library and decision vocabulary;
- new semantic entity types must define an Identity Contract before publication;
- Semantic Canonicalization becomes operational rather than purely architectural.


## Supersession Note

ADR-014 keeps the core goal of evidence-backed identity resolution but removes the heavier Identity Contract meta-model, multi-level test outcome vocabulary, and analysis-only Resolution Record as mandatory architecture.
