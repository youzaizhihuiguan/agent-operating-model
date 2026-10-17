# Agent Operating Model — Authoritative Plan

## Objective

Design and implement a lightweight `agent-operating-model` Skill for strong general-purpose models, while refactoring `webcodex-project-driver` so that generic agent discipline and WebCodex-specific control semantics are cleanly separated.

The governing principle is:

> The model owns intelligence; Skills own high-value boundaries, non-obvious platform semantics, and recurring failure prevention.

Project profile:

- this is a single-maintainer, primarily self-use project;
- optimize for correctness, recoverability, and low ceremony rather than organizational governance theater;
- do not assume a second human reviewer, change board, compliance process, or collaboration workflow exists;
- once the user authorizes a bounded objective, do not invent additional approvals or process gates unless repository policy, platform safety, or a material current risk actually requires them.

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

Large projects may remain in one Chat window, but one assistant execution should normally own one bounded objective. Tightly coupled low-risk closure steps for that same objective may be completed together; unrelated substantive phases should not be chained merely because time remains.

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

Phase A measured the real disk sources on accessible Mac and Windows Runners. The dominant source was replicated Rust `target/` output inside multiple managed worktrees, not Git worktree metadata itself.

Accepted design direction:

- default to an existing suitable checkout;
- create a managed worktree only for real isolation or concurrency requirements;
- reuse an existing suitable managed worktree when safe;
- account for build-output amplification, not just Git metadata;
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

### 6. Process self-blocking and invented gates

A workflow can become self-defeating when an agent promotes an optional mechanism into a mandatory gate that the authoritative contract never required.

Observed example in this project:

- Phase B exit criteria did not require independent review;
- an integration attempt nevertheless blocked itself waiting for an independent reviewer;
- the extra gate added delay without reducing a contract-defined risk.

Rule:

> A task is blocked only by an explicit user/repository/platform requirement or by a material current risk that must be resolved before safe continuation.

Do not invent reviewers, approvals, PRs, worktrees, ledgers, or other ceremony merely because they might be useful in some projects.

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

## External evidence baseline

Phase B satisfied the external-evidence requirement. Do not reopen broad framework research unless a later design question exposes a specific gap that could materially change the decision.

The completed research covered mature projects and community feedback including:

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
- add files, references, scripts, or infrastructure merely for structure;
- invent approval, reviewer, PR, worktree, or evidence gates that are not required by the user, repository, platform, or a material current risk.

## Execution policy for this project

One Chat window may own the entire project, but assistant execution must remain bounded.

Each assistant execution should own one bounded objective, not an arbitrary number of unrelated phases.

A bounded objective may include tightly coupled low-risk steps needed to finish that same objective — for example a targeted fix, verification, commit, and integration closeout — when doing them together is clearer and safely fits the execution window. Do not chain multiple substantive phases merely because time remains.

At the end of a material bounded objective:

1. verify the result;
2. persist only the durable state/evidence that has recovery, audit, or handoff value;
3. record the exact next action when future recovery matters;
4. stop before beginning the next substantive phase.

Reconcile uncertainty before continuing when the uncertainty is material to identity, authority, safety, or side effects. Do not turn harmless metadata gaps into blockers.

Completed phases stay complete unless new evidence invalidates a material premise. Later corrections should normally be targeted remediation, not a full replay of the phase.

Chat history is not the long-term project source of truth.

## Phases

### Phase A — Current-State Audit

Goal:

- load current Skill-creation requirements;
- inspect the installed/current `webcodex-project-driver`;
- verify live GitHub/WebCodex/account state;
- audit current responsibilities;
- classify content as KEEP / MOVE / DELETE / REWRITE;
- inspect current Git/worktree policy;
- inspect actual worktree-related disk/resource usage on the relevant Mac/Windows Runners when accessible, distinguishing managed worktrees from build outputs, dependency trees, caches, and other generated data.

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
- define boundaries with repository instructions and `webcodex-project-driver`;
- define the Skill trigger/composition contract, including positive triggers, negative triggers, co-activation with `webcodex-project-driver`, and authority precedence.

### Phase D — Build Agent Operating Model Skill

Goal:

- initialize the new Skill with the official Skill Creator flow and required structure;
- keep `SKILL.md` compact;
- include required `agents/openai.yaml`;
- add references/scripts only when they materially improve reliability;
- validate positive triggers, negative triggers, composition behavior, and representative task behavior;
- package and validate the complete Skill as `skill.zip` within the platform size limit.

