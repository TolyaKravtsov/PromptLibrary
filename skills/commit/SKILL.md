---
name: commit
description: Runs quality checks then groups and commits changes with conventional commit messages. Use when user says "commit", "commit my changes", "git commit", "stage and commit", "save my work to git", or wants to wrap up a coding session by saving progress.
---

# Commit — Smart Git Commit Workflow

Your job is to get the project into a committed state: verify it's healthy, understand what changed, group changes into logical commits, write good messages, and commit them. Be methodical but don't over-engineer it — most projects just need 1–3 commits.

## Step 1: Verify project health

Before touching git, confirm the project is in good shape. Run all three checks:

```bash
npm run type-check && npm run lint && npm run test
```

If **any check fails**, stop immediately. Report the exact error output to the user and ask them to fix it before committing. Don't try to auto-fix — that's not your job here, and partial fixes can make things worse.

If a command doesn't exist (e.g., `npm run type-check` exits with "Missing script"), note it and continue — don't block on missing scripts. But if it exists and fails, stop.

## Step 2: Understand what changed

```bash
git status          # what files are staged, unstaged, untracked
git diff            # unstaged changes
git diff --staged   # already staged changes
git log --oneline -10  # recent commit history (to match the project's style)
```

Read the diff carefully. Understand *what* changed and *why* — the commit messages you write should reflect intent, not just file names.

## Step 3: Group changes into logical commits

Most of the time, all changes belong in one commit. Split only when concerns are clearly separate:

- A bug fix and a new feature in the same session
- Infrastructure/config changes separate from product code
- A refactor unrelated to the feature being built

**Don't over-split.** Multiple tiny commits for the same feature make history noisy.

## Step 4: Stage and commit each group

1. **Stage explicitly** — never `git add .` or `git add -A`:
   ```bash
   git add path/to/file1 path/to/file2
   ```

2. **Write the commit message** using Conventional Commits:
   ```
   type(scope): short description
   ```
   Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `style`, `perf`, `build`, `ci`

   Good: `feat(auth): add JWT refresh token support`
   Bad: `update auth.ts`, `fix bug`

3. **Commit:**
   ```bash
   git commit -m "$(cat <<'EOF'
   type(scope): short description
   EOF
   )"
   ```

## Step 5: Confirm and summarize

```bash
git log --oneline -5
```

Show the user the final commits and a one-sentence summary.

## Edge cases

- **No changes**: Tell the user the working tree is clean.
- **Only untracked files**: Confirm which should be tracked before adding (some may be artifacts or secrets).
- **Merge conflicts**: Don't proceed. Ask user to resolve first.
- **No `package.json`**: Skip npm checks. Use language-equivalent checks if available (e.g., `cargo test`, `python -m pytest`).
