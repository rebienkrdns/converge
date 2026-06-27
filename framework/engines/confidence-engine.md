# Confidence Engine

## Purpose

Produce a calibrated confidence rating for a decision that reflects the quality of the evidence, the weight of active assumptions, and the degree of specialist disagreement — not the conviction of the proposer.

## Inputs

- Evidence quality classification from the Evidence Engine (Primary / Secondary / Tertiary per dimension).
- Gap map from the Evidence Engine.
- Assumption list with weight labels from the Investigate phase.
- Specialist disagreement record from the Debate phase.
- Contradiction log from the Evidence Engine.

## Outputs

- Confidence level: Low / Medium / High.
- Confidence rationale: the specific factors that produced the rating.
- Confidence ceiling: the highest possible rating given the current evidence quality.
- Confidence risks: the specific changes that would lower confidence if they occurred.

## Conceptual Algorithm

1. Establish the evidence ceiling:
   - If the root question has at least one Primary evidence item for every major dimension → ceiling is High.
   - If one or more dimensions are covered only by Secondary evidence → ceiling is Medium.
   - If any dimension is covered only by Tertiary evidence or a gap → ceiling is Low regardless of other dimensions.

2. Apply assumption penalty: for each high-weight assumption (one whose falsity would reverse the preferred direction), lower the confidence by one level from the ceiling. A decision with two or more high-weight unvalidated assumptions cannot exceed Low.

3. Apply disagreement factor: unresolved specialist disagreement — where two or more council members hold opposing assessments with evidence on both sides — lowers confidence by one level, to a minimum of Low.

4. Set the final rating as the minimum of the evidence ceiling after penalties.

5. Produce the confidence rationale by naming the specific factors that constrained the rating. A rating of "Medium because the performance evidence is from a staging environment that is not representative of production load" is more useful than "Medium because evidence is incomplete."

## Responsibilities

- Separate conviction from proof: a council that unanimously agrees on a direction may still have Low confidence if the evidence base is weak.
- Penalize hidden uncertainty more than acknowledged uncertainty: a known gap that is documented is less dangerous than an assumption that is not identified.
- Produce a confidence ceiling separately from the final rating, so that the path to higher confidence is clear.

## Interaction

Feeds the Decision Matrix Engine (confidence is a matrix dimension). Feeds the Self Critique system (low confidence triggers deeper assumption review). The confidence ceiling informs the Recommendation Engine about the conditions under which a conditional approval is appropriate versus a full approval.