### Phase E — Slim WebCodex Project Driver

Goal:

- remove or reduce generic methodology;
- preserve WebCodex-specific semantics;
- rewrite managed-worktree policy;
- preserve recovery, reconciliation, durable execution, and evidence safety.

Implementation occurs in the independent `youzaizhihuiguan/webcodex-project-driver` repository.

### Phase F — Repository Instruction Contract (conditional)

Goal:

- close any repository-instruction gap that remains after Phase C/E;
- define or refine a lightweight `AGENTS.md` model only if a separate deliverable still has material value;
- separate stable repository policy from dynamic execution state;
- avoid mass-updating unrelated repositories without explicit authorization.

If Phase C and the existing repository instructions already resolve this contract, Phase F may be marked NOT NEEDED with a short rationale instead of creating work for its own sake.

### Phase G — Integrated Adversarial Validation

Validate the smallest representative scenario set that covers the distinct high-value failure classes. One scenario may cover multiple bullets; there is no requirement to execute every candidate separately.

Candidate coverage includes:

- trivial / negative-trigger task;
- ordinary implementation task;
- unfamiliar problem where a mature solution may exist;
- large or interrupted work requiring durable recovery;
- uncertain outcome or dirty workspace;
- multi-agent / worktree candidate where topology should remain conditional;
- destructive or authority-bearing action;
- repository-local rule / composition case;
- verification that is intentionally proportional rather than automatically heavy.

Success requires that:

- simple tasks remain simple;
- heavy ceremony is not forced;
- the generic Skill and WebCodex Skill do not duplicate each other;
- trigger and authority precedence are clear;
- repository-local authority remains clear;
- recovery guarantees are preserved.

### Phase H — Closeout

Goal:

- final verification against the accepted design and current repository state;
- Git evidence;
- package validation;
- rollback point;
- concise final report;
- independent review only when the user requests it or when a concrete material risk makes independence valuable;
- then decide formal ChatGPT-account installation of the new `agent-operating-model` Skill and update of the existing `webcodex-project-driver` Skill identity.

## Phase exit criteria

A phase is complete only when its required result and evidence are durable in the authoritative repository. A phase is blocked when a required authority, capability, source, or decision is unavailable and continuing would require guessing, silently changing the contract, or accepting an unapproved material risk.

### Phase A exit criteria

Phase A is COMPLETE only when:

- the live installed/current `webcodex-project-driver` and its authoritative source state have been verified;
- current responsibilities are classified as KEEP / MOVE / DELETE / REWRITE;
- current managed-worktree guidance has been specifically audited;
- actual worktree-related disk/resource usage on the relevant accessible Runners has been measured sufficiently to identify the material sources of the reported >100 GB footprint, or the phase is explicitly BLOCKED on unavailable Runner evidence rather than guessing;
- discrepancies between chat assumptions and real repository/runtime/account state are resolved;
- audit evidence and the exact next action are persisted;
- no implementation mutation has been made.

### Phase B exit criteria

Phase B is COMPLETE only when:

- the principal mature external approaches selected for review have been examined sufficiently to support comparison;
- recurring failure-mode categories and meaningful community-reported drawbacks are captured;
- important candidate patterns have an explicit ADOPT / ADAPT / REJECT decision with rationale;
- sources supporting material design conclusions are recoverable;
- no known mandatory research gap remains that would materially change the design;
- the exact next action is persisted.

### Phase C exit criteria

Phase C is COMPLETE only when:

- `agent-operating-model` responsibilities and non-goals are explicit;
- the invariant/guardrail set is derived from evidence and real failure modes rather than an arbitrary rule count;
- overlap with repository-local instructions and `webcodex-project-driver` is resolved;
- the design has been checked for unnecessary ceremony or over-constraint;
- implementation acceptance criteria are explicit;
- positive/negative trigger behavior and Skill-composition expectations are explicit;
- the design candidate and exact next action are persisted.

### Phase D exit criteria

Phase D is COMPLETE only when:

- the official Skill structure is valid;
- the Skill implementation matches the accepted Phase C design;
- unnecessary example/resources files have been removed;
- positive-trigger, negative-trigger, composition, and representative behavior tests pass;
- the complete Skill validates and packages successfully as `skill.zip`;
- a recoverable Git commit identifies the accepted candidate.

