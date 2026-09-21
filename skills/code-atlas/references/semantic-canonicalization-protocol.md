# Semantic Canonicalization Protocol

## Purpose

Semantic Canonicalization prevents technical representations from becoming duplicate or misleading business knowledge.

Use it together with:

```text
semantic-identity-rules.md
```

Core rule:

> Source facts discover candidates; business meaning decides canonical identity.

Keep the flow simple:

```text
Fact
→ Candidate
→ Canonicalize
→ Verify
→ Publish
```

---

## 1. Fact

Start from visible current evidence:

- source code;
- configuration;
- SQL / schema / table usage;
- RPC / MQ / Job / callback behavior;
- state reads/writes;
- tests as auxiliary evidence only.

Facts are not business conclusions.

`ca-code-intel` owns Fact discovery.

---

## 2. Candidate

Owner Skills may propose business knowledge from facts.

Examples:

- Domain;
- Capability;
- Business Object;
- Lifecycle;
- Scenario;
- Process;
- Business Rule.

A Candidate is provisional.

Do not publish it directly into `docs/code-atlas/`.

---

## 3. Canonicalize

Compare the Candidate with already discovered knowledge using:

```text
semantic-identity-rules.md
```

Only four outcomes exist:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

### MERGE

Same semantic identity.

Keep one canonical entity and preserve all:

- aliases/source names;
- evidence;
- project/module discovery locations;
- relevant context.

### NEW

A materially different semantic identity has been proven.

Create a separate canonical entity.

### UNRESOLVED

Evidence is insufficient to safely merge or separate.

Keep the question explicit and continue tracing when useful.

### REJECT

The candidate is only a technical detail, weak artifact, or otherwise does not deserve canonical business knowledge.

---

## 4. Verify

Canonicalization answers:

> Is this the same knowledge or different knowledge?

Verification answers:

> Is there enough evidence to publish it?

The canonical Owner performs verification.

Rules:

- No Evidence → No Semantic Change.
- Technical boundaries do not create business boundaries.
- Name similarity does not prove identity.
- Lack of difference does not prove sameness.
- Lack of sameness does not prove difference.
- Do not use numeric confidence.

If new evidence changes a previous conclusion, re-evaluate it.

No special Challenge state machine is required.

---

## 5. Publish

Only verified canonical knowledge may enter final Markdown.

Requirements:

- canonical ID is unique;
- exactly one Owner;
- references point to canonical IDs;
- merged aliases/evidence are preserved;
- unresolved knowledge is clearly marked and never presented as verified;
- reverse relations are derived rather than manually duplicated.

Markdown remains the persistent Canonical Knowledge.

HTML remains derived presentation.

---

## Cross-Skill Responsibilities

### `ca-code-intel`

Discover facts.

Do not decide business identity.

### Owner Skills

Examples:

- `ca-project`
- `ca-business`
- `ca-scenario`
- `ca-chain`

They:

- propose Candidates;
- apply Identity Rules;
- decide `MERGE / NEW / UNRESOLVED / REJECT` for owned knowledge;
- verify promotion.

### `ca-knowledge`

Keep consistency simple:

- canonical ID uniqueness;
- alias/source-name → canonical ID mapping;
- owner/reference validation;
- reverse-relation derivation;
- unresolved-reference checks.

It is not a central business-semantic judge.

### `ca-audit`

Verify that:

- semantic identity is evidence-backed;
- technical/context differences did not create duplicate knowledge;
- materially independent concepts were not incorrectly merged;
- unresolved cases were not guessed;
- Owner boundaries were respected.

### `ca-visual`

Render canonical Markdown only.

Never create or repair business semantics.

---

## Extension Rule

Before adding a new canonical semantic concept, define only:

```text
Owner
Identity Rule
Promotion Rule
Canonical relations
ID rule
Audit rule
```

Do not add additional meta-model layers unless real analysis repeatedly requires them.

---

## Final Principle

> Canonicalization should make the Atlas easier to understand, not create another system that must be understood first.
