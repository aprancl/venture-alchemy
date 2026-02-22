# Planning Patterns

This reference provides the patterns, templates, and constraints used by the `plan-execution` skill to produce realistic, actionable execution plans for solo developers. The emphasis is on minimal viable revenue -- proving that customers will pay -- rather than building feature-complete products.

---

## 1. MVP Scoping Patterns

MVP scope should be the smallest product that tests a revenue hypothesis. The goal is not "minimum viable product" in the sense of a stripped-down app -- it is "minimum viable revenue": the smallest thing you can build that someone will pay for.

### 1.1 The Revenue Hypothesis Test

Before defining scope, articulate the revenue hypothesis in one sentence:

```
"{Target customer} will pay ${price} per {period} for {core value proposition}."
```

Every feature in the MVP must directly support proving or disproving this sentence. If a feature does not bring you closer to a paying customer, cut it.

### 1.2 Scoping Rules

| Rule | Description | Example |
|------|-------------|---------|
| **One core workflow** | MVP solves exactly one problem through one primary workflow | A reporting tool that generates one type of report, not a dashboard with five chart types |
| **Manual before automated** | Replace complex automation with manual processes behind the scenes ("Wizard of Oz") | Manually curate recommendations before building an ML pipeline |
| **Integrate, don't build** | Use existing services (Stripe, Auth0, SendGrid) instead of building infrastructure | Use Stripe Checkout instead of building a custom payment flow |
| **Ugly is fine** | Functional UI over polished UI; no custom design system for MVP | Use a CSS framework (Tailwind, shadcn) with default styling |
| **No admin panel** | Manage data via database directly or simple scripts until you have paying customers | Edit user records with SQL or a quick script rather than building an admin dashboard |
| **No scale engineering** | Design for 10-100 users, not 10,000; optimize later when revenue justifies it | SQLite or a single Postgres instance, not a distributed database |
| **Auth can wait** | Use magic links or a simple email/password flow; skip OAuth integrations for launch | Add "Sign in with Google" only after validating demand |
| **Ship weekly** | If the MVP takes more than 4 weeks of solo dev time, the scope is too large | Reassess and cut features until the timeline fits |

### 1.3 Feature Triage Matrix

Use this matrix to classify every proposed feature during scoping:

| Category | Criteria | Action |
|----------|----------|--------|
| **Must Have** | Directly required for the revenue hypothesis test; product is non-functional without it | Include in MVP |
| **Should Have** | Improves the experience but users can work around its absence | Defer to post-MVP milestone |
| **Nice to Have** | Makes the product more polished or competitive; no user has asked for it yet | Defer to growth phase or cut entirely |
| **Trap** | Technically interesting but does not contribute to revenue; often the features developers want to build most | Cut ruthlessly; revisit only if users request it |

### 1.4 Scope Reduction Techniques

When the MVP is still too large after applying the triage matrix:

1. **Narrow the audience**: Instead of "small businesses," target "freelance designers using Figma." Smaller audience = fewer edge cases = smaller product.
2. **Reduce the output**: Instead of a full dashboard, deliver a weekly email report. Instead of real-time sync, do daily batch.
3. **Limit the input**: Accept only one file format, one integration, or one data source. Expand after validation.
4. **Time-box the interaction**: Instead of an always-on service, build a tool that runs on demand (CLI tool, scheduled job, one-click action).
5. **Concierge MVP**: Deliver the value manually to the first 5-10 customers while building automation behind the scenes.

---

## 2. Milestone Templates

Execution plans follow a standard milestone progression. Each milestone has explicit completion criteria so the developer knows exactly when to move on.

### 2.1 Standard Milestone Structure

#### Milestone 0: Validation (1-2 weeks)

**Objective**: Confirm demand before writing code.

| Activity | Completion Criteria |
|----------|-------------------|
| Create a landing page describing the product and its core value proposition | Landing page is live with an email signup form |
| Drive traffic to the landing page via target communities (see Customer Acquisition Patterns) | At least 50 unique visitors within the first week |
| Collect email signups or waitlist registrations | At least 10 signups from the target audience (not friends/family) |
| Conduct 3-5 brief conversations with potential customers (DMs, calls, forum threads) | At least 3 conversations where the target customer confirms the problem is real and they would consider paying |
| Document findings | Validation summary written: problem confirmed (yes/no), willingness to pay confirmed (yes/no), feedback themes captured |

