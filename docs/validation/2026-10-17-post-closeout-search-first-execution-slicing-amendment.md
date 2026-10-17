# Post-Closeout Targeted Amendment — Search-First Handoff and Execution Slicing

Date: 2026-10-17

Status: **PASS**

Scope: targeted amendment to the accepted `agent-operating-model` after Phase H closeout. This is not a new project phase.

## Motivation

The original user problems were re-audited after closeout. Two gaps remained:

1. search/reuse existed, but the Skill did not strictly require presenting the mature-solution choice to the user before substantial implementation/installation/experimentation;
2. bounded-objective discipline existed, but one bounded objective could still grow into an overlong assistant execution.

This amendment closes only those two gaps.

## Source change

Updated file:

`skill/agent-operating-model/SKILL.md`

Candidate Git blob:

`b1392ae06fc8f58d0125100d0588100ccd9d1c0c`

UI metadata remains unchanged:

`skill/agent-operating-model/agents/openai.yaml`

Git blob:

`6502c3d76549169b4838f94cfbd2ab41d854256f`

## Amendment A — search, select, present, then implement

For unfamiliar or non-trivial problems where a mature solution plausibly exists and the solution choice is not already settled, the Skill now requires:

1. search repository-native, installed, official/platform, and mature maintained options;
2. bound research by whether more evidence could change the decision;
3. present the material options;
4. present the recommended solution;
5. explain why it is recommended;
6. present the intended implementation approach;
7. stop before substantial mutation or experimentation so the user sees the solution choice first.

The handoff is skipped when:

- the user already accepted the solution in the current conversation;
- an authoritative project instruction already settled the solution;
- the work is trivial and the choice is obvious.

This preserves low ceremony while enforcing the user's original "solution first, attempts second" preference.

## Amendment B — execution-sized slices inside a bounded objective

The Skill now distinguishes:

- **bounded objective** — what the current execution is trying to accomplish;
- **execution slice** — the next independently verifiable portion that can finish cleanly in one assistant execution.

If a bounded objective still contains multiple independently verifiable slices, the model must:

1. select the next slice with one observable result;
2. complete it;
3. verify it proportionally;
4. persist the minimum recovery state and exact next action;
5. stop instead of chaining later slices merely because they belong to the same objective.

The Skill also explicitly forbids mechanical micro-slicing when the current work can safely finish in one execution.

Tightly coupled remediation, verification, persistence, and integration required to close the current slice/objective remain allowed in the same execution.

## Targeted contract checks

The final candidate was checked for the following properties:

- execution-slice rule present: **PASS**
- independently verifiable slice boundary present: **PASS**
- mechanical micro-slicing explicitly rejected: **PASS**
- solution-selection / implementation separation present: **PASS**
- options + recommendation + reason + intended approach required: **PASS**
- stop-before-substantial-mutation rule present: **PASS**
- already-accepted solution bypass preserved: **PASS**
- trivial/obvious choice bypass preserved: **PASS**
- tightly coupled closure steps still allowed: **PASS**
- trivial negative trigger preserved: **PASS**

## Skill Creator validation

Current Skill Creator validator and packager:

- `quick_validate.py`: **PASS**
- `package_skill.py`: **PASS**

Final package:

- structure: exactly `SKILL.md` + `agents/openai.yaml`
- `SKILL.md`: 98 lines / 7,701 bytes
- `agents/openai.yaml`: 123 bytes
- `skill.zip`: 3,584 bytes
- concrete archive SHA256: `d42b552ab2fb7021d8ca75a7eefc17f1732b73c3f4da6a4636f511d0476fce8d`

As recorded in Phase H, archive SHA identifies this concrete ZIP instance; Git source/blob content remains the reproducible source authority.

## Representative behavior

### Case 1 — unfamiliar mature-solution candidate

Prompt shape:

> I need a save-sync solution. Search first; do not start building yet.

Expected:

- research mature/repository/platform options;
- present options/recommendation/reason/implementation approach;
- stop before mutation.

Result: **PASS**

### Case 2 — solution already accepted

Prompt shape:

> We already chose Syncthing. Continue the authorized configuration.

Expected:

- no repeated solution-selection handoff;
- proceed within current bounded execution slice.

Result: **PASS**

### Case 3 — large bounded objective with several independent checkpoints

Prompt shape:

> Complete the whole migration module, including implementation, integration, regression, docs, and cleanup.

Expected:

- identify the next independently verifiable execution slice;
- complete/verify/persist that slice;
- stop with exact next action rather than chaining all remaining checkpoints.

Result: **PASS**

### Case 4 — small task

Prompt shape:

> Fix this typo and verify the exact line.

Expected:

- no artificial execution slicing;
- no solution survey;
- finish directly.

Result: **PASS**

## Verdict

**PASS — targeted amendment accepted.**

No `webcodex-project-driver` source change is required.

The source project remains closed; this is a post-closeout correction to the final AOM Skill, not a new Phase I.
