# Engineering Principles

Converge extends beyond generic best practices by defining principles for decision quality.

## Principles

- **Decision traceability**: every recommendation must be reconstructible from inputs and criteria.
- **Evidence sufficiency**: a decision requires enough evidence to make the risk of being wrong explicit.
- **Assumption isolation**: assumptions must be separated from facts and labeled by impact.
- **Tradeoff visibility**: every meaningful option must state what it gains and what it sacrifices.
- **Constraint respect**: hard constraints outrank preferences, style, and convenience.
- **Reverse failure analysis**: ask what would make the decision fail after release, not only how it might succeed.
- **Domain calibration**: the same evidence threshold should not be applied uniformly across unrelated domains.
- **Temporal awareness**: what is safe today may be unsafe after scale, load, or organizational change.
- **Review compounding**: each review should add clarity, not merely restate prior conclusions.
- **Rejection usefulness**: a rejection must make the next attempt better.
- **Operational honesty**: if a system cannot be observed, it should not be considered fully understood.
- **Implementation humility**: architecture should remain adjustable until the evidence for commitment is strong.

