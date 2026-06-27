# Testing Gate

## Purpose

Block implementation when the correctness of the decision cannot be verified before or after deployment, or when the decision introduces behavior on high-risk paths that has no test coverage.

## Applies When

The decision introduces new behavior, changes existing behavior in a non-trivial way, modifies data transformation or persistence logic, changes an API contract, or adds a new code path to a system that already has a test suite.

## Approval Conditions

All of the following must be true:

- The acceptance criteria defined in the Understand phase are expressed as testable conditions: given a specific input, the system produces a specific, verifiable output. Criteria expressed only as quality attributes ("the system should be faster") without measurable definitions are not testable.
- Every critical path introduced by the decision — defined as a path whose failure would cause data loss, incorrect output to users, or unavailability — has at least one test that would fail if the path broke.
- Regression risk is addressed: if the decision changes existing behavior, there is evidence that the test suite covers the changed behavior sufficiently that a regression would be detected before production deployment.
- The test strategy is proportional to the risk: a high-risk path through a payment system requires integration tests on real data stores, not only unit tests on mocked dependencies. A low-risk utility function may require only unit tests.
- Tests are part of the delivery, not deferred: test coverage for the decision's new behavior is included in the implementation, not scheduled for a future sprint.

## Blocking Conditions

The gate blocks if any of the following is true:

- The acceptance criteria cannot be verified by any test because they are stated only as qualitative attributes with no measurable definition.
- A new path that handles financial data, personal data, or irreversible operations has no integration test that exercises the actual data store or external system.
- The decision removes or weakens tests that previously covered behavior that is not being intentionally changed.
- A contract change to a public API has no test that confirms existing consumers would not break under the new contract.
- Mocked dependencies in the test suite mask a behavior difference between the mock and the real dependency that is material to the decision.

## Evidence Required

- Test strategy: the types of tests (unit, integration, end-to-end, contract), the scope of coverage, and the rationale for the coverage level chosen.
- Coverage map: for each critical path introduced by the decision, the test or tests that would detect a failure on that path.
- Validation or rollback plan: how the team will verify correctness after production deployment, and how they will roll back if a correctness issue is discovered after deployment.

