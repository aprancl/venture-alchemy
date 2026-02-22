---
name: generate-idea
description: |
  Interview-driven startup and side-project idea generator.
  Uses market-first approach with deep user profiling, web research,
  and anti-slop filtering to generate evidence-backed ideas.
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - AskUserQuestion
  - Task
  - Read
  - Write
  - Glob
  - Grep
model: opus
---

# Generate Idea Skill

You are initiating the idea generation workflow. This process uses a market-first approach to discover personalized, evidence-backed startup and side-project ideas through structured user profiling, web research, and rigorous anti-slop filtering.

## Critical Rules

### AskUserQuestion is MANDATORY

**IMPORTANT**: You MUST use the `AskUserQuestion` tool for ALL questions to the user. Never ask questions through regular text output.

- Every interview question -> AskUserQuestion
- Market approval/rejection -> AskUserQuestion
- Weight selection -> AskUserQuestion
- Confirmation prompts -> AskUserQuestion
- Yes/no choices -> AskUserQuestion

Text output should only be used for:
- Summarizing what you have learned
- Presenting market analysis results
- Presenting ranked ideas with scores
- Explaining context or rationale

If you need the user to make a choice or provide input, use AskUserQuestion.

**NEVER do this** (asking via text output):
```
Which markets interest you?
1. Developer Tools
2. Healthcare SaaS
3. E-commerce
```

**ALWAYS do this** (using AskUserQuestion tool):
```yaml
AskUserQuestion:
  questions:
    - header: "Market Selection"
      question: "Which of these markets would you like to explore for idea generation?"
      options:
        - label: "Developer Tools"
          description: "SaaS products for software teams"
        - label: "Healthcare SaaS"
          description: "Software for healthcare providers"
        - label: "E-commerce"
          description: "Online retail and digital goods"
      multiSelect: true
```

### Plan Mode Behavior

**CRITICAL**: This skill generates ideas and market analysis, NOT an implementation plan. When invoked during Claude Code's plan mode:

- **DO NOT** create an implementation plan for how to build the skill itself
- **DO NOT** defer idea generation to an "execution phase"
- **DO** proceed with the full interview and idea generation workflow immediately
- **DO** write the idea and market files to the output paths as normal

Generating ideas IS the planning activity.

---

## Workflow Overview

This workflow has eleven phases:

1. **Settings Check** -- Load user configuration
2. **User Profiling Interview** -- 3-4 round interview gathering skills, experience, interests, constraints, and network
3. **Market Discovery** -- Identify 3-5 markets via web research based on user profile
4. **Market Presentation** -- Present markets for user approval, rejection, or alternatives
5. **Idea Generation** -- Generate 2-3 ideas per approved market
6. **Anti-Slop Pipeline** -- Apply personalization, contrarian, and freshness filters
7. **Scoring & Ranking** -- Score each idea on 4 dimensions and rank by composite score
8. **Network/Outreach** -- Optional: suggest outreach targets for chosen markets
9. **Output Generation** -- Write MARKET.md and idea-NNN.md files
10. **Results Presentation** -- Present ranked ideas with scores to the user
11. **Optional Sub-Skill Invocation (Hub Model)** -- Offer to invoke validate-idea and/or plan-execution as sub-steps, with full pipeline support

---

## Phase 1: Settings Check

Check if there is a settings file at `.claude/agent-alchemy.local.md` to get any custom configuration like output path or author name. If no settings file exists, proceed with defaults.

---

## Phase 2: User Profiling Interview

### Prepare for Interview

Before starting Round 1, read the interview question bank to load all questions, adaptive behaviors, and profile output structure:

```
Read: references/interview-questions.md
```

Use this as your primary source for questions throughout the interview.

### Interview Strategy

The interview is organized into 3-4 adaptive rounds. Each round groups related categories to create a natural conversational flow. The number of rounds adapts based on user engagement.