### Phase E exit criteria

Phase E is COMPLETE only when:

- the `webcodex-project-driver` implementation repository has been modified from verified live state;
- generic methodology has been removed, moved, or reduced according to the accepted responsibility map;
- WebCodex-specific recovery and control guarantees remain intact;
- managed-worktree selection, reuse, and cleanup policy is evidence-based and validated;
- the complete updated Skill validates and packages successfully;
- the implementation commit and relevant validation evidence are linked back here; record a branch or PR only if one is actually used.

### Phase F exit criteria

Phase F is COMPLETE or NOT NEEDED when:

- any material repository-instruction gap remaining after Phase C/E is resolved;
- stable repository policy is cleanly separated from dynamic execution state;
- composition among `AGENTS.md`, authoritative plans/state, and Skills is sufficiently clear for actual use;
- no unrelated repository has been mass-modified without separate authorization;
- if no separate Phase F work is needed, that decision is recorded briefly rather than manufacturing an artifact.

### Phase G exit criteria

Phase G is COMPLETE only when:

- representative adversarial scenarios have been executed against the integrated design;
- simple tasks remain lightweight;
- required recovery/safety behavior still works;
- generic and WebCodex-specific Skills do not duplicate or contradict each other;
- repository-local authority precedence is clear;
- material findings are either closed or explicitly recorded as blockers.

### Phase H exit criteria

Phase H is COMPLETE only when:

- final validation has no unresolved mandatory finding under the actual project contract;
- any independent review that was explicitly requested or risk-justified has been resolved, but absence of an independent reviewer is not itself a blocker;
- final Git refs, package validation, and rollback points are recorded;
- final report identifies authoritative source versions;
- installation of `agent-operating-model` is treated as a new Skill installation;
- `webcodex-project-driver` is updated in its existing Skill identity rather than creating a duplicate;
- any ChatGPT-account installation/update is either completed and verified or explicitly left as a separate authorized action.

## Git / branch / PR policy

- Treat `main` as the accepted baseline.
- Use a purpose-named branch when it materially improves rollback, isolation, or a multi-step change; direct work on `main` is acceptable for small low-risk documentation/governance changes when repository policy permits it.
- Preserve a recoverable commit at material boundaries.
- PRs are optional integration/review surfaces, not mandatory phase gates for this single-maintainer self-use project.
- Integration is normally the final step of an already-authorized bounded objective, not a separate phase that must be invented for every branch.
- A verified branch may be merged directly when the user/repository has already authorized the bounded objective and no applicable safety or repository rule requires extra review.
- Once a bounded objective is authorized, ordinary non-destructive Git operations needed to complete it — such as commit, push, updating an existing PR, or an authorized integration merge — do not each require separate confirmation.
- Release, history rewrite, destructive cleanup, account installation/update, and other materially irreversible/external effects still require the authority defined by the user/project/platform.
- Never create a branch, PR, reviewer, or worktree as ceremony alone.

## Evidence and provenance policy

Material claims that influence architecture, safety, workflow boundaries, or Skill behavior must be traceable.

For external evidence, retain enough information to recover:

- source / project / discussion;
- the observed failure mode or design mechanism;
- the relevant conclusion;
- the ADOPT / ADAPT / REJECT decision and rationale.

For implementation evidence, retain enough information to recover:

- repository and branch;
- commit or candidate identity;
- validation/test result;
- important runtime/environment facts;
- unresolved blockers;
- exact next action.

Do not convert anecdotal community feedback into universal policy without corroboration or a clear project-specific reason.

Failed formal evidence remains evidence. Do not hide, overwrite, or redefine it merely to produce a PASS.

Evidence should be proportional to consequence. A commit/diff plus a relevant test or verification result is enough for many low-risk changes; do not create a separate evidence document, ledger, review, or integration report when it adds no recovery or decision value.

Because this repository is public, durable evidence must follow minimum-necessary disclosure:

- never commit credentials, access tokens, cookies, passwords, private keys, raw authentication material, or other secrets;
- do not commit unnecessary personal identifiers or sensitive local-environment data;
- redact or summarize sensitive runtime identifiers and paths when exact disclosure is not needed for reproduction or audit;
- preserve enough evidence to verify the claim without dumping entire private runtime state.

## Current state

