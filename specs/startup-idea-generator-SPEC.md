# Startup Idea Generator PRD

**Version**: 1.0
**Author**: aprancl
**Date**: 2026-02-21
**Status**: Draft
**Spec Type**: New Product
**Spec Depth**: Detailed Specifications
**Description**: A Claude Code plugin that helps CC users generate startup and side-project ideas through market-first discovery, structured validation, and execution planning. Modeled after the agent-alchemy SDD tools workflow pattern.

---

## 1. Executive Summary

Startup Idea Generator is a Claude Code plugin that provides a structured, interview-driven workflow for discovering profitable startup and side-project ideas. It takes a market-first approach — identifying suitable markets before generating ideas — and uses web research, deep user profiling, and a formal scoring rubric to produce high-quality, evidence-backed ideas rather than generic "AI slop." The plugin consists of three skills (`generate-idea`, `validate-idea`, `plan-execution`) and a dedicated `market-researcher` agent, all following the agent-alchemy plugin architecture pattern.

## 2. Problem Statement

### 2.1 The Problem

Developers and aspiring entrepreneurs who have access to powerful AI-assisted development tools (like Claude Code with agent-alchemy) are fully capable of *building* software — but struggle to identify *what* to build. They fall into a cycle of starting projects based on initial enthusiasm, getting 70% through implementation, then losing confidence in the idea's value and abandoning it. There is no structured, evidence-based tool for market-first idea discovery tailored to solo developers.

### 2.2 Current State

Currently, developers looking for project ideas rely on:
- **Generic brainstorming with AI**: Produces "AI slop" — obvious, oversaturated ideas (yet another todo app, AI wrapper, etc.)
- **Browsing communities**: Reddit, Indie Hackers, Twitter — time-consuming and scattered
- **Personal inspiration**: Waiting for a "eureka moment" that may never come
- **Copying competitors**: Building clones without understanding market dynamics

None of these approaches provide structured market analysis, personal fit assessment, or evidence-backed validation.

### 2.3 Impact Analysis

Without a solution, developers continue to:
- Waste weeks/months building projects they abandon
- Lose confidence in their ability to identify profitable opportunities
- Miss actual market opportunities because they lack a systematic discovery process
- Underutilize powerful tools (CC + agent-alchemy) that could rapidly build validated ideas

### 2.4 Business Value

This plugin:
- Breaks the "build and abandon" cycle by providing confidence through evidence
- Creates a natural upstream complement to agent-alchemy SDD tools (ideas flow into specs flow into tasks flow into code)
- Serves a broad audience of developers looking to monetize their skills
- Establishes the venture-alchemy plugin as the entrepreneurial counterpart to agent-alchemy

## 3. Goals & Success Metrics

### 3.1 Primary Goals

1. Reliably generate actionable, non-generic startup/side-project ideas backed by market evidence
2. Provide a structured pipeline from market discovery through idea validation to execution planning
3. Give users confidence that they are building something with real revenue potential

### 3.2 Success Metrics

| Metric | Current Baseline | Target | Measurement Method | Timeline |
|--------|------------------|--------|-------------------|----------|
| Idea specificity | N/A (no tool) | Ideas reference specific markets, competitors, and pricing models | Manual review of generated ideas | MVP |
| Anti-slop effectiveness | N/A | <20% of ideas overlap with common "generic AI suggestions" | Compare output against curated generic idea list maintained in anti-slop-filters.md (e.g., todo apps, expense trackers, AI wrappers, weather apps, etc.) | MVP |
| Pipeline completion | N/A | User can go from /generate-idea to a concrete execution plan in one session | End-to-end workflow testing | Phase 3 |
| User confidence | Low (abandonment at 70%) | User proceeds to spec creation (via agent-alchemy) for generated ideas | Track pipeline handoff to /create-spec | Post-MVP |

### 3.3 Non-Goals

- This plugin does NOT write code or generate technical implementations
- This plugin does NOT replace agent-alchemy SDD tools — it feeds into them
- This plugin does NOT guarantee business success — it provides structured analysis to improve odds
- This plugin is NOT a real-time market data platform — it uses web search as a best-effort intelligence source

## 4. User Research

### 4.1 Target Users