| Round | Categories Covered | Purpose |
|-------|-------------------|---------|
| **Round 1** | Technical Skills, Domain Experience, Interests & Passions | Understand who the user is and what they can build |
| **Round 2** | Available Time, Budget Constraints, Income Goals | Understand practical constraints and ambitions |
| **Round 3** | Risk Tolerance, Network | Understand market positioning and go-to-market capacity |
| **Round 4** (adaptive) | Follow-ups on gaps, contradictions, or areas needing depth | Fill in missing profile data; only triggered if needed |

### Round Execution Rules

Each round MUST:
1. Summarize what you have learned so far (briefly) -- use text output
2. Ask 2-3 focused questions using `AskUserQuestion` -- REQUIRED, never use text for questions
3. Provide 2-4 recommended answer options per question
4. Include an escape hatch option ("Skip," "Other," or "Let me describe") for flexibility
5. Use a mix of multiple choice (for structured data) and open text (for details)
6. Acknowledge responses before moving to the next round

### Interview Categories

The interview must gather data across these 8 categories:

1. **Technical Skills** -- What the user can build (languages, frameworks, experience level)
2. **Domain Experience** -- Industries with insider knowledge (professional background, depth)
3. **Interests & Passions** -- What motivates beyond money (topics, personal pain points)
4. **Available Time** -- Realistic build capacity (hours/week, timeline expectations)
5. **Budget Constraints** -- Financial limits (monthly investment, validation budget)
6. **Income Goals** -- Revenue targets (amount, income type preference)
7. **Risk Tolerance** -- Comfort with uncertainty (competition, pivoting willingness)
8. **Network** -- Existing connections (industry contacts, communities, audience)

See `references/interview-questions.md` for the full question bank with specific questions, recommended options, follow-up logic, and adaptive behavior rules for each category.

### Adaptive Behaviors

**When user has no clear skills or interests:**
- Use the alternative skills exploration question (Q1.1-ALT) from the question bank
- Shift emphasis to interests and personal pain points over technical depth
- Suggest skill-adjacent markets and simpler build approaches (no-code, templates, content)
- Adjust scoring weights to favor Solo Buildability

**When user skips categories:**
- Essential categories (Technical Skills, Available Time): Never skip
- Important categories (Domain Experience, Income Goals): Encourage but allow skip
- Optional categories (Budget details, Risk Tolerance, Community Presence): Skip freely
- When skipped, use neutral/default assumptions as defined in the question bank

**When contradictions are detected:**
- Do not immediately challenge -- wait until Round 4
- Frame as clarifications, not corrections
- Common contradictions: no budget + high income goals, low time + fast timeline, no skills + complex interests
- Present options that let the user self-correct without feeling judged

**When answers are insufficient or vague:**
- Allow one follow-up attempt for clarification
- If still vague, accept and move on
- Mark dimensions with insufficient data in the profile
- Generate a wider range of ideas across more markets rather than deeply personalized ideas in few markets

**When user wants to wrap up early:**
- Acknowledge the request
- Summarize what has been gathered
- Use `AskUserQuestion` to confirm proceeding with partial data
- Flag gaps in the profile that may affect idea quality

### Round 4 Triggers

Round 4 is only conducted if any of the following conditions are met:
- User skipped 2+ categories in earlier rounds
- Contradictory answers detected (e.g., no budget + high income goals)
- Insufficient data to generate personalized ideas (e.g., no skills AND no interests identified)

If none of these conditions are met, proceed directly to Phase 3.

### Profile Construction

After the interview completes, organize gathered data into this internal profile structure for use by all subsequent phases:

```
User Profile:
- Technical Skills: [languages, frameworks, experience level]
- Domain Experience: [industries, depth of knowledge]
- Interests: [topics, personal pain points, passions]
- Available Time: [hours/week, timeline, full-time vs part-time]
- Budget: [monthly investment capacity, validation budget]
- Income Goals: [target revenue, income type preference]
- Risk Tolerance: [competition comfort, pivot willingness]
- Network: [industry connections, online communities, audience]
- Personalization Anchor: [the unique differentiator identified]
- Profile Gaps: [categories with insufficient data]
- Contradictions Resolved: [any contradictions detected and how they were resolved]
```

This profile feeds directly into market identification, idea generation, and idea scoring.

---

## Phase 3: Market Discovery

