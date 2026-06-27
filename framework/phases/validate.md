# Validate

## Purpose

Check the decision against structural gates, implementation constraints, and observable evidence before authorizing delivery. Validation exists because a logically coherent decision can still fail operationally. This phase is the last structured checkpoint before the cycle commits to implementation.

## Entry Condition

The decision record from Decide is complete. Gates applicable to the decision domain have been identified.

## Process

1. Run each applicable gate against the decision record. A gate evaluates a structural concern — security, performance, testing, observability, architecture, documentation, or maintainability — and produces either a pass, a conditional pass, or a block.
2. For each gate that produces a conditional pass, identify the specific condition that must be met and the owner of that condition.
3. For each gate that produces a block, determine whether the block is resolvable within the current cycle by revising the decision, or whether the block requires returning to Debate or Investigate.
4. Validate that the decision is implementable: the required skills, tools, time, and dependencies are available or planned for.
5. Validate that the success criteria from Understand can be measured after implementation: the instrumentation exists or is part of the delivery plan.
6. Validate that the reversal conditions from Decide are monitorable: if a reversal condition were met after delivery, the team would know.

## Outputs

- Gate results: pass, conditional pass, or block for each gate, with the reasoning.
- List of conditions that must be satisfied before delivery for conditional passes.
- Implementation feasibility assessment.
- Observability validation: confirmation that success criteria will be measurable.
- Reversal condition monitorability check.

## Exit Condition

All gates have passed or have documented conditional passes with named owners and deadlines. No gate is blocking. The decision is confirmed implementable and observable.

## Failure Signals

- Gates are applied mechanically without reading the decision.
- A gate block is resolved by removing the gate rather than addressing its concern.
- Implementation feasibility is not checked because "the team will figure it out."
- Success criteria from Understand cannot be measured after delivery.
- Reversal conditions exist in the decision record but no monitoring plan exists for them.

## Interaction with Subsequent Phases

A clean Validate result authorizes progression to Deliver. A blocked Validate result returns the cycle to the appropriate earlier phase. Validate findings that are not decision-blocking but reveal systemic concerns become input to Reflect.

