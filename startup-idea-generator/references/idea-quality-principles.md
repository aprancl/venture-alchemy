# Idea Quality Principles

This cross-skill reference defines the quality standards and anti-slop philosophy that all skills in the Startup Idea Generator plugin must adhere to. It establishes what "quality" means for generated ideas, how evidence should be handled, and how each skill contributes to maintaining high standards.

Every skill -- `generate-idea`, `validate-idea`, and `plan-execution` -- must internalize these principles. They are not optional guidelines; they are the core identity of this plugin.

---

## 1. Core Quality Principles

These five principles govern every idea, validation, and plan produced by the plugin. When in doubt, apply the principle that results in more honest, more specific, and more useful output.

### 1.1 Every Idea Must Be Backed by Market Evidence

An idea without evidence is a guess. This plugin does not produce guesses.

- Every idea presented to the user must reference **specific market data**: competitor names, pricing data points, demand signals, trend indicators, or community evidence
- Evidence must come from web research via the `market-researcher` agent, not from the LLM's training data alone
- If evidence is unavailable or insufficient, the idea must explicitly state what data is missing -- never fill gaps with plausible-sounding fabrications
- An idea with strong evidence and a moderate concept beats an idea with a brilliant concept and no evidence

**Test**: For any claim in an idea document, ask: "Where did this data come from?" If the answer is "it seemed reasonable," the claim must be rewritten or removed.

### 1.2 Ideas Must Be Personalized to the User's Profile

Generic ideas are the enemy. Every idea must connect to the specific user who requested it.

- Each idea must demonstrate substantive connections to **at least two** user profile attributes (skills, domain experience, interests, network, constraints, goals)
- "Substantive" means the connection is specific and meaningful -- not "this can be built with code" (true for every developer) but "this leverages your 5 years of experience in healthcare compliance"
- Ideas that any random person could have generated equally well have failed the personalization test
- When profile data is sparse, lean on interests and constraints rather than generating generic fallbacks

**Test**: "Would this exact idea have been generated for a user with a completely different profile?" If yes, it is not personalized enough.

### 1.3 Generic and Obvious Ideas Are Explicitly Filtered

This plugin actively rejects ideas that a simple ChatGPT prompt would produce.

- The three-layer anti-slop pipeline (personalization, contrarian filter, freshness filter) must process every idea before it reaches the user
- Ideas matching 2+ generic signals (no niche, no differentiation, technology-first thinking, no profile leverage) are rejected unless counterbalanced by strong contrarian signals
- A curated "slop list" of 35+ oversaturated idea categories serves as the baseline filter
- Before final rejection, one refinement pass is attempted: cross-reference the user's profile for a niche, angle, or advantage that could rescue the idea
- Rejection is not failure -- it is quality control. The plugin should generate more candidates than it presents, discarding the generic ones silently

**Test**: Compare the final idea list against the slop list. Fewer than 20% should overlap. If the overlap rate exceeds this threshold, the pipeline needs adjustment.

### 1.4 Uncertainty Must Be Stated Transparently

Never present speculation as fact. The user's trust depends on knowing what is solid and what is not.

- When web search fails or returns ambiguous results, flag the affected data as "unknown" or "insufficient data" -- never default to optimistic assumptions
- Confidence levels (High, Medium, Low) must be applied to all assessments and adjusted based on evidence strength
- Saturation flags (Low, Moderate, High, Unknown) must honestly reflect what the freshness filter found, including when it found nothing
- Scoring dimensions affected by missing data must carry an `[insufficient data]` marker with an explanation of what was unavailable
- Validation reports must present both bull case and bear case scenarios, not just the optimistic view

**Test**: Read the output as if you were about to invest $50,000 based on it. Does it tell you what it does not know? If not, it is misleading.

### 1.5 Quality Over Quantity

Fewer great ideas beat many mediocre ones. Do not pad the output.

- The plugin targets 2-3 ideas per market, not a long list of half-baked concepts
- Every idea presented should be worth the user's time to read and consider seriously
- If the pipeline filters out most candidates for a market, present fewer ideas rather than loosening the filters
- A session that produces 3 strong, evidence-backed, personalized ideas has succeeded; a session that produces 15 generic ideas has failed
- Validation and planning skills should go deep on one idea rather than shallow across many

**Test**: Would you personally spend a weekend building a prototype of this idea based on the evidence presented? If the answer is "maybe, but I'd want to check a few things first" -- those things should already be in the document.

---

## 2. Evidence Standards

### 2.1 What Counts as Valid Market Evidence

Not all information is evidence. Evidence must be specific, attributable, and relevant to the claim it supports.

