# Execution Plan: {Idea Name}

**Idea:** [{Idea Name}](../../{idea-NNN}.md)
**Market:** [{market-slug}](../../MARKET.md)
**Idea ID:** idea-{NNN}
**Validation Status:** {Validated | Partially Validated | Unvalidated}
**Plan Created:** {YYYY-MM-DD}
**Suggested Spec Name:** {idea-name-lowercase-hyphenated}

<!-- The Suggested Spec Name is the recommended name to use when running /create-spec. It should be a short, descriptive, lowercase-hyphenated identifier (e.g., "invoice-generator", "meal-planner-api", "freelancer-crm"). -->

---

## User Constraints Summary

> Constraints sourced from the user's generate-idea profile. These shape every recommendation in this plan.

- **Available time:** {X hours/week, or "Unknown -- assumed 10 hrs/week"}
- **Budget:** {Dollar amount or range, or "Unknown -- assumed $0 bootstrap"}
- **Technical skills:** {List of stated skills, or "Unknown -- assumed general programming ability"}
- **Non-technical skills:** {Relevant business/domain skills, or "None stated"}
- **Risk tolerance:** {Conservative / Moderate / Aggressive, or "Unknown -- assumed moderate"}
- **Timeline goal:** {Target launch date or timeframe, or "Unknown -- assumed 4-8 weeks to MVP"}

<!-- If user profile data is incomplete or unavailable, include this block: -->

<!--
> **Incomplete Profile Data**
>
> Some user constraints could not be determined from the generate-idea profile. The following assumptions were made:
> - {Constraint}: assumed {default value} because {reasoning}
> - {Constraint}: assumed {default value} because {reasoning}
>
> These assumptions affect the milestone timeline and scope recommendations below. If the assumptions are wrong, adjust the plan accordingly or re-run `/plan-execution` after providing updated constraints.
-->

---

## MVP Scope Definition

> The MVP is the smallest version of this product that can prove revenue potential. It is NOT a feature-complete product. Every feature below must directly contribute to answering: "Will someone pay for this?"

### Core Value Proposition

{One sentence describing what the MVP does and why a customer would pay for it.}

### Must-Have Features (MVP)

> These are the only features to build before seeking paying customers. Resist the urge to add more. Each feature below maps to a requirement for `/create-spec`.

1. **{Feature 1}:** {Brief description. Why it is essential for proving revenue.}
   - *User story:* As a {user type}, I want to {action} so that {benefit}.
   - *Key behaviors:* {1-2 specific behaviors that define "done" for this feature.}
2. **{Feature 2}:** {Brief description. Why it is essential for proving revenue.}
   - *User story:* As a {user type}, I want to {action} so that {benefit}.
   - *Key behaviors:* {1-2 specific behaviors that define "done" for this feature.}
3. **{Feature 3}:** {Brief description. Why it is essential for proving revenue.}
   - *User story:* As a {user type}, I want to {action} so that {benefit}.
   - *Key behaviors:* {1-2 specific behaviors that define "done" for this feature.}

### Nice-to-Have Features (Post-Validation)

> Build these ONLY after the MVP is generating revenue or has strong validation signals. They improve the product but are not required to prove the concept.

1. **{Feature A}:** {Brief description. What it adds beyond MVP.}
   - *User story:* As a {user type}, I want to {action} so that {benefit}.
2. **{Feature B}:** {Brief description. What it adds beyond MVP.}
   - *User story:* As a {user type}, I want to {action} so that {benefit}.
3. **{Feature C}:** {Brief description. What it adds beyond MVP.}
   - *User story:* As a {user type}, I want to {action} so that {benefit}.

### Explicitly Out of Scope

> Features that might seem obvious but should NOT be built yet. Including this list prevents scope creep.

- {Feature/capability to avoid}: {Why it is out of scope for now.}
- {Feature/capability to avoid}: {Why it is out of scope for now.}

---

## Feature Prioritization

> Priorities use the P0-P3 system compatible with agent-alchemy `/create-spec`:
> - **P0 (Critical):** Must have for initial release -- MVP cannot function without it
> - **P1 (High):** Important, build immediately after P0 -- significantly improves core experience
> - **P2 (Medium):** Nice to have post-validation -- improves product but users can work around absence
> - **P3 (Low):** Future consideration -- build only if users request it

