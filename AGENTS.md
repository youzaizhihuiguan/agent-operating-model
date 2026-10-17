# Agent Instructions

This repository is the authoritative source for the `agent-operating-model` project and Skill.

Before stateful work:

1. Read `PLAN.md`.
2. Verify the live repository state instead of relying on chat memory.
3. Resume the recorded current phase and Exact Next Action unless the current user explicitly authorizes a different bounded objective. If the user changes the objective, reconcile it with live repository state and persist the accepted change before subsequent stateful execution.
4. Complete one bounded objective per assistant execution. Tightly coupled low-risk steps needed to close that same objective may be completed together; do not chain unrelated substantive phases.
5. Preserve applicable acceptance criteria and project boundaries.
6. Persist verified state, evidence, blockers, and the next exact action when they have real recovery, audit, or handoff value; do not create paperwork for its own sake.

Authority rules:

- Repository-local instructions and authoritative project artifacts override stale chat assumptions.
- Do not silently change frozen scope, acceptance criteria, or safety boundaries.
- Do not invent approval gates, independent-review requirements, PR requirements, worktrees, or other ceremony that the current user instruction, `PLAN.md`, platform safety, or a concrete material risk does not require.
- This is a single-maintainer, primarily self-use project. Once a bounded objective is authorized, ordinary non-destructive Git steps needed to complete it do not require separate confirmation unless `PLAN.md` or the platform says otherwise.
- Do not publish, release, rewrite history, perform destructive cleanup, or make other materially irreversible/external changes unless the applicable project policy and authorization permit it.
- Prefer the least machinery that preserves correctness and recoverability; add isolation or review only when they buy something concrete.
- Do not replay a completed phase for a local wording/state defect. Use targeted remediation unless new evidence invalidates a material premise.
- Treat integration as part of an authorized bounded objective when appropriate; do not invent a separate integration phase merely because a branch or PR exists.

Chat history is not the long-term project source of truth.

Authoritative project plan and current durable state: `PLAN.md`.
