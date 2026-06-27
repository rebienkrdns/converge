# Converge

## Purpose

Reduce the candidate space to the strongest single direction. This phase eliminates ambiguity that Debate left open, resolves conflicts between specialist positions, and produces the final option the council will commit to in Decide. Converge is not consensus by exhaustion. It is resolution by reasoning.

## Entry Condition

Debate has produced a preliminary preferred option, a full set of evaluated candidates, and recorded disagreements. The evaluation criteria are agreed upon.

## Process

1. Review the candidates remaining after Debate and identify whether any can be eliminated cleanly: options that fail hard constraints, options that are strictly dominated by another candidate, or options whose evidence base collapsed during Challenge.
2. Examine unresolved disagreements. For each, determine whether the disagreement is about values (which must be resolved by explicit priority decision), about facts (which must be resolved by evidence or acknowledged as a gap), or about predictions (which must be resolved by identifying which prediction carries more risk if wrong).
3. Apply the convergence test: given the evaluation criteria, the evidence record, and the hard constraints, which option produces the best expected outcome while carrying the smallest unmitigated tail risk.
4. Validate that the selected direction is internally consistent: the architecture holds under the constraints, the security model is compatible with the performance requirements, the observability plan is compatible with the delivery timeline.
5. Produce the final preferred option with a stated rationale, the alternatives considered, and why each alternative was not chosen.

## Outputs

- Final preferred option with complete rationale.
- Eliminated options with the reason for elimination.
- Resolved disagreements with the reasoning used to resolve them.
- Unresolved disagreements that will carry into Decide as recorded dissent.
- Internal consistency check result.

## Exit Condition

The council has a single preferred direction. That direction is internally consistent and has survived the convergence test. No council member holds an unvoiced reservation about the direction itself (though they may disagree with the choice).

## Failure Signals

- The phase ends with two "preferred" options because the council could not eliminate one.
- Convergence is achieved by removing a council member from the discussion rather than addressing their objection.
- The selected option was never a candidate during Debate and appears now without explanation.
- Internal consistency between specialist domains was not checked.
- Disagreements were marked resolved but the underlying tension was not addressed.

## Interaction with Subsequent Phases

The output of Converge is the direct input to Decide. The selected option becomes the decision. The recorded dissent becomes part of the decision record. The eliminated alternatives feed into the self-critique and into future Reflect phases.

