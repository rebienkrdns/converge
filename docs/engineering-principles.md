# Engineering Principles

Converge is governed by a set of principles that define how engineering decisions should be made, evaluated, and maintained. These principles are not a restatement of general best practices. They are the specific commitments that distinguish Converge-based engineering from conventional practice.

Each principle is stated, explained, and paired with the antipattern it is designed to prevent.

---

## I. Decision Traceability

Every recommendation must be fully reconstructible from its inputs, evidence, and evaluation criteria. A decision that cannot be traced back to its reasoning is indistinguishable from a guess.

**Why it matters:** When a decision is revisited six months later — because context changed, a reversal condition was met, or a new team member needs to understand the system — the reasoning must be available without relying on the memory of the people who made it.

**Antipattern prevented:** Decisions that were made in a meeting, recorded only as an outcome, and defended later by appeal to authority or collective memory.

---

## II. Evidence Sufficiency

A decision requires enough evidence to make the risk of being wrong explicit. Sufficiency is not the same as certainty. A decision can proceed with partial evidence if the gaps are documented and the confidence level reflects them.

**Why it matters:** Demanding certainty before deciding paralyzes engineering. But proceeding without knowing what is unknown is reckless. Evidence sufficiency defines the threshold between the two.

**Antipattern prevented:** Decisions that proceed because no one objected, not because the evidence supported them. Also prevents the opposite failure: decisions that are indefinitely delayed waiting for perfect information.

---

## III. Assumption Isolation

Assumptions must be separated from facts and labeled by their decision weight. A high-weight assumption that is false invalidates the decision it supports. A low-weight assumption that is false adjusts a detail. These are not the same.

**Why it matters:** Mixed lists of facts and assumptions produce false confidence. When an assumption is later proven false, it is difficult to know whether the decision still holds. Isolated and weighted assumptions make the decision's fragility visible.

**Antipattern prevented:** A decision record that presents assumptions as established facts, making the decision appear more robust than it is.

---

## IV. Tradeoff Visibility

Every meaningful option must state what it gains and what it sacrifices. An option that appears to have no tradeoffs has not been evaluated carefully enough.

**Why it matters:** Engineering decisions exist because there is no option that is strictly superior in all dimensions. The value of the Converge cycle is finding the best tradeoff for the given constraints, not finding the perfect solution. A decision that does not acknowledge its tradeoffs cannot be maintained when the costs materialize.

**Antipattern prevented:** Decision records that describe the benefits of the chosen option and the costs of the alternatives, presenting an asymmetric comparison that makes the choice appear obvious when it was not.

---

## V. Constraint Priority

Hard constraints outrank preferences, style, and convenience. A preference, however strongly held, cannot override a constraint. A constraint that is negotiable is not a constraint.

**Why it matters:** Constraints represent the boundaries within which the decision must operate: regulatory requirements, contractual obligations, architectural commitments, or operational realities. Violating them does not produce a better decision. It produces a non-compliant decision that will be forced into compliance later at higher cost.

**Antipattern prevented:** Decisions that compromise on regulatory requirements or architectural invariants to achieve a preferred technical outcome, creating compliance debt that surfaces during audits or incidents.

---

## VI. Reverse Failure Analysis

Ask what would make the decision fail after release, not only how it might succeed. Success analysis and failure analysis are not symmetric. A decision that succeeds under expected conditions may still fail under corner cases, scale, organizational change, or time.

**Why it matters:** Engineering tends toward optimism. The systems and arguments that reach the decision stage are already survivors of a selection process that filtered for plausibility. Reverse failure analysis deliberately introduces pessimism to expose the scenarios that optimism missed.

**Antipattern prevented:** Decisions that are validated by showing they work in the happy path, without examining what happens when the dependency is slow, the data is malformed, the team member who built it leaves, or the volume triples.

---

## VII. Domain Calibration

The same evidence threshold should not be applied uniformly across unrelated domains. A performance claim requires different evidence than a security claim. A data model change requires different scrutiny than a configuration change.

**Why it matters:** Uniform thresholds either over-burden low-risk decisions or under-scrutinize high-risk ones. Domain calibration allows the council to allocate scrutiny proportionally to the consequence of being wrong in each domain.

**Antipattern prevented:** Applying a lightweight review process to a security decision because the team applies the same process to all decisions, regardless of domain risk.

---

## VIII. Temporal Awareness

