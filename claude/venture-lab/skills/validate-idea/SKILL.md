---
name: validate-idea
description: |
  Deep-dive validation of a startup idea with SWOT analysis,
  competitor research, devil's advocate approach, and go/no-go recommendation.
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - AskUserQuestion
  - Task
  - Read
  - Write
  - Glob
  - Grep
argument-hint: "[idea-path]"
arguments:
  - name: idea-path
    description: Path to the idea document to validate (optional — if omitted, the user will be interviewed)
    required: false
model: opus
---

# Validate Idea Skill

You are initiating the idea validation workflow. This process performs a deep-dive analysis of a startup or side-project idea using SWOT analysis, competitor research, devil's advocate challenges, scenario modeling, and a structured go/no-go recommendation -- all grounded in web research and the validation frameworks defined in the reference files.

## Critical Rules

### AskUserQuestion is MANDATORY

**IMPORTANT**: You MUST use the `AskUserQuestion` tool for ALL questions to the user. Never ask questions through regular text output.

- Confirmation prompts -> AskUserQuestion
- Requesting a corrected file path -> AskUserQuestion
- Asking about pivot preferences -> AskUserQuestion
- Yes/no choices -> AskUserQuestion

Text output should only be used for:
- Presenting validation findings (SWOT, competitor table, risks, scenarios)
- Summarizing research results
- Delivering the go/no-go recommendation
- Explaining reasoning or context

If you need the user to make a choice or provide input, use AskUserQuestion.

### Plan Mode Behavior

**CRITICAL**: This skill validates ideas, NOT an implementation plan. When invoked during Claude Code's plan mode:

- **DO NOT** create an implementation plan for how to build the skill itself
- **DO NOT** defer validation to an "execution phase"
- **DO** proceed with the full validation workflow immediately
- **DO** write the validation report to the output path as normal

Validating the idea IS the planning activity.

### Idea Input is Flexible

This skill accepts an idea via file path OR conversational interview. If no `idea-path` argument is provided, the skill asks the user how they want to supply their idea. If a path is provided but the file does not exist, prompt for correction or offer the interview alternative.

---

## Workflow Overview

This workflow has ten phases:

1. **Gather Idea** -- Obtain the idea from a file or through a conversational interview
2. **Load References** -- Read validation criteria and report template
3. **Deep Research** -- Spawn market-researcher agent for competitor/market analysis
4. **SWOT Analysis** -- Structured analysis using validation criteria frameworks
5. **Competitor Deep-Dive** -- Build comparison table with pricing, features, differentiation
6. **Devil's Advocate** -- Actively search for failure reasons, identify top 3 risks
7. **Bull/Bear Scenarios** -- Present optimistic and pessimistic outcomes
8. **Go/No-Go** -- Generate recommendation with confidence level
9. **Output** -- Write validation report to output path
10. **Present Results** -- Show key findings and recommendation to user

---

## Phase 1: Gather Idea

This phase has two paths depending on whether an `idea-path` argument was provided.

### Path A: No Argument Provided

If no `idea-path` was given, ask the user how they want to supply their idea:

```yaml
AskUserQuestion:
  questions:
    - header: "Your Idea"
      question: "How would you like to share the idea you want validated?"
      options:
        - label: "Describe it here"
          description: "I'll explain my idea and you'll ask me follow-up questions"
        - label: "Point to a file"
          description: "I have a document with my idea written up"
        - label: "Search for idea files"
          description: "Search the ideas/ directory for previously generated ideas"
      multiSelect: false
```

- If **"Point to a file"**: prompt for the path via `AskUserQuestion`, then continue to Path B.
- If **"Search for idea files"**: use `Glob` to find candidates (`ideas/**/idea-*.md`), present results via `AskUserQuestion`, then continue to Path B with the selected file.
- If **"Describe it here"**: continue to the **Idea Interview** below.

### Idea Interview

When the user chooses to describe their idea conversationally, conduct a short interview to gather enough detail for validation. The interview has two rounds.

#### Round 1: Core Concept

```yaml
AskUserQuestion:
  questions:
    - header: "The Idea"
      question: "Describe your idea. What does it do, and what problem does it solve?"
      options:
        - label: "It's a product/tool"
          description: "A software product, app, API, or developer tool"
        - label: "It's a service/platform"
          description: "A marketplace, agency model, SaaS platform, or managed service"
        - label: "It's content/community"
          description: "A media product, newsletter, community, course, or info product"
      multiSelect: false
```

