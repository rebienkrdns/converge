# Conformance Checklist

This checklist verifies behavioral conformance to the Converge specification. Each item is a behavioral assertion, not a structural inventory. Pass means the described behavior is present and correct. Fail means the behavior is absent, incorrect, or replaced by a structural workaround.

---

## Section 1: Decision Cycle

**1.1 Phase Ordering**
- [ ] The cycle produces phases in the canonical order: Observe, Understand, Investigate, Challenge, Debate, Converge, Decide, Validate, Reflect, Deliver.
- [ ] A phase that is abbreviated has a documented rationale explaining why the evidence made it redundant.
- [ ] No phase is absent without documented rationale.

**1.2 Phase Outputs**
- [ ] Observe produces a set of verifiable facts, not opinions. Each fact has a named source and a date.
- [ ] Understand produces a root question, hard constraints, and measurable success criteria.
- [ ] Investigate produces an evidence table classified by tier (Primary, Secondary, Tertiary) with quality and relevance ratings.
- [ ] Investigate produces a list of evidence gaps: facts that are missing and whose absence affects the decision.
- [ ] Challenge produces at least one reframing of the root question, and records whether the reframing changed the scope.
- [ ] Debate produces at least two candidate options, each with gains, costs, and specialist evaluations.
- [ ] Converge produces a record of eliminated options with elimination reasons, and names the dominant tradeoff.
- [ ] Decide produces a Decision Record that satisfies the schema in the specification.
- [ ] Validate produces a Gate Status for every applicable gate.
- [ ] Reflect produces an outcome-vs-criteria comparison and at least one improvement to the framework or process.

---

## Section 2: Council Behavior

**2.1 Permanent Council Participation**
- [ ] The Permanent Council is activated for every decision cycle, regardless of domain.
- [ ] Each Permanent Council member produces at least one named evaluation, question, or finding. A council member who participates only by endorsing another member's assessment does not satisfy this requirement.
- [ ] When a Permanent Council member raises a concern, the concern is addressed in the record — either resolved with evidence, accepted with rationale, or documented as an open risk.

**2.2 Adaptive Council Activation**
- [ ] The decision activates the Adaptive Council members whose domain is relevant to the decision, as defined in the Pack for that domain.
- [ ] The activation decision is documented: which specialists were activated and why.
- [ ] Adaptive Council members who are not activated are named, and the rationale for non-activation is recorded.

**2.3 Disagreement Handling**
- [ ] When two council members hold contradictory positions, the contradiction is surfaced explicitly in the Debate or Converge phase record.
- [ ] A contradiction is not resolved by omitting one position from the record.
- [ ] The resolution of a contradiction (or the decision to accept an unresolved one) is recorded with its effect on the Confidence level.

---

## Section 3: Engine Outputs

**3.1 Evidence Engine**
- [ ] Evidence is classified as Primary, Secondary, or Tertiary with the criteria defined in the specification.
- [ ] Evidence older than 90 days is downgraded by one tier unless recency is established by the investigation.
- [ ] When two pieces of evidence contradict each other, the contradiction is surfaced as a finding rather than silently selecting one.

**3.2 Confidence Engine**
- [ ] Confidence is derived from evidence tier, assumption weight, disagreement level, and evidence gap count — not asserted.
- [ ] The Confidence level names what would increase or decrease it.
- [ ] A `high` Confidence rating that rests on Tertiary or missing evidence is a defect.

**3.3 Impact Engine**
- [ ] Impact is assessed across four separate dimensions: user impact, blast radius, delivery cost, operational burden.
- [ ] Each dimension has a rated value: Low, Medium, High, or Critical.
- [ ] A `Critical` rating on any single dimension is reflected in the Decision Profile.

**3.4 Convergence Engine**
- [ ] The Convergence Engine eliminates dominated options before the council debates them.
- [ ] An option is dominated if another option is superior on all evaluated dimensions with no compensating advantage.
- [ ] Eliminated options and their elimination reasons appear in the final record.
- [ ] The Convergence Engine names the dominant tradeoff of the selected direction.