#### Primary Persona: The Side-Project Developer
- **Role/Description**: A developer who codes professionally or as a hobby and wants to monetize side projects
- **Goals**: Find a profitable project idea that matches their skills and available time, and actually ship it
- **Pain Points**: Gets stuck in ideation loops, starts projects and abandons them, generates "AI slop" ideas when brainstorming with LLMs, lacks market awareness
- **Context**: Uses Claude Code as their primary development environment, has access to agent-alchemy tools for implementation

#### Secondary Persona: The Aspiring Technical Entrepreneur
- **Role/Description**: Someone with technical skills who wants to build a real startup but hasn't found the right opportunity
- **Goals**: Identify a market with real demand, build a validated MVP, and launch a product that generates revenue
- **Pain Points**: Doesn't know how to systematically evaluate markets, lacks confidence in idea selection, doesn't know who to talk to for validation
- **Context**: CC usage is implied by virtue of invoking the skill; may also use agent-alchemy for downstream implementation

#### Secondary Persona: The Freelance Developer
- **Role/Description**: Working developer between contracts looking for a product to build during downtime
- **Goals**: Create a recurring-revenue product that reduces dependence on contract work
- **Pain Points**: Limited time for market research, needs ideas that are buildable solo in weeks not months
- **Context**: CC usage is implied by virtue of invoking the skill; values speed and efficiency due to limited availability between contracts

### 4.2 User Journey Map

```
[Frustrated with current ideas] --> [Invoke /generate-idea] --> [Complete profile interview]
    --> [Review market analysis] --> [Score and rank ideas] --> [Select top idea]
    --> [/validate-idea for deeper analysis] --> [/plan-execution for roadmap]
    --> [Hand off to /create-spec via agent-alchemy] --> [Build with confidence]
```

**Typical Flow:**
1. User invokes `/generate-idea` from Claude Code
2. Interview-style Q&A gathers skills, experience, interests, network, constraints
3. Plugin identifies suitable markets via web research
4. Ideas are generated per market, scored, and ranked
5. User reviews ranked ideas with supporting evidence
6. User selects an idea to validate deeper (`/validate-idea`)
7. Validated idea gets an execution plan (`/plan-execution`)
8. Execution plan feeds into agent-alchemy `/create-spec` for implementation

## 5. Functional Requirements

### 5.1 Feature: generate-idea (Hub Skill)

**Priority**: P0 (Critical)

#### User Stories

**US-001**: As a developer looking for a side project, I want to go through a structured interview about my skills, interests, and constraints so that the tool can generate personalized, non-generic startup ideas.

**Acceptance Criteria**:
- [ ] Skill is invocable via `/generate-idea` slash command
- [ ] Interview gathers: technical skills, domain experience, available time, budget constraints, income goals, interests, risk tolerance
- [ ] Interview uses `AskUserQuestion` for all user interactions (no text-based prompts)
- [ ] Interview provides recommended answers/options for each question
- [ ] Interview completes within 3-4 rounds maximum, with adaptive depth based on user engagement
- [ ] User profile is stored internally and used to filter/personalize all subsequent idea generation
- [ ] If the user provides insufficient or contradictory profile data, the skill flags gaps and asks targeted follow-up questions rather than proceeding with incomplete information

**US-002**: As a user, I want the tool to identify markets suitable for me before generating ideas so that ideas are grounded in real market opportunities.

**Acceptance Criteria**:
- [ ] After profiling, the skill identifies 3-5 markets based on user profile and web research
- [ ] Each market is presented with: market description, size estimate, trend direction, and why it fits the user
- [ ] User can approve, reject, or suggest alternative markets
- [ ] Market analysis uses the `market-researcher` agent for web-based intelligence
- [ ] If web search returns insufficient data for a market, the skill flags it as "low-confidence" and presents available evidence with caveats

**US-003**: As a user, I want each generated idea to come with a structured document including market evidence so that I can make informed decisions.

**Acceptance Criteria**:
- [ ] Each idea is output as a markdown file in `ideas/{market-slug}/idea-NNN.md`
- [ ] Each market has a `MARKET.md` file with the market analysis
- [ ] Idea document includes: idea name, description, target customer, revenue model, competitive landscape, estimated time to MVP, personal fit assessment
- [ ] Idea document references specific competitors, pricing data points, and market evidence gathered via web research
- [ ] If market evidence is sparse or unavailable for an idea, the document clearly states what data is missing and marks the idea as "partially validated"

**US-004**: As a user, I want ideas to be scored and ranked using a transparent rubric so that I can compare them objectively.

