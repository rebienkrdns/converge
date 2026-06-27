# Gate Engine

## Purpose

Determine which gates apply to a given decision, evaluate each applicable gate against the decision record and evidence, and produce a final gate status that either authorizes progression to Deliver or blocks it with actionable remediation requirements.

## Inputs

- Decision record from the Decide phase.
- Evidence inventory from the Evidence Engine.
- Impact rating from the Impact Engine (determines which gates are mandatory).
- Decision Matrix output (Quality Score and Confidence inform gate sensitivity).
- Specialist evaluations from the Debate phase (domain-specific gate evidence).

## Outputs

For each evaluated gate:
- **Status**: Pass / Conditional Pass / Block.
- **Finding**: the specific condition that produced the status, referenced to the decision record.
- **Required remediation** (for Conditional Pass or Block): the specific action, owner, and deadline that resolves the gate finding.

Final aggregated gate status: Pass (all gates pass or conditional), Block (at least one gate blocked with no resolution path).

## Conceptual Algorithm

1. **Gate selection**: not all gates apply to every decision. Apply the following selection rules:
   - Security Gate: mandatory for any decision that changes authentication, authorization, data handling, secrets management, or the external attack surface.
   - Performance Gate: mandatory for any decision that changes the critical path for user-facing latency, background throughput, or resource consumption.
   - Architecture Gate: mandatory for any decision that changes component boundaries, ownership models, or inter-component communication patterns.
   - Testing Gate: mandatory for any decision that introduces new behavior that is not covered by existing tests.
   - Observability Gate: mandatory for any decision deployed to production that introduces new failure modes.
   - Documentation Gate: mandatory for any decision that changes the operational model of a system that others depend on.
   - Maintainability Gate: mandatory for any decision with a Quality Score below 7 or a long expected operational lifespan.

2. **Gate evaluation**: for each selected gate, compare the decision record and evidence against the gate's approval conditions and blocking conditions. A gate passes when all approval conditions are met. A gate blocks when any blocking condition is unmet and the risk is material. A conditional pass is issued when a blocking condition exists but is resolvable before delivery, with a named owner and deadline.

3. **Block escalation**: a block does not terminate the decision cycle. It returns the cycle to the appropriate earlier phase — typically Investigate if evidence is missing, or Debate if a design element must change. The block finding must state which phase to return to and why.

4. **Conditional pass management**: conditions from conditional passes are transferred to the delivery checklist. No condition may carry forward past the deployment window without being resolved or explicitly accepted with a named risk owner.

## Responsibilities

- Apply gates proportionally to impact: a high-impact decision receives all applicable gates; a low-impact, easily reversible decision may need only one or two.
- Keep every block reason actionable: a block that says "security concerns exist" is not useful. A block that says "no threat model documents the handling of the OAuth token in transit to the third-party service" gives the team a clear next step.
- Never pass a gate by removing the gate's requirement. If the requirement cannot be met, the decision must change.

## Interaction

Consumes the Decision Matrix Engine, Evidence Engine, and Impact Engine. Emits gate status to the Recommendation Engine (gate blocks constrain the recommendation) and to the Executive Summary Engine (gate status appears in the decision record).