The user's free-text answer and selected option together form the idea description. After receiving their response, assess whether enough detail was provided to proceed. Enough detail means you have a clear understanding of:

- What the product/service is
- What problem it solves
- Who it is for (even if vaguely stated)

If the response is too vague (e.g., "an app for freelancers" with no further detail), ask a clarifying follow-up before moving to Round 2:

```yaml
AskUserQuestion:
  questions:
    - header: "Tell Me More"
      question: "Can you elaborate on what specifically it does? What would a user do with it day-to-day?"
      options:
        - label: "Let me explain more"
          description: "I'll give a more detailed description"
        - label: "I'm still figuring it out"
          description: "I have the general direction but not the specifics yet"
      multiSelect: false
```

If the user selects "I'm still figuring it out," work with what was given and note gaps in the validation report.

#### Round 2: Business Model & Target Customer

Once the core concept is understood, dig into the business specifics:

```yaml
AskUserQuestion:
  questions:
    - header: "Target Customer"
      question: "Who is the target customer? Be as specific as you can -- job title, company size, situation, or demographic."
      options:
        - label: "Individual consumers"
          description: "B2C — everyday people or a specific demographic"
        - label: "Small businesses / freelancers"
          description: "B2B SMB — solo operators, agencies, or small teams"
        - label: "Mid-market / enterprise"
          description: "B2B — companies with dedicated budgets and buying processes"
        - label: "Developers / technical users"
          description: "DevTools — engineers, data scientists, or technical teams"
      multiSelect: false
    - header: "Revenue Model"
      question: "How would this make money? Pick the closest model and add details in the text box (pricing thoughts, free tier, etc.)."
      options:
        - label: "Subscription (SaaS)"
          description: "Monthly or annual recurring fee"
        - label: "One-time purchase"
          description: "Pay once for the product or a license"
        - label: "Usage-based / pay-per-use"
          description: "Charge based on consumption, API calls, transactions, etc."
        - label: "Marketplace / commission"
          description: "Take a cut of transactions between buyers and sellers"
      multiSelect: false
    - header: "Competition"
      question: "Do you know of any existing competitors or alternatives? List names or describe them, or say 'not sure'."
      options:
        - label: "Yes, I know some"
          description: "I'll list competitors or alternatives I'm aware of"
        - label: "Not sure"
          description: "I haven't researched the competition yet"
      multiSelect: false
```

#### Synthesize Into Idea Document

After the interview, synthesize the user's responses into a structured idea document and write it to `ideas/ad-hoc/idea-{timestamp}.md` (using format `YYYYMMDD-HHMMSS` for the timestamp). The document should follow this structure:

```markdown
# {Idea Name}

> {One-line summary derived from the user's description}

## Overview

{2-3 paragraph description synthesized from the interview responses}

## Target Customer

{Target customer description from Round 2}

## Revenue Model

{Revenue model and any pricing details from Round 2}

## Known Competitors

{Competitors listed by the user, or "No competitors identified by the user — to be researched during validation."}

---

*Source: Conversational interview via /validate-idea | {YYYY-MM-DD}*
```

Derive a clear, concise idea name from the user's description. Do not use generic names like "My Idea" — synthesize what they described into a proper product concept name.

After writing the file, treat it as an external idea and continue to **Extract Key Details** below.

### Path B: Argument Provided

Read the idea document at the provided `idea-path`:

```
Read: {idea-path}
```

If the file does not exist or cannot be read, use `AskUserQuestion` to offer alternatives:

```yaml
AskUserQuestion:
  questions:
    - header: "File Not Found"
      question: "The file at '{idea-path}' could not be found. How would you like to proceed?"
      options:
        - label: "Let me fix the path"
          description: "I'll provide the correct file path"
        - label: "Describe my idea instead"
          description: "I'll explain my idea and you'll ask follow-up questions"
        - label: "Search for idea files"
          description: "Search the ideas/ directory for available idea documents"
      multiSelect: false
```

- If **"Describe my idea instead"**: go to the **Idea Interview** above.
- If **"Search for idea files"**: use `Glob` (`ideas/**/idea-*.md`), present results, and read the selected file.
- If **"Let me fix the path"**: prompt for the corrected path and retry.

