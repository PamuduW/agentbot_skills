# Contributing skills

This repository publishes small, reusable Agent Skills. Keep contributions
portable across Codex, Claude Code, Cursor, and GitHub Copilot.

## Skill contract

- Put each skill at `skills/<name>/SKILL.md`.
- Use the same stable `<name>` in the directory and front matter.
- Keep `SKILL.md` focused on when the skill applies, the workflow it requires,
  and the verification or safety boundaries that matter.
- Put detailed material in `references/` only when it is needed by the skill.
- Add `agents/openai.yaml` or other surface metadata only when it changes the
  presentation or routing for that surface.

## Verification checklist

```bash
npx skills add . --list
git diff --check
```

Before opening a change, confirm that:

- the Skills CLI discovers the intended skill names;
- no secret, token, private path, or machine-local configuration is present;
- references are linked from the skill and are not dead files;
- the README published-skills table matches the repository tree;
- the change does not vendor an upstream repository or generated install tree.

The repository does not auto-install or auto-publish skills. Installation and
global lock updates happen through the consuming `agentbot` repository.
