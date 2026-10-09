# Skills

Reusable agent skills for code review and refactoring. The instructions use the
standard `SKILL.md` format and are independent of any programming language,
project, model provider, or host-specific tool names.

## Available skills

- [explain-and-refactor](skills/explain-and-refactor/SKILL.md): simplify code through
  independent behavioral explanations, pseudocode, critical debate, and verified
  refactoring. Each pass uses a fresh explainer, while the parent inspects and
  edits the implementation. Preserve the user's scope and stop when further
  changes would not improve clarity.

## Use with Codex and Claude Code

Both tools support skill folders and symlinked installations. Keep one checkout
and link the skill into each tool's personal skills directory to use it across
local projects. Adjust the checkout path if needed.

```sh
git clone https://github.com/tsnl/skills.git "$HOME/Developer/skills"
mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills"
ln -s "$HOME/Developer/skills/skills/explain-and-refactor" "$HOME/.agents/skills/"
ln -s "$HOME/Developer/skills/skills/explain-and-refactor" "$HOME/.claude/skills/"
```

Invoke it as `$explain-and-refactor` in Codex or `/explain-and-refactor` in Claude
Code. Include the files to review, behavior to preserve, and any pass limits or
model preferences in your request. For example:

> Use explain-and-refactor on the parser module. Preserve its public API. Use a
> fresh explainer for each pass and repeat until further changes would not improve
> clarity. Keep all changes within the parser module and its tests.

Update the shared source with:

```sh
git -C "$HOME/Developer/skills" pull --ff-only
```

For repository-scoped discovery, use `.agents/skills/` for Codex and
`.claude/skills/` for Claude Code inside that repository. For a setup shared with
teammates, vendor or pin the skill source and use relative links within the
repository.

The workflow honors the model and reasoning effort available and requested in
its host. If isolated delegation is unavailable, the skill requires that the
limitation be disclosed; a local critique is not presented as an independent
review.

See the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills)
and [Claude Code skill documentation](https://code.claude.com/docs/en/skills).
