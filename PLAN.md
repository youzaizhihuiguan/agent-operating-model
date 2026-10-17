# Agent Operating Model — Authoritative Plan

## Objective

Design and implement a lightweight `agent-operating-model` Skill for strong general-purpose models, while refactoring `webcodex-project-driver` so that generic agent discipline and WebCodex-specific control semantics are cleanly separated.

The governing principle is:

> The model owns intelligence; Skills own high-value boundaries, non-obvious platform semantics, and recurring failure prevention.

This project must not recreate a heavy workflow framework such as Superpowers, GSD, Compound Engineering, or Trellis. Those systems may be researched as evidence sources, but their full workflow machinery must not be copied by default.

## Authoritative repositories

### Agent operating model

Repository:

`youzaizhihuiguan/agent-operating-model`

This repository is the authoritative source for:

- the `agent-operating-model` Skill;
- its design and research;
- its durable project state;
- its validation evidence;
- its final packaged source.

### WebCodex project driver

Repository:

`youzaizhihuiguan/webcodex-project-driver`

This remains an independent implementation repository for the existing `webcodex-project-driver` Skill.

Changes to that Skill must be implemented and validated in its own repository. This repository may record the corresponding branch, commit, PR, evidence, and design relationship.

## Target architecture

```text
agent-operating-model
    |
    | generic agent behavior boundaries
    | failure prevention
    | repository authority
    | scope / execution / verification discipline
    |
    v
repository-local AGENTS.md / project instructions
    |
    | project-specific truth and constraints
    |
    v
when the execution platform is WebCodex
    |
    v
webcodex-project-driver
    |
    | WebCodex-specific execution/control semantics
    v
WebCodex runtime
```

## Known real-world problems

These are confirmed design inputs, not merely hypothetical concerns.

### 1. Reinventing solutions before searching mature options

Agents may start writing code, scripts, infrastructure, or custom workflows before checking:

- existing repository implementation;
- already-installed dependencies or tools;
- official product/platform capabilities;
- mature external libraries or community solutions.

For non-trivial technical choices, prefer evidence gathering and solution selection before substantial mutation.

A useful selection order to investigate is:

```text
Adopt -> Extend -> Compose -> Build
```

This must not become mandatory research ceremony for trivial tasks.

### 2. Executions become too long even after decomposition

A large task may be conceptually decomposed yet still run several packages or phase gates in one assistant execution.

Observed consequences include:

- wall-clock cutoff;
- context truncation;
- unclear interruption point;
- difficult recovery;
- completed and incomplete work becoming mixed.

Large projects may remain in one Chat window, but one assistant execution should normally advance only one bounded package or one phase gate.

The purpose is recoverability, not mechanical micro-tasking.

### 3. WebCodex-managed worktrees consume excessive disk

Mac and Windows systems have accumulated more than roughly 100 GB of WebCodex/worktree-related disk usage.

WebCodex supports:

```text
work_on_project(mode="checkout")
work_on_project(mode="worktree")
```

and `mode="worktree"` can automatically create a managed worktree.

The current `webcodex-project-driver` contains guidance that prefers managed worktrees for substantial write/review work. That policy may be too aggressive.

Before final policy is written, audit the real disk sources and distinguish among:

- managed worktree checkouts;
- orphaned worktrees;
- Rust `target/`;
- `node_modules`;
- virtual environments;
- build outputs;
- caches;
- other generated artifacts.

Current design direction, pending audit:

- default to an existing suitable checkout;
- create a managed worktree only for real isolation or concurrency requirements;
- reuse an existing suitable managed worktree when safe;
- define a safe terminal cleanup lifecycle;
- never equate “substantial work”, “review”, or “long-running work” with an automatic worktree requirement.

### 4. Repository-local rules are not prominent enough

Agents may fail to prioritize repository-owned instructions and may:

- ignore current phase;
- miss frozen architecture;
- forget platform or Runner responsibilities;
- use stale chat state as current truth;
- miss authoritative documents;
- lose the exact next action.

Repositories should provide a short, prominent agent-facing contract such as root `AGENTS.md`.

Stable project rules belong there or in linked authoritative documents. Dynamic execution state should not be indiscriminately stuffed into `AGENTS.md`.

### 5. Heavy agent workflow frameworks can become counterproductive

