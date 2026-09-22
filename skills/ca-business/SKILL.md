---
name: ca-business
description: >
  Converge Code Atlas business topology from verified code evidence. Own Business
  Domains, optional Business Subdomains, Capabilities, Business Objects, Lifecycles
  and core-table semantics. Use full-scope Business Action discovery before
  Capability/Subdomain/Domain publication so technical projects and workflow stages
  are not mistaken for business boundaries.
---

# ca-business

## Mission

Own:

```text
Business Domain
  ↓
Business Subdomain [optional]
  ↓
Capability

Business Object
  ↓
Lifecycle
  ↓
State
  ↓
State Transition
```

Core discovery direction:

```text
Business Actions
→ Capability Convergence
→ Subdomain Grouping
→ Domain Convergence
```

Do not start by turning Projects/packages into Domains.

## Ownership

You own:

- Business Domain
- Business Subdomain
- Capability
- Business Object
- Lifecycle
- State
- State Transition
- core-table business semantics

You do not own:

- Scenario
- Process
- Sub-process
- Step
- Execution Chain
- technical project structure

## Required references

Use:

```text
../code-atlas/references/business-topology-convergence.md
../code-atlas/references/domain-responsibility-boundaries.md
../code-atlas/references/semantic-canonicalization-protocol.md
../code-atlas/references/semantic-identity-rules.md
```

Semantic outcomes remain:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

## Step 1 — Full-scope Business Action Inventory

Before final Domain/Capability publication, inspect the full selected scope and build an analysis-only inventory of business actions.

Cover:

- user/API/RPC operations;
- Jobs/MQ/Callback business effects;
- Business Object behavior;
- state mutations;
- important table writes;
- rules/configuration affecting business behavior.

Example:

```text
开户
启用
停用
冻结
充值
提现
审核
打款
结算
红冲
对账
```

This inventory exists to prevent local-first modeling.

Do not publish it as a Knowledge Entity.

## Step 2 — Capability Convergence

Definition:

> 一个长期稳定、具有独立业务目标和业务结果的业务功能。

Require:

```text
stable business goal
+
business behavior
+
Business Object/data effect
+
identifiable business result
```

### Mandatory Process-vs-Capability challenge

Before promoting a Capability, ask:

> Is this only one stage inside a larger end-to-end business goal?

Strong Process smells:

```text
受理
审核
拆分
推送
回写
回调
结果同步
失败检测
```

These words do not automatically mean Process, but they require explicit challenge.

Example:

```text
账单接收
打款单审核
打款执行
提现触发
```

must be tested as possible Processes of one Capability such as:

```text
COD 货款打款
```

Do not preserve implementation stages as Capabilities by default.

### Technical-name smell

A business Capability should normally not contain:

- project name;
- module name;
- framework/service name.

Example:

```text
余额批量核心记账（finance-balance-api）
```

must be challenged and renamed/converged based on business meaning.

## Step 3 — Mandatory Subdomain Evaluation

For every Domain candidate, answer:

> Does it contain two or more stable internal business responsibility clusters?

If yes, create embedded stable-ID Subdomains:

```text
SUBDOMAIN-<domain>-<subdomain>
```

If no, Domain may directly contain Capabilities.

Do not infer Subdomain from Project, Module, package, database, team, or UI menu.

Subdomain publication is optional.

Subdomain Evaluation is not optional.

## Step 4 — Domain Convergence

Definition:

> 当前分析范围内最大的稳定业务责任边界之一。

Review all candidate Domains together.

Challenge any Domain that is merely a Project, Business Object, one approval workflow, one execution stage, one technical subsystem, or a narrow source package.

Merge candidate Domains when their Capabilities form one coherent long-lived responsibility and use Subdomains to preserve meaningful internal boundaries.

There is no target Domain count.

Domain proliferation is a review signal, not proof.


## Step 4.1 — Responsibility Ownership Test

Do not assign a Capability to a Domain because that Capability modifies the Domain's data.

Classify ownership by:

