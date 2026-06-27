# Converge Specification

Converge is an open standard for engineering decision making before implementation. Its invariant: no implementation proceeds without a documented, evaluated, and gate-approved decision.

## Scope

This specification defines the concepts, artifact schemas, processing rules, and behavioral requirements for compatible Converge implementations. A compatible implementation must satisfy every SHALL constraint. MAY constraints describe valid extensions.

---

## 1. Canonical Concepts

The following concepts are canonical. Implementations MAY rename them in their own vocabulary, but MUST preserve their semantics.

| Concept | Definition |
|---|---|
| Decision Cycle | The ordered sequence of ten phases from Observe through Deliver. |
| Decision Packet | The complete structured artifact produced by a Decision Cycle. |
| Council | The group of specialists responsible for evaluating a decision. |
| Permanent Council | The baseline set of ten specialists present in every review. |
| Adaptive Council | Domain specialists activated when the decision requires them. |
| Engine | A deterministic component that transforms inputs into a structured output. |
| Gate | A blocking checkpoint that can prevent implementation from proceeding. |
| Pack | A domain-specific extension that adapts the framework for a technology. |
| Phase | A named stage of the Decision Cycle with defined entry conditions, outputs, and exit conditions. |
| Self-Critique | The adversarial review applied after convergence and before final delivery. |

---

## 2. Artifact Schemas

### 2.1 Finding

A Finding is a single verifiable observation produced during any phase of the cycle.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Unique within the Decision Packet. Format: `F-NNN`. |
| `category` | enum | YES | One of: `security`, `performance`, `architecture`, `reliability`, `cost`, `compliance`, `maintainability`, `observability`. |
| `severity` | enum | YES | One of: `critical`, `high`, `medium`, `low`, `informational`. |
| `statement` | string | YES | One sentence. Factual and verifiable. No recommendations. No opinions. |
| `evidence` | string[] | YES | At least one item. Each item is a pointer to a specific, named source. |
| `risk` | string | YES | One sentence describing the adverse outcome if the finding is not addressed. |
| `recommendation` | string | NO | One sentence describing the corrective action. Absent if the finding is informational. |
| `status` | enum | YES | One of: `open`, `addressed`, `accepted`, `deferred`. |

**Validation rules:**
- A `critical` Finding MUST have a `recommendation`.
- A Finding with `status: deferred` MUST include the deferral rationale in the `recommendation` field.
- A Finding whose `evidence` array is empty is invalid.

### 2.2 Assumption

An Assumption is a statement accepted temporarily because evidence is incomplete. Every assumption carries a weight that affects confidence scoring.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Unique within the Decision Packet. Format: `A-NNN`. |
| `statement` | string | YES | One sentence. Phrased as a positive claim that the decision depends on. |
| `weight` | enum | YES | One of: `high`, `medium`, `low`. |
| `falsification` | string | YES | The observable condition that would prove the assumption false. |
| `monitoring` | string | YES | The mechanism that detects falsification after deployment. |

**Validation rule:** A `high`-weight Assumption that cannot be falsified is a design flaw, not an assumption. It MUST be reclassified as a constraint or a risk.

### 2.3 Option

An Option is a candidate solution considered during the Debate phase.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Unique within the Decision Packet. Format: `O-NNN`. |
| `name` | string | YES | A short, unambiguous label. |
| `description` | string | YES | What the option does and how. |
| `gains` | string[] | YES | At least one item. What improves if this option is chosen. |
| `costs` | string[] | YES | At least one item. What is given up or made harder. |
| `violations` | string[] | NO | Hard constraints that this option violates. Present only if the option is invalid. |
| `status` | enum | YES | One of: `candidate`, `eliminated`, `selected`. |
| `elimination_reason` | string | NO | Required when `status: eliminated`. Why the option was not chosen. |

**Validation rule:** At most one Option in a Decision Packet may have `status: selected`.

### 2.4 Gate Status

A Gate Status records the result of evaluating a single gate against a decision.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `gate` | enum | YES | One of: `security`, `architecture`, `performance`, `testing`, `observability`, `documentation`, `maintainability`. |
| `status` | enum | YES | One of: `pass`, `conditional-pass`, `block`, `not-applicable`. |
| `findings` | string[] | NO | Finding IDs that drove this gate status. |
| `conditions` | string[] | NO | Required when `status: conditional-pass`. The specific conditions that must be met before deployment. |
| `condition_owner` | string | NO | Required when `status: conditional-pass`. The named person or team responsible for satisfying each condition. |
| `condition_deadline` | string | NO | Required when `status: conditional-pass`. ISO 8601 date by which conditions must be resolved. |
| `block_reason` | string | NO | Required when `status: block`. The specific blocking condition that was triggered. |

**Validation rule:** A `block` Gate Status stops the cycle. The decision MUST NOT proceed to the Decide phase while any gate has `status: block`.

### 2.5 Decision Profile

