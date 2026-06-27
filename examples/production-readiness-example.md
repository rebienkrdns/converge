# Production Readiness Example

This example demonstrates how the Validate phase and Production Readiness Review template are applied before releasing a customer-facing feature. It shows a realistic mix of gate results including one conditional pass and one block that is resolved before deployment.

**Scenario:** A checkout flow change introduces a new payment method (buy-now-pay-later via a third-party provider). This is a customer-facing change affecting revenue flow. The decision was approved with conditions in its Decision Cycle. This Production Readiness Review confirms that all conditions are met before the deployment window.

---

## Decision Context

- **Decision Record:** TDR-2024-047 — Introduce BNPL payment option at checkout
- **Prior recommendation:** Approve with conditions
- **Conditions from prior cycle:**
  1. A load test confirming the third-party BNPL provider's API can handle 3× peak checkout volume without timeout degradation. (Owner: Performance Engineer. Deadline: 3 days before deployment.)
  2. A rollback plan rehearsed in staging that removes BNPL from the checkout UI and disables the backend route, verifiable in under 5 minutes. (Owner: SRE. Deadline: 1 day before deployment.)
  3. An alerting rule that fires when BNPL API error rate exceeds 2% over a 5-minute window. (Owner: SRE. Deadline: day of deployment.)

---

## Gate Evaluations

### Security Gate

**Status: Pass**

Evidence on file:
- The BNPL provider integration uses server-side API calls only. No API keys are exposed to the client.
- API key is stored in the secret manager (HashiCorp Vault), not in environment variables or code.
- The payment intent creation endpoint is behind the existing authentication middleware. Unauthenticated requests return 401 before reaching the BNPL integration layer.
- A penetration test was not required for this change: the security surface is the same as the existing payment flow (existing auth, existing data validation). Finding F-001 (BNPL provider's API key rotation policy is unknown) is accepted with rationale: the provider's API documentation specifies automatic rotation on compromise. The key is scoped to checkout write operations only. Key compromise would allow fraudulent BNPL initiations but not data access. Monitoring is in place.

**Finding F-001: Accepted** — API key rotation policy not documented by provider. Risk is bounded: key is scoped, monitored, and rotation is provider-managed. Decision Owner acceptance on file.

---

### Performance Gate

**Status: Pass** (condition resolved)

Prior condition: Load test confirming the BNPL provider API handles 3× peak checkout volume.

Evidence on file:
- Load test completed 2024-11-12. Test parameters: 3× peak checkout rate (120 checkout/min sustained for 30 minutes). BNPL API response time p95: 340ms at peak load. Acceptable threshold: p95 below 500ms.
- The BNPL provider's SLA is 99.9% availability with p95 ≤ 800ms. Test results are within SLA with margin.
- The provider confirmed their infrastructure auto-scales. The test was run at 2 AM (off-peak for the provider) to simulate a conservative scenario.

Condition status: **Resolved**. Performance Engineer sign-off: confirmed 2024-11-13.

---

### Testing Gate

**Status: Pass**

Evidence on file:
- Integration tests cover: successful BNPL initiation and redirect to provider, successful return and order confirmation, failed BNPL authorization (returns user to payment selection), BNPL provider API timeout (returns user to payment selection with error message, no order state corruption), and concurrent payment method selections (only one payment method can be active per order).
- Edge case covered: if the BNPL provider returns a success code but order confirmation fails in our system, a compensation event is emitted and the BNPL authorization is voided.
- No mocked BNPL provider in integration tests — tests use the provider's sandbox environment with recorded session replays for deterministic results.
- Test coverage on the payment critical path: 94%. The 6% uncovered paths are dead code from a prior payment method (now removed) that was not cleaned up; removal is tracked separately.

---

### Observability Gate

**Status: Pass** (condition resolved)

Prior condition: Alerting rule for BNPL API error rate.

Evidence on file:
- Alert configured in Datadog: `bnpl_api_error_rate` > 2% over 5-minute window → PagerDuty page to SRE on-call.
- Additional alert: `bnpl_checkout_completion_rate` drops below 80% of baseline → PagerDuty warning to SRE on-call.
- Dashboard created: `checkout/bnpl-health` showing error rate, latency, completion rate, and order state transitions.
- Structured logs emitted for: BNPL initiation request, provider response (success/failure/timeout), user return from provider, order state transition.
- SRE runbook added for BNPL-specific failure modes: provider outage (disable BNPL route via feature flag, users see payment method unavailable), API timeout (handled by checkout service, no manual intervention needed), and provider returns unexpected success on known-failed authorization (compensation event triggered automatically, alert fires if compensation event fails).

Condition status: **Resolved**. SRE sign-off: confirmed 2024-11-14.

---

### Documentation Gate

**Status: Pass**

Evidence on file:
- Decision Record TDR-2024-047 is complete and stored in the team's documentation system.
- API contract for the BNPL checkout route documented in the internal API reference (request format, response format, error codes, timeout behavior).
- SRE runbook for BNPL failure modes is in the operations wiki.
- The feature flag (`bnpl_payment_enabled`) is documented: what it controls, who can toggle it, and the effect of each state.

---

### Maintainability Gate

**Status: Pass**

Evidence on file:
- The BNPL integration is isolated in a `payments/bnpl` module. The checkout flow imports only the module's public interface.
- The module has a named owner: Payments team lead.
- The BNPL provider's SDK is pinned to a specific version. The payments team owns the upgrade process, with a documented upgrade path in the module's README.
- A future engineer reading the module without context can understand: what it does (BNPL payment initiation), why it exists (TDR-2024-047), how to change it (README), and who owns it (BNPL section in team ownership doc).

---

### Architecture Gate

**Status: Pass**

The BNPL integration follows the existing pattern for third-party payment providers (same auth middleware, same error response format, same compensation event pattern as the existing Stripe integration). No new architectural patterns introduced.

---

## Rollback Plan

**Status: Rehearsed and verified** (condition resolved)

Rollback steps:
1. Toggle feature flag `bnpl_payment_enabled` to `false` via the feature flag service. Effect: BNPL option is removed from the checkout UI and backend route returns 404. No active orders are affected (BNPL orders in progress at time of toggle complete normally).
2. Confirm in Datadog that `bnpl_checkout_completion_rate` metric stops receiving data.
3. Notify customer support of the change.

Rehearsal results (staging, 2024-11-14):
- Step 1 (flag toggle) to Step 2 (metric confirmation): 47 seconds.
- No active order state corruption observed during toggle.
- Customer support notification template prepared.

**Rollback time: under 5 minutes.** Condition status: **Resolved**. SRE sign-off: confirmed 2024-11-14.

---

## Production Readiness Summary

| Gate | Status |
|---|---|
| Security | Pass (F-001 accepted by Decision Owner) |
| Performance | Pass (load test condition resolved) |
| Testing | Pass |
| Observability | Pass (alert condition resolved) |
| Documentation | Pass |
| Maintainability | Pass |
| Architecture | Pass |
| Rollback | Rehearsed — under 5 minutes |

**Deployment status: Approved for production deployment.**

All conditions from TDR-2024-047 are resolved. All gates pass. Deployment window: 2024-11-15, 02:00 UTC (low-traffic period). SRE on-call: [name]. Incident bridge: [link]. Rollback decision owner: SRE on-call.

