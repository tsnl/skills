# Skills

Reusable agent skills for repository setup, code review, and refactoring. The
instructions use the standard `SKILL.md` format and are independent of any
programming language, project, model provider, or host-specific tool names.

## Available skills

- [simplify](simplify/SKILL.md): simplify recently written modules lossily
  and from the top down. A fresh explainer describes the simplest model of
  the behavior; incidental behavior is cut and reported; stages,
  representations, and stored state are trimmed before functions, which
  target 10 lines, in the style of Bitwise. Whole-program simplification
  requires explicit user authorization.
- [install-repository-skills](install-repository-skills/SKILL.md): install or
  repair this collection in a repository, pin it at `.agents/skills`, and share
  that checkout with Claude Code through a symlink.

Third-party skills, pinned as submodules and credited to their authors:

- [humanizer](https://github.com/blader/humanizer) (MIT, blader): rewrite
  AI-sounding prose so it reads like the writer, without changing what it says.
- [hallmark](https://github.com/Nutlope/hallmark) (MIT, Nutlope): design new
  pages that look designed rather than generated. Its optional image step calls
  Together AI only when `TOGETHER_API_KEY` is set.
- [unslop](https://github.com/heydamir/unslop) (CC BY 4.0, Damir Sokolovsky):
  turn an existing AI-looking UI into a designed interface.

`humanizer` keeps its `SKILL.md` at its repository root, so its submodule sits
at the root. The other two keep theirs under `skills/<name>/`, so their
repositories live in `vendor/` and a root-level symlink exposes each skill
folder. Agents that do not follow symlinked skill folders will not see those
two. Update a pin with `git submodule update --remote <path>` and review the
log before committing it.

## Use with Codex and Claude Code

Each skill folder lives at the repository root. In a consuming Git repository,
add this repository directly at `.agents/skills` and link Claude Code's discovery
directory to the same checkout:

```sh
git submodule add https://github.com/tsnl/skills.git .agents/skills
git submodule update --init --recursive .agents/skills
mkdir -p .claude
ln -s ../.agents/skills .claude/skills
git add .gitmodules .agents/skills .claude/skills
```

An agent that can read
[`install-repository-skills/SKILL.md`](install-repository-skills/SKILL.md) can
perform this setup, including checking existing installations before changing them.
Once available, invoke it as `$install-repository-skills` in Codex or
`/install-repository-skills` in Claude Code, giving the target repository path.

These commands assume those paths are unused. Commit the submodule and symlink
in the consumer repository so everyone uses the same skill revision. The result
is `.agents/skills/simplify/SKILL.md`, also available through
`.claude/skills/simplify/SKILL.md`.

After cloning a consumer repository, initialize its pinned skills, including
the third-party submodules, with:

```sh
git submodule update --init --recursive .agents/skills
```

To adopt a newer version, review and commit the updated submodule reference:

```sh
git submodule update --remote .agents/skills
git submodule update --init --recursive .agents/skills
git diff --submodule=log -- .agents/skills
git add .agents/skills
```

For simplification, use `$simplify` in Codex or `/simplify` in Claude Code.
Include the files to review, behavior to preserve, and any pass limits or
model preferences in your request. For example:

> Use simplify on the new parser module. Preserve its public API. Use a
> fresh explainer for each pass and repeat until further changes would not improve
> clarity. Keep all changes within the parser module and its tests.

For personal use across projects, keep a separate checkout and link individual
skills into each tool's personal skills directory:

```sh
git clone --recurse-submodules https://github.com/tsnl/skills.git "$HOME/Developer/skills"
mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills"
ln -s "$HOME/Developer/skills/simplify" "$HOME/.agents/skills/"
ln -s "$HOME/Developer/skills/simplify" "$HOME/.claude/skills/"
```

Update that personal checkout with
`git -C "$HOME/Developer/skills" pull --ff-only --recurse-submodules`.

The `simplify` workflow honors the model and reasoning effort available and
requested in its host. If isolated delegation is unavailable, the skill requires that the
limitation be disclosed; a local critique is not presented as an independent
review.

See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills)
and [Claude Code skill documentation](https://code.claude.com/docs/en/skills).
