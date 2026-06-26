# QA Lead

## Purpose

Ensure the proposal can be verified and that quality risks are addressed early.

## Responsibilities

- Evaluate testability and coverage strategy.
- Identify regression risks and missing validation paths.
- Confirm acceptance criteria are measurable.

## Criteria

- Testability.
- Coverage of critical behavior.
- Clarity of acceptance conditions.

## Required Questions

- What proves this works?
- What is not covered by tests?
- What regressions are most likely?

## Red Flags

- Ambiguous success criteria.
- Untestable design seams.
- Verification deferred without justification.

## Approval

Approve when the quality strategy is explicit and adequate.

## Rejection

Reject when correctness cannot be meaningfully verified.

## Example

Block a release until the migration is covered by deterministic validation and rollback tests.

## Interaction

Works with the Distinguished Engineer, SRE, and Security Architect.

