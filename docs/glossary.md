# Glossary

Definitions are canonical. Implementations may use different surface vocabulary, but must preserve these semantics.

---

**Assumption**
A statement accepted temporarily because evidence is incomplete. Every assumption carries a weight (high, medium, low) that reflects how much the decision depends on it being true. A high-weight assumption whose falsification would change the recommendation must have a falsification condition and a monitoring mechanism. An assumption is not a fact and not a risk — it is the bridge between incomplete evidence and a decision that must be made now.

---

**Blast Radius**
The scope of systems, teams, users, or data that would be affected if the decision's implementation fails. Blast radius is one of the four dimensions of the Impact Engine. A decision that affects a single internal service has a narrow blast radius; a decision that changes a public API contract or a shared database schema has a wide blast radius. Blast radius is not the same as severity — a small feature can have a critical severity if it fails, while having a narrow blast radius.

---

**Confidence Level**
The measured strength of a decision, derived by the Confidence Engine from four inputs: evidence tier, assumption weight, disagreement level, and evidence gap count. Confidence is one of: `high`, `medium`, or `low`. It is derived, not asserted — a council member cannot declare confidence without a derivation. Confidence is not the same as certainty: High confidence means the evidence and reasoning strongly support the recommendation, not that the outcome is guaranteed.

---

**Council**
The group of specialists responsible for evaluating a decision. The Council is divided into the Permanent Council (present for every decision) and the Adaptive Council (activated by domain). The Council does not vote — it evaluates, challenges, and produces findings. The Decision Owner makes the final call, informed by the Council's work.

---

**Decision Cycle**
The ordered ten-phase process through which every decision in Converge passes: Observe → Understand → Investigate → Challenge → Debate → Converge → Decide → Validate → Reflect → Deliver. The cycle is not optional — skipping phases requires documented rationale. The cycle ensures that a decision is understood before it is debated, and validated before it is executed.

---

**Decision Owner**
The named person who is accountable for the decision and its outcomes. Not a team, not a role — a specific individual. The Decision Owner signs the Decision Record, owns the reversal conditions, and is responsible for ensuring that active assumptions are monitored. The Decision Owner may delegate implementation but cannot delegate accountability.

---

**Decision Packet**
The complete structured artifact produced by a Decision Cycle. It contains all findings, assumptions, options, council activations, gate statuses, the Decision Record, the Self-Critique, and the Executive Summary. The Decision Packet is the permanent, auditable record of how and why a decision was made.

---

**Decision Profile**
The structured evaluation artifact produced by the Decision Matrix Engine. It contains the Quality Score, confidence level, evidence status, impact assessment across four dimensions, and the derived recommendation. The Decision Profile is not an opinion — it is the quantified output of the matrix applied to the current evidence.

---

**Dominant Tradeoff**
The one exchange at the center of a decision: what is given up in order to gain what is gained. Every non-trivial decision has a dominant tradeoff. The Convergence Engine is responsible for naming it. A decision record that does not name a dominant tradeoff either chose an option with no cost (unlikely) or did not examine the tradeoffs with sufficient rigor.

---

**Engine**
A deterministic component that transforms specific inputs into a structured output. The eight engines in Converge are: Evidence, Confidence, Impact, Convergence, Decision Matrix, Gate, Recommendation, and Executive Summary. Engines are deterministic — the same inputs produce the same output. When human judgment is applied, it is applied as an input to the engine, not as an override of the engine's output.

---

**Evidence**
Verifiable material used to support or reject a claim. Evidence is classified by tier: Primary (direct, from the system under decision), Secondary (from comparable systems or trusted third-party data), Tertiary (case studies, benchmarks from unrelated systems, expert opinion). The tier classification affects the Confidence derivation. A claim that has no named evidence source is an assumption, not a finding.

---

**Evidence Gap**
A specific fact that is relevant to the decision but was not collected. Evidence gaps are not general uncertainty — they are named, specific, and evaluated for their impact on the decision. The Evidence Engine surfaces gaps; the council evaluates whether each gap is blocking (requires resolution before the decision) or acceptable (can be monitored post-deployment with a defined reversal condition).

---

**Finding**
A single verifiable observation produced during the Decision Cycle. A finding is factual and source-backed — it is not a recommendation, an opinion, or a restatement of a risk. Findings have severity levels (critical, high, medium, low, informational), categories (security, performance, architecture, etc.), and statuses (open, addressed, accepted, deferred). A critical finding that is not addressed must be explicitly accepted with rationale.

