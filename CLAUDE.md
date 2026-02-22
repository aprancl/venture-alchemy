# CLAUDE.md — venture-alchemy

## Project Overview

Claude Code plugin marketplace for entrepreneurial pursuits. Contains plugins that help users discover, validate, and plan startup and side-project ideas.

## Repository Structure

```
venture-alchemy/
├── specs/                          # Specification documents
│   └── {name}-SPEC.md              # PRD specs (input to /create-tasks)
├── startup-idea-generator/         # Plugin: idea generation pipeline
│   ├── plugin.yaml                 # Plugin manifest (name, version, skills)
│   ├── README.md
│   ├── skills/                     # Slash-command skills (SKILL.md)
│   ├── agents/                     # Agent definitions (.md)
│   └── references/                 # Shared cross-skill references
└── CLAUDE.md
```

## Plugin Architecture Conventions

### Directory Layout

Each plugin follows: `{plugin-name}/` containing `plugin.yaml`, `skills/`, `agents/`, `references/`, and `README.md`.

### SKILL.md Format

- YAML frontmatter with `name`, `description`, `user-invocable`, `model`, `tools`
- Critical Rules section (AskUserQuestion mandatory, Plan Mode Behavior)
- Numbered phases as `## Phase N: Name`
- Reference Files section at bottom listing all loaded references

### Agent Definitions

- Markdown files in `agents/` with YAML frontmatter (`name`, `description`, `model`, `tools`)
- System prompt as markdown body
- Spawned via `Task` tool with `subagent_type: {agent-name}`

### Reference Files

- Knowledge base files in `skills/{skill}/references/`
- Templates in `references/templates/` with HTML comments as conditional guidance
- Shared references at plugin root `references/` for cross-skill use

### Key Patterns

- All skills use `opus` model
- `AskUserQuestion` is mandatory for all user interaction (never plain text questions)
- Plan Mode Behavior section prevents Claude Code from treating skill invocation as plan-then-execute
- Source attribution: `*([Source title](URL), accessed YYYY-MM-DD)*`
- Confidence levels: High/Medium/Low with documented adjustment rules
- Output organized by market: `ideas/{market-slug}/`

## Versioning

Plugin versions are tracked in `plugin.yaml` using semantic versioning:
- **Patch** (0.1.x): Bug fixes and modifications
- **Minor** (0.x.0): Smaller additions
- **Major** (x.0.0): Larger additions or breaking changes

## Current Plugins

| Plugin | Version | Skills | Status |
|--------|---------|--------|--------|
| startup-idea-generator | 0.1.0 | `/generate-idea`, `/validate-idea`, `/plan-execution` | Complete |
