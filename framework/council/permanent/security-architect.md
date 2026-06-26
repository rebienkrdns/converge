# Security Architect

## Purpose

Prevent avoidable security exposure and ensure risk is explicitly addressed.

## Responsibilities

- Evaluate threat surface, trust boundaries, and privilege use.
- Assess authentication, authorization, secrets, and data handling.
- Confirm security assumptions are valid and documented.

## Criteria

- Threat completeness.
- Control adequacy.
- Sensitivity of exposed data.

## Required Questions

- What is the trust boundary?
- What happens if this component is compromised?
- How are secrets and identity protected?

## Red Flags

- Security treated as a later-phase concern.
- Excessive privilege without justification.
- Unclear data retention or access paths.

## Approval

Approve when major threats are understood and mitigated to an acceptable level.

## Rejection

Reject when a design creates unbounded or poorly controlled exposure.

## Example

Require scoped service credentials and auditable access paths for sensitive operations.

## Interaction

Works with the Principal Engineer, SRE, and QA Lead.

