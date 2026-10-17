# Phase A — Current-State Audit

Date: 2026-10-17

Status: **COMPLETE**

Scope: audit only. No `webcodex-project-driver` source was modified, no final `agent-operating-model` Skill was created, and no ChatGPT account Skill was installed or updated.

## 1. Authoritative inputs

Agent Operating Model baseline:

- repository: `youzaizhihuiguan/agent-operating-model`
- accepted baseline branch: `main`
- baseline HEAD observed before Phase A: `01efebf743f99e2042d18aefb1013e22f9c02b09`
- authoritative project files read before stateful work: root `AGENTS.md` and `PLAN.md`
- Phase A execution branch: `audit/phase-a-current-state-20261017`

WebCodex Project Driver source:

- repository: `youzaizhihuiguan/webcodex-project-driver`
- remote `main`: `82fd58e65e114a4878ef512c6f90093d1bb4046c`
- `review/v0.3.1-operational-hardening`: `82fd58e65e114a4878ef512c6f90093d1bb4046c`
- `review/v0.3-webcodex-native-orchestration`: `a483b07a11d055d08e7c398ff3b9a641baa4650d`
- `review/v0.2-control-plane`: `489437fff928becab4d9082a0eb67920fda2e753`

The current authoritative v0.3.1 source candidate is therefore commit `82fd58e65e114a4878ef512c6f90093d1bb4046c`; remote `main` and the v0.3.1 review branch currently point to the same commit.

## 2. Current Skill Creator requirements

The current installed Skill Creator guidance was read before this audit. Material requirements relevant to later phases are:

- Skills are compact reusable instruction bundles with a required `SKILL.md`.
- `SKILL.md` frontmatter contains only `name` and `description`; the description is the primary trigger contract.
- `agents/openai.yaml` is required UI metadata.
- Keep `SKILL.md` concise and use progressive loading; detailed material belongs in directly linked references when it materially improves reliability.
- Scripts/references/assets are optional and should exist only when useful.
- Current guidance targets a compact entrypoint (roughly below 500 lines) rather than a knowledge dump.
- Final packaging must validate and produce `skill.zip`; the packaged Skill must remain within the 25 MB upload limit.
- Existing-Skill updates preserve the full Skill and repackage the full result rather than returning a partial patch.

No Skill was created or repackaged in Phase A.

## 3. Installed ChatGPT Skill state

The current ChatGPT Skill catalog exposes the logical Skill:

`skills://webcodex-project-driver`

with the expected v0.3.1 trigger description.

Comparison against `youzaizhihuiguan/webcodex-project-driver@82fd58e...` found:

- all 16 reference files under `skill/references/` are byte-identical to the installed read surface;
- installed `SKILL.md` body is byte-identical to the source body after excluding YAML frontmatter; the installed Skill read surface strips that frontmatter, while the catalog exposes the same description;
- installed `agents/openai.yaml` preserves the same interface display name and short description, but the account/runtime representation is enriched with product/implicit-invocation policy metadata and normalized YAML formatting.

Conclusion: the installed logical Skill materially corresponds to the `82fd58e...` v0.3.1 source. The account representation is not a raw byte-for-byte package mirror because platform metadata normalization/enrichment is present.

## 4. Live WebCodex runtime state

Observed WebCodex runtime:

- runtime version: `0.4.6`
- server and both accessible Runners are build-aligned to commit `5072608fe74b2bfa6e8f208515c04242f475582f`
- accessible Runners: one macOS Runner and one Windows Runner
- both Runners were online and idle at audit start
- both advertise managed-worktree, Git, jobs, shell/script, file, Browser/Computer, Skill runtime/management, and related capabilities
- no CodingAgent provider is currently configured on either Runner

No `agent-operating-model` WebCodex Project was registered at audit start. Phase A persistence therefore uses the authoritative GitHub repository directly rather than manufacturing a new WebCodex workspace.

## 5. Existing webcodex-project-driver workspace state

The primary macOS checkout registered for `webcodex-project-driver` is **not** the authoritative source state:

- local branch: `main`
- local HEAD: `698b6514cfc69d2fb39d532449244bd9f1810852`
- upstream: `origin/main`
- observed relation: behind upstream by 5 commits
- worktree: dirty
- observed changed path: `skill.zip`

The checkout also has several existing managed worktrees, including worktrees at the v0.2, v0.3 and v0.3.1 review commits.

This local stale/dirty checkout was intentionally left untouched. Phase A source conclusions use the verified remote refs and exact Git objects rather than silently updating or cleaning the checkout.

## 6. Responsibility matrix

The following matrix classifies the current responsibilities of `webcodex-project-driver`. Disposition is a design input for later phases; Phase A does not edit the Skill.

