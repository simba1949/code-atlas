# Naming Conventions

## IDs

Use complete English type names:

```text
SYSTEM-<system>
PROJECT-<project>
DOMAIN-<domain>
CAPABILITY-<domain>-<capability>
SCENARIO-<domain>-<capability>-<scenario>
PROCESS-<process>
SUB-PROCESS-<process>-<sub-process>
STEP-<process>-<step>
BUSINESS-OBJECT-<object>
LIFECYCLE-<object>-<dimension>
STATE-<object>-<dimension>-<state>
TRANSITION-<object>-<dimension>-<from>-TO-<to>
EXECUTION-CHAIN-<process>-<role>
COMMON-CHAIN-<chain>
```

Do not use ambiguous type abbreviations.

Business IDs do not include Project/Module by default.

## Files

Use semantic slug only:

```text
processes/site-withdraw.md
objects/withdraw-order.md
lifecycles/withdraw-order-main.md
```

Do not use:

```text
proc-site-withdraw.md
obj-withdraw-order.md
lc-withdraw-order-main.md
```