| Evidence Type | Valid Examples | Invalid Examples |
|---------------|---------------|------------------|
| **Competitor data** | "Linear charges $8/user/mo and has raised $52M in funding" | "There are several competitors in this space" |
| **Market sizing** | "The global project management software market was valued at $6.6B in 2024 (Grand View Research)" | "This is a large and growing market" |
| **Demand signals** | "r/freelance has 3 weekly posts asking for better invoicing tools; 'invoice software for contractors' shows 12,000 monthly searches" | "People probably want this" |
| **Pricing validation** | "Competitors charge $15-$50/mo; Baremetrics shows median SaaS churn of 5-7% for this price range" | "Users would likely pay for this" |
| **Trend data** | "Google Trends shows 'AI code review' search interest up 340% year-over-year" | "AI is a growing trend" |
| **Community evidence** | "ProductHunt launch of [specific tool] got 1,200 upvotes; Hacker News discussion with 340 comments" | "There's a lot of interest in this area" |
| **Gap identification** | "All 6 competitors require enterprise contracts; no self-serve option exists below $100/mo" | "There's room for disruption" |

### 2.2 Source Attribution

Every piece of evidence must be traceable to its origin. Attribution enables the user to verify claims and build on the research.

**Attribution format in idea documents and validation reports:**

- Competitor names and URLs when referencing specific products
- Publication names and dates for market research citations
- Subreddit names, thread titles, and approximate dates for community evidence
- "Google Trends" with the specific search term and timeframe for trend data
- "Web search via market-researcher agent" with the query used, when the source is search results rather than a specific publication

**When exact attribution is unavailable:**

If a data point came from aggregated search results rather than a single authoritative source, state: "Based on web search results for '{query}' (market-researcher agent, {date})" -- this is honest about the provenance without fabricating a specific citation.

### 2.3 Confidence Level Indicators

Apply confidence levels to all assessments to help users calibrate their trust in the analysis.

| Level | Definition | When to Apply |
|-------|-----------|---------------|
| **High** | Multiple independent sources confirm the claim; data is recent (within 12 months); sources are authoritative | Strong market data, multiple competitor confirmations, clear trend data |
| **Medium** | Some supporting evidence exists but from limited sources; data may be older (12-24 months); sources are credible but not authoritative | Single-source market estimates, competitor data from indirect sources, community signals with moderate activity |
| **Low** | Evidence is weak, indirect, or speculative; based on adjacent market data or extrapolation; sources are anecdotal | Extrapolated market size, demand inferred from adjacent markets, limited community signals, LLM-generated estimates |

**Adjustment rules:**

- **Downgrade by one level** if the data is older than 18 months
- **Downgrade by one level** if the only source is the LLM's training data without web search confirmation
- **Downgrade by one level** if the evidence is from an adjacent market rather than the target market
- **Upgrade by one level** if multiple independent sources corroborate the same finding
- **Never rate above Medium** if web search was unavailable for verification

### 2.4 Handling Missing Data

Missing data is information, not an absence of it. How the plugin handles gaps defines its trustworthiness.

| Scenario | Required Behavior |
|----------|-------------------|
| Web search returns no results for a market | Flag as "insufficient data"; do not infer from LLM knowledge alone; recommend `/validate-idea` for deeper research |
| No competitors found | Could mean no market OR a genuine gap; present both interpretations; flag as "Low competition -- verify demand exists" |
| User profile is incomplete | Acknowledge reduced personalization confidence; flag ideas as "broadly personalized"; recommend re-running the interview with more detail |
| Pricing data unavailable | Score Revenue Potential with `[insufficient data]` marker; use adjacent market comparables if available (with explicit caveat); default to score of 3 |
| Search results are ambiguous | Attempt at least 3 query variations; if still ambiguous, flag as "Unknown" rather than guessing; document the queries attempted |
| Market data contradicts itself | Present both data points with their sources; let the user decide which is more credible; do not silently pick the more favorable one |

**The cardinal rule**: When data is missing, say so. Never fill gaps with plausible-sounding content. The user cannot distinguish between researched facts and fabricated ones -- that is precisely why transparency is mandatory.

---

## 3. Anti-Slop Philosophy

### 3.1 Why This Matters

The core problem this plugin exists to solve is that AI-generated startup advice is overwhelmingly generic. Ask any LLM "give me startup ideas" and you will get a predictable list: todo apps, AI wrappers, expense trackers, SaaS dashboards. This is "slop" -- output that sounds helpful but provides no actionable, differentiated insight.

Slop is dangerous because it feels productive. A developer reads 10 AI-generated startup ideas, picks one that sounds plausible, spends weeks building it, then discovers the market is saturated with identical products. The time was wasted not because the developer lacked skill, but because the idea lacked evidence.

This plugin's entire value proposition depends on producing output that a generic LLM prompt cannot. If the ideas it generates are indistinguishable from a ChatGPT brainstorming session, the plugin has failed regardless of how polished its output formatting is.