### Load Market Reference

Read the market taxonomy, discovery patterns, presentation format, and selection criteria:

```
Read: references/market-categories.md
```

### Identify Candidate Markets

Using the user's profile and the market categories reference, identify candidate markets by applying all applicable discovery patterns:

1. **Skill-to-Market Mapping** -- Map technical skills to markets where those skills provide a building advantage
2. **Experience-to-Market Mapping** -- Map domain experience to markets where insider knowledge is an advantage
3. **Interest-to-Market Mapping** -- Map personal interests to markets where sustained motivation matters
4. **Network-to-Market Mapping** -- Map existing connections to markets where distribution is easier
5. **Constraint-Based Filtering** -- Filter candidates based on time, budget, income goals, and risk tolerance

Intersect results from all applicable patterns to find the strongest market fits. This typically produces 8-15 candidate markets.

### Conduct Web Research

Spawn the `market-researcher` agent via the `Task` tool to research the top candidate markets:

```
Task prompt template:
"Research the market for {market_name} with focus on: competitor landscape,
market size estimation, pricing patterns, trend analysis, and community targets.

User context: {relevant profile attributes that connect to this market}

Return structured findings in the standard market research output format."

subagent_type: market-researcher
```

Use the market-researcher agent to gather current web-based intelligence on each candidate market. See `../../agents/market-researcher.md` for the agent's full capabilities and output format.

### Select Top Markets

Narrow from 8-15 candidates to 3-5 markets using the selection algorithm from `references/market-categories.md`:

1. Score each candidate on personal fit (5 dimensions: skill relevance, domain knowledge, network access, interest alignment, constraint compatibility)
2. Assess accessibility for solo developers (technical complexity, regulatory burden, cold start, distribution, defensibility, capital requirements)
3. Evaluate revenue potential indicators (willingness to pay, customer LTV, pricing power, path to $1K MRR, expansion revenue)
4. Rank by composite assessment: primary sort by personal fit score, tiebreaker by accessibility, secondary tiebreaker by revenue potential
5. Select top 3-5 ensuring diversity (at least one market from a different category than top-ranked), network advantage inclusion, and minimum fit threshold (2.5)

### Handle Edge Cases

- **User profile does not map clearly**: Broaden the search to skill-adjacent markets. Lead with constraints as the primary filter. Present a diverse selection with exploration framing.
- **Niche markets with limited data**: Flag as "emerging/unvalidated" in the market presentation. Include the market if fit signals are strong, but note the higher uncertainty.
- **Web search returns insufficient data**: Flag as "low-confidence" and present available evidence with caveats. Do not reject a market solely due to limited web data.

---

## Phase 4: Market Presentation

Present each of the 3-5 selected markets to the user using the presentation template from `references/market-categories.md`. Each market must include:

- **Description**: 2-3 sentence overview of the market, problems solved, and customers served
- **Market Size Estimates**: TAM, SAM, SOM with qualitative descriptors (Large, Mid-size, Niche, Small) and source reasoning
- **Trend Direction**: Growing, Stable, or Declining with evidence
- **Why This Fits You**: Specific connections to the user's skills, experience, network, and interests
- **Example Opportunities**: 2-3 specific ideas or gaps identified in this market
- **Confidence Level**: High, Medium, or Low based on quality of market data available

### Market Approval

After presenting all markets, use `AskUserQuestion` to let the user approve, reject, or suggest alternatives:

```yaml
AskUserQuestion:
  questions:
    - header: "Market Selection"
      question: "Which of these markets would you like me to generate ideas for? You can select multiple, reject any, or suggest alternatives."
      options:
        - label: "{Market 1 name}"
          description: "{Brief fit summary}"
        - label: "{Market 2 name}"
          description: "{Brief fit summary}"
        - label: "{Market 3 name}"
          description: "{Brief fit summary}"
        - label: "Suggest a different market"
          description: "I have a market in mind that wasn't listed"
      multiSelect: true
```

If the user suggests an alternative market:
1. Use `AskUserQuestion` to gather details about the suggested market
2. Research it via the market-researcher agent
3. Add it to the approved market list
4. Continue with idea generation for all approved markets

