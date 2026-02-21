# Scoring Rubric

This reference defines the formal scoring system used to evaluate and rank startup/side-project ideas. Each idea is scored across 4 dimensions on a 1-5 scale, weighted to produce a composite score for objective comparison.

---

## Scoring Dimensions

### 1. Revenue Potential

Evaluates the idea's ability to generate meaningful income based on market size, pricing model viability, and customer willingness to pay.

| Score | Label | Indicators |
|-------|-------|------------|
| 1 | Very Low | Tiny addressable market (<$1M TAM); no clear pricing model; target users strongly resist paying; free alternatives dominate with no differentiation path |
| 2 | Low | Small market ($1M-$10M TAM); pricing model exists but weak (ad-supported, freemium with low conversion); limited willingness to pay; commoditized space |
| 3 | Moderate | Mid-size market ($10M-$100M TAM); viable pricing model (subscription, one-time purchase); some evidence of willingness to pay; competitive but differentiable |
| 4 | High | Large market ($100M-$1B TAM); strong pricing model with proven comparables; clear willingness to pay demonstrated by competitors' revenue; underserved segments exist |
| 5 | Very High | Very large market (>$1B TAM); premium pricing viable; strong evidence of willingness to pay (competitors thriving, growing spend in category); clear monetization path from day one |

### 2. Solo Buildability

Evaluates how feasible it is for a single developer to build, launch, and maintain the product given typical solo-developer constraints.

| Score | Label | Indicators |
|-------|-------|------------|
| 1 | Very Low | Requires large team or specialized expertise beyond one person; MVP would take 6+ months; heavy infrastructure/ops burden; regulatory or compliance barriers |
| 2 | Low | Significant complexity requiring deep domain expertise; MVP in 3-6 months; moderate infrastructure needs; some external dependencies (APIs, data sources) that are unreliable or expensive |
| 3 | Moderate | Achievable with effort; MVP in 1-3 months; standard tech stack; manageable infrastructure; some ongoing maintenance required but predictable |
| 4 | High | Well within solo-dev capability; MVP in 2-6 weeks; familiar tech stack; minimal infrastructure (serverless, managed services); low maintenance burden |
| 5 | Very High | Trivially buildable; MVP in under 2 weeks; simple architecture; near-zero infrastructure cost; minimal ongoing maintenance; can launch on existing platforms |

### 3. Market Validation

Evaluates the strength of evidence that real demand exists for this product, including competitor gaps, trend alignment, and observable demand signals.

| Score | Label | Indicators |
|-------|-------|------------|
| 1 | Very Low | No evidence of demand; no competitors (may indicate no market); no relevant search trends or community discussion; purely speculative |
| 2 | Low | Weak demand signals; few competitors but unclear if they have traction; flat or declining search trends; minimal community discussion |
| 3 | Moderate | Some demand evidence; competitors exist with modest traction; stable search trends; occasional community mentions; gap exists but unconfirmed if users would switch |
| 4 | High | Strong demand signals; competitors with clear revenue but identifiable gaps; growing search trends; active community requests for better solutions; evidence of underserved segments |
| 5 | Very High | Overwhelming demand evidence; competitors thriving but leaving major gaps; rapidly growing search trends; frequent community complaints about existing solutions; users actively seeking alternatives |

### 4. Personal Fit

Evaluates how well the idea aligns with the user's specific skills, interests, professional network, and personal circumstances.

| Score | Label | Indicators |
|-------|-------|------------|
| 1 | Very Low | No relevant skills; no interest in the domain; no network connections; idea conflicts with stated constraints (time, budget, risk tolerance) |
| 2 | Low | Tangential skills (significant learning curve); mild interest; no network in the space; partially conflicts with constraints |
| 3 | Moderate | Related skills (some learning required); genuine interest; weak network connections; aligns with constraints but no particular advantage |
| 4 | High | Strong relevant skills; high interest and enthusiasm; some network connections in the market; aligns well with stated constraints and goals |
| 5 | Very High | Expert-level skills directly applicable; passionate about the domain; strong existing network (potential early customers/advisors); perfectly matches constraints, goals, and income targets |

---

## Default Weights

Each dimension carries equal weight by default:

| Dimension | Default Weight |
|-----------|---------------|
| Revenue Potential | 25% |
| Solo Buildability | 25% |
| Market Validation | 25% |
| Personal Fit | 25% |

---

## Weight Adjustment

Users may customize dimension weights before scoring to reflect their priorities. Weights must always sum to 100%.

### Adjustment Process

1. Before scoring begins, present the default weights to the user via `AskUserQuestion`
2. Offer predefined presets (see below) as recommended options
3. Allow the user to set custom weights if none of the presets fit
4. Validate that weights sum to 100%
5. Apply the chosen weights for the entire scoring session

### Predefined Presets

| Preset Name | Revenue | Buildability | Validation | Personal Fit | Best For |
|-------------|---------|--------------|------------|--------------|----------|
| **Balanced** (default) | 25% | 25% | 25% | 25% | General exploration; no strong preference |
| **Revenue-focused** | 40% | 15% | 30% | 15% | Users prioritizing income potential above all else |
| **Quick-win** | 15% | 40% | 20% | 25% | Users wanting to ship fast and iterate; limited time |
| **Passion-project** | 15% | 20% | 20% | 45% | Users who value enjoyment and skill alignment over revenue |
| **Evidence-driven** | 20% | 15% | 45% | 20% | Users who want strong market proof before committing |

