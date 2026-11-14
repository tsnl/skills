# Skills

Reusable agent skills for code review and refactoring, in the standard
`SKILL.md` format and installable with [`npx skills`](https://github.com/vercel-labs/skills).

## Install

```sh
npx skills add tsnl/skills                 # choose skills and agents interactively
npx skills add tsnl/skills --skill simplify
npx skills add tsnl/skills --list          # preview only
npx skills add tsnl/skills -g              # install for your user, not the project
npx skills update                          # update installed skills
```

## Available skills

- [simplify](skills/simplify/SKILL.md): simplify recently written modules
  lossily and from the top down. A fresh explainer describes the simplest model
  of the behavior; incidental behavior is cut and reported; stages,
  representations, and stored state are trimmed before functions, which target
  10 lines, in the style of Bitwise. Whole-program simplification requires
  explicit user authorization.
- [bootstrap-skills](skills/bootstrap-skills/SKILL.md): installs this
  collection and the companion skills below with `npx skills`, and migrates the
  old submodule layout. Install it first and ask your agent to run it.

## Companion skills

`npx skills` has no dependency mechanism, so these third-party skills are not
bundled. `bootstrap-skills` carries this list and installs them on request, or
run them yourself:

```sh
npx skills add blader/humanizer          # MIT: de-AI-ify prose
npx skills add Nutlope/hallmark          # MIT: design pages that don't look generated
npx skills add heydamir/unslop           # CC BY 4.0: redesign AI-looking UIs
npx skills add GoogleChrome/modern-web-guidance   # Apache-2.0: steer agents to modern web platform features
npx skills add cursor/plugins --skill thermo-nuclear-code-quality-review   # strict maintainability review
```

Usage: `/simplify` in Claude Code or `$simplify` in Codex, naming the files to
review, behavior to preserve, and any limits.