What is safe today may be unsafe after scale, load, or organizational change. A decision that is correct for current conditions may become incorrect as conditions change. The reversal conditions in the decision record must reflect this.

**Why it matters:** Many architectural decisions are made when systems are small and teams are cohesive. The same decisions that worked at ten users may fail at ten thousand. The decisions that worked when the original architect was available may fail when they are not. Temporal awareness asks: for how long is this decision valid, and what changes would invalidate it?

**Antipattern prevented:** Decisions made for current conditions that are treated as permanent commitments, without monitoring for the conditions under which they would no longer hold.

---

## IX. Review Compounding

Each review should add clarity, not merely restate prior conclusions. A second review that produces the same output as the first review has consumed time without creating value.

**Why it matters:** The Converge cycle involves multiple phases and multiple specialists. If each phase only confirms what the previous phase concluded, the cost of the process is not justified by its output. Review compounding requires that each phase has a specific mandate to produce something that was not produced before it.

**Antipattern prevented:** Review cycles that operate as rubber stamps, where later phases simply validate the direction established in early phases without independent scrutiny.

---

## X. Rejection Usefulness

A rejection must make the next attempt better. A rejection that only says "no" without specifying what would be needed to say "yes" is a failure of the review process.

**Why it matters:** The purpose of the Converge cycle is to produce better decisions, not to prevent decisions. A rejection that blocks without guiding creates frustration and repeated failed attempts. A rejection that identifies the specific gap — missing evidence, unmitigated risk, violated constraint — gives the proposer a clear path to a successful second submission.

**Antipattern prevented:** Review processes that are more effective at blocking than at improving. A blocked decision that comes back unchanged is a sign that the first rejection was not useful.

---

## XI. Operational Honesty

If a system cannot be observed, it should not be considered fully understood. A decision about a system that lacks adequate observability is a decision made in the dark.

**Why it matters:** Observability is not an optional enhancement. It is the mechanism by which a system's actual behavior is known rather than assumed. A decision that cannot be validated against production signals after deployment is a decision whose outcome cannot be confirmed.

**Antipattern prevented:** Decisions that are validated by logic and testing but have no production feedback loop, leaving the team unable to confirm whether the decision worked as intended.

---

## XII. Implementation Humility

Architecture should remain adjustable until the evidence for commitment is strong. Early commitment to implementation detail is a form of premature optimization applied to design.

**Why it matters:** The cost of changing a decision increases as implementation progresses. A decision made in the Observe phase that is still adjustable costs nothing to revise. A decision baked into a deployed service costs significantly more. Implementation humility asks: is the evidence strong enough to justify making this decision less reversible?

**Antipattern prevented:** Architectural decisions made early in the cycle and treated as committed before the evidence from Investigate, Challenge, and Debate has been fully processed.

---

## XIII. Proportional Formality

The formality of the decision process must be proportional to the cost of being wrong. A decision that is easily reversible and affects a small scope does not require the same documentation burden as an irreversible decision that affects the entire system.

**Why it matters:** A uniform process applied to all decisions creates overhead that makes low-risk decisions more expensive than the decisions themselves. Proportional formality ensures that the Converge cycle is applied most rigorously where the stakes are highest.

**Antipattern prevented:** Either extreme: applying a full council review to a cosmetic change, or applying no process to a migration that will affect all production data.

---

## XIV. Dissent Preservation

Recorded dissent is more valuable than suppressed disagreement. A decision record that shows unanimous agreement when the discussion was contentious is a falsified record.

**Why it matters:** When a dissenting view turns out to be correct after deployment, the record of the dissent allows the team to learn from it. When dissent is suppressed to create the appearance of consensus, the same disagreement will recur in future decisions, and the opportunity to improve is lost.

**Antipattern prevented:** Decision records that present a false consensus, hiding the reasoning of council members who disagreed with the chosen direction.

---

## XV. Specificity Over Generality

A vague principle, recommendation, or constraint carries less value than a specific one. "Improve performance" is not a success criterion. "Reduce p95 latency for the checkout API from 800ms to below 300ms under the current peak load model" is.

**Why it matters:** Vague statements create the appearance of agreement while leaving the interpretation to the reader. When two readers interpret the same vague statement differently and act on those interpretations, the resulting work diverges in ways that are difficult to reconcile.

**Antipattern prevented:** Decision records with success criteria, recommendations, or constraints that are specific enough to feel meaningful in the meeting but too vague to guide implementation or evaluation.

