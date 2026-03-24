# PromptLibrary — CLAUDE.md

This repository is **Anatoli's personal library of AI agents, skills, and prompts** for use with Claude Code, Cowork, and the Claude Agent SDK. It serves as the single source of truth for reusable AI building blocks.

---

## Repository Purpose

Store, version, and iterate on:

---

## Installation — Skills

Skills in this repo can be installed for use with **Claude Code** or **Cowork** by copying (or symlinking) the skill folder to the appropriate location.

### Claude Code (global)
```bash
# Symlink all skills so edits in the repo take effect immediately
for skill in ~/path/to/PromptLibrary/skills/*/; do
  ln -sf "$skill" ~/.claude/skills/$(basename "$skill")
done
```

### Cowork
Place skill folders inside the Cowork skills directory:
```
~/Documents/Claude/skills/<skill-name>/SKILL.md
```

### Install script
Run from the repo root:
```bash
./scripts/install-skills.sh
```
*(See `scripts/install-skills.sh` — created automatically if missing.)*

---

## Conventions

### Agent files (`AGENT.md`)
Each agent definition should include:
- **Purpose** — one-line description
- **Persona** — tone, role, constraints
- **Tools** — list of tools the agent may use
- **Trigger** — when/how this agent should be invoked
- **Workflow** — step-by-step execution logic
- **Examples** — at least one worked example

### Skill files (`SKILL.md`)
Follow the standard Claude Code skill format:
```markdown
---
name: skill-name
description: Brief description used for triggering (be specific about when to use)
---

# Skill Name

[Full instructions for Claude to follow when this skill is invoked]
```

### Prompt templates
Use `{{VARIABLE}}` syntax for substitution placeholders. Document all variables at the top of each template file.

### Naming conventions
- Folder names: `kebab-case`
- Agent names: descriptive nouns (`code-reviewer`, `meeting-summarizer`)
- Skill names: verb-noun pairs (`generate-pdf`, `commit-code`)
- Prompt names: descriptive (`system-researcher.md`, `chain-qa-with-citations.md`)

---

## Working in This Repo

When Claude opens this repo, it should:
1. Read this `CLAUDE.md` to understand the project structure
2. Check the relevant subdirectory (`agents/`, `skills/`, `prompts/`) for existing work before creating new files
3. Follow the naming and format conventions above
4. Never delete files without explicit confirmation
5. When adding a new skill, also update `skills/README.md` index

---

## Key Files Index

| Path | Purpose |
|------|---------|
| `CLAUDE.md` | This file — project conventions & structure |
| `agents/README.md` | Index of all agents |
| `skills/README.md` | Index of all skills |
| `prompts/README.md` | Index of all prompt templates |
| `workflows/README.md` | Index of all workflows |

---

## Notes

- All `.md` files are source-of-truth and live in this git repo
- Skills meant for daily use should be symlinked (not copied) so changes propagate immediately
- Tag commits with the type of change: `skill:`, `agent:`, `prompt:`, `workflow:`
