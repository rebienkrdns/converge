# Microservices Pack

Activates the Microservices Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions involving service decomposition, inter-service communication, distributed data ownership, or the migration from a monolith to a distributed architecture.

## Activate When

The decision involves extracting a new service from an existing system, establishing a service boundary, choosing a communication protocol between services (synchronous vs. asynchronous), designing a distributed transaction, or evaluating whether a microservices approach is appropriate for a given problem.

## Added Concerns

**Architectural concerns:**
- Is the service boundary drawn around a cohesive domain capability, or around a technical layer? Boundaries drawn around technical layers (a "database service", a "notification service") create high coupling and low cohesion. Boundaries drawn around domain capabilities (a "billing service", an "identity service") encapsulate change.
- Does each service own its data store exclusively? Shared databases between services couple their deployment and evolution. If two services must read the same data, one service should own the data and expose it through an API.
- Is the communication pattern (synchronous REST/gRPC vs. asynchronous events) chosen based on whether the caller needs a response before proceeding? Synchronous calls create temporal coupling. Asynchronous events allow independent scaling but require eventual consistency design.

**Reliability concerns:**
- Is every inter-service call designed to tolerate the failure of the downstream service? Circuit breakers, timeouts, and fallback behaviors must be designed, not assumed.
- Are cascading failure scenarios modeled? A service that calls three others synchronously has availability equal to the product of all four. This must be understood before deployment.

**Operational concerns:**
- Does the organization have the operational maturity to run multiple independent services? Microservices distribute operational complexity. A team that struggles to operate one service will struggle more with ten.
- Is distributed tracing in place across all service boundaries before the system enters production? Without it, debugging cross-service failures is extremely difficult.

**Security concerns:**
- Is every inter-service call authenticated? Services on a private network are not automatically trusted. A compromised service must not be able to call any other service without authorization.
- Is sensitive data classified and its flow through the service mesh documented?

## Council Interactions

The Microservices Specialist defers to the Security Architect on inter-service authentication and data classification, to the SRE on distributed tracing and operational complexity, and to the Software Architect on service boundary definition and data ownership design.
