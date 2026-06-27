# Architecture Gate

## Purpose

Block designs that introduce unresolved coupling, violate existing system boundaries without justification, or cannot sustain the expected lifespan and scale of the system.

## Applies When

The decision introduces a new component, changes the communication pattern between existing components, alters data ownership, modifies a service boundary, or makes a structural commitment that is difficult to reverse.

## Approval Conditions

All of the following must be true:

- Every component or service introduced by the decision has a single, clearly stated responsibility that does not duplicate an existing component's responsibility.
- Dependencies are directional: components depend on stable interfaces, not on implementations. Circular dependencies are absent. Any dependency on a component that does not yet exist is identified as a risk.
- The design has been evaluated at 5x and 10x the current data volume or traffic. The evaluation may conclude that the current design is sufficient or that a migration path exists — but the evaluation must exist.
- The boundary model is consistent with the existing system's boundary model, or the deviation is justified by a documented architectural decision that explains the exception.
- The tradeoffs of the selected architecture are explicitly described: what becomes easier, what becomes harder, and what is given up permanently.

## Blocking Conditions

The gate blocks if any of the following is true:

- The primary justification for the architectural choice is "this is how we have always done it" or "this is the industry standard" without reference to the specific problem's constraints.
- Two components in the design share mutable state without a defined ownership model — a form of unresolved coupling that will produce coordination overhead or race conditions as the system evolves.
- The design introduces a distributed system pattern (event sourcing, saga, CQRS, service mesh) that the team has no operational experience with, without a plan for acquiring that experience before production deployment.
- The decision makes a structural commitment that cannot be reversed without a rewrite, and the evidence for committing does not meet the Confidence Engine's threshold for High confidence.

## Evidence Required

- Architecture diagram or structured narrative describing the components, boundaries, and communication patterns.
- Boundary rationale: the documented reason for each boundary decision, including the alternatives considered.
- Scale analysis: the behavior of the design at 5x and 10x current volume.
- Completed Architecture Review using the Architecture Review Template.
- Tradeoff analysis: what the design gains and what it permanently gives up.