Status: PHASE H COMPLETE — SOURCE PROJECT CLOSED; ACCOUNT SKILL MUTATION NOT PERFORMED

Completed:

- project purpose clarified;
- new Skill renamed from the working title `project-agent-discipline` to `agent-operating-model`;
- authoritative repository selected: `youzaizhihuiguan/agent-operating-model`;
- high-level architecture and major known problems captured;
- bounded-objective execution policy agreed;
- bootstrap plan independently re-audited;
- phase exit criteria, Git/PR policy, evidence provenance, trigger/composition requirements, public-repository evidence hygiene, and account-installation boundaries defined;
- bootstrap governance closed;
- Phase A verified the current Skill Creator requirements;
- Phase A verified the installed/current `webcodex-project-driver` against its live authoritative source state;
- Phase A completed the KEEP / MOVE / DELETE / REWRITE responsibility audit;
- Phase A specifically audited the managed-worktree policy;
- Phase A measured accessible Mac and Windows managed-worktree roots and identified replicated Rust `target/` outputs as the dominant material source of the >100 GB footprint;
- Phase A evidence is persisted at `docs/audit/2026-10-17-phase-a-current-state-audit.md`;
- Phase B examined the principal mature external approaches and representative issue/community failure evidence;
- Phase B synthesized recurring failure modes and explicit ADOPT / ADAPT / REJECT decisions;
- Phase B found no mandatory research gap that would materially block Phase C;
- Phase B evidence is persisted at `docs/research/2026-10-17-phase-b-external-evidence-synthesis.md`;
- Phase B targeted supplement added current first-party OpenAI / GitHub / Anthropic evidence and corrected Superpowers, GSD, Trellis, and Compound Engineering upstream status without changing any ADOPT / ADAPT / REJECT classification;
- the supplemented Phase B evidence still has no mandatory research gap that would materially change Phase C;
- Phase B was integrated into `main` through PR #2, and the accepted Phase B evidence/governance is now part of the authoritative baseline for Phase C;
- Phase C defined the `agent-operating-model` responsibility boundary, explicit non-goals, nine evidence-derived guardrails, positive/negative trigger contract, repository/domain/WebCodex composition contract, anti-ceremony checks, and Phase D implementation acceptance criteria;
- Phase C design is persisted at `docs/design/2026-10-17-phase-c-lightweight-operating-model-design.md`;
- Phase C found no blocker that would materially prevent Phase D;
- Phase D implemented the minimal `agent-operating-model` Skill at `skill/agent-operating-model/` with only `SKILL.md` and `agents/openai.yaml`;
- Phase D validated the Skill with the current Skill Creator validator/packager plus trigger, boundary, anti-ceremony, WebCodex-composition, domain-composition, and targeted-remediation checks;
- Phase D validation is persisted at `docs/validation/2026-10-17-phase-d-skill-validation.md`;
- validated `skill.zip` size is 3,133 bytes with SHA256 `7a0509a6792aadcd20fc8ce254d4dcb7124c51b02ac35c545939df0c18c540e9`;
- Phase D found no blocker that would materially prevent Phase E;
- Phase E refactored `youzaizhihuiguan/webcodex-project-driver` from verified baseline `82fd58e65e114a4878ef512c6f90093d1bb4046c` and integrated the accepted implementation at `0d41a941eb51533f004db60b3dfee33f72a3c0ae`;
- Phase E narrowed `webcodex-project-driver` to WebCodex-specific identity, freshness, runtime, orchestration, placement, resource/scratch, wait/wake, cross-surface, recovery, and evidence semantics while delegating generic guardrails to project-local policy / `agent-operating-model`;
- Phase E rewrote managed-worktree policy to default to an existing suitable checkout, require concrete isolation/concurrency/preservation value for new worktrees, prefer safe reuse, account separately for generated-output amplification, and use Git/WebCodex-aware terminal cleanup;
- Phase E validation is persisted in the implementation repository at `docs/benchmark/2026-10-17-v0.3.2-operating-model-split-validation.md`;
- the validated Phase E `webcodex-project-driver` package is 35,961 bytes with SHA256 `532f303e90ea690bae1e046f943f8e3bd0074eedf859e40499dc7a55736636ef`;
- Phase E found no blocker that would materially prevent the conditional Phase F assessment;
- Phase F assessed the repository instruction contract and found **no material remaining gap** after Phase C/E;
- root `AGENTS.md` already serves as the short stable repository entrypoint: it directs agents to authoritative `PLAN.md`, requires live-state recovery, bounded execution, acceptance-boundary preservation, low-ceremony Git/process behavior, targeted remediation, and material-effect authority;
- dynamic project state, current phase, detailed architecture/non-goals, validation criteria, and Exact Next Action remain in `PLAN.md` rather than being duplicated into standing instructions;
- the accepted `agent-operating-model` and `webcodex-project-driver` composition contracts already assign repository/project truth to local instructions without inventing a universal host precedence hierarchy;
- Phase F therefore requires no `AGENTS.md` rewrite, nested instruction file, new artifact, or unrelated repository mutation and is recorded as **NOT NEEDED**;
- Phase G executed six integrated adversarial scenarios covering negative/trivial routing, ordinary bounded repository work, bounded search/reuse, interrupted/outcome-uncertain WebCodex recovery, dirty-workspace/worktree and multi-agent pressure, and material authority boundaries;
- Phase G also performed a read-only live WebCodex runtime check: both Runners were online/build-aligned, managed-worktree/instruction capabilities were available, and coding-agent provider readiness was absent, confirming that capability exposure must not be treated as provider readiness;
- Phase G found no material duplication or contradiction among root repository instructions, `agent-operating-model`, domain ownership, and Phase E `webcodex-project-driver`;
- Phase G evidence is persisted at `docs/validation/2026-10-17-phase-g-integrated-adversarial-validation.md`;
- Phase G found no mandatory source remediation and no blocker that would materially prevent Phase H;
- Phase H revalidated and repackaged both final Skill sources with the current Skill Creator validator/packager;
- Phase H confirmed the final `agent-operating-model` source blobs `29fe10d9d6e97f030c7f28a5cb11340cde434076` / `6502c3d76549169b4838f94cfbd2ab41d854256f` and the final Phase E `webcodex-project-driver` source at `0d41a941eb51533f004db60b3dfee33f72a3c0ae`;
- Phase H recorded final source/package/rollback evidence at `docs/validation/2026-10-17-phase-h-final-closeout.md`;
- Phase H found that ZIP SHA256 is an artifact-instance fence rather than a reproducible source identity because archive timestamps can change while internal file bytes remain identical;
- Phase H found no unresolved mandatory source/design/validation finding;
- Phase H did not mutate ChatGPT account Skills: `agent-operating-model` remains uninstalled and the installed `webcodex-project-driver` remains the pre-Phase-E account representation;
- a post-closeout audit against the user's four original problems found two remaining AOM gaps: mature-solution selection was not guaranteed to be presented before substantial attempts, and a bounded objective could still exceed a reasonable single assistant execution;
- the targeted post-closeout amendment closes those gaps by requiring a solution-selection handoff before substantial mutation when the solution is not already settled, and by requiring execution-sized independently verifiable slices when a bounded objective is still too large;
- the amended `agent-operating-model/SKILL.md` Git blob is `b1392ae06fc8f58d0125100d0588100ccd9d1c0c`;
- targeted amendment validation is persisted at `docs/validation/2026-10-17-post-closeout-search-first-execution-slicing-amendment.md`;
- the amended concrete package is 3,584 bytes with archive SHA256 `d42b552ab2fb7021d8ca75a7eefc17f1732b73c3f4da6a4636f511d0476fce8d`;
- this correction does not create a Phase I; the source project remains closed.