**Acceptance Criteria**:
- [ ] Each idea is scored on 4 dimensions (1-5 scale):
  - Revenue Potential: market size, pricing model viability, willingness to pay
  - Solo Buildability: technical complexity, time to MVP, maintenance burden
  - Market Validation: evidence of demand, competitor gaps, trend alignment
  - Personal Fit: skills match, interest level, network advantage
- [ ] Composite score is a weighted average with equal default weights (25% each); user can adjust weights before scoring
- [ ] Ideas are presented in ranked order with scores visible
- [ ] Scoring rationale is included in each idea document

**US-005**: As a user, I want the tool to actively filter out generic, oversaturated ideas so that I don't get "AI slop."

**Acceptance Criteria**:
- [ ] Deep user profiling produces hyper-personalized ideas (not one-size-fits-all)
- [ ] Every idea cites specific market evidence (competitor gaps, underserved niches, pricing data)
- [ ] Contrarian filter rejects "obvious" ideas (generic SaaS, yet another X)
- [ ] Freshness filter uses web search to check market saturation; flags ideas with 10+ established competitors
- [ ] Ideas with high saturation are either filtered out or explicitly flagged with rationale
- [ ] If the freshness filter cannot determine saturation (e.g., web search fails or returns ambiguous results), the idea is presented with a "saturation unknown" flag rather than silently passed through

**US-006**: As a user, I want the tool to help me identify people to talk to in my chosen market so that I can validate ideas through human connections.

**Acceptance Criteria**:
- [ ] Interview asks about the user's existing network in target markets
- [ ] If user knows nobody in a market, the skill suggests outreach targets from relevant platforms (the market-researcher agent determines appropriate platforms dynamically, which may include subreddits, forums, Discord servers, Slack communities, industry events, newsletters, cold emails, and cold calls)
- [ ] Network suggestions are specific and actionable (not generic "join a community" advice)
- [ ] This feature is optional — user can skip it for any given idea

**Edge Cases**:
- User has no clear skills or interests: Provide broader exploration questions and suggest "skill-adjacent" markets
- Web search returns limited data for a niche market: Flag as "emerging/unvalidated market" rather than rejecting it
- User wants ideas outside their skill set: Present with explicit "learning curve" assessment added to scoring

---

### 5.2 Feature: validate-idea

**Priority**: P1 (High)

#### User Stories

**US-007**: As a user who has selected a promising idea, I want to deep-dive into its market viability so that I can decide whether to commit to building it.

**Acceptance Criteria**:
- [ ] Skill is invocable via `/validate-idea` and accepts an idea document path as input
- [ ] Performs deeper web research on: competitors, pricing models, customer acquisition channels, market trends
- [ ] Produces a validation report with: SWOT analysis, competitor comparison table, qualitative revenue model scenarios, key risks
- [ ] Validation output saved to `ideas/{market-slug}/validation/idea-NNN-validation.md`
- [ ] Presents a clear "go/no-go" recommendation with confidence level and reasoning
- [ ] If web research yields insufficient competitive data, the validation report explicitly states data gaps and adjusts confidence level accordingly

**US-008**: As a user, I want validation to challenge my idea rather than just confirm it so that I avoid confirmation bias.

