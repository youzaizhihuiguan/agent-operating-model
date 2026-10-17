# Phase B — External Evidence Synthesis

Date: 2026-10-17

Status: **COMPLETE**

Scope: external evidence synthesis only. This phase did not create the `agent-operating-model` Skill, did not modify `webcodex-project-driver`, did not update any installed ChatGPT Skill, and did not start Phase C implementation/design work.

## 1. Baseline and research method

Accepted baseline at Phase B start:

- repository: `youzaizhihuiguan/agent-operating-model`
- accepted `main` HEAD: `8a345b0db42d988a42165467813b33269ed9648d`
- baseline state: `PHASE A COMPLETE — PHASE B READY`
- Phase A evidence: `docs/audit/2026-10-17-phase-a-current-state-audit.md`
- Phase B branch: `research/phase-b-external-evidence-20261017`

Research method:

1. read current primary repository documentation for the principal workflow/Skill systems named in `PLAN.md`;
2. inspect representative GitHub issues that expose real failure modes rather than relying only on project marketing;
3. inspect current community reports from Reddit, Hacker News, and LINUX DO as corroborating anecdotes, not universal truth;
4. include one current high-authority harness-maintenance source from the Claude Code team via Y Combinator;
5. separate **mechanism evidence** from **adoption decision**;
6. avoid numeric framework rankings and avoid treating star counts, testimonials, or one anecdote as proof.

The question is not "which framework wins?" It is:

> Which recurring mechanisms still earn their context, latency, autonomy, and maintenance cost with current strong models?

## 2. Primary source inventory

The following primary repositories were examined directly on 2026-10-17.

