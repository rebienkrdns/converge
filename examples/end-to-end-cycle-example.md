# End-to-End Cycle Example

This example traces a complete Converge cycle for a realistic engineering decision. It demonstrates every phase, the council interactions, the engine outputs, the gate evaluations, and the final decision record.

**Scenario:** A high-traffic e-commerce platform needs to introduce rate limiting on its public product search API. The API currently has no rate limiting, and a spike in scraper traffic is causing elevated latency for legitimate users.

---

## Phase 1: Observe

**Trigger (original form):** "Search is slow. I think bots are hammering the API."

**Observable facts collected:**

| Fact | Source | Date |
|---|---|---|
| p95 latency for `/api/search` increased from 120ms to 580ms over the last 14 days | Datadog APM | 2024-03-15 |
| Request rate on `/api/search` increased from 800 req/s to 3,200 req/s over the same period | NGINX access logs | 2024-03-15 |
| 78% of the new traffic originates from 14 IP addresses, all belonging to two ASNs | Log analysis | 2024-03-15 |
| Legitimate user sessions (identified by session cookie) are experiencing 3.4x the latency of 14 days ago | RUM data | 2024-03-15 |
| The database server CPU is at 87% average during peak hours, up from 31% | CloudWatch | 2024-03-15 |

**What is absent:** No visibility into what the scrapers are doing with the data. No previous incident of this type.

**Affected stakeholders:** End users (degraded search experience), Operations team (elevated on-call load), Business (potential revenue impact from degraded conversion).

**Initial scope boundary:** The `/api/search` endpoint and its upstream dependencies. The product catalog database. Out of scope: other API endpoints, the authentication system, the scraper actors themselves.

---

## Phase 2: Understand

**Root question:** What is the minimum intervention that reduces scraper-driven latency to an acceptable level for legitimate users without introducing a new class of operational complexity?

**System boundary:** The public API gateway, the `/api/search` route handler, the full-text search service (Elasticsearch), and the PostgreSQL product catalog that backs it.

**Stakeholders and impact:**
- End users: experiencing 580ms p95 on search. Acceptable threshold: 200ms p95.
- Operations: currently receiving two to three pages per day related to elevated DB CPU. Acceptable threshold: zero pages from this cause.

**Hard constraints:**
- No changes to the product catalog schema (a migration on this table requires a 4-hour maintenance window, which is not approved for this quarter).
- The solution must not block legitimate users. Rate limiting by IP alone is insufficient because some legitimate traffic comes from corporate proxies that appear as a single IP.
- The solution must be deployable within the current sprint (5 days).

**Success criteria:**
- p95 latency on `/api/search` returns to below 200ms within 24 hours of deployment.
- Zero legitimate user sessions blocked based on a 7-day post-deployment sample.
- DB CPU returns to below 45% average during peak hours.

---

## Phase 3: Investigate

**Evidence collected:**

| Item | Type | Quality | Relevance |
|---|---|---|---|
| 78% of elevated traffic comes from 14 IPs | Primary (log analysis) | High | Direct — identifies the source |
| 14 IPs belong to two ASNs (both are data center ASNs, not residential ISPs) | Primary (ASN lookup) | High | Direct — distinguishes scrapers from residential users |
| Elasticsearch receives 3,200 req/s; its auto-scaler is not triggered because CPU on Elasticsearch nodes is only 42% | Primary (CloudWatch) | High | Direct — bottleneck is in PostgreSQL, not Elasticsearch |
| PostgreSQL query for product enrichment (called after Elasticsearch search) runs one query per search result, up to 20 per request | Primary (query log) | High | Direct — identifies N+1 as a compounding factor |
| Token bucket rate limiting is available as a module in the existing API gateway (Kong) | Primary (Kong documentation and current config) | High | Direct — establishes feasibility |
| Similar e-commerce platform published a case study describing 5ms overhead per request for token bucket evaluation at their scale | Tertiary (public case study) | Low | Contextual — overhead estimate, not from our system |

**Evidence gaps:**
- No data on whether rate limiting by session token would correctly identify legitimate users behind corporate proxies. Requires a test against 7 days of real session data.