### Extract Key Details

From the idea document (whether loaded from file or synthesized from interview), extract:

- **Idea name** and one-line summary
- **Market slug** and market name (from the file path or document content)
- **Target customer** description
- **Revenue model** and pricing approach
- **Competitive landscape** already identified (if any)
- **Scoring data** (if generated by `/generate-idea`)
- **Personal fit** assessment (if available)
- **Idea ID** (e.g., `idea-001` from the filename)

### Detect External Ideas

Determine whether the idea was generated by `/generate-idea` or is an external idea. An external idea is identified when:

- The idea document does not follow the `idea-NNN.md` format
- No corresponding `MARKET.md` exists in the parent directory
- The idea document lacks a Scoring section or Personal Fit assessment
- The idea was gathered via the conversational interview
- The user explicitly states the idea is not from `/generate-idea`

If the idea is external:
1. Note that Personal Fit assessment will be unavailable
2. Plan to conduct full market research from scratch in Phase 3
3. Flag the confidence adjustment: reduce by one level if fit is a critical factor

### Load Existing Market Data

If a `MARKET.md` file exists in the same directory as the idea document, read it for baseline market context:

```
Read: ideas/{market-slug}/MARKET.md
```

This provides market size estimates, trend data, competitive landscape, and community targets already gathered during idea generation. Use this as a starting point -- Phase 3 will deepen the research.

If no `MARKET.md` exists (external idea or interview-sourced idea), all market research will be conducted from scratch in Phase 3.

---

## Phase 2: Load References

### Load Validation Criteria

Read the validation frameworks that guide all analysis phases:

```
Read: references/validation-criteria.md
```

This reference defines:
- SWOT analysis framework with startup-specific guidance (Section 1)
- Competitor analysis patterns and comparison table format (Section 2)
- Risk assessment framework with four risk categories (Section 3)
- Devil's advocate patterns and pre-mortem exercise (Section 4)
- Go/no-go decision framework with confidence levels (Section 5)
- Bull/bear case scenario structure (Section 6)
- Demand signal analysis for markets with no competitors (Section 7)
- Qualitative revenue model scenarios (Section 8)
- Handling external ideas (Section 9)
- Integration with validation report (Section 10)

### Load Validation Report Template

Read the output template to understand the report structure:

```
Read: references/templates/validation-report.md
```

This template defines the structure and conditional blocks for the validation report output.

---

## Phase 3: Deep Research

### Spawn Market Researcher Agent

Use the `Task` tool to spawn the `market-researcher` agent for deeper web research. The research request should go beyond what was gathered during idea generation (if applicable), focusing on validation-specific intelligence.

```
Task prompt template:
"Conduct deep validation research for the following idea:

Idea: {idea name}
Description: {idea summary}
Target customer: {target customer description}
Revenue model: {revenue model}
Market: {market name}

Research focus areas:
1. COMPETITOR DEEP-DIVE: Find 5-10 direct and adjacent competitors. For each, gather: pricing model and specific tiers, target audience, key features, estimated traction (users, revenue, growth signals), funding status, and key weaknesses. Search for negative reviews and user complaints.
2. PRICING LANDSCAPE: Document specific pricing data points across competitors. Identify the pricing sweet spot and common patterns.
3. CUSTOMER ACQUISITION CHANNELS: How do competitors acquire customers? What organic and paid channels work in this market?
4. MARKET TRENDS: What is the market trajectory? Are there regulatory changes, technology shifts, or behavioral trends affecting this space?
5. DEMAND SIGNALS: Search for evidence of customer demand -- forum discussions, search trends, 'wish there was' posts, complaint threads about existing solutions.
6. FAILED ATTEMPTS: Search for startups that tried and failed in this space. What went wrong?

{If external idea: 'This is an external idea with no prior market research. Conduct comprehensive market analysis from scratch.'}
{If existing MARKET.md data: 'Baseline market data is available. Focus on deepening competitor analysis, gathering pricing specifics, and finding demand/failure signals not covered in the baseline.'}

Return structured findings in the standard market research output format."

subagent_type: market-researcher
```

### Handle Research Failures

If the market-researcher agent fails or returns incomplete data:

1. **Do not abort validation** -- proceed with whatever data was gathered
2. **Note the gaps** in the validation report's "Data Gaps and Limitations" section
3. **Adjust confidence** accordingly (reduce by one level per Section 5.3 of validation criteria)
4. **Inform the user** during Phase 10 that research was limited
5. Use any data from the existing `MARKET.md` as a fallback baseline

