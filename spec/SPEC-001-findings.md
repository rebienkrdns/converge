# SPEC-001 — Findings, Assumptions, Options, Gate Status

This document specifies the schemas for the four primary observation and evaluation entities produced during a Decision Cycle.

---

## 1. Finding

A Finding is a single verifiable observation. It is factual and source-backed. It is not a recommendation, a risk statement, or an opinion. The distinction matters: a finding describes what is true; a recommendation describes what to do about it.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Format: `F-NNN`. Unique within Decision Packet. |
| `category` | enum | YES | `security` \| `performance` \| `architecture` \| `reliability` \| `cost` \| `compliance` \| `maintainability` \| `observability` |
| `severity` | enum | YES | `critical` \| `high` \| `medium` \| `low` \| `informational` |
| `statement` | string | YES | One sentence. Factual. Verifiable. No recommendations. No opinions. |
| `evidence` | string[] | YES | At least one entry. Each entry names a specific, retrievable source. |
| `risk` | string | YES | One sentence. The adverse outcome if the finding is not addressed. |
| `recommendation` | string | NO | One sentence. The corrective action. Required when severity is `critical`. Absent when severity is `informational`. |
| `status` | enum | YES | `open` \| `addressed` \| `accepted` \| `deferred` |
| `acceptance_rationale` | string | NO | Required when `status: accepted` and `severity: critical`. Signed by Decision Owner. |
| `deferral_rationale` | string | NO | Required when `status: deferred`. |

### Validation Rules

- `severity: critical` + `status: accepted` requires `acceptance_rationale` signed by the Decision Owner.
- `status: deferred` requires `deferral_rationale`.
- `evidence` array must not be empty. A Finding with no named source is an assumption, not a finding.
- `statement` must not contain the word "should" or "recommend" — those are recommendation fields.

### Status Transitions

```
open → addressed   (evidence of correction provided)
open → accepted    (risk acknowledged, Decision Owner signs acceptance)
open → deferred    (addressed in a future decision with rationale)
addressed → open   (if correction is later found insufficient)
```

### Example

```
id: F-003
category: performance
severity: high
statement: The product enrichment query runs one SQL query per search result, producing up to 20 queries per request at the observed result set sizes.
evidence:
  - "PostgreSQL slow query log, 2024-03-15, showing 18.4 avg queries per /api/search request"
risk: At 3,200 req/s, the N+1 pattern produces up to 64,000 DB queries per second, exceeding the DB's sustainable throughput.
recommendation: Replace the per-item query with a single batched query using IN() with the result ID set.
status: addressed
```

---

## 2. Assumption

An Assumption is a statement accepted temporarily because the evidence to confirm or deny it was not available at decision time.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Format: `A-NNN`. Unique within Decision Packet. |
| `statement` | string | YES | One sentence. A positive claim the decision depends on being true. |
| `weight` | enum | YES | `high` \| `medium` \| `low` |
| `falsification` | string | YES | The specific, observable event that would prove this assumption false. |
| `monitoring` | string | YES | The mechanism that detects falsification after deployment. Named signal or process. |

### Validation Rules

- A `high`-weight assumption that has no falsification condition is a design defect. Reclassify as a constraint or a risk.
- `falsification` must describe an observable event, not a feeling or a general concern.
- `monitoring` must name a specific signal, tool, or review process — not "we'll watch for it."

### Weight Semantics

- `high`: If this assumption is false, the recommendation changes. This assumption is load-bearing.
- `medium`: If this assumption is false, a reversal condition is triggered. The decision may stand, but requires review.
- `low`: If this assumption is false, it is noted and monitored but does not affect the decision.

### Example

```
id: A-001
statement: The 14 scraper IP addresses will not rotate to new addresses within 30 days of deployment.
weight: high
falsification: NGINX access logs show elevated traffic from IP addresses outside the known 14 within 30 days of deployment.
monitoring: Daily review of top-10 source IPs on /api/search for 30 days post-deployment. Alert if a new IP joins the top 10 with >100 req/min.
```

---

## 3. Option

An Option is a candidate solution evaluated during the Debate phase. Every Decision Packet must contain at least two options: the selected option and at least one alternative.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Format: `O-NNN`. Unique within Decision Packet. |
| `name` | string | YES | A short, unambiguous label. No jargon without definition. |
| `description` | string | YES | What the option does and how it achieves the goal. |
| `gains` | string[] | YES | At least one entry. Concrete improvements if this option is chosen. |
| `costs` | string[] | YES | At least one entry. What is given up or made harder. |
| `violations` | string[] | NO | Hard constraints from the Understand phase that this option violates. Present only if the option is invalid on constraint grounds. |
| `status` | enum | YES | `candidate` \| `eliminated` \| `selected` |
| `elimination_reason` | string | NO | Required when `status: eliminated`. |

### Validation Rules

- At most one option per Decision Packet may have `status: selected`.
- An option with `violations` populated MUST have `status: eliminated`.
- `elimination_reason` must explain why this option was not chosen, not just that it was worse. The reasoning must be specific enough that a future reader could evaluate whether the decision still holds.

### Elimination Reasons

An option may be eliminated for one of three reasons:
1. **Constraint violation**: The option violates a hard constraint from the Understand phase.
2. **Dominated**: Another option is superior on all evaluated dimensions with no compensating advantage.
3. **Out of scope**: The option exceeds the time, budget, or authority constraints of the current decision.

---

## 4. Gate Status

A Gate Status records the result of applying a single gate's criteria to the current decision.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `gate` | enum | YES | `security` \| `architecture` \| `performance` \| `testing` \| `observability` \| `documentation` \| `maintainability` |
| `status` | enum | YES | `pass` \| `conditional-pass` \| `block` \| `not-applicable` |
| `findings` | string[] | NO | Finding IDs that drove this gate result. |
| `conditions` | string[] | NO | Required when `status: conditional-pass`. Each condition is a specific action. |
| `condition_owner` | string | NO | Required when `status: conditional-pass`. A named person or team. |
| `condition_deadline` | string | NO | Required when `status: conditional-pass`. ISO 8601 date. |
| `block_reason` | string | NO | Required when `status: block`. The specific blocking condition triggered. |
| `not_applicable_reason` | string | NO | Required when `status: not-applicable`. |

### Validation Rules

- `status: block` prevents the cycle from advancing to Decide. A block may only be cleared by resolving the blocking condition or by an explicit Decision Owner risk-acceptance override, which is documented in the Decision Record.
- `status: conditional-pass` requires all three of: `conditions`, `condition_owner`, and `condition_deadline`. A conditional-pass without these fields is treated as a block.
- `status: not-applicable` requires `not_applicable_reason`. A gate is not-applicable only if the decision type definitionally excludes it (e.g., a pure documentation decision has no performance gate exposure).

### Status Semantics

- `pass`: All approval conditions for this gate were met. Evidence on file.
- `conditional-pass`: The gate can pass if the named conditions are met by the named owner before the deadline. Deployment may not proceed until conditions are verified.
- `block`: At least one blocking condition was triggered. The decision may not advance until the block is resolved.
- `not-applicable`: The gate's scope does not apply to this decision. Rationale required.
