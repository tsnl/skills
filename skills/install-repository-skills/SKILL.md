---
name: install-repository-skills
description: Install or update tsnl/skills in a repository with the npx skills CLI. Use for project-level setup or migration from the old .agents/skills submodule, not for authoring skills.
---

# Install repository skills

Install this collection with the `skills` CLI, which copies or links each skill
into every detected agent's directory (Claude Code, Codex, and others).

```sh
npx skills add tsnl/skills            # interactive: pick skills and agents
npx skills add tsnl/skills --skill simplify
npx skills add tsnl/skills --list     # preview without installing
```

Add `-g` for a user-wide install instead of the current project. Update with
`npx skills update`. Commit the installed skill directories and
`skills-lock.json` (if the CLI created one) so everyone shares one revision.

## Migrating from the old submodule layout

Earlier versions were installed as a submodule at `.agents/skills` with
`.claude/skills` symlinked to it. Inspect the consumer first: preserve local
changes and unrelated files. Then, only with the user's agreement:

1. `git submodule deinit -f .agents/skills && git rm -f .agents/skills`
2. Remove the `.claude/skills` symlink if it points at `../.agents/skills`.
3. Run `npx skills add tsnl/skills` and verify each `SKILL.md` is readable.

Report what was installed, where, and the verification performed. Do not claim
a running agent has reloaded its skill catalog without observing it.
