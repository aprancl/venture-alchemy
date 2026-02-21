# Anti-Slop Filters

This reference defines the three-layer anti-slop pipeline used by the `generate-idea` skill to prevent generic, oversaturated, and impersonal ideas from reaching the user. Each generated idea must pass through all three layers before being presented.

**Pipeline order:**
1. **Deep Personalization Filter** -- reject ideas that do not leverage the user's unique profile
2. **Contrarian Filter** -- reject "obvious" ideas that any generic brainstorming session would produce
3. **Freshness Filter** -- use web search to verify market saturation before presenting

An idea that fails any layer is either discarded or explicitly flagged with rationale. Ideas are never silently passed through.

---

## 1. Curated Generic Idea List (The "Slop List")

This list contains commonly generated, oversaturated ideas that LLMs and generic brainstorming sessions produce repeatedly. It serves as the baseline for the anti-slop effectiveness metric: fewer than 20% of generated ideas should overlap with entries on this list.

An idea "overlaps" with a slop list entry if it describes substantially the same product for the same audience, even with cosmetic rebranding. An idea that targets the same broad category but with a genuinely novel angle, underserved niche, or materially different approach is NOT considered overlapping (see [Novel Angle Exception](#novel-angle-exception)).

### 1.1 Productivity and Organization

| # | Generic Idea | Why It Is Slop |
|---|-------------|----------------|
| 1 | Todo / task management app | Thousands of alternatives exist (Todoist, Things, TickTick, etc.); no gap for a generic entrant |
| 2 | Note-taking app | Dominated by Notion, Obsidian, Apple Notes; extremely crowded |
| 3 | Habit tracker | Saturated market with 100+ apps; minimal differentiation path |
| 4 | Pomodoro / focus timer | Trivial to build, trivial to clone; no monetization path |
| 5 | Calendar / scheduling tool | Established players (Calendly, Cal.com, Google Calendar) with strong network effects |
| 6 | Bookmark manager | Repeated suggestion with low willingness to pay; browser built-ins suffice for most |
| 7 | Personal knowledge base / wiki | Overlaps with note-taking; Notion, Obsidian, Logseq already serve this |
| 8 | Daily journal / mood tracker | Low willingness to pay; saturated in app stores |

### 1.2 Finance and Budgeting

| # | Generic Idea | Why It Is Slop |
|---|-------------|----------------|
| 9 | Expense tracker / budgeting app | YNAB, Mint (now Credit Karma), Monarch, Copilot dominate; heavily regulated |
| 10 | Invoice generator | Commoditized; free tools abound (Wave, Zoho Invoice, PayPal) |
| 11 | Subscription tracker | Repeatedly suggested; Truebill/Rocket Money, Subly already cover this |
| 12 | Personal finance dashboard | Aggregation is hard (Plaid dependency), regulatory burden, and crowded |
| 13 | Tip / bill splitter | Trivial utility; built into most payment apps |

### 1.3 Developer Tools

| # | Generic Idea | Why It Is Slop |
|---|-------------|----------------|
| 14 | URL shortener | Solved problem; Bitly, short.io, and dozens of clones exist |
| 15 | Code snippet manager | GitHub Gists, Raycast snippets, and IDE extensions cover this |
| 16 | Portfolio website builder | Squarespace, Carrd, GitHub Pages; no gap for generic entrant |
| 17 | API documentation tool | Swagger, Readme, Mintlify well-established |
| 18 | Generic REST API boilerplate generator | Low barrier to cloning; free templates everywhere |
| 19 | Markdown editor | Typora, iA Writer, StackEdit; saturated and low willingness to pay |

### 1.4 AI Wrappers and Chatbots

| # | Generic Idea | Why It Is Slop |
|---|-------------|----------------|
| 20 | Generic AI chatbot / assistant | Directly competes with ChatGPT, Claude, Gemini; no differentiation |
| 21 | AI-powered writing assistant | Jasper, Copy.ai, Grammarly; established players with large datasets |
| 22 | AI resume / cover letter builder | Dozens of clones launched monthly; race to the bottom on pricing |
| 23 | AI image generator app | Wrapping DALL-E / Stable Diffusion APIs with no added value |
| 24 | AI customer support chatbot | Intercom, Zendesk, Freshdesk all have AI features now |
| 25 | AI email writer / auto-responder | Commoditized; built into Gmail, Outlook, and many CRM tools |

### 1.5 Lifestyle and Consumer

| # | Generic Idea | Why It Is Slop |
|---|-------------|----------------|
| 26 | Recipe / meal planning app | Mealime, Paprika, Eat This Much; saturated |
| 27 | Weather app | Relies on third-party data; Apple/Google built-ins are sufficient |
| 28 | Workout / fitness tracker | Strava, Strong, Fitbod, Nike Training Club; extremely crowded |
| 29 | Social media scheduler | Buffer, Hootsuite, Later; heavily competed market |
| 30 | Event planning / RSVP tool | Eventbrite, Luma, Partiful; cold-start problem for new entrants |
| 31 | Flash card / study app | Anki, Quizlet dominate; low willingness to pay for undifferentiated entry |
| 32 | Reading list / book tracker | Goodreads (Amazon-backed), StoryGraph, Literal; crowded niche |
| 33 | Password manager | 1Password, Bitwarden, LastPass; high trust barrier for new entrants |
| 34 | Grocery / shopping list app | Low willingness to pay; built into phone ecosystems |
| 35 | Travel itinerary planner | Wanderlog, TripIt, Google Travel; saturated with funded competitors |

### Novel Angle Exception

An idea that matches a slop list entry **by category** but has a genuinely novel angle should NOT be automatically rejected. Instead, it must pass a differentiation test:

1. **Specific niche**: The idea targets a narrow, underserved audience that existing solutions ignore (e.g., "habit tracker for chemotherapy patients tracking medication side effects" vs. "habit tracker")
2. **Unique mechanism**: The idea uses a fundamentally different approach that existing tools do not offer (e.g., "AI-powered expense categorization using bank transaction NLP for freelancers with mixed personal/business accounts" vs. "expense tracker")
3. **Unfair advantage**: The user has a specific advantage (domain expertise, network, data access) that makes their version defensible

If an idea passes at least one of these tests, it may proceed through the pipeline. However, it should carry a **"Caution: Adjacent to saturated market"** flag in the idea document's Saturation Assessment section, and the Competitive Landscape section must explicitly address how the idea differs from the saturated generic version.

---

## 2. Contrarian Filter Logic

The contrarian filter identifies and rejects "obvious" ideas -- those that any generic brainstorming session, LLM prompt, or startup idea list would produce. The goal is to surface non-obvious opportunities that leverage the user's unique circumstances.

### 2.1 Generic Idea Detection Rules

An idea triggers the contrarian filter if it matches **two or more** of the following signals:

| Signal | Description | Examples |
|--------|-------------|---------|
| **No niche specified** | The idea targets "everyone" or a mass-market audience without identifying a specific underserved segment | "A project management tool" (for whom?), "An AI assistant" (for what specifically?) |
| **No differentiation stated** | The idea description could apply equally to 5+ existing products without modification | "A better note-taking app with AI" -- how is it better, and for whom? |
| **Commonly generated pattern** | The idea follows a well-known template: "X but for Y," "Uber for Z," or "AI-powered [existing category]" without a novel mechanism | "Airbnb for storage," "AI-powered CRM" |
| **Technology-first, not problem-first** | The idea starts with a technology ("build something with GPT-4") rather than a specific user problem | "An app that uses computer vision" -- to solve what problem for whom? |
| **No user profile leverage** | The idea does not connect to any attribute of the user's profile (skills, experience, network, interests) | An idea a random person off the street could have generated equally well |
| **Generic SaaS pattern** | The idea is a standard B2B SaaS in a well-served category with no stated wedge | "A CRM for small businesses," "An HR tool for startups" |

### 2.2 Contrarian Signals (Positive Indicators)

An idea that exhibits the following signals is more likely to be non-obvious and valuable:

| Signal | Description | Why It Matters |
|--------|-------------|---------------|
| **Underserved niche** | Targets a specific, narrow audience that mainstream tools ignore | Smaller market but less competition; easier to reach |
| **Unique angle** | Approaches the problem from an unexpected direction | Harder for incumbents to copy; creates genuine differentiation |
| **Non-obvious market** | Targets a market that most people would not immediately think of | Lower competition because fewer people are looking there |
| **User's unfair advantage** | Leverages the specific user's skills, domain knowledge, network, or personal experience | Defensibility through expertise; faster time to value |
| **Problem-first framing** | Starts with a specific, validated pain point rather than a technology | Higher likelihood of product-market fit |
| **Counter-trend positioning** | Goes against the current conventional wisdom or hype cycle | Contrarian positions, when correct, face less competition |
| **Cross-domain combination** | Combines insights from two unrelated domains in a novel way | Creates new categories rather than competing in existing ones |

### 2.3 Filter Decision Matrix

| Generic Signals | Contrarian Signals | Decision |
|----------------|-------------------|----------|
| 0-1 generic signals | 1+ contrarian signals | **PASS** -- Idea is likely non-obvious |
| 2 generic signals | 2+ contrarian signals | **PASS with caution** -- Contrarian signals outweigh; flag for extra scrutiny in freshness filter |
| 2 generic signals | 0-1 contrarian signals | **REJECT** -- Idea is too generic; attempt to refine or replace |
| 3+ generic signals | Any | **REJECT** -- Idea is clearly generic; do not present |

### 2.4 Refinement Before Rejection

Before fully rejecting an idea, attempt one refinement pass:

1. Identify which generic signals triggered the filter
2. Cross-reference the user's profile for a possible niche, angle, or advantage that could be applied
3. If a refinement emerges that clears the contrarian filter, present the refined version instead
4. If no refinement is possible, discard the idea and generate a replacement

**Example refinement:**
- Original: "AI-powered customer support chatbot" (generic SaaS, no niche, no differentiation)
- User profile: 5 years in veterinary industry, knows clinic owners
- Refined: "AI triage assistant for veterinary clinics that pre-screens pet symptoms via chat before appointment booking, trained on veterinary intake forms"
- Result: Passes contrarian filter (underserved niche, user's unfair advantage, problem-first framing)

---

## 3. Freshness Filter Patterns

The freshness filter uses web search (via the `market-researcher` agent) to verify whether a generated idea enters a saturated market. This is the final layer of the anti-slop pipeline and provides real-world evidence to complement the heuristic-based contrarian filter.

### 3.1 Search Query Templates

For each idea that passes the contrarian filter, execute the following search queries to assess market saturation. Replace `{idea_description}` and `{market_niche}` with the specific idea details.

#### Competitor Count Queries

```
"{idea_description} software"
"{idea_description} tool"
"{idea_description} app"
"{market_niche} alternatives"
"best {idea_description} tools {current_year}"
"{idea_description} competitors"
"{market_niche} software comparison"
```

#### Market Saturation Signal Queries

```
"{market_niche} market saturated"
"{market_niche} too many competitors"
"{idea_description} crowded market"
"is {market_niche} a good market to enter"
"{market_niche} market size {current_year}"
```

#### Demand Signal Queries

```
"{market_niche} pain points"
"{market_niche} complaints {current_year}"
"looking for {idea_description}" site:reddit.com
"{idea_description} wish there was" site:reddit.com
"{market_niche} underserved"
```

### 3.2 Query Adaptation

Adapt query terms to the specific idea. For example:

| Idea Type | Query Adaptations |
|-----------|-------------------|
| B2B SaaS | Add industry vertical: "{idea} for {industry}" |
| Consumer app | Add platform: "{idea} app iOS," "{idea} app Android" |
| Developer tool | Add ecosystem: "{idea} for {language/framework}" |
| Marketplace | Search both supply and demand sides separately |
| API / Infrastructure | Search for "{idea} API," "{idea} as a service" |

### 3.3 Saturation Thresholds

Count the number of **established competitors** identified through search results. An "established competitor" is defined as a product that:
- Has a functional website or app store listing
- Appears to have active users (reviews, social media presence, recent updates)
- Has been operating for at least 6 months

| Competitor Count | Saturation Flag | Action |
|-----------------|-----------------|--------|
| 0-2 | **Low competition** | Pass. Note this could indicate no market OR a genuine gap. Check demand signals. |
| 3-5 | **Moderate competition** | Pass. Healthy market validation with room for a differentiated entrant. |
| 6-9 | **Moderate competition** (upper range) | Pass with caution. Differentiation must be clearly stated. |
| 10-14 | **High saturation** | Flag in idea document. Present only if differentiation is strong and user has unfair advantage. |
| 15+ | **High saturation** | Reject unless the idea has a genuinely unique angle (Novel Angle Exception) AND user has a clear unfair advantage. |

### 3.4 Saturation Flag Definitions

These flags appear in the idea document template's `Saturation Flag` field and `Saturation Assessment` section:

| Flag | Meaning | User Guidance |
|------|---------|--------------|
| **Low** | 0-2 established competitors found. Market may be early-stage or niche. | Validate that demand exists -- low competition could mean no market. Run `/validate-idea` for demand signal analysis. |
| **Moderate** | 3-9 established competitors found. Market is validated but not overcrowded. | Good sign. Focus differentiation on specific gaps competitors miss. |
| **High** | 10+ established competitors found. Market is crowded. | Proceed only with a clear, defensible wedge. The idea document must explain why this entrant would succeed despite saturation. |
| **Unknown** | Freshness filter could not determine saturation level. | Do NOT treat as "passed." Evaluate manually or run `/validate-idea` for deeper research. See [Handling Ambiguous Results](#35-handling-ambiguous-and-failed-search-results). |

### 3.5 Handling Ambiguous and Failed Search Results

The freshness filter may fail to produce a clear saturation assessment. In all failure modes, the idea must be flagged as **"saturation unknown"** -- never silently passed through as if it were checked.

#### When Web Search Fails Entirely

If the `market-researcher` agent cannot perform web searches (tool unavailable, network error, rate limited):

1. Flag the idea with **Saturation Flag: Unknown**
2. Set the Saturation Assessment's `Assessment basis` to: "Web search unavailable during freshness check"
3. Include the "Saturation Unknown" HTML comment block from the idea document template
4. **Do not discard the idea** -- present it with the flag and recommend the user run `/validate-idea` later
5. Log the failure for the session summary

#### When Search Returns Ambiguous Results

If search results are inconclusive (e.g., the idea description is too broad, results are for a different product category, or results are mixed):

1. **Attempt query refinement**: Narrow the search using more specific terms, the target niche, or the user's specific angle
2. **Try at least 3 query variations** before declaring ambiguity
3. If still ambiguous after refinement:
   - Flag the idea with **Saturation Flag: Unknown**
   - Set the `Assessment basis` to: "Search results inconclusive -- {brief explanation of why}"
   - Include specific query variations attempted
4. **Do not default to "Low competition"** when data is unclear -- "Unknown" is the honest assessment

#### When Competitor Count Is Borderline

If the competitor count falls exactly on a threshold boundary (e.g., exactly 10 competitors):

1. Examine the quality of competitors: Are they well-funded? Active? Growing?
2. Check how closely they match the specific idea (direct vs. adjacent competitors)
3. Apply the higher saturation flag if competitors are direct and active
4. Apply the lower flag if most competitors are adjacent or inactive
5. Document the reasoning in the Saturation Assessment

---

## 4. Deep Personalization Patterns

The personalization layer ensures every generated idea connects meaningfully to the user's unique profile. Ideas that any random person could have generated are deprioritized or rejected.

### 4.1 Profile Cross-Reference Requirements

Every idea must demonstrate a connection to **at least two** of the following profile attributes:

| Profile Attribute | Source | Cross-Reference Question |
|------------------|--------|--------------------------|
| **Technical skills** | Interview Q1.1-Q1.3 | Does this idea leverage the user's existing tech stack or strongest skills? |
| **Domain experience** | Interview Q2.1-Q2.2 | Does the user have professional experience in this idea's target market? |
| **Personal interests** | Interview Q3.1-Q3.2 | Does this idea connect to something the user is genuinely interested in? |
| **Professional network** | Interview Q8.1-Q8.2 | Does the user know potential customers, advisors, or collaborators in this space? |
| **Time constraints** | Interview Q4.1-Q4.2 | Can this idea be built and launched within the user's available time? |
| **Budget constraints** | Interview Q5.1 | Can this idea be built within the user's stated budget? |
| **Income goals** | Interview Q6.1-Q6.2 | Does this idea have a realistic path to the user's stated income target? |
| **Risk tolerance** | Interview Q7.1 | Does the risk profile of this idea match the user's stated tolerance? |

### 4.2 Minimum Personalization Threshold

An idea must connect to **at least two** profile attributes to be presented. The connections must be **specific and substantive**, not generic.

**Substantive connection examples:**
- "This idea leverages your experience building data pipelines at Stripe" (domain experience)
- "Your network of restaurant owners gives you direct access to beta testers" (network)
- "Your interest in cycling combined with your mobile dev skills positions you to build this" (interest + skills)

**Non-substantive (rejected) connections:**
- "This idea can be built with code" (every developer has this -- not personalized)
- "You expressed interest in making money" (universal -- not differentiated)
- "This is within your time budget of 20 hours/week" (constraint compliance is necessary but not a personalization signal)

### 4.3 Personalization Strength Tiers

| Tier | Profile Connections | Presentation |
|------|-------------------|-------------|
| **Strong** | 4+ substantive attribute connections | Present with high confidence in Personal Fit section |
| **Moderate** | 2-3 substantive attribute connections | Present with caveats about areas of weaker fit |
| **Weak** | 0-1 substantive attribute connections | **Reject** -- idea is not personalized enough. Attempt refinement or replace. |

### 4.4 Personalization-Driven Idea Generation

Rather than generating ideas and then checking for personalization fit, the preferred approach is to generate ideas **from** the user's profile:

1. **Start with the user's strongest attributes**: What is the intersection of their best skills, deepest domain experience, and strongest network connections?
2. **Identify problems in those intersection spaces**: What pain points exist in markets where the user has an unfair advantage?
3. **Generate ideas that are impossible without the user's profile**: The ultimate test of personalization is: "Would this exact idea have been generated for a user with a completely different profile?" If yes, it is not personalized enough.

### 4.5 Handling Sparse Profiles

If the user's profile is sparse (few skills, no domain experience, no network):

1. **Do not skip personalization** -- work with what is available
2. **Lean heavily on interests and constraints**: Even sparse profiles have stated interests and practical constraints
3. **Flag ideas as "broadly personalized"**: Acknowledge that the personalization is based on limited profile data
4. **Recommend profile enrichment**: Suggest the user re-run the interview with more detail for better results
5. **Prioritize buildability and market validation** over personal fit when fit data is limited

---

## 5. Pipeline Integration

### 5.1 Execution Order

For each candidate idea generated during the `generate-idea` skill workflow:

```
1. [Generate candidate idea]
        |
2. [Deep Personalization Filter]
   - Cross-reference against user profile (Section 4)
   - Minimum 2 substantive attribute connections
   - REJECT if weak personalization (0-1 connections)
        |
3. [Contrarian Filter]
   - Check against slop list (Section 1)
   - Evaluate generic vs. contrarian signals (Section 2)
   - REJECT if 2+ generic signals without compensating contrarian signals
   - Attempt refinement before final rejection
        |
4. [Freshness Filter]
   - Execute search queries via market-researcher agent (Section 3)
   - Count established competitors
   - Assign saturation flag
   - REJECT if 15+ competitors (unless Novel Angle Exception applies)
   - FLAG if 10-14 competitors
   - FLAG as "Unknown" if search fails or is ambiguous
        |
5. [Present idea to user]
   - Include saturation flag in idea document
   - Include personalization rationale in Personal Fit section
   - Include competitive landscape from freshness filter results
```

### 5.2 Rejection Tracking

Track rejections across the pipeline for transparency and debugging:

- Count ideas rejected at each filter stage
- If more than 50% of generated ideas are rejected by any single filter, log a warning -- this may indicate the idea generation prompt needs adjustment or the user's profile is unusually constrained
- Never present rejection statistics to the user unprompted, but include them in session metadata for quality monitoring

### 5.3 Performance Considerations

- **Batch freshness checks**: If multiple ideas target similar markets, reuse search results across ideas rather than running duplicate queries
- **Parallelize where possible**: Freshness filter searches for different ideas can run concurrently via the market-researcher agent
- **Cache within session**: If the same market was already researched (e.g., for market discovery), reuse that data rather than re-searching
- **Limit search depth**: Cap at 5-7 search queries per idea for the freshness filter to avoid excessive latency. If saturation is clear from the first 2-3 queries, skip remaining queries.

---

## 6. Anti-Slop Effectiveness Metric

The anti-slop effectiveness metric measures the percentage of generated ideas that overlap with the curated slop list (Section 1). This is the primary quality gate for the `generate-idea` skill.

### Measurement Method

1. For each idea presented to the user, compare it against the 35-item slop list
2. An idea "overlaps" if it describes substantially the same product for the same audience, ignoring cosmetic rebranding
3. Ideas that pass the Novel Angle Exception are NOT counted as overlaps
4. Calculate: `overlap_rate = overlapping_ideas / total_ideas_presented`
5. **Target**: `overlap_rate < 0.20` (fewer than 20% of ideas overlap with the slop list)

### When the Metric Fails

If `overlap_rate >= 0.20` for a session:
- Review the contrarian filter's rejection decisions -- it may be too lenient
- Check whether the user's profile is so broad that personalization cannot narrow the field
- Consider expanding the slop list if new generic patterns have emerged
- Log the metric result for continuous improvement
