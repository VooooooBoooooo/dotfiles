---
description: Verify, commit, synchronize, and push relevant changes with linear history
argument-hint: "[update-dotfiles | scope or instructions]"
---
Ship the relevant work end to end.

Mode or scope: ${ARGUMENTS:-all intended changes from this session}

This invocation authorizes commits and a push. Inspect `git status`, staged and unstaged diffs, the current branch and remotes, and `git log --oneline -10` before changing anything. Never commit secrets, runtime state, or unrelated user-owned work. Ask only when intent or a conflict cannot be resolved safely.

1. Stage only relevant changes, run the smallest useful checks, and commit them using the repository's existing message style. Do not create empty commits.
2. Fetch before integrating remote work. Rebase instead of merge so new history stays linear; resolve conflicts from source and context, then rerun affected checks.
3. Push the current branch to its configured push remote. Do not force-push in normal mode; stop and ask if one would be required.
4. Report commits, checks, conflict resolutions, and push result.

When the first argument is `update-dotfiles`:

- Treat all safe working-tree changes in the dotfiles repository as intended, while still excluding secrets and runtime artifacts.
- Commit local changes first, then run `git fetch --all --prune` and rebase the current `main` onto `upstream/main`; never create a merge commit.
- Resolve straightforward conflicts autonomously. Preserve fork-specific identity and preferences, adopt newer upstream architecture, and drop obsolete generated-file commits when upstream already supersedes them. If the correct result is genuinely ambiguous, leave the rebase paused and ask the user.
- Run the repository's documented checks after the rebase and commit any necessary follow-up fix.
- Fetch `origin` again immediately before publishing. Because this mode explicitly authorizes synchronizing rewritten `main`, use `git push --force-with-lease origin main` when a normal push cannot update it. Never use plain `--force`, and never rewrite any other branch.
