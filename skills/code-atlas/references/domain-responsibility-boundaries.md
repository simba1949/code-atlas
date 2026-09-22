# Domain Responsibility Boundaries

## Purpose

A business operation must belong to the Domain that owns its **business responsibility**,
not simply to the Domain whose data it mutates.

This rule prevents a common modeling error:

```text
"withdrawal changes wallet balance"
therefore
"withdrawal belongs to wallet/account domain"
```

That conclusion is not valid by itself.

Use the following responsibility test.

---

## 1. State / Intent / Fulfillment Test

When several Domains participate in one end-to-end flow, separate three questions:

```text
State
→ What business state/position exists now?

Intent
→ Why should that state change? What business transaction is occurring?

Fulfillment
→ How is the resulting obligation actually completed?
```

These are often different business responsibilities even when they operate on the same money,
order, inventory, entitlement, or other resource.

### Ownership rule

The owning Domain is determined by:

```text
business goal
+
authoritative Business Object
+
lifecycle/result owned by the behavior
```

not by:

```text
which table was updated
which service was called
which balance changed
which Project contains the method
```

---

# 2. Canonical Finance Example

For financial-account systems, the following distinction is a strong default heuristic.

It is not a hard-coded answer. Source evidence may justify a different boundary.

## Funds Account Domain

Responsibility question:

> 钱属于谁、在哪里、有多少、当前能不能用？

Typical ownership:

- account/wallet identity;
- open / close;
- enable / disable;
- account-level permissions;
- available balance;
- frozen balance;
- deposit/security amount;
- limit;
- freeze / unfreeze;
- atomic debit / credit / adjustment;
- account-level risk restriction.

Typical authoritative Business Objects:

```text
Account / Wallet
Balance / Position
Balance Reservation / Freeze
Account Status / Permission
```

The Funds Account Domain owns the **state of funds held in the account**.

It does not automatically own every business action that changes that state.

---

## Funds Transaction Domain

Responsibility question:

> 为什么这笔钱发生变化？当前正在执行什么资金业务？

Typical business capabilities:

```text
业务入账
充值
提现
消费
退款
转账
```

Typical authoritative Business Object:

```text
Funds Transaction
```

Typical transaction dimensions:

```text
transaction_type
transaction_status
business_reference
transaction_no
amount
```

The Funds Transaction Domain owns:

- the user's/business's financial intent;
- transaction lifecycle;
- transaction rules;
- transaction result.

Examples:

```text
提现 ≠ 扣减余额
退款 ≠ 增加余额
消费 ≠ 扣减余额
充值 ≠ 增加余额
```

The transaction may call the account domain to change balances.

That call does not move transaction ownership into the account domain.

---

## Funds Settlement Domain

Responsibility question:

> 已成立的资金义务，最终如何从付款方履约到收款方？

Typical ownership:

- settlement acceptance;
- payer/payee determination;
- settlement routing;
- clearing;
- payout/payment execution;
- settlement result confirmation;
- settlement reversal;
- settlement-level compensation.

Typical authoritative Business Objects may include:

```text
Settlement Instruction
Settlement Order
Clearing Task
Payout Instruction
```

The Funds Settlement Domain owns **fulfillment of a monetary obligation**.

It is normally downstream of an accepted Funds Transaction, although exact orchestration must be
proven from source evidence.

---

# 3. One Withdrawal Example

```text
Funds Transaction Domain
  Withdrawal Transaction TX001
      │
      ├── calls Funds Account Domain
      │      freeze 100
      │
      └── calls Funds Settlement Domain
             fulfill 100 payout
```

Successful completion may look like:

```text
Settlement
→ payout succeeded

Transaction
→ TX001 PROCESSING → SUCCESS

Account
→ frozen amount finalized / released according to the proven accounting model
```

This does not mean one Domain owns the whole implementation.

It means one business flow collaborates across Domains.

---

# 4. Account Action vs Business Transaction

This distinction is mandatory.

## Account action

Examples:

```text
credit(account, 100)
debit(account, 100)
freeze(account, 100)
unfreeze(account, 100)
adjust(account, delta)
```

Question answered:

> How did account state change?

Default ownership:

```text
Funds Account Domain
```

## Business transaction

Examples:

```text
withdraw 100
refund 100
consume 100
recharge 100
business credit 100
```

Question answered:

> Why did funds change?

Default ownership:

```text
Funds Transaction Domain
```

A transaction can invoke one or more Account actions.

Do not merge the two concepts because one eventually causes the other.

---

