---
name: install-repository-skills
description: Install or repair tsnl/skills in a Git repository using a pinned .agents/skills submodule and a .claude/skills symlink. Use for repository-local setup or migration, not personal or global skill installation.
---

# Install repository skills

Install `https://github.com/tsnl/skills.git` as a submodule directly at
`.agents/skills`. This repository keeps skill folders at its root. Share that
checkout with Claude Code through `.claude/skills -> ../.agents/skills`.

```text
.agents/skills/                  # Git submodule
  simplify/SKILL.md
  install-repository-skills/SKILL.md
.claude/skills -> ../.agents/skills
```

Use the user's intended consumer repository and follow their workspace or
worktree instructions. A request to install here does not include other
repositories or personal configuration. Do not install this repository inside
itself. Honor an explicitly requested source, revision, or layout.

## Inspect the existing installation

Check the consumer's Git status, `.gitmodules`, recorded submodule revisions,
and the destination paths before making changes. Recognize HTTPS and SSH URLs
for `tsnl/skills` as the same source. Inspect symlinks, including dangling links,
and their parent directories so writes stay inside the intended consumer.

- If the expected submodule already exists, preserve its pin. Initialize an
  uninitialized checkout with `git submodule update --init -- .agents/skills`;
  leave an initialized checkout in place, including local changes.
- If that submodule lives elsewhere, reuse it with `git mv` when migration is
  requested. Create `.agents` first. Git updates its path in `.gitmodules` and
  its working-tree metadata; inspect the result instead of editing `.git` files.
- If a destination contains unrelated files, a different submodule, or a link
  elsewhere, preserve it. Resolve the conflict from the user's instructions;
  ask only when a material choice remains. Do not delete files or force a link
  replacement to make installation succeed.

A reinstall is not an upgrade. Update an existing revision only when requested
or when an authorized migration requires the flat layout. Inspect and preserve
local work before any checkout change. If an old pin contains `skills/<name>`
instead of root-level skill folders, adopt a published flat-layout revision;
do not rearrange files inside the consumer's submodule.

## Install the submodule and shared discovery path

For an installation where the paths are unused, run these commands from the
consumer repository root:

```sh
mkdir -p .agents
git submodule add https://github.com/tsnl/skills.git .agents/skills
mkdir -p .claude
ln -s ../.agents/skills .claude/skills
```

The submodule records an exact commit. If the user specifies a revision, resolve
and check out that revision before recording the consumer's gitlink. Do not
copy individual skills, add another `skills/` directory, or create a separate
Claude checkout. An existing correct symlink needs no change. An empty ordinary
directory can be removed with `rmdir` before linking, after checking its status;
a nonempty directory requires the conflict handling above.

Document `git submodule update --init .agents/skills` in the consumer's setup
instructions and explain the shared Claude path. Keep that documentation short;
link to the skills repository for the workflow details.

## Verify and hand off

Check that `.gitmodules` names the intended source and path, the consumer records
`.agents/skills` as a gitlink, and skill folders contain readable `SKILL.md`
files directly below the submodule root. Confirm `.claude/skills` is a relative
symlink to `../.agents/skills` and both paths reach the same files. Use filesystem
checks; do not claim that a running agent has reloaded its skill catalog without
observing that separately.

Review the consumer diff, including submodule changes, and check for whitespace
errors. Leave unrelated files and staged work alone. `git submodule add` and
`git mv` stage the metadata they change; retain that staging. Stage additional
installation changes when requested or needed for an authorized commit;
never use `git add .`.
The intended tracked paths are `.gitmodules`, `.agents/skills`, `.claude/skills`,
the old gitlink path when migrating, and any setup documentation you changed.

Commit or push only within the user's authorization. Before publishing a
consumer commit, verify that its pinned submodule commit is available from the
recorded remote. When a fresh-clone check is warranted, use a temporary clone
with submodules initialized and verify the shared discovery paths there.

Report the consumer location, pinned revision, changes made, and verification.
If a conflict or unavailable remote prevents completion, identify what remains
without claiming the installation succeeded.
