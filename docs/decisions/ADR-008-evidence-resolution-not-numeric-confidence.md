# ADR-008: Evidence Resolution Instead of Numeric Confidence

## Status

Accepted

## Context

Numeric confidence such as `82%` appears precise but usually has no calibrated statistical meaning in repository analysis.

It can hide whether evidence is actually missing, contradictory, or merely incomplete.

## Decision

Use categorical evidence/result states such as:

```text
VERIFIED
PARTIAL
UNRESOLVED
```

At the Fact/source-resolution layer, distinguish when useful:

```text
RESOLVED
PARTIAL
UNRESOLVED
NOT_FOUND
```

Where:

```text
NOT_FOUND
= current visible scope found no corresponding implementation

UNRESOLVED
= continuation/existence is known or suspected from evidence,
  but the target cannot currently be resolved
```

Evidence should remain attached to the fact/entity/relation it proves.

## Rejected Alternatives

### Numeric Confidence Score

Rejected because it creates false precision without calibrated semantics.

### Treat Missing Evidence as Negative Evidence

Rejected because absence in current scope may reflect unavailable source or external implementation.

## Consequences

Users and agents can see why knowledge is incomplete rather than relying on an opaque score.
