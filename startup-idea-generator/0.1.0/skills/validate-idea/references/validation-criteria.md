# Validation Criteria

This reference defines the analysis frameworks and decision patterns used by the `validate-idea` skill to perform deep-dive validation of a startup or side-project idea. It covers SWOT analysis, competitor comparison, risk assessment, devil's advocate challenges, go/no-go decision criteria, scenario modeling, demand signal analysis, and qualitative revenue modeling.

All frameworks are designed for solo-developer and small-team contexts. The validate-idea skill should adapt its depth based on the data available -- when evidence is strong, go deep; when evidence is sparse, acknowledge gaps and adjust confidence accordingly.

---

## 1. SWOT Analysis Framework

The SWOT analysis provides a structured overview of an idea's internal strengths/weaknesses and external opportunities/threats, tailored to the startup and solo-developer context.

### 1.1 Structure

Organize the SWOT analysis into a 2x2 matrix:

```
                  Helpful                          Harmful
            +-----------------------+    +-----------------------+
  Internal  |     STRENGTHS         |    |     WEAKNESSES        |
  (idea +   | What the idea and     |    | What limits the idea  |
   founder) | founder do well       |    | or founder            |
            +-----------------------+    +-----------------------+
  External  |     OPPORTUNITIES     |    |     THREATS           |
  (market + | External factors that |    | External factors that |
   environ) | could help            |    | could hurt            |
            +-----------------------+    +-----------------------+
```

### 1.2 Startup-Specific Guidance

When populating each quadrant, evaluate the following dimensions:

#### Strengths (Internal, Helpful)

Focus on advantages the founder and the specific idea bring to the table:

| Dimension | Guiding Questions |
|-----------|-------------------|
| **Founder skills** | Does the founder have direct technical skills to build this? How much of the stack is within their expertise? |
| **Domain knowledge** | Does the founder have professional or personal experience in this market? Can they speak the customer's language? |
| **Network advantage** | Does the founder know potential customers, advisors, or distribution partners? |
| **Unfair advantage** | Is there something about this specific founder + idea combination that a generic competitor cannot replicate? |
| **Speed to market** | Can an MVP be built and shipped significantly faster than funded competitors could react? |
| **Cost structure** | Can this be built and operated at near-zero cost initially (serverless, free tiers, bootstrapped)? |

#### Weaknesses (Internal, Harmful)

Focus on limitations and resource gaps:

| Dimension | Guiding Questions |
|-----------|-------------------|
| **Skill gaps** | What critical skills does the founder lack (design, marketing, sales, specific tech)? |
| **Time constraints** | How many hours per week can the founder dedicate? Is the timeline realistic for the opportunity? |
| **Budget constraints** | What financial limits exist for infrastructure, marketing, or tooling? |
| **No brand or audience** | Does the founder start from zero in terms of reputation in this market? |
| **Single point of failure** | As a solo developer, what happens during illness, burnout, or life events? |
| **Missing capabilities** | Are there critical non-technical capabilities needed (e.g., regulatory knowledge, content creation, sales)? |

#### Opportunities (External, Helpful)

Focus on market and environmental tailwinds:

| Dimension | Guiding Questions |
|-----------|-------------------|
| **Market trends** | Is the target market growing? Are there macro trends (regulation changes, technology shifts, behavioral changes) creating new demand? |
| **Competitor gaps** | What do existing solutions do poorly? Where are customers visibly frustrated? |
| **Underserved segments** | Are there customer segments that current solutions ignore or poorly serve? |
| **Technology enablers** | Are new technologies (APIs, AI models, platforms) making this idea newly feasible or significantly cheaper to build? |
| **Distribution channels** | Are there untapped channels to reach the target audience (communities, marketplaces, partnerships)? |
| **Timing** | Why is now the right time for this idea? What has changed recently? |

#### Threats (External, Harmful)

Focus on external risks and competitive dynamics:

| Dimension | Guiding Questions |
|-----------|-------------------|
| **Incumbent response** | Could an established player easily add this feature or acquire a competitor? |
| **Market saturation** | Is the market already crowded with similar solutions? |
| **Customer acquisition cost** | Is it expensive or difficult to reach and convert the target audience? |
| **Platform dependency** | Does the idea depend on a platform (app store, API provider, marketplace) that could change terms or compete directly? |
| **Regulatory risk** | Are there existing or pending regulations that could impact the business model? |
| **Market timing** | Could the market window close before the product is ready (e.g., a trend fading, a technology being commoditized)? |

### 1.3 Output Format

Present the SWOT analysis in the validation report as follows:

```markdown
## SWOT Analysis

### Strengths
- {Specific strength with evidence or reasoning}
- {Another strength}

### Weaknesses
- {Specific weakness with impact assessment}
- {Another weakness}

### Opportunities
- {Specific opportunity with market evidence}
- {Another opportunity}

### Threats
- {Specific threat with likelihood and severity assessment}
- {Another threat}
```

Each bullet should be specific and evidence-backed where possible, not generic platitudes. Reference web research findings, competitor data, and the user's profile.

---

## 2. Competitor Analysis Patterns

The competitor analysis provides a structured comparison of existing solutions in the idea's market, identifying gaps, differentiation opportunities, and competitive dynamics.

### 2.1 Competitor Identification

Identify competitors in three tiers:

| Tier | Definition | How to Find |
|------|-----------|-------------|
| **Direct** | Products solving the same problem for the same audience | Search: "{idea description} software/tool/app," "best {category} tools {current_year}" |
| **Adjacent** | Products solving a related problem or serving a nearby audience | Search: "{broader category} tools," "{audience} software" |
| **Potential** | Large players who could easily enter this space | Search: "{large company} + {category}," recent acquisition activity in the space |

### 2.2 Competitor Comparison Table Format

Build a structured comparison table with the following columns:

```markdown
## Competitor Comparison

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
```

### 2.3 Data Collection Guidance

For each competitor, gather data from these sources via the `market-researcher` agent:

| Data Point | Sources |
|-----------|---------|
| **Pricing** | Pricing pages, G2/Capterra listings, Product Hunt launches |
| **Features** | Product pages, documentation, changelog, comparison pages |
| **Traction** | SimilarWeb estimates, app store reviews count, social media followers, job postings |
| **Funding** | Crunchbase, PitchBook, press releases, LinkedIn company size |
| **Weaknesses** | Negative reviews (G2, Capterra, Reddit), support forums, social media complaints |

### 2.4 Competitive Positioning Analysis

After building the comparison table, synthesize the findings into a positioning statement:

1. **Where competitors cluster**: What approach do most competitors take? (e.g., "Most competitors are enterprise-focused SaaS platforms priced at $50+/user/month")
2. **Where the gap exists**: What is the underserved position? (e.g., "No solution targets solo freelancers at a sub-$20/month price point")
3. **Your proposed position**: Where does this idea fit relative to competitors?
4. **Defensibility assessment**: How easy would it be for existing competitors to move into your position?

### 2.5 Handling Insufficient Competitive Data

When web research yields limited competitor information:

1. **Acknowledge the data gap explicitly** in the validation report
2. **Document what searches were attempted** and what was found
3. **Adjust the confidence level** of the competitive analysis (see Section 5 -- Go/No-Go Framework)
4. **Pivot to demand signal analysis** if no direct competitors exist (see Section 7)
5. **Do not fabricate competitor data** -- present what is known and clearly mark unknowns

---

## 3. Risk Assessment Framework

The risk assessment identifies, categorizes, and evaluates threats to the idea's success. Each risk is assessed on likelihood and impact to prioritize mitigation efforts.

### 3.1 Risk Categories

Evaluate risks across four categories:

#### Market Risk

Risks related to customer demand and market dynamics.

