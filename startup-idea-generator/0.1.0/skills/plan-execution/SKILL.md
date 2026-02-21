---
name: plan-execution
description: |
  Generates a concrete execution plan for a validated startup idea.
  Creates MVP scope, milestones, launch strategy, and customer acquisition plan
  formatted to feed directly into agent-alchemy /create-spec.
user-invocable: true
disable-model-invocation: false
allowed-tools:
  - AskUserQuestion
  - Task
  - Read
  - Write
  - Glob
  - Grep
argument-hint: "[idea-path] [--validation validation-path]"
arguments:
  - name: idea-path
    description: Path to the idea document
    required: true
  - name: validation-path
    description: Path to the validation report (optional)
    required: false
model: opus
---

# Plan Execution Skill

You are initiating the execution planning workflow. This process takes a validated (or unvalidated) startup idea and produces a concrete, actionable execution plan -- MVP scope, milestones, launch strategy, revenue model, and customer acquisition plan -- designed for a solo developer and structured to feed directly into agent-alchemy `/create-spec`.

## Critical Rules

### AskUserQuestion is MANDATORY

**IMPORTANT**: You MUST use the `AskUserQuestion` tool for ALL questions to the user. Never ask questions through regular text output.

- Constraint-gathering questions -> AskUserQuestion
- Confirmation prompts -> AskUserQuestion
- Priority choices -> AskUserQuestion
- Yes/no decisions -> AskUserQuestion

Text output should only be used for:
- Summarizing what you have learned
- Presenting plan sections
- Explaining context or rationale

If you need the user to make a choice or provide input, use AskUserQuestion.

**NEVER do this** (asking via text output):
```
How much time do you have per week to work on this?
1. Less than 5 hours
2. 5-10 hours
3. 10-20 hours
```

**ALWAYS do this** (using AskUserQuestion tool):
```yaml
AskUserQuestion:
  questions:
    - header: "Available Time"
      question: "How many hours per week can you dedicate to building this product?"
      options:
        - label: "Less than 5 hours/week"
          description: "Side project pace -- MVP in 3-6 months"
        - label: "5-10 hours/week"
          description: "Steady progress -- MVP in 2-4 months"
        - label: "10-20 hours/week"
          description: "Serious commitment -- MVP in 1-2 months"
        - label: "20+ hours/week (full-time)"
          description: "Full-time focus -- MVP in 2-4 weeks"
      multiSelect: false
```

### Plan Mode Behavior

**CRITICAL**: This skill generates an execution plan for the user's idea, NOT an implementation plan for the skill itself. When invoked during Claude Code's plan mode:

- **DO NOT** create an implementation plan for how to build this skill
- **DO NOT** defer plan generation to an "execution phase"
- **DO** proceed with the full planning workflow immediately
- **DO** write the execution plan to the output path as normal

Generating the execution plan IS the planning activity.

---

## Workflow Overview

This workflow has nine phases:

1. **Load & Parse** -- Read idea document and optional validation report
2. **Load User Profile** -- Check for generate-idea profile data; prompt for constraints if unavailable
3. **Load References** -- Read planning patterns and execution plan template
4. **MVP Scoping** -- Define minimal feature set using planning patterns
5. **Milestone Planning** -- Create milestone breakdown with completion criteria
6. **Revenue Strategy** -- Define pricing, acquisition strategy for first 10/100/1000 users
7. **Validation Checkpoints** -- Insert checkpoints where user should assess real-world feedback
8. **Output** -- Write execution plan to `ideas/{market-slug}/execution-plans/idea-NNN-plan.md`
9. **Present Results** -- Show plan summary, offer next steps (handoff to `/create-spec`)

---

## Phase 1: Load & Parse

### Read the Idea Document

Read the idea document at the path provided by the user:

```
Read: {idea-path}
```

Extract the following from the idea document:
- **Idea name** and one-line summary
- **Market slug** and market name
- **Idea ID** (e.g., idea-001)
- **Target customer** profile and pain points
- **Revenue model** and pricing signals
- **Competitive landscape** and differentiation
- **Time to MVP** estimate (if present)
- **Scoring breakdown** (if present)
- **Validation status** and saturation flag