| Current responsibility | Primary source area | Disposition | Audit conclusion |
| --- | --- | --- | --- |
| Exact WebCodex Project/Session/Task/Job/resource identity | core invariants, execution model | KEEP | Platform-specific control truth. |
| Freshness/generation/fence/snapshot discipline | core invariants, execution model, recovery | KEEP | Distinctive WebCodex safety semantics. |
| WebCodex authority/capability/readiness discovery | capabilities, execution model | KEEP | Must remain live-runtime driven. |
| Reconcile uncertain WebCodex outcomes before retry | core, recovery | KEEP | Generic principle exists, but exact replay/Job/Attempt/resource semantics belong here. |
| Workflow Session semantics and durable handoff | execution/recovery/orchestration | KEEP | WebCodex-specific durable collaboration. |
| Goal/Agent/AgentTask/Attempt/Wait/Wake semantics | orchestration, continuation | KEEP | WebCodex-native lifecycle. |
| Job/process/persistent-shell lifecycle | long-running work, capabilities | KEEP | Runtime-specific execution ownership. |
| Runner placement and cross-platform execution identity | runner placement | KEEP | Platform-specific resource placement. |
| Browser/Computer/SSH/artifact control semantics | cross-surface operations | KEEP | WebCodex-specific surfaces and freshness rules. |
| Scratch ownership and Runner-local scratch lifecycle | scratch lifecycle | KEEP | Tied to WebCodex Runner/Project/task identities. |
| Host continuation/wake/attention routing | continuation, attention routing | KEEP | Platform-specific; readiness must remain separate from API exposure. |
| Control-plane evidence provenance, exact integration candidate identity and failed-evidence preservation | validation/audit | KEEP | Keep the WebCodex provenance layer; do not own domain quality methods. |
| Domain/project/WebCodex ownership composition | composition boundaries | REWRITE | Retain the WebCodex ownership boundary, but compose below the new generic operating model and repository-local authority without duplicate generic rules. |
| Bounded execution as a general agent discipline | core/execution model | MOVE | Becomes a generic `agent-operating-model` responsibility; WebCodex keeps only mechanics needed to persist/recover bounded work. |
| Repository authority first / do not rely on chat memory | core/recovery/software adaptation | MOVE | Generic rule belongs in the upper operating model and repository instructions; WebCodex keeps exact Project/runtime reconciliation. |
| “Least machinery” / proportional topology as a general default | orchestration | MOVE | Generic principle belongs above; this Skill should keep only when/why WebCodex topology primitives are needed. |
| Generic “do not invent work when objective is ambiguous” | core/recovery | MOVE | Generic operating-model rule; WebCodex can retain a narrow execution-side fail-closed statement. |
| Generic verification discipline / diagnose before remediation | validation/audit, autonomy | MOVE | Upper-layer discipline; WebCodex keeps exact run/evidence/reconciliation semantics. |
| Generic destructive/irreversible decision boundaries | autonomy, cross-surface, closeout | MOVE | Upper-layer rule; retain only WebCodex-specific external-effect and authority mechanics. |
| Generic Git branch/checkpoint/push/release advice | software-git-adaptation | MOVE | Repository/operating-model concern. Keep only WebCodex managed-worktree and runtime integration semantics. |
| Generic software-development methodology or explanatory duplication after responsibilities move | several references | DELETE | Remove duplicated teaching text that provides no WebCodex-specific semantics once the upper layer exists. |
| Repeated generic “continue automatically / do not ask continue?” guidance | autonomy/execution/attention | DELETE | Keep one platform-specific decision-boundary contract; remove redundant generic phrasing. |
| Managed-worktree selection policy | software-git-adaptation | REWRITE | Current selection proxy is too aggressive and is contradicted by observed disk cost. |
| Managed-worktree cleanup/reuse lifecycle | software-git-adaptation + closeout | REWRITE | Must explicitly cover reuse, terminal cleanup, generated-output cost and registry/filesystem drift. |
| Skill trigger/composition description after the upper Skill exists | `SKILL.md` description + composition | REWRITE | Future description should trigger on WebCodex control-plane work, not generic substantial-project discipline. |

### Matrix summary

The intended split is not “move most of the Skill.” The durable WebCodex control plane remains substantial. The primary reduction is removal of generic agent/software methodology that can be owned once by `agent-operating-model` or repository-local instructions.

## 7. Managed-worktree policy audit

Current v0.3.1 guidance says to *prefer* a WebCodex-managed worktree for substantial write/review work when several isolation conditions may apply.

Audit verdict: **REWRITE**.

The problem is not that managed worktrees are intrinsically wrong. The problem is using task size or review as a broad proxy for isolation. In the current environment, each isolated worktree can accumulate a separate multi-gigabyte dependency/build tree. Repeated native validation creates multiplicative disk cost.

Evidence-backed requirements for the later rewrite:

1. Existing suitable checkout should be the default when isolation/concurrency is not actually required.
2. “Substantial,” “review,” or “long-running” must not independently imply a new worktree.
3. A new managed worktree needs a concrete isolation reason such as concurrent mutable branches, preservation of a stable checkout, independent audit requiring filesystem isolation, or preservation of failed/experimental state.
4. Reuse an existing suitable managed worktree when its exact repository/candidate/state is compatible and reuse is safe.
5. Worktree selection must account for generated-output cost, especially Rust `target/` and large dependency trees.
6. Terminal managed worktrees require an explicit safe cleanup/reuse decision after evidence and unique state are reconciled.
7. Registry/filesystem drift must be observable and must not be handled by blind directory deletion.
8. Build-cache/output policy should be considered separately from Git worktree policy; deleting or recreating worktrees alone does not solve repeated build-output amplification.

