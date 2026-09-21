# Semantic Identity Rules

## Purpose

These rules help Code Atlas answer one simple question:

> Are two candidates the same business knowledge, or different knowledge?

Keep the mechanism simple.

Use only four outcomes:

```text
MERGE
NEW
UNRESOLVED
REJECT
```

Meaning:

- `MERGE` — same semantic identity; keep one canonical entity and preserve all evidence/aliases.
- `NEW` — materially different semantic identity; create a separate canonical entity.
- `UNRESOLVED` — current evidence is insufficient; do not guess.
- `REJECT` — technical detail or weak candidate that should not become canonical business knowledge.

No numeric confidence or score is used.

---

## 1. Common Rule Format

For each knowledge type, define only:

```yaml
identity:
  # what makes it a distinct semantic thing

ignore_differences:
  # differences that normally do not create a new identity

promotion:
  # what evidence is needed before publishing it
```

Do not introduce additional identity meta-models unless real use proves they are necessary.

---

## 2. Four Common Tests

### Same Meaning Test

Ask:

> After removing technical names and locations, do A and B mean the same thing in this system?

If yes, this supports `MERGE`.

Name similarity alone is not evidence.

### Independent Existence Test

Ask:

> Can A and B exist independently in the same real business instance?

Example:

```text
Withdraw Transaction TX001
Payment Order PAY001
```

If both independently exist, they should normally be `NEW` identities rather than merged.

### Independent Lifecycle Test

Ask:

> Can A and B independently be in different business states?

Example:

```text
Transaction = PROCESSING
PaymentOrder = FAILED
```

If yes, this strongly supports separate Business Objects.

### Independent Goal / Result Test

Ask:

> Does this behavior have its own business goal and recognizable business result?

Use this mainly to distinguish:

```text
Step
vs
Process
vs
Capability
```

A technical method call is not a Process merely because it is reused.

---

## 3. Global Rules

### Do not default to merge

```text
not proven different
!=
proven same
```

### Do not default to split

```text
not proven same
!=
proven different
```

If evidence is insufficient:

```text
UNRESOLVED
```

### Technical boundary is not business identity

Do not split business knowledge merely because of:

- Project;
- Module;
- package;
- Controller/Service/Mapper;
- RPC/MQ/Job;
- sync/async implementation;
- transaction/thread boundary.

### Merge is lossless

When candidates merge, preserve:

- source names;
- aliases;
- projects/modules where discovered;
- evidence;
- unresolved contradictions.

---

# Knowledge-type Rules

## 4. Business Domain

```yaml
identity:
  - stable_business_responsibility
  - business_boundary
  - coherent_capabilities

ignore_differences:
  - project
  - module
  - package
  - team
  - database

promotion:
  - stable_business_responsibility_is_proven
  - capability_cluster_is_supported_by_evidence
```

Anti-rule:

```text
Project != Domain
Module  != Domain
```

---

## 5. Business Subdomain — optional

Use only when the active model needs a meaningful intermediate business grouping.

```yaml
identity:
  - parent_domain
  - stable_internal_business_responsibility
  - coherent_capability_group

ignore_differences:
  - project
  - module
  - package
  - team

promotion:
  - multiple_related_capabilities_exist
  - grouping_has_stable_business_meaning
```

Do not create a Subdomain merely to mirror technical structure.

---

## 6. Capability

```yaml
identity:
  - stable_business_function
  - business_result

ignore_differences:
  - channel
  - actor
  - scenario
  - technical_entry
  - project
  - module

promotion:
  - function_is_stable_and_business_meaningful
  - result_is_evidence_backed
```

Useful question:

> Remove channel/role/path qualifiers. Is the remaining phrase still a complete stable business function?

Example:

```text
bank-card withdrawal
flexible-work withdrawal
```

may share:

```text
Capability = withdrawal
```

while remaining different Scenarios.

---

## 7. Business Object

```yaml
identity:
  - stable_business_identity
  - independent_business_meaning
  - independent_reference_or_behavior

ignore_differences:
  - class_name
  - interface_name
  - dto_do_vo
  - table_split
  - rpc_model
  - project
  - module
  - role_in_current_process

promotion:
  - object_is_a_real_business_concept
  - identity_or_independent_reference_is_evidence_backed
```

Use these tests most often:

- Same Meaning Test;
- Independent Existence Test;
- Independent Lifecycle Test.

