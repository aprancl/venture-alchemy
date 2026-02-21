# User Profiling Interview Question Bank

This document contains the interview questions organized by category and round for gathering user profile data during the generate-idea skill's interview phase. All questions are designed for use with `AskUserQuestion` and should complete within 3-4 rounds maximum.

---

## Interview Structure Overview

The interview is organized into 3-4 adaptive rounds. Each round groups related categories to create a natural conversational flow. The number of rounds adapts based on user engagement: users who give detailed answers may trigger deeper follow-ups, while users who give brief or "skip" answers move through faster.

| Round | Categories Covered | Purpose |
|-------|-------------------|---------|
| **Round 1** | Technical Skills, Domain Experience, Interests & Passions | Understand who the user is and what they can build |
| **Round 2** | Available Time, Budget Constraints, Income Goals | Understand practical constraints and ambitions |
| **Round 3** | Risk Tolerance, Network | Understand market positioning and go-to-market capacity |
| **Round 4** (adaptive) | Follow-ups on gaps, contradictions, or areas needing depth | Fill in missing profile data; only triggered if needed |

**Round 4 triggers**: Round 4 is only conducted if any of the following conditions are met:
- User skipped 2+ categories in earlier rounds
- Contradictory answers detected (e.g., "no budget" + "high income goals")
- Insufficient data to generate personalized ideas (e.g., no skills AND no interests identified)

---

## Round 1: Skills, Experience, and Interests

### Category 1: Technical Skills

**Purpose**: Determine what the user can realistically build, their technology comfort zones, and their experience level.

#### Q1.1: Primary Technical Skills

**Question**: "What are your strongest technical skills?"

**Recommended Options** (select multiple or describe):
1. "Frontend development (React, Vue, Angular, etc.)"
2. "Backend/full-stack development (Node, Python, Go, etc.)"
3. "Mobile development (iOS, Android, React Native, Flutter)"
4. "Other / Let me describe my skills"

**Follow-up based on answer**:
- If option 1, 2, or 3: Ask Q1.2 (specific technologies)
- If option 4: Ask "What technologies or tools are you most comfortable with?" as a free-form follow-up
- If user seems uncertain: Offer Q1.1-ALT (see below)

**Adaptive behavior**:
- **Skip trigger**: Never skip this category -- technical skills are essential for idea personalization
- **Probe deeper trigger**: If user selects multiple areas, ask which they are strongest in and want to use for their project

#### Q1.1-ALT: Skills Exploration (for users with no clear technical focus)

**Question**: "Which of these best describes your technical experience?"

**Recommended Options**:
1. "I code professionally and have shipped production software"
2. "I code as a hobby and have built personal projects"
3. "I'm learning to code and have completed tutorials/courses"
4. "I have non-technical skills (design, marketing, sales, etc.)"

**Follow-up based on answer**:
- If option 1 or 2: Proceed to Q1.2
- If option 3: Note as early-stage developer; ask what they are learning and how far along they are
- If option 4: Ask what non-technical skills they have; note that ideas will lean toward no-code/low-code or partnership models

**Adaptive behavior**:
- If user selects option 3 or 4, adjust idea generation to favor simpler technical builds or platforms that require less custom code (e.g., Shopify apps, WordPress plugins, no-code tools)

#### Q1.2: Technology Depth

**Question**: "Which specific technologies are you most proficient with?"

**Recommended Options** (varies based on Q1.1 answer):
- For frontend: "React", "Vue", "Angular", "Svelte / Other"
- For backend: "Node.js/TypeScript", "Python", "Go/Rust", "Other"
- For mobile: "React Native", "Flutter", "Native iOS (Swift)", "Native Android (Kotlin)"

**Follow-up based on answer**:
- Note specific stack for idea filtering (e.g., Python proficiency opens ML/data ideas)
- If user lists an unusual stack, ask if they are open to adjacent technologies

**Adaptive behavior**:
- **Skip trigger**: If user already gave detailed skills in Q1.1 free-form answer, skip this question
- **Probe deeper trigger**: If user mentions ML/AI, data engineering, or DevOps, these unlock specialized idea categories

---

### Category 2: Domain Experience

**Purpose**: Identify industries and domains where the user has insider knowledge, which is a competitive advantage for building products.

#### Q2.1: Professional Background

**Question**: "What industries or domains have you worked in professionally?"

**Recommended Options**:
1. "Software/Tech (SaaS, developer tools, etc.)"
2. "Finance, Healthcare, or another regulated industry"
3. "E-commerce, Education, or Media/Content"
4. "Other / Multiple industries -- let me describe"