| Risk | Guiding Questions | Red Flags |
|------|-------------------|-----------|
| **No real demand** | Is there evidence that people actually want this? Are they searching for it? Complaining about alternatives? | No search volume, no forum discussions, no competitor traction |
| **Market too small** | Is the addressable market large enough to support the revenue goals? | Niche too narrow, total addressable audience in the hundreds |
| **Timing wrong** | Is the market ready for this now? Is it too early or too late? | Trend is declining, technology not yet mature, customer behavior hasn't shifted |
| **Customer acquisition** | Can the target customers be reached affordably? | No clear channel, high CAC, audience is fragmented or hard to identify |
| **Willingness to pay** | Will customers pay for this, or do they expect it to be free? | Free alternatives dominate, target audience has low purchasing power |

#### Technical Risk

Risks related to building and maintaining the product.

| Risk | Guiding Questions | Red Flags |
|------|-------------------|-----------|
| **Complexity underestimated** | Is the core technical challenge harder than it appears? | Requires ML/AI with no training data, complex integrations, real-time processing at scale |
| **Platform dependency** | Does the product depend on third-party APIs, platforms, or data sources? | Single API dependency, no fallback, terms of service could change |
| **Scaling challenges** | Will the architecture handle growth without a rewrite? | Data-intensive workloads, multi-tenancy complexity, real-time requirements |
| **Maintenance burden** | How much ongoing effort is needed to keep the product running? | Data freshness requirements, compliance updates, API version tracking |
| **Technical moat** | Is the technical approach easily replicable? | No proprietary data, standard tech stack, no switching costs |

#### Execution Risk

Risks related to the founder's ability to deliver.

| Risk | Guiding Questions | Red Flags |
|------|-------------------|-----------|
| **Skill gaps** | Does the founder lack critical skills for this idea? | Requires design expertise the founder lacks, needs sales skills, regulatory knowledge |
| **Time to market** | Can the MVP be built before the market window closes or motivation fades? | MVP estimate exceeds 3 months for a solo developer, scope creep risk |
| **Solo bottleneck** | Are there tasks that require multiple people working simultaneously? | Content creation + development + marketing needed in parallel |
| **Burnout risk** | Is the project scope sustainable alongside the founder's other commitments? | Exceeds stated time availability, requires sustained intensity |
| **Distribution** | Can the founder reach customers without a marketing team or budget? | No existing audience, no community presence, no organic distribution channel |

#### Financial Risk

Risks related to revenue, costs, and sustainability.

| Risk | Guiding Questions | Red Flags |
|------|-------------------|-----------|
| **Revenue timeline** | How long before the product generates meaningful revenue? | Long free trial expectations, slow sales cycle, enterprise sales process |
| **Unit economics** | Do the per-customer economics work at the expected price point? | High infrastructure cost per user, low price point with high support needs |
| **Upfront investment** | What costs are required before any revenue is generated? | Expensive APIs, required certifications, mandatory infrastructure |
| **Pricing pressure** | Could competitor pricing compress margins? | Race to the bottom in the category, free open-source alternatives emerging |
| **Runway** | Can the founder sustain development until revenue covers costs? | No savings buffer, expensive infrastructure from day one |

### 3.2 Risk Scoring Matrix

For each identified risk, assess:

| Factor | Scale | Description |
|--------|-------|-------------|
| **Likelihood** | Low / Medium / High | How likely is this risk to materialize? |
| **Impact** | Low / Medium / High | If it materializes, how severely would it affect the idea? |

Combine into a priority classification:

| Likelihood \ Impact | Low Impact | Medium Impact | High Impact |
|---------------------|-----------|--------------|-------------|
| **High Likelihood** | Monitor | Mitigate | Critical |
| **Medium Likelihood** | Accept | Monitor | Mitigate |
| **Low Likelihood** | Accept | Accept | Monitor |

### 3.3 Output Format

Present the top risks in the validation report:

```markdown
## Risk Assessment

### Top 3 Risks

1. **{Risk name}** ({Category})
   - Likelihood: {High/Medium/Low}
   - Impact: {High/Medium/Low}
   - Priority: {Critical/Mitigate/Monitor/Accept}
   - Description: {What the risk is and why it matters}
   - Mitigation: {Suggested approach to reduce likelihood or impact}

2. **{Risk name}** ({Category})
   ...

3. **{Risk name}** ({Category})
   ...

### Full Risk Register

| Risk | Category | Likelihood | Impact | Priority | Mitigation |
|------|----------|-----------|--------|----------|------------|
| ... | Market | High | High | Critical | ... |
| ... | Technical | Low | Medium | Accept | ... |
```

