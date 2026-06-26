# Recommendation Engine

## Purpose

Turn evaluation into an actionable recommendation.

## Inputs

- Matrix scores.
- Gate outcomes.
- Specialist consensus or dissent.

## Outputs

- Recommended action.
- Conditions.
- Rationale.

## Conceptual Algorithm

Recommend the least risky acceptable path that still satisfies the stated objective and constraints.

## Responsibilities

- Keep the recommendation specific.
- Preserve conditional approval when needed.
- Reject vague next steps.

## Interaction

Consumes the Decision Matrix Engine and informs the Executive Summary Engine.

