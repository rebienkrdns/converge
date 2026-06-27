# Decision Matrix Engine

## Purpose

Produce the canonical evaluation of the preferred direction across six fixed dimensions, making explicit the quality, confidence, evidence, impact, recommendation, and executive summary of the decision in a single structured artifact.

## Inputs

- Preferred direction from the Convergence Engine.
- Evidence quality and gap map from the Evidence Engine.
- Confidence level and rationale from the Confidence Engine.
- Impact ratings from the Impact Engine.
- Specialist evaluations from the Debate phase.
- Hard constraints and success criteria from the Understand phase.

## Outputs

The six canonical Decision Matrix dimensions:

1. **Quality Score** (1–10): the structural integrity of the decision given the criteria.
2. **Confidence** (Low / Medium / High): the epistemic strength of the conclusion.
3. **Evidence** (Sufficient / Conditional / Insufficient): the adequacy of the supporting evidence.
4. **Impact** (Low / Medium / High / Critical): the scope and consequence of implementation.
5. **Recommendation** (Approve / Approve with conditions / Reject / Return to investigation).
6. **Executive Summary**: a self-contained paragraph readable without the full matrix.

## Conceptual Algorithm

**Quality Score derivation:**
Score each of the following sub-criteria on a 1–10 scale, then compute a weighted average:
- Problem–solution fit (weight 2): does the preferred option directly address the root question from Understand?
- Constraint satisfaction (weight 3): does the preferred option satisfy all hard constraints, or are there known violations?
- Specialist coherence (weight 2): is the preferred option internally consistent across all specialist domains?
- Tradeoff explicitness (weight 1): are the tradeoffs of the preferred option fully described?
- Reversibility (weight 2): can the decision be reversed if a reversal condition is met?

**Confidence assignment:** pass through from the Confidence Engine. Do not override it with council sentiment.

**Evidence rating derivation:**
- Sufficient: Primary or Secondary evidence exists for every major dimension of the root question.
- Conditional: at least one dimension has only Tertiary evidence, but the gap is acknowledged and does not change the recommendation.
- Insufficient: one or more dimensions have no evidence, or the gap is material enough to change the recommendation.

**Impact assignment:** pass through from the Impact Engine. The highest single-dimension rating governs.

**Recommendation derivation:**
- Quality Score ≥ 7 AND Confidence ≥ Medium AND Evidence ≥ Conditional AND no gate block → Approve.
- Quality Score ≥ 5 AND specific resolvable conditions exist → Approve with conditions (conditions must be named).
- Quality Score < 5 OR a gate block with no resolution path → Reject.
- Evidence = Insufficient with a recoverable gap → Return to investigation.

**Executive Summary composition:** one paragraph stating the decision, the primary justification, the most significant accepted tradeoff, and the next verification point.

## Responsibilities

- Score from evidence and criteria, not from the strength of the proposer's argument.
- Never assign a recommendation that is inconsistent with the Confidence or Evidence rating.
- Keep the Executive Summary self-contained — a reader who reads only the summary must understand the decision.

## Interaction

Feeds the Gate Engine (matrix scores determine which gates are triggered), the Recommendation Engine (scores produce the structured recommendation), and the Executive Summary Engine (matrix output is compressed into the final record).

