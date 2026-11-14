---
name: bootstrap-skills
description: Bootstrap an agent skill setup with the npx skills CLI. Installs tsnl/skills and its companion third-party skills in a project or user-wide, and migrates the old .agents/skills submodule layout. Use for setup, update, or repair, not for authoring skills.
---

# Bootstrap skills

`npx skills` has no dependency mechanism, so this skill is the dependency list.
Install this collection first, then each companion below the user wants.

## Install

Run from the project root. Add `-g` for a user-wide install, and `--skill <name>`
to pick individual skills. Preview any source with `--list` before installing.

```sh
npx skills add tsnl/skills
```

## Companions

Offer these; install only the ones the user accepts. Check each with `--list`,
since upstream layouts can change, and report any that fail to resolve.

| Command | Purpose | License |
| --- | --- | --- |
| `npx skills add blader/humanizer` | Rewrite AI-sounding prose | MIT |
| `npx skills add Nutlope/hallmark` | Design pages that don't look generated | MIT |
| `npx skills add heydamir/unslop` | Redesign AI-looking UIs | CC BY 4.0 |
| `npx skills add GoogleChrome/modern-web-guidance` | Modern web platform guidance | Apache-2.0 |
| `npx skills add cursor/plugins --skill thermo-nuclear-code-quality-review` | Strict maintainability review | see upstream |

## Update and verify

`npx skills update` refreshes installed skills. Commit the installed skill
directories and `skills-lock.json` (if created) so a team shares one revision.
Verify each installed `SKILL.md` is readable. Do not claim a running agent has
reloaded its catalog without observing it.

## Migrating from the old submodule layout

Earlier versions used a submodule at `.agents/skills` with `.claude/skills`
symlinked to it. Inspect the consumer first and preserve local changes and
unrelated files. Then, only with the user's agreement:

1. `git submodule deinit -f .agents/skills && git rm -f .agents/skills`
2. Remove `.claude/skills` if it is a symlink to `../.agents/skills`.
3. Run the install commands above.

Report what was installed, where, and how it was verified.