**Assumption list:**
- [High weight] The 14 scraper IPs will not change their IPs in response to IP-based blocking. If false, the solution requires additional mechanism.
- [Medium weight] The N+1 query can be resolved with eager loading within the sprint. If false, the DB CPU improvement may be smaller than expected.
- [Low weight] Kong's rate limiting module adds no more than 20ms to request latency at our traffic volume.

---

## Phase 4: Challenge

**Challenge to root question:** The framing is "rate limiting." An alternative framing is "query optimization" — the N+1 pattern means each scraper request produces up to 20 database queries. If the N+1 is resolved, the 3,200 req/s load produces the same DB load as 160 legitimate req/s. This may resolve the CPU problem without any rate limiting.

**Result of challenge:** The alternative framing is valid but insufficient alone. Even with the N+1 resolved, 3,200 req/s on a public endpoint with no access control is an ongoing risk. The two interventions are complementary, not alternatives. The root question is revised: what combination of rate limiting and query optimization resolves the immediate latency problem and prevents recurrence?

**Scope adjustment:** Add the N+1 query pattern to the in-scope problem definition.

**Blind spot identified:** The council did not initially consider that scraper traffic, once slowed, may increase its IP diversity. The high-weight assumption that scrapers will not rotate IPs must be treated as a monitoring item post-deployment.

---

## Phase 5: Debate

**Candidate options:**

**Option A: IP-based rate limiting via Kong (100 req/min per IP)**
Gains: deployable in hours, uses existing infrastructure, no code change.
Costs: blocks legitimate corporate users behind shared IPs, does not address the N+1 query.

**Option B: Session-token-based rate limiting via Kong (500 req/min per session) + N+1 fix**
Gains: does not block legitimate corporate users, addresses root DB load, uses existing infrastructure.
Costs: requires 7-day session data analysis to validate thresholds, requires a code change for N+1 fix, approximately 3 days of implementation.

**Option C: Dedicated scraper detection service with ML-based classification**
Gains: most precise, handles IP rotation and distributed scraping.
Costs: 4–6 weeks to build, requires new operational component, exceeds sprint constraint.

**Specialist evaluations:**

- Security Architect: Option B preferred. IP-based limiting (Option A) is trivially bypassed by IP rotation, creating false confidence. Session-token limiting is harder to bypass without access to a valid session.
- Performance Engineer: Option B with N+1 fix is the only option that addresses the root cause of DB CPU load. Option A would reduce request volume but not fix the query pattern, so CPU may remain elevated.
- SRE: Option C is out of scope for the sprint and introduces a new failure mode. Option B is feasible within the sprint if the session data analysis starts immediately.
- Software Architect: The N+1 fix in Option B is independently valuable and should be implemented regardless of the rate limiting choice.

---

## Phase 6: Converge

**Eliminated options:**
- Option A: dominated by Option B on security and performance dimensions, with no compensating advantage except slightly faster deployment. Eliminated.
- Option C: violates the sprint constraint. Eliminated.

**Preferred direction:** Option B — session-token-based rate limiting at 500 req/min per session, combined with N+1 query resolution.

**Dominant tradeoff:** The session data analysis adds approximately 2 days to the implementation timeline. In exchange, the solution does not block legitimate corporate users and addresses the root cause of the DB load.

**Unresolved conflict:** None. The council agrees on Option B.

---

## Phase 7: Decide

**Decision statement:** The `/api/search` endpoint will be rate limited at 500 requests per minute per authenticated session token using the existing Kong rate limiting plugin. Unauthenticated requests will be rate limited at 50 requests per minute per IP as a secondary control. Concurrently, the N+1 product enrichment query will be replaced with a single batched query in the same sprint.

**Confidence level:** Medium.

**Confidence basis:** Primary evidence supports the rate limiting approach and the N+1 fix. Medium rather than High because the session threshold (500 req/min) is based on statistical analysis of historical session data, not a controlled experiment. The threshold may need adjustment after deployment.

**Alternatives considered:**
- Option A (IP-only rate limiting): rejected because it blocks legitimate corporate users and does not address DB load.
- Option C (ML-based scraper detection): rejected because it exceeds the sprint constraint.