If the idea document path is invalid or the file cannot be read, use `AskUserQuestion` to prompt the user for the correct path:

```yaml
AskUserQuestion:
  questions:
    - header: "Idea Document Not Found"
      question: "The idea document at '{idea-path}' could not be found. Please provide the correct path to your idea document."
      options:
        - label: "Let me type the path"
          description: "I'll provide the correct file path"
        - label: "List my idea files"
          description: "Search for idea documents in the ideas/ directory"
      multiSelect: false
```

If the user selects "List my idea files," use `Glob` to search for `ideas/**/idea-*.md` and present the results.

### Read the Validation Report (Optional)

If a `--validation` path was provided, read the validation report:

```
Read: {validation-path}
```

Extract the following from the validation report:
- **Go/No-Go recommendation** and confidence level
- **Risk factors** identified
- **Demand signals** (confirmed or unconfirmed)
- **Competitive moat assessment**
- **Customer willingness-to-pay signals**
- **Recommended pivots or adjustments**

If the validation report path was provided but the file cannot be found, inform the user and proceed without it:

> **Note**: Validation report at `{validation-path}` could not be found. Proceeding with execution plan based on the idea document alone. Consider running `/validate-idea` later for deeper analysis.

If no validation path was provided, proceed normally -- the validation report is optional.

### Read Market Analysis

Attempt to read the market analysis file for context:

```
Read: ideas/{market-slug}/MARKET.md
```

If the market analysis exists, extract:
- Market size estimates and trend direction
- Key players and competitive landscape
- Underserved niches
- Why this market fits the user

If the market analysis does not exist, proceed without it.

---

## Phase 2: Load User Profile

### Check for Generate-Idea Profile

Look for user profile data from a previous `generate-idea` session. Profile data may be available in several locations:

1. Check the idea document itself -- it may contain personal fit data (skills match, interest alignment, network advantage, constraint compatibility)
2. Check the generate-idea interview output -- look for a profile section in the market analysis or idea files
3. Check if the skill was invoked as a sub-skill from `generate-idea` -- in which case the user profile may be included in the invocation prompt

Extract whatever profile data is available:
- **Available time** (hours/week)
- **Budget** (monthly investment capacity)
- **Technical skills** (languages, frameworks, experience level)
- **Non-technical skills** (business, domain, marketing)
- **Risk tolerance** (conservative, moderate, aggressive)
- **Timeline goal** (target launch timeframe)
- **Network** (relevant communities, contacts)

### Prompt for Missing Constraints

If key constraints are missing or the profile data is unavailable, prompt the user using `AskUserQuestion`. At minimum, you need **available time** and **technical skills** to produce a realistic plan.

```yaml
AskUserQuestion:
  questions:
    - header: "Your Constraints"
      question: "To create a realistic execution plan, I need to understand your constraints. How much time per week can you dedicate to building this product?"
      options:
        - label: "Less than 5 hours/week"
          description: "Side project pace -- expect 3-6 months to MVP"
        - label: "5-10 hours/week"
          description: "Steady progress -- expect 2-4 months to MVP"
        - label: "10-20 hours/week"
          description: "Serious commitment -- expect 1-2 months to MVP"
        - label: "20+ hours/week (full-time)"
          description: "Full-time focus -- expect 2-4 weeks to MVP"
      multiSelect: false
```

If technical skills are also unknown, ask a follow-up:

```yaml
AskUserQuestion:
  questions:
    - header: "Your Technical Skills"
      question: "What best describes your technical background? This helps me scope the MVP appropriately."
      options:
        - label: "Full-stack developer"
          description: "Comfortable with frontend, backend, and deployment"
        - label: "Backend-focused"
          description: "Strong on server-side; less experienced with UI/frontend"
        - label: "Frontend-focused"
          description: "Strong on UI; less experienced with backend/infrastructure"
        - label: "General programmer"
          description: "Can code but not specialized in web/app development"
        - label: "Non-technical or learning"
          description: "Minimal coding experience; may need no-code or outsourcing guidance"
      multiSelect: false
```

