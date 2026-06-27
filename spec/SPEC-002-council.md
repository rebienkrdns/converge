# SPEC-002 — Council

This document specifies the structure, composition rules, participation requirements, and activation criteria for both the Permanent Council and the Adaptive Council.

---

## 1. Council Purpose

The Council evaluates decisions, not implementations. Its role is to surface risks, contradictions, and gaps before the decision is finalized — not to approve work after it is done. A council that functions as a rubber stamp is non-conformant.

---

## 2. Permanent Council

The Permanent Council is activated for every decision cycle, regardless of domain, technology, or decision type.

### Composition

| Role | Primary Responsibility |
|---|---|
| Principal Engineer | Code quality, maintainability, technical correctness of the implementation approach |
| Distinguished Engineer | Long-term architectural coherence, cross-system consistency, precedent implications |
| CTO | Business alignment, resource allocation, strategic risk |
| Security Architect | Threat surface, trust boundaries, authentication, authorization, data protection |
| Performance Engineer | Latency, throughput, resource consumption, scalability under load |
| Software Architect | Component boundaries, dependency direction, extensibility, design patterns |
| Staff Engineer | Implementation feasibility, team capability, delivery risk |
| SRE | Operational complexity, failure modes, recovery procedures, on-call impact |
| QA Lead | Testability, coverage of critical paths, regression risk, test strategy |
| Product Architect | User impact, product coherence, feature interaction, business outcome alignment |

### Participation Requirements

Each Permanent Council member must produce, per cycle, at least one of:
- A named question that challenges an assumption, a gap in the evidence, or the framing of the decision.
- A finding classified by severity and category.
- A specialist evaluation that assesses the decision against the member's domain criteria.

A council member who participates only by endorsing another member's assessment does not satisfy this requirement. Endorsements are not evaluations.

### Disagreement Handling

When two Permanent Council members hold contradictory positions:
1. The contradiction is surfaced explicitly in the Debate or Converge phase record.
2. The record states both positions and the evidence each rests on.
3. The resolution (or accepted unresolved conflict) is noted with its effect on the Confidence level.
4. A contradiction resolved by omitting one position is non-conformant.

---

## 3. Adaptive Council

The Adaptive Council consists of domain specialists activated when the decision involves their specific technology or operating context.

### Activation Rules

Activation is determined by the Pack for the relevant domain. The Pack specifies the condition under which the specialist is activated. Adaptive Council members are not activated by default — they are activated only when the condition is met.

| Specialist | Activation Condition |
|---|---|
| Laravel Specialist | Decision involves a Laravel application |
| Next.js Specialist | Decision involves a Next.js application or SSR strategy |
| React Specialist | Decision involves React component architecture or state management |
| Node.js Specialist | Decision involves Node.js runtime, event loop, or async patterns |
| Python Specialist | Decision involves Python services, data pipelines, or ML infrastructure |
| Go Specialist | Decision involves Go services or concurrency patterns |
| Rust Specialist | Decision involves Rust code, memory safety guarantees, or systems programming |
| Docker Specialist | Decision involves containerization, image construction, or runtime configuration |
| AWS Specialist | Decision involves AWS services, IAM, or cloud infrastructure on AWS |
| Azure Specialist | Decision involves Azure services or cloud infrastructure on Azure |
| Kubernetes Specialist | Decision involves container orchestration, pod scheduling, or cluster operations |
| PostgreSQL Specialist | Decision involves PostgreSQL schema, query patterns, or replication |
| Redis Specialist | Decision involves Redis data structures, caching strategy, or persistence |
| RabbitMQ Specialist | Decision involves message queue topology, routing, or consumer patterns |
| Kafka Specialist | Decision involves event streaming, partition strategy, or consumer groups |
| Microservices Specialist | Decision involves service boundary definition or inter-service communication |
| AI/ML Specialist | Decision involves machine learning models, training pipelines, or inference infrastructure |
| RAG Specialist | Decision involves retrieval-augmented generation or vector database patterns |
| LLM Specialist | Decision involves large language model integration, prompting strategy, or model selection |

### Documentation Requirement

The activation record must state:
- Which specialists were activated and the specific Pack condition that triggered activation.
- Which specialists were considered and not activated, and why they do not apply.

Non-activation of a relevant specialist must be documented. Undocumented non-activation is a conformance defect.

### Adaptive Council Constraints

Adaptive Council members:
- Evaluate the decision against domain-specific criteria defined in their Pack.
- Raise domain-specific Required Questions and Red Flags.
- Interact with Permanent Council members when their domain evaluation affects a cross-cutting concern.
- Do NOT replace Permanent Council members. The Adaptive Council supplements; it does not substitute.

---

## 4. Council Record

The Council Record is the section of the Decision Packet that documents council participation.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `permanent_council_members` | string[] | YES | All ten roles must be listed. |
| `adaptive_council_activated` | string[] | YES | The specialists who participated. May be empty. |
| `adaptive_council_considered` | object[] | YES | Each entry: `{ specialist, not_activated_reason }`. |
| `contradictions` | object[] | NO | Each entry: `{ positions, resolution, confidence_effect }`. |
| `open_conflicts` | string[] | NO | Contradictions that were not resolved, accepted as-is. |

---

## 5. What Constitutes an Evaluation

An evaluation is not a vote. A council member who says "I approve this" has not evaluated it. Evaluation requires:

1. **Criteria applied**: The council member applied their domain's specific criteria to the decision.
2. **Finding or question produced**: At least one concrete output — a finding with evidence, or a question that surfaces a gap.
3. **Position on the critical paths**: The council member assessed the decision's effect on the paths most relevant to their domain.

The Permanent Council member most likely to find a defect in the decision should be the one scrutinizing it hardest, not the one most likely to approve it.
