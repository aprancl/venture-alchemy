# Execution Context

## Project Patterns
- Plugin structure: `{plugin-name}/{version}/` with `skills/`, `agents/`, `references/`, `README.md`
- SKILL.md: YAML frontmatter + Critical Rules + Workflow Overview + Numbered Phases + Reference Files
- All three skills follow same structure; generate-idea (11 phases), validate-idea (10 phases), plan-execution (9 phases)
- AskUserQuestion mandatory in all skills, with positive/negative examples in Critical Rules
- Plan Mode Behavior section in all skills prevents CC from treating as plan-then-execute
- Agent spawning: `Task` tool with `subagent_type: {agent-name}`
- Sub-skill invocation: `Task` tool with `skill: {skill-name}`
- Templates: HTML comments as conditional guidance, source attribution format
- Confidence levels (High/Medium/Low) with adjustment rules
- Go/No-Go: four options (Go, Conditional Go, No-Go, Pivot Recommended)

## Key Decisions
- All skills use opus model
- Next Steps shows /create-spec → /create-tasks → /execute-tasks pipeline
- External idea handling with detection logic and confidence adjustments
- No-competitor fallback: demand signal analysis
- validate-idea output: `ideas/{market-slug}/validation/idea-NNN-validation.md`
- plan-execution output: `ideas/{market-slug}/execution-plans/idea-NNN-plan.md`

## Known Issues
<!-- No issues encountered -->

## File Map
- `startup-idea-generator/0.1.0/README.md` - Plugin documentation
- `startup-idea-generator/0.1.0/agents/market-researcher.md` - Market research agent
- `startup-idea-generator/0.1.0/skills/generate-idea/SKILL.md` - Hub skill (743 lines, 11 phases)
- `startup-idea-generator/0.1.0/skills/generate-idea/references/` - 4 references + 2 templates
- `startup-idea-generator/0.1.0/skills/validate-idea/SKILL.md` - Validation skill (~767 lines, 10 phases)
- `startup-idea-generator/0.1.0/skills/validate-idea/references/validation-criteria.md` - Frameworks (~706 lines)
- `startup-idea-generator/0.1.0/skills/validate-idea/references/templates/validation-report.md` - Template (208 lines)
- `startup-idea-generator/0.1.0/skills/plan-execution/SKILL.md` - Planning skill (762 lines, 9 phases)
- `startup-idea-generator/0.1.0/skills/plan-execution/references/planning-patterns.md` - Patterns (~431 lines)
- `startup-idea-generator/0.1.0/skills/plan-execution/references/templates/execution-plan.md` - Template (358 lines)

## Task History
### Prior Tasks Summary (Tasks 1-15)
All 14 PASS. Created complete plugin structure: directory, README, market-researcher agent, all generate-idea references/templates/SKILL.md, all validate-idea references/templates, all plan-execution references/templates.

### Task [13]: Create validate-idea SKILL.md - PASS
- Created ~767 lines, 10 phases. Go/No-Go with 4 options, external idea handling, demand signal fallback.

### Task [16]: Create plan-execution SKILL.md - PASS
- Created 762 lines, 9 phases. Next Steps shows full agent-alchemy pipeline. Plan Mode Behavior included.
