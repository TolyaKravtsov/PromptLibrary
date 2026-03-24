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

Each skill is a **folder** named in `kebab-case` containing a `SKILL.md` file. The file must be named exactly `SKILL.md` (case-sensitive). Never put a `README.md` inside the skill folder — documentation goes in `references/` or the body of `SKILL.md`.

**Minimal required format:**
```markdown
---
name: skill-name-in-kebab-case
description: [What it does] + [When to use it]. Include specific trigger phrases users would say.
---

# Skill Name

## Instructions

### Step 1: [First Major Step]
Clear explanation of what happens.
```

**YAML frontmatter rules:**
- `name`: kebab-case only, no spaces or capitals, must match folder name
- `description`: MUST include both WHAT the skill does AND WHEN to trigger it (trigger phrases). Under 1024 chars. No XML angle brackets (`<` or `>`).
- Optional: `license`, `compatibility`, `metadata` (author, version, etc.)
- Forbidden: XML tags, skills named with "claude" or "anthropic" prefix

**Description formula:**
```
[What it does]. Use when user says "[phrase1]", "[phrase2]", or asks to [action].
```

Good:
```yaml
description: Runs quality checks then commits changes with conventional messages. Use when user says "commit", "commit my changes", or wants to save work to git.
```

Bad:
```yaml
description: Helps with projects.          # too vague, no trigger
description: Implements the git workflow.  # technical, no user phrases
```

**Instructions best practices:**
- Be specific and actionable — include exact commands to run, not vague directives
- Include error handling for common failures
- Use `## Important` or `## CRITICAL` headers for must-follow rules
- Move detailed docs to `references/` and link to them — keep `SKILL.md` under 5,000 words
- Progressive disclosure: core steps in body, edge cases and references in linked files

**Folder structure (optional extras):**
```
skill-name/
├── SKILL.md          # Required
├── scripts/          # Optional: executable code
├── references/       # Optional: detailed docs loaded on demand
└── assets/           # Optional: templates, examples
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
