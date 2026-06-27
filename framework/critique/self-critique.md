# Self-Critique

Self-critique is the built-in adversarial layer of Converge. It is not a review by an external party — it is the council turning its reasoning against itself before finalizing the decision.

It must be applied after the initial convergence and before the Deliver phase. A decision that skips self-critique skips the one mechanism designed to surface what the council most wants to avoid finding.

---

## What Self-Critique Is Not

Self-critique is not a compliance checkbox. Completing six section headers with one sentence each is not self-critique — it is the appearance of self-critique. The test is whether the output of the critique changed something in the decision record: an assumption reclassified, a confidence rating adjusted, a reversal condition added, a finding elevated. If nothing changed after self-critique, either the decision was already unusually sound, or the critique was performed superficially. The record should state which.

---

## Section 1: Assumptions

**What it must contain:** Every assumption that carries decision weight, classified as load-bearing or low-impact. A load-bearing assumption is one whose falsification would change the recommendation. A low-impact assumption is one whose falsification would require monitoring but not reversal.

**What constitutes shallow work:** Listing assumptions without weight classification. Listing facts as assumptions. Listing assumptions that were already surfaced in the Investigate phase without adding the classification or falsification condition.

**What constitutes genuine work:** Each assumption has a falsification condition (a specific, observable event that would prove it false) and a monitoring mechanism (the signal that would detect that event post-deployment). Load-bearing assumptions that cannot be falsified are reclassified as constraints or risks.

**Genuine example:**
> "Assumption A-002 [high weight]: The N+1 query fix will reduce database CPU to below 45% at peak load. Falsification condition: database CPU remains above 45% during peak hours in the 48 hours following deployment, after the query fix is confirmed deployed. Monitoring: CloudWatch CPU metric on the primary DB instance, alerted at 45% sustained for 10 minutes. If falsified, the rate limiting component addresses traffic volume but not query load — the DB will require vertical scaling or query caching as a next intervention."

**Shallow example to reject:**
> "We assume the fix will work as expected."

---

## Section 2: Blind Spots

**What it must contain:** What the council may not be seeing, and why. Blind spots arise from three sources: (1) data that is unavailable or was not collected, (2) perspectives that are not represented on the council, and (3) domain boundaries the council did not cross during investigation.

**What constitutes shallow work:** Acknowledging that blind spots exist without naming them. Naming blind spots that were already surfaced as findings. Naming blind spots without evaluating their potential impact.

**What constitutes genuine work:** For each blind spot, the critique names the specific gap, the reason the council does not have visibility, and the worst-case outcome if the gap turns out to matter. It also states whether the blind spot is acceptable given the decision's stakes, or whether it requires additional investigation before the decision can be finalized.

**Genuine example:**
> "The council has no visibility into whether the session-based rate limit will be perceived as discriminatory by legitimate high-frequency users (automated test suites, integration partners, internal QA tools). This blind spot exists because the council did not review the client list for the API. Worst case: a production integration partner's automated test suite hits the rate limit and their tests begin failing, causing an escalation. This blind spot is acceptable at the current decision stage because the rate limit can be adjusted by configuration within hours if a legitimate high-frequency user is identified. Mitigation: the reversal condition monitoring includes a review of blocked sessions for session age and API key classification."

**Shallow example to reject:**
> "We may have missed some edge cases."

---

## Section 3: Missing Evidence

**What it must contain:** Facts that are unknown and whose absence affects the decision. Not general uncertainty — specific named facts that were sought but not found, or that were not sought but should have been.

**What constitutes shallow work:** Stating that more evidence would be better. Naming evidence that was already classified as a gap in the Investigate phase without adding the evaluation of whether the decision can proceed safely without it.

**What constitutes genuine work:** For each piece of missing evidence, the critique states: what the evidence is, why it was not collected, the specific risk created by its absence, and whether the decision should proceed anyway (with what safeguards) or should be held pending collection.

**Genuine example:**
> "Missing evidence: the distribution of legitimate request rates by session, broken down by client type (browser, mobile app, internal tool, integration partner). Not collected because the session log analysis focused on aggregate request volume, not per-session rates. Risk: the 500 req/min threshold was derived from the aggregate, not from the tail of legitimate heavy users. If a legitimate client has sessions running at 600–800 req/min, they will be blocked. Decision can proceed with the threshold provisionally set at 500 req/min, but the reversal condition monitoring must include a daily review of blocked sessions classified by client type for the first 14 days. If a legitimate client is identified, the threshold is raised by configuration before the 14-day window closes."

**Shallow example to reject:**
> "We don't have all the data we need, but we'll proceed."

---

## Section 4: Counterarguments

**What it must contain:** The strongest case against the selected option, presented by someone who genuinely believes it. Not a weak objection that the council can easily dismiss. Not a list of risks (risks are covered in their own section). A counterargument is a coherent alternative position that says: "the council is wrong, and here is why."

