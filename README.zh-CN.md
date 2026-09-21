# Code Atlas

[English](./README.md) | [简体中文](./README.zh-CN.md)

Code Atlas 是一套面向复杂代码库的多 Skill 分析系统，用于将当前可见代码转换成**可导航、可验证、可追溯**的业务与技术知识 Atlas。

它不止分析 Class、Method 或 Call Graph，而是从业务视角建立：

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

同时建立 Business Object 生命周期轴：

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

核心原则：

> 技术边界可以切分代码，但不能切断业务。

> 所有正式知识都必须能够追溯到当前可见代码事实。

## Code Atlas 能解决什么问题

Code Atlas 用于帮助工程师回答：

- 当前系统由哪些 Project / Module 构成？
- 系统实现了哪些 Business Domain？
- 每个 Domain 提供哪些 Capability？
- 同一个 Capability 下有哪些真实 Scenario？
- 每个 Scenario 如何编排 Process？
- Process 内部有哪些 Sub-process / Step？
- 哪些 Business Object 参与其中？
- Business Object 的 Lifecycle 如何变化？
- 哪些 Execution Chain 实现了某个 Process / Step？
- DB / RPC / MQ / Job / External System 如何参与整条业务？
- 一个业务如何跨多个 Project / Module 连续运行？
- 如何从业务下钻到代码？
- 如何从代码反查业务？

## Skills

Code Atlas 当前由 9 个 Skill 组成：

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

### 用户可直接使用的 Skill

| Skill | 职责 |
|---|---|
| `code-atlas` | 全量编排：发现 → 建模 → 追链 → 反证 → 收敛 → 审计 → 发布 |
| `ca-project` | System / Project / Module / 技术底图 |
| `ca-business` | Business Domain / Capability / Business Object / Lifecycle |
| `ca-scenario` | Scenario / Process / Sub-process / Step |
| `ca-chain` | Execution Chain / Common Chain |

### 内部支撑 Skill

| Skill | 职责 |
|---|---|
| `ca-code-intel` | 发现代码智能能力，并生成 `Fact + Relation + Evidence` |
| `ca-audit` | 发布前质量门禁 |
| `ca-knowledge` | ID、引用、Owner、关系一致性、反向查询 |
| `ca-visual` | Canonical Markdown → HTML Atlas |

## 核心设计原则

### Evidence First

没有当前代码证据，不生成正式业务结论。

### 技术边界不能切断业务

Project / Repository / Module 属于技术边界，而不是业务边界。

### Markdown 是唯一事实源

> Markdown = Canonical Knowledge

> HTML = 交互式 Atlas Viewer

HTML 只能从当前全量生成的 Markdown 派生，不能成为第二套事实源。

Viewer 默认从业务出发，并支持全局搜索：既可以从业务地图逐层进入 Scenario → Process → 对象/数据 → 代码，也可以从类、方法、表、RPC、MQ、Job 反查所属业务。

### 全量分析、全量生成

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

## 仓库结构

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

## 列举 Code Atlas Skills

查看 `simba1949/code-atlas` 仓库中包含哪些 Skill：

```bash
npx skills add simba1949/code-atlas --list
```

预期包含：

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

这个命令只查询 `simba1949/code-atlas` 仓库。

## 安装

### 交互式安装

```bash
npx skills add simba1949/code-atlas
```

### 安装完整 Code Atlas 到 Codex

```bash
npx skills add simba1949/code-atlas --skill '*' -a codex -y
```

### 安装完整 Code Atlas 到 Claude Code

```bash
npx skills add simba1949/code-atlas --skill '*' -a claude-code -y
```

### 同时安装到 Claude Code 与 Codex

推荐：

```bash
npx skills add simba1949/code-atlas --skill '*' -a claude-code -a codex -y
```

### 全局安装

```bash
npx skills add simba1949/code-atlas --skill '*' -a claude-code -a codex -g -y
```

`--skill '*'` 只表示安装 `simba1949/code-atlas` 仓库中的全部 Skill，不会影响用户其他 Skill。

## 更新

只更新 Code Atlas 自己的 9 个 Skill。

### 项目级更新

```bash
npx skills update code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -y
```

### 全局更新

```bash
npx skills update code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -g -y
```

如果不希望影响用户其他 Skill，不建议直接使用：

```bash
npx skills update
```

## 删除

只删除 Code Atlas 自己的 Skill。

### 从 Claude Code 与 Codex 删除

```bash
npx skills remove code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -a claude-code -a codex -y
```

### 删除全局安装

```bash
npx skills remove code-atlas ca-code-intel ca-project ca-business ca-scenario ca-chain ca-audit ca-knowledge ca-visual -g -y
```

不要使用：

```bash
npx skills remove --all
```

因为这可能删除用户其他 Skill。

## 输出

Code Atlas 会为一次分析工作区生成一套 Canonical Atlas。

单项目：

```text
project/docs/code-atlas/
```

多项目：

```text
workspace/
├── project-a/
├── project-b/
└── docs/code-atlas/
```

跨项目分析不会在每个 Project 下分别生成 Atlas；Project 边界会在同一套 Atlas 内表达。

随后可以基于同一套 Canonical Markdown 生成 HTML Atlas。

主要内容包括：

- System / Project / Module
- Business Domain / Capability
- Scenario / Process
- Business Object / Lifecycle
- Execution Chain
- DB / RPC / MQ / Job / External System
- 业务到代码的下钻关系
- 代码到业务的反向导航

## License

本项目采用 [Apache License 2.0](./LICENSE)。
