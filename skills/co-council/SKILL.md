---
name: co-council
description: "Runs a bounded, Luna-only Codex subagent council for independent reconnaissance, review, debugging, or non-overlapping implementation. Enforces explicit GPT-5.6 Luna model routing when the active spawn interface supports it, keeps synthesis and integration in the parent, and stops rather than silently inheriting an unintended parent model."
---

# Co-Council

Use this skill only when at least two concrete subtasks can run independently or
when one independent reviewer would materially reduce risk.

The parent remains responsible for the user’s requirements, architectural
judgment, final plan or artifact, integration, validation, and completion report.
This is delegation-first, not delegation-only.

Read
[`references/codex-subagent-workflow.md`](references/codex-subagent-workflow.md)
before the first fan-out. Read
[`references/runtime-compatibility.md`](references/runtime-compatibility.md)
when the active Codex spawn interface is missing fields, rejects a spawn, or
starts a child without executing its task.

## Required workflow

1. Read the complete request, repository instructions, and enough primary
   evidence to identify the critical path.
2. Decide what the parent should do locally now. Do not delegate the immediate
   blocking task or routine file operations.
3. Identify independent delegate tasks, exact scopes, read/write permissions,
   expected evidence, and integration boundaries.
4. Inspect the active `spawn_agent` schema. Treat the live schema and its
   available-model list as the source of truth.
5. Enforce the capability gate below before spawning.
6. Select a Luna reasoning effort in the parent. A worker never chooses its own
   model or effort.
7. Spawn the smallest useful council with self-contained task messages.
8. Continue meaningful non-overlapping parent work while delegates run. Do not
   repeatedly poll by reflex.
9. Inspect every returned artifact, diff, test result, and material claim.
10. Resolve contradictions with primary evidence. Use at most one targeted
    follow-up or one bounded escalation.
11. Deliver one integrated result. Do not relay a bundle of summaries as the
    final answer.

## Capability gate

A model-routed council may launch only when all of the following are true:

- the active spawn interface exposes `model`;
- the active spawn interface exposes `reasoning_effort`;
- `gpt-5.6-luna` is present in the available model overrides or the active
  interface otherwise confirms that exact model slug is accepted;
- the selected effort is supported by that model;
- the delegate task can be expressed without copying the entire parent context.

When the gate passes, every spawn must explicitly set:

```text
model: gpt-5.6-luna
reasoning_effort: medium | high | xhigh | max
```

Use `fork_turns: "none"` by default for bounded workers. Use a small positive
turn count only when the delegate genuinely needs recent conversation context
and the active interface supports it. Avoid full-history forks for routed
delegates unless the current runtime explicitly supports the combination and the
extra context is necessary.

Omit `agent_type` unless a configured custom role is explicitly required.

If the gate fails, do not silently spawn agents that inherit the parent model.
Report that Luna routing cannot be enforced in the current Codex build and
continue locally. Inherited routing is allowed only after explicit user
authorization.

## Luna routing

| Lane | Exact effort | Use for |
|---|---|---|
| Scout | `medium` | Bounded codebase mapping, symbol or contract tracing, focused logs/tests, narrow documentation research, explicit checklist verification. |
| Worker | `high` | Default bounded implementation, multi-file tracing, focused debugging, subsystem analysis, and meaningful review. |
| Deep | `xhigh` | Ambiguous failures, cross-module reasoning, security/reliability review, difficult integration questions, or an independent final review. |
| Escalation | `max` | One hardest bounded question after a lower effort failed, or a highest-consequence decision where extra verification is justified. |

Do not use Luna Low. Mechanical file creation, deletion, known-file reads, and
routine commands should normally remain in the parent.

Prefer the lowest lane likely to produce dependable evidence. Escalate one
specific task; never rerun the whole council at a stronger effort.

## Council budget

Unless the user explicitly authorizes a different bounded budget:

- planning or reconnaissance: normally 2–3 delegates; hard maximum 4;
- implementation: normally 0–2 delegates; hard maximum 3 per phase;
- concurrent writers: maximum 2 with disjoint ownership;
- `xhigh` or `max` delegates: maximum 1 total per council;
- follow-up after the initial fan-out: maximum 1 targeted delegate;
- nested or recursive spawning: prohibited;
- duplicate scouts: prohibited.

“Use as many agents as needed” is not authorization for unlimited fan-out.

## Delegate brief contract

Every delegate message must include:

- one objective;
- exact repository, directory, file, or subsystem scope;
- read-only or write permission;
- relevant requirements and non-goals;
- expected evidence or files changed;
- required validation;
- a compact response format;
- an instruction not to spawn subagents.

For write tasks, assign a disjoint path boundary and require the final response
to list changed files and checks run.

## Parent ownership

The parent should normally perform directly:

- known-file reads and small searches;
- routine shell commands;
- simple file creation, deletion, or movement;
- small or tightly coupled edits;
- final plan writing;
- integration and conflict resolution;
- final diff inspection and validation.

The parent must not use subagents to avoid understanding the repository or the
requirements.
