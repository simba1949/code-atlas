# Knowledge Model Reference

## Canonical Model

```text
Entity + Relation + Evidence
```

Business axis:

```text
Business Domain → Capability → Scenario → Process → Sub-process / Step
```

Technical implementation:

```text
Process / Activity ← Execution Chain
```

Object axis:

```text
Business Object → Lifecycle → State / Transition
```

Canonical relationship ownership:

| Relation | Owner |
|---|---|
| Domain → Capability | Domain |
| Scenario → Capability | Scenario |
| Scenario → Process Flow | Scenario |
| Process → Sub-process / Step | Process |
| Process → Business Object | Process |
| Process / Activity → Transition | Process |
| Lifecycle → Business Object | Lifecycle |
| Lifecycle → State / Transition | Lifecycle |
| Business Object → Table | Business Object |
| Execution Chain → Process | Execution Chain |
| Execution Chain → Activity | Execution Chain |
| Execution Chain → Common Chain | Execution Chain |

Reverse relations are derived.



## Shared Semantic Rules

Business-semantic knowledge follows:

```text
Fact
→ Candidate
→ Canonicalize
→ Verify
→ Publish
```

Use:

```text
semantic-canonicalization-protocol.md
semantic-identity-rules.md
```

The protocol defines the flow.

The Identity Rules define, in plain language, when two candidates are the same or different.

Keep the knowledge model separate from internal analysis mechanics.