**Acceptance Criteria**:
- [ ] Validation actively searches for reasons the idea might fail (devil's advocate approach)
- [ ] Identifies the top 3 risks to the idea's success
- [ ] If significant risks are found, suggests pivots or modifications that could address them
- [ ] Presents both bull case and bear case scenarios
- [ ] If the idea has no direct competitors to compare against, the validation pivots to demand signal analysis (search trends, adjacent markets, forum activity) and states the limitation

**Edge Cases**:
- Idea is in a brand-new market with no competitors: Validate demand signals (search trends, forum discussions, adjacent markets) rather than competitor analysis
- User provides an idea not generated by generate-idea: Validate it using the same rubric and research approach

---

### 5.3 Feature: plan-execution

**Priority**: P1 (High)

#### User Stories

**US-009**: As a user with a validated idea, I want a concrete execution plan so that I know exactly what to build and in what order.

**Acceptance Criteria**:
- [ ] Skill is invocable via `/plan-execution` and accepts an idea document (and optionally a validation report) as input
- [ ] Produces an execution plan with: MVP scope definition, feature prioritization, milestone breakdown, launch strategy
- [ ] Plan includes: revenue model implementation details, customer acquisition strategy for first 10/100/1000 users, pricing recommendations
- [ ] Output saved to `ideas/{market-slug}/execution-plans/idea-NNN-plan.md`
- [ ] Plan is structured to feed directly into agent-alchemy `/create-spec` for technical implementation

**US-010**: As a user, I want the execution plan to be realistic for a solo developer so that I don't end up with an overwhelming roadmap.

**Acceptance Criteria**:
- [ ] Plan accounts for the user's stated available time and constraints (from generate-idea profile)
- [ ] MVP scope is aggressively minimal — focused on proving revenue potential, not building a complete product
- [ ] Each milestone has clear completion criteria
- [ ] Plan includes "validation checkpoints" — points where the user should assess real-world feedback before continuing
- [ ] If the user's profile data (from generate-idea) is unavailable or incomplete, the skill prompts for key constraints (available time, budget) before generating the plan

**Edge Cases**:
- Idea requires skills the user doesn't have: Plan includes specific learning resources or suggests outsourcing specific components
- Market moves fast: Plan prioritizes speed to market over feature completeness

---

### 5.4 Feature: market-researcher Agent

**Priority**: P0 (Critical)

#### User Stories

**US-011**: As the generate-idea and validate-idea skills, I need a specialized research agent that can gather current market intelligence via web search.

**Acceptance Criteria**:
- [ ] Agent is defined as `agents/market-researcher.md` following agent-alchemy agent pattern
- [ ] Has access to: `WebSearch`, `WebFetch` tools
- [ ] Can be spawned by generate-idea and validate-idea via `Task` tool with `subagent_type: market-researcher`
- [ ] Returns structured research findings in a consistent format
- [ ] Research covers: competitor analysis, market size estimates, pricing data, trend analysis, community/outreach targets
- [ ] Includes source attribution for all findings
- [ ] If web search fails or returns no relevant results, the agent returns a structured "no data" response with the queries attempted, rather than failing silently

**Edge Cases**:
- Web search returns irrelevant results: Agent should filter and rank results by relevance, flagging low-confidence findings
- Rate limiting or search failures: Agent should gracefully degrade and report what it could/couldn't find

## 6. Non-Functional Requirements

### 6.1 Performance
- Interview rounds should respond within normal Claude Code interaction time (no artificial delays)
- Web research via market-researcher agent may take 30-60 seconds per query — user should be informed that research is in progress
- Idea generation for 3-5 markets with 2-3 ideas each should complete within approximately 20-40 minutes of active interaction, staying within a single CC context window (quality is prioritized over speed)

### 6.2 Security
- No sensitive user data is stored beyond the current session (idea documents are local markdown files)
- Web search queries should not include personally identifiable information about the user
- No external API keys required — uses CC's built-in web search capabilities

### 6.3 Scalability
- Not applicable — this is a single-user CC plugin, not a hosted service
- Idea documents are local files; no database or external storage required

### 6.4 Accessibility
- Not applicable — all interaction is via Claude Code terminal interface
- Markdown output files are inherently accessible

## 7. Technical Considerations

### 7.1 Architecture Overview

The plugin follows the agent-alchemy architecture pattern: skills defined via `SKILL.md` files with YAML frontmatter, agents defined via markdown files with tool whitelists, and reference files providing knowledge bases for each skill. The hub skill (`generate-idea`) can invoke the other skills as sub-steps, and all skills share the `market-researcher` agent.

### 7.2 Tech Stack

- **Plugin Format**: Claude Code plugin (SKILL.md + agents/ + references/)
- **Skill Logic**: Markdown-based workflow instructions (no code)
- **Output Format**: Structured markdown files
- **Market Intelligence**: CC built-in WebSearch and WebFetch tools
- **User Interaction**: AskUserQuestion tool (required for all user prompts)

### 7.3 Integration Points

| System | Integration Type | Purpose |
|--------|-----------------|---------|
| Claude Code | Plugin host | Provides the runtime environment, tool access, and slash command registration |
| WebSearch / WebFetch | Built-in tools | Market research, competitor analysis, trend data |
| Agent-alchemy SDD tools | Output handoff | Execution plans feed into `/create-spec` for technical implementation |
| AskUserQuestion | Built-in tool | All user interaction during interviews |
| Task tool | Built-in tool | Spawning market-researcher agent and invoking sub-skills |

### 7.4 Technical Constraints

- Plugin must follow CC plugin format (SKILL.md with valid frontmatter)
- All user interaction must use `AskUserQuestion` tool (no text-based prompts)
- Web search quality depends on CC's WebSearch capabilities and the LLM's ability to parse results
- Ideas are generated by the LLM based on user profile + market research — quality is bounded by model capability
- Plugin directory must be installable via CC plugin system

### 7.5 Plugin Directory Structure

**Versioning Convention**: Versions are tracked in `plugin.yaml` using semantic versioning (semver). Patch versions (0.1.x) for modifications to existing capability; minor versions (0.x.0) for adding smaller-scale capabilities; major versions (x.0.0) for adding larger capabilities or breaking changes.

```
venture-alchemy/
└── startup-idea-generator/
    ├── plugin.yaml                              # Plugin manifest (name, version, skills)
    ├── skills/
    │   ├── generate-idea/
    │   │   ├── SKILL.md                         # Hub skill - interview + idea generation
    │   │   └── references/
    │   │       ├── interview-questions.md        # User profiling question bank
    │   │       ├── market-categories.md          # Market taxonomy and discovery patterns
    │   │       ├── scoring-rubric.md             # Formal 4-dimension scoring criteria
    │   │       ├── anti-slop-filters.md          # Contrarian + freshness filter logic
    │   │       └── templates/
    │   │           ├── idea-document.md           # Template for idea output files
    │   │           └── market-analysis.md         # Template for MARKET.md files
    │   ├── validate-idea/
    │   │   ├── SKILL.md                          # Deep validation skill
    │   │   └── references/
    │   │       ├── validation-criteria.md         # SWOT, competitor analysis patterns
    │   │       └── templates/
    │   │           └── validation-report.md       # Template for validation output
    │   └── plan-execution/
    │       ├── SKILL.md                          # Execution planning skill
    │       └── references/
    │           ├── planning-patterns.md           # MVP scoping, milestone patterns
    │           └── templates/
    │               └── execution-plan.md          # Template for plan output
    ├── agents/
    │   └── market-researcher.md                  # Web research agent definition
    ├── references/                               # Shared plugin-level references
    │   └── idea-quality-principles.md            # Cross-skill quality standards
    └── README.md                                 # Plugin overview and usage
```

### 7.6 Output Directory Structure

```
ideas/
├── {market-slug}/
│   ├── MARKET.md                    # Market analysis document
│   ├── idea-001.md                  # Individual idea document
│   ├── idea-002.md                  # Another idea in this market
│   └── validation/
│       └── idea-001-validation.md   # Validation report for idea-001
├── {another-market-slug}/
│   ├── MARKET.md
│   ├── idea-001.md
│   └── execution-plans/
│       └── idea-001-plan.md         # Execution plan (market-grouped)
```

## 8. Scope Definition

### 8.1 In Scope

- Interview-based user profiling skill (`/generate-idea`)
- Market discovery via web research
- Idea generation with formal scoring rubric (Revenue Potential, Solo Buildability, Market Validation, Personal Fit)
- Three-layer anti-slop filtering (deep profiling + market evidence + contrarian/freshness filter)
- Network/outreach suggestions for chosen markets
- Idea validation skill (`/validate-idea`) with devil's advocate approach
- Execution planning skill (`/plan-execution`) with solo-developer-appropriate scope
- Market-researcher agent for web-based intelligence
- Structured markdown output documents
- Plugin structure mirroring agent-alchemy pattern

### 8.2 Out of Scope

- **Code implementation**: The plugin does not write code — use agent-alchemy SDD tools for that
- **Real-time market data feeds**: Uses web search as best-effort intelligence, not live APIs
- **Multi-user features**: Personal-use plugin only, no collaboration or sharing
- **Financial projections**: Revenue model suggestions are qualitative, not quantitative financial models
- **Legal/compliance advice**: Plugin may mention regulatory considerations but does not provide legal guidance
- **Hosting/deployment**: This is a local CC plugin, not a hosted service

### 8.3 Future Considerations

- **Idea tracking/portfolio**: Track which ideas were generated, validated, and acted upon over time
- **Feedback loop**: After building and launching an idea, capture real-world results to improve future idea generation
- **Market monitoring**: Periodic re-check of market conditions for ideas in progress
- **Community sharing**: Option to share anonymized idea templates or market analyses with other users
- **Integration with other venture-alchemy plugins**: As more plugins are built, create a unified entrepreneurial workflow

## 9. Implementation Plan

### 9.1 Phase 1: Foundation — Plugin Structure + generate-idea Core

**Completion Criteria**: User can invoke `/generate-idea`, complete the interview, and receive a ranked list of ideas with market context as markdown files.

| Deliverable | Description | Dependencies |
|-------------|-------------|--------------|
| Plugin directory structure | Create the full plugin directory matching agent-alchemy pattern | None |
| generate-idea SKILL.md | Core skill definition with interview workflow, user profiling, and idea generation logic | Plugin structure |
| Interview question bank | `references/interview-questions.md` — questions for user profiling (skills, interests, constraints, network, goals) | None |
| Market categories reference | `references/market-categories.md` — taxonomy of markets and discovery patterns | None |
| Scoring rubric reference | `references/scoring-rubric.md` — formal 4-dimension scoring criteria and weight system | None |
| Idea document template | `references/templates/idea-document.md` — structure for idea output files | None |
| Market analysis template | `references/templates/market-analysis.md` — structure for MARKET.md files | None |
| market-researcher agent | `agents/market-researcher.md` — web research agent for market intelligence | None |
| Anti-slop filters reference | `references/anti-slop-filters.md` — contrarian filter logic, freshness check patterns, and curated generic idea list | None |
| Freshness filter integration | Web search-based saturation check integrated into generate-idea workflow | market-researcher agent |
| Network/outreach suggestions | Community and outreach target identification integrated into generate-idea | market-researcher agent |
| README.md | Plugin overview, usage instructions, skill descriptions | All above |

**Checkpoint Gate**: Test generate-idea end-to-end — verify that the interview produces personalized, non-generic ideas with market evidence. Verify anti-slop filters meaningfully reject generic ideas. Review idea documents for quality and completeness.

---

### 9.2 Phase 2: Core Features — validate-idea

**Completion Criteria**: User can validate a generated idea with deep market analysis, receiving a go/no-go recommendation with evidence.

| Deliverable | Description | Dependencies |
|-------------|-------------|--------------|
| validate-idea SKILL.md | Validation skill with SWOT analysis, competitor deep-dive, and go/no-go recommendation | Phase 1 (idea document format) |
| Validation criteria reference | `references/validation-criteria.md` — patterns for SWOT, competitor analysis, risk assessment | None |
| Validation report template | `references/templates/validation-report.md` — structure for validation output | None |

**Checkpoint Gate**: Test validate-idea produces actionable go/no-go recommendations with evidence. Verify devil's advocate approach surfaces meaningful risks.

---

### 9.3 Phase 3: Enhancement — plan-execution + Pipeline Integration

**Completion Criteria**: User can go from `/generate-idea` through validation to a concrete execution plan ready to hand off to agent-alchemy `/create-spec`.

| Deliverable | Description | Dependencies |
|-------------|-------------|--------------|
| plan-execution SKILL.md | Execution planning skill with MVP scope, milestones, launch strategy, and validation checkpoints | Phase 1 (idea format), Phase 2 (validation format) |
| Planning patterns reference | `references/planning-patterns.md` — MVP scoping patterns, milestone templates, solo-dev constraints | None |
| Execution plan template | `references/templates/execution-plan.md` — structure for execution plan output | None |
| Hub model integration | generate-idea can optionally invoke validate-idea and plan-execution as sub-steps | All skills |
| Idea quality principles | `references/idea-quality-principles.md` — cross-skill quality standards and anti-slop philosophy | All phases |
| Agent-alchemy handoff | Execution plan output is formatted to feed directly into `/create-spec` | plan-execution skill |

**Checkpoint Gate**: Test full pipeline end-to-end: generate → validate → plan → verify plan is usable as input for `/create-spec`.

## 10. Dependencies

### 10.1 Technical Dependencies

| Dependency | Owner | Status | Risk if Delayed |
|------------|-------|--------|-----------------|
| Claude Code plugin system | Anthropic | Available | Blocking — plugin format must be supported |
| WebSearch / WebFetch tools | Anthropic (CC) | Available | Degrades market research quality if unavailable |
| Agent-alchemy SDD tools | sequenzia | Available (installed) | Non-blocking — only needed for downstream handoff |
| AskUserQuestion tool | Anthropic (CC) | Available | Blocking — required for all user interaction |

### 10.2 Cross-Team Dependencies

| Team | Dependency | Status |
|------|------------|--------|
| None | This is a personal project with no external team dependencies | N/A |

## 11. Risks & Mitigations

| Risk | Impact | Likelihood | Mitigation Strategy | Owner |
|------|--------|------------|---------------------|-------|
| Generated ideas are generic "AI slop" | High | Medium | Three-layer anti-slop: deep profiling + market evidence + contrarian/freshness filter | generate-idea skill |
| Web search returns low-quality market data | Medium | Medium | Agent flags low-confidence findings; skill presents uncertainty transparently | market-researcher agent |
| Plugin structure is incorrect or doesn't register | High | Low | Mirror agent-alchemy exactly; test installation early | Phase 1 |
| Interview is too long or tedious | Medium | Medium | Adaptive interview depth; allow early exit; keep to 3-4 rounds max | generate-idea skill |
| Ideas look good on paper but fail in practice | High | Medium | Validation skill with devil's advocate approach; network outreach for real-world feedback | validate-idea skill |
| Scope creep during development | Medium | High | Strict phase boundaries; MVP focuses on generate-idea only; iterate from there | All phases |

## 12. Open Questions

| # | Question | Owner | Due Date | Resolution |
|---|----------|-------|----------|------------|
| 1 | How should the plugin be installed/distributed? (Local install, git clone, or CC plugin registry?) | aprancl | Phase 1 | TBD |
| 2 | Should idea documents include visual elements (charts, diagrams) or stay text-only for CC compatibility? | aprancl | Phase 1 | TBD |
| 3 | How frequently should the freshness filter re-check market saturation for previously generated ideas? | aprancl | Phase 2 | TBD |
| 4 | Should there be a "portfolio" view that tracks all generated ideas across sessions? | aprancl | Post-MVP | TBD |

## 13. Appendix

### 13.1 Glossary

| Term | Definition |
|------|------------|
| CC | Claude Code — Anthropic's CLI for Claude, the runtime environment for this plugin |
| Skill | A Claude Code plugin component defined by a SKILL.md file that becomes a slash command |
| Agent | A specialized Claude instance spawned via the Task tool with specific tools and capabilities |
| Anti-slop | Techniques to prevent generic, oversaturated, or impractical ideas from being presented |
| Freshness filter | Web search-based check for market saturation before presenting an idea |
| Contrarian filter | Logic that rejects "obvious" ideas and favors non-obvious opportunities |
| Scoring rubric | Formal 4-dimension (Revenue, Buildability, Validation, Fit) scoring system with weighted composite |
| Hub model | Architecture where generate-idea serves as the main entry point and can invoke other skills as sub-steps |
| Agent-alchemy | An existing CC plugin ecosystem for spec-driven development (create-spec, create-tasks, execute-tasks) |
| Market-first approach | Identifying suitable markets before generating ideas, rather than brainstorming ideas in a vacuum |

### 13.2 References

- [Agent Alchemy SDD Tools](https://github.com/sequenzia/agent-alchemy/tree/main/claude/sdd-tools/skills) — Source of inspiration and architectural pattern
- Claude Code Plugin Documentation — Plugin format, SKILL.md specification, agent definition
- Agent-alchemy plugin cache at `~/.claude/plugins/cache/agent-alchemy/` — Reference implementation

### 13.3 Agent Recommendations (Accepted)

*The following recommendations were suggested based on industry best practices and accepted during the interview:*

1. **Output Structure**: Organized directory layout (`ideas/{market-slug}/`) grouping markets with their ideas and validation documents
   - Rationale: Mirrors agent-alchemy's file-based chaining pattern and keeps market context grouped with its ideas
   - Applies to: All skills (output format)

2. **Scoring Rubric**: Formal 1-5 scoring across 4 dimensions (Revenue Potential, Solo Buildability, Market Validation, Personal Fit) with weighted composite
   - Rationale: Makes idea ranking transparent, repeatable, and user-adjustable rather than subjective
   - Applies to: generate-idea skill (idea ranking)

3. **Market Researcher Agent**: Dedicated web research agent shared by generate-idea and validate-idea
   - Rationale: Centralizes research logic, ensures consistent evidence-gathering, and elevates ideas beyond pure LLM knowledge
   - Applies to: generate-idea, validate-idea (market intelligence)

4. **Idea Freshness Filter**: Active market saturation verification via web search before presenting ideas
   - Rationale: The contrarian filter in action — instead of just avoiding obvious ideas by prompt engineering, actively verifies market saturation with real data
   - Applies to: generate-idea skill (anti-slop pipeline)

---

*Document generated by SDD Tools — Create Spec Skill*
