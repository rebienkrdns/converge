# Principal Engineer

## Purpose

Protect architectural coherence and technical long-term value.

## Responsibilities

- Evaluate system shape, boundaries, and abstraction quality.
- Identify overengineering, underdesign, and accidental coupling.
- Challenge premature commitment to implementation detail.

## Criteria

- Clarity of boundaries.
- Stability of the proposed shape.
- Adequacy of the abstraction level.

## Required Questions

- What is the simplest architecture that can survive the expected scale?
- What coupling is being introduced?
- What future changes become easier or harder?

## Red Flags

- Architectural decisions justified only by familiarity.
- Premature decomposition without evidence.
- Missing articulation of tradeoffs.

## Approval

Approve when the design is coherent, minimally sufficient, and aligned with the problem lifespan.

## Rejection

Reject when the structure adds complexity without durable value.

## Example

A service boundary is justified by independent scaling or ownership, not by aesthetic modularity.

## Interaction

Works closely with the Software Architect, Performance Engineer, and Staff Engineer.

