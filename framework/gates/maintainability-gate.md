# Maintainability Gate

## Purpose

Block implementations whose long-term support cost is disproportionate to their benefit, or that introduce complexity that the team does not have the capacity to understand, change, or operate over the expected lifespan of the system.

## Applies When

The decision introduces a new abstraction or pattern that did not exist before in the codebase, adds a dependency with a complex operational model, establishes a design that requires specialized knowledge to change safely, or extends the scope of a component beyond its original responsibility.

## Approval Conditions

All of the following must be true:

- An engineer who is familiar with the codebase but was not involved in this decision can read the relevant code and documentation and understand what the component does, why it exists, and how to change it safely — without assistance from the original author.
- All dependencies introduced by the decision have active maintenance, a documented migration path if deprecated, and an owner on the team who understands the dependency well enough to evaluate its upgrade impact.
- The design can be changed in the most likely future directions (scaling the system, replacing a dependency, adding a new feature in the same domain) without requiring a rewrite or a global refactor.
- Ownership is defined: a named team or individual is responsible for the long-term maintenance of every component introduced by the decision. Components without owners become orphaned, and orphaned components become unmaintainable.
- The complexity introduced by the decision is incidental only where necessary: the decision does not add abstraction layers, configuration options, or generalization hooks for hypothetical future requirements that are not part of the current decision.

## Blocking Conditions

The gate blocks if any of the following is true:

- The decision introduces an abstraction that cannot be understood without reading its implementation — a leaky abstraction that requires consumers to know its internals to use it correctly.
- A dependency is introduced that requires the team to maintain deep expertise in a system they do not currently understand, with no plan for acquiring that expertise.
- The design couples two components in a way that makes it impossible to change one without changing the other, and the coupling is not justified by a business requirement.
- No one on the team can name who is responsible for the long-term maintenance of the components introduced by this decision.
- The decision adds generalization or configurability for use cases that do not currently exist and are not part of the current decision scope.

## Evidence Required

- Maintainability rationale: an explanation of how the design balances current requirements with future changeability, and why the complexity level is appropriate.
- Change impact analysis: for the two or three most likely future changes in the same domain, a description of what would need to change in the implementation and whether it would be a local or systemic modification.
- Ownership and dependency map: a list of every component and dependency introduced by the decision, with the named owner of each and the maintenance commitment it carries.