---

## 4. Devil's Advocate Patterns

The devil's advocate analysis actively challenges the idea to counter confirmation bias. Rather than looking for reasons the idea will succeed, this section searches for reasons it might fail.

### 4.1 Structured Challenge Questions

Work through these challenge categories systematically. For each question, search for evidence that supports the negative case.

#### Market Challenges

| Challenge Question | What to Search For |
|-------------------|--------------------|
| Why has nobody built this yet? | Search for failed startups in this space, reasons the market may be unviable |
| What if the problem is not painful enough to pay for? | Search for free alternatives, workarounds, evidence that people tolerate the status quo |
| What if the market is smaller than it appears? | Search for actual market size data, check if TAM estimates are inflated |
| What if the timing is wrong? | Search for market readiness signals, technology adoption curves, regulatory timelines |
| Who will be the first 10 customers, specifically? | Assess whether the founder can name or describe 10 real people/companies who would buy |

#### Competitive Challenges

| Challenge Question | What to Search For |
|-------------------|--------------------|
| What stops a well-funded competitor from copying this in 3 months? | Assess defensibility: proprietary data, network effects, switching costs, brand |
| What if the perceived competitor gap is intentional? | Search for competitor strategy: maybe they tested and rejected this feature/market segment |
| What if a big player announces this feature tomorrow? | Check roadmaps, recent acquisitions, and patent filings of adjacent large companies |
| Is the differentiation real or cosmetic? | Honestly assess whether the difference is meaningful to customers or just visible to the builder |

#### Execution Challenges

| Challenge Question | What to Search For |
|-------------------|--------------------|
| What is the hardest part of building this, and does the founder know how to do it? | Identify the single hardest technical or business challenge and assess founder readiness |
| What if it takes 3x longer than estimated? | Assess whether the opportunity still exists in 3x the timeline; check for deadline-dependent factors |
| What if the founder loses motivation at 60% completion? | Check if the idea has intrinsic interest to the founder or is purely opportunistic |
| What is the minimum viable distribution strategy? | Assess whether there is a plausible path to first users without a marketing budget |

#### Financial Challenges

| Challenge Question | What to Search For |
|-------------------|--------------------|
| What if customers will pay half of the expected price? | Reassess unit economics at 50% of projected price; check if the business still works |
| What if customer acquisition costs are 5x higher than expected? | Search for actual CAC benchmarks in the category |
| What if it takes 12 months to reach first revenue? | Assess founder's financial runway and willingness to sustain unpaid effort |

### 4.2 The "Pre-Mortem" Exercise

Conduct a structured pre-mortem by assuming the idea has failed after 12 months and working backward:

1. **Assume failure**: "It is 12 months from now. The product was launched but failed. What went wrong?"
2. **Generate failure scenarios**: List 5-7 plausible reasons for failure
3. **Assess plausibility**: For each scenario, rate how plausible it is (high/medium/low)
4. **Identify preventable failures**: Which failure scenarios could be mitigated or avoided with changes now?
5. **Recommend adjustments**: For each preventable failure, suggest a pivot, scope change, or mitigation

### 4.3 Output Format

```markdown
## Devil's Advocate Analysis

### Key Challenges

1. **{Challenge}**: {Evidence or reasoning for why this could cause failure}
   - Severity: {High/Medium/Low}
   - Counterargument: {The best case for why this concern may be overblown}
   - Suggested mitigation: {What to do about it}

2. ...

### Pre-Mortem: Why This Idea Could Fail

| Failure Scenario | Plausibility | Preventable? | Suggested Adjustment |
|-----------------|-------------|-------------|---------------------|
| {Scenario} | {High/Med/Low} | {Yes/No} | {Adjustment or "Accept risk"} |
| ... | ... | ... | ... |
```

