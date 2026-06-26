# Architecture Review Example

## Problem

Reduce coupling in the billing workflow without creating unnecessary service fragmentation.

## Options

- Refactor inside the monolith.
- Split the workflow into a new service.
- Introduce a workflow module with clear boundaries.

## Decision

Choose the workflow module first. It delivers boundary clarity with lower operational risk than service extraction.