### 3.2 The Three-Layer Approach

Anti-slop is not a single filter -- it is a philosophy embedded across three complementary layers. Each layer catches a different category of generic output.

**Layer 1: Deep Personalization**

The first defense against slop is making every idea specific to the user.

- Draws on a detailed user profile gathered through structured interview (skills, domain experience, interests, network, constraints, goals, risk tolerance)
- Requires at least 2 substantive profile attribute connections per idea
- Ideas are generated *from* the profile, not generated generically and then checked against it
- The `generate-idea` skill owns this layer via its interview phases and the personalization patterns in `anti-slop-filters.md`

**Layer 2: Market Evidence**

The second defense is grounding every idea in real-world data rather than LLM pattern matching.

- The `market-researcher` agent performs web searches for competitor data, market sizing, demand signals, and trend indicators
- Evidence must be specific and attributable (see Section 2.1)
- Ideas without market evidence are flagged, not presented as validated
- The `generate-idea` skill owns market discovery; the `validate-idea` skill goes deeper on selected ideas; the `plan-execution` skill ensures plans are based on validated market assumptions

**Layer 3: Contrarian and Freshness Filtering**

The third defense actively rejects ideas that are obvious or oversaturated.

- The contrarian filter catches ideas that match known generic patterns (no niche, no differentiation, technology-first thinking)
- The freshness filter uses web search to count established competitors and flag saturated markets
- A curated slop list of 35+ generic idea categories serves as the baseline rejection list
- Ideas must survive both filters before reaching the user; rejected ideas are refined or replaced, never silently passed through
- The `generate-idea` skill owns both filters via the anti-slop pipeline defined in `anti-slop-filters.md`

### 3.3 How Each Skill Contributes to Quality

Quality is not the responsibility of one skill -- it is a chain where each link must hold.

| Skill | Quality Responsibilities | Key Quality Mechanisms |
|-------|--------------------------|----------------------|
| **generate-idea** | Produce personalized, evidence-backed, non-generic ideas | Three-layer anti-slop pipeline; formal scoring rubric; structured interview for deep profiling; market-researcher agent for evidence gathering |
| **validate-idea** | Stress-test ideas with honest, evidence-based analysis | Devil's advocate approach (actively searches for failure modes); SWOT analysis grounded in research; go/no-go recommendation with confidence level; demand signal analysis when competitor data is sparse |
| **plan-execution** | Ensure plans are realistic, actionable, and evidence-based | Solo-developer constraints applied to all timelines; MVP scoping focused on minimum viable revenue; validation checkpoints built into milestones; realistic resource assumptions based on user profile |

**Cross-skill quality chain:**

```
generate-idea                    validate-idea                 plan-execution
     |                                |                              |
 [Personalized +              [Devil's advocate              [Realistic scope +
  evidence-backed +            challenges the idea;           solo-dev constraints;
  anti-slop filtered]          adjusts confidence             validation checkpoints
                               based on evidence]             built into milestones]
     |                                |                              |
     v                                v                              v
 Ideas that a generic          Validation that honestly       Plans that a solo dev
 LLM prompt cannot produce     surfaces risks and gaps        can actually execute
```

If `generate-idea` produces a generic idea, `validate-idea` should catch it through honest risk assessment. If `validate-idea` gives a false "go" signal, `plan-execution` should surface the gap when scoping reveals the idea cannot be built or monetized within the user's constraints. No single skill is a sufficient quality gate -- the pipeline as a whole provides the guarantee.

---

## 4. Consistency Standards

### 4.1 Scoring Rubric Usage Across Skills

The formal scoring rubric (Revenue Potential, Solo Buildability, Market Validation, Personal Fit) is the shared language for evaluating ideas across the plugin. All skills must use it consistently.

| Aspect | Standard | Applies To |
|--------|----------|------------|
| **Dimensions** | Always 4 dimensions: Revenue Potential, Solo Buildability, Market Validation, Personal Fit | All skills that evaluate ideas |
| **Scale** | Always 1-5, where 1 = Very Low and 5 = Very High | generate-idea (initial scoring), validate-idea (score adjustment) |
| **Weights** | Default 25% each; user-adjustable via presets or custom values; weights always sum to 100% | generate-idea (scoring phase) |
| **Composite calculation** | Weighted average: `(W_r * S_r) + (W_b * S_b) + (W_v * S_v) + (W_f * S_f)` | generate-idea (ranking), validate-idea (re-ranking if scores adjust) |
| **Insufficient data** | Score defaults to 3 with `[insufficient data]` marker; never omit the dimension | All skills |
| **Rationale** | Every score must include a 2-3 sentence rationale citing specific evidence | generate-idea (idea documents), validate-idea (validation reports) |
| **Tiebreaker order** | Market Validation > Revenue Potential > Personal Fit > Solo Buildability | generate-idea (ranking), validate-idea (re-ranking) |

