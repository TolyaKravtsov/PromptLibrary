---
name: ship
description: Commits all changes, pushes to remote, and creates a pull request. Use when user says "ship", "ship my changes", "push and open a PR", "create a pull request", or wants to publish their work to GitHub.
---

# Ship — Commit, Push, and Open PR

Perform the following git workflow in order:

## Step 1: Understand all changes

```bash
git status
git diff HEAD
git diff --cached
```

Note all modified, added, and deleted files.

## Step 2: Group into logical commits

Each commit must be a complete, working unit:
- Feature change + its tests together (never separate)
- Type/schema change + all files that depend on it
- Rename/refactor as one atomic commit
- Unrelated fixes each get their own commit

## Step 3: Commit each group

- `git add <specific files>` — never `git add .` or `git add -A`; exclude secrets/env files
- `git commit -m "type(scope): description"` using conventional commits
- Types: `feat` `fix` `refactor` `chore` `docs` `test`
- Each commit must leave the codebase in a working state

## Step 4: Push

```bash
git push -u origin <branch-name>   # if no upstream set
git push                            # otherwise
```

## Step 5: Create PR

```bash
gh pr create --title "..." --body "..."
```

PR description must include:
- Summary of changes (bullet points covering all commits)
- Test plan

Target the appropriate base branch (usually `main`).

## Error handling

If any step fails, stop and report the error. Ask for confirmation before creating the PR if there are concerns about the changes.
