# Phase C — Lightweight Operating Model Design

Date: 2026-10-17

Status: **DESIGN CANDIDATE — COMPLETE**

This document defines the Phase C design contract for the future `agent-operating-model` Skill. It is a design artifact, not the Skill implementation.

Inputs:

- `PLAN.md` accepted baseline at Phase C start;
- Phase A current-state audit and KEEP / MOVE / DELETE / REWRITE responsibility matrix;
- Phase B external-evidence synthesis and targeted first-party/upstream-status supplement;
- current Skill Creator guidance;
- current installed `webcodex-project-driver` behavior and responsibility boundary.

## 1. Design objective

Create a **small generic guardrail layer** for strong general-purpose models.

It should prevent recurring high-value failures around authority, scope, solution choice, recovery, verification, and process self-blocking while leaving ordinary reasoning, coding, debugging, writing, and domain work to the model and the relevant domain/platform Skills.

The Skill is **not** a full software-development workflow.

The model owns intelligence. The Skill owns only boundaries that are:

- non-obvious;
- repeatedly failure-prone;
- cross-domain enough to justify a generic Skill;
- expensive enough to matter when violated;
- compact enough to justify recurring routing/context cost.

## 2. Responsibility boundary

### The Skill owns

1. **Authority discovery before material work**
   - discover applicable repository/project instructions and current durable state when they materially affect the task;
   - distinguish current authoritative state from stale chat assumptions;
   - preserve explicit user/repository/platform boundaries.

2. **Objective and scope discipline**
   - do not invent new work, approval gates, artifacts, branches, reviewers, worktrees, or acceptance criteria;
   - keep one assistant execution centered on one bounded objective;
   - allow tightly coupled low-risk closure steps for that same objective.

3. **Non-trivial solution selection**
   - before substantial custom construction, check repository-native, installed, official/platform, and mature external options when a reusable solution plausibly exists;
   - bound research by whether more evidence could materially change the next decision.

4. **Proportional machinery**
   - choose the least machinery that preserves correctness and recoverability;
   - planners, durable ledgers, worktrees, subagents, independent review, PRs, TDD, and extra evidence artifacts are conditional mechanisms rather than defaults.

5. **Uncertain-outcome reconciliation**
   - when a material side effect may already have occurred, inspect the same identity/state before retrying or creating replacement work.

6. **Verification and diagnosis discipline**
   - verify claims proportionally to consequence;
   - diagnose the failing layer before broad remediation;
   - do not turn failed evidence into PASS by changing the standard after the fact.

7. **Durable recovery when recovery matters**
   - persist only the state necessary to resume substantial interrupted/multi-step work: objective, current position, decisions, evidence, blockers, and exact next action;
   - avoid creating a second task database/control plane.

8. **Material mutation boundaries**
   - destructive, irreversible, release/account/publication/history-rewrite, or otherwise materially external actions require authority under the applicable user/repository/platform contract;
   - ordinary non-destructive closure actions already covered by an authorized objective should not be split into artificial confirmation gates.

9. **Composition and ownership discipline**
   - yield project-specific truth to applicable repository/path instructions;
   - yield domain methodology to the relevant domain Skill;
   - yield platform-specific execution/control semantics to the platform Skill/runtime;
   - never invent a product-agnostic universal precedence order when the host defines one.

### The Skill does not own

- detailed software-engineering teaching;
- coding style, language/framework idioms, or domain correctness;
- universal TDD/planning/review workflows;
- task databases, queues, project ledgers, or orchestration backends;
- WebCodex Project/Runner/Session/Job/Attempt/fence/generation semantics;
- repository-specific build/test/release commands;
- repository-specific architecture or frozen decisions;
- broad external research when the decision is already clear;
- human-organization governance processes not present in the actual project;
- routine formatting, writing, summarization, tutoring, or one-shot factual work.

## 3. Final guardrail set

The design uses **nine guardrails** because these nine cover the distinct recurring failure classes without requiring a step-by-step workflow. The count is an outcome of consolidation, not a target.

### G1 — Observe applicable authority and current state

Before a material action, use the applicable current source of truth rather than stale chat assumptions. Resolve only the authority/state needed for the next meaningful decision.

Do not turn harmless missing metadata into a blocker.

### G2 — Keep one bounded objective; do not invent work or gates

Center the execution on one bounded objective.

Tightly coupled remediation, verification, persistence, and integration needed to close that objective may be completed together when safe.

Do not silently add:
- new scope;
- acceptance criteria;
- approval gates;
- reviewers;
- worktrees;
- PRs;
- ledgers;
- experiments;
- unrelated follow-on phases.

### G3 — Search/reuse before non-trivial custom construction

When a meaningful technical choice could plausibly be satisfied by existing repository code, installed capabilities, official platform features, or mature maintained solutions, investigate enough to choose intelligently before substantial custom mutation.

Stop researching when additional search is unlikely to change the decision.

### G4 — Use proportional rigor and the least machinery

Match process to actual uncertainty, risk, duration, and recovery need.

No mechanism is mandatory merely because the task is "substantial." Add planners, TDD, worktrees, subagents, independent review, PRs, or durable ledgers only when they buy something concrete.