**Reversal conditions:**

| Condition | Monitoring mechanism | Owner |
|---|---|---|
| Legitimate session blocked rate exceeds 0.1% of sessions in 7-day sample | RUM data — daily review for 7 days post-deployment | SRE on-call |
| Scrapers rotate IPs and the 14 known IPs are no longer responsible for elevated load | NGINX log analysis — daily review for 14 days | Platform team |
| p95 latency does not return to below 200ms within 24 hours of deployment | Datadog alert | SRE on-call |

**Active assumptions:**
- Scrapers will not immediately rotate session tokens in response to session-based limiting. Monitoring: session blocking rate by session age — a spike in very-new sessions being blocked would indicate token rotation.

**Decision owner:** Engineering Lead, Platform team.

---

## Phase 8: Validate

**Gate evaluations:**

| Gate | Status | Finding |
|---|---|---|
| Security | Pass | Rate limiting at the API gateway does not introduce new trust boundaries. Session tokens are already validated upstream. Unauthenticated IP limiting is a defense-in-depth measure, not a primary auth control. |
| Performance | Conditional Pass | Session data analysis must confirm the 500 req/min threshold before deployment. Owner: Platform team. Deadline: 2 days before deployment window. |
| Architecture | Pass | Rate limiting via Kong is consistent with the existing API gateway usage. No new component is introduced. |
| Testing | Pass | Kong rate limiting module is already in use on other endpoints. The N+1 fix requires an integration test that confirms the batched query produces identical results to the per-item query for all edge cases (empty result sets, products with missing enrichment data). |
| Observability | Pass | Kong emits rate limit hit metrics by default. An alert will be configured for rate limit hits exceeding 1% of total requests. The N+1 fix will be verified by a drop in total DB query count per request in the query log. |
| Documentation | Pass | This decision record is the documentation. The Kong rate limiting configuration will be added to the team's infrastructure documentation. |
| Maintainability | Pass | Both changes are additive to existing systems. The N+1 fix reduces complexity. No new abstraction is introduced. |

**Implementation feasibility:** Confirmed. Both changes are within the team's current capability and timeline.

---

## Phase 9: Reflect

*(Completed 14 days after delivery)*

**Outcome vs. success criteria:**
- p95 latency: 140ms (target: below 200ms). Met.
- Legitimate users blocked: 0.04% (target: below 0.1%). Met.
- DB CPU at peak: 38% average (target: below 45%). Met.

**Reversal condition check:** Scrapers did not rotate IPs or session tokens within the 14-day observation window.

**Evidence quality retrospective:** The session data analysis used to derive the 500 req/min threshold was valuable and should be a standard step for future rate limiting decisions. The Tertiary evidence on Kong overhead (from the case study) was unnecessary — the overhead in production was 3ms, well below the 20ms estimate, and could have been dismissed without referencing the case study.

**Assumption accuracy:** The high-weight assumption (scrapers would not rotate IPs immediately) held for 14 days. The monitoring mechanism for session token rotation should remain active for 30 more days.

**Improvement identified:** The Investigate phase should include a step to check for N+1 patterns on the affected endpoint as a standard investigation item for performance-related decisions. This would have surfaced the N+1 earlier in the cycle, potentially changing the initial framing. Assigned to: QA Lead, to add to the investigation checklist.

---

## Decision Matrix Summary

| Dimension | Value |
|---|---|
| Quality Score | 8/10 |
| Confidence | Medium |
| Evidence | Conditional (session threshold requires post-deployment validation) |
| Impact | Medium (user-facing latency, DB load) |
| Recommendation | Approved with conditions |
| Executive Summary | The `/api/search` endpoint will be rate limited at the session level using the existing Kong plugin, combined with a batched product enrichment query to address the root DB load. This is conditionally approved pending confirmation of the session rate threshold from 7-day session data analysis. The primary tradeoff is 2 days of additional analysis time in exchange for a solution that does not block legitimate corporate users. Reversal conditions are monitored for 30 days post-deployment. |
