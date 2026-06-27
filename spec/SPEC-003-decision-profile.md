# SPEC-003 — Decision Profile and Decision Record

This document specifies the schemas and derivation rules for the Decision Profile, Decision Record, and Decision Packet — the three top-level artifacts that constitute a completed Decision Cycle.

---

## 1. Decision Profile

The Decision Profile is the structured evaluation output of the Decision Matrix Engine. It is derived — not asserted. Every field that carries a rating or recommendation must show its derivation.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `quality_score` | integer | YES | Range 1–10. Derived from five sub-criteria. |
| `quality_breakdown` | object | YES | Sub-scores for: problem clarity, option quality, evidence strength, risk coverage, council agreement. |
| `confidence` | enum | YES | `high` \| `medium` \| `low`. Derived by Confidence Engine. |
| `confidence_basis` | string | YES | The inputs that produced this confidence level and what would change it. |
| `evidence_status` | enum | YES | `complete` \| `conditional` \| `insufficient` |
| `evidence_gaps` | string[] | NO | Required when `evidence_status` is `conditional` or `insufficient`. |
| `impact` | object | YES | Four rated dimensions (see below). |
| `recommendation` | enum | YES | `approve` \| `approve-with-conditions` \| `reject` \| `defer`. Derived. |
| `recommendation_basis` | string | YES | The Quality Score and Confidence values that produced this recommendation. |
| `dominant_tradeoff` | string | YES | One sentence. What is given up in exchange for what is gained. |

### Quality Score Derivation

The Quality Score is the weighted average of five sub-criteria, each rated 1–10:

| Sub-criterion | Weight | What it measures |
|---|---|---|
| Problem clarity | 20% | Is the root question specific, bounded, and testable? |
| Option quality | 20% | Are the options genuinely different? Are their costs and gains honestly assessed? |
| Evidence strength | 25% | What tier is the evidence? Are gaps named and evaluated? |
| Risk coverage | 20% | Are failure modes identified? Do reversal conditions address the critical risks? |
| Council agreement | 15% | Do the council members agree on the key facts? Are contradictions resolved or documented? |

**Quality Score = (0.20 × problem_clarity) + (0.20 × option_quality) + (0.25 × evidence_strength) + (0.20 × risk_coverage) + (0.15 × council_agreement)**

Rounded to one decimal place. Presented as an integer after rounding.

### Impact Assessment

Four dimensions, each rated independently:

| Dimension | Definition | Low | Medium | High | Critical |
|---|---|---|---|---|---|
| `user_impact` | Effect on end users if the decision's implementation fails | <1% of users affected | 1–10% of users or degraded experience | >10% of users or significant degradation | Service unavailable or data loss |
| `blast_radius` | Scope of systems affected by a failure | Single internal component | Multiple components, one team | Cross-team or shared infrastructure | Platform-wide or cross-company |
| `delivery_cost` | Implementation effort and timeline risk | <1 week, no dependencies | 1–4 weeks, few dependencies | 1–3 months, cross-team coordination | >3 months or requires re-architecture |
| `operational_burden` | On-call and maintenance overhead introduced | No new operational surface | Minor new surface, existing processes handle it | New runbooks, new alerts, new oncall rotation | Requires dedicated operational staffing |

### Recommendation Derivation Table

The Recommendation is derived deterministically from Quality Score and Confidence:

| Quality Score | Confidence: High | Confidence: Medium | Confidence: Low |
|---|---|---|---|
| 9–10 | approve | approve-with-conditions | approve-with-conditions |
| 7–8 | approve | approve-with-conditions | approve-with-conditions |
| 5–6 | approve-with-conditions | approve-with-conditions | defer |
| 3–4 | approve-with-conditions | defer | reject |
| 1–2 | reject | reject | reject |

An override of the derived Recommendation requires: a written override rationale, the name of the person overriding, and a note in the Decision Record's consequences field.

---

## 2. Decision Record

The Decision Record is the permanent artifact that captures what was decided, why, and under what conditions the decision should be revisited.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Format: `TDR-YYYY-NNN`. Unique within system. |
| `title` | string | YES | One sentence naming the decision. |
| `date` | string | YES | ISO 8601 date of the decision. |
| `decision_owner` | string | YES | A named person. Not a team name. |
| `decision_statement` | string | YES | One paragraph. What was decided. Not why — the context field holds why. |
| `context` | string | YES | The observable conditions that made this decision necessary. |
| `options_considered` | string[] | YES | Option IDs. At least two. |
| `selected_option` | string | YES | The Option ID of the selected option. |
| `rejection_rationale` | object | YES | For each non-selected option: the reason it was not chosen. Keyed by Option ID. |
| `consequences` | object | YES | Two keys: `positive` (what improves) and `negative` (what gets worse or harder). |
| `reversal_conditions` | object[] | YES | At least one. Each has: `condition`, `monitoring_mechanism`, `owner`. |
| `active_assumptions` | string[] | YES | Assumption IDs that remain active post-decision. |
| `gate_statuses` | GateStatus[] | YES | One per applicable gate. |
| `decision_profile` | DecisionProfile | YES | The Decision Profile for this decision. |

### Reversal Condition Constraints

A reversal condition is non-conformant if:
- The `condition` is a generic statement ("if things go wrong").
- The `monitoring_mechanism` is unspecified or says "we'll watch for it."
- The `owner` is a team name rather than a named person.

A valid reversal condition example:
```
condition: The p95 latency on /api/search does not return to below 200ms within 24 hours of deployment.
monitoring_mechanism: Datadog alert configured on p95 latency metric for /api/search route, threshold 200ms.
owner: SRE on-call rotation (primary: [name])
```

---

## 3. Decision Packet

The Decision Packet is the complete output of a Decision Cycle. It is the outermost container for all artifacts.

### Schema

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Format: `DP-YYYY-NNN`. Unique within system. |
| `title` | string | YES | One sentence naming the decision context. |
| `phases_completed` | string[] | YES | The phases completed, in canonical order. |
| `council_record` | CouncilRecord | YES | As defined in SPEC-002. |
| `findings` | Finding[] | NO | May be empty for decisions with no actionable findings. |
| `assumptions` | Assumption[] | NO | May be empty for decisions with complete evidence. |
| `options` | Option[] | YES | At least two. |
| `gate_statuses` | GateStatus[] | YES | One per applicable gate. |
| `decision_record` | DecisionRecord | YES | Required when the cycle reaches the Decide phase. |
| `self_critique` | object | YES | All six critique sections: Assumptions, Blind Spots, Missing Evidence, Counterarguments, Confidence Analysis, Future Risks. |
| `executive_summary` | string | YES | 200 words or fewer. Produced by the Executive Summary Engine. |

### Completeness Rules

A Decision Packet is complete when:
1. All applicable phases have outputs.
2. All applicable gates have a status.
3. The Decision Record is present and contains at least one reversal condition.
4. The Self-Critique has all six sections populated with substantive content.
5. The Executive Summary is 200 words or fewer and does not introduce new information.

An incomplete Decision Packet may not proceed to the Deliver phase.
