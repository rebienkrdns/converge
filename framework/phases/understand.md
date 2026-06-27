# Understand

## Purpose

Translate the raw observation into a structured problem definition that the council can reason about. This phase converts symptoms into a system model and converts stakeholder language into engineering language.

## Entry Condition

The Observe phase has produced a fact-based trigger record with a defined scope boundary.

## Process

1. Identify the system boundary: what components, services, or subsystems are in scope.
2. Define the stakeholders: who is affected, in what way, and with what severity.
3. State the constraints that cannot be relaxed: regulatory, contractual, architectural, or operational.
4. State the success condition: what does a resolved or improved state look like, and how would it be measured.
5. Map the dependencies: what the affected area consumes from and provides to the rest of the system.
6. Identify the root question the council must answer. A well-formed root question is specific, answerable, and decides something meaningful.

## Outputs

- Structured problem statement with system boundary.
- Stakeholder impact map.
- Hard constraints list with rationale for each.
- Success criteria with at least one measurable indicator.
- Dependency map for the in-scope components.
- Root question for the council.

## Exit Condition

Every council member can state the problem in their own words without contradiction. The root question is agreed upon. The success criteria are explicit enough that disagreement about whether the outcome was achieved would be resolvable by evidence.

## Failure Signals

- The root question keeps changing mid-discussion.
- Success criteria are vague or unmeasurable ("better performance", "more maintainable").
- The scope keeps expanding without explicit decisions to expand it.
- Constraints are assumed rather than identified and recorded.
- The team cannot distinguish what the user needs from what the user asked for.

## Interaction with Subsequent Phases

The root question and success criteria from Understand govern the entire cycle. Investigate collects evidence to answer the root question. Challenge tests whether the root question is the right one. Every gate in Validate uses the success criteria as its reference.