If the user rejects all markets:
1. Ask for guidance on what they are looking for
2. Broaden the search using alternative discovery patterns
3. Re-present a new set of markets

---

## Phase 5: Idea Generation

For each approved market, generate 2-3 startup/side-project ideas that are:

- **Personalized**: Leverage the user's specific skills, experience, network, or interests
- **Problem-first**: Start from a specific pain point, not a technology
- **Evidence-backed**: Grounded in market research findings from Phase 3
- **Actionable**: Scoped to something a solo developer can realistically build

### Generation Approach

Follow the personalization-driven generation approach from the anti-slop filters:

1. **Start with the user's strongest attributes**: Identify the intersection of their best skills, deepest domain experience, and strongest network connections
2. **Identify problems in those intersection spaces**: What pain points exist in markets where the user has an unfair advantage?
3. **Generate ideas that require the user's profile**: The ultimate test: "Would this exact idea have been generated for a user with a completely different profile?" If yes, it is not personalized enough.

### Ideas Outside User's Skill Set

If any generated idea requires skills the user does not currently have:
- Present with an explicit "learning curve" assessment
- Estimate the additional learning time required
- Suggest whether the gap is bridgeable (weeks of learning) or significant (months of study)
- Factor the learning curve into the Solo Buildability score

---

## Phase 6: Anti-Slop Pipeline

### Load Anti-Slop Filters

Read the complete anti-slop filter definitions:

```
Read: references/anti-slop-filters.md
```

### Apply Three-Layer Pipeline

Every generated idea must pass through all three layers in order. Ideas are never silently passed through -- each must be explicitly checked.

#### Layer 1: Deep Personalization Filter

Cross-reference each idea against the user profile:
- Every idea must connect to **at least two** substantive profile attributes
- Connections must be specific and substantive (not generic like "this can be built with code")
- **Reject** ideas with 0-1 substantive connections (Weak personalization tier)
- **Pass with caveats** ideas with 2-3 connections (Moderate tier)
- **Pass with confidence** ideas with 4+ connections (Strong tier)

#### Layer 2: Contrarian Filter

Evaluate each idea against the slop list and generic/contrarian signal framework:
- Check against the 35-item curated slop list
- Count generic signals (no niche, no differentiation, commonly generated pattern, technology-first, no profile leverage, generic SaaS pattern)
- Count contrarian signals (underserved niche, unique angle, non-obvious market, unfair advantage, problem-first, counter-trend, cross-domain)
- Apply decision matrix:
  - 0-1 generic + 1+ contrarian signals -> **PASS**
  - 2 generic + 2+ contrarian signals -> **PASS with caution**
  - 2 generic + 0-1 contrarian signals -> **REJECT** (attempt refinement first)
  - 3+ generic signals -> **REJECT**

**Refinement before rejection**: Before fully rejecting an idea, attempt one refinement pass. Cross-reference the user's profile for a possible niche, angle, or advantage. If a refinement emerges that clears the filter, present the refined version instead.

**Novel Angle Exception**: Ideas matching slop list entries by category but with a genuinely novel angle (specific niche, unique mechanism, or unfair advantage) may proceed. They must carry a "Caution: Adjacent to saturated market" flag.

#### Layer 3: Freshness Filter

Use the `market-researcher` agent to verify market saturation via web search:

1. Execute search queries from the templates in `references/anti-slop-filters.md` (competitor count queries, market saturation signal queries, demand signal queries)
2. Count established competitors (functional website/app, active users, operating 6+ months)
3. Assign saturation flags based on thresholds:
   - 0-2 competitors -> **Low** (check demand signals -- could mean no market)
   - 3-5 competitors -> **Moderate** (healthy validation with room)
   - 6-9 competitors -> **Moderate** (upper range, differentiation needed)
   - 10-14 competitors -> **High** (flag in document, present only with strong differentiation)
   - 15+ competitors -> **Reject** (unless Novel Angle Exception + unfair advantage)

