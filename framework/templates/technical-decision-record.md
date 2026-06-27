# Technical Decision Record Template

A Technical Decision Record (TDR) captures a significant engineering decision in a form that remains useful after the context that produced it is gone. A decision that cannot be reconstructed from its record is a decision that will be made again, often with worse outcomes.

---

## Record Metadata

**TDR-ID:** [Sequential identifier, e.g., TDR-042]

**Title:** [One sentence naming the decision. Must identify what was decided, not what was discussed.]

**Status:** Proposed / Accepted / Superseded / Deprecated

**Decision date:** [date]

**Decision owner:** [Named individual accountable for this decision and its revision]

**Supersedes:** [TDR-ID if this record replaces a prior decision]

**Superseded by:** [TDR-ID if a later decision replaces this one]

---

## Decision Statement

State the decision in one or two sentences. The statement must be specific enough that someone reading it without prior context would understand exactly what was decided, in which system or component, and to what effect.

_Insufficient: "We decided to use Redis."_

_Sufficient: "The session store for the authentication service will use Redis with TTL-based expiration, deployed as a managed cluster, with no fallback to the database for session reads."_

---

## Context

Describe the situation that made this decision necessary. Include:

- The system or component affected.
- The problem or opportunity that triggered the decision.
- The constraints that shaped the option space (technical, organizational, regulatory, time).
- The relevant history: prior decisions, prior failures, prior investigations that led here.

_Guidance: Write this section for a reader who joins the team two years from now. They will not know what was obvious at the time._

---

## Options Considered

For each option that was seriously evaluated, provide enough detail that a reader can understand why it was not chosen.

### Option [N]: [Name]

**Summary:** What this option does.

**Rationale for consideration:** Why this was a legitimate candidate.

**Why it was not chosen:** The specific reason this option lost to the chosen alternative. This must be the actual reason, not a post-hoc justification.

---

## Consequences

### Positive consequences

Describe what becomes easier, cheaper, more reliable, or more maintainable as a result of this decision.

### Negative consequences

Describe what becomes harder, more expensive, or introduces new risk. A decision with no negative consequences was not evaluated carefully enough.

### Accepted tradeoffs

State explicitly what is being given up, and confirm that the tradeoff is intentional and understood.

---

## Confidence Level

**Level:** Low / Medium / High

**Basis:** State the evidence quality, assumption weight, and council agreement level that produce this confidence rating. Do not state "High" without a basis.

---

## Reversal Conditions

State the specific, observable conditions that would require this decision to be revisited. Each condition must be monitorable.

| Reversal condition | Monitoring mechanism | Owner |
|---|---|---|
| | | |

_A decision without reversal conditions is a decision that cannot be maintained. It will eventually be abandoned rather than revised._

---

## Active Assumptions

List the claims this decision depends on that are not yet fully evidenced.

| Assumption | Risk if false | Validation plan |
|---|---|---|
| | | |