**Follow-up based on answer**:
- If option 1: Ask about specific sub-domains (B2B SaaS, dev tools, infrastructure, etc.)
- If option 2: Ask about specific pain points they have observed in that regulated environment
- If option 3: Ask about their role and what inefficiencies they noticed
- If option 4: Ask them to list their top 2-3 industries and what problems stood out

**Adaptive behavior**:
- **Skip trigger**: If user explicitly says they have no professional experience (e.g., student, career changer), skip to Q2.2
- **Probe deeper trigger**: If user mentions a niche industry (e.g., logistics, agriculture tech, legal tech), probe for specific pain points -- niche domain knowledge is a strong competitive advantage

#### Q2.2: Domain Knowledge Depth

**Question**: "In your strongest domain, how would you rate your industry knowledge?"

**Recommended Options**:
1. "Deep expertise -- I know the players, problems, and pricing"
2. "Working knowledge -- I understand the basics from experience"
3. "Surface level -- I've touched the industry but don't know it deeply"
4. "No strong domain -- I'm a generalist"

**Follow-up based on answer**:
- If option 1: This is a primary filter for idea generation; ask what the biggest unsolved problem in that domain is
- If option 2 or 3: Note domain as a secondary filter; ideas will need more market research validation
- If option 4: Skip domain-specific idea generation; focus on horizontal/cross-industry opportunities

**Adaptive behavior**:
- **Probe deeper trigger**: If user has deep expertise, this is the strongest signal for high-quality ideas -- spend extra time here

---

### Category 3: Interests and Passions

**Purpose**: Identify what motivates the user beyond money, which affects project sustainability and selection.

#### Q3.1: Core Interests

**Question**: "What problems or topics are you most excited about working on?"

**Recommended Options**:
1. "Developer productivity and tooling"
2. "Helping small businesses or creators"
3. "Automation, AI, or data-driven products"
4. "Something else -- let me describe"

**Follow-up based on answer**:
- For any selection: Ask "What specifically about [topic] excites you?" to get more granular
- If option 4: Let user describe freely, then map their interests to potential market categories

**Adaptive behavior**:
- **Skip trigger**: If user says "I don't care about the topic, I just want revenue," note this and skip follow-ups -- focus on pure market opportunity instead of passion fit
- **Probe deeper trigger**: If user describes a specific problem they personally experience, this is a strong idea signal -- probe for who else has this problem and how they currently solve it

#### Q3.2: Personal Pain Points

**Question**: "Is there a tool, service, or workflow in your life or work that frustrates you?"

**Recommended Options**:
1. "Yes -- there's something specific I wish existed"
2. "Yes -- something I use is terrible but I can't find a better option"
3. "Not really -- I'm open to exploring markets I don't personally use"
4. "Skip this question"

**Follow-up based on answer**:
- If option 1 or 2: Ask them to describe it in detail -- personal pain points often become the best product ideas
- If option 3: Note preference for market-driven (rather than pain-driven) idea generation
- If option 4: Skip and proceed

**Adaptive behavior**:
- **Probe deeper trigger**: If user describes a specific frustration, ask: "Have you looked for alternatives? What's the closest thing that exists?" This validates demand and identifies competitors early.

---

## Round 2: Constraints and Ambitions

### Category 4: Available Time

**Purpose**: Determine realistic build capacity, which directly affects idea scope and complexity recommendations.

#### Q4.1: Weekly Time Commitment

**Question**: "How much time can you dedicate to building a side project each week?"

**Recommended Options**:
1. "5-10 hours/week (evenings and weekends)"
2. "10-20 hours/week (serious side project)"
3. "20-40 hours/week (between jobs or part-time)"
4. "Full-time (40+ hours/week)"

**Follow-up based on answer**:
- If option 1: Ideas will be filtered to micro-SaaS, simple tools, and low-maintenance products
- If option 2: Standard side-project scope; most ideas are feasible
- If option 3 or 4: Full startup scope is feasible; can consider more ambitious ideas

**Adaptive behavior**:
- **Skip trigger**: Never skip -- time availability is critical for idea feasibility scoring
- **Probe deeper trigger**: If user selects option 1, ask about timeline expectations (see Q4.2)

#### Q4.2: Timeline Expectations

**Question**: "When do you want to have something launched?"

**Recommended Options**:
1. "Within 2-4 weeks (fast MVP)"
2. "Within 1-3 months (solid MVP)"
3. "Within 3-6 months (more polished product)"
4. "No specific timeline -- I'll work on it as I can"

**Follow-up based on answer**:
- If option 1: Emphasize extremely minimal MVPs; prioritize ideas that can launch as a landing page + waitlist or single-feature tool
- If option 4: Note flexibility; may suggest a phased approach

