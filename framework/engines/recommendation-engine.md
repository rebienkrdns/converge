# Recommendation Engine

## Purpose

Derive a specific, actionable recommendation from the Decision Matrix output and gate status, ensuring that the recommendation type is consistent with the evidence quality and confidence level, and that conditional approvals are expressed with named conditions rather than as vague caveats.

## Inputs

- Decision Matrix output: Quality Score, Confidence, Evidence rating, Impact rating.
- Gate status from the Gate Engine: Pass / Conditional Pass / Block per gate.
- Specialist consensus or dissent record from the Debate phase.
- Reversal conditions from the Decide phase.

## Outputs

- **Recommendation type**: Approve / Approve with conditions / Reject / Return to investigation.
- **Rationale**: the primary reason the recommendation type was selected, tied to matrix dimensions and gate findings.
- **Conditions** (for conditional approval): each condition named with an owner and a deadline.
- **Dissent record**: any specialist who disagrees with the recommendation, with their reasoning.
- **Escalation note** (for Reject or Return): the specific gap or violation that must be resolved, and the phase to return to.

## Conceptual Algorithm

The recommendation type is derived deterministically from the matrix and gate outputs:

| Condition | Recommendation |
|---|---|
| All gates pass AND Quality Score ≥ 7 AND Confidence ≥ Medium | Approve |
| All gates pass or conditional AND Quality Score ≥ 5 AND named conditions exist | Approve with conditions |
| Evidence = Insufficient AND gap is recoverable | Return to investigation |
| Quality Score < 5 | Reject |
| Any gate blocked with no resolution path | Reject |
| Confidence = Low AND impact = High or Critical | Approve with conditions (at minimum) |

No recommendation may be upgraded above what the matrix and gate outputs support. A unanimous council that wants to approve a decision with a Quality Score of 4 must raise the quality score — they cannot override the engine.

**Conditions specification**: each condition in a conditional approval must name:
1. The specific action required.
2. The person responsible for completing it.
3. The deadline by which it must be complete.
4. The verification method that confirms it is done.

A condition that cannot be specified at this level of detail is not a valid condition — it is an unresolved concern that should lower the recommendation type.

## Responsibilities

- Keep the recommendation specific: "Approve, pending observability plan for the new event stream, owned by the SRE team, due before the deployment window" is a valid recommendation. "Approve, but make sure observability is handled" is not.
- Preserve conditional approval when conditions are genuine and resolvable — do not force a binary Approve/Reject when a conditional path exists.
- Record dissent: a specialist who disagrees with the recommendation must appear in the dissent record with their reasoning. Suppressing dissent is not consensus.

## Interaction

Consumes the Decision Matrix Engine (for scores) and the Gate Engine (for gate status). Feeds the Executive Summary Engine (the recommendation and conditions appear in the final record). Feeds the Decide phase (the recommendation becomes the decision or the basis for returning to an earlier phase).