### AskUserQuestion Format

When presenting the weight choice, use `AskUserQuestion` with the following structure:

- **Question**: "How would you like to weight the scoring dimensions? Choose a preset or specify custom weights."
- **Recommended answers**: List each preset by name with a brief description, plus a "Custom weights" option
- **Default**: "Balanced (25% / 25% / 25% / 25%)"

If the user selects "Custom weights," follow up with a second `AskUserQuestion` asking for the four weight values (must sum to 100%).

---

## Composite Score Calculation

The composite score is a weighted average of the four dimension scores:

```
Composite Score = (W_r * S_r) + (W_b * S_b) + (W_v * S_v) + (W_f * S_f)
```

Where:
- `W_r` = Revenue Potential weight (decimal, e.g., 0.25)
- `S_r` = Revenue Potential score (1-5)
- `W_b` = Solo Buildability weight (decimal)
- `S_b` = Solo Buildability score (1-5)
- `W_v` = Market Validation weight (decimal)
- `S_v` = Market Validation score (1-5)
- `W_f` = Personal Fit weight (decimal)
- `S_f` = Personal Fit score (1-5)

**Score range**: 1.00 to 5.00

### Example Calculation

Given scores: Revenue=4, Buildability=5, Validation=3, Fit=4 with default weights:

```
Composite = (0.25 * 4) + (0.25 * 5) + (0.25 * 3) + (0.25 * 4)
          = 1.00 + 1.25 + 0.75 + 1.00
          = 4.00
```

---

## Tiebreaker Rules

When two or more ideas share the same composite score, apply tiebreakers in the following order:

1. **Higher Market Validation score wins** -- ideas with stronger evidence of demand are preferred since they carry less risk
2. **Higher Revenue Potential score wins** -- among equally validated ideas, higher revenue ceiling is preferred
3. **Higher Personal Fit score wins** -- among ideas equal on validation and revenue, better fit increases likelihood of follow-through
4. **Higher Solo Buildability score wins** -- all else equal, the easier-to-build idea reduces execution risk
5. **If still tied**: Present ideas as equally ranked and let the user decide based on qualitative factors

---

## Insufficient Data Handling

When a dimension cannot be scored due to insufficient information (e.g., no market data found, web search returned no results, user profile incomplete for fit assessment):

1. **Assign a default score of 3** for the dimension
2. **Flag the dimension** with an `[insufficient data]` marker in the scoring output
3. **Document what data was missing** in the scoring rationale (e.g., "No competitor pricing data found; defaulting to 3")
4. **Inform the user** that the composite score may be less reliable for this idea
5. **Do not exclude the idea** from ranking -- present it alongside fully-scored ideas with the data gaps visible

### Display Format

In the ranked idea list, ideas with insufficient-data flags should show the marker next to the affected dimension:

```
Idea: WidgetSync
  Revenue Potential:   4/5
  Solo Buildability:   5/5
  Market Validation:   3/5 [insufficient data]
  Personal Fit:        4/5
  Composite Score:     4.00
  * Market Validation scored with insufficient data -- consider additional research
```

---

## Scoring Rationale Format

Each idea document must include a scoring rationale section that documents the reasoning behind each dimension's score. This provides transparency and allows users to challenge or adjust scores.

### Format in Idea Documents

Include the following section in each `idea-NNN.md` file:

```markdown
## Scoring

**Weight Preset**: {preset name or "Custom"}
**Weights**: Revenue {W_r}% | Buildability {W_b}% | Validation {W_v}% | Fit {W_f}%

| Dimension | Score | Rationale |
|-----------|-------|-----------|
| Revenue Potential | {1-5}/5 | {2-3 sentence explanation citing specific evidence: market size data, competitor pricing, willingness-to-pay signals} |
| Solo Buildability | {1-5}/5 | {2-3 sentence explanation: tech stack assessment, estimated MVP timeline, infrastructure needs, maintenance considerations} |
| Market Validation | {1-5}/5 | {2-3 sentence explanation: demand signals found, competitor gaps identified, trend data, community evidence} |
| Personal Fit | {1-5}/5 | {2-3 sentence explanation: skills match from user profile, interest alignment, network connections, constraint compatibility} |

**Composite Score**: {score}/5.00 {[insufficient data flag if applicable]}

{If any dimension has insufficient data:}
> **Data gaps**: {dimension} was scored with insufficient data. {What data was missing and what queries were attempted.} Consider running `/validate-idea` for deeper research.
```

### Rationale Quality Guidelines

Each rationale entry must:
- Reference **specific evidence** (not generic statements like "good market")
- Cite **sources** where applicable (competitor names, trend data, community URLs)
- Explain the **reasoning** that maps evidence to the chosen score level
- Note any **uncertainty** or assumptions made

Bad example: "Revenue Potential: 4/5 -- Good revenue potential in this market."
Good example: "Revenue Potential: 4/5 -- The project management SaaS market is estimated at $7B+ (TAM). Competitors like Linear ($35/user/mo) and Shortcut ($8.50/user/mo) demonstrate willingness to pay. The niche of solo-dev-focused PM tools is underserved, with Todoist being the closest but lacking dev-specific features."