**Adaptive behavior**:
- **Skip trigger**: If user selected full-time in Q4.1, this question is less critical but still useful for setting expectations

---

### Category 5: Budget Constraints

**Purpose**: Understand financial constraints that affect technology choices, marketing approach, and idea feasibility.

#### Q5.1: Initial Investment Capacity

**Question**: "How much are you willing to invest upfront in tools, services, and infrastructure?"

**Recommended Options**:
1. "$0 -- I want to use only free tools and services"
2. "Up to $50/month for essential tools"
3. "Up to $200/month -- willing to pay for quality tools and some marketing"
4. "More than $200/month -- I can invest in growth"

**Follow-up based on answer**:
- If option 1: Ideas must be buildable with free tiers only; note this as a hard constraint
- If option 3 or 4: Ask if they have a total budget ceiling or a monthly comfort number

**Adaptive behavior**:
- **Skip trigger**: If user says "budget is flexible" or "not a concern," note as unconstrained and skip follow-ups
- **Probe deeper trigger**: If user selects $0 but has ambitious income goals, flag this as a potential contradiction (addressed in Round 4)

#### Q5.2: Willingness to Spend on Validation

**Question**: "Would you spend money to validate your idea before building (e.g., ads, landing page tools, market research)?"

**Recommended Options**:
1. "No -- I'll validate through free channels only"
2. "Yes, up to $100 for ads or landing page tests"
3. "Yes, up to $500 for thorough pre-launch validation"
4. "Skip this question"

**Follow-up based on answer**:
- If option 2 or 3: Note this as a positive signal for idea validation strategy in later stages
- If option 1: Plan validation around organic methods (community posts, direct outreach, free tools)

**Adaptive behavior**:
- **Skip trigger**: If user is in the "just exploring" phase, skip and revisit during validate-idea skill

---

### Category 6: Income Goals

**Purpose**: Calibrate idea scale and revenue model recommendations to match user expectations.

#### Q6.1: Revenue Target

**Question**: "What's your income goal from this project?"

**Recommended Options**:
1. "Side income: $500-$2,000/month"
2. "Significant income: $2,000-$10,000/month"
3. "Full replacement: $10,000+/month"
4. "I'm not focused on revenue yet -- I want to build something useful first"

**Follow-up based on answer**:
- If option 1: Focus on micro-SaaS, small tools, and niche products with simple pricing
- If option 2: Focus on SaaS with growth potential, marketplace products, or B2B tools
- If option 3: Focus on B2B SaaS, platform businesses, or products with clear scaling paths
- If option 4: Note this; weight "interest fit" higher than "revenue potential" in scoring

**Adaptive behavior**:
- **Skip trigger**: If user selected option 4, skip Q6.2
- **Probe deeper trigger**: If user selects option 3, ask Q6.2 about income type preference

#### Q6.2: Income Type Preference

**Question**: "What type of income stream do you prefer?"

**Recommended Options**:
1. "Recurring revenue (subscriptions, SaaS)"
2. "One-time sales (tools, templates, courses)"
3. "Usage-based or transactional (pay-per-use, marketplace cuts)"
4. "No preference -- whatever works best"

**Follow-up based on answer**:
- This directly informs the revenue model dimension of idea generation and scoring

**Adaptive behavior**:
- **Skip trigger**: If user chose option 4 in Q6.1, skip this question entirely

---

## Round 3: Risk and Network

### Category 7: Risk Tolerance

**Purpose**: Understand how comfortable the user is with uncertainty, competition, and pivoting, which affects idea selection strategy.

#### Q7.1: Comfort with Competition

**Question**: "How do you feel about entering a market with existing competitors?"

**Recommended Options**:
1. "I prefer markets with no competition (blue ocean)"
2. "Some competition is fine -- it validates demand"
3. "I'm comfortable competing -- I'll differentiate on execution"
4. "I actually prefer competitive markets -- more proven demand"

**Follow-up based on answer**:
- If option 1: Focus on emerging niches, underserved segments, and novel approaches; flag that "no competition" sometimes means "no demand"
- If option 3 or 4: Open up competitive but validated markets; focus on differentiation angles

**Adaptive behavior**:
- **Skip trigger**: If user seems disengaged or rushed, infer moderate risk tolerance (option 2) and skip
- **Probe deeper trigger**: If user selects option 1, ask whether they are open to underserved segments of existing markets (which is different from "no competitors at all")

#### Q7.2: Willingness to Pivot

**Question**: "If your initial idea doesn't gain traction, how would you respond?"