| Priority | Feature | Effort Estimate | Revenue Impact | Rationale |
|----------|---------|-----------------|----------------|-----------|
| P0 (Critical) | {Feature 1} | {Hours/days} | {Direct / Indirect / None} | {Why this is critical for MVP} |
| P0 (Critical) | {Feature 2} | {Hours/days} | {Direct / Indirect / None} | {Why this is critical for MVP} |
| P1 (High) | {Feature 3} | {Hours/days} | {Direct / Indirect / None} | {Why this is high priority} |
| P2 (Medium) | {Feature A} | {Hours/days} | {Direct / Indirect / None} | {Why this can wait until post-validation} |
| P2 (Medium) | {Feature B} | {Hours/days} | {Direct / Indirect / None} | {Why this can wait until post-validation} |
| P3 (Low) | {Feature X} | {Hours/days} | {Direct / Indirect / None} | {Why this is a future consideration} |

---

## Milestone Breakdown

### Milestone 1: {Name} -- Foundation

**Target duration:** {X days/weeks}
**Focus:** {What this milestone accomplishes.}

**Deliverables:**
- [ ] {Deliverable 1}
- [ ] {Deliverable 2}
- [ ] {Deliverable 3}

**Completion criteria:**
- {Specific, testable criterion -- e.g., "User can sign up and complete core action"}
- {Specific, testable criterion -- e.g., "Landing page is live and accessible"}
- {Specific, testable criterion -- e.g., "Core data model is functional with test data"}

---

### VALIDATION CHECKPOINT 1: Smoke Test

> **Stop and assess before continuing.** This checkpoint determines whether to proceed, pivot, or stop.

**When:** After Milestone 1 is complete.
**What to validate:**
- {Question to answer -- e.g., "Can you demonstrate the core value to a potential user?"}
- {Question to answer -- e.g., "Does the technical approach work without major blockers?"}

**How to validate:**
- {Action -- e.g., "Show the working prototype to 3 people in your target audience."}
- {Action -- e.g., "Post a demo or screenshot in {relevant community}."}

**Go/no-go signals:**
- **Proceed if:** {Positive signal -- e.g., "At least 1 person says they would use/pay for this."}
- **Pivot if:** {Concerning signal -- e.g., "People understand the value but want a different core feature."}
- **Stop if:** {Negative signal -- e.g., "No one in the target audience shows interest after 5+ conversations."}

---

### Milestone 2: {Name} -- Revenue-Ready MVP

**Target duration:** {X days/weeks}
**Focus:** {What this milestone accomplishes.}

**Deliverables:**
- [ ] {Deliverable 1}
- [ ] {Deliverable 2}
- [ ] {Deliverable 3}

**Completion criteria:**
- {Specific, testable criterion -- e.g., "Payment flow works end-to-end with test transaction"}
- {Specific, testable criterion -- e.g., "First 3 beta users can complete the core workflow"}
- {Specific, testable criterion -- e.g., "Pricing page is live with clear value proposition"}

---

### VALIDATION CHECKPOINT 2: Revenue Test

> **Critical checkpoint.** This is the moment of truth -- will someone pay?

**When:** After Milestone 2 is complete.
**What to validate:**
- {Question -- e.g., "Will a real customer complete a purchase?"}
- {Question -- e.g., "What objections arise during the buying process?"}

**How to validate:**
- {Action -- e.g., "Offer the product to 10 people in your target audience at your planned price."}
- {Action -- e.g., "Run a 48-hour launch in {specific community/channel}."}

**Go/no-go signals:**
- **Proceed if:** {Positive signal -- e.g., "At least 1 paying customer within the first week."}
- **Pivot if:** {Concerning signal -- e.g., "Interest but price objections -- consider adjusting pricing model."}
- **Stop if:** {Negative signal -- e.g., "Zero conversions after 20+ qualified leads see the offer."}

---

### Milestone 3: {Name} -- Growth Foundation

**Target duration:** {X days/weeks}
**Focus:** {What this milestone accomplishes post-validation.}

**Deliverables:**
- [ ] {Deliverable 1}
- [ ] {Deliverable 2}
- [ ] {Deliverable 3}

**Completion criteria:**
- {Specific, testable criterion -- e.g., "Automated onboarding flow reduces setup time by 50%"}
- {Specific, testable criterion -- e.g., "3 nice-to-have features shipped based on user feedback"}
- {Specific, testable criterion -- e.g., "Monthly recurring revenue exceeds ${amount}"}

---

### VALIDATION CHECKPOINT 3: Growth Test

> **Assess scalability.** Can you grow beyond your initial users?

**When:** After Milestone 3 is complete.
**What to validate:**
- {Question -- e.g., "Are users finding you without direct outreach?"}
- {Question -- e.g., "Is retention strong enough to sustain growth?"}

**How to validate:**
- {Action -- e.g., "Review acquisition channels -- what percentage is organic vs. direct outreach?"}
- {Action -- e.g., "Calculate 30-day retention rate."}