**3.5 Decision Matrix Engine**
- [ ] The Quality Score is derived from sub-criteria, not asserted as a single number.
- [ ] The sub-criteria scored are: problem clarity, option quality, evidence strength, risk coverage, and council agreement.
- [ ] The Recommendation is derived deterministically from the Quality Score and Confidence level.
- [ ] An override of the derived Recommendation is documented with the overriding reason and the person responsible.

**3.6 Gate Engine**
- [ ] The applicable gates for the decision are selected by the decision type, not by the council's preference.
- [ ] A `block` gate status halts the cycle. The decision does not advance to Decide while any gate is blocked.
- [ ] A `conditional-pass` gate status names the specific conditions, the owner of each condition, and the deadline for resolution.

**3.7 Recommendation Engine**
- [ ] The final recommendation is one of: `approve`, `approve-with-conditions`, `reject`, `defer`.
- [ ] The recommendation states the basis for the recommendation — not just the conclusion.
- [ ] An `approve-with-conditions` recommendation names the conditions and who owns them.

**3.8 Executive Summary Engine**
- [ ] The Executive Summary is 200 words or fewer.
- [ ] The Executive Summary names the decision, the selected option, the dominant tradeoff, and the confidence level.
- [ ] The Executive Summary does not introduce new information that is not present in the rest of the Decision Packet.

---

## Section 4: Gate Behavior

**4.1 Activation**
- [ ] Each applicable gate produces a status: `pass`, `conditional-pass`, `block`, or `not-applicable`.
- [ ] `not-applicable` is accompanied by the rationale for why the gate does not apply to this decision.

**4.2 Evidence Requirements**
- [ ] A gate that passes has evidence satisfying its approval conditions on file.
- [ ] A gate that passes without evidence is a defect.
- [ ] A gate that issues `conditional-pass` records what evidence must be produced before deployment and by whom.

**4.3 Blocking**
- [ ] A gate that blocks names the specific blocking condition that was triggered (not just "gate failed").
- [ ] The blocking condition is resolvable: the record describes what must change for the block to be lifted.
- [ ] A blocked gate cannot be bypassed. The cycle must either resolve the block or escalate to an explicit risk-acceptance decision by the Decision Owner.

---

## Section 5: Self-Critique

**5.1 Depth**
- [ ] Each of the six critique sections (Assumptions, Blind Spots, Missing Evidence, Counterarguments, Confidence Analysis, Future Risks) contains substantive analysis, not a checklist.
- [ ] The Counterarguments section presents the strongest opposing view — one that a reasonable opponent would actually make.
- [ ] The Confidence Analysis section names the specific factors that set the current confidence level and what single change would move it by a full tier.

**5.2 Effect**
- [ ] The Self-Critique is applied before the Deliver phase. A cycle that delivers without Self-Critique is non-conformant.
- [ ] The Self-Critique changes something in the record: it either updates an assumption, adds a finding, adjusts confidence, adds a reversal condition, or explicitly states that each section was considered and the decision remains unchanged.
- [ ] A Self-Critique that changes nothing and adds no qualifications is evidence that it was performed superficially.

---

## Section 6: Decision Record

**6.1 Completeness**
- [ ] The Decision Record names a Decision Owner who is a specific person (not a team name).
- [ ] The Decision Record lists all options considered, not only the selected option.
- [ ] The Decision Record states the rejection rationale for each non-selected option.
- [ ] The Decision Record lists active assumptions and their monitoring mechanisms.

**6.2 Reversal Conditions**
- [ ] The Decision Record contains at least one reversal condition.
- [ ] Each reversal condition names the observable event that triggers reconsideration, the monitoring mechanism, and the owner.
- [ ] Reversal conditions are not platitudes ("if things go wrong, reconsider"). They are specific, measurable, and owned.

---

## Section 7: Pack Conformance

**7.1 Scope**
- [ ] Each Pack activates only when the decision involves the Pack's domain.
- [ ] The Pack does not redefine canonical matrix dimensions, gate semantics, or required artifact fields.
- [ ] The Pack's specialist activation criteria are documented.

**7.2 Specialist Criteria**
- [ ] Each Pack's specialist has defined Required Questions — questions whose absence is a defect in that specialist's participation.
- [ ] Each Pack's specialist has defined Red Flags — conditions that, if observed, require the specialist to raise a finding.
- [ ] Each Pack's specialist has defined Approval and Rejection criteria specific to the domain.

