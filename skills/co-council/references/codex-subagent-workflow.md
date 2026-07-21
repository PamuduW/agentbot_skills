# Codex Subagent Workflow Reference

Use this reference to design a small, evidence-producing Luna council.

## 1. Eligibility test

Use a council when at least one condition is true:

- two or more independent repository areas must be investigated;
- two or more non-overlapping implementation slices can proceed in parallel;
- a focused independent review can catch a material risk while the parent
  continues useful work;
- a noisy log, test, or documentation investigation benefits from isolation.

Do not use a council when:

- one agent can complete the task directly;
- the next parent action is blocked on the delegated result;
- the task is a routine file operation or known-file edit;
- implementation slices would touch the same files;
- the parent has not yet understood the requirements or current state.

## 2. Parent ledger

Before fan-out, record:

| Task name | Objective | Scope | Permission | Lane | Evidence | Integration |
|---|---|---|---|---|---|---|
| `example_task` | One bounded question | Exact paths | Read-only/write | medium/high/xhigh/max | Required output | How parent will use it |

If the parent cannot fill this row clearly, the task is not ready to delegate.

## 3. Spawn contract

Treat the active tool schema as authoritative.

Preferred routed spawn shape:

```yaml
task_name: subsystem_contracts
message: |
  Objective: Trace the contract between service A and service B.

  Scope:
  - Read only: services/a/**, services/b/**
  - Do not inspect unrelated directories.
  - Do not edit files.
  - Do not spawn subagents.

  Requirements:
  - Identify request/response schemas.
  - Cite exact files and symbols.
  - Flag mismatches and uncertain assumptions.

  Validation:
  - Confirm findings against both call sites and tests.

  Return:
  1. Findings
  2. Evidence: path + symbol
  3. Risks or uncertainty
  4. Recommended parent action
model: gpt-5.6-luna
reasoning_effort: medium
fork_turns: none
```

Use a unique lowercase `task_name` with letters, digits, and underscores.

### Context policy

Use `fork_turns: "none"` by default. The delegate brief must therefore be
self-contained.

Use a small positive turn count only when recent discussion contains essential
task-local decisions that would be inefficient or error-prone to restate.

Avoid `fork_turns: "all"` for routed delegates because it:

- increases replicated context and quota use;
- can expose unrelated requirements to the child;
- has had compatibility problems with overrides in older MultiAgentV2 builds;
- makes it easier for a worker to behave like another orchestrator instead of
  executing one task.

### Role policy

Omit `agent_type` unless a real configured role is required. Model and effort
routing are sufficient for this skill.

## 4. Routing guide

### Luna Medium

Use for bounded read-heavy work that needs interpretation:

- map one subsystem;
- locate definitions and call sites;
- inspect a focused test or log failure;
- compare implementation against an explicit checklist;
- research one narrow current-documentation question;
- verify whether a planned file set is complete.

Do not assign ambiguous architecture, broad implementation, or high-risk review.

### Luna High

Default worker lane:

- implement one bounded, disjoint slice;
- debug a focused failure with several plausible causes;
- trace behavior across a few modules;
- review a subsystem against concrete requirements;
- create or update tests for one component.

### Luna xhigh

Use for one difficult or consequential question:

- cross-module root-cause analysis;
- security or reliability review;
- architecture conflict;
- independent review of a risky implementation;
- reconciling contradictory findings.

### Luna Max

Use only when:

- a High or xhigh attempt was materially incomplete;
- the question is bounded and highest consequence;
- additional exploration and verification justify the latency and quota use.

Maximum one Max delegate. Never use Max for broad reconnaissance or mechanical
work.

## 5. Fan-out patterns

### Planning

Typical:

- one Medium scout for current-state mapping;
- one Medium or High scout for tests/contracts;
- optional one xhigh reviewer for the hardest architectural or risk question.

The parent verifies findings and writes the final plan.

### Implementation

Typical:

- parent owns the critical path;
- zero to two High workers receive disjoint write scopes;
- optional one xhigh reviewer runs after or alongside integration when it can
  catch a concrete risk.

The parent reviews every diff and runs integrated validation.

### Debugging

Typical:

- parent reproduces the problem and keeps the main hypothesis ledger;
- one Medium agent inspects logs/tests;
- one High agent traces a separate subsystem;
- one xhigh agent is used only if evidence remains contradictory.

## 6. Waiting and follow-up

After spawning:

- continue non-overlapping parent work;
- wait only when the next critical step genuinely needs a result;
- do not busy-poll;
- use at most one targeted follow-up;
- do not respawn the whole council because one answer was weak.

When an agent returns, verify key claims before accepting them.

## 7. Escalation

Escalate only the failed task:

```text
medium -> high -> xhigh -> max
```

Do not skip directly to Max unless consequence or complexity clearly warrants
it. Stop escalating when the expected value no longer justifies quota use.

## 8. Completion

The council is complete only when:

- the parent has inspected material evidence;
- delegated writes have been reviewed and integrated;
- relevant checks have run;
- contradictions are resolved or explicitly recorded;
- the final response is one synthesized result;
- the parent reports any routing or runtime limitation honestly.
