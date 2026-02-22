# venture-alchemy

A plugin for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that helps you discover, validate, and plan startup and side-project ideas -- all from your terminal.

It interviews you about your skills, experience, and constraints, researches real markets via web search, and generates personalized ideas scored on revenue potential, buildability, market validation, and personal fit. From there it can validate an idea with SWOT analysis and competitor research, then produce a concrete execution plan with MVP scope, milestones, and pricing strategy ready to hand off to a spec generator.

## What you get

The plugin provides three slash commands that form a pipeline:

| Command | What it does |
|---------|-------------|
| `/generate-idea` | Runs a multi-round interview to understand your background, researches markets, generates ideas with anti-slop filtering, and scores them |
| `/validate-idea` | Takes a generated idea and performs deep competitor analysis, SWOT, devil's advocate challenges, and a go/no-go recommendation |
| `/plan-execution` | Turns a validated idea into an execution plan with MVP features, milestones, pricing, and customer acquisition strategy |

Each command produces structured markdown files in an `ideas/` directory organized by market, so you build up a portfolio of research over time.

## Getting started

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- A Claude plan that supports tool use (WebSearch and WebFetch are used for market research)

### Install the plugin

Clone this repo and install it as a Claude Code plugin:

```bash
git clone https://github.com/yourusername/venture-alchemy.git
claude mcp add-plugin venture-alchemy ./venture-alchemy/venture-lab
```

Or add it manually to your Claude Code settings by pointing to the `venture-lab` directory.

### Run your first idea generation

Open Claude Code in any project directory and run:

```
/venture-lab:generate-idea
```

The skill will walk you through:

1. **Interview** (3-4 rounds) -- your technical skills, domain experience, interests, time, budget, goals, and network
2. **Market discovery** -- web research on 3-5 markets that fit your profile
3. **Idea generation** -- 2-3 ideas per market, filtered for originality and personalization
4. **Scoring** -- each idea scored 1-5 on Revenue Potential, Solo Buildability, Market Validation, and Personal Fit
5. **Output** -- markdown files saved to `ideas/{market}/` in your working directory

At the end you can choose to continue into validation or execution planning without leaving the session.

### Validate an idea

```
/venture-lab:validate-idea ideas/developer-tools/idea-001.md
```

Produces a validation report with SWOT analysis, competitor comparison, risk assessment, and a go/no-go recommendation.

### Plan execution

```
/venture-lab:plan-execution ideas/developer-tools/idea-001.md
```

Produces an execution plan with MVP scope, milestone breakdown, pricing, and a customer acquisition strategy designed for solo developers. The plan is structured to feed directly into `/create-spec` if you use [agent-alchemy](https://github.com/agent-alchemy) for spec-driven development.

## Output structure

All output lives in an `ideas/` directory at the root of wherever you run the commands:

```
ideas/
├── developer-tools/
│   ├── MARKET.md                        # Market analysis
│   ├── idea-001.md                      # Idea with scores
│   ├── idea-002.md
│   ├── validation/
│   │   └── idea-001-validation.md       # Validation report
│   └── execution-plans/
│       └── idea-001-plan.md             # Execution plan
└── healthcare-saas/
    ├── MARKET.md
    └── idea-001.md
```

## License

MIT
