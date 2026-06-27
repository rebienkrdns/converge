# SPEC-000 — Meta Specification

This document defines the canonical entity model for Converge. All other SPEC files reference the types defined here.

---

## Purpose

To establish a single, unambiguous vocabulary for Converge artifacts so that every implementation, tool, and reviewer uses the same concepts with the same constraints.

---

## Entity Hierarchy

```
Decision Packet
├── Phases (1–10, ordered)
├── Finding[]
├── Assumption[]
├── Option[]
│   └── status: candidate | eliminated | selected
├── Council Activation[]
│   ├── Permanent Council (always present)
│   └── Adaptive Council (domain-triggered)
├── Gate Status[] (one per applicable gate)
├── Decision Record
│   ├── Decision Profile
│   │   ├── Quality Score (1–10, derived)
│   │   ├── Confidence (high | medium | low, derived)
│   │   ├── Impact (4 dimensions, each rated)
│   │   └── Recommendation (derived)
│   └── Reversal Condition[]
├── Self-Critique (6 sections)
└── Executive Summary (≤200 words)
```

---

## Canonical Entity List

| Entity | Defined In | Role |
|---|---|---|
| Finding | SPEC-001 | A verifiable observation with severity, evidence, and status |
| Assumption | SPEC-001 | A claim accepted temporarily due to incomplete evidence |
| Option | SPEC-001 | A candidate solution evaluated in the Debate phase |
| Gate Status | SPEC-001 | The result of a single gate evaluation |
| Council | SPEC-002 | The group of specialists responsible for evaluation |
| Permanent Council | SPEC-002 | The ten base specialists present in every cycle |
| Adaptive Council | SPEC-002 | Domain specialists activated by Pack criteria |
| Decision Profile | SPEC-003 | The structured evaluation artifact from the Decision Matrix Engine |
| Decision Record | SPEC-003 | The permanent artifact capturing the final decision |
| Decision Packet | SPEC-003 | The complete output of a Decision Cycle |

---

## Shared Type Definitions

### Severity
Used by Finding.

```
critical | high | medium | low | informational
```

- `critical`: The decision cannot proceed safely without addressing this finding.
- `high`: The finding significantly affects the recommendation but does not block alone.
- `medium`: The finding should be tracked and may affect conditions.
- `low`: The finding is noted for awareness; does not affect the recommendation.
- `informational`: No action required; recorded for completeness.

### Weight
Used by Assumption.

```
high | medium | low
```

- `high`: Falsification of this assumption would change the recommendation.
- `medium`: Falsification would require monitoring and may trigger a reversal condition.
- `low`: Falsification would not materially affect the decision.

### Gate Name
Used by Gate Status.

```
security | architecture | performance | testing | observability | documentation | maintainability
```

### Gate Result
Used by Gate Status.

```
pass | conditional-pass | block | not-applicable
```

### Confidence
Used by Decision Profile. Derived by the Confidence Engine. Not asserted.

```
high | medium | low
```

### Recommendation
Used by Decision Profile. Derived deterministically from Quality Score and Confidence. Not asserted.

```
approve | approve-with-conditions | reject | defer
```

### Evidence Tier
Used by the Evidence Engine for classification.

```
primary | secondary | tertiary
```

- `primary`: Direct evidence from the system under decision (logs, metrics, benchmarks run on the actual system).
- `secondary`: Evidence from comparable systems, trusted third-party reports, or structured analysis of adjacent systems.
- `tertiary`: Case studies, general benchmarks, expert opinion, or anecdotal comparisons from unrelated systems.

### Impact Rating
Used by the four dimensions of the Impact Engine.

```
low | medium | high | critical
```

---

## ID Format Conventions

| Entity | Format | Example |
|---|---|---|
| Finding | F-NNN | F-001, F-042 |
| Assumption | A-NNN | A-001, A-003 |
| Option | O-NNN | O-001, O-002 |
| Decision Record | TDR-YYYY-NNN | TDR-2024-001 |
| Decision Packet | DP-YYYY-NNN | DP-2024-001 |

IDs are unique within a Decision Packet. TDR and DP IDs are unique within the system.

---

## Invariants

These constraints hold across all entity instances and all implementations:

1. A Decision Packet without a Decision Record has not completed the cycle.
2. A Decision Record without reversal conditions is non-conformant.
3. A Gate Status of `block` prevents the cycle from advancing to Decide.
4. A `conditional-pass` Gate Status without named conditions, owner, and deadline is non-conformant.
5. A Confidence level that is asserted (not derived) is non-conformant.
6. A Recommendation that is asserted (not derived) is non-conformant.
7. At most one Option per Decision Packet may have `status: selected`.
8. A Finding with `severity: critical` and `status: accepted` must carry explicit acceptance rationale signed by the Decision Owner.
