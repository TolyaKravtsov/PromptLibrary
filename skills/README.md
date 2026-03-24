# Skills

Reusable Claude Code skills. Each skill lives in its own folder with a `SKILL.md` file.

## Install

Symlink all skills so edits propagate immediately:

```bash
for skill in ~/Work/PromptLibrary/skills/*/; do
  ln -sf "$skill" ~/.claude/skills/$(basename "$skill")
done
```

## Git Workflow

| Skill | Trigger | Description |
|-------|---------|-------------|
| `commit/` | "commit", "commit my changes", "git commit" | Health checks → logical grouping → conventional commit messages |
| `daily-summary/` | "what did I do today?", "daily summary", "standup" | Plain-English summary of today's git commits |
| `ship/` | "ship", "push and PR", "create a pull request" | Commit + push + open PR in one workflow |