**Go/No-Go Decision**: Proceed to Milestone 1 if you have 10+ signups AND at least 2 conversations confirming willingness to pay. If not, pivot the value proposition or target audience and repeat validation.

---

#### Milestone 1: MVP Build (2-4 weeks)

**Objective**: Build the minimum product that proves the revenue hypothesis.

| Activity | Completion Criteria |
|----------|-------------------|
| Implement the single core workflow identified in scoping | Core workflow is functional end-to-end for a single user |
| Integrate payment (Stripe Checkout or equivalent) | A user can complete a purchase or start a paid subscription |
| Deploy to production | Product is accessible via a public URL with basic reliability (not localhost) |
| Set up basic error monitoring and analytics | Errors are captured (Sentry, LogRocket, or equivalent); basic usage events tracked |
| Write a "getting started" guide or onboarding flow | A new user can understand and use the product without the developer's help |

**Go/No-Go Decision**: Proceed to Milestone 2 when the core workflow works end-to-end and at least one person outside your immediate circle has successfully used it.

---

#### Milestone 2: First Revenue (2-4 weeks)

**Objective**: Get the first paying customer.

| Activity | Completion Criteria |
|----------|-------------------|
| Announce to waitlist and target communities | Launch announcement sent to all collected emails and posted in 3+ relevant communities |
| Offer early-adopter pricing or lifetime deals to incentivize first purchases | Pricing offer is live and communicated to prospects |
| Provide hands-on support to every early user (concierge onboarding) | Every user who signs up receives a personal welcome and offer of help |
| Collect and act on feedback from first users | Feedback log maintained; at least one improvement shipped based on user feedback |
| Track conversion funnel: visitor -> signup -> active user -> paying customer | Funnel metrics documented, even if numbers are small |

**Go/No-Go Decision**: Proceed to Milestone 3 if you have at least 1 paying customer (not a friend or family member). If no one pays after 4 weeks of active outreach, revisit the value proposition, pricing, or target audience.

---

#### Milestone 3: Growth Foundation (4-8 weeks)

**Objective**: Establish repeatable acquisition and retention.

| Activity | Completion Criteria |
|----------|-------------------|
| Reach $100+ MRR or 10+ paying customers (whichever comes first) | Revenue milestone achieved |
| Identify the top 1-2 acquisition channels that produce paying customers | Channels documented with rough CAC estimates |
| Implement the top 3 feature requests from paying customers | Features shipped and confirmed by the requesting users |
| Reduce manual processes: automate onboarding, billing, or support steps that are consuming >2 hours/week | Time spent on manual ops reduced measurably |
| Set up basic retention tracking (churn rate, usage frequency) | Retention metrics established with a baseline measurement |

**Go/No-Go Decision**: Continue scaling if MRR is growing month-over-month and churn is manageable. If MRR is flat or declining despite active effort, investigate whether the problem is acquisition, activation, or retention before adding features.

---

### 2.2 Accelerated Timeline (Speed-to-Market Variant)

For fast-moving markets where a competitor could capture the niche:

| Milestone | Duration | Key Difference |
|-----------|----------|----------------|
| Validation | 3-5 days | Landing page only; skip conversations; use signup count as sole signal |
| MVP Build | 1-2 weeks | Even more aggressive scope cuts; concierge where possible |
| First Revenue | 1-2 weeks | Launch immediately to waitlist; iterate publicly |
| Growth Foundation | 4-6 weeks | Same as standard |

Use this variant when: (a) the market window is closing, (b) a competitor just launched something similar, or (c) the idea is simple enough that validation and build can overlap.

### 2.3 Milestone Customization Rules

- **Adjust durations** based on the user's stated available hours per week (from the generate-idea profile). The templates assume 15-25 hours/week. For users with <10 hours/week, double the durations.
- **Merge milestones** if the product is simple enough (e.g., a single API or CLI tool may combine Validation and MVP Build).
- **Add milestones** if the product requires regulatory compliance, data collection phases, or partnerships (rare for solo dev projects).