If budget and risk tolerance are unknown, assume moderate defaults:
- **Budget**: $0 bootstrap (no paid tools or services unless free tiers available)
- **Risk tolerance**: Moderate (willing to invest time but cautious with money)
- **Timeline goal**: 4-8 weeks to MVP (adjusted based on available time)

Document all assumptions clearly in the User Constraints Summary section of the output.

---

## Phase 3: Load References

### Read Planning Patterns

Load the planning patterns reference for MVP scoping rules, milestone templates, solo-developer constraints, validation checkpoints, customer acquisition strategies, and pricing patterns:

```
Read: references/planning-patterns.md
```

### Read Execution Plan Template

Load the execution plan template to use as the output structure:

```
Read: references/templates/execution-plan.md
```

---

## Phase 4: MVP Scoping

### Define the Revenue Hypothesis

Before scoping any features, articulate the revenue hypothesis in one sentence:

```
"{Target customer} will pay ${price} per {period} for {core value proposition}."
```

This sentence is the north star for the entire MVP scope. Every feature must directly support proving or disproving this hypothesis.

### Identify the Core Workflow

Determine the single core workflow that delivers the product's value. The MVP implements exactly one primary workflow -- the shortest path from user input to value delivered. Ask yourself:

- What is the one thing a user does with this product that makes them willing to pay?
- What is the minimum sequence of steps to deliver that value?
- What can be done manually, deferred, or skipped entirely?

### Apply Scoping Rules

Using the scoping rules from `references/planning-patterns.md`, evaluate every proposed feature:

1. **One core workflow** -- The MVP solves exactly one problem through one primary workflow
2. **Manual before automated** -- Replace complex automation with manual processes where possible
3. **Integrate, don't build** -- Use existing services (Stripe, Auth0, SendGrid) instead of building infrastructure
4. **Ugly is fine** -- Functional UI over polished UI; use CSS frameworks with defaults
5. **No admin panel** -- Manage data via database directly until you have paying customers
6. **No scale engineering** -- Design for 10-100 users, not 10,000
7. **Auth can wait** -- Simple auth flow; skip OAuth integrations for launch
8. **Ship weekly** -- If the MVP takes more than 4 weeks of solo dev time, the scope is too large

### Apply the Feature Triage Matrix

Classify every proposed feature into one of four categories:

| Category | Criteria | Action |
|----------|----------|--------|
| **Must Have** | Directly required for the revenue hypothesis test; product is non-functional without it | Include in MVP |
| **Should Have** | Improves the experience but users can work around its absence | Defer to post-MVP milestone |
| **Nice to Have** | Makes the product more polished or competitive; no user has asked for it yet | Defer to growth phase or cut entirely |
| **Trap** | Technically interesting but does not contribute to revenue; often the features devs want to build most | Cut ruthlessly; revisit only if users request it |

### Scope Reduction

