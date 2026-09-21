# ADR-012: Semantic Canonicalization Is a Unified Reasoning Protocol

## Status

Accepted; execution model simplified by ADR-014

## Context

Code Atlas has multiple Owner Skills that recover business semantics from raw implementation evidence.

Local canonicalization rules emerged independently, for example:

- Scenario candidate normalization;
- duplicate Business Object detection;
- Rule deduplication;
- relationship deduplication.

This creates a risk that each Skill uses a different concept of:

- candidate;
- identity;
- normalization;
- merge/split;
- unresolved semantics;
- promotion.

The same real business concept can appear through many technical representations:

```text
classes
tables
interfaces
DTOs
configuration
conditions
state fields
MQ messages
callbacks
foreign keys
```

A repository-wide reasoning contract is therefore required.

## Decision

Semantic Canonicalization becomes a unified Code Atlas reasoning protocol.

Every semantic entity must pass:

```text
Fact
→ Candidate
→ Normalize
→ Resolve Identity
→ Owner Promotion Gate
→ Canonical Registration
```

before publication.

The full protocol is defined in:

```text
skills/code-atlas/references/semantic-canonicalization-protocol.md
```

### Canonicalization and Promotion Are Separate

Canonicalization answers:

> Are these candidates the same semantic identity or materially different identities?

Promotion answers:

> Is the semantic claim sufficiently proven to become Canonical Knowledge?

Both are required.

### Owner Skills Retain Semantic Authority

The unified protocol does not create a centralized semantic super-Skill.

Each Owner Skill:

- defines its entity identity signature;
- defines normalization dimensions;
- resolves identity for owned entities;
- runs its Promotion Gate.

`ca-knowledge` supports registry, duplicate detection, ownership, and reference consistency.

It must not decide owner-specific business semantics.

### Canonicalization Must Be Lossless

When candidates merge:

- aliases remain;
- source representations remain;
- evidence remains;
- contextual qualifiers remain;
- contradictions remain visible.

Canonicalization removes duplicate identities, not information.

### Allowed Resolution Outcomes

```text
MERGE_EXISTING
PROMOTE_NEW
REJECT_TECHNICAL_ONLY
UNRESOLVED
CHALLENGE
```

No numeric semantic confidence is introduced.

### New Semantic Entity Contract

A future semantic entity type is incomplete until it defines:

1. Owner Skill;
2. identity signature;
3. normalization dimensions;
4. Promotion Gate;
5. canonical relationship ownership;
6. ID rules;
7. audit rules.

## Rejected Alternatives

### Let Every Skill Define Its Own Canonicalization Semantics

Rejected because cross-Skill identity rules drift and duplicate knowledge becomes likely.

### Put All Semantic Decisions in `ca-knowledge`

Rejected because registry/consistency ownership is different from business-semantic ownership.

A centralized semantic judge would break Single Writer / Single Owner.

### Merge by Name or Structural Similarity

Rejected because similar names and schemas do not prove shared business identity.

### Split by Technical Boundary

Rejected because Project, Module, RPC, MQ, table, class, or package boundaries do not prove separate business identity.

### Use Numeric Confidence to Resolve Ambiguity

Rejected because a percentage does not replace evidence-backed identity resolution.

Use explicit `UNRESOLVED` state instead.

## Consequences

- all semantic Skills use one reasoning vocabulary;
- local canonicalization becomes a specialization of a shared protocol;
- duplicate representations can converge without losing evidence;
- new entity types have a consistent admission contract;
- cross-Skill challenges become easier to route;
- `Converge` has an explicit meaning: semantic canonicalization, conflict resolution, deduplication, and canonical registration;
- Canonical Knowledge remains singular while raw evidence remains traceable.
