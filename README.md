# Prompt Library

Library of reusable AI building blocks for Claude Code — skills, agents, and prompt templates. Single source of truth for everything that makes Claude smarter and more consistent across projects.

---

## What's in here

| Directory | Purpose |
|-----------|---------|
| `skills/` | Step-by-step workflows Claude follows when triggered |
| `agents/` | Specialized Claude personas launched as subprocesses |
| `prompts/` | Reusable prompt snippets included in skills/agents |

---

## Skills vs Agents

These are two fundamentally different concepts. Understanding the difference is key to using this library well.

### Skills

Instructions **for Claude itself** — loaded when you're already in a conversation. A skill activates automatically based on trigger phrases in its description, or you can invoke it explicitly via `/skill-name`.

**Use skills for:**
- Repeatable workflows (`/commit`, `/ship`, `/daily-summary`)
- Tasks that need a clear, step-by-step procedure
- Anything you do frequently within a single session

```
You say: "commit my changes"
         ↓
Claude loads the commit skill
         ↓
Claude follows the workflow: health check → group changes → commit
```

### Agents (subagents)

**Separate Claude processes** launched by the main Claude via the `Agent` tool. Each agent has its own isolated context and a defined role. They can run in parallel or sequentially.

**Use agents for:**
- Parallel independent tasks (research + analysis happening simultaneously)
- Context isolation (heavy work that would pollute the main conversation)
- Specialized roles (code-reviewer, architecture-designer, marketing-writer)
- Long background tasks

```
You say: "kick off the project"
         ↓
Orchestrator skill runs
         ↓
Launches agents in parallel:
    ├── architecture-agent  → proposes tech stack
    ├── design-agent        → proposes UI/UX concept
    └── marketing-agent     → defines audience & positioning
         ↓
Main Claude collects results and presents a unified plan
```

### Can agents live inside skills?

Yes. A skill can instruct Claude to launch subagents via the `Agent` tool. This is the **orchestrator pattern** — the skill is the conductor, agents do the specialized work.

```markdown
# Kickoff Project Skill

1. Ask user for a 1-2 sentence project description
2. Launch in parallel via Agent tool:
   - architecture-agent: design the technical architecture
   - design-agent: propose UX/UI concept and design system
   - marketing-agent: define audience and positioning
3. Collect results, surface conflicts, present unified plan
```

### Decision guide

| Situation | Use |
|-----------|-----|
| "When I say `/commit`, do X" | Skill |
| "Explore a large codebase in parallel" | Agent |
| "Repeatable process with heavy work inside" | Skill that launches Agents |
| "Specialized role (reviewer, tester, researcher)" | Agent definition in `agents/` |
| "Run design + architecture + marketing simultaneously" | Orchestrator skill + multiple agents |

---

## Install Skills

Symlink all skills so edits in this repo take effect immediately:

```bash
for skill in ~/Work/PromptLibrary/skills/*/; do
  ln -sf "$skill" ~/.claude/skills/$(basename "$skill")
done
```

Or run:

```bash
npm run install-skills
```

---

## Available Skills

| Skill | Trigger | What it does |
|-------|---------|-------------|
| `commit` | "commit", "commit my changes" | Health checks → logical grouping → conventional commits |
| `daily-summary` | "what did I do today?", "standup" | Plain-English summary of today's git commits |
| `ship` | "ship", "push and PR" | Commit + push + open pull request |

## Available Prompt Snippets

| Prompt | What it does |
|--------|-------------|
| `prompts/design/typography.md` | Font choices, pairings, weight/size extremes |
| `prompts/design/rpg-theme.md` | RPG aesthetic: fantasy colors, parchment, ornate borders |
| `prompts/design/frontend-aesthetics.md` | Anti-AI-slop rules: distinctive design, motion, atmosphere |