```text
business goal
+
authoritative Business Object/result
+
lifecycle/result owned by the behavior
```

Use three questions:

```text
State
→ What current business state is owned?

Intent
→ What business transaction/request explains the change?

Fulfillment
→ What obligation must actually be completed?
```

### Financial-system diagnostic

For financial systems, test these candidate responsibilities separately:

```text
Funds Account Domain
→ 钱属于谁、在哪里、有多少、当前能不能用

Funds Transaction Domain
→ 为什么这笔钱变化、当前是什么资金业务

Funds Settlement Domain
→ 已成立的资金义务如何从付款方履约到收款方
```

Short form:

```text
账户管状态
交易管意图
结算管履约
```

Typical classification:

```text
open / close account           → Funds Account
enable / disable account       → Funds Account
freeze / unfreeze balance      → Funds Account
atomic debit / credit          → Funds Account

withdraw / recharge            → Funds Transaction
consume / refund / transfer    → Funds Transaction
business credit transaction    → Funds Transaction

settlement route               → Funds Settlement
clearing                       → Funds Settlement
payout fulfillment             → Funds Settlement
settlement reversal/compensate → Funds Settlement
```

Do not treat this table as hard-coded truth.

Source evidence may justify another model.

### Mandatory ambiguity checks

`入账` must be disambiguated:

```text
业务入账
→ transaction with independent business identity/lifecycle
→ Funds Transaction

账户记账 / 贷记
→ atomic balance posting
→ Funds Account
```

`风控` must be disambiguated:

```text
account availability / freeze / permission control
→ Funds Account

independent cross-product risk decisioning
→ possible independent Risk Domain
```

### Subject-owner rule

```text
employee account
site account
merchant account
```

do not automatically become three Domains.

Evaluate whether they are:

- Subdomains;
- Scenario/context dimensions;
- or one unified Account model with owner type.

The same rule applies to employee/site/merchant transactions.

## Business Object

> Business Object != Data Table.

Promotion requires:

1. stable business identity;
2. meaningful business data/state;
3. meaningful behavior that reads or mutates it.

Do not use one class/table as one Business Object by default.

A Business Object may map to multiple tables.

Persistence roles may include:

```text
PRIMARY
DETAIL
RELATION
STATE_HISTORY
BUSINESS_FLOW
AUDIT_LOG
SNAPSHOT
CONFIG
TECHNICAL
```

Store canonical direction:

```text
Business Object → Table
```

## Lifecycle

Lifecycle belongs to a Business Object.

Promotion requires:

1. confirmed Business Object;
2. confirmed state dimension;
3. confirmed state values;
4. transition write/control-flow evidence for every published arrow.

Never infer transitions from enum ordering.

Separate independent state fields into separate Lifecycle dimensions unless code proves otherwise.

## Evidence discipline

Use Fact Inventory, source behavior, data reads/writes, state reads/writes, configuration, validated chain evidence, and tests only as auxiliary evidence.

No Evidence → No Semantic Change.

## Output requirement

Before handing topology to `ca-scenario`, produce one analysis-only topology skeleton:

```text
Domain
├── Subdomain [optional]
│   ├── Capability
│   └── Capability
└── Capability
```

Every Capability must have:

- business goal;
- business result;
- supporting evidence;
- Domain;
- optional Subdomain.

Do not generate final Domain Markdown until full-scope convergence is complete.

## IDs

Use:

```text
DOMAIN-<domain>
SUBDOMAIN-<domain>-<subdomain>
CAPABILITY-<domain>-<capability>
BUSINESS-OBJECT-<object>
LIFECYCLE-<object>-<dimension>
STATE-<object>-<dimension>-<state>
TRANSITION-<object>-<dimension>-<from>-TO-<to>
```

Do not encode Project/Module into business identity by default.

## Handoff

Send confirmed Domains, Subdomains, Capabilities, Business Objects and Lifecycles to `ca-scenario`.

Every confirmed Capability must then undergo Scenario Evaluation before publication.