---

## 3. Solo-Developer Constraints

These constraints reflect the reality of building a product alone. Every recommendation in the execution plan must be checked against these constraints.

### 3.1 Time Budget

| User's Available Time | Realistic Weekly Output | MVP Timeline |
|-----------------------|------------------------|--------------|
| <5 hours/week | One small feature or fix | 3-6 months (consider if this is viable) |
| 5-10 hours/week | One medium feature | 2-4 months |
| 10-20 hours/week | One feature + marketing/support | 1-2 months |
| 20-40 hours/week (full-time) | Multiple features + marketing | 2-4 weeks |

### 3.2 Role Overload

A solo developer fills every role. Plans must account for time spent on non-coding work:

| Role | Estimated Time (% of total) | Cannot Skip |
|------|------------------------------|-------------|
| Development | 40-50% | Core product work |
| Customer support | 10-15% | After launch, this grows; budget for it |
| Marketing/outreach | 15-20% | Without this, no one finds the product |
| Operations/infrastructure | 5-10% | Deployments, monitoring, billing issues |
| Planning/research | 10-15% | Deciding what to build next |

**Key implication**: Only 40-50% of available time goes to writing code. A developer with 20 hours/week has roughly 8-10 hours of coding time.

### 3.3 Skill Gap Handling

When the execution plan requires skills the user does not have:

| Skill Gap Type | Recommended Approach |
|----------------|---------------------|
| **Design (UI/UX)** | Use component libraries (shadcn, Tailwind UI, Chakra); buy a template ($50-200); skip custom design entirely for MVP |
| **Copywriting** | Use AI-assisted copywriting for landing pages and emails; study 3 competitor landing pages for structure; iterate based on conversion data |
| **Marketing/SEO** | Focus on community-based acquisition first (see Section 5); learn SEO basics only after product-market fit; avoid paid ads pre-revenue |
| **DevOps/Infrastructure** | Use PaaS (Vercel, Railway, Fly.io, Render); avoid Kubernetes, Docker orchestration, or custom CI/CD for MVP |
| **Mobile development** | Ship as a PWA or responsive web app; native apps only if the use case demands it (e.g., offline access, push notifications critical to value prop) |
| **Specialized domain knowledge** | Partner with a domain expert (revenue share, not equity); interview 5+ practitioners before building; use the concierge MVP approach to validate understanding |
| **Data science / ML** | Use pre-built APIs (OpenAI, HuggingFace Inference); avoid training custom models until you have both data and revenue |
| **Legal / Compliance** | Use standard terms-of-service templates; consult a lawyer only for regulated industries (health, finance); do not let legal fear prevent launching |

### 3.4 What Solo Developers Should NOT Build for MVP

Avoid these patterns unless they are the core value proposition:

- Real-time collaborative editing (complexity explosion)
- Multi-tenant admin systems with role-based access control
- Custom authentication systems (use Auth0, Clerk, Supabase Auth)
- Native mobile apps (unless mobile-only value proposition)
- Browser extensions for multiple browsers (pick one; Chrome has the largest market)
- Marketplace or two-sided platforms (chicken-and-egg problem requires significant marketing budget)
- Anything requiring a critical mass of users to provide value (network effects are hard for solo devs)

---

## 4. Validation Checkpoints

Validation is not a one-time event. The plan embeds checkpoints at each stage where the developer must assess real-world feedback before proceeding.

### 4.1 Checkpoint Definitions

#### Checkpoint 1: Pre-Build (After Milestone 0)

**When**: Before writing any product code.

**Questions to answer**:
- Did at least 10 people from the target audience sign up for the waitlist?
- Did at least 2-3 potential customers confirm the problem is real in conversation?
- Did anyone express willingness to pay (even hypothetically)?
- Is there a clear, specific target customer (not "small businesses" but "freelance UX designers who use Figma")?