### Handle No Direct Competitors

If the market-researcher returns 0-1 direct competitors:

1. **Pivot to demand signal analysis** as defined in Section 7 of validation-criteria.md
2. Spawn an additional research request focused on demand signals:

```
Task prompt template:
"No direct competitors were found for this idea. Conduct demand signal analysis:

Idea: {idea name}
Description: {idea summary}
Target customer: {target customer description}

Research focus:
1. SEARCH TRENDS: Search volume trends for the problem keywords, category terms, and related queries
2. COMMUNITY ACTIVITY: Reddit threads, Hacker News posts, Stack Overflow questions, Twitter discussions about the problem
3. ADJACENT MARKETS: What is the closest existing product category? Are customers in adjacent markets asking for this?
4. 'WISH THERE WAS' SIGNALS: Forum posts, tweets, or discussions where people describe wanting a solution like this

Return structured findings with demand signal strength assessment (Strong/Moderate/Weak)."

subagent_type: market-researcher
```

3. Use demand signal findings in place of competitor comparison in the validation report
4. State the limitation explicitly in the report

---

## Phase 4: SWOT Analysis

Using the SWOT framework from Section 1 of `references/validation-criteria.md`, build a structured SWOT analysis for the idea.

### Populate Each Quadrant

#### Strengths (Internal, Helpful)

Evaluate against the startup-specific dimensions:
- **Founder skills**: Does the founder have direct technical skills to build this?
- **Domain knowledge**: Does the founder have professional or personal experience in this market?
- **Network advantage**: Does the founder know potential customers, advisors, or distribution partners?
- **Unfair advantage**: Is there something about this specific founder + idea combination that a competitor cannot replicate?
- **Speed to market**: Can an MVP be shipped significantly faster than funded competitors could react?
- **Cost structure**: Can this be built and operated at near-zero cost initially?

If the idea is external (no user profile data), assess strengths based only on the idea's inherent characteristics. Note that founder-specific strengths cannot be assessed.

#### Weaknesses (Internal, Harmful)

Evaluate:
- **Skill gaps**: What critical skills are missing?
- **Time constraints**: Is the timeline realistic for the opportunity?
- **Budget constraints**: What financial limits exist?
- **No brand or audience**: Does the founder start from zero reputation in this market?
- **Single point of failure**: Solo developer risks
- **Missing capabilities**: Non-technical capabilities needed (regulatory, content, sales)

#### Opportunities (External, Helpful)

Evaluate using research findings from Phase 3:
- **Market trends**: Is the target market growing?
- **Competitor gaps**: What do existing solutions do poorly?
- **Underserved segments**: Are there ignored customer segments?
- **Technology enablers**: Are new technologies making this newly feasible?
- **Distribution channels**: Are there untapped channels?
- **Timing**: Why is now the right time?

Each opportunity bullet must reference specific evidence from the research.

#### Threats (External, Harmful)

Evaluate using research findings:
- **Incumbent response**: Could an established player easily add this feature?
- **Market saturation**: Is the market already crowded?
- **Customer acquisition cost**: Is it expensive to reach and convert the target audience?
- **Platform dependency**: Does the idea depend on a platform that could change terms?
- **Regulatory risk**: Are there regulations that could impact the business model?
- **Market timing**: Could the window close before the product is ready?

Each threat bullet must reference specific evidence from the research.

### SWOT Balance Assessment

After completing all four quadrants, synthesize:
- Is the overall SWOT balance **positive** (strengths + opportunities outweigh weaknesses + threats), **neutral**, or **negative**?
- What is the single most significant finding from the SWOT analysis?
- This assessment feeds into the Go/No-Go decision in Phase 8.

---

## Phase 5: Competitor Deep-Dive

### Build Comparison Table

Using competitor data from Phase 3, build the comparison table defined in Section 2.2 of `references/validation-criteria.md`:

| Attribute | {Competitor 1} | {Competitor 2} | {Competitor 3} | **Your Idea** |
|-----------|---------------|---------------|---------------|---------------|
| **Product type** | {SaaS, app, API, etc.} | ... | ... | ... |
| **Target audience** | {Who they serve} | ... | ... | ... |
| **Core features** | {Key capabilities} | ... | ... | ... |
| **Missing features / gaps** | {What they lack} | ... | ... | {Your differentiation} |
| **Pricing model** | {Free, freemium, subscription, one-time} | ... | ... | ... |
| **Price point** | {Specific pricing if available} | ... | ... | ... |
| **Market position** | {Leader, challenger, niche, emerging} | ... | ... | ... |
| **Estimated traction** | {Users, revenue, growth signals} | ... | ... | ... |
| **Funding / backing** | {Bootstrapped, VC-funded, corporate} | ... | ... | ... |
| **Key weakness** | {Primary vulnerability} | ... | ... | ... |
| **Differentiation from your idea** | {How your idea differs} | ... | ... | -- |

Include at least 3 competitors when available. If more than 5 direct competitors exist, include the top 3 direct and top 2 adjacent competitors.

### Competitive Positioning Analysis

After the table, synthesize:

1. **Where competitors cluster**: What approach do most competitors take?
2. **Where the gap exists**: What is the underserved position?
3. **Your proposed position**: Where does this idea fit relative to competitors?
4. **Defensibility assessment**: How easy would it be for existing competitors to move into this position?

### Pricing Landscape Summary

Summarize the pricing patterns observed:
- Common pricing models in this market
- Price range (entry tier to enterprise)
- Free tier availability
- The pricing sweet spot for solo-developer products in this category

### Demand Signal Analysis (No Competitors)

If no direct competitors were found (0-1 results), replace the competitor comparison table with demand signal analysis as defined in Section 7 of `references/validation-criteria.md`:

1. **Search trends**: Keyword volume and direction
2. **Community activity**: Reddit, Hacker News, forums, Twitter discussion
3. **Adjacent market signals**: Closest existing category and spillover demand
4. **Overall demand signal strength**: Strong / Moderate / Weak

Include an explicit statement: "No direct competitors were identified for this idea. This analysis assesses latent demand signals in lieu of traditional competitor comparison."

---

## Phase 6: Devil's Advocate

This phase actively challenges the idea to counter confirmation bias. Rather than looking for reasons the idea will succeed, actively search for reasons it might fail.

### Structured Challenge Questions

Work through the challenge categories from Section 4.1 of `references/validation-criteria.md`:

#### Market Challenges
- Why has nobody built this yet?
- What if the problem is not painful enough to pay for?
- What if the market is smaller than it appears?
- What if the timing is wrong?
- Who will be the first 10 customers, specifically?

#### Competitive Challenges
- What stops a well-funded competitor from copying this in 3 months?
- What if the perceived competitor gap is intentional?
- What if a big player announces this feature tomorrow?
- Is the differentiation real or cosmetic?

#### Execution Challenges
- What is the hardest part of building this, and does the founder know how to do it?
- What if it takes 3x longer than estimated?
- What if the founder loses motivation at 60% completion?
- What is the minimum viable distribution strategy?

#### Financial Challenges
- What if customers will pay half of the expected price?
- What if customer acquisition costs are 5x higher than expected?
- What if it takes 12 months to reach first revenue?

For each challenge, search the research findings for evidence that supports the negative case. Do not dismiss challenges without evidence.

### Pre-Mortem Exercise

Conduct a structured pre-mortem as defined in Section 4.2 of `references/validation-criteria.md`:

1. **Assume failure**: "It is 12 months from now. The product was launched but failed. What went wrong?"
2. **Generate failure scenarios**: List 5-7 plausible reasons for failure
3. **Assess plausibility**: Rate each scenario (high/medium/low)
4. **Identify preventable failures**: Which could be mitigated or avoided?
5. **Recommend adjustments**: For each preventable failure, suggest a pivot, scope change, or mitigation

### Identify Top 3 Risks

From the devil's advocate analysis and risk assessment framework (Section 3 of `references/validation-criteria.md`), identify the top 3 risks:

For each risk:
- **Category**: Market, Technical, Execution, or Financial
- **Likelihood**: High / Medium / Low
- **Impact**: High / Medium / Low
- **Priority**: Critical / Mitigate / Monitor / Accept (from the risk scoring matrix)
- **Description**: What the risk is and why it matters
- **Mitigation**: Specific, actionable suggestion to reduce likelihood or impact

### Suggest Pivots and Modifications

When significant risks are found (any risk rated High severity):