---

**Gate**
A blocking checkpoint that prevents implementation from proceeding until specific conditions are met. The seven gates in Converge are: Security, Architecture, Performance, Testing, Observability, Documentation, and Maintainability. A gate produces one of four statuses: pass, conditional-pass, block, or not-applicable. A blocked gate cannot be bypassed by council consensus — it requires the Decision Owner to explicitly accept the risk, which becomes a documented override.

---

**Gate Status**
The recorded result of evaluating a single gate against a specific decision. Gate status includes the gate name, the result (pass/conditional-pass/block/not-applicable), the findings that drove the result, and — for conditional-pass — the specific conditions, the owner of each condition, and the deadline for resolution.

---

**Pack**
A domain-specific extension that adapts the Converge framework for a particular technology or operating context. Packs define which Adaptive Council members to activate, what domain-specific questions the council must ask, what domain-specific Red Flags exist, and how the Council Interactions work in this domain. Packs may add to the framework but must not redefine canonical concepts.

---

**Permanent Council**
The baseline set of ten specialists present in every review, regardless of domain: Principal Engineer, Distinguished Engineer, CTO, Security Architect, Performance Engineer, Software Architect, Staff Engineer, SRE, QA Lead, and Product Architect. The Permanent Council provides the cross-functional evaluation that prevents decisions from being evaluated only by the people closest to the implementation.

---

**Adaptive Council**
The set of domain specialists activated when a decision involves their specific technology or operating context. Adaptive Council members are activated based on Pack criteria, not by default. When activated, they bring domain-specific criteria, Required Questions, and Red Flags to the evaluation. They work alongside the Permanent Council — they do not replace it.

---

**Phase**
A named stage of the Decision Cycle with defined entry conditions, process steps, outputs, and exit conditions. Phases are sequential and their outputs are inputs to subsequent phases. A phase that is abbreviated must produce at minimum its required outputs — abbreviation means less process, not less output. The ten phases are: Observe, Understand, Investigate, Challenge, Debate, Converge, Decide, Validate, Reflect, Deliver.

---

**Quality Score**
An integer from 1 to 10 derived by the Decision Matrix Engine from five sub-criteria: problem clarity, option quality, evidence strength, risk coverage, and council agreement. The Quality Score is not a grade assigned by the council — it is derived from the content of the Decision Packet. A high Quality Score with a low Confidence level produces an `approve-with-conditions` recommendation; a low Quality Score regardless of confidence produces a `defer` or `reject`.

---

**Recommendation**
The conclusion produced deterministically by the Recommendation Engine based on the Quality Score and Confidence Level. One of: `approve`, `approve-with-conditions`, `reject`, `defer`. The recommendation is not an opinion — it is the output of the matrix applied to the evidence. A council member who disagrees with the derived recommendation may log a dissent, but the derived recommendation stands unless the Decision Owner issues a documented override.

---

**Reversal Condition**
A specific, observable event that, if it occurs, requires the decision to be reopened. A reversal condition is not a generic risk statement — it names the condition (the event), the monitoring mechanism (how it will be detected), and the owner (who is responsible for acting when it is detected). Every Decision Record must contain at least one reversal condition. A decision without reversal conditions cannot be safely maintained over time.

---

**Risk**
A credible adverse outcome with meaningful impact and non-trivial likelihood. A risk is different from an assumption: an assumption is something the council believes is true; a risk is something the council believes might go wrong. Risks drive the selection of applicable gates, the content of reversal conditions, and the monitoring requirements post-deployment.

---

**Self-Critique**
The adversarial review layer applied after convergence and before delivery. The council turns its reasoning against itself across six dimensions: Assumptions, Blind Spots, Missing Evidence, Counterarguments, Confidence Analysis, and Future Risks. Self-critique is not a compliance step — it must change something in the record, or explicitly state that each section was considered and the decision is unchanged, and why.

---

**Technical Decision Record (TDR)**
The permanent artifact that captures a final decision. It contains the decision statement, context, options considered, rejection rationale for each non-selected option, consequences (positive and negative), active assumptions, reversal conditions, gate statuses, and the Decision Profile. The TDR is the primary documentation artifact of the Decision Cycle — a decision not captured in a TDR did not happen in a form that can be audited or maintained.

