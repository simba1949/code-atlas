---
name: ca-scenario
description: >
  Enumerate and canonicalize evidence-backed business Scenarios for every confirmed
  Capability, then model Process, Sub-process and Step boundaries. Use canonical
  Business Process Flow to distinguish real business paths from channels, technical
  entries, workflow stages, failures or implementation details.
---

# ca-scenario

## Mission

Own:

```text
Capability
  ↓
Scenario
  ↓
Process
  ↓
Sub-process / Step
```

Every confirmed Capability must undergo Scenario Evaluation.

Do not silently skip:

```text
Capability → Process
```

## Required references

Use:

```text
../code-atlas/references/business-topology-convergence.md
../code-atlas/references/domain-responsibility-boundaries.md
../code-atlas/references/semantic-canonicalization-protocol.md
../code-atlas/references/semantic-identity-rules.md
```

## Inputs

Consume the converged Domain / Subdomain / Capability topology from `ca-business`, Business Object / Lifecycle, Fact Inventory from `ca-code-intel`, technical context from `ca-project`, and trace evidence/challenges from `ca-chain`.

## Step 1 — Scenario Enumeration for Every Capability

For each Capability ask:

> What stable business paths implement this Capability?

Search broadly for candidate selectors:

- account/object type;
- payment/settlement route;
- business mode;
- actor/role;
- channel;
- rule/config;
- lifecycle state;
- external business system;
- asynchronous continuation.

A selector is only a discovery clue.

It is not Scenario identity.

Required evaluation result:

```text
one stable path
→ publish one Scenario

multiple materially different stable paths
→ publish multiple Scenarios

insufficient evidence
→ explicit UNRESOLVED Scenario Evaluation
```

Do not leave a Capability with no Scenario analysis record.

## Scenario identity

Definition:

> 同一 Capability 下，具有稳定且实质不同 Canonical Business Process Flow 的真实业务路径。

Core rule:

> Selector discovers candidates; Process Flow decides Scenario identity.

Normalize away differences that do not materially change business flow, including HTTP/RPC/MQ/Job entry, Project/Module/package, Controller/Facade/Service, ordinary actor/channel labels, amount bands, parameter/rule values, and sync/async mechanics.

Use the De-dimension Test:

> Remove the differing labels. Does a materially different business Process Flow remain?

If no: `MERGE`.

If yes and evidence is sufficient: `NEW`.

Otherwise: `UNRESOLVED`.

## End-to-end path rule

Do not model each workflow stage as its own Capability/Scenario pair when the stages implement one end-to-end business goal.

Prefer:

```text
Capability: COD 货款打款

Scenario: 支付中心出款
账单接收
→ 审核
→ 防重受理
→ 支付中心出款
→ 结果回写
→ 触发网点提现

Scenario: TFBS 三方出款
账单接收
→ 审核
→ 防重受理
→ TFBS 出款
→ 结果回写
→ 触发网点提现
```

over separate Capabilities named:

```text
账单接收 / 审核 / 打款 / 提现触发
```

when evidence shows they are stages of one goal.

## Exception / retry rule

Success/failure/retry/compensation does not automatically create a Scenario.

Promote an exception path as a Scenario only when it is stable and repeatable, materially different in business responsibility/process flow, independently meaningful to operators/business, and evidence-backed.

Otherwise keep it as Process branch or Retry/Compensation continuation.


## Cross-domain Scenario Ownership

One Scenario may collaborate with several Domains.

Do not move or split Scenario ownership merely because one Process calls another Domain.

Ownership stays with the Capability that owns the Scenario's end-to-end business goal.

Example:

```text
Funds Transaction Domain
Capability: Withdrawal
Scenario: Bank-card Withdrawal

1. Validate withdrawal transaction
2. Create transaction
3. [Funds Account Domain] Freeze funds
4. [Funds Settlement Domain] Execute payout
5. Confirm transaction
6. [Funds Account Domain] Finalize / release funds
```

The Scenario remains:

```text
Funds Transaction Domain → Withdrawal
```

Account and Settlement are collaborating Domains.

Represent these as cross-domain references in Process/Scenario documentation.

Do not create:

```text
Account Withdrawal Scenario
Settlement Withdrawal Scenario
```

solely because implementation crosses those Domains.

## Scenario Process Flow

Scenario owns ordered/conditional Process orchestration.

For every edge retain evidence proving the order or condition.

Deduplicate accidental repeated references, but do not remove intentional repeated execution. If the same Process legitimately occurs twice, represent two flow occurrences/steps rather than silently duplicating an ID.

## Process definition

> Scenario 中具有相对独立业务目标、明确业务边界和业务结果的业务流程单元。

A Process is not a Controller, Service, Method, RPC, MQ, Job, transaction boundary or Project.

Use:

```text
independent business goal
+
business responsibility boundary
+
recognizable result/closure
```

to decide Process identity.

Process may cross Projects/modules synchronously.

## Sub-process and Step

Keep hierarchy shallow.

Use Sub-process only when a Process contains a stable business sub-flow that materially helps understanding.

Use Step for business actions, not raw technical calls.

## Business Topology handoff

After Scenario Enumeration, update one complete analysis-only topology:

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

Challenge globally before technical tracing:

- every Capability evaluated;
- Scenario names represent paths, not stages;
- no technical entry created a Scenario by itself;
- Processes are not duplicated as Capabilities;
- Process Flow is end-to-end enough to explain the business outcome.

Then hand Processes to `ca-chain`.
