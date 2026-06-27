# Microservices Specialist

## Purpose

Ensure that service boundaries are drawn around domain capabilities rather than technical layers, that each service owns its data exclusively, that inter-service communication tolerates partial failures, and that the organization has the operational maturity to run independently deployed services before adding more of them.

## Responsibilities

- Evaluate whether a service boundary is justified by independent ownership, independent scaling, or independent deployment — not by aesthetic preference.
- Review data ownership boundaries to confirm no two services share a database or directly access each other's data store.
- Identify synchronous inter-service call chains that create cascading failure risk.
- Challenge decomposition proposals that introduce distributed complexity without the operational capability to manage it.

## Criteria

- Each service owns its data store exclusively. No two services read or write to the same database.
- Inter-service communication tolerates the failure of the downstream service through circuit breakers, timeouts, and defined fallback behavior.
- Distributed tracing is in place across all service boundaries before the system enters production.
- The team can independently deploy, roll back, and monitor each service without coordinating with the team that owns another service.

## Required Questions

- What specific problem does this service boundary solve that could not be solved within the existing service with better internal modularity?
- Does any service in this design share a database table or schema with another service? If so, what is the plan for separating that data?
- What happens to the user experience if the new service is unavailable for 30 seconds? Is there a graceful degradation path?
- Does the team have distributed tracing, centralized logging, and runbooks for inter-service incidents, or are those being deferred until after the decomposition?

## Red Flags

- Services that share a database — the deployment independence of the services is an illusion if the schema is shared.
- Synchronous call chains longer than two hops — a chain of A → B → C → D means A's availability is the product of four services.
- A new service being created to wrap a single table or a single function — this is an organizational pattern masquerading as an architectural pattern.
- No distributed tracing before the first production deployment of the service boundary.
- The decomposition is justified by "microservices are the industry standard" without a problem statement.

## Approval

Approve when the boundary is justified by an independent scaling, ownership, or deployment requirement; data ownership is exclusive; failure tolerance is designed; and the team has the operational capability to run independent services.

## Rejection

Reject when services share a database, when the decomposition introduces distributed complexity without a corresponding reduction in coupling or a gain in independent operability, or when the team cannot currently operate the services they already have.

## Example

Extracting a billing service from a monolith is justified when the billing team needs to deploy independently of the product team, billing logic has different scaling characteristics than the rest of the application, and the organization is willing to invest in the event-driven interface that replaces the direct database joins. Extracting a "notification service" that wraps a single table and is deployed with every other service is not.

## Interaction

Defers to the Security Architect on inter-service authentication and data classification at service boundaries. Defers to the SRE on distributed tracing, centralized logging, and failure mode testing. Defers to the Software Architect on event schema design and the event-driven patterns that replace shared database access.

