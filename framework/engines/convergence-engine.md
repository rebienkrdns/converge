# Convergence Engine

## Purpose

Reduce the candidate option set to the smallest set of viable paths, resolve duplicate arguments, surface contradictions that Debate left open, and produce either a preferred direction or a clearly bounded unresolved conflict for the council to address.

## Inputs

- Candidate option list from the Debate phase.
- Per-specialist evaluations from the Debate phase.
- Evidence inventory and contradiction log from the Evidence Engine.
- Impact ratings from the Impact Engine.
- Confidence rating and rationale from the Confidence Engine.
- Self-critique output from the Critique system.

## Outputs

- Reduced option set: options that survive elimination.
- Eliminated options: each option that was removed, with the explicit elimination reason.
- Dominant tradeoffs: the most material tradeoff the decision creates, stated in terms of what is gained and what is given up.
- Preferred direction: the option the council considers strongest, with the full chain of reasoning.
- Unresolved conflicts: disagreements that cannot be eliminated by reasoning alone and must be resolved by explicit council decision.

## Conceptual Algorithm

1. **Eliminate dominated options**: an option is dominated if another option in the set satisfies every evaluation criterion at least as well and at least one criterion strictly better. Remove dominated options first.

2. **Eliminate constraint violations**: an option that fails a hard constraint identified in the Understand phase is eliminated regardless of its scores on other dimensions.

3. **Consolidate duplicate arguments**: multiple specialist evaluations may make the same point through different lenses. Identify the unique arguments across all evaluations and group duplicates. The consolidated argument list is shorter and easier to weigh.

4. **Surface contradictions**: identify evaluations where two specialists hold opposing positions supported by evidence on both sides. These are genuine disagreements, not errors, and they cannot be resolved by the engine alone. Surface them explicitly as unresolved conflicts.

5. **Apply impact weighting**: among the remaining non-dominated, non-violating options, weight the evaluation dimensions by their impact on the success criteria from Understand. Dimensions that are most material to success receive more weight than dimensions that are important but not decisive.

6. **Identify the preferred direction**: the option with the best weighted tradeoff profile given the evidence, constraints, and impact analysis. If no option is clearly preferred — because two options are close on all weighted dimensions — state this explicitly as a genuine tie that requires a council decision rather than a framework decision.

## Responsibilities

- Synthesize debate into convergence without suppressing legitimate disagreement.
- Eliminate options based on evidence and criteria, not on the authority or volume of the advocate.
- Preserve the full set of eliminated options with their elimination reason, so the decision is reconstructible.

## Interaction

Consumes outputs from the Evidence Engine, Impact Engine, Confidence Engine, and Critique system. Produces the input to the Decision Matrix Engine, which scores the preferred direction across the canonical dimensions. Unresolved conflicts are passed to the council as explicit decision points before Decide begins.

