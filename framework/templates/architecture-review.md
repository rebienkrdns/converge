# Architecture Review Template

Use this template when evaluating a structural change to a system: introducing a new component, removing a boundary, changing a communication pattern, or altering the data ownership model. Fill every section before presenting to the council.

---

## Problem Statement

Describe the architectural problem in one paragraph. Include the system area affected, the constraint being violated or opportunity being addressed, and the consequence of not acting.

_Guidance: Avoid describing the solution here. The problem statement must be readable by someone who disagrees with your preferred direction._

## System Boundary

Identify the components, services, data stores, and external dependencies within scope. List explicitly what is out of scope and why.

_Guidance: Draw the boundary around the decision, not around the familiar. If a component is affected by the change but excluded from scope, state the risk of that exclusion._

## Hard Constraints

List the constraints that cannot be relaxed. For each constraint, state its source (regulatory, contractual, SLA, organizational policy) and the cost of violating it.

| Constraint | Source | Cost of Violation |
|---|---|---|
| | | |

## Options Considered

For each realistic option, provide:

### Option A: [Name]

**Description:** What this option does structurally.

**Tradeoffs gained:** What becomes easier or cheaper.

**Tradeoffs given up:** What becomes harder, more expensive, or more risky.

**Specialist concerns:**
- Architecture: [cohesion, coupling, evolution cost]
- Security: [trust boundary changes, attack surface changes]
- Performance: [latency, throughput, resource implications]
- Maintainability: [operational overhead, team cognitive load]
- Observability: [what becomes visible or invisible]

### Option B: [Name]

_(Repeat the structure above for each option)_

## Evaluation Matrix

| Criterion | Option A | Option B | Option N |
|---|---|---|---|
| Boundary clarity | | | |
| Coupling introduced | | | |
| Change cost over 12 months | | | |
| Operational complexity | | | |
| Security surface change | | | |
| Reversibility | | | |

_Guidance: Score each cell with a qualitative rating (Low / Medium / High) and a one-line reason. Do not leave cells blank._

## Recommendation

State the recommended option and the primary reason it wins against the alternatives given the constraints. Include the most important tradeoff being accepted.

## Reversal Conditions

State the specific observations or events that would require this architectural decision to be revisited. A reversal condition must be observable and must be assigned to a monitoring owner.

| Condition | Monitoring mechanism | Owner |
|---|---|---|
| | | |

## Open Risks

List the risks that are accepted as part of this decision, the mitigation approach for each, and the residual exposure.

| Risk | Mitigation | Residual |
|---|---|---|
| | | |

