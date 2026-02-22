# Market Categories Reference

This reference provides the market taxonomy, discovery patterns, presentation format, and selection criteria used by the `generate-idea` skill to identify and present markets suitable for the user.

---

## 1. Market Taxonomy

The following categories represent the primary market types a solo developer or small team can realistically enter. Each category includes subcategories with examples. This taxonomy is not exhaustive -- see [Niche and Emerging Markets](#niche-and-emerging-markets) for guidance on markets that fall outside these categories.

### 1.1 B2B SaaS

Software-as-a-service products sold to businesses, typically on a subscription model.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Developer Tools | CI/CD platforms, code review tools, monitoring dashboards, API testing | Per-seat or usage-based subscription |
| HR Tech | Applicant tracking, employee onboarding, performance reviews, time tracking | Per-employee/month |
| Fintech | Invoicing, expense management, payroll, financial reporting | Tiered subscription |
| Sales & Marketing | CRM, email outreach, lead scoring, analytics dashboards | Per-seat subscription |
| Operations | Project management, inventory tracking, scheduling, workflow automation | Tiered subscription |
| Legal & Compliance | Contract management, compliance tracking, e-signatures | Per-document or subscription |
| Vertical SaaS | Industry-specific tools (real estate, healthcare, construction, logistics) | Subscription + onboarding fees |

### 1.2 B2C Apps

Consumer-facing applications that solve personal problems or fulfill lifestyle needs.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Productivity | Note-taking, habit tracking, personal finance, calendar tools | Freemium, premium subscription |
| Health & Wellness | Fitness tracking, meditation, nutrition planning, sleep analysis | Subscription, in-app purchases |
| Education | Language learning, skill courses, flashcard apps, tutoring platforms | Subscription, one-time purchase |
| Entertainment | Games, creative tools, social apps, content discovery | Freemium, ads, in-app purchases |
| Personal Finance | Budgeting, investing, debt payoff, savings automation | Freemium, premium tier |
| Lifestyle | Travel planning, recipe management, home organization, pet care | Subscription, marketplace fees |

### 1.3 Marketplace / Platform

Two-sided or multi-sided platforms connecting buyers and sellers, or supply and demand.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Service Marketplaces | Freelancer platforms, tutoring matching, home services | Transaction fees (10-20%) |
| Product Marketplaces | Niche e-commerce, handmade goods, digital assets | Transaction fees, listing fees |
| Aggregators | Price comparison, review aggregation, deal discovery | Affiliate commissions, ads |
| Community Platforms | Niche forums, professional networks, hobbyist communities | Membership fees, premium tiers |
| Rental/Sharing | Equipment rental, space sharing, tool libraries | Transaction fees, insurance fees |

### 1.4 Content / Media

Products built around content creation, distribution, or community building.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Newsletters | Industry insights, curated links, niche expertise | Paid subscription, sponsorships |
| Online Courses | Video courses, cohort-based learning, workshops | One-time purchase, subscription |
| Communities | Paid Slack/Discord groups, membership sites, masterminds | Monthly membership |
| Content Tools | Writing assistants, editing tools, publishing platforms | Subscription, per-use pricing |
| Podcasts / Video | Niche podcasts, YouTube channels, live streaming | Sponsorships, membership, ads |

### 1.5 E-commerce

Selling physical or digital products directly to consumers or businesses.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Niche Physical Products | Specialty items, custom merchandise, curated boxes | Direct sales, subscription boxes |
| Digital Goods | Templates, themes, presets, fonts, icons, 3D models | One-time purchase, bundles |
| Print-on-Demand | Custom apparel, accessories, home decor | Per-sale margin |
| Subscription Boxes | Curated product selections delivered regularly | Monthly subscription |
| B2B Supplies | Niche supplies for specific industries or professions | Direct sales, bulk pricing |

### 1.6 Services

Service-based businesses that leverage technical or domain expertise.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Consulting | Technical consulting, strategy advising, fractional CTO | Hourly or project-based |
| Freelance / Agency | Web development, design, marketing, data analysis | Project-based, retainer |
| Managed Services | Server management, security monitoring, data pipelines | Monthly retainer |
| Productized Services | Fixed-scope offerings (e.g., "website audit for $500") | Fixed-price packages |
| Training & Coaching | 1-on-1 coaching, group workshops, bootcamps | Per-session, cohort-based |

### 1.7 Infrastructure / API

Backend services, APIs, and developer infrastructure products.

| Subcategory | Examples | Typical Revenue Model |
|-------------|----------|----------------------|
| Developer APIs | Payment processing, email delivery, SMS, geocoding | Usage-based (per-call) |
| Data Services | Data enrichment, scraping-as-a-service, analytics APIs | Usage-based, subscription |
| Infrastructure Tools | Hosting, deployment, CI/CD, monitoring, logging | Tiered subscription |
| AI/ML Services | Model hosting, fine-tuning platforms, inference APIs | Usage-based, subscription |
| Integration Platforms | Zapier-like connectors, webhook services, ETL tools | Tiered subscription |

### Niche and Emerging Markets

Not every market fits neatly into the categories above. When encountering markets that fall outside the taxonomy:

1. **Classify by closest analogy**: Determine which existing category the market most resembles in terms of business model and customer type
2. **Flag as "emerging"**: If the market is less than 3 years old or lacks established players, label it as an emerging market and note the higher uncertainty
3. **Evaluate with extra scrutiny**: Apply the same selection criteria but weight "market validation" evidence more carefully -- emerging markets may have strong signals (growing search trends, active communities) without established revenue data
4. **Examples of emerging/niche markets**:
   - Creator economy tools (overlaps Content/Media and B2B SaaS)
   - Climate tech / sustainability tools (vertical across multiple categories)
   - Web3 / blockchain applications (infrastructure + consumer hybrid)
   - AI-native products (tools built primarily around LLM capabilities)
   - Longevity / biohacking (overlaps Health and E-commerce)
   - Remote work infrastructure (overlaps B2B SaaS and Services)

---

## 2. Market Discovery Patterns

These patterns map attributes from the user's profile (gathered during the interview phase) to suitable markets. Apply all applicable patterns and intersect the results to find the strongest market fits.

### 2.1 Skill-to-Market Mapping

Map the user's technical skills to markets where those skills provide a building advantage.

| Skill Profile | Candidate Markets | Reasoning |
|--------------|-------------------|-----------|
| Python + data analysis | Data services, analytics dashboards, fintech tools | Can build data pipelines and visualization tools |
| JavaScript/React + design | B2C apps, content tools, SaaS frontends | Can build polished user-facing products |
| Backend/DevOps + infrastructure | Developer APIs, infrastructure tools, monitoring | Can build reliable backend services |
| Mobile development (iOS/Android) | B2C apps, health/fitness, productivity | Can ship native mobile experiences |
| ML/AI expertise | AI/ML services, data enrichment, intelligent tooling | Can build differentiated AI-powered products |
| Full-stack generalist | Vertical SaaS, marketplaces, productized services | Can build end-to-end solutions for specific niches |
| Low-code/no-code familiarity | Content/media, communities, productized services | Can rapidly prototype and validate |

### 2.2 Experience-to-Market Mapping

Map the user's professional or domain experience to markets where insider knowledge is an advantage.

| Experience Profile | Candidate Markets | Reasoning |
|-------------------|-------------------|-----------|
| Worked in healthcare | Healthcare SaaS, health tech, medical compliance tools | Understands pain points, regulations, workflows |
| Finance/banking background | Fintech, personal finance, financial reporting | Knows compliance requirements, user needs |
| Education sector experience | EdTech, course platforms, tutoring marketplaces | Understands educators' and learners' needs |
| E-commerce/retail experience | Niche e-commerce, retail SaaS, inventory tools | Knows supply chain and retail operations |
| Agency/freelance background | Productized services, project management, client tools | Understands service delivery and client management |
| Real estate industry | Real estate SaaS, property management, listing tools | Knows transaction flows and stakeholder needs |
| Restaurant/hospitality | Restaurant tech, booking systems, supply management | Understands operational challenges firsthand |

### 2.3 Interest-to-Market Mapping

Map the user's personal interests and passions to markets where sustained motivation matters.

| Interest Profile | Candidate Markets | Reasoning |
|-----------------|-------------------|-----------|
| Fitness / sports | Fitness apps, coaching platforms, sports analytics | Personal passion sustains long-term commitment |
| Gaming | Game dev tools, gaming communities, esports platforms | Deep understanding of gamer needs |
| Music / audio | Music production tools, audio APIs, podcast platforms | Domain knowledge enables better products |
| Personal finance / investing | Budgeting apps, investment tools, financial education | Genuine interest drives quality content |
| Writing / content creation | Writing tools, newsletter platforms, CMS products | Understands creator pain points firsthand |
| Travel | Travel planning tools, accommodation platforms, travel content | Can validate ideas through own usage |
| Sustainability / environment | Climate tech, carbon tracking, sustainable commerce | Growing market aligned with values |

### 2.4 Network-to-Market Mapping

Map the user's existing professional and personal network to markets where distribution is easier.

| Network Profile | Candidate Markets | Reasoning |
|----------------|-------------------|-----------|
| Knows restaurant owners | Restaurant tech, food industry SaaS | Direct access to first customers and feedback |
| Connected to developers | Developer tools, infrastructure, coding education | Can validate and distribute through peer network |
| Active in parenting communities | Parenting apps, family planning tools, kids' education | Built-in beta testers and word-of-mouth |
| Professional network in HR | HR tech, recruiting tools, onboarding platforms | Direct sales channel through existing relationships |
| Embedded in creator community | Creator tools, monetization platforms, content services | Understands needs, has distribution advantage |
| Strong local business connections | Local business SaaS, booking tools, service marketplaces | Can sign first customers through personal outreach |

### 2.5 Constraint-Based Filtering

After identifying candidate markets through the patterns above, filter based on the user's constraints.

| Constraint | Markets to Prefer | Markets to Avoid |
|-----------|-------------------|------------------|
| < 10 hrs/week available | Content/media, digital goods, APIs | Marketplaces (cold start problem), services |
| No budget for marketing | Communities (organic growth), developer tools (word-of-mouth) | B2C apps (high CAC), marketplaces |
| Wants revenue within 3 months | Productized services, consulting, digital goods | Marketplaces, infrastructure (long ramp-up) |
| Risk averse | B2B SaaS (predictable revenue), services | Consumer apps (hit-driven), marketplaces |
| Income goal > $10K/month | B2B SaaS, vertical SaaS, infrastructure | Content/newsletters (slow scaling), hobby apps |

### 2.6 When User Profile Does Not Clearly Map

If the user's profile does not produce strong matches through the patterns above:

1. **Broaden the search**: Consider "skill-adjacent" markets -- markets that are one step removed from the user's direct skills (e.g., a Python developer might not map directly to fitness tech, but could build data-driven fitness analytics)
2. **Lead with constraints**: When skills and experience do not point anywhere specific, use the user's constraints (time, budget, income goals) as the primary filter and present a diverse selection across categories
3. **Suggest exploration**: Present 3-5 markets from different categories with the framing that this is an exploration phase -- the user can reject all and provide more specific direction
4. **Ask targeted follow-ups**: If the user's answers were vague, ask specific questions like:
   - "What problem have you personally encountered this week that software could solve?"
   - "Which of your friends or colleagues complain most about their tools?"
   - "What industry do you find yourself reading about most often?"

---

## 3. Market Presentation Format

Each identified market should be presented to the user in the following consistent format. This ensures the user can compare markets on equal footing.

### Per-Market Presentation Template

```markdown
### Market: {Market Name}

**Category**: {Taxonomy category from Section 1} > {Subcategory}

**Description**: {2-3 sentence description of the market, what problems are solved, and who the customers are}

**Market Size Estimates**:
- **TAM** (Total Addressable Market): {Broad global estimate with source/reasoning}
- **SAM** (Serviceable Addressable Market): {Segment the user could realistically target}
- **SOM** (Serviceable Obtainable Market): {What a solo developer could capture in 1-2 years}

**Trend Direction**: {Growing / Stable / Declining} -- {1-2 sentence explanation with evidence}

**Why This Fits You**:
- Skill match: {How the user's skills apply}
- Experience match: {How the user's background is relevant}
- Network advantage: {Any existing connections in this market}
- Interest alignment: {How this connects to personal interests}

**Example Opportunities**:
1. {Specific idea or gap in this market}
2. {Another specific idea or gap}
3. {A third specific idea or gap}

**Confidence Level**: {High / Medium / Low} -- {Based on quality of market data available}
```

### Market Size Guidance (TAM/SAM/SOM)

Since this is a research-aided skill (not a market data platform), market size estimates should follow these guidelines:

- **TAM**: Use publicly available industry reports, analyst estimates, or well-known benchmarks. Cite the source or reasoning. If no data is available, provide a reasoned estimate based on adjacent markets.
- **SAM**: Narrow TAM to the user's geographic region, target customer segment, and product scope. Typically 10-30% of TAM for niche segments.
- **SOM**: Estimate what a solo developer with no marketing budget could realistically capture in 12-24 months. For most markets, this is $1K-$50K ARR range. Be honest -- do not inflate.
- **When data is unavailable**: State "Estimate based on limited data" and provide the reasoning. Never fabricate specific dollar figures without basis.

### Trend Direction Guidelines

- **Growing**: Market is expanding > 10% annually, new entrants are succeeding, increasing search trends, VCs are funding the space
- **Stable**: Market is mature with steady demand, established players, moderate new entry, < 10% annual growth
- **Declining**: Market is shrinking, key players are exiting or pivoting, decreasing search trends, being disrupted by alternatives

Always cite the evidence for the trend assessment (search trends, industry reports, funding activity, competitor movements).

### Confidence Level Guidelines

- **High**: Multiple data points from web research confirm the market assessment; clear competitors, pricing data, and trend signals exist
- **Medium**: Some data is available but gaps exist; market exists but evidence is partially inferential
- **Low**: Limited or no web research data available; assessment is based primarily on reasoning rather than evidence. Flag as "low-confidence" per spec requirements (US-002)

---

## 4. Market Selection Criteria

After generating a candidate list of markets (typically 8-15 from the discovery patterns), narrow to the 3-5 most promising markets to present to the user.

### 4.1 Personal Fit Scoring

Score each candidate market on how well it fits the user's profile. Each dimension is scored 1-5.

| Dimension | 1 (Poor Fit) | 3 (Moderate Fit) | 5 (Strong Fit) |
|-----------|-------------|-------------------|-----------------|
| **Skill Relevance** | User would need to learn entirely new tech stack | Some skills transfer, some learning needed | User's existing skills directly apply |
| **Domain Knowledge** | No familiarity with the market or its users | Some exposure or general awareness | Deep experience or active participant |
| **Network Access** | Knows nobody in the market | Has indirect connections (friends of friends) | Has direct relationships with potential customers |
| **Interest Alignment** | No personal interest in the domain | Mildly curious or professionally interested | Passionate about the domain |
| **Constraint Compatibility** | Market requires more time/money/risk than user can offer | Market is workable but stretches constraints | Market fits within stated constraints comfortably |

**Personal Fit Score** = Average of all five dimensions (1.0 - 5.0)

### 4.2 Accessibility for Solo Developers

Evaluate how feasible it is for a solo developer to enter and compete in each market.

| Factor | Favorable | Unfavorable |
|--------|-----------|-------------|
| **Technical complexity** | Can build MVP in 2-4 weeks | Requires 6+ months of development |
| **Regulatory burden** | No special licenses or compliance | Heavy regulation (HIPAA, PCI, etc.) |
| **Cold start problem** | Product works for first user | Needs critical mass to be useful (marketplaces) |
| **Distribution** | Can reach users via SEO, communities, word-of-mouth | Requires enterprise sales team or ad budget |
| **Defensibility** | Domain expertise or data creates moat | Easily cloned by larger players |
| **Capital requirements** | Can launch with < $100/month in costs | Requires significant upfront investment |

Markets with 4+ favorable factors are highly accessible. Markets with 3+ unfavorable factors should be flagged with a warning.

### 4.3 Revenue Potential Indicators

Assess the revenue potential for a solo developer (not total market opportunity).

| Indicator | Positive Signal | Negative Signal |
|-----------|----------------|-----------------|
| **Willingness to pay** | Existing competitors charge $20+/month | Market expects everything to be free |
| **Customer LTV** | Subscription model, low churn | One-time purchase, high churn |
| **Pricing power** | B2B customers, ROI-justifiable | Price-sensitive consumer market |
| **Path to $1K MRR** | Clear customer acquisition path exists | Would need 10,000+ free users to convert |
| **Expansion revenue** | Upsell potential (more seats, higher tiers) | Flat pricing with no growth per customer |

### 4.4 Selection Algorithm

1. **Score each candidate market** on personal fit (Section 4.1)
2. **Assess accessibility** (Section 4.2) -- flag markets with 3+ unfavorable factors
3. **Evaluate revenue potential** (Section 4.3) -- flag markets with 3+ negative signals
4. **Rank by composite assessment**:
   - Primary sort: Personal Fit Score (descending)
   - Tiebreaker: Accessibility (more favorable factors wins)
   - Secondary tiebreaker: Revenue potential (more positive signals wins)
5. **Select top 3-5 markets**:
   - Always include the top-ranked market
   - Include at least one market from a different category than the top-ranked (diversity)
   - If a lower-ranked market has unique network advantage (user knows people in it), include it even if overall score is lower
   - If fewer than 3 markets score above 2.5 personal fit, broaden the search using the guidance in Section 2.6
6. **Present with transparency**: Show the user why each market was selected and what trade-offs exist