No cleanup policy was implemented in Phase A.

## 8. Runner disk/resource evidence

Read-only scans were performed on the accessible macOS and Windows managed-worktree roots. The scan classified directory contents by material source.

### macOS managed-worktree root

- filesystem directories observed: 62
- runtime Projects matching the managed-worktree root: 59
- total measured size: **99.116 GiB**
- Rust `target/`: **93.829 GiB**
- `node_modules/`: **0.949 GiB**
- Python virtual environments: **2.636 GiB**
- cache-class directories: **0.015 GiB**
- build/dist/out-class directories outside the categories above: **0.054 GiB**
- remaining checkout/other data: **1.633 GiB**
- configured project scratch root: effectively empty at scan time

Largest observed macOS managed worktree: about **20.489 GiB**, of which about **20.267 GiB** was Rust `target/`.

### Windows managed-worktree root

- filesystem directories observed: 26
- runtime Projects matching the managed-worktree root: 25
- total measured size: **58.110 GiB**
- Rust `target/`: **55.835 GiB**
- `node_modules/`: **0.940 GiB**
- Python virtual environments: effectively zero
- cache-class directories: effectively zero
- build/dist/out-class directories outside the categories above: **0.061 GiB**
- remaining checkout/other data: **1.275 GiB**
- configured project scratch root: effectively empty at scan time

Largest observed Windows managed worktree: about **7.782 GiB**, of which about **7.512 GiB** was Rust `target/`.

### Combined finding

Across the two accessible managed-worktree roots:

- total measured footprint: about **157.226 GiB**
- Rust `target/`: about **149.664 GiB**
- Rust build outputs therefore account for about **95%** of the measured footprint
- ordinary checkout/other data is only a small minority of the total
- `node_modules` and virtual environments contribute, but are secondary compared with Rust `target/`

The reported >100 GB issue is therefore real, but “Git worktrees themselves are huge” is not an accurate diagnosis. The material source is **generated build/dependency state replicated inside many isolated worktrees**, dominated by Rust `target/`.

### Registry/filesystem lifecycle drift

The runtime registry count and filesystem directory count do not map 1:1 on either Runner. A bounded name comparison also surfaced filesystem entries not represented by a matching runtime Project name. On macOS, several such entries are valid Git-linked KeyQuent worktree directories; Windows also contains an extra non-Git target-like directory under the worktree root.

This is sufficient evidence that lifecycle/cleanup drift exists, but it is **not** a deletion list. Phase A did not destructively determine which entries are safe to remove, and no cleanup was attempted.

## 9. Phase A conclusions

1. The current installed `webcodex-project-driver` corresponds materially to the verified v0.3.1 source at `82fd58e...`, with platform metadata normalization/enrichment.
2. The authoritative source is the remote `82fd58e...` state, not the stale/dirty primary macOS checkout.
3. The Skill contains a strong WebCodex-specific control-plane core that should remain.
4. A smaller but important set of generic agent/software discipline should move upward into `agent-operating-model`.
5. Redundant generic teaching text should be deleted after ownership moves.
6. Composition and managed-worktree policy require rewrite rather than simple deletion.
7. The managed-worktree disk problem is primarily a generated-output amplification problem: Rust `target/` alone is ~95% of the measured 157+ GiB.
8. Managed-worktree creation should therefore be driven by concrete isolation/concurrency needs, not by “substantial work/review” as a broad default.
9. Worktree lifecycle needs explicit reuse and terminal cleanup semantics, but destructive cleanup requires a separate authorized action.
10. No Phase A evidence requires changing the frozen project scope.

## 10. Blockers

**None for Phase A completion.**

The registry/filesystem drift is a finding for later policy design and possible separately authorized cleanup, not a blocker to identifying the material source of disk usage.

## 11. Mutation audit

Phase A performed no implementation mutation to:

- `youzaizhihuiguan/webcodex-project-driver`;
- the installed ChatGPT `webcodex-project-driver` Skill;
- any final/new `agent-operating-model` Skill;
- any managed worktree contents or cleanup state.

The only intended durable mutations are Phase A evidence/state updates in the `agent-operating-model` repository on its dedicated audit branch.

## 12. Exact next action

Execute **Phase B — External Evidence Synthesis only** from this Phase A closeout.

Phase B should research the mature external approaches named in `PLAN.md`, capture failure modes and community-reported drawbacks, and make evidence-backed ADOPT / ADAPT / REJECT decisions for candidate generic operating-model patterns.

Do not create the final Skill, do not refactor `webcodex-project-driver`, and do not perform worktree cleanup in that execution.
