# Pamudu Agent Skills

Public personal skills for the [Vercel Skills CLI](https://skills.sh/).
This repository is intentionally a small Skills CLI source repository, not a
second bootstrap application. `agentbot` owns installation policy and
registers this repository in `skills.sources.yaml` with `skills: all`.

## Published skills

| Skill | Purpose |
|-------|---------|
| `co-council` | Run a bounded Luna-only Codex subagent council with enforced routing, parent-owned synthesis, and quota guardrails. |

Each published skill follows the Skills CLI layout:

```text
skills/<skill-name>/
├── SKILL.md
├── agents/              # optional agent-surface metadata
└── references/          # optional supporting material
```

The skill name is the directory name and the `name` in `SKILL.md`. Keep those
identities stable after publication; renaming a skill changes the install and
lockfile contract for every consumer.

## Local validation

Run the Skills CLI discovery check from the repository root:

```bash
npx skills add . --list
```

Before publishing a change, also verify the changed skill’s `SKILL.md`,
metadata, and references together. The repository must contain no credentials,
machine-specific paths, generated lockfiles, or copied upstream skill trees.

## Installation through Agentbot

The live `agentbot` manifest contains:

```yaml
- id: pamudu-agent-bootstrap-skills
  repo: PamuduW/agentbot_skills
  skills: all
```

From the sibling `agentbot` repository, install or refresh the managed
global skill set with:

```bash
./install.sh skills install
./install.sh doctor
```

The installer records managed global pins in
`~/.agents/.skill-lock.json` and refreshes the Codex and Claude-compatible
skill surfaces. A direct Skills CLI install is useful for local discovery, but
the Agentbot manifest remains the reproducible source for the normal workflow.

## Adding a skill

1. Create `skills/<name>/SKILL.md` with a stable name and focused description.
2. Add only supporting `agents/` metadata or `references/` that the skill
   actually needs.
3. Update this README’s published-skills table.
4. Run `npx skills add . --list`.
5. Review the diff for secrets, local paths, generated files, and accidental
   upstream material before committing.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the maintainer checklist and
[`AGENTS.md`](AGENTS.md) for repository-specific agent guidance.