**When freshness filter cannot determine saturation** (web search fails, returns ambiguous results, or is inconclusive):
- Flag the idea with **Saturation Flag: Unknown**
- Do NOT silently pass the idea as if it were checked
- Do NOT default to "Low competition" when data is unclear
- Present the idea with the "saturation unknown" flag
- Recommend `/validate-idea` for deeper research

**Performance optimization**: Batch freshness checks for ideas in similar markets. Reuse search results from Phase 3 market research when applicable. Cap at 5-7 queries per idea. If saturation is clear from 2-3 queries, skip remaining queries.

---

## Phase 7: Scoring & Ranking

### Load Scoring Rubric

Read the scoring system definition:

```
Read: references/scoring-rubric.md
```

### Weight Selection

Before scoring begins, present the weight options to the user via `AskUserQuestion`:

```yaml
AskUserQuestion:
  questions:
    - header: "Scoring Weights"
      question: "How would you like to weight the scoring dimensions? Choose a preset or specify custom weights."
      options:
        - label: "Balanced (default)"
          description: "25% Revenue / 25% Buildability / 25% Validation / 25% Fit"
        - label: "Revenue-focused"
          description: "40% Revenue / 15% Buildability / 30% Validation / 15% Fit"
        - label: "Quick-win"
          description: "15% Revenue / 40% Buildability / 20% Validation / 25% Fit"
        - label: "Passion-project"
          description: "15% Revenue / 20% Buildability / 20% Validation / 45% Fit"
        - label: "Evidence-driven"
          description: "20% Revenue / 15% Buildability / 45% Validation / 20% Fit"
        - label: "Custom weights"
          description: "I want to set my own weights"
      multiSelect: false
```

If the user selects "Custom weights," follow up with a second `AskUserQuestion` asking for the four weight values (must sum to 100%).

### Score Each Idea

Score each surviving idea across all 4 dimensions (1-5 scale):

1. **Revenue Potential** -- Market size, pricing model viability, willingness to pay, revenue ceiling
2. **Solo Buildability** -- Technical complexity, time to MVP, maintenance burden, infrastructure needs
3. **Market Validation** -- Evidence of demand, competitor gaps, trend alignment, data confidence
4. **Personal Fit** -- Skills match, interest alignment, network advantage, constraint compatibility

Use the detailed scoring indicators from `references/scoring-rubric.md` to assign each score.

### Scoring Rationale Requirements

Each score MUST include a 2-3 sentence rationale that:
- References **specific evidence** (not generic statements like "good market")
- Cites **sources** where applicable (competitor names, trend data, community URLs)
- Explains the **reasoning** mapping evidence to the chosen score level
- Notes any **uncertainty** or assumptions

### Insufficient Data Handling

When a dimension cannot be scored due to insufficient information:
1. Assign a default score of 3
2. Flag with an `[insufficient data]` marker
3. Document what data was missing in the rationale
4. Inform the user that the composite score may be less reliable

### Composite Score Calculation

```
Composite Score = (W_r * S_r) + (W_b * S_b) + (W_v * S_v) + (W_f * S_f)
```

Score range: 1.00 to 5.00

### Ranking

Rank all ideas by composite score (descending). Apply tiebreaker rules in order:
1. Higher Market Validation score wins
2. Higher Revenue Potential score wins
3. Higher Personal Fit score wins
4. Higher Solo Buildability score wins
5. If still tied, present as equally ranked and let the user decide

---

## Phase 8: Network/Outreach (Optional)

This phase is optional. Use `AskUserQuestion` to ask if the user wants network and outreach suggestions:

```yaml
AskUserQuestion:
  questions:
    - header: "Network & Outreach"
      question: "Would you like me to suggest people and communities to connect with for your top ideas?"
      options:
        - label: "Yes, for all ideas"
          description: "Get outreach suggestions for every idea"
        - label: "Yes, for my top pick only"
          description: "Just for the idea I'm most interested in"
        - label: "Skip this step"
          description: "I'll figure out outreach on my own"
      multiSelect: false
```

If the user opts in:

1. Review the user's stated network from the interview (Q8.1 and Q8.2)
2. If user knows nobody in the target market, use the `market-researcher` agent to find outreach targets
3. Search for specific, actionable targets -- the market-researcher agent determines appropriate platforms dynamically, which may include:
   - Relevant subreddits and forums
   - Discord servers and Slack communities
   - Industry events and conferences
   - Newsletters and podcasts
   - Cold email and cold call targets (specific companies or roles)
4. Suggestions must be **specific and actionable** -- not generic "join a community" advice
5. Include outreach targets in the idea documents' Network & Outreach section

If the user skips, leave the Network & Outreach section of idea documents with a note that the section was skipped.

---

## Phase 9: Output Generation

### Load Templates

Read the templates for output files:

```
Read: references/templates/market-analysis.md
Read: references/templates/idea-document.md
```

### Create Output Directory Structure

For each approved market, create the output directory and files:

```
ideas/
  {market-slug}/
    MARKET.md              # Market analysis document
    idea-001.md            # First idea in this market
    idea-002.md            # Second idea in this market
    idea-003.md            # Third idea (if generated)
```

### Write MARKET.md Files

For each approved market, write a `MARKET.md` file using the `references/templates/market-analysis.md` template. Populate with:

- Market description, category, and slug
- Market size estimates (TAM/SAM/SOM) using qualitative descriptors
- Trend direction with evidence and source attribution
- Key players and competitive landscape
- Underserved niches identified during research
- Why this market fits the user's profile (skill, experience, interest, network, constraint matches)
- Sources and attribution for all factual claims
- Confidence level assessment

Include conditional notice blocks when applicable:
- **Low-Confidence Market Notice** -- when confidence is Low
- **Emerging/Unvalidated Market Notice** -- when market is less than 3 years old or lacks established players

### Write idea-NNN.md Files

For each idea, write an `idea-NNN.md` file using the `references/templates/idea-document.md` template. Populate with:

- Idea name, one-line summary, market link, validation status, saturation flag
- Problem statement with specific evidence
- Target customer with demographics, current behavior, willingness to pay, where to find them
- Revenue model with pricing approach, price point, willingness-to-pay indicators, revenue ceiling, monetization timeline
- Competitive landscape table with specific competitors, pricing, and gaps
- Market evidence with source attribution (minimum 2 evidence points for "Validated" status)
- Saturation assessment with competitor count and assessment basis
- Time to MVP with core scope, key technical components, and biggest build risk
- Personal fit with skills match, interest alignment, network advantage, constraint compatibility
- Full scoring section with dimension scores, rationales, composite score, weights, and rank
- Network & outreach suggestions (if Phase 8 was not skipped)

Include conditional blocks when applicable:
- **Data gaps** -- when market evidence is sparse
- **Partially Validated** -- when fewer than 2 evidence points with source attribution
- **Saturation Unknown** -- when freshness filter could not determine saturation

### File Naming Conventions

- **Market slug**: Lowercase, hyphenated version of market name (e.g., "developer-tools", "healthcare-saas")
- **Idea numbering**: Sequential within each market, starting at 001 (e.g., idea-001.md, idea-002.md)
- **Date format**: YYYY-MM-DD (ISO 8601)

---

## Phase 10: Results Presentation

Present the complete ranked list of ideas to the user. Include for each idea:

1. **Rank** and **idea name**
2. **Market** it belongs to
3. **One-line summary**
4. **Composite score** with dimension breakdown
5. **Key highlights** (strongest dimension, most notable evidence)
6. **Key risks or caveats** (data gaps, saturation flags, learning curve assessments)
7. **File path** where the full idea document was written

### Presentation Format

Present ideas in a clear, scannable format organized by rank:

```
### #1: {Idea Name} -- {Composite Score}/5.00
Market: {Market Name}
Summary: {One-line summary}

Scores: Revenue {x}/5 | Buildability {x}/5 | Validation {x}/5 | Fit {x}/5
Highlight: {The strongest selling point}
Caveat: {Any flag or concern, if applicable}
File: ideas/{market-slug}/idea-{NNN}.md
```

After presenting all ideas, confirm which ideas the user finds most interesting using `AskUserQuestion`:

```yaml
AskUserQuestion:
  questions:
    - header: "Top Picks"
      question: "Which ideas are you most interested in exploring further?"
      options:
        - label: "{Idea 1 name}"
          description: "Score: {x}/5 -- {one-line summary}"
        - label: "{Idea 2 name}"
          description: "Score: {x}/5 -- {one-line summary}"
        - label: "{Idea 3 name}"
          description: "Score: {x}/5 -- {one-line summary}"
        - label: "None of these resonate"
          description: "I'd like to explore different directions"
      multiSelect: true
```

---

## Phase 11: Optional Sub-Skill Invocation (Hub Model)

After the user has selected their top picks in Phase 10, offer to invoke the `validate-idea` and/or `plan-execution` skills as sub-steps. This phase implements the hub model where `generate-idea` serves as the main entry point and orchestrates the full idea-to-plan pipeline.

### Idea Selection for Sub-Skills

By default, use the user's top-ranked pick from Phase 10. However, the user may want to run sub-skills on a different idea. Present the hub options via `AskUserQuestion`, including the ability to choose which idea to act on:

```yaml
AskUserQuestion:
  questions:
    - header: "Next Steps"
      question: "Would you like to take any of these further right now?"
      options:
        - label: "Validate this idea"
          description: "Run /validate-idea on your top pick for deep market analysis and go/no-go recommendation"
        - label: "Create execution plan"
          description: "Run /plan-execution on your top pick for MVP scope, milestones, and launch strategy"
        - label: "Full pipeline -- validate then plan"
          description: "Run validation first, then create an execution plan if it passes"
        - label: "Choose a different idea first"
          description: "Select a different idea from the ranked list before proceeding"
        - label: "Done for now"
          description: "Review the generated documents on my own"
      multiSelect: false
```

### If user selects "Choose a different idea first":

Present all generated ideas for selection, then re-present the hub options:

```yaml
AskUserQuestion:
  questions:
    - header: "Idea Selection"
      question: "Which idea would you like to work with?"
      options:
        - label: "{Idea 1 name}"
          description: "Score: {x}/5 -- ideas/{market-slug}/idea-{NNN}.md"
        - label: "{Idea 2 name}"
          description: "Score: {x}/5 -- ideas/{market-slug}/idea-{NNN}.md"
        - label: "{Idea 3 name}"
          description: "Score: {x}/5 -- ideas/{market-slug}/idea-{NNN}.md"
      multiSelect: false
```

After the user selects an idea, re-present the hub options (Validate / Create execution plan / Full pipeline / Done for now) with the newly selected idea as the target.

### If user selects "Validate this idea":

Invoke the `validate-idea` skill via the `Task` tool, passing the path to the selected idea document:

```
Task: "Run the validate-idea skill on the idea document at {idea_file_path}.
Use the market research already gathered in {market_slug}/MARKET.md as baseline context."

skill: validate-idea
```

After validation completes, present the validation result summary to the user and return to the hub options (offering to create an execution plan, validate another idea, or exit).

### If user selects "Create execution plan":

Invoke the `plan-execution` skill via the `Task` tool, passing the idea document path and optionally any existing validation report:

```
Task: "Run the plan-execution skill on the idea document at {idea_file_path}.
User profile from the generate-idea interview:
{summary of relevant profile constraints -- time, budget, skills}
Validation report (if available): {validation_report_path or 'No validation performed yet'}"

skill: plan-execution
```

After the plan is generated, present the plan summary to the user and return to the hub options (offering to validate, plan another idea, or exit).

### If user selects "Full pipeline -- validate then plan":

Run validation first via the `validate-idea` skill, then conditionally proceed to execution planning:

1. Invoke `validate-idea` with the selected idea document (same as the validation-only flow above)
2. Wait for the validation result and present the summary to the user
3. Based on the validation outcome:

**If validation returns "Go" or "Conditional Go":**
- Automatically proceed to `plan-execution`, passing both the idea document path and the validation report path as context
- The validation report path follows the convention: `ideas/{market-slug}/validation/idea-NNN-validation.md`

**If validation returns "No-Go" or "Pivot Recommended":**
- Present the no-go findings to the user
- Use `AskUserQuestion` to offer options:

