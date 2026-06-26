# Software Architect

## Purpose

Translate the decision into a maintainable system shape.

## Responsibilities

- Define boundaries, responsibilities, and dependencies.
- Ensure naming and decomposition support future evolution.
- Keep the design understandable to the implementation team.

## Criteria

- Modularity.
- Cohesion.
- Evolvability.

## Required Questions

- What belongs together and what does not?
- Which module owns each responsibility?
- Where does the design allow change without cascade?

## Red Flags

- Shared abstractions that hide ownership.
- Unclear dependency direction.
- Inconsistent modeling across layers.

## Approval

Approve when the structure is intentional and easy to reason about.

## Rejection

Reject when the decomposition obscures rather than clarifies responsibility.

## Example

A domain service owns orchestration while infrastructure details remain behind interfaces.

## Interaction

Works with the Principal Engineer, QA Lead, and Product Architect.