**Actions based on results**:
| Result | Action |
|--------|--------|
| All questions answered "yes" | Proceed to MVP Build |
| Signups are low but conversations are strong | Improve landing page messaging; retest for one more week |
| Conversations reveal a different problem than expected | Pivot the value proposition; repeat Milestone 0 |
| No traction at all | Discard this idea or fundamentally rethink the target audience |

---

#### Checkpoint 2: Post-Landing-Page (During Milestone 1)

**When**: Landing page has been live for at least 1 week with active traffic.

**Questions to answer**:
- What is the signup conversion rate (visitors to email signups)?
- Which traffic sources produced the most signups?
- What questions did visitors ask (via email, chat, or community replies)?
- Are signups coming from the intended target audience or someone else?

**Benchmarks**:
| Metric | Healthy | Concerning | Action if Concerning |
|--------|---------|------------|---------------------|
| Signup conversion rate | >5% | <2% | Rewrite landing page copy; test a different value proposition angle |
| Signups from target audience | >70% | <50% | Reassess target audience definition or adjust messaging |
| Questions about "when will it launch?" | Frequent | None | Concerning lack of urgency; consider whether the problem is painful enough |

---

#### Checkpoint 3: Post-MVP (After Milestone 1, During Milestone 2)

**When**: MVP is live and accessible to early users.

**Questions to answer**:
- Are users completing the core workflow without hand-holding?
- How many users return after their first session?
- What is the most common drop-off point in the workflow?
- Are users providing unsolicited feedback or feature requests?
- Has anyone voluntarily shared the product with others?

**Actions based on results**:
| Result | Action |
|--------|--------|
| Users complete the workflow and return | Proceed with launch and revenue push |
| Users sign up but do not complete the workflow | Fix onboarding or simplify the core workflow before pursuing revenue |
| Users complete the workflow once but do not return | The product may solve a one-time problem; consider if a one-time purchase model fits better than subscription |
| No one uses it despite signups | Reach out personally to signups; the product may have an activation problem, not a demand problem |

---

#### Checkpoint 4: Revenue Milestone (During Milestone 2-3)

**When**: Product has been available for purchase for at least 2 weeks.

**Questions to answer**:
- How many people have paid?
- What is the conversion rate from free user/trial to paid?
- What reasons do non-paying users give for not purchasing?
- Are paying customers continuing to use the product (retention)?
- What is the current MRR trajectory?

**Revenue health assessment**:
| MRR Trajectory | Signal | Action |
|----------------|--------|--------|
| Growing week-over-week | Product-market fit is emerging | Double down on acquisition; implement top feature requests |
| Flat | Acquisition or activation problem | Investigate where the funnel breaks; run experiments |
| One-time spike then flat | Launch effect only; not sustainable | Build retention features; investigate churn reasons |
| Zero after 4+ weeks of effort | Revenue hypothesis is likely wrong | Pivot pricing, audience, or value proposition; consider killing the project |

---

### 4.2 Checkpoint Execution Rules

- **Never skip a checkpoint**. Skipping validation to "ship faster" is the most common solo-dev mistake.
- **Write down results**. Document checkpoint outcomes in the execution plan log, not just in your head.
- **Set hard kill criteria**. Before starting, define what would make you abandon the project (e.g., "If I have zero paying customers after 8 weeks post-launch, I stop").
- **Time-box pivots**. If a checkpoint suggests a pivot, allow one pivot attempt (2 weeks). If the pivot also fails at the next checkpoint, move on.

---

## 5. Customer Acquisition Strategy Patterns

Acquisition strategy changes as the user base grows. These patterns provide stage-appropriate tactics for a solo developer with limited or no marketing budget.

### 5.1 First 10 Customers (Manual, High-Touch)

**Strategy**: Direct, personal outreach. Every customer is acquired one at a time.

| Tactic | How | Expected Conversion |
|--------|-----|---------------------|
| **Personal network** | Message friends, colleagues, former coworkers who match the target audience; ask them to try it and pay (even a small amount) | 10-20% of people you ask |
| **Community participation** | Actively participate in 2-3 communities where the target audience hangs out (Reddit, Discord, Slack, forums); help people with problems your product solves; mention the product when relevant | 1-3 customers per community per month |
| **Direct DMs** | Identify people who publicly express the problem your product solves (tweets, Reddit posts, forum threads); send a short, personalized message offering your solution | 5-10% response rate; 10-20% of responders convert |
| **Concierge onboarding** | Offer to personally walk each early user through the product; treat it as a customer interview | Increases activation rate significantly; generates word-of-mouth |
| **Cold email to micro-businesses** | For B2B products: identify 50-100 small businesses in your niche; send a brief, value-focused email | 2-5% reply rate; 10-20% of replies convert |

