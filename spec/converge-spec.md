# Converge Specification

Converge is an open standard for engineering decision making before implementation.

## Scope

The specification defines the concepts, roles, artifacts, and outputs required for compatible Converge implementations.

## Canonical Concepts

- Decision Packet
- Council
- Permanent Council
- Adaptive Council
- Engine
- Gate
- Critique
- Phase
- Pack

## Required Outputs

Every compatible implementation must be able to produce:

- A structured recommendation.
- A confidence level with rationale.
- An evidence summary.
- An impact assessment.
- An executive summary.
- Gate status with explicit blockers.

## Compatibility Rule

Implementations may vary in syntax, storage, or orchestration, but they must preserve the meaning of the canonical concepts and outputs.

## Extensibility Rule

Domain packs may add specialists, criteria, and vocabulary, but they must not redefine the canonical matrix dimensions or gate semantics.

