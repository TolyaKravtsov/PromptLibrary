Perform the following git workflow:

1. **Understand all changes**: Run `git status` and `git diff HEAD` (includes staged + unstaged) to see everything that has changed. Also check `git diff --cached` for already-staged changes. Note all modified, added, and deleted files.

2. **Group changes into logical commits**: Analyze the diff and mentally group files by concern. Each commit must be a complete, working unit — no partial features, no broken imports, no half-done refactors. Typical groupings:
   - A feature change + its tests together (never separate them)
   - A type/schema change + all files that depend on it
   - A rename/refactor as one atomic commit
   - Unrelated fixes each get their own commit

3. **Commit each group**: For each logical group:
   - `git add <specific files>` — never `git add .` or `git add -A` blindly; exclude secrets/credentials/env files
   - `git commit -m "type(scope): description"` using conventional commits format
   - types: `feat` `fix` `refactor` `chore` `docs` `test`
   - Message must accurately describe what this commit alone does
   - Each commit must leave the codebase in a working state

4. **Push**: Push the current branch to remote
   - If no upstream is set, use `git push -u origin <branch-name>`
   - Otherwise use `git push`

5. **Create PR**: Create a pull request using GitHub CLI
   - Use `gh pr create` with a clear title and description
   - PR description should include:
     - Summary of changes (bullet points covering all commits)
     - Test plan
   - Target the appropriate base branch (usually `main` or parent task branch per CLAUDE.md conventions)

If any step fails, stop and report the error. Ask for confirmation before creating the PR if there are concerns about the changes.