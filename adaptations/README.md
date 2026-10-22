# Adapted skills

Some skills here are an upstream skill with a few deliberate edits. Each is
listed in `../adaptations.yaml` with the upstream commit it came from, and its
edits are kept as a patch in this folder, so the adaptation can be redone on
newer upstream text. `agentbot status` shows "upstream changed since <commit>"
when that is worth doing.

To bring one up to date (example: `brainstorming` from `obra/superpowers`):

1. Clone the upstream repository and note its commit: `git rev-parse HEAD`.
2. Replace `skills/brainstorming/` here with upstream's `skills/brainstorming/`.
3. Re-apply the edits from inside that folder:
   `git apply -p1 ../../adaptations/brainstorming.patch`. If a hunk no longer
   fits, upstream changed that passage: redo the edit by hand, keeping its
   intent (the entry's reason below).
4. Regenerate the patch against the new upstream text, so it stays the exact
   difference: `git diff --no-index --no-prefix <upstream copy> <this copy>`
   with the two folders named `a` and `b`. Normalize a context line containing
   only one space to an empty line so the stored patch passes whitespace checks;
   Git accepts either form. Replay the patch in a fresh upstream copy and compare
   every file to the adaptation before recording the new base.
5. In `adaptations.yaml`, set `commit` to the new upstream commit and `tree` to
   `git rev-parse HEAD:skills/brainstorming` in the upstream clone.
6. Validate: `npx skills add . --list` and `git diff --check`.

Why each adaptation exists:

- `brainstorming`: upstream's trigger is "You MUST use this before any creative
  work"; this one triggers when a request leaves the goal, the audience or the
  constraints unclear. The v7.0.1 review adopts context inspection before questions,
  context-specific questions and reasoned approach comparisons. It retains the
  existing small-change-in-chat workflow instead of upstream's mandatory design
  document, approach-selection and writing-plan gates for every change. It also
  preserves explicit opt-out and uses plain skill names. User authorization and
  repository workflow still control approvals and publication.
- `test-driven-development`: upstream deletes code written before its test,
  with no exception for code the user already had; this one adds tests around
  existing code and sets aside only a draft written in the task.