If the MVP scope still exceeds 4 weeks of development time (adjusted for user's available hours), apply scope reduction techniques:

1. **Narrow the audience** -- More specific target = fewer edge cases = smaller product
2. **Reduce the output** -- Simpler output format (email report instead of dashboard)
3. **Limit the input** -- Accept only one file format, one integration, or one data source
4. **Time-box the interaction** -- On-demand tool instead of always-on service
5. **Concierge MVP** -- Deliver value manually to the first 5-10 customers while building automation

### Confirm Scope with User

Present the proposed MVP scope to the user and confirm before proceeding:

```yaml
AskUserQuestion:
  questions:
    - header: "MVP Scope Review"
      question: "Here is the proposed MVP scope. Does this feel right, or would you like to adjust?"
      options:
        - label: "Looks good -- proceed"
          description: "The scope is appropriate; continue with milestone planning"
        - label: "Too ambitious -- cut more"
          description: "I want an even smaller MVP; help me reduce further"
        - label: "Too small -- add a feature"
          description: "I think a key feature is missing from the MVP"
        - label: "Let me describe what I want"
          description: "I have specific scope preferences to share"
      multiSelect: false
```

If the user wants to cut more, apply additional scope reduction. If the user wants to add a feature, evaluate it against the triage matrix and explain the trade-off (added scope = longer timeline).

---

## Phase 5: Milestone Planning

### Build Milestone Breakdown

Using the milestone templates from `references/planning-patterns.md`, create a milestone breakdown tailored to this idea and user. The standard progression is:

#### Milestone 0: Validation (Pre-Build)

**Objective**: Confirm demand before writing code.

Activities:
- Create a landing page with the core value proposition and email signup
- Drive traffic via target communities identified in the idea document
- Collect email signups or waitlist registrations
- Conduct 3-5 brief conversations with potential customers
- Document findings: problem confirmed, willingness to pay confirmed, feedback themes

**Completion criteria** must be specific and measurable -- e.g., "At least 10 signups from the target audience within 1 week."

#### Milestone 1: MVP Build

**Objective**: Build the minimum product that proves the revenue hypothesis.

Activities:
- Implement the single core workflow
- Integrate payment processing (Stripe Checkout or equivalent)
- Deploy to production (public URL, basic reliability)
- Set up basic error monitoring and analytics
- Write a getting-started guide or onboarding flow

**Completion criteria** -- e.g., "Core workflow functional end-to-end; at least one person outside immediate circle has used it."

#### Milestone 2: First Revenue

**Objective**: Get the first paying customer.

Activities:
- Announce to waitlist and target communities
- Offer early-adopter pricing or lifetime deals
- Provide hands-on support to every early user
- Collect and act on feedback
- Track conversion funnel

**Completion criteria** -- e.g., "At least 1 paying customer (not a friend or family member)."

#### Milestone 3: Growth Foundation

**Objective**: Establish repeatable acquisition and retention.

Activities:
- Reach $100+ MRR or 10+ paying customers
- Identify top 1-2 acquisition channels
- Implement top 3 feature requests from paying customers
- Reduce manual processes
- Set up retention tracking

**Completion criteria** -- e.g., "MRR growing month-over-month; at least one repeatable acquisition channel identified."

### Adjust Timelines for User Constraints

Use the time budget table from `references/planning-patterns.md` to adjust milestone durations:

| Available Time | Realistic Weekly Output | MVP Timeline |
|----------------|------------------------|--------------|
| <5 hours/week | One small feature or fix | 3-6 months |
| 5-10 hours/week | One medium feature | 2-4 months |
| 10-20 hours/week | One feature + marketing/support | 1-2 months |
| 20-40 hours/week | Multiple features + marketing | 2-4 weeks |

Remember: only 40-50% of available time goes to writing code. The rest is support, marketing, operations, and planning.

### Apply Accelerated Timeline (If Applicable)

If the idea's market is fast-moving (validation report flagged speed-to-market urgency, or the market has emerging competitors), use the accelerated timeline variant:

| Milestone | Duration | Key Difference |
|-----------|----------|----------------|
| Validation | 3-5 days | Landing page only; skip conversations; signup count as sole signal |
| MVP Build | 1-2 weeks | Even more aggressive scope cuts; concierge where possible |
| First Revenue | 1-2 weeks | Launch immediately to waitlist; iterate publicly |
| Growth Foundation | 4-6 weeks | Same as standard |

---

## Phase 6: Revenue Strategy

### Select Pricing Model

Using the pricing patterns from `references/planning-patterns.md`, recommend a pricing model based on the product type and target audience:

| Product Type | Recommended Model | Rationale |
|-------------|-------------------|-----------|
| SaaS tool (ongoing value) | Monthly subscription with annual discount | Recurring revenue; predictable |
| One-time tool or utility | One-time purchase with optional support tier | Simpler to sell; no churn concern |
| API or developer tool | Usage-based with a free tier | Aligns cost with value |
| Content or education | One-time purchase or cohort-based pricing | Avoid subscription fatigue |
| Marketplace or platform | Transaction fee | Aligns revenue with user success |
| Community or network | Freemium with premium features | Free tier for network effects |

### Set Pricing Levels

Use the pricing level guidelines from `references/planning-patterns.md` as a floor:

| Target Audience | Price Floor (Monthly) |
|-----------------|----------------------|
| Individual consumers | $5-15/month |
| Freelancers / creators | $15-50/month |
| Small businesses (1-10 employees) | $30-100/month |
| Mid-market businesses (10-100 employees) | $100-500/month |

Apply the "Start High, Discount Down" pattern: set the list price 20-30% above your target, then offer early-adopter discounts.

### Define Customer Acquisition Strategy

Create stage-appropriate acquisition tactics:

**First 10 Customers (Manual, High-Touch)**:
- Personal network outreach to people matching the target audience
- Active participation in 2-3 target communities (identified from the idea document)
- Direct DMs to people who publicly express the problem
- Concierge onboarding for every early user
- What NOT to do: paid ads, SEO, content marketing, Product Hunt launch

**First 100 Customers (Scalable Manual + Early Content)**:
- Double down on the best channel from 0-10
- Launch on aggregators (Product Hunt, Hacker News, Indie Hackers)
- Write 3-5 SEO-targeted articles
- Build in public (Twitter/X or blog)
- Referral incentive for existing customers

**First 1,000 Customers (Systematic + Leverage)**:
- SEO as primary inbound channel (15-30 articles)
- Integration partnerships with complementary tools
- Email nurture sequence for new signups
- Paid acquisition with small budget (only after organic channels proven)
- Community building (Slack, Discord, forum)
- Case studies and testimonials

### Define Payment Integration

Recommend specific payment integration options based on the pricing model:
- **Payment processor**: Stripe, Lemon Squeezy, Gumroad, or Paddle -- based on product type and complexity
- **Integration complexity**: Simple / Moderate / Complex
- **Billing features needed for MVP**: minimal requirements only
- **Estimated integration time**: hours or days

### Define Monetization Timeline

- **Pre-launch**: any revenue activities before full launch (pre-sales, waitlist deposits)
- **Launch**: when first revenue is expected relative to development start
- **Break-even estimate**: when revenue might cover ongoing costs

---

## Phase 7: Validation Checkpoints

### Insert Checkpoints After Each Milestone

Every milestone MUST be followed by a validation checkpoint. Use the checkpoint definitions from `references/planning-patterns.md`:

#### Checkpoint 1: Pre-Build (After Milestone 0)

**Questions to answer**:
- Did at least 10 people from the target audience sign up?
- Did at least 2-3 potential customers confirm the problem is real?
- Did anyone express willingness to pay?
- Is there a clear, specific target customer?

**Go/No-Go signals**: Proceed, pivot, or stop based on results.

#### Checkpoint 2: Post-MVP (After Milestone 1)

**Questions to answer**:
- Are users completing the core workflow without hand-holding?
- How many users return after their first session?
- What is the most common drop-off point?
- Are users providing unsolicited feedback?

**Go/No-Go signals**: Proceed with revenue push, fix onboarding, or investigate activation.

#### Checkpoint 3: Revenue Test (After Milestone 2)

**Questions to answer**:
- How many people have paid?
- What is the conversion rate from free/trial to paid?
- What reasons do non-paying users give?
- Are paying customers continuing to use the product?

**Go/No-Go signals**: Double down, pivot pricing/audience, or consider killing the project.

#### Checkpoint 4: Growth Assessment (After Milestone 3)

**Questions to answer**:
- Are users finding you without direct outreach?
- Is retention strong enough to sustain growth?
- What is the MRR trajectory?

**Go/No-Go signals**: Continue scaling, investigate retention, or plateau response.

### Checkpoint Execution Rules

Include these rules in the output plan:
- **Never skip a checkpoint.** Skipping validation to "ship faster" is the most common solo-dev mistake.
- **Write down results.** Document checkpoint outcomes, not just in your head.
- **Set hard kill criteria.** Before starting, define what would make you abandon the project.
- **Time-box pivots.** Allow one pivot attempt (2 weeks). If the pivot also fails, move on.

---

## Phase 8: Output

### Assess Skills Gaps

Before writing the final plan, check if the idea requires skills the user does not have:

1. Compare required skills (frontend, backend, DevOps, marketing, design, domain-specific) against the user's stated skills
2. For each gap, recommend a specific approach from the skill gap handling patterns in `references/planning-patterns.md`:
   - Design gaps: component libraries, templates
   - Copywriting gaps: AI-assisted copy
   - Marketing/SEO gaps: community-based acquisition first
   - DevOps gaps: PaaS platforms (Vercel, Railway, Fly.io)
   - Mobile gaps: PWA or responsive web app
   - Domain knowledge gaps: partner with experts, concierge MVP
   - ML/Data science gaps: pre-built APIs
3. Include learning resource recommendations and outsourcing options with estimated costs

### Determine Output Path

Construct the output file path:

```
ideas/{market-slug}/execution-plans/idea-NNN-plan.md
```

Where:
- `{market-slug}` is extracted from the idea document (lowercase, hyphenated market name)
- `NNN` is the idea number from the idea document (e.g., 001, 002)

Create the `execution-plans/` directory if it does not exist.

### Write the Execution Plan

Write the complete execution plan using the template from `references/templates/execution-plan.md`. Populate all sections:

1. **Header** -- Idea name, market, idea ID, validation status, date, and **Suggested Spec Name** (a short, lowercase-hyphenated name for use with `/create-spec`)
2. **User Constraints Summary** -- All known constraints with assumptions documented
3. **MVP Scope Definition** -- Core value proposition, must-have features (with user stories and key behaviors), nice-to-have features (with user stories), explicitly out of scope
4. **Feature Prioritization** -- Priority table using P0-P3 system (P0 Critical, P1 High, P2 Medium, P3 Low) with effort estimates and revenue impact
5. **Milestone Breakdown** -- All milestones with deliverables, completion criteria, and target durations
6. **Validation Checkpoints** -- One after each milestone with go/no-go signals
7. **Revenue Model Implementation Details** -- Pricing approach, pricing table, payment integration, monetization timeline
8. **Customer Acquisition Strategy** -- 10/100/1000 user acquisition tactics
9. **Launch Strategy** -- Pre-launch, soft launch, public launch checklists
10. **Skills Gap Assessment** -- Required skills vs. user skills with gap strategies (if applicable)
11. **Next Steps: Agent-Alchemy Handoff** -- Step-by-step instructions for invoking `/create-spec` with this plan, phased spec approach for many features, recommended workflow

### Ensure Create-Spec Compatibility

The execution plan must be structured so that a user can hand it to agent-alchemy `/create-spec` as context for generating a technical specification. This means:

- **Suggested Spec Name** is included in the header so the user knows what to enter when `/create-spec` asks for a spec name
- MVP features include user stories and key behaviors that map directly to spec requirements and acceptance criteria
- Feature prioritization uses the P0-P3 system that `/create-spec` uses (P0 Critical, P1 High, P2 Medium, P3 Low)
- Milestone 1 deliverables provide a clear MVP definition for the spec
- Technology choices (when specified) are explicit
- Out-of-scope items are listed so the spec does not accidentally include them
- If the plan has more than 5-6 MVP features, the Next Steps section recommends a phased `/create-spec` approach (P0 features first, then P1, then P2) to avoid monolithic specs

---

## Phase 9: Present Results

### Show Plan Summary

Present a concise summary of the execution plan to the user:

```
### Execution Plan Summary

**Idea:** {Idea Name}
**Market:** {Market Name}
**MVP Timeline:** {Estimated time to MVP based on user's available hours}
**Revenue Hypothesis:** "{Target customer} will pay ${price} per {period} for {core value proposition}."

**MVP Scope:** {3-4 bullet points of must-have features}

**Milestones:**
1. Validation: {duration} -- Confirm demand before building
2. MVP Build: {duration} -- {brief description}
3. First Revenue: {duration} -- {brief description}
4. Growth Foundation: {duration} -- {brief description}

**Pricing:** {Model} at {price point} targeting {audience}
**First Acquisition Channel:** {Primary channel for first 10 users}

**Plan saved to:** ideas/{market-slug}/execution-plans/idea-NNN-plan.md
```

### Offer Next Steps

Use `AskUserQuestion` to present next steps, explicitly referencing agent-alchemy `/create-spec`:

```yaml
AskUserQuestion:
  questions:
    - header: "Next Steps"
      question: "Your execution plan is ready. What would you like to do next?"
      options:
        - label: "Generate a technical spec"
          description: "Run /create-spec with this execution plan to produce a detailed technical specification for the MVP"
        - label: "Review and adjust the plan"
          description: "Open the plan file and make manual adjustments before proceeding"
        - label: "Go back and validate the idea first"
          description: "Run /validate-idea if you haven't validated this idea yet"
        - label: "I'm done for now"
          description: "I'll review the plan on my own and come back later"
      multiSelect: false
```

### If User Selects "Generate a technical spec"

Provide instructions for using agent-alchemy `/create-spec`:

> To generate a technical specification from this execution plan, run:
>
> ```
> /create-spec
> ```
>
> When prompted, provide the execution plan file at `ideas/{market-slug}/execution-plans/idea-NNN-plan.md` as context. The `/create-spec` skill from [agent-alchemy-sdd-tools](https://github.com/agent-alchemy/agent-alchemy-sdd-tools) will generate a full technical specification covering architecture, data models, API design, and implementation tasks -- scoped to the MVP features defined in this plan.
>
> After the spec is created, use `/create-tasks` to generate implementation tasks, then `/execute-tasks` to build the MVP:
>
> ```
> /create-spec  --> Technical specification (PRD)
> /create-tasks --> Implementation task list
> /execute-tasks --> Working MVP code
> ```

### If User Selects "Go back and validate the idea first"

Invoke the `validate-idea` skill via the `Task` tool:

```
Task: "Run the validate-idea skill on the idea document at {idea_file_path}.
Use the market research already gathered in {market_slug}/MARKET.md as baseline context."

skill: validate-idea
```

### If User Selects "I'm done for now"

Summarize what was generated and remind the user of file locations and available next steps:

> Your execution plan has been saved to `ideas/{market-slug}/execution-plans/idea-NNN-plan.md`. When you are ready to proceed:
>
> 1. **Validate the idea**: Run `/validate-idea {idea-path}` for a deeper go/no-go analysis
> 2. **Generate a tech spec**: Run `/create-spec` and provide the execution plan as context
> 3. **Build the MVP**: After creating a spec, use `/create-tasks` then `/execute-tasks` to implement it
>
> The plan includes validation checkpoints at each milestone. Review the first checkpoint (Pre-Build) before writing any code.

---

## Error Handling

### Invalid Idea Document Path

If the idea document cannot be read at the provided path:
1. Inform the user that the file was not found
2. Use `AskUserQuestion` to prompt for the correct path
3. Offer to search for idea files using `Glob` on `ideas/**/idea-*.md`
4. Do not proceed until a valid idea document is loaded

### Validation Report Not Found

If a `--validation` path was provided but the file does not exist:
1. Inform the user that the validation report was not found
2. Proceed without it -- the validation report is optional
3. Note in the plan that no validation report was used and recommend running `/validate-idea`

### Insufficient Profile Data

If the user's profile data is unavailable or incomplete:
1. Prompt for the minimum required constraints (available time, technical skills) via `AskUserQuestion`
2. Use moderate defaults for optional constraints (budget, risk tolerance)
3. Document all assumptions in the User Constraints Summary section
4. Note that re-running `/plan-execution` after updating constraints may improve the plan

### Idea Requires Skills the User Lacks

If the idea requires skills the user does not have:
1. Include a Skills Gap Assessment section in the plan
2. For each gap, provide a specific recommendation: learning resource, outsourcing option, or alternative approach
3. Estimate additional time required for skill acquisition
4. Adjust milestone timelines to account for the learning curve
5. If the gap is significant (months of learning), flag this prominently and suggest whether the idea is still viable for this user

### Fast-Moving Markets

If the idea is in a fast-moving market (competitor landscape changing rapidly, emerging technology, time-sensitive opportunity):
1. Use the accelerated timeline variant from `references/planning-patterns.md`
2. Prioritize speed to market over feature completeness
3. Recommend concierge MVP or Wizard-of-Oz approach to compress validation
4. Note the trade-offs of speed vs. polish in the plan

---

## Reference Files

This skill loads the following reference files at the indicated workflow phases:

- `references/planning-patterns.md` -- MVP scoping patterns, milestone templates, solo-dev constraints, validation checkpoints, customer acquisition strategies, pricing patterns (loaded in Phase 3)
- `references/templates/execution-plan.md` -- Template for execution plan output files (loaded in Phase 3)
