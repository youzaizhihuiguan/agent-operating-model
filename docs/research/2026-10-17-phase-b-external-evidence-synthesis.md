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
| [EveryInc/compound-engineering-plugin](https://github.com/EveryInc/compound-engineering-plugin) | `README.md` | blob `7a86cef92094b644de4dffab69895ba233870e8b` | Brainstorm → plan → work → simplify → review → compound loop; deliberate knowledge compounding; 36-skill surface. |
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

Observed counter-evidence:

- [#895 — plans over-specify implementation](https://github.com/obra/superpowers/issues/895): a detailed plan contained full function bodies, test files, shell commands and repeated TDD sequences; the reporter found that execution became copy/paste and adapting to a better implementation felt like violating the plan. The completed issue explicitly argues for contracts + acceptance criteria rather than implementation bodies.
- [#991 — subagent development auto-created worktrees](https://github.com/obra/superpowers/issues/991): automatic worktree creation surprised the user and left cleanup burden. The issue was completed after arguing worktrees should be opt-in or policy-driven rather than an implicit consequence of the development workflow.
- [#743 — response slowness after enabling the Skill](https://github.com/obra/superpowers/issues/743): current open report with substantial discussion about added context/latency.
- [#1900 — TDD does not map cleanly to prompt-heavy content work](https://github.com/obra/superpowers/issues/1900): deterministic executable tests can validate shape while missing content quality.

Phase B conclusion: Superpowers is strong evidence for explicit intent clarification, verification and systematic debugging; it is also strong evidence that **mandatory full-stack workflow, line-by-line planning, universal TDD and automatic worktree behavior can over-constrain capable models and users**.

### Compound Engineering

Primary workflow value:

- It explicitly tries to make future work cheaper by capturing reusable lessons.
- It treats planning, review and retained knowledge as higher leverage than raw code generation.

Observed counter-evidence:

- [#139 — ~12k idle-token report](https://github.com/EveryInc/compound-engineering-plugin/issues/139): the reporter measured the plugin dominating controllable context because many agents/skills were eagerly surfaced.
- [#156 — skill-only components loaded as agents](https://github.com/EveryInc/compound-engineering-plugin/issues/156): duplicate/eager agent surfaces wasted tokens and produced invocation confusion.
- Current issue history also contains examples of delegated-agent supervision and cross-model review behavior varying by host/model.

Phase B conclusion: "compound learning" is useful only if learning is **high-signal, scoped, and loaded on demand**. Automatically growing an always-visible skill/agent surface turns retained knowledge into context debt.

### GSD Core

Primary workflow value:

- Current GSD Core explicitly targets context rot with fresh-context workers.
- It persists `STATE.md` / `CONTEXT.md` and verifies before shipping.
- Its five-step loop is easy to state and recover.

Observed counter-evidence:

- [#857 — Capability system](https://github.com/open-gsd/gsd-core/issues/857): the project itself documents that optional behavior had become welded into 1.7k+ line workflow bodies, with multiple independent toggles and a multi-file registration tax; the proposed direction is a small core plus selectable capabilities.
- [#4649 — unconditional project-skill index reads](https://github.com/open-gsd/gsd-core/issues/4649): reading every discovered `SKILL.md` across repeated agent spawns creates an unbounded context tax; the issue proposes relevance filtering and/or explicit size budgets.
- Historical [gsd-build/get-shit-done#2086](https://github.com/gsd-build/get-shit-done/issues/2086): a codebase-mapping run reportedly became stuck for hours while consuming a large token budget.

Phase B conclusion: fresh contexts and durable state are high-value, but **fresh-context orchestration is not free**. The generic operating model should own the decision to bound work and persist state, not prescribe a heavyweight subagent topology for every task.

### Trellis

Primary workflow value:

- Trellis persists project standards, PRDs, task state and journals in the repository.
- It separates plan, implementation, verification and finish.
- It promotes selected learning back into scoped specs.

Observed counter-evidence:

- [#511 — active task silently replaced](https://github.com/mindfold-ai/Trellis/issues/511): a wrong/inherited context key plus implicit task activation could overwrite another in-progress task pointer without an explicit switch. This is evidence for fail-closed identity/authority transitions.
- [#141 — worktree incompatibility](https://github.com/mindfold-ai/Trellis/issues/141): project hook paths broke under a worktree layout, showing that generic workflow assumptions can become host/workspace bugs.
- [#530 — reversible capability ablation](https://github.com/mindfold-ai/Trellis/issues/530): explicitly argues that workflow instructions, hooks, subagents, memory and checks may become redundant or harmful as models improve, and should be removable/re-evaluated rather than accumulated forever.

Phase B conclusion: repo-local durable context is useful; hidden pointer changes are not. The operating model should require explicit identity/authority for state transitions and treat harness layers as **falsifiable, removable components**.

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

> A Skill rule is recurring inference-time debt. Keep it only when it encodes a high-value boundary, current platform semantics, or a repeated failure that the model does not reliably avoid on its own.

This is not an argument for "no prompts" or "no Skills." It is an argument for **empirical minimalism and periodic deletion tests**.

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
| Generic automatic commit/push/PR behavior | **REJECT** as default | These are authority-bearing effects and repository policy varies. Permit only when project/user authority clearly grants them. |

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

## 12. Exact next action

Execute **Phase C — Lightweight Operating Model Design only** from this Phase B closeout.

Phase C should:

1. recover Phase A and Phase B evidence from the repository;
2. define the exact `agent-operating-model` responsibility boundary and explicit non-goals;
3. derive the final invariant/guardrail set from the evidence without choosing an arbitrary rule count;
4. define positive triggers, negative triggers, and co-activation/precedence with repository-local instructions and `webcodex-project-driver`;
5. define implementation acceptance criteria for Phase D;
6. check the design for unnecessary ceremony and model-hobbling;
7. persist the Phase C design candidate and exact next action;
8. stop without building the Skill or refactoring `webcodex-project-driver`.
