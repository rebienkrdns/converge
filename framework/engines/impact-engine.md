# Impact Engine

## Purpose

Estimate the operational, product, and organizational consequences of implementing the decision, across multiple time horizons and stakeholder groups, so that the Decision Matrix reflects real-world tradeoffs rather than theoretical ones.

## Inputs

- Success criteria and stakeholder map from the Understand phase.
- Evidence inventory from the Evidence Engine.
- Specialist evaluations from the Debate phase.
- Identified risks from the Challenge phase.

## Outputs

- Impact rating: Low / Medium / High / Critical per dimension.
- Positive consequence summary: what improves and for whom.
- Negative consequence summary: what degrades or becomes more expensive.
- Blast radius: the scope of affected systems, teams, or users.
- Second-order effects: consequences that are not immediate but are credible within the decision's lifespan.

## Conceptual Algorithm

1. Evaluate impact across four dimensions separately:

   - **User impact**: how does this decision affect the end user's experience, reliability, or access to the system?
   - **Technical blast radius**: how many components, services, or teams are affected by this change? What is the coupling surface?
   - **Delivery and change cost**: what does it cost in time, effort, and risk to implement this decision? What does it cost to reverse it?
   - **Operational burden**: how does this decision affect the ongoing operational load — monitoring, incident response, deployment complexity, dependency management?

2. For each dimension, assign a rating using the scale:
   - **Low**: contained effect, reversible within hours, affects a small number of users or components.
   - **Medium**: moderate scope, reversible within days, affects a user cohort or a set of related services.
   - **High**: broad scope, reversible within weeks, affects a major user population or cross-team systems.
   - **Critical**: systemic scope, difficult or impossible to reverse, affects the entire user base or the organization's ability to operate.

3. Identify second-order effects: consequences that do not materialize immediately but become relevant as the system scales, the team changes, or the market shifts. State them explicitly rather than omitting them because they are uncertain.

4. Derive the overall Impact rating as the highest rating across any single dimension. A decision with Low user impact but Critical operational burden carries a Critical overall impact rating.

## Responsibilities

- Quantify where the evidence supports it: "latency increases by approximately 40ms at the p95 based on staging benchmarks" is more useful than "performance may degrade."
- Describe second-order effects even when they are speculative: label them as speculative, but do not omit them.
- Prevent hidden cost from being overlooked: implementation cost, reversal cost, and operational burden are all forms of impact that must appear in the evaluation.

## Interaction

Feeds the Decision Matrix Engine (impact rating is a matrix dimension). Feeds the Gate Engine (high-impact decisions trigger a wider set of gates). Second-order effects feed the Self Critique system's Future Risks section.