Optional external follow-on only:

- separately authorized ChatGPT-account installation of the **post-closeout amended** `agent-operating-model` and update of the existing `webcodex-project-driver` identity, followed by installed-surface routing/composition verification, when an account Skill-management interface is available.

## Current decision record

1. `agent-operating-model` is the generic upper-layer Skill and project name.
2. `webcodex-project-driver` remains an independent WebCodex-specific Skill and repository.
3. Mature external frameworks are evidence sources, not default installation targets.
4. There is no fixed 10–15 invariant limit.
5. The final Skill should be as complete as justified by evidence while staying lightweight.
6. Worktree policy is WebCodex-specific and belongs primarily in `webcodex-project-driver`.
7. Durable project truth belongs in GitHub rather than chat history.
8. Trigger precision and Skill composition are explicit design/validation requirements; over-triggering is a failure.
9. Phase A measured the reported >100 GB worktree/resource issue: replicated Rust `target/` outputs inside managed worktrees were the dominant material source. Final policy should therefore control isolation frequency, reuse, build-output amplification, and safe cleanup.
10. This public repository stores only minimum necessary, non-secret evidence.
11. External research keeps recoverable provenance for material design claims, but license review is not a mandatory project phase gate.
12. Bootstrap completed on the accepted `main` baseline. Phase A happened to use audit branch + PR #1 for integration and produced the authoritative Phase A baseline; that historical choice is not a general requirement for later phases.
13. Standing instruction surfaces carry recurring routing/context cost: Skill metadata participates in discovery while the full body/resources are loaded on activation. Keep the trigger surface and activated core compact, defer detail through progressive loading, retain only evidence-backed high-value boundaries/current platform semantics/recurring failure prevention, and re-evaluate them after material model or harness changes.
14. Proportional rigor is required: trivial and ordinary work must not pay a mandatory full-spec, TDD, worktree, multi-agent, or independent-review tax.
15. Research-before-build applies to non-trivial, decision-relevant choices where a mature option plausibly exists; it should be repository-first, source-aware, bounded, and skippable rather than universal ceremony.
16. Plans should normally specify intent, contracts, constraints, dependencies, and acceptance criteria rather than complete implementation bodies or duplicated execution recipes.
17. Worktrees and multi-agent topology require concrete isolation, concurrency, context-reset, or independence value; task size labels alone are not sufficient justification.
18. Durable plans/ledgers/state and independent review are conditional mechanisms for complex or long-horizon work, recovery, handoff, risk, or genuinely material gates rather than mandatory phases for every task. In this single-maintainer self-use project, lack of a second reviewer is never a blocker unless the user explicitly requested independent review or a concrete risk justifies it. When durable planning is warranted, preserve current progress, decisions/rationale, discoveries/evidence, acceptance state, and recovery/next-action information.
19. Knowledge compounding promotes only stable, verified, reusable lessons to the narrowest authoritative surface; observations and run history must not automatically become always-on context.
20. The generic operating model must not create a second task database or control plane; it composes with repository-owned state and platform-specific control systems.
21. Repository- and path-specific rules should live at the narrowest authoritative local scope that owns them. The generic operating model should require discovery and respect of applicable local instructions, but exact instruction precedence is host semantics and must not be replaced by a universal hierarchy invented by the Skill.
22. Phase B was integrated through PR #2 because that integration surface already existed; this is historical provenance, not a precedent that future phases require PRs or independent reviewers.
23. This is a single-maintainer, primarily self-use project. Optimize governance for direct progress, correctness, and recoverability rather than organizational ceremony.
24. Do not invent process gates. Optional mechanisms become blocking only when required by explicit user/repository/platform policy or by a concrete material risk in the current task.
25. One assistant execution owns one bounded objective, but may complete tightly coupled remediation, verification, Git persistence, and integration steps for that same objective when doing so is safe and efficient.
26. PRs, independent reviewers, standalone evidence reports, worktrees, and subagents are conditional tools, not default milestones.
27. Project-local standing authority may cover routine non-destructive Git operations needed to complete an already-authorized bounded objective; the generic Skill must still treat authority-bearing effects according to the applicable user/repository/platform contract.
28. Do not replay a completed phase merely because a later wording/state issue is found. Use targeted remediation unless new evidence invalidates a material premise of that phase.
29. Phase C consolidates the generic operating model into nine decision guardrails, not a nine-step workflow: authority/current state, bounded objective/no invented gates, bounded search/reuse, proportional machinery, uncertain-outcome reconciliation, diagnosis/verification, conditional durable recovery, material-mutation authority, and ownership/composition.
30. The generic Skill may co-activate with repository instructions, domain Skills, and `webcodex-project-driver`, but it must not duplicate their owned specifics or invent a universal host precedence hierarchy.
31. Phase D should start from a minimal package shape — `SKILL.md` plus `agents/openai.yaml` — and add references/scripts/assets only if implementation evidence shows they materially improve reliability or context efficiency.
32. Phase D accepted the minimal two-file package without supporting resources; pre-install routing/behavior validation passes, while account-side auto-invocation remains intentionally deferred to later integrated/account validation.
33. The accepted Phase D package artifact is identified by SHA256 `7a0509a6792aadcd20fc8ce254d4dcb7124c51b02ac35c545939df0c18c540e9`; repository source plus validation evidence are authoritative for regeneration until account installation is explicitly authorized.
34. Phase E accepted `webcodex-project-driver@0d41a941eb51533f004db60b3dfee33f72a3c0ae` as the repository baseline that composes with `agent-operating-model`; account installation remains unchanged until the later authorized installation stage.
35. The accepted managed-worktree policy is: existing suitable checkout by default; new managed worktree only for concrete isolation/concurrency/preservation value; safe reuse before creation; generated-output amplification treated separately from Git metadata; terminal cleanup only after exact ownership/evidence/ref reconciliation.
36. The accepted Phase E `webcodex-project-driver` package is identified by SHA256 `532f303e90ea690bae1e046f943f8e3bd0074eedf859e40499dc7a55736636ef` and validation evidence at `youzaizhihuiguan/webcodex-project-driver/docs/benchmark/2026-10-17-v0.3.2-operating-model-split-validation.md`.
37. Phase F is **NOT NEEDED**: the existing root `AGENTS.md` is already the appropriate lightweight stable repository contract, while `PLAN.md` owns dynamic phase/state/acceptance details; adding another instruction layer would duplicate authority and standing context without closing a material gap.
38. Phase G accepted the integrated source composition without remediation: simple tasks remain simple, search/reuse is bounded, worktree/multi-agent topology remains conditional, destructive/release authority stays repository/user-owned, and WebCodex-specific recovery semantics remain intact.
39. Phase G live-runtime evidence confirms that advertised capabilities are not equivalent to provider readiness; current WebCodex source must continue to discover provider/Host/Runner readiness live rather than inferring from API/tool exposure.
40. Account-side routing is intentionally still unproven at Phase G because `agent-operating-model` is not yet installed and the installed `webcodex-project-driver` is still the pre-Phase-E account version; this is a Phase H installation/verification concern, not a Phase G failure.
41. Final reproducibility authority is the Git source commit/blob content, not the ZIP SHA alone. A ZIP SHA identifies one concrete packaged artifact and may change when archive metadata/timestamps change even when internal Skill file bytes are identical.
42. Phase H final revalidation produced `agent-operating-model` archive SHA256 `97b7b763db753ebac6782639e03ceff3bc4fd2f521bac0eab75f0b2a940b720b` (3,133 bytes) and `webcodex-project-driver` archive SHA256 `532f303e90ea690bae1e046f943f8e3bd0074eedf859e40499dc7a55736636ef` (35,961 bytes), both with current Skill Creator validation PASS.
43. Repository/source rollback points are `3622a5c10ba06154f036074868cdc75f9b0b0326` for the pre-Phase-D Agent Operating Model baseline and `82fd58e65e114a4878ef512c6f90093d1bb4046c` for the pre-Phase-E WebCodex Project Driver baseline.
44. Phase H closes the source/design/validation project without ChatGPT account mutation. Any later account install/update is a separate explicitly authorized external action, not a new project phase.
45. For unfamiliar or non-trivial work where a mature solution plausibly exists and the choice is not already settled, solution selection and implementation are separate: search boundedly, present material options/recommendation/reason/intended approach to the user, and stop before substantial mutation or experimentation. Skip this handoff when the solution is already accepted or the work is trivial/obvious.
46. A bounded objective must also be execution-sized. When one objective contains multiple independently verifiable slices, complete and verify the current slice, persist the minimum recovery state and exact next action, then stop; do not keep chaining later slices merely because they belong to the same objective. Do not mechanically micro-slice work that can safely finish now.
47. The post-closeout correction is a targeted amendment to the final AOM Skill, not a reopened phase. Current AOM source authority is the amended Git source/blob plus its targeted validation evidence; the Phase H closeout report remains historical evidence for the pre-amendment state.

## Exact next action

No project phase remains.

The source/design/validation project is closed. The only optional follow-on is a **separately authorized ChatGPT account action** to:

1. install the post-closeout amended `agent-operating-model` Skill from Git blob `b1392ae06fc8f58d0125100d0588100ccd9d1c0c` / its validated package;
2. update the existing `webcodex-project-driver` logical Skill identity to the accepted Phase E source;
3. verify installed identities/content plus representative positive/negative routing and AOM/WPD composition;
4. preserve the previous installed WPD revision as the account rollback target when the platform exposes revision history.

Do not invent a Phase I or reopen completed source phases merely because account installation has not been performed.