Modern strong models already provide substantial planning, coding, debugging, testing, tool selection, and general reasoning ability.

A mandatory workflow such as:

```text
brainstorm
-> spec
-> plan
-> worktree
-> implementation
-> TDD
-> multi-agent review
-> finish
```

can add more cost than value for many tasks.

Potential side effects include:

- unnecessary ceremony for small work;
- higher token and wall-clock cost;
- unnecessary worktrees, subagents, and specs;
- workflow rules overriding model judgment;
- duplicated control planes.

Therefore this project should extract durable guardrails and failure-prevention ideas from mature frameworks without copying their full operating systems.

## Agent operating model scope

The new `agent-operating-model` Skill should prevent high-value recurring failure modes while preserving model autonomy.

Candidate categories include, but are not limited to:

- search before building when a mature option may exist;
- solution selection before substantial non-trivial mutation;
- bounded execution and scope control;
- durable recovery and handoff;
- repository authority first;
- least machinery / proportional rigor;
- prefer direct authoritative capabilities over indirect workarounds;
- reconcile unknown outcomes before retry;
- workspace and Git safety;
- disciplined concurrency and ownership;
- verification proportional to risk;
- diagnose before broad remediation;
- destructive / irreversible action boundaries;
- acceptance-contract preservation;
- avoiding silent scope or standard drift.

There is no predetermined number of invariants.

The number must be determined by evidence and failure-mode coverage.

However, every rule must justify its context cost. Do not add rules merely for completeness, and do not turn the Skill into a mandatory step-by-step software-development workflow.

## WebCodex Project Driver target scope

`webcodex-project-driver` should retain only WebCodex-specific semantics and closely related execution safety, including areas such as:

- Project and Runner identity;
- Workflow Session;
- Goal / Agent / AgentTask / Attempt / Wait;
- Job lifecycle;
- identity, freshness, generation, fence, and authority;
- uncertain-outcome reconciliation;
- interruption recovery;
- Runner placement;
- Browser / Computer / SSH / artifact surfaces;
- scratch lifecycle;
- continuation / wake / attention routing;
- live WebCodex capability discovery;
- WebCodex resource ownership;
- WebCodex-managed worktree lifecycle.

Generic software-engineering methodology should be moved to the generic operating model, repository-local instructions, or removed when the model can already handle it reliably.

## Repository-local AGENTS.md role

Repository-local instructions own repository-specific truth, for example:

- repository purpose and non-goals;
- source-of-truth documents;
- frozen architecture and constraints;
- platform / Runner responsibilities;
- validation commands and gates;
- Git / release boundaries;
- prohibited changes;
- location of current durable state and exact-next-action information.

The generic Skill should require agents to honor applicable repository instructions without embedding project-specific content into the generic Skill.

## External evidence requirements

Before finalizing the operating model, research mature projects and community feedback, including where useful:

- obra/superpowers;
- EveryInc Compound Engineering;
- GSD;
- Trellis;
- search-first style Skills;
- SteveVitali/agent-skills;
- Codagent Agent Skills;
- other mature agent/coding workflow repositories;
- GitHub Issues and Discussions;
- Reddit;
- Hacker News;
- LINUX DO.

For each potentially useful design pattern, determine:

```text
What failure mode does it address?
Why does the mechanism work?
What real-world drawbacks are reported?
Is it still useful with current strong models?
Should we adopt the principle, adapt it, or reject it?
```

Do not copy full external workflows merely because they are mature.

## Explicit non-goals

This project must not, without new evidence and explicit justification:

- recreate Superpowers;
- create a large family of micro-Skills;
- create a second task database;
- create a second control plane;
- force planner usage for all tasks;
- force TDD for all tasks;
- force multi-agent execution;
- force worktree use;
- teach generic software engineering in detail;
- enumerate every conceivable scenario;
- add files, references, scripts, or infrastructure merely for structure.

## Execution policy for this project

One Chat window may own the entire project, but assistant execution must remain bounded.

Each assistant execution may complete at most one bounded package or one phase gate.

At the end of each bounded package:

1. verify the result;
2. persist durable state and evidence in the appropriate repository;
3. record the exact next action;
4. stop instead of casually starting the next package.

If repository identity, branch/HEAD, Runner state, authority, or a prior side effect is uncertain, fail closed and reconcile real state before continuing.

Chat history is not the long-term project source of truth.

## Phases

