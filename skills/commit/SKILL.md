---
name: commit
description: >
  Use this skill whenever the user wants to commit code to git, run /commit, or asks Claude to "commit my changes", "make a commit", "git commit", "stage and commit", or "save my work to git". Also trigger when the user says things like "commit this", "push my changes" (even if they don't mention git explicitly), or wants to wrap up a coding session by saving progress. This skill runs quality checks (type-check, lint, tests) first, then intelligently groups changes into logical commits with well-written messages. Always use this skill for any commit-related workflow — even if the user just says "commit" with no other context.
---

# /commit — Smart Git Commit Workflow

Your job is to get the project into a committed state: verify it's healthy, understand what changed, group changes into logical commits, write good messages, and commit them. Be methodical but don't over-engineer it — most projects just need 1–3 commits.

## Step 1: Verify project health

Before touching git, confirm the project is in good shape. Run all three checks:

```bash
npm run type-check && npm run lint && npm run test
```

If **any check fails**, stop immediately. Report the exact error output to the user and ask them to fix it before committing. Don't try to auto-fix — that's not your job here, and partial fixes can make things worse.

If a command doesn't exist (e.g., `npm run type-check` exits with "Missing script"), note it and continue — don't block on missing scripts. But if it exists and fails, stop.

## Step 2: Understand what changed

Run these to get a full picture:

```bash
git status          # what files are staged, unstaged, untracked
git diff            # unstaged changes
git diff --staged   # already staged changes
git log --oneline -10  # recent commit history (to match the project's style)
```

Read the diff carefully. Understand *what* changed and *why* — the commit messages you write should reflect intent, not just file names.

## Step 3: Group changes into logical commits

Most of the time, all the changes belong in one commit. But sometimes the work spans multiple concerns — a bug fix alongside a new feature, or a refactor plus a config change. When that happens, split into separate commits so the history stays readable.

**Good signals for splitting:**
- A bug fix and a new feature landed in the same session
- Infrastructure/config changes (e.g., `.env`, `package.json`, CI) separate from product code
- A refactor that's unrelated to the feature being built
- Changes to two completely different parts of the app with no logical connection

**Don't over-split.** If everything is part of one coherent unit of work, one commit is correct. Multiple tiny commits for the same feature make history noisy.

For each group, list which files belong to it and why.

## Step 4: Stage and commit each group

For each logical commit group:

1. **Stage exactly the right files:**
   ```bash
   git add path/to/file1 path/to/file2
   # Use -p for partial staging if only part of a file belongs to this commit:
   # git add -p path/to/file
   ```
   Never use `git add .` or `git add -A` — be explicit about what's going in.

2. **Write the commit message** using Conventional Commits format:
   ```
   <type>(<scope>): <short description>

   <optional body — explain the why, not the what>
   ```

   Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `style`, `perf`, `build`, `ci`

   **Good messages** explain *why* the change was made, not just what files changed:
   - ✅ `feat(auth): add JWT refresh token support`
   - ✅ `fix(cart): prevent duplicate items when adding from search results`
   - ❌ `update auth.ts`
   - ❌ `fix bug`

   For a multi-line body, explain the motivation or context. Keep the subject line under 72 characters.

3. **Commit:**
   ```bash
   git commit -m "$(cat <<'EOF'
   type(scope): short description

   Optional longer explanation of why this change was made.
   EOF
   )"
   ```

## Step 5: Confirm and summarize

After all commits are done, run:

```bash
git log --oneline -5
```

Show the user the final commits and a one-sentence summary of what was committed.

---

## Example flows

**Simple case — one coherent change:**
> User added a new API endpoint and its tests.
> → One commit: `feat(api): add /users/search endpoint with pagination`

**Two concerns in one session:**
> User fixed a typo in an error message AND added a new settings page.
> → Commit 1: `fix(errors): correct typo in 404 message`
> → Commit 2: `feat(settings): add user preferences page`

**Checks fail:**
> `npm run lint` exits with ESLint errors.
> → Report the errors. Don't commit. Ask user to fix and re-run `/commit`.

---

## Edge cases

- **No changes to commit**: Tell the user the working tree is clean.
- **Only untracked files**: Add them explicitly after confirming with the user which ones should be tracked (some might be build artifacts or secrets that shouldn't be committed).
- **Merge conflicts**: Don't proceed. Tell the user to resolve conflicts first.
- **No `package.json` / not a Node project**: Skip `npm run ...` checks. If there are equivalent checks (e.g., `python -m pytest`, `cargo test`), use those if the user mentioned them. Otherwise proceed to git steps.
