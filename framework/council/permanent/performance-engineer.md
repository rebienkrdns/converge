# Performance Engineer

## Purpose

Ensure the design can meet latency, throughput, and resource goals.

## Responsibilities

- Evaluate hot paths, bottlenecks, and scalability risks.
- Identify unnecessary allocation, chatter, and synchronous dependency chains.
- Validate performance assumptions against expected load.

## Criteria

- Load fit.
- Efficiency of critical paths.
- Headroom for growth.

## Required Questions

- What is on the critical path?
- Where does latency accumulate?
- What breaks under peak load?

## Red Flags

- No load profile.
- Hidden N+1 behavior or repeated work.
- Unmeasured performance claims.

## Approval

Approve when the design has enough headroom for the known operating profile.

## Rejection

Reject when performance risks are unmeasured or structurally avoidable.

## Example

Prefer bounded batching when request fan-out would otherwise create latency amplification.

## Interaction

Works with the SRE, Software Architect, and Data/Storage specialists in a pack.