| Source | Observed primary artifact | Snapshot identity | What it contributes |
| --- | --- | --- | --- |
| [obra/superpowers](https://github.com/obra/superpowers) | `README.md` | blob `e40ddfa8f87a374e1b9a3663b80c1aa954cf8eec` | Complete mandatory software-development methodology; brainstorming, worktrees, detailed planning, TDD, subagents, review, finish/cleanup. |
| [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) | `README.md` | blob `7a86cef92094b644de4dffab69895ba233870e8b` | Brainstorm → plan → work → simplify → review → compound loop; deliberate knowledge compounding; the README snapshot lists 36 skills, but that count is snapshot metadata rather than a design-quality signal. |
| [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core) | `README.md` | blob `82770333164cd599dfa2f1ee44cc66997913cc40` | Discuss → Plan → Execute → Verify → Ship phase loop; fresh-context subagents; durable STATE/CONTEXT artifacts; current GSD home. |
| [mindfold-ai/Trellis](https://github.com/mindfold-ai/Trellis) | `README.md` | blob `caa0d13ddfdd99c69893c750cacb259844f7a7fe` | Repo-persisted specs, task PRDs, journals, scoped context injection, implementation/check subagents, learning promotion. |
| [SteveVitali/agent-skills](https://github.com/SteveVitali/agent-skills) | `README.md` | blob `d468e123bfed7a417947e1c91cab68a54b27e411` | Durable ledgers, fresh-context review, evidence-first reporting, proportional rigor with heavyweight phases individually opt-out. |
| [Codagent-AI/agent-skills](https://github.com/Codagent-AI/agent-skills) | `README.md` | blob `a47f63c2709e987e576bedaa6c00d867f7fa55c1` | Explicit quick path vs full path vs no-skill path; artifact handoff between planning/testing/PR stages. |
| [shimo4228/search-first](https://github.com/shimo4228/search-first) | `README.md` | blob `b28a1bf79114c2f103de03fdbd4986c1f69127ad` | Research-before-build with repo-first search, stop conditions, sourced findings, Adopt / Adopt-part / Build recommendation and a user skip path. |

### GSD lineage note

The earlier `gsd-build/get-shit-done` repository now explicitly redirects to `open-gsd/gsd-core`. Current design claims therefore use `open-gsd/gsd-core`. Historical issues from the archived repository are used only when they document a concrete earlier failure mode.

## 3. Representative failure evidence from project issue trackers


### Superpowers

Primary workflow value:

- It makes design, planning, verification, debugging and review explicit rather than allowing the model to jump straight to code.
- It treats "evidence over claims" as a first-class philosophy.

Historical failure evidence and current upstream remediation:

- [#895 — plans over-specify implementation](https://github.com/obra/superpowers/issues/895) is a **historical completed issue**. The failure class was a plan containing full function bodies, large test files, exact commands and repeated TDD sequences, leaving little executor judgment. Current `skills/writing-plans/SKILL.md` (blob `4cf0275d88d67aa4ef09f4bbca9970ed6ff74a00`, read 2026-10-17) now explicitly says the implementer writes function bodies unless an algorithm cannot be inferred from signatures/tests, defines a plan as the decisions the implementer cannot make alone, and includes a proportionality self-review: a plan that is several times longer than the spec or mostly code has become a transcript rather than a plan.
- [#991 — subagent development auto-created worktrees](https://github.com/obra/superpowers/issues/991) is also a **historical completed issue**. Current `skills/using-git-worktrees/SKILL.md` (blob `1381dacb3516b94382dd6ec736ede21ee8ede2b3`, read 2026-10-17) first detects whether the harness already provides isolation, forbids creating another worktree when already isolated, prefers harness-native isolation, and asks for user consent before creating a worktree when no preference is already declared.
- [#743 — response slowness after enabling the Skill](https://github.com/obra/superpowers/issues/743) remains a current report about context/latency cost and is retained only as issue evidence, not a universal measurement.

Phase B conclusion: the **failure classes remain relevant even where upstream remediated them**. Superpowers now independently reinforces two design implications already favored here: plans should preserve implementation judgment rather than transcribe code, and isolation/worktree creation should follow explicit need and policy rather than being a hidden consequence of "substantial work." This supplement does **not** claim current Superpowers still has the exact #895/#991 behavior.


### Compound Engineering

Primary workflow value:

- It explicitly tries to make future work cheaper by capturing reusable lessons.
- It treats planning, review and retained knowledge as higher leverage than raw code generation.

Historical surface-cost evidence and current snapshot correction:

- [#139 — ~12k idle-token report](https://github.com/EveryInc/compound-engineering-plugin/issues/139) measured an **older plugin snapshot** in which many agent/skill definitions were eagerly surfaced; the reported counts and token total belong to that snapshot and must not be read as current product inventory.
- [#156 — skill-only components loaded as agents](https://github.com/EveryInc/compound-engineering-plugin/issues/156) documented the related failure class: a component represented on an always-visible agent surface can consume context and create invocation ambiguity even when it should be loaded only on demand.
- The current README snapshot used in this report (blob `7a86cef92094b644de4dffab69895ba233870e8b`) lists **36 skills** and also documents a newer root-native, skills-oriented layout. The number itself is not used as evidence that the system is "too large"; inventory counts change over time.

Phase B conclusion: "compound learning" is useful only if learning is **high-signal, scoped, and loaded on demand**. The durable evidence is the failure mode—large or duplicated always-visible surfaces can create context/invocation cost—not any particular historical count such as 28 agents, 15 skills, 35 skills, or 36 skills.


### GSD Core

Primary workflow value:

- Current GSD Core explicitly targets context rot with fresh-context workers.
- It persists `STATE.md` / `CONTEXT.md` and verifies before shipping.
- Its five-step loop is easy to state and recover.

Current upstream status correction:

- [ADR-857 — Capability system](https://github.com/open-gsd/gsd-core/blob/next/docs/adr/857-capability-system.md) is **not merely a proposal anymore**. The current ADR (blob `8d5a1b04a67dd9ce34789c15968574352fc7733e`) is `Accepted`, says it was ratified on 2026-07-17, and records that the capability system was already shipped architecture in GSD 1.6.0/1.7.0. It cites the implemented capability lifecycle/registry, 30+ descriptors, twelve live Loop Extension Points, decomposition of the former `core.cts` god-module, dedicated tests and closed rollout governance.
- This shipped architecture grew from the earlier problem described in [#857](https://github.com/open-gsd/gsd-core/issues/857): optional features had become welded into very large workflow bodies and several independent toggle surfaces. The upstream response was to keep a privileged core loop while moving optional behavior behind explicit capability seams.
- [#4649 — unconditional project-skill index reads](https://github.com/open-gsd/gsd-core/issues/4649) remains useful current evidence that even a modular framework can pay an unbounded context tax if discovery eagerly reads every skill index across repeated agent spawns.

Phase B conclusion: GSD now supplies **implementation evidence**, not just proposal evidence, for a small stable core plus selectively activated capabilities. Fresh contexts and durable state remain high-value, but fresh-context orchestration and capability surfaces still carry cost; the generic operating model should therefore preserve bounded/recoverable work without mandating the full GSD topology for every task.


### Trellis

Primary workflow value:

- Trellis persists project standards, PRDs, task state and journals in the repository.
- It separates plan, implementation, verification and finish.
- It promotes selected learning back into scoped specs.

Failure evidence and current upstream implementation:

- [#511 — active task silently replaced](https://github.com/mindfold-ai/Trellis/issues/511) documents a containment failure where implicit task activation plus an incorrect/inherited context key could replace another in-progress task pointer without an explicit switch. This remains evidence for fail-closed identity/authority transitions.
- [#141 — worktree incompatibility](https://github.com/mindfold-ai/Trellis/issues/141) documents how assumptions about project-relative hooks can break in a worktree layout; it supports treating workspace topology as host-specific rather than as generic ceremony.
- [#530 — reversible capability ablation](https://github.com/mindfold-ai/Trellis/issues/530) began as a proposal to make workflow/harness layers falsifiable after model changes. That idea is now an **implemented mechanism**: current source includes `packages/cli/src/commands/ablate.ts`, the backend spec marks `trellis ablate` / `trellis restore` as Done, and the 0.6.16 migration manifest records the commands as shipped. The commands preserve a recovery transaction outside the project and refuse restoration writes when managed paths changed while ablated.

Phase B conclusion: repo-local durable context is useful; hidden pointer changes are not. Trellis now provides concrete implementation evidence that a harness can make its own influence reversible and testable, strengthening—not changing—the decision that standing workflow layers should remain falsifiable/removable after model or harness changes.

### SteveVitali agent-skills

Primary workflow value:

- Explicit "evidence over claims" acceptance reporting.
- Durable on-disk run ledgers for long horizons.
- Fresh-context review.
- Most importantly, **proportional rigor**: heavyweight phases are individually opt-out so a tiny change does not pay a large workflow tax.

Phase B conclusion: this is one of the clearest positive examples for the target architecture: preserve rigorous mechanisms while making them conditional on task/risk.

### Codagent Agent Skills

Primary workflow value:

- It has a documented **quick path** for small bounded changes.
- It has a **full path** for cross-cutting or unclear work.
- It explicitly says one-off questions, debugging conversations and quick manual edits may need **no skill at all**.

Observed counter-evidence:

- [#37 — end-to-end workflow confusion](https://github.com/Codagent-AI/agent-skills/issues/37): users can lose the mental model when many phase commands accumulate.
- [#53 — finalize loop cannot converge](https://github.com/Codagent-AI/agent-skills/issues/53): independently reasonable CI/review rules can compose into a loop that never reaches a valid terminal state.
- [#59 — acceptance dismissed retained failure evidence](https://github.com/Codagent-AI/agent-skills/issues/59): a required acceptance test failed, but the acceptance pass attributed it elsewhere without reading retained evidence.
- [#61 — scratch folders leaked](https://github.com/Codagent-AI/agent-skills/issues/61): scratch ownership was underspecified and temporary folders accumulated.

Phase B conclusion: proportional paths are useful; pipelines need explicit terminal semantics, evidence provenance and resource ownership or their internal rules can contradict each other.

### search-first

Primary workflow value:

- states the research question before searching;
- checks the current repository first;
- chooses sources by question type;
- has explicit stop conditions;
- records what was searched and what remains unknown;
- produces a sourced recommendation;
- supports a lightweight Quick mode and an explicit user "skip research" path.

Phase B conclusion: adopt the **principle**, but not as ceremony for every edit. Research should trigger when a mature option plausibly exists and the choice matters; it should stop when additional search is unlikely to change the next decision.

## 4. High-authority current harness evidence

A current Y Combinator interview with Boris Cherny from the Claude Code team is unusually relevant:

- [Boris Cherny: We Cut 80% of Claude Code's Prompt](https://www.ycombinator.com/library/UN-boris-cherny-we-cut-80-of-claude-code-s-prompt)
- reported practice: with model releases, Claude Code deletes/reworks substantial prompt and tool scaffolding, then uses ablation to add back only what demonstrably helps;
- he explicitly describes older instructions as often compensating for behaviors a newer model already handles;
- he also notes that some product instructions remain useful for safety, permissions and desired user-facing behavior.

This supports a central project principle:

> A standing Skill surface has recurring cost in layers: metadata participates in discovery/routing, while the full Skill body and supporting resources consume context when activated. Keep the trigger surface and activated rules compact, and retain detailed standing guidance only when it encodes a high-value boundary, current platform semantics, or a repeated failure that the model does not reliably avoid on its own.

This is not an argument for "no prompts" or "no Skills." It is an argument for **empirical minimalism, progressive loading, and periodic deletion/ablation tests**.

## 5. Community feedback — corroborating, not normative

These sources are anecdotes and community discussion. They are used to identify recurring pain points, not to establish universal policy.

### Reddit

- [Anyone else not a fan of the superpowers plugin?](https://www.reddit.com/r/ClaudeCode/comments/1rmi6pr/anyone_else_not_a_fan_of_the_superpowers_plugin/) reports over/under-engineering, too many obvious questions, workflow takeover, unsolicited commits and frequent worktrees.
- [What's your actual Claude Code workflow by task size?](https://www.reddit.com/r/ClaudeCode/comments/1wmax2u/whats_your_actual_claude_code_workflow_by_task/) is organized around exactly the proportional-rigor question: direct prompting vs planning vs full spec/task workflows; the initiating complaint is that a relatively small feature can be stretched into days of repeated reviews.
- [GSD usage](https://www.reddit.com/r/ClaudeCode/comments/1qh24np/gsd_get_shit_done_usage/) provides positive counterweight: users value requirement discussion, planning, context retention, verification and explicit phase confirmation.

### Hacker News

- [Task-specific agents vs role-specific agents](https://news.ycombinator.com/item?id=45230751) reports better results from agents assigned one concrete task/template than from broad job-role personas.
- [Ask HN: Are you using Spec Driven Development?](https://news.ycombinator.com/item?id=48510002) captures the useful-but-not-sufficient nature of specs: smaller specs and review pauses improve control, but code quality/style can still drift despite instructions.
- [Components of a Coding Agent](https://news.ycombinator.com/item?id=47638810) includes discussion supporting constrained context rather than accumulating full conversational history.

### LINUX DO

- [Superpowers + GSD + Gstack guide](https://linux.do/t/topic/1954517) explicitly reports that combining full frameworks becomes too heavy for small changes and costly in tokens, and describes manually disabling unused Skills to keep context cleaner.
- [Superpowers workflow discussion](https://linux.do/t/topic/1485031) highlights the positive side: brainstorming, staged planning, bounded execution and verification can pull agents away from "code first, think later."
- [Workflow optimization discussion](https://linux.do/t/topic/2058994) expresses the countervailing concern that adding GSD + Superpowers automation can reduce operator control or quality if too much is automated.

Cross-community signal: users generally value **planning when needed, durable context, explicit verification and recoverability**, but repeatedly complain when frameworks become an always-on operating system.

## 6. Recurring failure-mode synthesis

Phase B found the following recurring categories across otherwise different systems.

### F1. Reinvention / stale solution selection

Failure:
- custom code is written before checking repository capabilities, packages, official APIs, existing Skills/tools or maintained external solutions.

Useful mechanism:
- repo-first, decision-oriented search with stop conditions and a final adopt/adapt/build decision.

Risk if over-applied:
- every small edit turns into a research project.

### F2. Context rot / long-horizon drift

Failure:
- long sessions mix completed and incomplete work, forget prior decisions, or lose exact next action.

Useful mechanism:
- durable state + bounded units + fresh context when the horizon actually warrants it.

Risk if over-applied:
- unnecessary subagent/session churn for work that easily fits one context.

### F3. Acceptance drift / unsupported completion claims

Failure:
- "done" is asserted without mapping requirements to current evidence; retained failure evidence may be ignored.

Useful mechanism:
- explicit acceptance criteria, exact evidence provenance, re-observation before claiming PASS.

Risk if over-applied:
- heavyweight validation for trivial low-risk work.

### F4. Plan over-specification

Failure:
- planning dictates function bodies, exact test code, commands and micro-steps, leaving the executor unable to use new information or stronger model judgment.

Useful mechanism:
- plan at the **intent / contract / constraint / acceptance** level.

Risk:
- too little detail on truly high-risk or unfamiliar work.

### F5. Workflow takeover / hidden topology mutation

Failure:
- the framework silently creates worktrees, switches active tasks, commits/pushes, or changes the execution surface.

Useful mechanism:
- explicit authority and visible transitions for material workspace/Git/external state changes.

### F6. Always-on context tax

Failure:
- every Skill/agent/policy is eagerly surfaced on every turn or spawn.

Useful mechanism:
- narrow trigger descriptions, progressive loading, relevance filtering, context budgets.

### F7. Orchestration non-convergence

Failure:
- individual rules are reasonable but their composition creates loops, deadlocks, duplicate work or supervisors that stop supervising.

Useful mechanism:
- smallest topology, explicit terminal conditions, bounded retries, exact ownership and reconciliation.

### F8. Role proliferation

Failure:
- artificial PM/architect/reviewer personas multiply context and handoffs without buying independent evidence or true parallelism.

Useful mechanism:
- add a separate agent only for a concrete separable task, independent review boundary, or real concurrency need.

### F9. Harness obsolescence

Failure:
- instructions introduced for older model weaknesses remain forever and begin hobbling newer models.

Useful mechanism:
- periodic ablation and re-validation after material model/harness changes.

### F10. Knowledge accumulation becomes knowledge debt

Failure:
- every lesson is promoted permanently, making context bigger and more contradictory.

Useful mechanism:
- promote only stable, verified, reusable lessons to the narrowest authoritative surface; keep event/run history separate from standing instructions.

### F11. Human comprehension gap

Failure:
- agent artifacts can be technically complete but expensive for a human to review, encouraging rubber-stamping.

Useful mechanism:
- concise decision/verification summary pointing to full evidence rather than duplicating it.

## 7. ADOPT / ADAPT / REJECT decision matrix

| Candidate pattern | Decision | Phase B rationale |
| --- | --- | --- |
| Repository/project authority before chat memory | **ADOPT** | Repeatedly supported by GSD/Trellis/SteveVitali and already validated in this project. Stable authority belongs outside transient model context. |
| Durable exact next action / recoverable handoff for multi-step work | **ADOPT** | High leverage against interruption and context rot; does not require a second task system. |
| Evidence over completion claims | **ADOPT** | Cross-framework agreement; failure cases show false PASS is materially worse than extra verification. |
| Proportional rigor / quick vs full vs no-skill paths | **ADOPT** | Explicitly supported by Codagent and SteveVitali; directly addresses community complaints about process tax. |
| Periodic prompt/Skill/harness ablation after major model changes | **ADOPT** | Strong current evidence from Claude Code practice plus Trellis' implemented ablation direction. |
| Explicit authority for material workspace/Git/external mutations | **ADOPT** | Worktree/task-pointer incidents show hidden transitions are a recurring class of failure. |
| Search before building | **ADAPT** | Use for non-trivial choices where a mature solution plausibly exists; repo-first, source-aware, bounded, skippable. Do not make it a mandatory preamble to trivial edits. |
| Up-front planning/specification | **ADAPT** | Valuable when ambiguity, scope or risk justify it. Prefer intent, contracts, constraints and acceptance criteria over complete implementation recipes. |
| Fresh-context subagents | **ADAPT** | Useful for long horizons, independent review, or separable research. Not the default for work that fits one context. |
| Independent review | **ADAPT** | Use at material gates or where independence materially reduces risk. Do not review every tiny edit with another agent. |
| Durable run ledger | **ADAPT** | Required when recovery/handoff matters; unnecessary for trivial single-turn work. |
| Learning/knowledge compounding | **ADAPT** | Promote only verified, stable, reusable lessons to scoped authoritative surfaces; never automatically grow always-on prompt context. |
| TDD | **ADAPT** as a domain technique; **REJECT** as universal agent law | Excellent where executable behavior is the right oracle; inappropriate as a mandatory shape for every task, especially content/prompt/design work. |
| Multi-agent execution | **ADAPT** to task topology | Use separate agents for separable tasks, real concurrency, or independent evidence; avoid role theater. |
| Human-facing concise closeout/decision summary | **ADAPT** | Keep full evidence durable, but surface a compact review layer at meaningful gates. |
| Automatic managed worktree for "substantial" or review work | **REJECT** | Phase A local evidence plus Superpowers/Trellis failures show cost and surprise; require concrete isolation/concurrency value. |
| Mandatory full brainstorm → spec → plan → TDD → multi-agent review → finish pipeline | **REJECT** | Useful as an optional high-rigor path, but incompatible with the project's lightweight target and current-model ablation evidence. |
| Line-by-line implementation plans as the default | **REJECT** | Over-specifies executor behavior and duplicates implementation reasoning. |
| Role-based agent swarms by default | **REJECT** | More handoffs/context without guaranteed evidence gain; prefer task-specific workers. |
| Always-on loading of all Skills/agents/rules | **REJECT** | Demonstrated context/token cost; violates progressive loading. |
| A new generic task database / second control plane | **REJECT** | Durable repository state and WebCodex already exist; another system would duplicate authority. |
| Generic automatic commit/push/PR behavior | **REJECT** as a project default | **External evidence supports the narrower failure class:** hidden or surprising material mutations can violate operator expectations and complicate recovery. **Project policy supplies the stronger authority rule:** in this project, commit/push/PR and similar Git/external effects are treated as authority-bearing actions whose defaults depend on explicit repository/user policy. This row is a project decision informed by evidence, not a claim that every external framework proves one universal Git policy. |

## 8. Design inputs carried into Phase C

Phase C should derive the final operating model from these evidence-backed families, not copy any external workflow verbatim.

### Strong candidate responsibility families

1. **Authority and objective**
   - repository/project truth before chat assumptions;
   - objective ambiguity is a decision boundary, not permission to invent work;
   - preserve explicit acceptance/scope contracts.

2. **Solution selection**
   - search/reuse before non-trivial custom construction;
   - choose Adopt → Extend/Compose → Build only when the choice is decision-relevant.

3. **Proportional execution**
   - one bounded unit / phase gate;
   - smallest topology that preserves correctness;
   - fresh context and subagents only when they buy real isolation, independence or context reset.

4. **Verification**
   - evidence proportional to risk;
   - diagnose the failing layer before broad remediation;
   - completion claims require current evidence.

5. **State and recovery**
   - persist exact next action and essential evidence when interruption/handoff matters;
   - reconcile uncertain outcomes before retrying.

6. **Mutation safety**
   - material destructive/irreversible/external/Git/workspace transitions need clear authority;
   - do not silently create workflow artifacts/topology.

7. **Context economy**
   - progressive loading;
   - no always-on framework dump;
   - every standing rule must earn repeated inference cost.

8. **Harness evolution**
   - treat instructions as removable hypotheses;
   - re-evaluate or ablate after material model/platform changes.

These are **candidate families, not the final invariant wording or count**. Phase C owns consolidation and trigger/composition design.

## 9. What Phase B explicitly rejects carrying forward

Phase C should not begin from assumptions that:

- every coding task needs a spec;
- every implementation needs TDD;
- every substantial task needs a worktree;
- every plan needs subagents;
- every change needs independent review;
- every useful observation should become a permanent rule;
- a richer workflow is automatically a safer workflow;
- more detailed instructions are automatically more reliable;
- one framework's command taxonomy should become the generic Skill;
- community popularity is validation.

## 10. Research limitations and confidence boundaries

- GitHub issue reports are concrete failure evidence but may describe versions later fixed. They support the **failure class**, not a claim that the current release still has the bug.
- Reddit, Hacker News and LINUX DO are anecdotal and potentially selection-biased. They are corroborating sources only.
- SteveVitali/agent-skills and search-first have much smaller public adoption surfaces than Superpowers/GSD/Trellis; their mechanisms are evaluated on design coherence and source transparency, not popularity.
- No benchmark or star-count claim was used as decisive evidence.
- The research does not attempt to prove a universal optimal workflow. The target is a lightweight set of boundaries robust enough to prevent recurring failures while leaving current strong models room to reason.

## 11. Phase B exit-criteria check

- Principal mature approaches examined sufficiently for comparison: **PASS**.
- Recurring failure categories and community-reported drawbacks captured: **PASS**.
- Important patterns have explicit ADOPT / ADAPT / REJECT decisions: **PASS**.
- Material design claims have recoverable sources: **PASS**.
- Known mandatory research gap that would materially change Phase C: **NONE**.
- No final Skill created: **PASS**.
- No `webcodex-project-driver` mutation: **PASS**.

## 12. Targeted Supplement — First-party and Upstream-status Corrections

Date: 2026-10-17

Purpose: raise the authority and freshness of Phase B evidence before integration. This supplement does not reopen the framework survey and does not start Phase C.

### 12.1 OpenAI Skill / Skill Creator evidence

Current first-party guidance materially strengthens the lightweight-Skill conclusion.

**Current installed Skill Creator guidance (read 2026-10-17):**

- Skill instructions should be as lightweight as possible while remaining precise.
- Skills share context with the system prompt, conversation and other active Skills; general concepts and obvious guidance should not be restated.
- The description is the primary trigger contract and should say both what the Skill does and when it should activate.
- Progressive loading is explicit: metadata is available for discovery; the main `SKILL.md` body loads after activation; supporting references/scripts/assets are consulted only when relevant.
- The guidance calls `SKILL.md` a **control plane, not a knowledge dump**, targets roughly less than 500 lines, and says supporting files should exist only when they materially improve reliability, clarity or output quality.
- Scripts are appropriate for deterministic/fragile/repeatable operations; references are appropriate for detailed material that need not occupy the core body.

**Public OpenAI corroboration:**

- [OpenAI API — Skills](https://developers.openai.com/api/docs/guides/tools-skills) says discovery exposes a Skill's name and description first; the model decides whether to invoke from that metadata and reads the full `SKILL.md` only when invoked.
- [OpenAI Plugins — Skills](https://developers.openai.com/plugins/concepts/skills) likewise defines description as the condition for considering the Skill and distinguishes the Skill workflow layer from live data/auth/action surfaces.
- [OpenAI developer article — Using skills to accelerate OSS maintenance](https://developers.openai.com/blog/skills-agents-sdk) explicitly describes `description` as part of the routing contract and the format as progressive disclosure.

**Precision correction on "inference-time debt":** a Skill does not impose its full body on every turn. The recurring cost has two layers: metadata/routing surface is available for discovery, while the larger body/resources consume context only when the Skill activates. The design implication is still to keep the global trigger surface and activated core compact and to defer details progressively.

**Design implication for Phase C:** strong first-party support now exists for `agent-operating-model` being a small guardrail/control layer containing specific, non-obvious, recurring failure prevention—not a general software-engineering textbook or an always-loaded end-to-end workflow framework.

### 12.2 OpenAI Codex repository instructions / AGENTS.md

Current first-party Codex source provides concrete repository-scope semantics:

- [`openai/codex: codex-rs/core/src/agents_md.rs`](https://github.com/openai/codex/blob/main/codex-rs/core/src/agents_md.rs), blob `8e0c475d90339a236f51da33f85e70b967a61f26`, discovers project instructions from the project root down to the current working directory and does not walk above the project root. It also enforces a project-document byte budget.
- [Codex base instructions](https://github.com/openai/codex/blob/main/codex-rs/protocol/src/prompts/base_instructions/default.md), blob `907ff8b877026871b088f01f4366cea36e1f02cd`, state that an `AGENTS.md` applies to the directory tree rooted where it lives; deeper nested files win when applicable instructions conflict; direct system/developer/user instructions outrank `AGENTS.md`.

This is stronger evidence for **scoped repository authority**, but it also prevents Phase C from inventing a product-agnostic universal precedence ladder. The exact precedence between Skill, repository instructions and other surfaces is host semantics: for example, OpenAI Skills are user-prompt input while Codex injects repository instructions through its own harness path.

**Design implication:** repository-/path-specific truth belongs at repository/path scope. The generic Skill should require discovery and respect of applicable local authority, while platform-specific Skills/runtimes own the exact mechanics and precedence.

### 12.3 OpenAI long-running work / ExecPlan evidence

Two current first-party repository sources show planning as a **conditional long-horizon mechanism**, not an all-task prerequisite:

- [OpenAI Cookbook — Using PLANS.md for multi-hour problem solving](https://github.com/openai/openai-cookbook/blob/main/articles/codex_exec_plans.md), blob `d9c8a75a8df6e78d59fd3d784727872272b4e19c`, introduces ExecPlans for complex features/significant refactors and multi-hour work.
- [`openai/openai-agents-python/PLANS.md`](https://github.com/openai/openai-agents-python/blob/main/PLANS.md), blob `21c840ac2fd278e26c37f6b850ac6eb7dec28f82`, requires an ExecPlan for multi-step/multi-file/new-feature/refactor/>~1 hour work but explicitly makes it optional for trivial fixes.

When used, the plan is a living recovery artifact: current progress/stopping points, discoveries with evidence, decisions/rationale, outcomes, context/orientation, concrete steps, explicit acceptance, and idempotence/recovery must stay current.

**Design implication:** this strengthens `ADAPT` for planning and `ADAPT` for durable run state. It does not justify a mandatory planner for trivial tasks.

### 12.4 GitHub Copilot scoped repository/custom instructions

Current GitHub documentation independently validates the same scope split:

- [Adding repository custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide) distinguishes repository-wide `.github/copilot-instructions.md`, path-specific `.github/instructions/*.instructions.md`, and `AGENTS.md`; the nearest applicable `AGENTS.md` wins in the directory tree on supported surfaces.
- [Copilot customization cheat sheet](https://docs.github.com/en/copilot/reference/customization-cheat-sheet) describes custom instructions as **always-on only within their defined scope**, while Agent Skills are loaded when relevant to a task.
- [Copilot CLI custom instructions](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions) discovers instruction files across repository/CWD/file paths and includes path-specific instructions only when `applyTo` matches. Importantly, Copilot CLI says it **does not define a general precedence order** among multiple applicable instruction-file types and recommends avoiding conflicts.

**Design implication:** local rules should live at the narrowest authoritative repository/path scope that owns them. Phase C should define composition principles without pretending all hosts implement the same precedence algorithm.

### 12.5 Anthropic current harness / context / permissions evidence

Current Anthropic first-party material points in the same direction:

- [Give Claude context: CLAUDE.md and better prompts](https://support.claude.com/en/articles/14553240-give-claude-context-claude-md-and-better-prompts) explicitly calls `CLAUDE.md` project memory, distinguishes global / repository-root / on-demand subdirectory scopes, recommends keeping the root file short and signal-dense, omitting obvious/full-reference/history material, deleting stale instructions, using `/clear` between unrelated tasks, and relying on project memory rather than carrying chat history indefinitely.
- [Steering Claude Code: when to use CLAUDE.md, skills, hooks, and subagents](https://claude.com/resources/articles/steering-claude-code-skills-hooks-rules-subagents-and-more) explicitly compares instruction mechanisms by load time, compaction behavior, context cost and authority. Root `CLAUDE.md` is session-wide context; subdirectory instructions and Skills can load on demand. The article warns that unowned root instructions can accumulate cost and recommends moving scoped conventions/procedures to narrower surfaces.
- [Beyond permission prompts](https://claude.com/resources/articles/beyond-permission-prompts-making-claude-code-more-secure-and-autonomous) describes Claude Code's permission model and sandboxing as a way to define boundaries within which more autonomous execution can happen safely. This supports separating **authority boundaries** from workflow methodology.
- [Boris Cherny: We Cut 80% of Claude Code's Prompt](https://www.ycombinator.com/library/UN-boris-cherny-we-cut-80-of-claude-code-s-prompt) is retained with a narrower reading: Cherny says Claude Code changes/deletes prompts and tools on model upgrades because instructions compensating for one model may not transfer, and that "simple mode" is used as an ablation. He also explicitly says some prompts remain valuable for product behavior and user experience.
- Anthropic's later official article [The new rules of context engineering for Claude 5 generation models](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) directly frames the >80% prompt reduction as a context-engineering lesson rather than a claim that prompts are inherently harmful.

**Design implication:** prompt/Skill/harness layers should remain falsifiable and periodically revalidated, but safety, permission, product-behavior and genuinely non-obvious workflow instructions can remain load-bearing.

### 12.6 OpenAI Agents SDK multi-agent evidence

Current first-party OpenAI Agents SDK guidance supports **conditional topology**:

- [`openai/openai-agents-python/docs/multi_agent.md`](https://github.com/openai/openai-agents-python/blob/main/docs/multi_agent.md), blob `7ee2b700a2514eb94cc0a7c67248c9ac2639df04`, distinguishes manager-style "agents as tools" from handoffs by who should own the user-facing result and when a specialist should take over.
- The same guide describes parallel agents as useful for speed when there are **multiple tasks that do not depend on each other**.
- It also presents code orchestration and LLM orchestration as alternatives with trade-offs rather than a mandatory hierarchy.

**Design implication:** this materially strengthens—but does not reclassify—the existing `ADAPT` decision for multi-agent execution. Separate agents should buy bounded specialization, ownership separation, independence, or true parallelism.

### 12.7 Evidence versus project-policy boundary

Phase B evidence supports failure classes and mechanisms; project policy remains a separate normative layer.

Example:

- **External evidence supports:** hidden or surprising material mutations (worktree creation, active-task switching, automatic external effects) can violate operator expectations and make recovery harder.
- **This project's policy decides:** commit, push, PR, protected-branch promotion and similar effects are authority-bearing actions whose defaults depend on explicit repository/user authorization.

The same distinction applies throughout the decision matrix. An `ADOPT / ADAPT / REJECT` label is an **Agent Operating Model project decision informed by cited evidence**, not a claim that every external project or platform endorses the identical rule.

### 12.8 Upstream-status correction summary

| Project | Earlier evidence | Current upstream state checked 2026-10-17 | Design effect |
| --- | --- | --- | --- |
| Superpowers | #895 plan over-specification; #991 automatic worktrees | Both issues completed. Current writing-plans adds proportionality/executor-judgment rules; current worktree skill detects existing isolation, prefers native isolation and asks for consent absent a declared preference. | Keep historical failure classes, stop presenting the exact behavior as current; classification unchanged. |
| GSD Core | #857 described capability-system direction | ADR-857 is Accepted/ratified and records the capability system as shipped architecture of 1.6.0/1.7.0. | Evidence strengthened from proposed design to implemented architecture; classification unchanged. |
| Trellis | #530 proposed reversible ablation | `trellis ablate` / `trellis restore` are implemented, specified, tested and marked Done; 0.6.16 records the feature as shipped. | Evidence strengthened from proposal to implemented falsifiability mechanism; classification unchanged. |
| Compound Engineering | #139/#156 surfaced context/invocation costs with historical agent/skill counts | Current README snapshot lists 36 skills and documents a newer root-native skills-oriented layout. | Historical counts are snapshot metadata only; retain the general context-surface failure class, not the numbers as a quality argument. |

### 12.9 Targeted-supplement verdict

1. **Do the existing Phase B core conclusions still stand?** **YES.** The supplement increases authority/freshness and corrects upstream status; it does not overturn the core synthesis.
2. **Does any ADOPT / ADAPT / REJECT classification change?** **NO.** No candidate pattern changes category.
3. **Which conclusions gained materially stronger evidence?**
   - lightweight/progressively-loaded Skill design and context economy;
   - repository/path-scoped authority rather than global rule dumping;
   - planning/durable state as a conditional long-horizon mechanism;
   - periodic harness/prompt ablation after model changes;
   - multi-agent execution as conditional topology tied to ownership/independence/dependency structure;
   - explicit authority boundaries as distinct from workflow methodology.
4. **Which earlier descriptions required correction?**
   - Superpowers #895/#991 are historical failure evidence with current upstream remediation;
   - GSD ADR-857 is Accepted/ratified/shipped, not merely a proposed direction;
   - Trellis ablation is implemented, not merely proposed;
   - Compound Engineering inventory numbers are historical/current snapshot metadata, not the core design argument.
5. **Research gap that could materially change Phase C:** **NONE**.

Therefore Phase B remains:

`PHASE B COMPLETE — PHASE C READY`

but the next repository action is integration of the completed Phase B branch before Phase C begins.

## 13. Integration handoff

Phase B integration is gated through PR #2 from `research/phase-b-external-evidence-20261017` to `main`. This section records the handoff that led to that PR; it is not a standing instruction to recreate or re-run the integration after acceptance.

Once PR #2 is accepted, the authoritative next action is the one recorded in `PLAN.md`: **Phase C — Lightweight Operating Model Design only**.