Examples:

```text
EmpAccount
Wallet
emp_account
```

may be different representations of one Business Object.

But:

```text
WithdrawTransaction
PaymentOrder
```

should remain separate when they independently exist, have separate IDs/states, or are independently queried/retried.

Do not use:

```text
one table = one Business Object
one class = one Business Object
```

---

## 8. Lifecycle

```yaml
identity:
  - business_object
  - business_state_dimension

ignore_differences:
  - field_name
  - enum_name
  - table_column
  - java_constant

promotion:
  - state_dimension_is_business_meaningful
  - real_state_values_are_proven
  - transitions_are_only_published_when_evidence_exists
```

Important:

> Similar state values do not mean the same Lifecycle.

`transaction_status`, `payment_status`, and `audit_status` may be different lifecycle dimensions even if all contain `INIT/SUCCESS/FAILED`.

---

## 9. Scenario

```yaml
identity:
  - capability
  - canonical_business_process_flow

ignore_differences:
  - channel
  - actor
  - amount_band
  - project
  - module
  - http_rpc_mq_job
  - sync_async
  - retry_callback_technical_mechanics

promotion:
  - stable_business_path_exists
  - process_flow_is_evidence_backed
```

Core rule:

> Selector discovers candidate paths; business Process Flow defines Scenario identity.

### De-dimension Test

Remove proven non-identity differences such as channel, role, amount band, Project and transport.

If the business Process Flow becomes the same:

```text
MERGE
```

If a stable material business-path difference remains:

```text
NEW
```

This rule prevents Scenario combinatorial explosion.

---

## 10. Process

```yaml
identity:
  - business_goal
  - business_boundary
  - business_result_or_closure

ignore_differences:
  - method
  - service
  - rpc
  - mq
  - job
  - project
  - module
  - transaction_boundary

promotion:
  - independent_business_goal_exists
  - recognizable_business_result_or_boundary_exists
```

Use the Independent Goal / Result Test.

Do not promote:

```text
checkWalletStatus()
Mapper.update(...)
callDubbo(...)
```

into Processes merely because they are reusable technical operations.

Cross-Scenario reuse supports Process identity but is not required.

---

## 11. Business Rule

Business Rule is normally embedded in Capability, Scenario, Process, Lifecycle or Business Object documentation.

Do not create a standalone Markdown entity by default.

```yaml
identity:
  - business_question_or_decision
  - business_consequence

ignore_differences:
  - code_location
  - if_switch_config_storage
  - rule_parameter_values

promotion:
  - rule_changes_business_eligibility_calculation_selection_or_constraint
  - rule_is_evidence_backed
```

Example:

```text
level 1 → 2000
level 2 → 1500
level 3 → 1000
```

can be one Rule:

```text
星级决定押款金额
```

Do not turn every `if` into a Business Rule.

---

## 12. Business Relationships

Keep relationships lightweight.

Do not require a large relationship-type taxonomy.

Prefer:

```yaml
source:
target:
business_relation:
evidence:
```

Example:

```text
业务员钱包
-- 发生交易 -->
业务员交易
```

or:

```text
业务员交易
-- 形成账务流水 -->
账务流水
```

A foreign key or object reference is evidence, not the business meaning itself.

Store one canonical direction; derive reverse navigation.

---

## 13. Execution Chain

```yaml
identity:
  - trigger
  - continuous_execution_context
  - termination

ignore_differences:
  - local_method_rename
  - class_move
  - line_number
  - project_or_module_crossing_during_same_sync_context

promotion:
  - real_trigger_is_proven
  - real_execution_path_is_traced
  - termination_or_unresolved_boundary_is_explicit
```

Split a Chain when a new independent execution context begins, such as:

- MQ consumer;
- callback;
- Job;
- retry worker;
- compensation worker;
- manual re-trigger.

Do not split merely because synchronous execution crosses Project/Module/RPC boundaries.

---

## 14. Common Chain

```yaml
identity:
  - same_shared_concrete_implementation

ignore_differences:
  - caller
  - scenario
  - local_call_site

promotion:
  - implementation_is_actually_shared
```

Similar code is not a Common Chain.

---

## 15. Final Rule

Keep semantic identity reasoning explainable in plain language.

A reviewer should be able to understand:

```text
Why were these merged?
Why were these kept separate?
Why is this still unresolved?
Why was this rejected?
```

without learning a second meta-model first.
