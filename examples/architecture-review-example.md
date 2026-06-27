# Architecture Review Example

This example demonstrates how Converge is applied to a structural architecture decision: resolving coupling in the billing workflow. It shows the Architectural Review template in use alongside the full council evaluation.

**Scenario:** A mid-sized SaaS product has a monolithic backend where the billing workflow is tightly coupled to the user management module. Every billing change requires knowledge of the user module internals. The team has experienced three regressions in the past quarter caused by user module changes that unexpectedly affected billing behavior.

---

## Phase 1: Observe

**Observable facts:**

| Fact | Source | Date |
|---|---|---|
| 3 billing regressions in Q3 caused by user module changes | JIRA incident log | 2024-09-30 |
| Average time to diagnose a billing regression: 4.5 hours | Engineering team incident retrospectives | 2024-09-30 |
| The billing module imports 14 functions directly from the user module's internal packages | Static analysis (dependency-cruiser) | 2024-10-01 |
| 6 of those 14 imports access internal state that is not part of the user module's intended public API | Code review notes, team lead | 2024-10-01 |
| The billing module has no integration tests that isolate it from the user module | CI pipeline test report | 2024-10-01 |

**What is absent:** No data on how often legitimate billing-user integration is needed vs. incidental coupling.

---

## Phase 2: Understand

**Root question:** What is the minimum structural change that prevents user module changes from causing unintended billing regressions, without introducing operational overhead that exceeds the current coupling cost?

**Hard constraints:**
- No changes may break the existing billing API contract (used by 3 downstream consumers).
- The change must be deliverable within 6 weeks by the current team (2 engineers).
- No new infrastructure may be introduced (no new services, no new databases).

**Success criteria:**
- Zero billing regressions attributable to user module changes in the 90 days following the change.
- The billing module imports zero internal functions from the user module — only public API functions.
- Diagnosis time for billing issues drops to under 60 minutes (measured on next incident).

---

## Phase 3: Investigate

**Evidence collected:**

| Item | Type | Quality | Relevance |
|---|---|---|---|
| 14 internal imports identified via dependency-cruiser | Primary | High | Direct — quantifies the coupling surface |
| 6 imports access internal state not in the public API | Primary | High | Direct — these are the regression-causing imports |
| User module has a defined public API surface (3 functions) documented in its README | Primary | High | Direct — a public API already exists, it is not being used |
| Martin Fowler's "Strangler Fig" pattern describes incremental module extraction | Tertiary | Low | Contextual — pattern validation, not directly applicable here |
| Similar coupling reduction on the notification module took 3 weeks for 2 engineers | Secondary | Medium | Relevant for effort estimation |

**Evidence gap:** No data on whether the 6 internal state imports can be replaced by adding new functions to the user module's public API, or whether the billing logic genuinely requires access to internal state that cannot be exposed.

---

## Phase 4: Challenge

**Challenge to framing:** The root question assumes the billing module is the problem. An alternative framing: the user module's public API is inadequate. The billing module is reaching into internals because the public API doesn't provide what billing needs. If that's true, the fix is to expand the user module's public API — not to create a module boundary.

**Result:** The alternative framing is valid for 4 of the 6 internal state imports. Code review shows these 4 access state that could be exposed via new public functions. The remaining 2 access internal implementation details that should not be exposed — they represent genuinely incorrect coupling. Scope is refined: fix the public API gap (4 imports) and restructure the billing module to remove the 2 incorrect couplings.

---

## Phase 5: Debate

**Option A: Refactor inside the monolith — add public API functions to the user module and remove internal imports from billing**

Gains: No new abstraction. Addresses the root cause directly. Deliverable in 2 weeks.
Costs: Does not create a formal module boundary — future developers could re-introduce internal imports. Does not prevent the pattern from recurring.

**Option B: Extract billing into a separate service**

Gains: Hard enforcement of the API boundary. Billing can be deployed independently.
Costs: Violates the "no new infrastructure" constraint. Requires a data synchronization strategy. 10–16 weeks of effort. High operational risk.