**What constitutes shallow work:** Repeating the costs of the selected option as counterarguments. Listing objections that were already addressed in the Debate phase. Presenting a counterargument and immediately refuting it in the same sentence.

**What constitutes genuine work:** The counterargument is presented in its strongest form. After presenting it, the critique states why the council's position holds despite the counterargument — not why the counterargument is wrong, but why the council's position is more defensible given the constraints of the decision.

**Genuine example:**
> "Strongest counterargument: the problem is not rate limiting, it is authentication. The `/api/search` endpoint is unauthenticated, which means any rate limiting system based on session tokens is limited to the authenticated portion of traffic. If the scrapers are operating without sessions, session-based rate limiting will not affect them, and the secondary IP-based control (50 req/min per IP) is trivially bypassed by a botnet of ten IPs each staying under the limit. The correct intervention is to require authentication on the search endpoint before implementing rate limiting, which would provide a first-class identity for every request. The council holds its position because requiring authentication on a public search endpoint is a product decision outside the scope of the current sprint and requires coordination with the product team. The combination of session-based and IP-based rate limiting is a short-term control that addresses the current scraper behavior — two ASNs with static IPs — not every possible scraper. This limitation is documented in Reversal Condition 3 (scrapers rotate IPs and IP-based limiting becomes ineffective), and the authentication question is tracked as a future architectural recommendation."

**Shallow example to reject:**
> "Some might argue the solution is too complex, but the council disagrees."

---

## Section 5: Confidence Analysis

**What it must contain:** An explanation of why confidence is at its current level, expressed as the specific inputs that produced it. A statement of what single change would move confidence by a full tier in either direction. Not a restatement of the confidence rating — an analysis of it.

**What constitutes shallow work:** Restating the confidence level without explaining it. Listing the evidence without connecting it to the confidence rating. Saying "confidence is medium because there is some uncertainty."

**What constitutes genuine work:** The analysis names the ceiling factor (the specific evidence gap or assumption that prevents higher confidence) and the floor factor (the evidence that prevents lower confidence). It also identifies the single change that would most efficiently raise confidence, so the council or decision owner can evaluate whether that investment is worth making before proceeding.

**Genuine example:**
> "Current confidence: Medium. Ceiling factor: the session rate threshold (500 req/min) is derived from aggregate session data analysis, not from a controlled experiment on production traffic. This is a secondary evidence source with medium quality. If the threshold is wrong, the first symptom is legitimate users being blocked — which would be detected by the reversal condition monitoring within 24 hours, but would have already affected those users. Floor factor: the scraper identification evidence (78% of elevated traffic from 14 IPs in two data-center ASNs) is primary evidence with high quality, and the Kong rate limiting module is already in production on other endpoints with known overhead. This prevents confidence from falling to Low. What would raise confidence to High: a 7-day session data analysis that confirms no legitimate session exceeds 400 req/min, providing a 20% margin below the proposed threshold. Estimated cost: 2 additional days of analysis. The council recommends proceeding at Medium confidence with the current reversal condition monitoring rather than delaying for the 7-day analysis, because the reversal condition would detect and contain a threshold error within 24 hours of deployment."

**Shallow example to reject:**
> "Confidence is medium because we can't be 100% sure."

---

## Section 6: Future Risks

**What it must contain:** Risks that are not blocking today but could invalidate the decision over time. Not risks that belong in the current risk assessment (those are present-tense risks). Future risks are conditional on changes to the system, the environment, or the actors involved.

**What constitutes shallow work:** Listing risks that are already covered in the decision's gate evaluations. Generic statements about technical debt or changing requirements. Listing risks without a time horizon or triggering condition.

**What constitutes genuine work:** Each future risk names the condition that would activate it, the time horizon in which the condition might occur, the consequence if it occurs, and whether the current decision's reversal conditions would catch it or whether an additional monitoring item is needed.

**Genuine example:**
> "Future risk: the session-based rate limiting approach assumes that the session token is a reliable proxy for user identity. This assumption holds under the current authentication architecture. If the team moves to a stateless JWT-based authentication model (currently under discussion for Q3), the session token structure may change and the Kong rate limiting plugin configuration will need to be updated. Time horizon: 3–6 months. Consequence: if the JWT migration changes the session identifier field and the Kong plugin is not updated, rate limiting will silently stop working for all new sessions. Detection: this risk is not covered by the current reversal condition monitoring. Additional monitoring item required: the platform team must add a review of this decision to the pre-migration checklist for the JWT authentication project. Owner: Engineering Lead, Platform team."

**Shallow example to reject:**
> "Things may change in the future that affect this decision."

---

## Character

Self-critique is a reasoning discipline, not a ritual. Every section should either change something in the decision record or explicitly state that the section was considered and the record is unchanged, and why. A self-critique that changes nothing and states no reason is evidence of superficial execution. The test: could a council member who reads only the self-critique section understand what the council was most worried about and what they decided to do about it?

