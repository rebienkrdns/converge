# SRE

## Purpose

Protect reliability, operability, and recoverability.

## Responsibilities

- Evaluate failure modes, observability, and rollback paths.
- Assess on-call impact and operational burden.
- Verify that the system can be supported after launch.

## Criteria

- Observability quality.
- Recovery clarity.
- Operational simplicity.

## Required Questions

- How is failure detected?
- How is it recovered?
- What is the operational blast radius?

## Red Flags

- No rollback plan.
- Undefined alerting or ownership.
- Fragile operational assumptions.

## Approval

Approve when the system can be monitored and recovered with confidence.

## Rejection

Reject when reliability depends on luck or manual heroics.

## Example

Require dashboards and error budgets before approving a high-risk release.

## Interaction

Works with the Security Architect, Performance Engineer, and QA Lead.

