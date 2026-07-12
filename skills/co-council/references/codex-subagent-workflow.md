# Codex Subagent Workflow Reference

Use this reference when the active task benefits from parallel, independent Codex delegates.

## Before delegation

1. Read the parent repository's `AGENTS.md` and the relevant source files.
2. Partition the work so each delegate has a distinct question or write boundary.
3. Decide whether the current interface can enforce a requested Luna tier. If not, state that the tier is a routing request only.
4. Record the delegate name, requested tier, scope, write permission, and expected evidence in a parent-owned ledger.

## Delegate briefs

Every delegate brief should include:

- one bounded objective;
- exact paths or subsystem boundaries;
- whether the work is read-only or may edit;
- required validation or evidence;
- the required response format.

Do not paste unrelated conversation history. Do not give two delegates overlapping ownership of the same files.

## Suggested tiers

| Tier | Use for |
|---|---|
| `5.6 luna low` | File inventories, symbol search, narrow documentation lookup, simple test/log inspection. |
| `5.6 luna medium` | Focused code review, subsystem mapping, contract tracing, independent reproduction. |
| `5.6 luna high` | Implementation, multi-module reasoning, architecture analysis, ambiguous debugging. |
| `5.6 luna extra high` | Security review, migrations, system-wide decisions, hard root-cause analysis, final conflict resolution. |

## Synthesis

The parent verifies key claims against source or command output, resolves contradictions, and reports a single conclusion. Delegate summaries are evidence, not the final answer.
