# Startup Idea Generator

A Claude Code plugin that helps users generate startup and side-project ideas through interview-driven discovery, market-first validation, and execution planning. Uses deep user profiling, web research, and anti-slop filtering to produce evidence-backed ideas rather than generic suggestions.

## Skills

| Skill | Invocable | Description |
|-------|-----------|-------------|
| `/generate-idea` | Yes | Interview-driven startup and side-project idea generator. Uses a market-first approach with deep user profiling, web research, and anti-slop filtering to generate evidence-backed ideas scored across four dimensions. |
| `/validate-idea` | Yes | Structured validation of generated ideas through market analysis, competitive landscape review, and feasibility assessment. Provides a go/no-go recommendation with evidence. |
| `/plan-execution` | Yes | Execution planning that transforms validated ideas into actionable implementation plans with milestones, MVP scoping, and resource estimates for solo developers. |

## Agents

| Agent | Model | Purpose |
|-------|-------|---------|
| `market-researcher` | sonnet | Market and domain research agent. Searches external sources via WebSearch/WebFetch for competitor analysis, market size estimates, pricing intelligence, trend data, and community/outreach targets. Shared by generate-idea and validate-idea skills. |

## The Idea Pipeline

```
/generate-idea   -->  Interview + market-first idea discovery
                        |
                   ideas/{market-slug}/MARKET.md
                   ideas/{market-slug}/idea-001.md
                        |
/validate-idea   -->  Evidence-backed validation
                        |
                   ideas/{market-slug}/validation/idea-001-validation.md
                        |
/plan-execution  -->  Actionable execution plan
                        |
                   ideas/{market-slug}/execution-plans/idea-001-plan.md
```

## Directory Structure

```
startup-idea-generator/
├── plugin.yaml                            # Plugin manifest (name, version, skills)
├── skills/
│   ├── generate-idea/
│   │   ├── SKILL.md                       # Hub skill — interview + idea generation (11 phases)
│   │   └── references/
│   │       ├── interview-questions.md     # User profiling question bank (8 categories)
│   │       ├── market-categories.md       # Market taxonomy and discovery patterns
│   │       ├── scoring-rubric.md          # 4-dimension scoring criteria
│   │       ├── anti-slop-filters.md       # Three-layer anti-slop pipeline
│   │       └── templates/
│   │           ├── idea-document.md       # Template for idea output files
│   │           └── market-analysis.md     # Template for MARKET.md files
│   ├── validate-idea/
│   │   └── references/
│   │       └── templates/
│   └── plan-execution/
│       └── references/
│           └── templates/
├── agents/
│   └── market-researcher.md              # Web research agent definition
├── references/                           # Shared plugin-level references
└── README.md
```

## Usage

### Generate Ideas

Invoke the idea generation skill to start an interactive interview and receive personalized startup ideas:

```
/startup-idea-generator:generate-idea
```

The skill walks you through:
1. **User profiling interview** (3-4 adaptive rounds covering skills, interests, constraints, network, and goals)
2. **Market discovery** via web research using the market-researcher agent
3. **Idea generation** with anti-slop filtering (personalization, contrarian, and freshness checks)
4. **Scoring** across four dimensions: Revenue Potential, Solo Buildability, Market Validation, and Personal Fit (25% each)
5. **Output** as structured markdown files in the `ideas/` directory

### Validate Ideas

Run structured validation on a generated idea:

```
/startup-idea-generator:validate-idea
```

Performs deep market analysis, competitive landscape review, and feasibility assessment. Produces a go/no-go recommendation backed by evidence.

### Plan Execution

Transform a validated idea into an actionable plan:

```
/startup-idea-generator:plan-execution
```

Generates an implementation plan with MVP scoping, milestones, and resource estimates tailored for solo developers.

## Output Structure

All output is organized by market in the `ideas/` directory:

```
ideas/
├── {market-slug}/
│   ├── MARKET.md                    # Market analysis document
│   ├── idea-001.md                  # Individual idea document
│   ├── idea-002.md                  # Another idea in this market
│   ├── validation/
│   │   └── idea-001-validation.md   # Validation report
│   └── execution-plans/
│       └── idea-001-plan.md         # Execution plan
└── {another-market-slug}/
    ├── MARKET.md
    └── idea-001.md
```

## Prerequisites

- **Claude Code** with plugin support
- **WebSearch** and **WebFetch** tool access (required by the market-researcher agent for external research)

## Versioning

This plugin follows semantic versioning (semver):
- **Patch** (0.1.x): Modifications and bug fixes to existing capability
- **Minor** (0.x.0): Smaller-scale additions and enhancements
- **Major** (x.0.0): Larger additions or breaking changes
