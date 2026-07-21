# Runtime Compatibility and Failure Handling

Codex subagent behavior varies by release and feature configuration. This skill
must adapt to the active runtime rather than assuming that documentation,
another machine, or a previous session matches the current tool schema.

## Capability probe

Before the first fan-out, inspect the active `spawn_agent` schema.

Required for enforced Luna routing:

- `model`;
- `reasoning_effort`;
- an accepted `gpt-5.6-luna` override;
- the selected supported effort.

Useful optional fields:

- `fork_turns`;
- `task_name`;
- lifecycle tools such as wait, list, follow-up, interrupt, or close.

The current source can expose model and reasoning overrides through
MultiAgentV2 configuration, but older stable builds, feature rollouts, and
account-specific model availability can differ.

## Safe fork policy

For maximum compatibility, use:

```text
fork_turns: "none"
```

with a self-contained delegate message.

A small positive integer is acceptable when the active schema supports it and
recent task context is essential.

Avoid a full-history fork for routed workers. Older MultiAgentV2 builds have
rejected role/model/reasoning overrides with full-history forks. Newer source
may permit more combinations, but context-free or partial forks remain cheaper
and less ambiguous for bounded work.

## When routing fields are absent

Do not continue silently.

Report:

> Co-Council cannot enforce `gpt-5.6-luna` routing in this Codex build because
> the active spawn interface does not expose the required model and reasoning
> overrides. I will continue locally unless you explicitly authorize inherited
> parent-model subagents.

Do not claim that a natural-language request such as “Luna High” changed the
child model.

## When Luna is unavailable

If `gpt-5.6-luna` is absent from the live model override list or the spawn call
rejects it:

1. stop additional fan-out;
2. report the exact limitation;
3. continue locally;
4. use another model only with explicit user authorization.

## When the child starts but does not execute

Some MultiAgentV2 builds have produced a child that acknowledges context but
does not perform the initial task.

If the child returns no requested evidence and appears idle or immediately
complete:

1. do not respawn the council;
2. send one explicit follow-up task through the active follow-up/message
   mechanism, repeating the bounded task;
3. if it still does not execute, mark the runtime incompatible and continue
   locally.

## When a spawn rejects overrides

If the error mentions full-history inheritance or incompatible overrides:

1. retry that one spawn with `fork_turns: "none"`;
2. omit `agent_type`;
3. keep the explicit Luna model and effort;
4. if the retry fails, stop routed fan-out and report the limitation.

## When metadata is hidden

A successful tool call proves that the runtime accepted the requested fields,
not necessarily that the UI exposes the child model.

Do not claim backend verification unless the active UI, logs, or returned
metadata confirms it. Report “explicit routing accepted by the spawn
interface” when that is the strongest evidence available.

## Version checks

Record:

```bash
codex --version
npm view @openai/codex dist-tags
```

Use the latest stable build for normal work. Use an alpha build only when the
required feature is unavailable on stable and the user accepts prerelease risk.

A skill update cannot add model-routing fields to an older Codex binary. The
skill can only probe, use, or reject the capabilities the runtime exposes.
