# ADR-011: Scenario Canonicalization Prevents Combinatorial Explosion

## Status

Accepted

## Context

Real systems expose many decision dimensions:

```text
channel
actor / role
account type
settlement method
amount band
policy result
entry mechanism
sync / async implementation
success / failure
retry / callback / compensation
```

If each unique combination becomes a Scenario, Scenario count grows as a Cartesian product.

Example:

```text
PC × star-user × large-amount × bank-card × success
RPC × normal-user × small-amount × bank-card × success
...
```

This produces repetitive documentation and makes Scenario identity unstable.

It also confuses:

```text
condition difference
technical difference
```

with:

```text
business-path difference
```

## Decision

Use this principle:

> Selector discovers candidate paths; Canonical Business Process Flow defines Scenario identity.

Selectors create `Scenario Candidate` paths only.

Before promotion:

1. trace candidate paths;
2. build a Scenario Candidate Signature;
3. normalize technical and parameter dimensions that do not determine business identity;
4. compare canonical Business Process Flow;
5. merge business-equivalent candidates;
6. apply the Scenario Promotion Gate only to remaining materially distinct candidates.

Add a mandatory `Canonicalization Gate` to Scenario promotion.

### Scenario Candidate Signature

Internal analysis-only structure:

```yaml
capability:
selectors:
applicable_conditions:
canonical_process_flow:
business_objects:
lifecycle_impact:
business_result:
business_responsibility_handoffs:
business_route:
evidence:
```

It is not a published Knowledge Entity.

### De-dimension Test

Before splitting two Scenarios, remove differing labels such as:

- channel;
- role;
- amount band;
- object subtype;
- entry mechanism;
- policy result.

Then ask:

> Do the candidates still have materially different canonical Business Process Flows?

If no, merge them.

Record the differing values as:

- Path Selection;
- Applicable Conditions;
- Business Rules;
- Branches.

If yes, continue Scenario promotion.

### What Does Not Create Scenario Identity by Itself

- HTTP / RPC / MQ / Job;
- Project / Module;
- sync vs async;
- different Execution Chain;
- ordinary channel differences;
- ordinary role differences;
- amount thresholds;
- fee/rate/deposit calculations;
- retry/callback/compensation;
- success/failure/timeout;
- one additional validation;
- one technical method/database call.

### When a Difference May Create a Scenario

A difference becomes Scenario-relevant when it causes stable, materially different business orchestration, such as:

- different Process composition/order;
- different business responsibility handoff;
- materially different Business Object interaction;
- materially different Lifecycle impact;
- different business settlement/handling route;
- different external business participant;
- different stable business result/closure.

## Example

Candidate paths:

```text
PC + normal + BANK
PC + star + BANK
RPC + normal + BANK
RPC + star + BANK
```

all canonicalize to:

```text
validate withdrawal
→ create withdrawal order
→ freeze balance
→ bank payment
→ confirm result
```

Therefore publish one:

```text
Scenario: Bank-card withdrawal
```

and keep PC/RPC and normal/star as applicable conditions.

A flexible-employment path may remain distinct if it introduces:

```text
contract check
→ flexible-employment order
→ platform settlement
→ platform callback
```

because the canonical Process Flow materially differs.

## Rejected Alternatives

### One Selector Value = One Scenario

Rejected because selectors are discovery signals, not business identity.

### One Condition Combination = One Scenario

Rejected because it creates Cartesian-product explosion.

### Different Execution Chain = Different Scenario

Rejected because technical execution and business identity are separate dimensions.

### Different Rule Result = Different Scenario

Rejected unless the rule causes material, stable business-path divergence.

## Consequences

- Scenario count remains stable and human-readable.
- Scenario names describe business paths rather than parameter combinations.
- Business Rules and Applicable Conditions absorb non-identity differences.
- Execution Chain variation remains technical unless it changes business Process orchestration.
- Scenario split requires evidence; merge is preferred when business flows are equivalent.