**Recommended Options**:
1. "I'd pivot quickly to something else"
2. "I'd iterate and adjust the approach"
3. "I'd stick with it longer -- I believe in persistence"
4. "Skip this question"

**Follow-up based on answer**:
- If option 1: Note high adaptability; suggest ideas where quick validation is possible
- If option 3: Note persistence preference; suggest ideas with longer validation cycles but higher potential payoff
- This primarily influences the execution plan, but also affects which ideas to prioritize (faster-to-validate vs deeper commitment)

**Adaptive behavior**:
- **Skip trigger**: This is a lower-priority question; if interview is running long, skip it

---

### Category 8: Network

**Purpose**: Identify existing connections that could accelerate market entry, provide beta users, or enable partnerships.

#### Q8.1: Professional Network

**Question**: "Do you have connections in any specific industry or market?"

**Recommended Options**:
1. "Yes -- I know people in [specific industry] who could be early users"
2. "I have a general tech/developer network"
3. "I have an online audience (blog, Twitter/X, YouTube, newsletter)"
4. "Not really -- I'd be starting from scratch"

**Follow-up based on answer**:
- If option 1: Ask which industry -- this is a strong signal for market selection. Having potential beta users is a major advantage.
- If option 3: Ask about audience size and engagement -- even a small engaged audience is valuable for launch
- If option 4: Note this; ideas should not depend on existing network for initial traction

**Adaptive behavior**:
- **Skip trigger**: If user seems uncomfortable discussing their network, accept option 4 gracefully and move on
- **Probe deeper trigger**: If user has industry connections, ask: "Would any of them pay for a solution to a problem they have?" This tests whether the network is just social or commercially relevant.

#### Q8.2: Community Presence

**Question**: "Are you active in any online communities related to your interests or industry?"

**Recommended Options**:
1. "Yes -- I'm active in relevant subreddits, Discord servers, or forums"
2. "I lurk but don't actively participate"
3. "No -- I'm not active in any communities"
4. "Skip this question"

**Follow-up based on answer**:
- If option 1: Ask which communities -- these become distribution channels and validation resources
- If option 2: Suggest becoming more active as a pre-launch strategy; communities are where early users live

**Adaptive behavior**:
- **Skip trigger**: This is supplementary to Q8.1; if the user already described a strong network, this can be skipped
- **Probe deeper trigger**: If user mentions specific communities with large memberships, note as potential distribution channels

---

## Round 4: Adaptive Follow-Ups (Conditional)

Round 4 is only triggered when the interview has identified gaps, contradictions, or areas needing additional depth. If none of these conditions are met, proceed directly to market research.

### Trigger: Skipped Categories

If the user skipped 2 or more categories, ask a consolidated catch-up question.

#### Q-SKIP: Profile Completion

**Question**: "To generate the best ideas for you, it would help to know a bit more. Which of these can you tell me about?"

**Recommended Options** (dynamically generated based on what was skipped):
1. "[First skipped category] -- brief description of what's needed"
2. "[Second skipped category] -- brief description of what's needed"
3. "I'd rather skip these and see what you come up with"
4. "Let me tell you about all of them"

**Adaptive behavior**:
- If option 3: Accept the gaps and proceed with what you have. Note in the profile that these dimensions are unknown and will receive neutral/average scores.
- If option 4: Walk through each skipped category with the Round 1/2/3 questions

---

### Trigger: Contradictory Answers

Common contradictions to detect and resolve:

#### Contradiction: No Budget + High Income Goals

**Question**: "You mentioned you want to invest $0 upfront but are targeting $10,000+/month in revenue. Building a high-revenue product typically requires some investment. Which of these fits your situation?"

**Recommended Options**:
1. "I can bootstrap with sweat equity -- I have the time to compensate for no budget"
2. "I could invest a small amount ($50-100/month) if the idea justifies it"
3. "My income goal is aspirational -- I'd be happy starting smaller"
4. "Let me reconsider my budget"

#### Contradiction: Low Time + Fast Timeline

**Question**: "You mentioned 5-10 hours/week but want to launch within 2-4 weeks. That's about 20-40 total hours. Would you like to adjust either your time commitment or timeline?"

**Recommended Options**:
1. "I can increase my time commitment for the initial push"
2. "I'm okay with a more minimal MVP that fits my time"
3. "Let me adjust my timeline to be more realistic"
4. "Keep both -- I work fast and want the constraint"

#### Contradiction: No Skills + Complex Ambitions

**Question**: "You mentioned you're early in your coding journey but are interested in ambitious projects. How would you like to approach this?"

