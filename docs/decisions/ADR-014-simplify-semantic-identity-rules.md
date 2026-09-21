# ADR-014: Keep Semantic Identity Rules Intentionally Lightweight

## Status

Accepted

## Supersedes

ADR-013.

ADR-012 remains valid in principle: Semantic Canonicalization is still repository-wide, but ADR-014 simplifies how it is executed.

## Context

The semantic model became increasingly difficult to understand because identity reasoning accumulated:

- Identity Kernel;
- Context/Representation/Unknown classifications;
- Decisive vs supporting evidence levels;
- ten shared tests;
- Identity Resolution Records;
- complex registry concepts and states.

These mechanisms were internally coherent, but they created a second meta-model that maintainers had to learn before using Code Atlas.

Code Atlas exists to simplify understanding of complex codebases, not to become a complex knowledge-management framework itself.

## Decision

Keep only:

```text
Semantic Canonicalization Protocol
+
Semantic Identity Rules
```

The common flow is:

```text
Fact
→ Candidate
→ Canonicalize
→ Verify
→ Publish
```

Canonicalization has four outcomes only:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

Each knowledge type defines only:

```text
Identity
Ignore Differences
Promotion
```

Use four common identity questions:

1. Same Meaning Test;
2. Independent Existence Test;
3. Independent Lifecycle Test;
4. Independent Goal / Result Test.

Scenario keeps one additional specialized test:

```text
De-dimension Test
```

### Registry stays lightweight

`ca-knowledge` maintains only the consistency needed for a full run:

```text
Canonical ID → Knowledge Entity
Alias / Source Name → Canonical ID
Unresolved → open question
```

It does not require a complex registry state machine, Candidate index hierarchy, or published-index architecture.

### Business concepts stay lightweight

Do not introduce fixed taxonomies for:

- Business Object roles/archetypes;
- Business Relationship kinds;
- Business Rule subtypes;
- Business Event entities;

unless real repeated use demonstrates a clear need.

Business Rules remain embedded by default.

Business Relationships use direct business wording by default.

## Rejected Alternatives

### Keep the Full Identity Contract Model

Rejected because its precision did not justify the conceptual cost for the current project.

### Remove Semantic Canonicalization Entirely

Rejected because duplicate semantic identities and Scenario explosion remain real problems.

### Use Numeric Similarity Scores

Rejected because semantic identity still requires evidence-backed judgment.

## Consequences

- fewer concepts for Claude Code, Codex and human maintainers to learn;
- fewer duplicated rules across Skills;
- `UNRESOLVED` remains the safe alternative to guessing;
- Scenario anti-explosion behavior remains;
- Business Object deduplication remains;
- future complexity must be justified by repeated real analysis problems rather than anticipated needs.