---

## 5. Go/No-Go Decision Framework

The go/no-go recommendation synthesizes all validation findings into an actionable decision with a clear confidence level.

### 5.1 Recommendation Levels

| Recommendation | Meaning | When to Apply |
|---------------|---------|---------------|
| **Go** | The idea has strong validation signals and manageable risks. Proceed to execution planning. | SWOT balance is positive, top risks are mitigable, competitive position is defensible, demand evidence exists |
| **Conditional Go** | The idea has potential but specific concerns must be addressed first. Proceed only after resolving the stated conditions. | Some validation signals are strong but key uncertainties remain (e.g., pricing needs testing, one critical risk needs mitigation plan) |
| **No-Go** | The idea has significant unresolved risks or insufficient evidence of viability. Do not proceed in current form. | Critical risks with no mitigation path, no evidence of demand, competitive position is indefensible, idea requires resources the founder lacks |
| **Pivot Recommended** | The core insight has merit but the current formulation is flawed. Suggest a specific alternative direction. | Demand exists but for a different segment, the technical approach is wrong, the pricing model needs fundamental change |

### 5.2 Confidence Levels

Each recommendation carries a confidence level indicating how much trust to place in the assessment:

| Confidence | Meaning | Criteria |
|-----------|---------|----------|
| **High** | Strong evidence supports the recommendation. Multiple independent data points confirm the assessment. | 3+ competitors analyzed with pricing data, demand signals confirmed via multiple sources, SWOT based on researched evidence, risk mitigations identified |
| **Medium** | Moderate evidence supports the recommendation. Some data points are present but gaps exist. | 1-2 competitors analyzed, some demand signals found, SWOT partially evidence-based, some risks are uncertain |
| **Low** | Limited evidence available. The recommendation is based primarily on reasoning and limited data. | Few or no competitors found to analyze, demand signals are weak or absent, SWOT based largely on assumptions, significant unknowns remain |

### 5.3 Confidence Adjustment Rules

Adjust the confidence level based on data availability:

| Condition | Adjustment |
|-----------|-----------|
| Web research returned no competitor data | Reduce confidence by one level |
| Demand signals are based on a single source | Reduce confidence by one level |
| Founder has direct domain experience validating assumptions | Increase confidence by one level |
| Multiple independent data sources confirm a finding | Increase confidence by one level |
| Market is brand-new with no precedent | Cap confidence at Medium unless demand signals are strong |
| Idea was not generated by generate-idea (external idea, no profile data) | Note that Personal Fit assessment is unvalidated; reduce confidence by one level if fit is a critical factor |

### 5.4 Decision Criteria Checklist

Walk through this checklist to arrive at the recommendation:

```markdown
### Go/No-Go Assessment

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

**Recommendation**: {Go / Conditional Go / No-Go / Pivot Recommended}
**Confidence**: {High / Medium / Low}
**Reasoning**: {2-3 sentences summarizing the key factors driving the recommendation}
```

### 5.5 Conditions for Conditional Go

When recommending a Conditional Go, list specific, actionable conditions:

- Each condition must be **testable** (the user can determine whether it is met)
- Each condition must be **achievable** (within the user's capabilities and resources)
- Limit to **3 conditions maximum** to keep the action clear

Example:
```markdown
**Recommendation**: Conditional Go
**Conditions**:
1. Validate pricing with 5 potential customers before building (target: $X/month acceptable)
2. Confirm that the {specific API} provides the data granularity needed for the core feature
3. Identify at least 2 organic distribution channels where the target audience gathers
```

---

## 6. Bull Case / Bear Case Scenarios

Scenario analysis presents both the optimistic and pessimistic outcomes for an idea, helping the user understand the range of possible futures.

### 6.1 Bull Case (Optimistic Scenario)

The bull case assumes things go well -- not perfectly, but favorably. It answers: "What does success look like if key assumptions prove correct?"

Structure the bull case around:

| Element | Description |
|---------|-------------|
| **Key assumptions that hold** | Which 2-3 assumptions must be true for this scenario? |
| **Market response** | How do target customers react? Adoption trajectory. |
| **Competitive dynamics** | How does the competitive landscape play out favorably? |
| **Revenue trajectory** | Qualitative description of revenue growth (see Section 8 for revenue modeling) |
| **Timeline** | How long does it take to reach meaningful milestones? |
| **End state (12-18 months)** | What does the product and business look like if the bull case plays out? |

### 6.2 Bear Case (Pessimistic Scenario)

The bear case assumes things go poorly -- not catastrophically, but unfavorably. It answers: "What does partial failure look like?"

Structure the bear case around:

| Element | Description |
|---------|-------------|
| **Key assumptions that break** | Which 2-3 assumptions fail, and why? |
| **Market response** | How do target customers fail to adopt? What signals indicate this? |
| **Competitive dynamics** | How does the competitive landscape work against the idea? |
| **Sunk cost** | What has the founder invested (time, money) by the time failure is apparent? |
| **Salvageable value** | What can be reused, pivoted, or learned from the effort? |
| **Exit points** | At what milestones should the founder consider abandoning or pivoting? |

### 6.3 Output Format

```markdown
## Scenario Analysis

### Bull Case
**If these assumptions hold:**
- {Assumption 1}
- {Assumption 2}
- {Assumption 3}

**Then:**
- {Month 1-3}: {Early traction description}
- {Month 3-6}: {Growth description}
- {Month 6-12}: {Maturity description}
- **End state**: {What success looks like at 12-18 months}

### Bear Case
**If these assumptions break:**
- {Assumption 1 fails because...}
- {Assumption 2 fails because...}

**Then:**
- {Month 1-3}: {Early warning signs}
- {Month 3-6}: {Failure signals}
- **Sunk cost**: {Time and money invested before failure is clear}
- **Salvageable value**: {What can be reused or pivoted}
- **Exit decision point**: {When the founder should reassess}
```

### 6.4 Scenario Balance

Both scenarios must be grounded and plausible:

- The bull case should not be fantasy -- it should be achievable given the evidence
- The bear case should not be doom -- it should represent a realistic downside, not a worst-case catastrophe
- Both scenarios should reference specific evidence from the competitive analysis, demand signals, and risk assessment
- The gap between bull and bear cases indicates the uncertainty level of the idea

---

## 7. Demand Signal Analysis

When an idea targets a brand-new market with no direct competitors, traditional competitor analysis cannot be performed. In this case, pivot to demand signal analysis to assess whether latent demand exists.

This section also applies when the `validate-idea` skill receives an idea that was not generated by `generate-idea` (an external idea) -- the same demand signal techniques help validate ideas regardless of their origin.

### 7.1 When to Use Demand Signal Analysis

| Situation | Trigger |
|-----------|---------|
| No direct competitors found | Competitor search returns 0-1 results |
| Brand-new market category | The idea creates a new category with no established players |
| External idea with no prior research | Idea not generated by `generate-idea`; no prior market analysis exists |
| Competitor data is insufficient | Competitors exist but no pricing, traction, or feature data is available |

### 7.2 Search Trend Analysis

Use the `market-researcher` agent to assess search-based demand signals:

| Signal | What to Search | Interpretation |
|--------|---------------|----------------|
| **Search volume trends** | Google Trends for the problem keywords, category terms, and related queries | Rising trend = growing awareness; flat = stable demand; declining = waning interest |
| **Question volume** | "{problem} how to," "{problem} solution," "{problem} help" on Q&A sites | High question volume = real pain; low volume = may not be a priority |
| **Comparison searches** | "{category} vs," "{tool} alternative," "best {category}" | Presence of comparison searches = active buying intent; absence = early or nonexistent market |

### 7.3 Forum and Community Activity

Search for demand signals in online communities:

| Source | What to Search | Positive Signal | Negative Signal |
|--------|---------------|-----------------|-----------------|
| **Reddit** | Relevant subreddits for the problem description, complaints, "wish there was" | Active threads, upvoted complaints, requests for solutions | No relevant threads, low engagement |
| **Hacker News** | "Ask HN" or "Show HN" posts related to the problem | Discussion threads with engagement, people describing the pain | No relevant posts or low interest |
| **Stack Overflow / forums** | Technical questions related to the problem | Frequent questions, active discussion | No relevant questions |
| **Twitter/X** | Complaints, feature requests, "someone should build" | Viral threads about the problem, repeated mentions | No organic discussion |
| **Industry forums** | Niche community discussions about the problem | Active threads, professionals discussing workarounds | Silence on the topic |

### 7.4 Adjacent Market Analysis

When no direct competitors exist, analyze adjacent markets for demand signals:

1. **Identify the closest existing category**: What is the nearest product category to this idea?
2. **Assess spillover demand**: Are customers in the adjacent market asking for features that this idea addresses?
3. **Check migration patterns**: Are users leaving adjacent products because of limitations that this idea solves?
4. **Evaluate market evolution**: Is the adjacent market evolving toward the space this idea occupies?

### 7.5 Demand Signal Scoring

Synthesize demand signals into an overall assessment:

| Signal Strength | Indicators | Interpretation |
|----------------|------------|----------------|
| **Strong** | Multiple independent sources confirm demand (search trends + forum activity + adjacent market spillover) | Market likely exists even without established competitors; opportunity may be early-stage |
| **Moderate** | Some signals present but inconsistent (e.g., forum interest but flat search trends) | Demand may exist but is unproven; higher risk, proceed with extra validation |
| **Weak** | Few or no signals found across any source | Demand is unconfirmed; idea may be too early, too niche, or solving a nonexistent problem |

### 7.6 Output Format

```markdown
## Demand Signal Analysis

> **Note**: No direct competitors were found for this idea. This analysis assesses latent demand signals in lieu of traditional competitor comparison.

### Search Trends
- {Keyword}: {Trend direction and volume assessment}
- {Keyword}: {Trend direction and volume assessment}

### Community Activity
- {Source}: {Summary of relevant discussions, engagement level}
- {Source}: {Summary of relevant discussions, engagement level}

### Adjacent Markets
- {Adjacent category}: {Relevance and spillover demand assessment}

### Overall Demand Signal: {Strong / Moderate / Weak}
{2-3 sentence summary of what the signals indicate about market viability}
```

---

## 8. Qualitative Revenue Model Scenarios

Rather than producing quantitative financial projections (which carry false precision for early-stage ideas), the validate-idea skill describes revenue potential qualitatively. The goal is to help the user understand the revenue characteristics of the idea without implying specific dollar amounts.

### 8.1 Revenue Model Characteristics

For each idea, describe the following qualitative characteristics:

| Characteristic | Description | Example Descriptors |
|---------------|-------------|-------------------|
| **Revenue type** | How does the product generate money? | Subscription (recurring), one-time purchase, usage-based, marketplace commission, freemium with paid tier |
| **Revenue timing** | How quickly can the product start generating revenue after launch? | Immediate (day-one pricing), delayed (free period required to build user base), gradual (ramp-up as features expand) |
| **Revenue ceiling** | What is the realistic upper bound of revenue for a solo-operated product? | Lifestyle business ($5-20K/month ceiling), scalable SaaS (ceiling grows with users), agency-like (limited by founder's time) |
| **Revenue predictability** | How stable and predictable is the revenue stream? | High (monthly subscriptions), moderate (annual contracts), low (one-time purchases, project-based), variable (usage-based, seasonal) |
| **Path to first dollar** | What must happen between launch and first paying customer? | Direct (product solves immediate need, customers self-serve), indirect (requires education, demos, sales process), long (requires network effects or content volume) |

### 8.2 Comparable Revenue Patterns

Instead of projecting specific revenue numbers, reference comparable products and their known revenue patterns:

```markdown
### Revenue Comparables

| Comparable Product | Revenue Model | Known Data Point | Relevance |
|-------------------|--------------|-----------------|-----------|
| {Product name} | {Model} | {Public revenue data, if known: "reported $X ARR at Y users"} | {Why this is a useful comparison} |
| {Product name} | {Model} | {Data point} | {Relevance} |
```

Sources for comparable data:
- Indie Hackers revenue reports
- Open Startup pages
- Product Hunt launch metrics
- Public interviews with solo founders

### 8.3 Revenue Scenario Descriptions

Describe revenue scenarios in qualitative terms tied to the bull/bear case analysis:

```markdown
### Revenue Outlook

**Bull case revenue pattern**: {Description of revenue trajectory if things go well}
Example: "With strong product-market fit, this subscription SaaS could reach a sustainable lifestyle-business revenue level within 6-12 months, similar to comparable tools in the {category} space that report $5-15K MRR as solo operations."

**Bear case revenue pattern**: {Description of revenue trajectory if things go poorly}
Example: "If customer acquisition proves harder than expected, the product may struggle to convert free users to paid, resulting in minimal revenue after 6+ months of development effort."

**Key revenue driver**: {The single most important factor determining revenue success}
Example: "Revenue is gated primarily on conversion rate from free to paid tier. If the core feature delivers enough value to justify the price point, the subscription model provides predictable recurring revenue."
```

### 8.4 Revenue Model Risk Flags

Flag specific revenue model risks:

| Risk Flag | Description | When to Flag |
|-----------|-------------|-------------|
| **Free alternative pressure** | Viable free alternatives exist that could suppress willingness to pay | Open-source tools, free tiers of large platforms, or ad-supported alternatives serve the same need |
| **Long time-to-revenue** | Significant development or user acquisition needed before first dollar | Marketplace models, content platforms, or products requiring network effects |
| **Founder-time ceiling** | Revenue is limited by the founder's available hours | Consulting-like models, done-for-you services, or products requiring heavy customer support |
| **Single-channel dependency** | Revenue depends on a single acquisition channel that could dry up | SEO-dependent, single-marketplace listing, or reliant on a single partnership |

---

## 9. Handling External Ideas

When the `validate-idea` skill receives an idea that was not generated by `generate-idea`, additional steps are needed because no user profile or prior market research exists.

### 9.1 Detection

An external idea is identified when:
- The idea document does not follow the `idea-NNN.md` format from generate-idea
- No corresponding `MARKET.md` exists in the parent directory
- The idea document lacks a Scoring section or Personal Fit assessment
- The user explicitly states the idea is not from generate-idea

### 9.2 Adaptation

When validating an external idea:

1. **Skip profile-dependent analysis**: Do not attempt Personal Fit assessment without a user profile. Note this gap in the validation report.
2. **Conduct full market research from scratch**: Do not assume any prior research exists. Run competitor identification, demand signal analysis, and market sizing independently.
3. **Apply all frameworks equally**: SWOT, risk assessment, devil's advocate, and scenario analysis all apply regardless of the idea's origin.
4. **Adjust confidence accordingly**: The lack of user profile data means Personal Fit cannot be assessed, which may affect the overall confidence level (see Section 5.3).
5. **Note the limitation**: Include a note in the validation report header:

```markdown
> **Note**: This idea was not generated by the `generate-idea` skill. Personal Fit assessment is unavailable. All other validation frameworks have been applied. Consider running `/generate-idea` first for a more complete analysis.
```

---

## 10. Integration with Validation Report

The validate-idea skill should produce a validation report that integrates all frameworks from this reference. The report structure follows this order:

1. **Summary**: One-paragraph overview of findings and recommendation
2. **SWOT Analysis** (Section 1)
3. **Competitor Comparison** (Section 2) or **Demand Signal Analysis** (Section 7) if no competitors
4. **Risk Assessment** (Section 3) -- top 3 risks highlighted
5. **Devil's Advocate Analysis** (Section 4)
6. **Scenario Analysis** -- Bull/Bear Cases (Section 6)
7. **Revenue Outlook** (Section 8)
8. **Go/No-Go Recommendation** (Section 5) -- final, informed by all preceding sections

Each section should reference specific evidence gathered during the validation process. Where data is insufficient, the section should explicitly state the gap and its impact on confidence.
