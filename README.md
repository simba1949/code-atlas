# Code Atlas

[English](./README.md) | [简体中文](./README.zh-CN.md)

Code Atlas is a multi-skill system for turning a complex codebase into a **navigable, evidence-backed, traceable** business and technical atlas.

It models:

```text
Business Domain
  ↓
Capability
  ↓
Scenario
  ↓
Process
  ↓
Sub-process / Step
  ↓
Execution Chain
```

and the Business Object lifecycle axis:

```text
Business Object
  ↓
Lifecycle
  ↓
State
  ↓
State Transition
       ↑
Process / Sub-process / Step
```

Core principles:

> Technical boundaries may split code, but they must not break a continuous business flow.

> Every published knowledge claim must be traceable to the current visible codebase.

## What Code Atlas Helps You Understand

Code Atlas helps answer:

- What projects and modules make up the system?
- What Business Domains and Capabilities does the system implement?
- What real Scenarios exist under a Capability?
- How does each Scenario orchestrate Processes?
- Which Sub-processes and Steps make up a Process?
- Which Business Objects participate?
- How do Business Object Lifecycles change?
- Which Execution Chains implement a Process or Step?
- How do DB, RPC, MQ, Job, and external systems participate?
- How does a business flow cross project/module boundaries?
- How can you drill down from business to code and navigate back from code to business?

## Skills

Code Atlas currently includes 9 skills:

```text
code-atlas
├── ca-code-intel
├── ca-project
├── ca-business
├── ca-scenario
├── ca-chain
├── ca-audit
├── ca-knowledge
└── ca-visual
```

### User-facing skills

| Skill | Responsibility |
|---|---|
| `code-atlas` | Full orchestration: discover → model → trace → challenge → converge → audit → publish |
| `ca-project` | System / Project / Module / technical map |
| `ca-business` | Business Domain / Capability / Business Object / Lifecycle |
| `ca-scenario` | Scenario / Process / Sub-process / Step |
| `ca-chain` | Execution Chain / Common Chain |

### Supporting skills

| Skill | Responsibility |
|---|---|
| `ca-code-intel` | Discover code-intelligence capabilities and build `Fact + Relation + Evidence` |
| `ca-audit` | Publication quality gate |
| `ca-knowledge` | IDs, references, ownership, relation consistency, reverse navigation |
| `ca-visual` | Canonical Markdown → HTML Atlas |

## Core Design Principles

### Evidence First

No current-code evidence, no published semantic claim.

### Business Continuity

Project / Repository / Module boundaries are technical boundaries, not business boundaries.

### Canonical Markdown

> Markdown = Canonical Knowledge

> HTML = Interactive Atlas Viewer

HTML is derived from the current run's Markdown and must not become a second source of truth.

The viewer is business-first and searchable: start from a Business Map or search a business/technical name, drill through Scenario → Process → Object/Data → Code, and navigate back from code to business.

### Full Analysis, Full Generation

```text
Current Code
  ↓
Discover
  ↓
Model
  ↓
Trace
  ↓
Challenge
  ↓
Converge
  ↓
Audit
  ↓
Canonical Markdown
  ↓
HTML Atlas
```

## Repository Layout

```text
code-atlas/
├── LICENSE
├── AGENTS.md
├── README.md
├── README.zh-CN.md
├── docs/
│   ├── code-atlas-design.md
│   └── decisions/
│       └── ADR-*.md
└── skills/
    ├── code-atlas/
    │   ├── SKILL.md
    │   ├── references/
    │   └── templates/
    ├── ca-code-intel/
    ├── ca-project/
    ├── ca-business/
    ├── ca-scenario/
    ├── ca-chain/
    ├── ca-audit/
    ├── ca-knowledge/
    └── ca-visual/
```

## List Code Atlas Skills

```bash
npx skills add simba1949/code-atlas --list
```

Expected skills:

```text
code-atlas
ca-code-intel
ca-project
ca-business
ca-scenario
ca-chain
ca-audit
ca-knowledge
ca-visual
```

This lists only skills available from `simba1949/code-atlas`.

## Install

### Interactive installation

```bash
npx skills add simba1949/code-atlas
```

### Install the complete bundle to Codex

```bash
npx skills add simba1949/code-atlas --skill '*' -a codex -y
```

### Install the complete bundle to Claude Code

```bash
npx skills add simba1949/code-atlas --skill '*' -a claude-code -y
```

### Install to Claude Code and Codex

Recommended:

```bash
npx skills add simba1949/code-atlas --skill '*' -a claude-code -a codex -y
```

### Global installation

```bash
npx skills add simba1949/code-atlas --skill '*' -a claude-code -a codex -g -y
```

`--skill '*'` applies only to skills discovered from `simba1949/code-atlas`.

## Update

Update only the Code Atlas skills.

### Project scope

```bash
npx skills update code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -y
```

### Global scope

```bash
npx skills update code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -g -y
```

Avoid bare `npx skills update` if you do not want unrelated installed skills to be updated.

## Remove

Remove only Code Atlas skills.

### Remove from Claude Code and Codex

```bash
npx skills remove code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -a claude-code -a codex -y
```

### Remove global installation

```bash
npx skills remove code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -g -y
```

Do not use `npx skills remove --all` for Code Atlas removal because it can affect unrelated user skills.

## Output

Code Atlas generates one Canonical Atlas per analysis workspace.

Single project:

```text
project/docs/code-atlas/
```

Multi-project workspace:

```text
workspace/
├── project-a/
├── project-b/
└── docs/code-atlas/
```

Project boundaries are represented inside the shared Atlas; a cross-project analysis is not split into one Atlas per project.

An interactive HTML Atlas can then be derived from the same Canonical Markdown, with global search, Scenario Process Flow, progressive technical drill-down, cross-project Execution Chain visualization, and code-to-business reverse navigation.

Typical output covers:

- System / Project / Module
- Business Domain / Capability
- Scenario / Process
- Business Object / Lifecycle
- Execution Chain
- DB / RPC / MQ / Job / External System
- business → code drill-down
- code → business reverse navigation



For financial systems, Code Atlas explicitly challenges account state, transaction intent, and obligation fulfillment as separate responsibilities. A withdrawal is not assigned to the Account Domain merely because it changes balance; final ownership remains evidence-backed.

## License

Licensed under the [Apache License 2.0](./LICENSE).
