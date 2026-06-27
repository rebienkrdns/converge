# Decision Matrix Template

The Decision Matrix is the canonical output of the Converge cycle. It synthesizes the outputs of all engines into a single structured artifact that communicates the quality, confidence, and direction of a decision to any stakeholder. Complete this template after Converge and before Decide.

---

## Decision Reference

**Decision title:** [From the TDR or decision record]

**Cycle date:** [date the matrix is produced]

**Council composition:** [List of specialist roles that participated]

---

## Quality Score

**Score:** [1–10]

**Criteria used:**

The Quality Score reflects the structural integrity of the decision: how well it fits the problem, how coherent it is across specialist domains, and how well the selected option satisfies the hard constraints identified in Understand.

| Criterion | Rating (1–10) | Weight | Weighted score |
|---|---|---|---|
| Problem–solution fit | | | |
| Constraint satisfaction | | | |
| Architectural coherence | | | |
| Security posture | | | |
| Operational sustainability | | | |
| Tradeoff explicitness | | | |

**Composite Quality Score:** [Weighted average]

**Interpretation:**
- 9–10: Decision is well-formed and ready for Decide without reservations.
- 7–8: Decision is sound with minor gaps that should be noted in the record.
- 5–6: Decision is acceptable under constraint but carries meaningful quality risk.
- Below 5: Decision requires revision before proceeding.

---

## Confidence

**Level:** Low / Medium / High

**Basis:**

Confidence reflects the epistemic strength of the decision, not agreement about the choice. A council may unanimously prefer an option while holding Low confidence because the evidence base is thin.

| Confidence factor | Assessment |
|---|---|
| Evidence quality | Strong / Partial / Weak |
| Evidence coverage | Complete / Gaps acknowledged / Gaps unknown |
| Assumption weight | Low / Medium / High |
| Specialist disagreement | None / Minor / Significant |
| Prior precedent | Established / Analogous / Novel |

**Confidence summary:** [One paragraph explaining why the confidence level was assigned, referencing the factors above]

---

## Evidence

**Sufficiency:** Sufficient / Conditional / Insufficient

| Evidence item | Type | Quality | Relevance | Gap |
|---|---|---|---|---|
| | Primary / Secondary | High / Medium / Low | Direct / Supporting / Contextual | None / Known / Critical |

**Evidence gaps:** [List any gaps and their impact on the decision confidence]

**Sufficiency rationale:** [Explain why evidence is or is not sufficient to support proceeding]

---

## Impact

**Overall impact rating:** Low / Medium / High / Critical

Assess impact across each dimension:

| Impact dimension | Rating | Description |
|---|---|---|
| User experience | Low / Medium / High | |
| System reliability | Low / Medium / High | |
| Security posture | Low / Medium / High | |
| Operational load | Low / Medium / High | |
| Development velocity | Positive / Neutral / Negative | |
| Reversibility | Easy / Moderate / Difficult | |
| Dependency surface | Unchanged / Expanded / Reduced | |

**Impact summary:** [One paragraph describing the primary impact and the most significant secondary effect]

---

## Recommendation

**Recommendation:** Approve / Approve with conditions / Reject / Return to investigation

**Recommended option:** [Name of the selected option from Debate]

**Primary rationale:** [The strongest single reason this option wins given the constraints, evidence, and impact profile]

**Conditions (if conditional approval):**

| Condition | Owner | Deadline |
|---|---|---|
| | | |

**Dissent:** [Record any council member who disagrees with the recommendation, with their reasoning]

---

## Executive Summary

_Write this section last. It must be self-contained and readable without the rest of the matrix._

**In one paragraph:** What was decided, why this option was selected, what the primary tradeoff is, and what the team should watch for after implementation.

**Key risks accepted:** [Bulleted list, maximum three items]

**Next verification point:** [Date or observable event at which the outcome will be evaluated]