**Score evolution across the pipeline:**

- `generate-idea` assigns initial scores based on market discovery and user profile
- `validate-idea` may adjust scores based on deeper research (adjustments must cite new evidence and explain the change)
- `plan-execution` does not re-score but must ensure its plans are consistent with the idea's scores (e.g., a Solo Buildability score of 2 should result in a longer timeline and more aggressive scope cuts)

### 4.2 Terminology Consistency

Use these terms consistently across all skills, references, templates, and output documents. Do not use synonyms or informal alternatives.

| Canonical Term | Definition | Do NOT Use |
|----------------|-----------|------------|
| **Idea document** | The markdown file (`idea-NNN.md`) containing a single idea with evidence and scoring | "idea report," "idea card," "idea summary" |
| **Validation report** | The markdown file (`idea-NNN-validation.md`) containing deep-dive analysis | "validation document," "validation summary" |
| **Execution plan** | The markdown file (`idea-NNN-plan.md`) containing the actionable plan | "roadmap," "implementation plan," "project plan" |
| **Market analysis** | The markdown file (`MARKET.md`) containing market-level research | "market report," "market summary" |
| **Scoring rubric** | The 4-dimension evaluation framework | "scoring system," "ranking criteria," "evaluation matrix" |
| **Composite score** | The weighted average of all 4 dimension scores | "total score," "overall score," "final score" |
| **Saturation flag** | The market saturation indicator (Low, Moderate, High, Unknown) | "competition level," "market crowding" |
| **Confidence level** | The assessment certainty indicator (High, Medium, Low) | "certainty," "trust level," "reliability" |
| **Anti-slop pipeline** | The three-layer filtering process | "quality filter," "idea filter," "slop check" |
| **Contrarian filter** | The heuristic-based generic idea detector | "uniqueness check," "originality filter" |
| **Freshness filter** | The web search-based market saturation check | "competition check," "market saturation check" |
| **Go/No-Go** | The recommendation output of validate-idea (Go, Conditional Go, No-Go, Pivot Recommended) | "recommendation," "verdict," "decision" |
| **MVP** | Minimum Viable Product -- the smallest buildable scope | "V1," "first version," "prototype" (unless specifically referring to a pre-MVP prototype) |
| **Validation checkpoint** | A point in the execution plan where real-world feedback should be assessed | "check-in," "review point," "gate" |

### 4.3 Output Format Consistency

All skills produce markdown files following a shared structural pattern. Consistency in format enables pipeline chaining -- the output of one skill feeds directly into the next.

**Shared formatting rules:**

| Rule | Description |
|------|-------------|
| **File naming** | Lowercase, hyphenated: `idea-001.md`, `idea-001-validation.md`, `idea-001-plan.md` |
| **Directory structure** | All output under `ideas/{market-slug}/` with subdirectories for `validation/` and `execution-plans/` |
| **Metadata header** | Every output file begins with a YAML-style metadata block (idea name, date, skill version, confidence level) |
| **Section ordering** | Follow the canonical section order defined in each template; do not reorder or rename sections |
| **Evidence formatting** | Inline citations with source attribution; no footnotes or endnotes |
| **Score display** | Always `{score}/5` format for individual dimensions; `{score}/5.00` for composite scores |
| **Flag display** | Bold labels: **Saturation Flag: {value}**, **Confidence: {value}**, **[insufficient data]** |
| **HTML comments** | Used as conditional guidance blocks in templates -- content within `<!-- -->` is instructional and must not appear in final output |
| **Source attribution** | "Source: {description} ({origin}, {date})" format for all evidence citations |

**Pipeline chaining requirements:**

- `generate-idea` output (idea documents) must be parseable by `validate-idea` as input
- `validate-idea` output (validation reports) must be parseable by `plan-execution` as input
- `plan-execution` output (execution plans) must be structured to feed into agent-alchemy `/create-spec`
- Each skill must preserve upstream metadata (idea name, market slug, scoring data) in its output so downstream skills can reference it without re-reading the original documents

---

## Summary

The quality bar for this plugin is simple: **every output should be something the user cannot get from a generic LLM prompt.** If a piece of output -- an idea, a validation, a plan -- could have been produced by asking ChatGPT the same question without any user profiling, market research, or filtering, then it has failed the quality test.

The three pillars are:

1. **Personalization** -- Ideas are tailored to this specific user, not "developers in general"
2. **Evidence** -- Claims are grounded in researched data, not pattern-matched plausibility
3. **Honesty** -- Gaps, risks, and uncertainties are stated transparently, not papered over

Every skill, every reference, and every template in this plugin exists to uphold these three pillars.
