---
id: DOMAIN-<domain>
type: DOMAIN
source_name: <evidence-backed source anchor>
business_name: <business domain name>
owner: ca-business
analysis_result: VERIFIED|PARTIAL|UNRESOLVED
---

# <Business Domain>


## Responsibility Question

<The one question this Domain primarily answers.>

Examples for a financial system:

```text
Funds Account
→ Who owns the money, where is it, how much exists, can it be used?

Funds Transaction
→ Why is money changing; what financial business transaction is occurring?

Funds Settlement
→ How is the established monetary obligation fulfilled from payer to payee?
```

## Business Responsibility

<The stable business responsibility owned by this Domain.>

## Authoritative Business Objects / Results

- ...

## Ownership Boundary

### Owns

- ...

### Does Not Own

- ...

A capability must not be assigned here merely because it mutates this Domain's data.


## Boundary

### Includes

- ...

### Excludes

- ...

## Subdomain Evaluation

Result:

```text
SUBDOMAINS_REQUIRED | DIRECT_CAPABILITIES | UNRESOLVED
```

Reason:

<Why this Domain does or does not need Subdomains.>

## Business Topology

When Subdomains are needed:

### `SUBDOMAIN-<domain>-<subdomain>` — <Subdomain Name>

Responsibility:

<Stable internal business responsibility.>

Capabilities:

| Capability ID | Business Name | Business Result |
|---|---|---|
| `CAPABILITY-...` | ... | ... |

When no Subdomain is needed, list Capabilities directly.

## Business Objects

- `BUSINESS-OBJECT-...`

## Cross-domain Collaboration

| Collaborating Domain | Why / Capability | Direction |
|---|---|---|
| `DOMAIN-...` | ... | calls / called-by / event |

## Unresolved

- ...

## Evidence

- ...