**What NOT to do at this stage**: Paid ads, SEO, content marketing, Product Hunt launch, viral features. These do not work at the scale of 0-10 customers and waste time.

---

### 5.2 First 100 Customers (Scalable Manual + Early Content)

**Strategy**: Find the one channel that works and double down on it.

| Tactic | How | Expected Outcome |
|--------|-----|------------------|
| **Double down on the best channel from 0-10** | Whichever tactic produced the most paying customers, do more of it systematically | 2-5x the volume of your best channel |
| **Launch on aggregators** | Product Hunt, Hacker News Show HN, Indie Hackers, relevant subreddit "launch" posts | Spike of 100-500 visitors; 2-5% convert to signups; subset convert to paid |
| **Write 3-5 SEO-targeted articles** | Identify long-tail keywords your target audience searches for; write genuinely helpful content that mentions your product | Takes 2-3 months to rank; provides steady inbound traffic once it does |
| **Build in public** | Share your building journey, metrics, and lessons on Twitter/X or a blog; attracts a small audience of engaged followers who become customers | 50-200 followers in 2-3 months; 5-10% become customers |
| **Referral incentive** | Offer existing customers a benefit (free month, extended trial, feature unlock) for referring someone who pays | 10-20% of happy customers will refer at least one person |
| **Micro-influencer partnerships** | Identify niche content creators (1K-10K followers) in your target market; offer free access in exchange for an honest review | 1-3 partnerships can drive 20-50 signups each |

**Key metric to track**: Customer Acquisition Cost (CAC) per channel. At this stage, your time IS the cost (value it at your hourly rate). Focus on channels where CAC is lower than 3-month LTV.

---

### 5.3 First 1,000 Customers (Systematic + Leverage)

**Strategy**: Systematize what works; begin using leverage (content, integrations, partnerships).

| Tactic | How | Expected Outcome |
|--------|-----|------------------|
| **SEO as primary inbound channel** | Expand content library to 15-30 articles targeting increasingly competitive keywords; build backlinks through guest posts and tool mentions | 500-2000 organic visitors/month; 2-5% convert |
| **Integration partnerships** | Build integrations with tools your customers already use; get listed in their marketplaces/directories | 10-50 customers per active integration listing |
| **Email nurture sequence** | Build an automated email sequence for new signups: welcome -> value demonstration -> conversion offer | Improves trial-to-paid conversion by 20-40% |
| **Paid acquisition (small budget)** | Test Google Ads or Facebook/Instagram ads with a small budget ($200-500/month); only after organic channels are proven | Use to amplify what already works; target exact keywords/audiences from organic learnings |
| **Community building** | Create a Slack, Discord, or forum for your users; facilitate peer-to-peer value exchange | Improves retention; creates organic word-of-mouth; reduces support burden |
| **Case studies and testimonials** | Document success stories from your best customers; use in landing pages and marketing materials | Increases conversion rate by 10-30% across all channels |

**Key milestone**: Achieving $1,000+ MRR with a clear, repeatable acquisition funnel. At this point, consider whether to stay solo or begin planning for help (contractor, part-time hire, co-founder).

---

## 6. Pricing Recommendation Patterns

Pricing is one of the highest-leverage decisions a solo developer can make. These patterns guide pricing strategy based on the product type and target audience.

### 6.1 Pricing Model Selection

