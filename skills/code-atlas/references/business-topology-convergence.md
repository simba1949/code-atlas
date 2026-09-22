# Business Topology Convergence

## Purpose

Code Atlas must not publish business hierarchy directly from Project, package, table, API, or one local code path.

The business topology is discovered bottom-up and published top-down.

```text
Bottom-up discovery

Business Actions
→ Capability Candidates
→ Capability Convergence
→ Subdomain Grouping
→ Domain Convergence
→ Scenario Enumeration

Top-down publication

Domain
→ Subdomain [when meaningful]
→ Capability
→ Scenario
→ Process
→ Execution Chain
```

This is a modeling stage, not a new Knowledge Entity.

## 1. Business Action Inventory

Before final Domain/Capability publication, build an analysis-only inventory of business actions across the full selected scope.

Examples:

```text
开户
启用
停用
冻结
解冻
充值
提现
入账
消费
退款
审核
打款
红冲
对账
结算
```

Use evidence from entries, Business Object behavior, state writes, table writes, rules/config, and RPC/MQ/Job flows.

Do not publish the inventory itself.

Its purpose is coverage: prevent the model from declaring a Domain after reading only one Project/package.

## 2. Capability Convergence

Cluster Business Actions into stable business functions.

A Capability must have:

```text
stable business goal
+
independent business result
```

### Capability smell test

A candidate is probably a Process rather than a Capability when:

- it is only meaningful as one stage of a larger end-to-end goal;
- its name is primarily a workflow stage such as 受理 / 审核 / 拆分 / 推送 / 回写;
- it exists only because a Service/Job/API entry exists;
- it exposes an implementation project/module in the business name;
- several candidates naturally form one stable end-to-end business function.

Example:

```text
COD 账单接收
COD 打款单审核
COD 打款执行
COD 提现触发
```

should first be challenged as possible Processes under:

```text
Capability: COD 货款打款
```

Do not preserve all four as Capabilities merely because four code areas exist.

## 3. Subdomain Evaluation Is Mandatory

`Subdomain` is optional in the published hierarchy.

`Subdomain Evaluation` is mandatory for every Domain.

Question:

> Does this Domain contain two or more stable, coherent business responsibility clusters?

If yes:

```text
Domain
→ Subdomain
→ Capability
```

If no:

```text
Domain
→ Capability
```

A Subdomain is justified by a stable cluster of Capabilities sharing meaningful business responsibility, vocabulary, Business Objects and/or rules.

Do not create Subdomain from Project, Module, package, database, team, or one screen/menu.

Do not skip Subdomain simply because it is optional.

## 4. Domain Convergence

Domain is the largest stable business responsibility boundary in the current analysis scope.

Before publication, compare all candidate Domains globally.

Challenge a Domain when it is actually:

- one Business Object;
- one workflow;
- one approval stage;
- one technical subsystem;
- one Project;
- one isolated entry family.

Ask:

> If several candidate Domains were placed under one long-lived business responsibility, would their Capabilities, Business Objects, rules and vocabulary form a coherent whole?

If yes, converge them and use Subdomains where needed.

There is no target Domain count, but proliferation is a review signal.


## 4.1 Responsibility Ownership Test

Before finalizing Domain ownership, ask three independent questions:

```text
State
→ What current business state/position is owned?

Intent
→ What business transaction/request explains why the state changes?

Fulfillment
→ What obligation must be completed, and who owns its completion?
```

A Domain must not claim a Capability merely because its Business Object/table is mutated by the Capability.

Use:

```text
domain-responsibility-boundaries.md
```

### Mandatory anti-pattern

```text
"X changes Wallet"
therefore
"X belongs to Wallet Domain"
```

is invalid without additional semantic evidence.

For financial systems, explicitly challenge:

```text
Account / Wallet
vs
Funds Transaction
vs
Settlement
```

using:

```text
账户管状态
交易管意图
结算管履约
```

Treat shared data mutation as collaboration evidence, not ownership evidence.

## 5. Scenario Enumeration Is Mandatory

Every confirmed Capability must undergo Scenario Evaluation.

Ask:

```text
What stable business paths implement this Capability?
```

Inspect candidate selectors such as account/object type, payment/settlement route, channel, actor, rule/config, lifecycle state, external business system, and asynchronous continuation.

Selectors only discover candidate paths.

Scenario identity still comes from canonical Business Process Flow.

### Outcome

For each Capability:

```text
one stable path
→ one Scenario

multiple materially different stable paths
→ multiple Scenarios

insufficient evidence
→ explicit UNRESOLVED Scenario Evaluation
```

Do not silently use:

```text
Capability → Process
```

as a substitute for Scenario analysis.

## 6. End-to-End Scenario Rule

Do not turn every workflow stage into a Capability + Scenario pair.

Prefer:

```text
Capability
  ↓
Scenario
  ↓
Process A
  ↓
Process B
  ↓
Process C
```

over:

```text
Capability A: 受理
Capability B: 审核
Capability C: 打款
Capability D: 回写
```

when those stages exist mainly to implement one stable business goal.

Shared pre/post Processes may appear in multiple Scenarios when evidence proves reuse.

## 7. Business Topology Freeze

Before final Markdown generation, create one complete analysis-only topology:

```text
Domain
├── Subdomain [optional]
│   ├── Capability
│   │   ├── Scenario
│   │   │   ├── Process
│   │   │   └── Process
│   │   └── Scenario
│   └── Capability
└── Capability
```

Challenge it globally:

- Is a Domain really only a Project/object/workflow?
- Was Subdomain Evaluation performed?
- Is a Capability really a workflow stage?
- Does a Capability contain technical implementation words?
- Was Scenario Evaluation performed for every Capability?
- Are Scenarios split by business-flow divergence rather than channel/technical noise?
- Are Processes duplicated as Capabilities?
- Are cross-project business paths still continuous?

Only after this topology is stable may publication proceed.

## 8. Publication Hierarchy

The default business hierarchy is strictly:

```text
Domain
→ Subdomain [optional]
→ Capability
→ Scenario
→ Process
```

Cross-level relationships may exist for search, reverse navigation, impact analysis and evidence.

They must not flatten the default business topology.

In particular, the default topology must not add convenience edges such as:

```text
Domain → Scenario
Domain → Process
Capability → Process
```

when intermediate canonical levels exist.

## 9. Audit Blocking Conditions

Publication is `BLOCKING` when:

1. Domain identity primarily mirrors Project/Module/package without business-responsibility evidence.
2. A Domain with clearly distinct responsibility clusters skipped Subdomain Evaluation.
3. Capability set was not reviewed against a full-scope Business Action Inventory.
4. A workflow stage was promoted as Capability without an independent business goal/result.
5. A technical Project/module name leaks into Capability identity without semantic necessity.
6. Any Capability skipped Scenario Evaluation.
7. Scenario identity was not based on canonical Business Process Flow.
8. Process was duplicated as Capability merely because of implementation boundaries.
9. Default HTML business topology bypasses intermediate hierarchy levels.
10. Shared state mutation was used as the primary reason for Domain ownership without business-responsibility evidence.

## Final Principle

> Discover actions first. Converge capabilities second. Group responsibilities third. Enumerate scenarios fourth. Trace implementation last.

This keeps the model business-shaped instead of code-shaped.