1. Identify specific pivots or modifications that could address the risks
2. For each suggestion:
   - Describe the change clearly
   - Explain which risk it addresses
   - Assess the trade-off (what is gained vs. what is lost)
3. Limit to 3 pivot suggestions maximum
4. Pivots should be actionable and concrete, not generic advice

If no significant risks are found, note that no pivots are recommended and standard risk monitoring is sufficient.

---

## Phase 7: Bull/Bear Scenarios

Present both optimistic and pessimistic outcomes using the framework from Section 6 of `references/validation-criteria.md`.

### Bull Case (Optimistic Scenario)

Structure around:
- **Key assumptions that hold**: Which 2-3 assumptions must be true?
- **Market response**: How do target customers react? Adoption trajectory.
- **Competitive dynamics**: How does the competitive landscape play out favorably?
- **Revenue trajectory**: Qualitative description of revenue growth (see Section 8 of validation criteria for revenue modeling patterns)
- **Timeline**: How long to reach meaningful milestones?
- **End state (12-18 months)**: What does success look like?

### Bear Case (Pessimistic Scenario)

Structure around:
- **Key assumptions that break**: Which 2-3 assumptions fail, and why?
- **Market response**: How do target customers fail to adopt?
- **Competitive dynamics**: How does the competitive landscape work against the idea?
- **Sunk cost**: What has the founder invested by the time failure is apparent?
- **Salvageable value**: What can be reused, pivoted, or learned?
- **Exit points**: At what milestones should the founder consider abandoning or pivoting?

### Revenue Outlook

Include qualitative revenue model scenarios as defined in Section 8 of `references/validation-criteria.md`:

- **Revenue type**: Subscription, one-time, usage-based, marketplace, freemium
- **Revenue timing**: Immediate, delayed, gradual
- **Revenue ceiling**: Lifestyle business, scalable SaaS, agency-like
- **Revenue predictability**: High, moderate, low, variable
- **Path to first dollar**: Direct, indirect, long

Reference comparable products and their known revenue patterns when available (Indie Hackers reports, Open Startup pages, public founder interviews).

### Scenario Balance

Both scenarios must be grounded and plausible:
- The bull case should be achievable given the evidence, not fantasy
- The bear case should represent a realistic downside, not worst-case catastrophe
- Both must reference specific evidence from the competitive analysis, demand signals, and risk assessment
- The gap between bull and bear indicates the uncertainty level

---

## Phase 8: Go/No-Go

### Decision Criteria Checklist

Walk through the go/no-go assessment checklist from Section 5.4 of `references/validation-criteria.md`:

| Criterion | Assessment | Weight |
|-----------|-----------|--------|
| **Demand evidence** | {Strong/Moderate/Weak/None} | Critical |
| **Competitive position** | {Defensible/Moderate/Weak} | Critical |
| **Top risk severity** | {Manageable/Concerning/Critical} | Critical |
| **Founder-idea fit** | {Strong/Moderate/Weak} | Important |
| **Revenue path clarity** | {Clear/Plausible/Unclear} | Important |
| **Time to MVP** | {Within constraints/Tight/Exceeds constraints} | Important |
| **SWOT balance** | {Positive/Neutral/Negative} | Supporting |
| **Devil's advocate survivability** | {Survived/Partially survived/Failed} | Supporting |

### Determine Recommendation

Based on the checklist and all preceding analysis, assign one of four recommendations:

| Recommendation | When to Apply |
|---------------|---------------|
| **Go** | SWOT balance is positive, top risks are mitigable, competitive position is defensible, demand evidence exists |
| **Conditional Go** | Some validation signals are strong but key uncertainties remain; list specific conditions (max 3) that must be met |
| **No-Go** | Critical risks with no mitigation path, no evidence of demand, competitive position is indefensible |
| **Pivot Recommended** | The core insight has merit but the current formulation is flawed; suggest a specific alternative direction |

### Determine Confidence Level

Assign a confidence level using the criteria from Section 5.2 of `references/validation-criteria.md`:

| Confidence | Criteria |
|-----------|----------|
| **High** | 3+ competitors analyzed with pricing data, demand signals confirmed via multiple sources, SWOT based on researched evidence, risk mitigations identified |
| **Medium** | 1-2 competitors analyzed, some demand signals found, SWOT partially evidence-based, some risks are uncertain |
| **Low** | Few or no competitors found to analyze, demand signals are weak or absent, SWOT based largely on assumptions, significant unknowns remain |