**Option C: Introduce a billing module with a defined internal API and an anti-corruption layer**

Gains: Creates a formal module boundary within the monolith. The anti-corruption layer (a thin adapter) translates user module state into billing-domain types. Future internal imports are structurally prevented.
Costs: Requires introducing the adapter abstraction. Adds approximately 1 week beyond Option A.

**Specialist evaluations:**

- Software Architect: Option C is preferred. The anti-corruption layer formalizes the boundary without new infrastructure. Option A is a patch; it fixes today's coupling but not the structural reason the coupling occurred.
- Staff Engineer: Option C is feasible in 3 weeks. The adapter layer is a well-understood pattern and does not require the team to learn new infrastructure.
- Principal Engineer: Option B violates the sprint constraint and introduces distributed system complexity that is disproportionate to the problem size. Eliminated.
- SRE: Option B is out of scope for this decision. If service extraction is revisited in the future, it should be its own decision cycle.

---

## Phase 6: Converge

**Eliminated options:**
- Option B: Violates the "no new infrastructure" hard constraint. Eliminated on constraint grounds.

**Preferred direction:** Option C — introduce a billing module with an internal API and an anti-corruption layer over the user module's public functions.

**Dominant tradeoff:** The adapter abstraction adds one week of implementation in exchange for a structural boundary that prevents the recurrence of internal coupling.

---

## Phase 7: Decide

**Decision statement:** The billing workflow will be restructured as a bounded module within the monolith. A thin anti-corruption layer (adapter) will be introduced to translate user module public API responses into billing-domain types. All 6 internal imports from the billing module to the user module will be removed. For the 4 imports where user module public API gaps exist, new public functions will be added to the user module. For the 2 imports that represent genuinely incorrect coupling, the billing module will be restructured to derive the required state from billing-domain data rather than user module internals.

**Confidence level:** High.

**Confidence basis:** All evidence is Primary and from the current system. The coupling surface is fully mapped (14 imports, 6 internal). The effort estimate is grounded in a comparable prior refactor.

**Reversal conditions:**

| Condition | Monitoring mechanism | Owner |
|---|---|---|
| A billing regression occurs within 90 days traceable to a user module change | JIRA incident label: billing-regression + root cause analysis | QA Lead |
| A new internal import from billing to user module internals appears | dependency-cruiser check added to CI pipeline; fails build on internal import | Staff Engineer |

**Decision owner:** Engineering Lead.

---

## Phase 8: Validate

**Gate evaluations:**

| Gate | Status | Finding |
|---|---|---|
| Architecture | Pass | The anti-corruption layer enforces the boundary within the monolith. No new external dependencies introduced. |
| Maintainability | Pass | The adapter pattern is well-documented. The new public API functions on the user module are named and documented. Ownership is clear. |
| Testing | Conditional Pass | Integration tests isolating the billing module from the user module must be added as part of this delivery. Owner: QA Lead. Deadline: before merge. |
| Security | Not Applicable | No trust boundary changes. No new data access patterns. |
| Performance | Not Applicable | No new I/O paths. Adapter is in-process. |
| Observability | Not Applicable | No new operational surface. Existing monitoring covers both modules. |
| Documentation | Pass | Decision record is this document. The user module's public API documentation will be updated to include the new functions. |

---

## Decision Matrix Summary

| Dimension | Value |
|---|---|
| Quality Score | 9/10 |
| Confidence | High |
| Evidence Status | Complete |
| Impact | Low (internal refactor, no user-facing change) |
| Recommendation | Approve |
| Dominant Tradeoff | One additional week of implementation in exchange for a structural boundary that CI enforces going forward. |
| Executive Summary | The billing module is structurally coupled to the user module's internals through 14 imports, 6 of which access non-public state. This decision introduces a thin anti-corruption layer to enforce a formal module boundary within the monolith, adds public API functions to the user module for the 4 legitimate integration points, and restructures the billing module to eliminate the 2 genuinely incorrect couplings. CI enforcement (dependency-cruiser) prevents recurrence. Approved. Estimated delivery: 3 weeks. |

