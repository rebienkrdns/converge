# Performance Review Template

Use this template when introducing or modifying a component that handles user-facing latency, background throughput, batch processing, or resource-intensive computation. Complete this review before the Performance Gate can pass.

---

## Scope

Describe what is being reviewed: the component, its role, and the performance characteristic under evaluation. Identify whether the concern is latency, throughput, resource consumption, or capacity.

## Load Model

Describe the expected operational load. Be specific about the numbers and their source.

| Metric | Current | Expected at launch | Peak / worst-case | Source |
|---|---|---|---|---|
| Requests per second | | | | |
| Concurrent users | | | | |
| Data volume per request | | | | |
| Background job rate | | | | |
| Infrastructure tier | | | | |

_Guidance: "High traffic" is not a load model. Use actual numbers with a stated basis. If numbers are estimated, state the estimation method._

## Performance Budget

State the acceptable threshold for each performance metric relevant to the scope.

| Metric | Target | Maximum acceptable | Source of requirement |
|---|---|---|---|
| p50 latency | | | |
| p95 latency | | | |
| p99 latency | | | |
| Error rate under load | | | |
| Memory ceiling | | | |
| CPU ceiling | | | |

## Critical Path Analysis

Identify the code paths that carry the highest performance sensitivity. For each critical path:

### Path [N]: [Name]

**Description:** What this path does and when it is invoked.

**Latency breakdown:** Where time is spent across this path (I/O, computation, serialization, network, waiting for a dependency).

**Bottleneck:** The single step that most constrains performance.

**Current measurement:** The actual measured or modeled value.

**Target vs. actual gap:** Whether the current value meets the performance budget.

## Evidence

Document every measurement or benchmark used in this review.

| Evidence item | Type | Date | Method | Value | Meets budget |
|---|---|---|---|---|---|
| | Benchmark / Production metric / Load test / Model | | | | Yes / No |

_Guidance: Evidence from a production system is stronger than a benchmark. A benchmark on representative data is stronger than a synthetic benchmark. All evidence must state its collection method._

## Risk Assessment

Identify the conditions under which the performance budget would be violated.

| Condition | Likelihood | Impact | Mitigation |
|---|---|---|---|
| | | | |

## Conclusion

State whether the performance profile is acceptable under the load model and within the budget. If conditional, state the condition. If not acceptable, state what must change before the Performance Gate can pass.