**Go/no-go signals:**
- **Proceed if:** {Positive signal -- e.g., "Organic sign-ups are growing week over week."}
- **Pivot if:** {Concerning signal -- e.g., "Retention is low -- users try once but don't return."}
- **Stop if:** {Negative signal -- e.g., "Growth has plateaued despite active marketing efforts."}

<!-- Add more milestones as needed. Each milestone MUST be followed by a validation checkpoint. -->

---

## Revenue Model Implementation Details

### Pricing Approach

**Model:** {Subscription | One-time purchase | Freemium | Usage-based | Marketplace | Other}
**Rationale:** {Why this pricing model fits this product and market.}

### Pricing Recommendations

| Tier | Price | What's Included | Target Segment |
|------|-------|-----------------|----------------|
| {Free / Trial} | {$0 or trial period} | {Limited features or time-bound access} | {Who this tier serves} |
| {Core / Pro} | {$X/month or one-time} | {Full feature set} | {Primary paying segment} |
| {Premium} | {$X/month or one-time} | {Additional features or limits} | {Power users or teams} |

<!-- Not all tiers are required. A single-tier price is perfectly valid for an MVP. -->

**Price justification:**
- Competitor benchmark: {What competitors charge for similar value.}
- Willingness-to-pay signals: {Evidence from idea document or validation report.}
- Solo developer margin: {Whether this price covers costs and provides meaningful income at realistic volumes.}

### Payment Integration Needs

- **Payment processor:** {Recommended option -- e.g., Stripe, Lemon Squeezy, Gumroad, Paddle}
- **Integration complexity:** {Simple / Moderate / Complex -- with brief explanation}
- **Billing features needed for MVP:** {e.g., "One-time charge only" or "Monthly subscription with cancellation"}
- **Estimated integration time:** {Hours/days}

### Monetization Timeline

- **Pre-launch:** {Any revenue activities before full launch -- e.g., pre-sales, waitlist deposits}
- **Launch:** {When first revenue is expected relative to development start}
- **Break-even estimate:** {When revenue might cover ongoing costs, given realistic user projections}

---

## Customer Acquisition Strategy

### First 10 Users (Validation Phase)

> These are your proof-of-concept users. Prioritize learning over growth.

- **Channel 1:** {Specific community or outreach method -- e.g., "Post in r/{subreddit} with a demo and feedback request."}
- **Channel 2:** {Specific community or outreach method.}
- **Channel 3:** {Direct outreach -- e.g., "DM 20 people who have posted about this problem on {platform}."}

**Goal:** Validate that real people find value in the product. Revenue is a bonus at this stage.

### First 100 Users (Traction Phase)

> Repeatable channels that can scale beyond personal outreach.

- **Channel 1:** {Scalable channel -- e.g., "SEO-optimized landing page targeting '{keyword}'."}
- **Channel 2:** {Scalable channel -- e.g., "Weekly posts in {community} sharing insights from the product's domain."}
- **Channel 3:** {Scalable channel -- e.g., "Referral incentive -- existing users get {benefit} for inviting others."}

**Goal:** Establish at least one channel that consistently brings new users without manual effort per user.

### First 1,000 Users (Growth Phase)

> Strategies that require the product to be proven and polished.

- **Channel 1:** {Growth channel -- e.g., "Content marketing / blog targeting long-tail keywords in the niche."}
- **Channel 2:** {Growth channel -- e.g., "Partnerships or integrations with complementary tools."}
- **Channel 3:** {Growth channel -- e.g., "Paid acquisition (only if unit economics support it)."}

**Goal:** Sustainable growth with positive unit economics.

---

## Launch Strategy

### Pre-Launch (Before MVP is complete)

- [ ] {Action -- e.g., "Create a landing page with email capture."}
- [ ] {Action -- e.g., "Start participating in target communities to build presence."}
- [ ] {Action -- e.g., "Identify 5-10 potential beta users and share progress with them."}

### Soft Launch (MVP ready, limited audience)

- [ ] {Action -- e.g., "Invite beta users and collect structured feedback."}
- [ ] {Action -- e.g., "Fix critical issues found during beta."}
- [ ] {Action -- e.g., "Set up payment processing and pricing page."}

### Public Launch

- [ ] {Action -- e.g., "Post on Product Hunt / Indie Hackers / Hacker News."}
- [ ] {Action -- e.g., "Share in all identified target communities simultaneously."}
- [ ] {Action -- e.g., "Begin tracking key metrics: sign-ups, conversions, retention."}

---

## Skills Gap Assessment

<!-- Include this section if the idea requires skills the user lacks. Omit if all required skills are covered. -->