### Phase A — Current-State Audit

Goal:

- load current Skill-creation requirements;
- inspect the installed/current `webcodex-project-driver`;
- verify live GitHub/WebCodex/account state;
- audit current responsibilities;
- classify content as KEEP / MOVE / DELETE / REWRITE;
- inspect current Git/worktree policy.

Constraint:

- audit only;
- do not create the final new Skill;
- do not modify `webcodex-project-driver`;
- do not update installed account Skills.

### Phase B — External Evidence Synthesis

Goal:

- research mature agent Skills/frameworks and community feedback;
- identify recurring failure modes;
- distinguish useful guardrails from outdated compensation mechanisms and excessive ceremony;
- persist evidence-backed design inputs.

Constraint:

- do not create the final Skill yet.

### Phase C — Lightweight Operating Model Design

Goal:

- define the `agent-operating-model` responsibilities;
- derive invariants from evidence and real failure modes;
- define explicit non-goals;
- check that the design does not over-constrain strong models;
- define boundaries with repository instructions and `webcodex-project-driver`.

### Phase D — Build Agent Operating Model Skill

Goal:

- initialize and implement the Skill using the official Skill structure;
- keep `SKILL.md` compact;
- add references/scripts only when they materially improve reliability;
- validate triggering and behavior;
- package and validate the complete Skill.

### Phase E — Slim WebCodex Project Driver

Goal:

- remove or reduce generic methodology;
- preserve WebCodex-specific semantics;
- rewrite managed-worktree policy;
- preserve recovery, reconciliation, durable execution, and evidence safety.

Implementation occurs in the independent `youzaizhihuiguan/webcodex-project-driver` repository.

### Phase F — Repository Instruction Contract

Goal:

- define a lightweight `AGENTS.md` model;
- separate stable repository policy from dynamic execution state;
- avoid mass-updating unrelated repositories without explicit authorization.

### Phase G — Integrated Adversarial Validation

Validate at least representative cases covering:

- trivial task;
- ordinary bug fix;
- unfamiliar technical problem;
- mature third-party solution exists;
- large multi-stage task;
- interrupted execution;
- uncertain outcome;
- dirty workspace;
- multi-agent candidate;
- worktree-isolation candidate;
- destructive action;
- frozen repository rule;
- too-heavy verification;
- too-light verification;
- handoff to fresh context.

Success requires that:

- simple tasks remain simple;
- heavy ceremony is not forced;
- the generic Skill and WebCodex Skill do not duplicate each other;
- trigger and authority precedence are clear;
- repository-local authority remains clear;
- recovery guarantees are preserved.

### Phase H — Closeout

Goal:

- independent review;
- Git evidence;
- package validation;
- rollback point;
- final report;
- only then decide formal ChatGPT-account Skill installation/update.

## Current state

Status: PROJECT BOOTSTRAP

Completed:

- project purpose clarified;
- new Skill renamed from the working title `project-agent-discipline` to `agent-operating-model`;
- authoritative repository selected: `youzaizhihuiguan/agent-operating-model`;
- high-level architecture and major known problems captured;
- bounded-turn policy agreed.

Not yet completed:

- Phase A live audit;
- external evidence synthesis;
- final invariant set;
- Skill implementation;
- WebCodex Skill refactor;
- integrated validation;
- account installation/update.

## Current decision record

1. `agent-operating-model` is the generic upper-layer Skill and project name.
2. `webcodex-project-driver` remains an independent WebCodex-specific Skill and repository.
3. Mature external frameworks are evidence sources, not default installation targets.
4. There is no fixed 10–15 invariant limit.
5. The final Skill should be as complete as justified by evidence while staying lightweight.
6. Worktree policy is WebCodex-specific and belongs primarily in `webcodex-project-driver`.
7. Durable project truth belongs in GitHub rather than chat history.

## Exact next action

Execute **Phase A — Current-State Audit only**.

The next execution should:

1. load the official Skill Creator guidance;
2. load the currently installed `webcodex-project-driver`;
3. verify live GitHub / WebCodex / installed-Skill state;
4. inspect the current `webcodex-project-driver` source and references;
5. produce a KEEP / MOVE / DELETE / REWRITE responsibility matrix;
6. audit the current managed-worktree policy;
7. persist Phase A evidence and the next exact action;
8. stop without starting Phase B.
