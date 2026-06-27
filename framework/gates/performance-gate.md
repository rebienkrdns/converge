# Performance Gate

## Purpose

Block decisions that cannot demonstrate a credible path to meeting the performance budget defined in the decision's success criteria, or that introduce unbounded resource growth into a production system.

## Applies When

The decision changes a user-facing latency path, introduces a new background processing workload, modifies database query patterns on tables larger than 100,000 rows, changes resource allocation on shared infrastructure, or adds a new dependency to the critical path.

## Approval Conditions

All of the following must be true:

- A load model exists: the expected request rate, concurrency, data volume, and peak load are stated with the basis for each figure (measurement, estimation method, or analogy). "High traffic" without numbers is not a load model.
- Performance budgets are defined: the acceptable p50, p95, and p99 latency (or equivalent throughput metric) for each path in scope, with the source of the requirement.
- The critical path is analyzed: every step that consumes meaningful time or resources on the critical path is identified, with an estimate of its contribution. The bottleneck — the single step most constraining performance — is named.
- Performance evidence exists at a scale representative of production: measurements from a staging environment on representative data, a load test under the load model, or profiling output from production. Evidence from a developer laptop on synthetic data does not satisfy this condition.
- Headroom exists: the measured or modeled performance at peak load is within budget, with at least 20% headroom before the budget threshold is reached.

## Blocking Conditions

The gate blocks if any of the following is true:

- No performance measurement exists beyond synthetic benchmarks on non-representative data.
- A new database query on a table expected to exceed 500,000 rows shows a sequential scan in EXPLAIN ANALYZE under the expected query pattern, and no index exists or is planned.
- The decision introduces O(n) or worse behavior in a loop that executes per-request, where n is an unbounded user-controlled value.
- Resource consumption (memory, connections, file descriptors) grows without a bound or a defined ceiling enforced by the system.
- The decision adds a new synchronous dependency to the critical path of a user-facing request that has no timeout or circuit breaker.

## Evidence Required

- Load model with stated basis for all figures.
- Performance budget per path: p50, p95, p99 targets with source.
- Critical-path analysis identifying the bottleneck.
- Performance evidence: benchmark results, load test report, or EXPLAIN ANALYZE output on production-scale data.
- Completed Performance Review using the Performance Review Template.

