# AGENTS.md

## Project

This repository is the public Skills CLI source for Pamudu’s personal skills.
It currently publishes `co-council` and should remain a small, portable skills
repository.

## Working rules

- Read the root `README.md` and `CONTRIBUTING.md` before changing a skill.
- Keep the standard `skills/<name>/SKILL.md` layout.
- Update a skill’s `SKILL.md`, surface metadata, references, and README entry
  together when its contract changes.
- Do not add credentials, private paths, generated lockfiles, or vendored
  upstream trees.
- Validate with `npx skills add . --list` and `git diff --check`.
- Do not change `agentbot/skills.sources.yaml` from this repository;
  manifest registration belongs to the consuming `agentbot` repo.

## Scope boundary

`agentbot` owns installation, global pins, Agentbot menus, and rendering.
This repository owns only the published skill content and its public metadata.
