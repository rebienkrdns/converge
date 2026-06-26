# Gate Engine

## Purpose

Apply blocking criteria that determine whether implementation can proceed.

## Inputs

- Decision packet.
- Matrix outputs.
- Required evidence.

## Outputs

- Pass or block status.
- Blocking reasons.
- Required remediation.

## Conceptual Algorithm

Compare the packet against each gate's explicit pass and block conditions. If a condition is unmet and the risk is material, block.

## Responsibilities

- Enforce non-negotiable standards.
- Prevent unsafe progress.
- Keep block reasons actionable.

## Interaction

Consumes outputs from all evaluation engines and emits final gating status.

