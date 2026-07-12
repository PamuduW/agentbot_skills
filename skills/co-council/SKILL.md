---
name: co-council
description: "Explores a codebase or problem area through focused Codex subagents, assigns each delegate 5.6 Luna Low, Medium, High, or Extra High according to task complexity and risk, then synthesizes verified findings. Use for multi-area codebase review, reconnaissance before planning, architecture investigation, parallel research, or bounded implementation work that benefits from independent delegates."
---

# Co-Council

1. Scope the question before delegating. Identify independent partitions, the required output, write ownership, and the evidence each delegate must return.
2. Check repo-local instructions and the current subagent interface before fan-out. Respect its concurrency limit and whether it supports an explicit model-selection argument.
3. Choose a model **in the parent** for every delegate using [`references/codex-subagent-workflow.md`](references/codex-subagent-workflow.md). Do not ask a worker to select its own model.
4. Delegate only independent work in parallel. Use `spawn_agent` for Codex delegates, give each a unique task name, and provide only the task-local context it needs.
5. Make some delegates on-brief and, when useful, add one off-angle delegate for failure modes, cross-cutting impact, or alternative explanations. Do not create duplicate scouts.
6. Keep exploration, review, and judgment delegates read-only. Assign a writer a non-overlapping ownership boundary; serialize overlapping edits.
7. Inspect returned artifacts and relevant repository state yourself. Reconcile contradictions with primary evidence. Use one targeted follow-up delegate only if a material uncertainty remains.
8. Deliver the requested result or planning input; do not merely relay a bundle of subagent summaries.

## Model-routing contract

Select `5.6 luna low` for mechanical, tightly bounded work: file inventories, symbol searches, narrow documentation lookup, and simple test or log inspection.

Select `5.6 luna medium` for bounded work that still needs interpretation: codebase mapping, focused code review against explicit criteria, independent reproduction, and contract tracing within one subsystem.

Select `5.6 luna high` for work needing multi-step reasoning or meaningful judgment: architecture analysis, cross-module tracing, implementation, debugging ambiguous failures, and non-critical reliability review.

Select `5.6 luna extra high` for the highest-consequence or hardest work: security review, complex migrations, system-wide design decisions, difficult root-cause analysis, and final conflict resolution where wrong conclusions would be expensive.

Prefer the lowest available tier that can produce dependable evidence. Escalate Low to Medium, Medium to High, or High to Extra High when a result is incomplete, contradictory, or needs deeper reasoning; do not re-run the entire council.

The requested Luna tier is a routing decision, not a claim that every Codex runtime can enforce it. If the active `spawn_agent` interface exposes a model field or supported routing mechanism, set it explicitly. If it does not, state that model enforcement is unavailable in the current harness and continue only if inherited routing is acceptable; never pretend the requested tier was applied.

## Fan-out rules

- Use two to four delegates for distinct concerns; add more only for genuinely independent partitions.
- Keep a parent-owned ledger of each delegate's scope, requested tier, write permission, and expected evidence.
- For broad work, start with Low scouts where the questions are mechanical, use Medium for interpreted findings, High for integration or high-risk areas, and reserve Extra High for the hardest bounded question or a single disagreement resolver.
- Avoid concurrent writers that touch the same files. Give each writer an explicit path boundary and verify its diff before synthesis.
- Stop when the evidence answers the user’s question. A council is a means to an outcome, not a mandatory ceremony.