A Decision Profile is the structured evaluation artifact produced by the Decision Matrix Engine.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `quality_score` | integer | YES | Range: 1–10. Derived by the Decision Matrix Engine from sub-criteria. |
| `quality_breakdown` | object | YES | Sub-scores for each dimension: problem clarity, option quality, evidence strength, risk coverage, council agreement. |
| `confidence` | enum | YES | One of: `high`, `medium`, `low`. Derived by the Confidence Engine. |
| `confidence_basis` | string | YES | The factors that set confidence and what would move it. |
| `evidence_status` | enum | YES | One of: `complete`, `conditional`, `insufficient`. |
| `evidence_gaps` | string[] | NO | Required when `evidence_status: conditional` or `insufficient`. |
| `impact` | object | YES | Four fields: `user_impact`, `blast_radius`, `delivery_cost`, `operational_burden`. Each is `low`, `medium`, `high`, or `critical`. |
| `recommendation` | enum | YES | One of: `approve`, `approve-with-conditions`, `reject`, `defer`. Derived deterministically from quality score and confidence. |
| `recommendation_basis` | string | YES | The inputs that produced this recommendation. |
| `dominant_tradeoff` | string | YES | One sentence naming what is given up in exchange for what is gained. |

### 2.6 Decision Record

A Decision Record is the permanent artifact that captures the final decision.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Unique within the system. Format: `TDR-YYYY-NNN`. |
| `title` | string | YES | One sentence naming the decision. |
| `date` | string | YES | ISO 8601 date of the decision. |
| `decision_owner` | string | YES | Named person responsible for the decision and its outcomes. |
| `decision_statement` | string | YES | One paragraph. What was decided, not why. |
| `context` | string | YES | What made this decision necessary. The observable conditions that created the problem. |
| `options_considered` | string[] | YES | The IDs of Options that were evaluated. At least two. |
| `selected_option` | string | YES | The ID of the selected Option. |
| `rejection_rationale` | object | YES | For each non-selected option, why it was eliminated. |
| `consequences` | object | YES | Two keys: `positive` (what improves) and `negative` (what gets worse or harder). |
| `reversal_conditions` | object[] | YES | At least one reversal condition. Each has: `condition`, `monitoring_mechanism`, `owner`. |
| `active_assumptions` | string[] | YES | The IDs of Assumptions that remain active after the decision. |
| `gate_statuses` | object[] | YES | One Gate Status record per applicable gate. |
| `decision_profile` | object | YES | The Decision Profile for this decision. |

### 2.7 Decision Packet

A Decision Packet is the complete output of a Decision Cycle.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `id` | string | YES | Unique within the system. Format: `DP-YYYY-NNN`. |
| `title` | string | YES | One sentence naming the decision context. |
| `phases_completed` | enum[] | YES | The phases completed, in order. |
| `findings` | Finding[] | NO | All findings produced across all phases. |
| `assumptions` | Assumption[] | NO | All assumptions active at any point in the cycle. |
| `options` | Option[] | YES | All options evaluated in the Debate phase. At least two. |
| `council_activations` | string[] | YES | The permanent and adaptive council members who participated. |
| `decision_record` | DecisionRecord | YES | Required for cycles that reach the Decide phase. |
| `self_critique` | object | YES | Required before the Deliver phase. All six critique sections populated. |
| `executive_summary` | string | YES | The compressed summary produced by the Executive Summary Engine. |

---

## 3. Processing Rules

### 3.1 Phase Ordering

Phases MUST be executed in order: Observe → Understand → Investigate → Challenge → Debate → Converge → Decide → Validate → Reflect → Deliver. A phase MAY be abbreviated with documented rationale when evidence makes it redundant. No phase MAY be skipped by default.

### 3.2 Gate Evaluation

Gates are evaluated during the Validate phase. The applicable gates for a decision are determined by the Gate Engine using the decision type. A gate result of `block` on any applicable gate MUST prevent advancement to the Decide phase.

### 3.3 Confidence Derivation

Confidence is derived by the Confidence Engine using four inputs: evidence tier (Primary/Secondary/Tertiary), assumption weight (high/medium/low), council disagreement level, and evidence gap count. Confidence MUST NOT be asserted without derivation.

### 3.4 Recommendation Derivation

Recommendation is derived deterministically from the Decision Profile's quality score and confidence level. An implementation MUST NOT allow a council member to assert a recommendation that contradicts the derived recommendation without documenting the override and its rationale.

---

## 4. Required Outputs

Every compatible implementation MUST be able to produce the following for any completed cycle:

1. A Decision Record that satisfies the schema in §2.6.
2. A Confidence level derived by the process in §3.3, with written basis.
3. An Evidence summary classifying evidence by tier with gap identification.
4. An Impact assessment with all four dimensions: user impact, blast radius, delivery cost, operational burden.
5. Gate status for all applicable gates, with all conditions and blocking reasons documented.
6. An Executive Summary of 200 words or fewer.

---

## 5. Compatibility Rule

Implementations MAY vary in syntax, storage format, tooling, or orchestration mechanism. They MUST preserve the semantics of the canonical concepts in §1, the field constraints in §2, and the processing rules in §3. An implementation that omits required fields, changes the derivation logic for Confidence or Recommendation, or allows gate blocks to be silently bypassed is not compatible.

---

## 6. Extensibility Rule

Domain packs MAY add specialist roles, evaluation criteria, domain-specific questions, and vocabulary. They MUST NOT:
- Redefine the canonical matrix dimensions (Quality Score, Confidence, Evidence, Impact, Recommendation, Executive Summary).
- Change the gate semantics (what a `block` means or its effect on the cycle).
- Remove required fields from canonical artifact schemas.
- Override the deterministic derivation of Confidence or Recommendation.

