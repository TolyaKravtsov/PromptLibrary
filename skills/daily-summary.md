---
name: daily-summary
description: |
  Generates a concise business summary of today's git commits for the current user. Use this skill whenever the user asks for a daily summary, wants to know what they worked on today, asks for a standup update, or says things like "what did I do today?", "summarize my commits today", "write a daily update", "generate a standup", or "what was done today in this repo". Also triggers on the slash command /dailySummary. Trigger even if the user doesn't say "git" or "commits" — the intent to summarize today's development work is enough.
slash_command: dailySummary
---

# Daily Git Summary Skill

Generate a short, plain-English business summary of what was accomplished today based on today's git commits by the current user.

## Steps

1. **Find the repository root** — run `git rev-parse --show-toplevel`. If it fails, ask the user to navigate to their git repository.

2. **Get today's commits by the current user** — run:
   ```bash
   git log --oneline --since="midnight" --author="$(git config user.email)" --no-merges
   ```

3. **Handle edge cases:**
   - **No commits today** → respond: "Nothing committed today yet."
   - **Many commits (10+)** → summarize all of them, don't truncate

4. **Write the summary** as a bullet-point list of key accomplishments. Each bullet starts with a strong action verb (Fixed, Reduced, Added, Improved, etc.) and states exactly what changed — not in abstract terms, but in concrete observable terms. Never mention commit hashes, file names, branch names, or technical terms.

   **Rules:**
   - No abstract language. Not "improved formatting" — instead: "Chart values now show 1 decimal place instead of 3."
   - Not "fixed a bug" — instead: "Fixed a bug where exported CSV files showed timestamps in the wrong timezone."
   - Each bullet = one concrete thing that changed.

## Output format

Bullet list only. No intro sentence, no conclusion, no headers, no metadata.