| Product Type | Recommended Model | Why |
|-------------|-------------------|-----|
| **SaaS tool (ongoing value)** | Monthly subscription with annual discount | Recurring revenue; predictable; customers expect it for tools they use regularly |
| **One-time tool or utility** | One-time purchase with optional support tier | Simpler to sell; no churn concern; works well for tools used occasionally |
| **API or developer tool** | Usage-based with a free tier | Aligns cost with value; developers expect this model; low barrier to entry |
| **Content or education** | One-time purchase or cohort-based pricing | Avoid subscription fatigue for content; charge premium for live/cohort access |
| **Marketplace or platform** | Transaction fee (percentage or flat) | Aligns revenue with user success; avoids upfront pricing resistance |
| **Community or network** | Freemium with premium features | Free tier provides network effects; premium tier for power users |

### 6.2 Pricing Level Guidelines

Solo developers consistently underprice. Use these guidelines as a floor, not a ceiling:

| Target Audience | Price Floor (Monthly) | Rationale |
|-----------------|----------------------|-----------|
| **Individual consumers** | $5-15/month | Below $5, payment friction outweighs the revenue; above $15 requires strong justification for individuals |
| **Freelancers / creators** | $15-50/month | Freelancers value tools that save time or make money; $30/month is easy to justify if it saves 2+ hours/week |
| **Small businesses (1-10 employees)** | $30-100/month | Small businesses routinely pay $50-100/month for tools; price based on value delivered, not cost of goods |
| **Mid-market businesses (10-100 employees)** | $100-500/month | Per-seat pricing works well here; even $10/seat for 20 users = $200/month |
| **Enterprise** | Not recommended for solo devs | Requires sales process, security reviews, SLAs, and support capacity a solo dev cannot provide at MVP stage |

### 6.3 Pricing Strategy Patterns

#### Pattern: "Start High, Discount Down"

Set the initial price higher than you think is right, then offer early-adopter discounts. It is much easier to give a discount than to raise prices later.

- List price: Your target price + 20-30%
- Early-adopter discount: 30-50% off for the first 50-100 customers ("founding member" pricing)
- This locks in early customers at a price they feel great about while establishing a higher anchor for future customers

#### Pattern: "Lifetime Deal for Validation"

Offer a one-time lifetime access price during the validation phase to generate immediate revenue and prove willingness to pay.

- Typical range: 2-3x the annual subscription price
- Cap the number of lifetime deals (50-200) to avoid long-term liability
- Use this to fund the first few months of development; transition to subscription for all new customers afterward

#### Pattern: "Three Tiers"

When offering a subscription, use three tiers:

| Tier | Purpose | Pricing Psychology |
|------|---------|-------------------|
| **Free / Starter** | Remove risk; let users experience value before paying | Generous enough to be useful but limited enough to create upgrade desire |
| **Pro / Standard** | The tier most customers should choose; includes all core features | Price at 1x your target; this is your "real" price |
| **Team / Business** | For power users or small teams; includes everything + priority support | Price at 2-3x Pro; makes Pro look like a good deal (anchoring effect) |

The free tier is optional and depends on whether your product benefits from network effects or if a trial period is sufficient.

#### Pattern: "Charge from Day One"

Do not launch as free with plans to monetize later. Charging from day one:
- Validates willingness to pay immediately
- Attracts customers who value the product (not freebie-seekers)
- Provides revenue to fund development
- Sets the expectation that the product is worth paying for

If offering a free tier, ensure it has clear, visible limitations that create natural upgrade motivation.

### 6.4 Pricing Anti-Patterns (What to Avoid)

| Anti-Pattern | Why It Fails | Better Approach |
|-------------|-------------|-----------------|
| **Free forever with no plan to monetize** | Builds a user base that resists paying; no revenue signal | Charge from day one, even a small amount |
| **Pricing based on cost** | Your costs (hosting, API calls) are irrelevant to customers; they pay for value | Price based on the value delivered or the time/money saved |
| **Complex usage-based pricing** | Confuses customers; creates unpredictable bills; hard to explain | Simple tiers or straightforward per-unit pricing |
| **Matching the cheapest competitor** | Race to the bottom; signals low quality | Match or exceed competitors on value; price at or above mid-market |
| **No annual discount** | Misses an opportunity to reduce churn and improve cash flow | Offer 15-20% discount for annual prepayment |
| **Enterprise pricing for solo-dev products** | Cannot deliver the support and SLAs enterprise customers expect | Stay focused on SMB and individual pricing until you have a team |