# 5. "入账" Is Ambiguous — Disambiguate It

Never classify the word `入账` from its name alone.

## Business credit transaction

If "入账" has:

- business source;
- transaction number;
- type;
- lifecycle/status;
- idempotency;
- independent business result;

then it is usually:

```text
Funds Transaction Domain
→ Business Credit / 入账 Transaction
```

## Account posting / credit

If "入账" only means:

```text
account balance + amount
```

with no independent business transaction identity, it is usually:

```text
Funds Account Domain
→ Account Posting / Credit
```

Use explicit terms in generated documents whenever possible:

```text
业务入账
账户记账 / 账户贷记
```

Do not use one ambiguous word for both.

---

# 6. Risk Control Is Also Ambiguous

Do not create or assign a `Risk Domain` based on the word "risk" alone.

## Account-level control

Examples:

- disable account;
- disable withdrawal;
- freeze balance;
- account risk flag;
- limit account operations.

If these are primarily account availability/permission controls, they belong to:

```text
Funds Account Domain
```

## Independent risk-decision responsibility

If code proves:

- independent risk case/object;
- independent policy/rule lifecycle;
- cross-product decision service;
- independently auditable risk result;

then a separate Risk Domain/Subdomain may be justified.

In that case:

```text
Risk Domain
→ produces decision

Funds Account / Transaction Domain
→ consumes decision
```

Do not collapse independent risk decisioning into account management.

---

# 7. Subject Type Does Not Automatically Define Domain

Examples:

```text
employee account
site account
merchant account
```

may be:

```text
Funds Account Domain
├── Employee Account Subdomain
├── Site Account Subdomain
└── Merchant Account Subdomain
```

but only if they have stable model/responsibility differences.

If all are one unified model:

```text
Account
owner_type = EMPLOYEE / SITE / MERCHANT
```

with substantially shared lifecycle, rules and capabilities, they may remain one Subdomain with
owner type as context.

The same applies in Funds Transaction Domain:

```text
employee withdrawal
site withdrawal
merchant withdrawal
```

must not become separate Domains merely because the account owner differs.

Use Scenario/Subdomain only when stable business-flow/responsibility differences are proven.

---

# 8. Cross-Domain Collaboration Does Not Transfer Ownership

A Scenario belongs to the Domain/Capability that owns its business goal.

Calls to other Domains are references/collaborations.

Example:

```text
Funds Transaction Domain
Capability: Withdrawal
Scenario: Bank-card Withdrawal

Process:
1. Validate withdrawal
2. Create Withdrawal Transaction
3. [Account Domain] Freeze funds
4. [Settlement Domain] Execute payout
5. Confirm Withdrawal Transaction
6. [Account Domain] Finalize/release funds
```

The Scenario remains:

```text
Funds Transaction Domain → Withdrawal
```

The account and settlement operations are cross-domain collaborations.

Do not split one end-to-end Scenario merely because it crosses Domain, Project, RPC, MQ, or JVM boundaries.

---

# 9. Domain Responsibility Decision Table

Use this question-first classification:

| Question | Typical Domain |
|---|---|
| Who owns this account/wallet? | Funds Account |
| Does the account exist? | Funds Account |
| Is the account enabled/disabled? | Funds Account |
| Is balance available/frozen? | Funds Account |
| Perform atomic debit/credit/freeze/unfreeze? | Funds Account |
| Why is money changing? | Funds Transaction |
| Is this withdraw/refund/recharge/consume/transfer? | Funds Transaction |
| What is the transaction lifecycle/result? | Funds Transaction |
| Who must ultimately pay whom? | Funds Settlement |
| What route/clearing path fulfills the obligation? | Funds Settlement |
| Did payout/settlement complete? | Funds Settlement |
| How is settlement reversed/compensated? | Funds Settlement |

This table is a heuristic.

Actual ownership still requires source evidence.

---

# 10. Generalization Beyond Finance

The same pattern can appear elsewhere:

```text
State Domain
→ owns current resource/account/inventory state

Intent/Transaction Domain
→ owns business reason/order/request that changes state

Fulfillment Domain
→ owns execution of the resulting obligation
```

Examples include:

- inventory / order / fulfillment;
- entitlement / subscription order / provisioning;
- account / transfer instruction / payment settlement.

Do not force this three-domain pattern where the source model does not support it.

Use it as a boundary diagnostic.

---

## Final Rule

> State ownership, transaction intent, and obligation fulfillment are different responsibilities until evidence proves they are one.

For financial systems:

> 账户管状态，交易管意图，结算管履约。
