# Risk Assessment Template

Use this template to evaluate a specific risk identified during any phase of the Converge cycle. Each risk must have its own assessment. A list of risks sharing a single assessment is not an assessment — it is a list.

---

## Risk Identification

**Risk ID:** [Unique identifier, e.g., RISK-001]

**Risk title:** [One sentence that states the risk, not the cause. "The authentication service may become a single point of failure" is a risk. "We are using a single authentication service" is a cause.]

**Risk category:** [Security / Performance / Availability / Data integrity / Compliance / Operational / Architectural / Organizational]

**Identified by:** [Named individual or council role]

**Date identified:** [date]

## Risk Description

Describe the risk in concrete, specific terms. State the condition under which the risk materializes, who is affected, and what the observable failure mode looks like. Avoid generic descriptions.

_Example of insufficient description: "There is a risk of data loss."_

_Example of sufficient description: "If the background job queue reaches maximum depth during a traffic spike, new write operations will be rejected without notification to the user, causing unacknowledged data loss for operations submitted during the saturation window."_

## Likelihood Assessment

**Rating:** Low / Medium / High / Critical

**Basis:** State the evidence, precedent, or reasoning used to derive this rating. A rating without a basis is an assumption, not an assessment.

_Likelihood criteria:_
- _Low: No historical precedent, conditions are difficult to produce, no known attack vector._
- _Medium: Has occurred in similar systems, conditions can arise during normal operations._
- _High: Has occurred in this system or close analogues, conditions arise regularly._
- _Critical: Actively exploited, or conditions are inherent to the design._

## Impact Assessment

**Rating:** Low / Medium / High / Critical

**Basis:** State the consequence and who bears it. Include the scope (single user, user cohort, entire system), the recovery cost, and any compliance or contractual implication.

_Impact criteria:_
- _Low: Minor user friction, recoverable within minutes, no data loss, no compliance implication._
- _Medium: Service degradation for a user cohort, recovery within hours, possible data inconsistency._
- _High: Service unavailability for a significant user base, recovery within hours to days, data loss or breach risk._
- _Critical: Regulatory breach, unrecoverable data loss, reputational damage, or financial loss above defined threshold._

## Risk Score

**Score:** [Likelihood × Impact using: Low=1, Medium=2, High=3, Critical=4]

_Score interpretation: 1–4 Monitor / 5–8 Mitigate / 9–16 Block_

## Mitigation Plan

Describe the specific actions that reduce likelihood, impact, or both. For each action, state the owner, the deadline, and how the action will be verified as complete.

| Action | Effect | Owner | Deadline | Verification |
|---|---|---|---|---|
| | Reduces likelihood / Reduces impact / Both | | | |

## Residual Risk

**Residual rating:** [Recalculate after mitigation]

**Residual description:** Describe what risk remains after all mitigations are applied. State explicitly whether the residual risk is accepted, further mitigated in a future cycle, or blocking.

**Accepted by:** [Named individual if accepted]

**Monitoring mechanism:** [How this risk will be monitored in production]

