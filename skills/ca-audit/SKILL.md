---
name: ca-audit
description: >
  Audit a Code Atlas analysis before publication. Validate evidence, ownership,
  entity promotion, relationship direction, Scenario/Process boundaries, Lifecycles,
  Execution Chains, unresolved boundaries, and Markdown consistency. Use as a strict
  quality gate; never invent or directly repair business facts.
---

# ca-audit

## Mission

Be the publication quality gate.

> Audit facts; do not create facts.

> Return problems to the owning Skill.

## Never do

Do not:

- invent missing Evidence
- infer a missing business path
- create a Transition to make a diagram complete
- rewrite another owner's semantic model directly
- hide unresolved boundaries
- assign numerical confidence scores

## Audit dimensions

### 1. Evidence

Check:

- every confirmed entity identity has evidence
- critical business claims have evidence
- every Scenario Process Flow edge has evidence
- every State Transition has write/control-flow evidence
- every critical Chain edge has evidence
- final evidence uses durable source/config/database anchors

### 2. Ownership

Check that:

- `ca-project` owns technical structure
- `ca-business` owns Domain/Capability/Object/Lifecycle
- `ca-scenario` owns Scenario/Process/Sub-process/Step
- `ca-chain` owns Chains
- no skill overwrote another owner's knowledge

### 3. Promotion

Reject entities whose existence is supported only by:

- names
- comments
- one DTO/VO
- one ambiguous table
- enum alone
- test alone
- industry intuition

Unknown parts inside a confirmed entity may remain explicit.

An unconfirmed entity itself must not be published.

### 4. Scenario

Verify:

- same Capability
- real path selector
- material business divergence
- stable/repeatable path
- evidence-backed Process Flow
- not merely a technical entry/stage/failure branch

### 5. Process

Verify:

- clear Business Goal
- Start/End boundaries
- independent Business Result or justified boundary
- Business Objects
- lifecycle impact
- at least one implementation Chain
- separation is not based only on method/service/project/MQ/RPC boundaries

### 6. Sub-process / Step

Verify:

- Sub-process is a local flow, not an independent reusable Process
- no recursive Sub-process tree by default
- Step is a key business action
- technical operations are kept in Chains

### 7. Lifecycle

Verify:

- Business Object is confirmed
- state dimension is explicit
- states are evidence-backed
- transitions are actual transitions, not arrows inferred from enum ordering
- multiple state fields are not merged without proof
- Process only references canonical transitions

### 8. Execution Chain

Verify:

- trigger exists
- path is code-backed
- sync RPC/project crossing is not arbitrarily cut
- MQ/callback/job/retry/compensation boundaries are handled
- external boundary is explicit
- termination is explicit
- unresolved provider/consumer is not invented
- Common Chain represents real shared implementation

### 9. Relationship direction

Ensure only canonical directions are authored.

Flag manually duplicated reverse indexes such as:

- Process → Execution Chains
- Lifecycle → Processes
- Business Object → Processes
- Common Chain → used-by Chains

when those are meant to be derived.

### 10. Analysis Workspace / Output Scope

Verify:

- one analysis scope resolves to one Analysis Workspace Root;
- one multi-project analysis publishes one Canonical Atlas under `<analysis-workspace-root>/docs/code-atlas/`;
- Project boundaries are represented inside the Atlas rather than by separate per-Project Atlases;
- the selected Project set is explicit and does not automatically expand to every sibling repository;
- cross-Project synchronous execution is not split merely because repository/project changes;
- output paths do not escape the resolved Analysis Workspace Root.




### 11. Semantic Canonicalization

Audit against:

```text
../code-atlas/references/semantic-canonicalization-protocol.md
../code-atlas/references/semantic-identity-rules.md
```

Verify:

- aliases/technical names were not treated as separate business identities without evidence;
- independently existing business concepts were not incorrectly merged;
- evidence-insufficient cases remain `UNRESOLVED`;
- rejected technical details were not published as business knowledge;
- merged knowledge preserved aliases and evidence;
- the canonical Owner made the semantic decision;
- numeric confidence was not used.

Treat unsupported Merge/New decisions as `BLOCKING`.

### 12. Scenario Canonicalization

### 13. Scenario Canonicalization

Verify:

- selectors were used to discover candidates, not directly promoted to Scenario identity;
- equivalent candidate paths were merged before publication;
- role/channel/amount/entry/technical differences did not create Scenarios unless they caused material canonical Process Flow divergence;
- different Execution Chains did not directly cause Scenario split;
- rule differences alone did not create Scenario split;
- Scenario names do not encode unnecessary Cartesian-product dimensions;
- each published Scenario remains materially distinct after the De-dimension Test;
- the Scenario set represents canonical business paths rather than raw code branches.

Treat obvious Scenario explosion as `BLOCKING`.

### 13. Tests

Verify tests are only auxiliary evidence and are absent from production entry/chain topology by default.

## Analysis result

Use:

```text
VERIFIED
PARTIAL
UNRESOLVED
```

Interpretation:

- VERIFIED: verified within current visible-code scope.
- PARTIAL: confirmed model with meaningful known gaps.
- UNRESOLVED: continuation/entity relation is known to exist but cannot be resolved.

Do not claim VERIFIED means omniscient system truth.

## NOT_FOUND vs UNRESOLVED

- `NOT_FOUND`: analysis found no corresponding implementation in current scope.
- `UNRESOLVED`: evidence shows something exists/continues, but target cannot be resolved.

Do not conflate them.

## Evidence coverage

Prefer qualitative coverage marks:

```text
✓ verified
? unresolved/partial
- not applicable/not found
```

Do not use arbitrary numeric confidence percentages.

## Audit finding format

Return issues like:

```yaml
finding:
  severity: BLOCKING | WARNING
  owner: ca-business | ca-scenario | ca-chain | ca-project | ca-knowledge
  entity:
  rule:
  observation:
  required_action:
  evidence:
```

`BLOCKING` prevents publication.

`WARNING` may coexist with publication when uncertainty is explicit and the confirmed entity remains valid.

## Publication gate

Do not approve final publication until:

- blocking unsupported entities removed/fixed
- broken references fixed
- ownership conflicts resolved
- duplicate canonical facts removed
- Scenario Process Flow supported
- Lifecycle transitions supported
- Chain terminations explicit
- unresolved boundaries visible
- candidate knowledge excluded

Return a final summary:

```text
AUDIT_PASSED
```

or:

```text
AUDIT_FAILED
```

with owner-routed findings.
