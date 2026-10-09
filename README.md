# Skills

Reusable agent skills for code review and refactoring. The instructions use the
standard `SKILL.md` format and are independent of any programming language,
project, model provider, or host-specific tool names.

## Available skills

- [explain-and-refactor](explain-and-refactor/SKILL.md): simplify code through
  independent behavioral explanations, pseudocode, critical debate, and verified
  refactoring. Each pass uses a fresh explainer, while the parent inspects and
  edits the implementation. Preserve the user's scope and stop when further
  changes would not improve clarity.

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

These commands assume those paths are unused. Commit the submodule and symlink
in the consumer repository so everyone uses the same skill revision. The result
is `.agents/skills/explain-and-refactor/SKILL.md`, also available through
`.claude/skills/explain-and-refactor/SKILL.md`.

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

Invoke it as `$explain-and-refactor` in Codex or `/explain-and-refactor` in Claude
Code. Include the files to review, behavior to preserve, and any pass limits or
model preferences in your request. For example:

> Use explain-and-refactor on the parser module. Preserve its public API. Use a
> fresh explainer for each pass and repeat until further changes would not improve
> clarity. Keep all changes within the parser module and its tests.

For personal use across projects, keep a separate checkout and link individual
skills into each tool's personal skills directory:

```sh
git clone https://github.com/tsnl/skills.git "$HOME/Developer/skills"
mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills"
ln -s "$HOME/Developer/skills/explain-and-refactor" "$HOME/.agents/skills/"
ln -s "$HOME/Developer/skills/explain-and-refactor" "$HOME/.claude/skills/"
```

Update that personal checkout with `git -C "$HOME/Developer/skills" pull --ff-only`.

The workflow honors the model and reasoning effort available and requested in
its host. If isolated delegation is unavailable, the skill requires that the
limitation be disclosed; a local critique is not presented as an independent
review.

See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills)
and [Claude Code skill documentation](https://code.claude.com/docs/en/skills).