### Apply Confidence Adjustments

Adjust the confidence level based on these rules (Section 5.3):

| Condition | Adjustment |
|-----------|-----------|
| Web research returned no competitor data | Reduce by one level |
| Demand signals are based on a single source | Reduce by one level |
| Founder has direct domain experience validating assumptions | Increase by one level |
| Multiple independent data sources confirm a finding | Increase by one level |
| Market is brand-new with no precedent | Cap at Medium unless demand signals are strong |
| External idea (no profile data) | Reduce by one level if fit is a critical factor |

### Write Reasoning

Write 2-3 paragraphs explaining the recommendation:
- What evidence supports this recommendation?
- What are the strongest arguments for and against pursuing this idea?
- Why was this confidence level assigned?
- If Conditional Go: what specific, testable, achievable conditions must be met (max 3)?

---

## Phase 9: Output

### Load Report Template

Read the validation report template:

```
Read: references/templates/validation-report.md
```

### Determine Output Path

The validation report is saved to: `ideas/{market-slug}/validation/idea-NNN-validation.md`

- **market-slug**: Derived from the idea document's location or content (lowercase, hyphenated)
- **idea-NNN**: Matches the idea document's ID (e.g., idea-001)
- If the `validation/` subdirectory does not exist, create it

For external ideas not in the standard `ideas/{market-slug}/` structure:
- Create the output path based on the idea content (derive a market slug from the idea's market description)
- Place the validation report alongside the idea document if no standard structure exists

### Write Validation Report

Write the complete validation report to the output path using the template from `references/templates/validation-report.md`. Populate all sections:

1. **Header**: Idea name, market, idea ID, validation date, recommendation, confidence level
2. **External Idea Notice** (conditional): Include if the idea was not generated by `/generate-idea`
3. **Idea Summary**: 1-2 paragraph restatement of the idea
4. **SWOT Analysis**: From Phase 4 findings
5. **Competitor Comparison**: From Phase 5 findings (or Demand Signal Analysis if no competitors)
6. **Revenue Model Scenarios**: From Phase 7 revenue outlook
7. **Top 3 Risks**: From Phase 6 risk identification, with mitigations
8. **Suggested Pivots and Modifications**: From Phase 6 pivot suggestions (or "no pivots needed" block)
9. **Go/No-Go Recommendation**: From Phase 8 decision, with reasoning and confidence drivers
10. **Data Gaps and Limitations**: Document all research gaps, their impact, and recommended follow-up
11. **Sources and Attribution**: All sources referenced throughout the report

### Include Conditional Blocks

Use the template's conditional blocks when applicable:

- **External Idea Notice**: When the idea was not generated by `/generate-idea`
- **No Direct Competitors / Demand Signal Analysis**: When no direct competitors were found
- **No Pivots Needed**: When no significant risks were found
- **Source coverage: Limited**: When fewer than 3 authoritative sources were found

### Source Attribution

Every factual claim in the report must be attributed to its source using the format:
```
*([Source title](URL), accessed YYYY-MM-DD)*
```

---

## Phase 10: Present Results

### Show Key Findings

Present a summary of the validation findings to the user. Include:

1. **Recommendation**: Go / No-Go / Conditional Go / Pivot Recommended
2. **Confidence Level**: High / Medium / Low, with brief explanation
3. **SWOT Highlights**: 1-2 strongest strengths and 1-2 most concerning threats
4. **Top Risk**: The single most critical risk and its suggested mitigation
5. **Scenario Summary**: One-sentence description of each scenario (bull and bear)
6. **File Path**: Where the full validation report was saved

### Format

Present the summary in a clear, scannable format:

```
### Validation Result: {Idea Name}

**Recommendation: {Go/No-Go/Conditional Go/Pivot Recommended}**
**Confidence: {High/Medium/Low}**

**Key Strengths:**
- {Strength 1}
- {Strength 2}

**Key Concerns:**
- {Threat/Risk 1}
- {Threat/Risk 2}

**Top Risk:** {Risk description} -- Mitigation: {Suggested approach}

**Bull Case:** {One-sentence optimistic scenario}
**Bear Case:** {One-sentence pessimistic scenario}

**Full Report:** {file path to validation report}
```

### Offer Next Steps

After presenting results, use `AskUserQuestion` to offer next steps based on the recommendation:

#### If Go or Conditional Go:

```yaml
AskUserQuestion:
  questions:
    - header: "Next Steps"
      question: "The validation suggests this idea is worth pursuing. What would you like to do next?"
      options:
        - label: "Create an execution plan"
          description: "Run /plan-execution for MVP scope, milestones, and launch strategy"
        - label: "Review the full report"
          description: "I'll read the detailed validation report at {file path}"
        - label: "Validate another idea"
          description: "Run /validate-idea on a different idea"
        - label: "I'm done for now"
          description: "I'll review the report on my own"
      multiSelect: false
```

#### If No-Go or Pivot Recommended:

```yaml
AskUserQuestion:
  questions:
    - header: "Next Steps"
      question: "The validation raised significant concerns about this idea. What would you like to do?"
      options:
        - label: "Validate a different idea"
          description: "Run /validate-idea on another idea from your portfolio"
        - label: "Explore the suggested pivots"
          description: "Discuss the pivot suggestions in more detail"
        - label: "Generate new ideas"
          description: "Run /generate-idea to discover fresh opportunities"
        - label: "I'm done for now"
          description: "I'll review the report and decide later"
      multiSelect: false
```

### Handle Sub-Skill Invocations

If the user selects "Create an execution plan," invoke the `plan-execution` skill:

```
Task: "Run the plan-execution skill on the idea document at {idea_file_path}.
Validation report available at: {validation_report_path}
{Include relevant user profile constraints if available from the idea document}"

skill: plan-execution
```

If the user selects "Validate a different idea," use `Glob` to find available idea files and present them via `AskUserQuestion`.

If the user selects "Generate new ideas," invoke the `generate-idea` skill:

```
Task: "Run the generate-idea skill to discover new startup and side-project ideas."

skill: generate-idea
```

---

## Error Handling

### Invalid Idea Path

Handled in Phase 1, Path B. The user is offered three options: fix the path, describe the idea conversationally, or search for existing idea files.

### Interview-Sourced Ideas with Sparse Detail

If the user provides very little detail during the interview and selects "I'm still figuring it out," proceed with validation but:

1. Note all gaps in the validation report's "Data Gaps and Limitations" section
2. Cap confidence at Low
3. Frame the validation as exploratory rather than definitive
4. Recommend running `/generate-idea` for a more structured discovery process

### Market Researcher Agent Failure

If the market-researcher agent fails, times out, or returns an error:

1. **Report partial findings**: Use whatever data was gathered before the failure
2. **Fall back to existing data**: Use MARKET.md data if available
3. **Adjust confidence**: Reduce by one level
4. **Note in report**: Document the research failure in the "Data Gaps and Limitations" section
5. **Continue validation**: Do not abort -- proceed with limited data and appropriate confidence adjustments
6. **Inform the user**: During Phase 10, note that research was limited and recommend follow-up

### Insufficient Web Research Data

If web research yields limited or no useful data:

1. **State gaps explicitly** in every affected section of the report
2. **Adjust confidence level** accordingly (typically to Low)
3. **Do not fabricate data** -- present what is known and clearly mark unknowns
4. **Recommend follow-up research** in the "Data Gaps and Limitations" section
5. **Include the idea's self-reported data** (from the idea document) with a note that it is unverified

### External or Interview-Sourced Ideas with No Context

When validating an idea with no prior research, no user profile, and no MARKET.md (whether from a file or the conversational interview):

1. Conduct all research from scratch via the market-researcher agent
2. Skip founder-specific analysis (Personal Fit, founder skills, network advantage)
3. Note the limitation in the report header using the External Idea Notice block
4. Apply the confidence reduction for external ideas (Section 5.3 of validation criteria)
5. Recommend running `/generate-idea` for a more complete analysis pipeline

---

## Reference Files

This skill loads the following reference files at the indicated workflow phases:

- `references/validation-criteria.md` -- Validation frameworks: SWOT, competitor analysis, risk assessment, devil's advocate, go/no-go decision, scenarios, demand signals, revenue modeling (loaded in Phase 2)
- `references/templates/validation-report.md` -- Template for validation report output files (loaded in Phase 2, used in Phase 9)
- `../../agents/market-researcher.md` -- Market research agent definition (used in Phase 3)
