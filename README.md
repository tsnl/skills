# Skills

Reusable agent skills for repository setup, code review, and refactoring. The
instructions use the standard `SKILL.md` format and are independent of any
programming language, project, model provider, or host-specific tool names.

## Available skills

- [simplify](simplify/SKILL.md): simplify a handful of recently written modules through
  independent behavioral explanations, pseudocode, critical debate, and verified
  refactoring. Each pass uses a fresh explainer, while the parent inspects and
  edits the implementation. Preserve the user's scope and stop when further
  changes would not improve clarity. Whole-program simplification requires
  explicit user authorization. Target functions of at most 10 LOC and use
  Bitwise as a reference for direct, explanatory code.
- [install-repository-skills](install-repository-skills/SKILL.md): install or
  repair this collection in a repository, pin it at `.agents/skills`, and share
  that checkout with Claude Code through a symlink.

## Use with Codex and Claude Code

Each skill folder lives at the repository root. In a consuming Git repository,
add this repository directly at `.agents/skills` and link Claude Code's discovery
directory to the same checkout:

```sh
git submodule add https://github.com/tsnl/skills.git .agents/skills
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

After cloning a consumer repository, initialize its pinned skills with:

```sh
git submodule update --init .agents/skills
```

To adopt a newer version, review and commit the updated submodule reference:

```sh
git submodule update --remote .agents/skills
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
git clone https://github.com/tsnl/skills.git "$HOME/Developer/skills"
mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills"
ln -s "$HOME/Developer/skills/simplify" "$HOME/.agents/skills/"
ln -s "$HOME/Developer/skills/simplify" "$HOME/.claude/skills/"
```

Update that personal checkout with `git -C "$HOME/Developer/skills" pull --ff-only`.

The `simplify` workflow honors the model and reasoning effort available and
requested in its host. If isolated delegation is unavailable, the skill requires that the
limitation be disclosed; a local critique is not presented as an independent
review.

See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills)
and [Claude Code skill documentation](https://code.claude.com/docs/en/skills).