**Recommended Options**:
1. "Suggest simpler ideas that match my current skill level"
2. "Suggest ideas where I can learn as I build"
3. "Suggest ideas that use no-code/low-code platforms"
4. "I'm more skilled than my answer suggested -- let me clarify"

---

### Trigger: Insufficient Data for Personalization

If the profile lacks enough data to differentiate ideas from generic suggestions, ask a consolidation question.

#### Q-INSUF: Differentiation Anchor

**Question**: "To make sure your ideas are personalized and not generic, what's the one thing that makes you different from other developers looking for side project ideas?"

**Recommended Options**:
1. "My unique domain knowledge in [specific area]"
2. "My access to a specific audience or customer base"
3. "A specific technical skill that's uncommon"
4. "Honestly, I'm not sure -- help me figure that out"

**Follow-up based on answer**:
- If option 1, 2, or 3: Use this as the primary personalization anchor for idea generation
- If option 4: Generate ideas using a broader exploration approach, focusing on skill-adjacent markets and currently-trending opportunities; flag to the user that ideas may be less personalized

---

## Adaptive Behavior Guidelines

### General Principles

1. **Respect user pace**: If a user gives short answers and selects "skip" frequently, compress remaining questions rather than pushing for detail
2. **Never ask more than 3 questions per round**: Group related questions and present the most important one; use follow-ups only when the answer warrants depth
3. **Infer when possible**: If a user says "I'm a senior React developer at a fintech company," you can infer: frontend skills (React), professional experience, finance domain knowledge -- do not re-ask these
4. **Track engagement level**: Users who give detailed free-form answers are engaged; probe deeper. Users who pick the shortest option repeatedly are rushed; move faster.

### When User Has No Clear Skills or Interests

If the user indicates they are early in their journey with no clear technical focus or domain expertise:

1. **Do not dismiss them** -- many successful products are built by people learning as they go
2. **Shift to Q1.1-ALT** to assess their general experience level
3. **Emphasize Q3.1 and Q3.2** (interests and pain points) -- passion and personal pain points matter more than technical depth for idea selection
4. **Suggest skill-adjacent markets**: Markets where their current skills (even non-technical ones) provide an edge
5. **Recommend simpler build approaches**: No-code tools, template-based businesses, content products, community-based products
6. **Adjust scoring weights**: Increase weight on "Solo Buildability" and decrease weight on "Technical Complexity"

### When User Wants to Skip Categories

1. **Essential categories** (never skip): Technical Skills (Q1.1), Available Time (Q4.1)
2. **Important categories** (encourage but allow skip): Domain Experience, Income Goals
3. **Optional categories** (skip freely): Budget details (Q5.2), Risk Tolerance (Q7.2), Community Presence (Q8.2)
4. When a category is skipped, use neutral/default assumptions:
   - Budget skipped: Assume moderate ($50-100/month)
   - Risk tolerance skipped: Assume moderate (option 2)
   - Network skipped: Assume starting from scratch (option 4)
   - Income goals skipped: Assume side income ($500-2,000/month)

### Handling Contradictory Answers

1. **Do not immediately challenge the user** -- wait until Round 4 to address contradictions
2. **Frame contradictions as clarifications**, not corrections: "I want to make sure I understand your situation correctly"
3. **Common contradictions to detect**:
   - No budget + high income goals
   - Low time + fast timeline
   - No skills + complex interests
   - Risk averse + wanting to enter competitive markets
4. **Resolution approach**: Present the contradiction gently with options that let the user self-correct without feeling judged

### Handling Insufficient or Vague Answers

1. **One follow-up attempt**: If an answer is too vague, ask one clarifying follow-up
2. **Then accept and move on**: If the follow-up is also vague, accept the data and move on
3. **Note gaps in the profile**: Clearly mark dimensions where data is insufficient so that idea generation can account for uncertainty
4. **Use broader exploration**: When profile data is thin, generate a wider range of ideas across more markets rather than deeply personalized ideas in few markets

---

## Question Constraints for AskUserQuestion

All questions in this bank are designed to work within AskUserQuestion constraints:

- **2-4 answer options per question**: Every question provides 2-4 selectable options
- **Options are concise**: Each option is a short phrase or single sentence
- **One "escape hatch" option**: Most questions include a "Skip," "Other," or "Let me describe" option for users who don't fit the predefined choices
- **No open-ended questions without options**: Even exploratory questions provide starting-point options that the user can select or build upon

---

## Profile Output Structure

After the interview completes, the gathered data should be organized into this internal profile structure for use by subsequent idea generation steps:

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

This profile feeds directly into market identification and idea scoring.
