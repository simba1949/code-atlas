# ADR-017: Domain Ownership Follows Business Responsibility, Not Shared Data Mutation

## Status

Accepted

## Context

A financial-domain modeling review exposed another systematic classification risk:

```text
withdrawal changes wallet balance
→ therefore withdrawal belongs to wallet/account domain
```

This mixes:

- current account state;
- business transaction intent;
- fulfillment of the resulting monetary obligation.

The same implementation may touch all three responsibilities.

Assigning ownership by the table/object being modified collapses distinct business models and causes
Capabilities and Scenarios to move to the wrong Domain.

## Decision

Domain ownership must be based on:

```text
business goal
+
authoritative Business Object/result
+
lifecycle/result owned by the behavior
```

not on shared data mutation.

Use the responsibility test:

```text
State
Intent
Fulfillment
```

### Financial-system diagnostic

Use this as a strong default heuristic:

```text
Funds Account Domain
→ owns account/wallet state

Funds Transaction Domain
→ owns why funds change and the financial transaction lifecycle

Funds Settlement Domain
→ owns fulfillment of the monetary obligation
```

Chinese short form:

```text
账户管状态
交易管意图
结算管履约
```

Typical examples:

```text
open / close / enable / disable account
freeze / unfreeze
atomic debit / credit
→ Funds Account

withdraw / recharge / consume / refund / transfer
business credit transaction
→ Funds Transaction

settlement route / clearing / payout fulfillment
settlement reversal / compensation
→ Funds Settlement
```

Source evidence may justify another boundary.

This is a diagnostic, not a hard-coded ontology.

### Ambiguous "入账"

Distinguish:

```text
业务入账
→ independent transaction identity/lifecycle
→ Transaction responsibility

账户记账 / 贷记
→ atomic balance posting
→ Account responsibility
```

### Risk

Distinguish:

```text
account availability/freeze/permission control
→ Account responsibility

independent cross-product risk decisioning
→ possible independent Risk responsibility
```

### Cross-domain Scenario

A Scenario remains under the Capability that owns its end-to-end business goal.

Calls to Account, Settlement, Risk or other Domains are collaboration references and do not transfer
Scenario ownership.

## Rejected Alternatives

### Assign Domain by Mutated Table/Object

Rejected because many business transactions necessarily mutate account/order/inventory state owned by
another Domain.

### Assign Domain by Service/Project Location

Rejected because technical placement does not establish business ownership.

### Merge Account, Transaction and Settlement into One Domain by Default

Rejected because they answer different business questions and may have independent objects/lifecycles.

### Always Force Three Financial Domains

Rejected. The source model may legitimately combine responsibilities; evidence remains authoritative.

## Consequences

- wallet/account state remains separate from financial transaction intent;
- withdrawal/refund/recharge/consume are less likely to be misclassified;
- settlement/clearing responsibility is clearer;
- cross-domain flows become explicit instead of forcing ownership changes;
- ambiguous words such as `入账` require semantic disambiguation;
- the rule generalizes to other State / Intent / Fulfillment business systems.
