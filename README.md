# Converge v1

Converge is an open engineering intelligence framework for making technical decisions before writing code.

Its purpose is not to accelerate implementation by default. Its purpose is to improve the quality, traceability, and durability of engineering decisions across teams, domains, and AI systems.

## Core Thesis

> No escribimos código primero. Tomamos decisiones primero.

Converge defines a decision-first operating model for software engineering. It standardizes how teams observe a problem, challenge assumptions, collect evidence, evaluate impact, deliberate through specialist councils, and only then authorize delivery.

## What This Repository Contains

- `docs/` for the philosophy, constitution, decision cycle, principles, glossary, manifesto, and roadmap.
- `framework/` for the operational model: councils, engines, gates, critique, phases, templates, and packs.
- `examples/` for applied decision artifacts.
- `spec/` for the open specification that other implementations can follow.
- `tests/` for conformance-oriented checks and fixtures.

## Design Goals

- Make decision quality explicit.
- Separate evidence from opinion.
- Keep approvals and rejections auditable.
- Support both general-purpose and domain-specific review.
- Remain implementation-agnostic so any agent can use it.

## Reading Order

1. `docs/constitution.md`
2. `docs/decision-cycle.md`
3. `framework/council/README.md`
4. `framework/engines/README.md`
5. `framework/gates/README.md`
6. `spec/converge-spec.md`

## Status

This repository represents Converge v1, the first public baseline of the framework.

