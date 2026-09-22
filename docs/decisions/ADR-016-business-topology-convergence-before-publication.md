# ADR-016: Business Topology Converges Before Publication

## Status

Accepted

## Context

A real multi-project Code Atlas run exposed a structural modeling failure:

- too many narrow Domains;
- meaningful Subdomains were skipped;
- workflow stages were promoted as Capabilities;
- many Capabilities had no Scenario evaluation;
- technical Project names leaked into Capability names;
- HTML convenience edges flattened Domain / Capability / Scenario / Process into one graph.

The individual entity definitions were not sufficient to prevent this.

The missing control was global ordering: local code discoveries were being promoted before the whole business topology was compared.

## Decision

Introduce a mandatory Business Topology Convergence phase.

Discovery is bottom-up:

```text
Business Actions
→ Capability Convergence
→ Subdomain Evaluation
→ Domain Convergence
→ Scenario Enumeration
```

Publication is top-down:

```text
Domain
→ Subdomain [optional]
→ Capability
→ Scenario
→ Process
→ Execution Chain
```

### Business Action Inventory

Before Domain/Capability publication, analyze business actions across the full selected scope.

This is analysis-only and is not a new canonical entity.

### Capability challenge

Workflow stages such as:

```text
受理
审核
拆分
推送
回写
```

must be challenged as Process candidates.

They become Capabilities only when they have independent stable business goals/results.

### Subdomain

Business Subdomain is an embedded stable-ID entity owned by `ca-business`.

Subdomain publication remains optional.

Subdomain Evaluation is mandatory for every Domain.

### Scenario

Every confirmed Capability must undergo Scenario Evaluation.

A Capability may resolve to one or multiple Scenarios, or to an explicit unresolved Scenario analysis.

It may not silently bypass Scenario analysis.

### Topology freeze

Before final Markdown generation, the full business hierarchy must be globally challenged and stabilized.

### HTML

The default Business Map may not flatten the hierarchy with convenience edges such as:

```text
Domain → Scenario
Domain → Process
Capability → Process
```

Cross-level relations are allowed only for search, context, reverse navigation and impact analysis.

## Rejected Alternatives

### Add More Entity Types

Rejected. The problem is sequencing and convergence, not lack of concepts.

### Set a Fixed Domain Count

Rejected. Domain count depends on the business; count is only a review signal.

### Treat Project as Domain and Repair Later

Rejected because Project-first modeling contaminates Capability and Scenario boundaries downstream.

### Let HTML Reorganize Incorrect Canonical Knowledge

Rejected. HTML is derived presentation and must not become the semantic repair layer.

## Consequences

- Domain/Subdomain/Capability boundaries are decided globally rather than project-locally;
- workflow stages are less likely to be misclassified as Capabilities;
- Scenario coverage becomes explicit for every Capability;
- Subdomain omission becomes auditable;
- the HTML hierarchy remains readable;
- Code Atlas stays conceptually small: the change adds a mandatory convergence stage, not a new framework of meta-entities.
