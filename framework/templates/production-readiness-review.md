# Production Readiness Review Template

Use this template immediately before a new system, major feature, or significant change enters a production environment. All referenced gate reviews must be complete before this template is filled.

---

## Release Scope

Describe what is entering production: the system or feature name, the change type (new system / new feature / significant modification / removal), and the affected user base.

**Deployment window:** [date and time range]
**Deployment owner:** [named individual]
**Rollback owner:** [named individual]
**Incident contact:** [named individual or on-call rotation]

## Gate Status Summary

| Gate | Status | Reference | Owner of open conditions |
|---|---|---|---|
| Architecture | Pass / Conditional / Block | | |
| Security | Pass / Conditional / Block | | |
| Performance | Pass / Conditional / Block | | |
| Testing | Pass / Conditional / Block | | |
| Documentation | Pass / Conditional / Block | | |
| Observability | Pass / Conditional / Block | | |
| Maintainability | Pass / Conditional / Block | | |

_A status of Block on any gate prevents production release. A status of Conditional requires all conditions to be resolved before the deployment window opens or explicitly accepted by the release owner with a documented rationale._

## Observability Readiness

Confirm that the system can be understood from the outside after deployment.

- [ ] Structured logs are in place for all critical operations.
- [ ] Metrics are emitted for all success criteria defined in the decision record.
- [ ] Alerting rules are configured and tested.
- [ ] Dashboards are available and verified to reflect real data.
- [ ] Distributed tracing covers the critical path.
- [ ] Log retention meets compliance requirements.

## Rollback Readiness

Confirm that the release can be reversed safely and quickly.

- [ ] Rollback procedure is documented and has been rehearsed or dry-run.
- [ ] Database migrations are reversible, or irreversible migrations have a documented recovery path.
- [ ] Feature flags or blue/green routing are in place if rollback requires traffic switching.
- [ ] Rollback time estimate is within the SLA tolerance for the affected user base.
- [ ] The rollback owner has been briefed and is available during the deployment window.

## Open Items

List every item that is not yet resolved at the time of this review.

| Item | Type | Owner | Deadline | Risk if unresolved |
|---|---|---|---|---|
| | Blocker / Accepted exception / Monitoring item | | | |

_Guidance: An item typed "Blocker" must be resolved before the deployment window. An item typed "Accepted exception" must include a rationale and be signed off by the release owner._

## Approval

**Release authorized:** Yes / No / Conditional

**Authorizing party:** [named individual or council]

**Date:** [date]

**Conditions for conditional approval:** [if applicable — list each condition with owner and deadline]

**Next verification point:** [date, milestone, or observable signal at which readiness will be re-evaluated if conditional, or at which success criteria will be evaluated after release]

