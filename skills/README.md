# Skills

Reusable Claude Code skills. Install by symlinking into `~/.claude/skills/`.

```bash
for skill in ~/Work/PromptLibrary/skills/*/; do
  ln -sf "$skill" ~/.claude/skills/$(basename "$skill")
done
```

## Git Workflow

| Skill | Trigger | Description |
|-------|---------|-------------|
| `commit` | `/commit` or "commit my changes" | Health checks, logical grouping, conventional commit messages |
| `daily-summary` | `/dailySummary` or "what did I do today?" | Plain-English summary of today's git commits |
| `ship` | `/ship` | Commit + push + open PR in one workflow |
