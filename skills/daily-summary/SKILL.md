---
name: daily-summary
description: Generates a plain-English bullet summary of today's git commits by the current user. Use when user asks "what did I do today?", "daily summary", "standup update", "summarize my commits today", "write a daily update", or "what was done today in this repo".
---

# Daily Git Summary

Generate a short, plain-English business summary of what was accomplished today based on today's git commits by the current user.

## Steps

1. **Find the repository root:**
   ```bash
   git rev-parse --show-toplevel
   ```
   If it fails, ask the user to navigate to their git repository.

2. **Get today's commits by the current user:**
   ```bash
   git log --oneline --since="midnight" --author="$(git config user.email)" --no-merges
   ```

3. **Handle edge cases:**
   - No commits today → respond: "Nothing committed today yet."
   - Many commits (10+) → summarize all of them, don't truncate

4. **Write the summary** as a bullet list of key accomplishments. Each bullet starts with a strong action verb (Fixed, Added, Improved, Reduced, etc.) and states exactly what changed in concrete, observable terms.

   CRITICAL: Never mention commit hashes, file names, branch names, or internal technical terms.

   Bad: "Improved formatting"
   Good: "Chart values now show 1 decimal place instead of 3"

   Bad: "Fixed a bug"
   Good: "Fixed a bug where exported CSV files showed timestamps in the wrong timezone"

## Output format

Bullet list only. No intro sentence, no conclusion, no headers, no metadata.