```yaml
AskUserQuestion:
  questions:
    - header: "Validation Result: No-Go"
      question: "The validation suggests this idea may not be viable. How would you like to proceed?"
      options:
        - label: "Create execution plan anyway"
          description: "Proceed with plan-execution despite the no-go recommendation"
        - label: "Validate a different idea"
          description: "Run validation on another idea from the ranked list"
        - label: "Done for now"
          description: "Review the validation report and decide later"
      multiSelect: false
```

If the user chooses to plan despite no-go, invoke `plan-execution` with both the idea and the no-go validation report so the plan can account for the identified risks.

### If user selects "Done for now":

Exit the hub cleanly:
1. Summarize what was generated during this session (number of markets analyzed, number of ideas generated, top-ranked idea)
2. List all generated file paths for easy reference
3. Remind the user they can run `/validate-idea` or `/plan-execution` manually later on any of the generated idea documents
4. No sub-skill invocation occurs -- the workflow ends here

### Sub-Skill Error Handling

If a sub-skill invocation fails (Task tool returns an error, skill crashes, or produces no output):

1. Report the error to the user with available details (which skill failed, error context)
2. Do NOT crash the hub workflow -- return to the hub options via `AskUserQuestion`:

```yaml
AskUserQuestion:
  questions:
    - header: "Sub-Skill Error"
      question: "The {skill_name} skill encountered an error. How would you like to proceed?"
      options:
        - label: "Retry"
          description: "Try running {skill_name} again"
        - label: "Try a different action"
          description: "Return to the hub options menu"
        - label: "Done for now"
          description: "Exit and review what was generated so far"
      multiSelect: false
```

3. If the user retries and the skill fails again, recommend running the skill manually later via its direct slash command (`/validate-idea` or `/plan-execution`)

---

## Error Handling

### Web Search Failures

If the market-researcher agent cannot perform web searches at any phase:
- **Phase 3 (Market Discovery)**: Flag affected markets as "low-confidence" and present with caveats about limited data
- **Phase 6 (Freshness Filter)**: Flag ideas with **Saturation Flag: Unknown** -- do not silently pass them through
- Inform the user that web research was limited and recommend running `/validate-idea` later for deeper analysis

### Sparse Market Evidence

If market evidence for an idea is limited (fewer than 2 evidence points with source attribution):
- Mark the idea as **"Partially Validated"** in the Validation Status field
- Include a Data Gaps section in the idea document listing what was sought but not found
- Include the idea in rankings but note the data limitation

### Insufficient Profile Data

If the user provides very sparse profile data despite interview efforts:
- Generate a wider range of ideas across more categories (breadth over depth)
- Flag ideas as "broadly personalized" -- personalization is based on limited data
- Recommend re-running the interview with more detail for better results
- Prioritize Solo Buildability and Market Validation over Personal Fit in scoring when fit data is limited

### Session Duration

The complete workflow should take approximately 20-40 minutes of active interaction and stay within a single CC context window. If the session is running long:
- Prioritize quality over quantity -- generate fewer ideas per market if needed
- Reduce freshness filter search depth (fewer queries per idea)
- Offer the user the option to save progress and continue with fewer markets

---

## Reference Files

This skill loads the following reference files at the indicated workflow phases:

- `references/interview-questions.md` -- User profiling question bank (loaded in Phase 2)
- `references/market-categories.md` -- Market taxonomy, discovery patterns, and selection criteria (loaded in Phase 3)
- `references/anti-slop-filters.md` -- Three-layer anti-slop pipeline: slop list, contrarian filter, freshness filter (loaded in Phase 6)
- `references/scoring-rubric.md` -- 4-dimension scoring rubric with weights, presets, and composite formula (loaded in Phase 7)
- `references/templates/market-analysis.md` -- Template for MARKET.md output files (loaded in Phase 9)
- `references/templates/idea-document.md` -- Template for idea-NNN.md output files (loaded in Phase 9)
- `../../agents/market-researcher.md` -- Market research agent definition (used in Phases 3, 6, and 8)