<!-- If the user's skill profile is unavailable, include this block instead: -->

<!--
> **Skills Assessment Unavailable**
>
> The user's technical skill profile was not available from the generate-idea output. This section assumes general programming ability. If you have specialized skills or significant gaps, re-run `/plan-execution` after providing your skill set, or manually review the technical requirements below.
-->

| Required Skill | User Has It? | Gap Strategy |
|----------------|-------------|--------------|
| {Skill 1 -- e.g., "Frontend (React)"} | {Yes / No / Partial} | {If gap: "Use {template/framework} to minimize custom frontend work" or "Outsource to {platform} -- estimated ${cost}"} |
| {Skill 2 -- e.g., "Payment integration"} | {Yes / No / Partial} | {If gap: "Follow Stripe's quickstart guide (~2 hrs)" or "Use no-code payment link as MVP alternative"} |
| {Skill 3 -- e.g., "Marketing / copywriting"} | {Yes / No / Partial} | {If gap: "Use AI-assisted copy generation" or "Hire freelance copywriter for landing page -- ~${cost}"} |

**Recommended learning resources:**
- {Resource 1 -- e.g., "Stripe Integration Guide: {URL} (estimated 2-4 hours)"}
- {Resource 2 -- e.g., "{Framework} crash course: {URL} (estimated 1 weekend)"}

**Outsourcing recommendations:**
- {Component to outsource}: {Platform -- e.g., Fiverr, Upwork} | Estimated cost: {$range} | Timeline: {days}

---

## Next Steps: Agent-Alchemy Handoff

> This execution plan is designed to feed directly into technical implementation via agent-alchemy's `/create-spec` command. The MVP features, user stories, and priorities above are structured to map directly to spec requirements.

### How to Generate a Technical Spec

1. **Run `/create-spec`** in Claude Code.
2. **When prompted for a spec name**, use: **{Suggested Spec Name from header above}**
3. **When prompted for type**, select: "New product"
4. **When prompted for depth**, select: "Detailed specifications" (recommended) or "Full technical documentation"
5. **When prompted for a description**, paste or reference the **Core Value Proposition** and **Must-Have Features** sections from this plan.
6. **During the interview rounds**, use this plan as your reference. The feature descriptions, user stories, key behaviors, and priority levels (P0-P3) above are formatted to answer `/create-spec`'s questions directly.

> **Tip:** You can share this entire plan file path with `/create-spec` as context. When asked about features, priorities, or scope, refer back to the relevant section.

<!-- If the plan has more than 5-6 MVP features, include this phased spec guidance: -->

### Phased Spec Approach (For Plans with Many Features)

If this plan defines more than 5-6 MVP features, consider generating specs in phases rather than one monolithic spec:

1. **Phase 1 spec:** Include only P0 (Critical) features. Run `/create-spec` scoped to just these features. Then use `/create-tasks` and `/execute-tasks` to build and ship them first.
2. **Phase 2 spec:** After P0 features are built and validated, run `/create-spec` again for P1 (High) features, using the existing codebase as context.
3. **Phase 3 spec:** After revenue validation, run `/create-spec` for P2 (Medium) features based on actual user feedback.

This phased approach keeps each spec focused, avoids scope creep, and lets real-world feedback inform later phases.

### Immediate Actions

1. **Review this plan.** Adjust milestones, pricing, and scope based on your judgment. This plan is a starting point, not a rigid prescription.
2. **Commit to Milestone 1.** Set a start date and block time on your calendar.
3. **Generate a technical spec.** Follow the steps above to run `/create-spec` with this plan as context.
4. **Execute the spec.** Once the spec is created, use `/create-tasks` to generate implementation tasks, then `/execute-tasks` to build the MVP.

### Recommended Workflow

```
This plan (plan-execution output)
    |
    v
/create-spec  --> Technical specification (PRD)
    |               Use Suggested Spec Name: {spec name from header}
    |               Scope to P0 + P1 features for MVP
    v
/create-tasks --> Implementation task list
    |
    v
/execute-tasks --> Working MVP code
    |
    v
Validate with real users (see Validation Checkpoints above)
    |
    v
/create-spec  --> Next-phase spec (P2+ features, informed by user feedback)
```

### Before You Start Building

- [ ] Confirm your available time commitment for the next {X weeks}
- [ ] Set up your development environment for {key technology}
- [ ] Join {1-2 target communities} and start observing conversations
- [ ] Identify your first 3 potential beta users by name or handle

---

*Generated by [startup-idea-generator](../../README.md) | Idea: idea-{NNN} | Market: {market-slug} | {YYYY-MM-DD}*
