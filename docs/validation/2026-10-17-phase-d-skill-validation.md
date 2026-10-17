# Phase D — Agent Operating Model Skill Validation

Date: 2026-10-17

Status: **PASS**

Candidate: `agent-operating-model`

Scope: pre-install implementation validation. This validates package structure, trigger contract, representative behavior, composition boundaries, compactness, and packaging. It does not install the Skill into the ChatGPT account and does not modify `webcodex-project-driver`.

## Candidate shape

```text
agent-operating-model/
├── SKILL.md
└── agents/
    └── openai.yaml
```

No scripts, references, assets, template placeholders, task database, or second control plane are bundled.

## Official validator / packager

Commands executed with the current Skill Creator scripts:

- `quick_validate.py <candidate>` → PASS
- `package_skill.py <candidate> <dist>` → PASS

Observed package:

- `SKILL.md`: 94 lines, 6,259 bytes
- `agents/openai.yaml`: 123 bytes
- `skill.zip`: 3,133 bytes
- `skill.zip` SHA256: `7a0509a6792aadcd20fc8ce254d4dcb7124c51b02ac35c545939df0c18c540e9`
- ZIP entries: exactly `agent-operating-model/SKILL.md` and `agent-operating-model/agents/openai.yaml`

The package is comfortably below the 25 MB upload limit.

## Static contract checks

Additional local checks passed:

- `SKILL.md` frontmatter contains exactly `name` and `description`;
- `name` is exactly `agent-operating-model`;
- description contains both positive and negative trigger boundaries;
- description does not use broad triggers such as “all coding”, “all projects”, or “all tool use”;
- body contains all nine Phase C guardrail families;
- body delegates exact WebCodex mechanics to `webcodex-project-driver`;
- no universal worktree/TDD/PR/reviewer requirement is present;
- no extra resource directories survive initialization cleanup.

## Trigger and representative-behavior matrix

These are pre-install contract tests against the accepted description/body. They validate intended routing and required behavior before account installation. Runtime auto-invocation itself is deferred to the later integrated/account validation stage.

| Case | Prompt shape | Expected routing | Expected behavior | Result |
| --- | --- | --- | --- | --- |
| T1 | “What is a Markov chain?” | Do not trigger | Pure domain explanation; domain tutor owns the method. | PASS |
| T2 | “Fix this typo in README.” | Normally do not trigger | Direct trivial edit; no plan/branch/reviewer/ledger ceremony. | PASS |
| T3 | “Continue this repo from PLAN.md and finish the current objective.” | Trigger | Read applicable authority/current state, keep one bounded objective, persist only useful recovery state. | PASS |
| T4 | “Before writing a custom sync service, check whether the repo/platform already provides one.” | Trigger | Bounded repository/official/mature-solution search; prefer Adopt → Extend → Compose → Build; stop when decision is clear. | PASS |
| T5 | “The deploy may have succeeded but the response was lost.” | Trigger | Reconcile the same durable/external state before retrying. | PASS |
| T6 | “This project keeps creating worktrees and reviewers for every change.” | Trigger | Choose proportional rigor/least machinery; optional process must not become a gate. | PASS |
| T7 | “Rewrite history on main and publish the result.” | Trigger | Respect material mutation authority; do not normalize destructive/external effects as low ceremony. | PASS |
| T8 | “Use WebCodex to continue a durable multi-run task.” | Co-activate with `webcodex-project-driver` | Generic Skill owns scope/proportionality; WebCodex Skill owns exact Project/Runner/Session/Task/Attempt/Job/fence/runtime mechanics. | PASS |
| T9 | “Debug this Python function; no repo/project state is relevant.” | Normally defer to engineering/domain Skill | Do not hijack routine domain methodology. | PASS |
| T10 | “Phase B is complete, but one sentence in PLAN is stale; fix it.” | Trigger or compose if project state is active | Targeted remediation; do not replay Phase B or invent a new review gate. | PASS |

## Near-boundary routing checks

### B1 — Ordinary multi-file implementation with clear local instructions

Expected: the Skill may activate if project authority/recovery/process-selection is material, but it must not force a separate plan, worktree, PR, reviewer, TDD regime, or evidence report. Repository instructions own exact commands and policy.

Result: PASS.

### B2 — Simple WebCodex status lookup

Expected: `webcodex-project-driver` can act alone when the question is purely about WebCodex-specific state and no generic scope/recovery/process issue exists.

Result: PASS.

### B3 — Domain work plus material project-state risk

Expected: co-activate. Domain Skill owns correctness/method; `agent-operating-model` owns only cross-cutting authority/scope/recovery/verification boundaries.

Result: PASS.

## Anti-ceremony check

Candidate does not:

- prescribe a fixed software-development lifecycle;
- force planning/TDD/worktrees/subagents/review/PRs;
- require a second reviewer for a single-maintainer project;
- create a task database or durable orchestration layer;
- force research when the choice is already settled;
- teach generic software engineering;
- duplicate WebCodex runtime semantics.

Candidate still preserves authority boundaries for destructive, irreversible, publication, account, release, protected-branch, and history-rewrite effects.

Result: PASS.

## Phase C acceptance mapping

- valid Skill structure: PASS
- compact `SKILL.md`: PASS
- positive triggers: PASS
- negative triggers: PASS
- near-boundary cases: PASS
- ordinary bounded task remains direct: PASS
- search/reuse case: PASS
- uncertain-outcome reconciliation: PASS
- repository authority: PASS
- over-processing avoidance: PASS
- material mutation boundary: PASS
- WebCodex composition: PASS
- domain-Skill composition: PASS
- targeted remediation: PASS
- no unnecessary resources: PASS
- packaging: PASS

## Known limitation

This is pre-install validation. It proves package validity and design/trigger contract consistency, but not account-side auto-invocation behavior. Account installation/update is intentionally reserved for the later authorized closeout stage; integrated adversarial validation can re-check routing behavior on the actual installed surface.

## Verdict

**PASS — accepted Phase D implementation candidate.**

No finding requires redesigning Phase C or adding supporting files.
