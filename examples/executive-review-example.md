# Executive Review Example

This example demonstrates the Executive Summary Engine output and the CTO-level review artifact. It represents the final layer of the Decision Cycle before delivery: the compressed view that a decision owner or executive sponsor uses to confirm the decision is sound without reading the full Decision Packet.

**Scenario:** The platform team proposes extracting the notification service from the monolith into a separate event-driven service. The decision has completed all prior phases and gates. This is the executive review artifact.

---

## Executive Summary

*(200 words or fewer — produced by the Executive Summary Engine)*

The notification service will be extracted from the monolith into a standalone event-driven service that consumes domain events via RabbitMQ and delivers email, SMS, and push notifications. This extraction is necessary because the current coupling prevents the notifications team from deploying independently, causing a 2-week average delay per notification feature release.

**Selected approach:** Event-driven service with RabbitMQ as the message broker. Notifications subscribe to domain events published by the monolith; no direct database access or synchronous API calls between services.

**Dominant tradeoff:** The team accepts 3–4 months of elevated delivery risk and increased operational complexity in exchange for independent deployability and the ability to scale notification throughput separately from the core application.

**Confidence:** Medium. The RabbitMQ topology is new operational surface for this team. Two conditions must be resolved before deployment: (1) a rehearsed message replay procedure for the failure scenario where notifications are consumed but delivery fails; (2) an SRE runbook reviewed and signed off by the on-call rotation.

**Recommendation:** Approve with conditions. Conditions are owned by the SRE lead with a deadline 5 days before the deployment window.

---

## Decision Context

| Field | Value |
|---|---|
| Decision Record | TDR-2024-039 |
| Decision Owner | Platform Team Lead |
| Date | 2024-10-28 |
| Selected Option | O-002: Event-driven notification service with RabbitMQ |
| Quality Score | 7/10 |
| Confidence | Medium |
| Recommendation | Approve with conditions |

---

## What Was Decided

The notification delivery functionality currently embedded in the monolith will be extracted as an independent service. The service will receive domain events (user registered, order placed, payment failed, password reset requested) via a RabbitMQ exchange and deliver notifications through three channels: email (SendGrid), SMS (Twilio), and push (Firebase). The monolith will publish events; it will not know whether or how notifications are delivered.

---

## Options Considered

**O-001: Remain in the monolith with module boundaries**
- Gains: No new operational surface. Lowest delivery risk.
- Costs: Does not solve the independent deployability problem. Teams remain coupled on the release cycle.
- Status: Eliminated. Does not meet the stated success criterion (independent notification deployments).

**O-002: Event-driven notification service with RabbitMQ** *(selected)*
- Gains: Full deployment independence. Notifications can scale separately. The event model decouples the monolith from delivery implementation.
- Costs: New operational component. RabbitMQ is new infrastructure. Message failure handling requires operational discipline (dead-letter queues, replay procedures).

**O-003: Notifications as a SaaS product (e.g., Customer.io, Iterable)**
- Gains: No operational burden. Feature-rich out of the box.
- Costs: Significant data sharing with a third party (user PII and order data). Requires a data processing agreement. Privacy review estimated at 6 weeks. Cost: $4,200/month at current volume.
- Status: Eliminated. Privacy review timeline exceeds the delivery constraint.

---

## Why the Recommendation Is Approve with Conditions

**What supports approval:**
- The architectural merit is clear: the event model provides hard decoupling and is consistent with the platform's stated direction toward event-driven architecture.
- Ownership is defined: the notifications team owns the service end-to-end.
- The RabbitMQ cluster is already in production for two other consumers, so this is not a greenfield infrastructure introduction.
- The blast radius of a notification delivery failure is medium: users miss a notification but no core data is affected and the system recovers when the service recovers.

**What makes it conditional:**
- The team has not operated a dead-letter queue in production before. The failure scenario — message consumed but delivery fails — requires a rehearsed replay procedure. Without it, the team would be learning the recovery process during an incident.
- The SRE on-call rotation has not reviewed the runbook. An alert that fires at 2 AM is only useful if the responder knows what to do.

**What would change the recommendation to Reject:**
- If the dead-letter queue replay rehearsal surfaces an architectural gap (e.g., notification events cannot be safely replayed because of missing idempotency keys), the service is not deployment-ready and the architecture requires revision.
- If the SRE runbook review identifies a failure mode with no recovery path.

---

## Active Conditions

| Condition | Owner | Deadline |
|---|---|---|
| Rehearsed message replay procedure for failed delivery scenario, with evidence that the procedure produces correct results and does not re-deliver successfully-sent notifications | SRE Lead | 2024-11-10 |
| SRE runbook for RabbitMQ consumer failure reviewed and signed off by the primary on-call rotation | SRE Lead | 2024-11-10 |

---

## Reversal Conditions

| Condition | Monitoring | Owner |
|---|---|---|
| Notification delivery error rate exceeds 1% over any 24-hour window in the first 30 days | Datadog alert on delivery error rate; daily review report for 30 days | Notifications team lead |
| Dead-letter queue depth exceeds 100 messages and does not drain within 4 hours | Datadog alert on DLQ depth | SRE on-call |
| Monolith deployment velocity does not improve after extraction (measured as time from PR merge to production over 90 days) | Sprint retrospective data; 90-day review | Platform team lead |

---

## Risk Summary

**Top risk:** The team's operational unfamiliarity with RabbitMQ dead-letter queue recovery. Mitigated by the conditional requirement for a rehearsed replay procedure.

**Second risk:** The event schema between the monolith and the notification service is currently undocumented. If the monolith team changes an event field without coordinating with the notifications team, notifications silently stop for that event type. Mitigation: event schema is pinned to a versioned contract in a shared schema registry before deployment. Schema changes require a migration plan.

**Third risk:** Over the next 12 months, the cost of operating RabbitMQ may increase as additional consumers are added. This decision accepted that cost implicitly by choosing RabbitMQ as the broker for the first new service consumer. Future services should evaluate whether the operational cost per consumer remains proportional to the benefit.

---

## CTO Sign-Off Checklist

- [ ] The dominant tradeoff is understood and accepted: 3–4 months of elevated delivery risk in exchange for independent deployability.
- [ ] The two conditions are assigned to a named owner with a hard deadline.
- [ ] The three reversal conditions are monitored. The 90-day review is scheduled.
- [ ] The event schema contract is in place before the deployment window.
- [ ] The SRE on-call rotation knows about this deployment and has reviewed the runbook.

