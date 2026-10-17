# Agent Instructions

This repository is the authoritative source for the `agent-operating-model` project and Skill.

Before stateful work:

1. Read `PLAN.md`.
2. Verify the live repository state instead of relying on chat memory.
3. Resume the recorded current phase and Exact Next Action unless the current user explicitly authorizes a different bounded objective. If the user changes the objective, reconcile it with live repository state and persist the accepted change before subsequent stateful execution.
4. Complete at most one bounded package or one phase gate per assistant execution.
5. Preserve applicable acceptance criteria and project boundaries.
6. Persist verified state, evidence, blockers, and the next exact action before ending the execution.

Authority rules:

- Repository-local instructions and authoritative project artifacts override stale chat assumptions.
- Do not silently change frozen scope, acceptance criteria, or safety boundaries.
- Do not merge to `main`, publish, release, rewrite history, or perform destructive cleanup unless the applicable project policy and authorization permit it.
- Prefer the least machinery that preserves correctness, recoverability, isolation, and reviewability.

Chat history is not the long-term project source of truth.

Authoritative project plan and current durable state: `PLAN.md`.
