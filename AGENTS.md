# Agent instructions

This repository is a collection of agent skills, installed with
[`npx skills`](https://github.com/vercel-labs/skills).

## Installing

```sh
npx skills add tsnl/skills                      # interactive
npx skills add tsnl/skills --skill simplify     # one skill
npx skills add tsnl/skills --list               # preview only
npx skills add tsnl/skills -g                   # user-wide instead of project
npx skills update                               # update installed skills
```

Commit the installed skill directories and `skills-lock.json` (if created) so a
team shares one revision. Verify each installed `SKILL.md` is readable; do not
claim a running agent has reloaded its skills without observing it.

Not bundled, install separately if wanted (check with `--list` first):

```sh
npx skills add cursor/plugins --skill thermo-nuclear-code-quality-review
```

### Migrating from the old submodule layout

Earlier versions were installed as a submodule at `.agents/skills` with
`.claude/skills` symlinked to it. Preserve local changes, then, with the user's
agreement: `git submodule deinit -f .agents/skills && git rm -f .agents/skills`,
remove `.claude/skills` if it links to `../.agents/skills`, and run the install.

## Developing

- Skills live in `skills/<name>/SKILL.md`. Third-party skills (`humanizer`,
  `hallmark`, `unslop`, `modern-web-guidance`) are pinned git submodules under
  `skills/`; clone with `--recurse-submodules`.
- Update a pin with `git submodule update --remote skills/<name>` and review the
  log before committing.
- After changes, confirm discovery with
  `npx skills add <this-repo-or-path> --list`.