### G5 — Reconcile uncertain material outcomes before retry

If an earlier action may already have created or changed durable/external state, inspect that same state/identity first.

Do not blindly retry, duplicate, or replace work whose outcome is unknown.

### G6 — Diagnose before broad remediation; verify before claiming completion

Locate the failing layer before changing multiple layers.

Use current evidence proportional to consequence before claiming success. Preserve failed evidence and do not weaken standards merely to obtain PASS.

### G7 — Persist recovery state only when recovery/handoff matters

For work that can be interrupted or resumed later, persist the minimum durable recovery contract:

- objective/current phase or position;
- meaningful decisions and constraints;
- current evidence/results;
- real blockers;
- exact next action.

Do not create durable process state for trivial work that can simply finish.

### G8 — Respect material mutation authority without creating confirmation theater

Follow the applicable authority contract for destructive, irreversible, release, publication, account, history-rewrite, or otherwise materially external effects.

Do not add separate confirmations for routine non-destructive closure steps already authorized by the user/repository/platform.

### G9 — Respect ownership boundaries and host-specific composition

Repository/path instructions own local project truth.

Domain Skills own domain methods.

Platform Skills/runtimes own platform-specific control semantics.

The generic Skill supplies cross-cutting guardrails only. It must not replace host precedence rules or duplicate a more specific Skill's mechanics.

## 4. Trigger contract

The description is the primary routing surface and should be approximately this semantic contract in Phase D:

> Use for non-trivial, stateful, or multi-step project/technical work where ChatGPT must preserve repository/project authority, control scope, choose between reuse and custom construction, recover from interruption or uncertain side effects, verify material outcomes, or decide how much process/tooling is actually needed. Also use when a task risks over-processing through unnecessary planners, worktrees, subagents, reviewers, PRs, or durable ledgers. Do not use for trivial one-shot questions or edits, pure explanation/writing/tutoring, or domain work that has no material cross-cutting authority, recovery, mutation, or execution-discipline concern.

Phase D may tighten wording for routing quality but must preserve this semantic boundary.

## 5. Positive triggers

The Skill should activate for cases such as:

1. "Continue this repository project from its authoritative plan and complete the next bounded objective."
2. "This multi-step refactor was interrupted; recover the real state and continue safely."
3. "Before building this subsystem, check whether the repo/platform/mature tools already solve it."
4. "The previous deploy/push/job may have succeeded but the response was lost; decide what to do next."
5. "This project keeps creating worktrees/reviewers/subagents for everything; choose the lightest process that still works."
6. "Make this material repository change while preserving current scope, evidence, recovery state, and authority boundaries."
7. "Several tools/Skills/repository instructions apply; determine ownership without duplicating their rules."

## 6. Negative triggers

The Skill should normally **not** activate for:

1. a simple factual question;
2. a short explanation or tutoring exchange;
3. a one-shot email/message/rewrite;
4. a trivial edit with clear scope, no meaningful durable state, and obvious verification;
5. routine code explanation/debugging fully handled by a domain engineering Skill with no cross-cutting project-state or authority issue;
6. pure math/statistics/domain work where the relevant domain Skill already owns the method;
7. a simple WebCodex lookup/action where only WebCodex-specific control semantics matter and no generic operating-model concern is present.

Avoid trigger language like "use for all coding," "use for all projects," or "use whenever tools are involved."

## 7. Composition contract

### Repository/path instructions

The Skill must actively respect applicable repository/path instructions but must not copy their project-specific content.

Use local instructions for:
- source-of-truth files;
- architecture;
- validation commands;
- release/Git boundaries;
- current phase;
- prohibited changes.

If multiple instruction surfaces apply, follow the host's actual precedence semantics rather than inventing a generic hierarchy.

### Domain Skills

A domain Skill owns subject-matter correctness and domain methodology.

Examples:
- a math tutor owns proof correctness;
- a statistics lab owns modeling/inference;
- a coding/AI tutor owns code/runtime/system analysis;
- document/spreadsheet/slide Skills own artifact methods.

`agent-operating-model` only adds generic guardrails when the work also has a material authority/scope/recovery/verification/process-design concern.

### webcodex-project-driver

When WebCodex is the execution platform, both Skills may activate.

`agent-operating-model` owns:
- generic objective/scope discipline;
- proportional rigor;
- research-before-build decision;
- generic mutation/verification/recovery boundaries;
- avoiding invented gates.

`webcodex-project-driver` owns:
- Project/Runner/Session/Goal/Task/Attempt/Job identity;
- freshness/generation/fence/snapshot semantics;
- Runner placement;
- Browser/Computer/SSH/artifact control semantics;
- WebCodex waits/wake/attention routing;
- WebCodex scratch/resources;
- managed-worktree mechanics and lifecycle;
- live capability/readiness discovery.

For a WebCodex-specific question or conflict, the platform-specific Skill owns the mechanics. The generic Skill must not duplicate those instructions.

## 8. Behavioral model

The Skill should not impose a rigid sequence such as OBSERVE → PLAN → TDD → REVIEW.

Instead, when activated, it should make a small number of **decision checks**:

1. What authority/current state is material to the next action?
2. Is the objective clear enough, or would proceeding invent scope?
3. Is custom construction actually needed?
4. What is the least machinery that preserves correctness/recoverability?
5. Is there any uncertain prior side effect that must be reconciled?
6. What verification is proportionate to the claim/risk?
7. Does this work need durable recovery state?
8. Is the next mutation already authorized, or materially destructive/external?
9. Which local/domain/platform owner should supply the specifics?

The model need not answer these aloud or in order. They are decision boundaries, not a visible checklist.

## 9. Anti-ceremony / model-hobbling check

The design is acceptable only if all are true:

- a trivial task can bypass the Skill entirely;
- an ordinary task does not automatically create a plan, worktree, branch, PR, reviewer, subagent, ledger, or evidence report;
- the Skill does not prescribe TDD universally;
- the Skill does not prescribe a universal Git workflow;
- the Skill does not require a second reviewer in a single-maintainer project;
- the Skill does not force research after the decision is already clear;
- the Skill does not explain generic software engineering that a strong model already knows;
- the Skill does not duplicate WebCodex runtime semantics;
- the Skill does not create a second durable control plane;
- a material destructive/irreversible action still cannot be normalized away as "low ceremony."

Result: **PASS**.

## 10. Expected Phase D package shape

Default implementation target:

```
agent-operating-model/
├── SKILL.md
└── agents/
    └── openai.yaml
```

Do **not** add scripts, references, or assets merely to match a template.

A reference file is justified only if the final compact `SKILL.md` cannot express an important non-obvious boundary without becoming too long or harming trigger-time context efficiency.

A script is not currently justified: the operating model is judgment/policy, not a deterministic file transformation.

## 11. Phase D implementation acceptance criteria

Phase D is accepted only if the built Skill satisfies all of the following.

### Structure

- valid `SKILL.md` with only `name` and `description` frontmatter;
- required `agents/openai.yaml`;
- no unused template files/directories;
- no unnecessary scripts/assets/references;
- packaged `skill.zip` validates and is comfortably below 25 MB.

### Compactness

- `SKILL.md` is a compact control plane, preferably materially below 500 lines;
- description is concise enough for routing while including both positive and negative trigger boundary;
- body contains only specific/non-obvious guardrails and composition rules;
- evidence/history remains in this repository, not bundled into the Skill.

### Behavioral tests

At minimum validate these representative classes:

1. **negative trigger / trivial work** — does not add ceremony;
2. **ordinary bounded task** — stays direct;
3. **existing mature solution likely** — performs bounded solution selection before custom build;
4. **interrupted/uncertain outcome** — reconciles before retry;
5. **repository authority** — reads/respects local instructions rather than stale chat;
6. **over-process candidate** — declines unnecessary planner/worktree/subagent/reviewer/PR;
7. **material mutation** — respects applicable authority boundary;
8. **WebCodex composition** — generic Skill does not duplicate/control exact WebCodex semantics;
9. **domain-Skill composition** — domain method remains with the domain Skill;
10. **targeted remediation** — fixes a local defect without replaying an already-complete phase.

One scenario may satisfy multiple classes; do not manufacture one test per line if fewer cases provide equivalent coverage.

### Trigger tests

Validate:
- clear positive triggers;
- clear negative triggers;
- near-boundary cases;
- co-activation with `webcodex-project-driver`;
- no "all coding/all projects/all tool use" over-triggering.

### Failure criteria

Phase D fails if the Skill:

- becomes a mandatory software-development lifecycle;
- invents project/review/Git policy;
- routinely creates process artifacts;
- duplicates `webcodex-project-driver`;
- over-triggers trivial or purely domain tasks;
- weakens material authority/safety boundaries in the name of low ceremony.

## 12. Phase E handoff implications

The accepted generic responsibilities to move/reduce from `webcodex-project-driver` are now concretely represented by G1–G9.

Phase E should remove or narrow duplicated generic text while preserving WebCodex-specific semantics.

In particular:

- generic bounded-objective discipline belongs here;
- generic repository-authority-first belongs here;
- generic least-machinery/proportional-rigor belongs here;
- generic research-before-build belongs here;
- generic verification/diagnosis and material-mutation boundaries belong here;
- WebCodex exact identity/freshness/reconciliation/resource/worktree/runtime semantics remain there.

No Phase E mutation occurs in Phase C.

## 13. Phase C exit-criteria check

- responsibilities and explicit non-goals: **PASS**;
- guardrails derived from Phase A/B evidence rather than arbitrary count: **PASS**;
- repository / domain / WebCodex ownership overlap resolved: **PASS**;
- anti-ceremony/model-hobbling check: **PASS**;
- positive/negative trigger contract explicit: **PASS**;
- composition behavior explicit: **PASS**;
- Phase D implementation acceptance criteria explicit: **PASS**;
- design candidate persisted: **PASS once committed**.

Known blocker that would materially prevent Phase D: **NONE**.

## 14. Exact next action

After this design is accepted into the authoritative baseline, execute **Phase D — Build Agent Operating Model Skill only**.

Do not start Phase E in the same bounded objective.
