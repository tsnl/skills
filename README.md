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
  collection with `npx skills`, offers the non-vendored companion, and migrates
  the old submodule layout.

## Vendored third-party skills

Pinned as git submodules under `skills/`, so `npx skills add tsnl/skills`
installs them with this collection (verified with `--list`):

- [humanizer](https://github.com/blader/humanizer) (MIT, blader): rewrite
  AI-sounding prose without changing what it says.
- [hallmark](https://github.com/Nutlope/hallmark) (MIT, Nutlope): design new
  pages that look designed rather than generated. Its optional image step calls
  Together AI only when `TOGETHER_API_KEY` is set.
- [unslop](https://github.com/heydamir/unslop) (CC BY 4.0, Damir Sokolovsky):
  turn an existing AI-looking UI into a designed interface.
- [modern-web-guidance](https://github.com/GoogleChrome/modern-web-guidance)
  (Apache-2.0, Google Chrome): steer agents toward modern web platform features.

Update a pin with `git submodule update --remote skills/<name>` and review the
log before committing. Clone with `--recurse-submodules` when developing here.

Not vendored, because it lives in the large `cursor/plugins` repository:

```sh
npx skills add cursor/plugins --skill thermo-nuclear-code-quality-review
```

Usage: `/simplify` in Claude Code or `$simplify` in Codex, naming the files to
review, behavior to preserve, and any limits.
