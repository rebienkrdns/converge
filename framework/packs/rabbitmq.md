# RabbitMQ Pack

Activates the RabbitMQ Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions involving asynchronous messaging, queue design, exchange topology, or consumer reliability using RabbitMQ.

## Activate When

The decision involves introducing RabbitMQ as a message broker, designing a new exchange and queue topology, modifying consumer acknowledgment behavior, configuring dead-letter exchanges, or evaluating RabbitMQ as an alternative to another messaging system.

## Added Concerns

**Architectural concerns:**
- Is the exchange type (direct, fanout, topic, headers) chosen based on the routing requirements, or by convention? Each type carries different coupling semantics between producers and consumers.
- Is the message contract (schema, versioning, backward compatibility) explicitly designed? A consumer that cannot deserialize a message produced by a newer producer will fail silently unless dead-lettering is configured.
- Are dead-letter exchanges and dead-letter queues configured for every queue that handles messages that must not be lost on processing failure?

**Reliability concerns:**
- Are consumers acknowledging messages only after successful processing, not on receipt? Auto-ack means a message is lost if the consumer crashes between receipt and processing completion.
- Are publishers using publisher confirms for messages that must not be lost in transit?
- Are queues and exchanges declared as durable? Non-durable queues and exchanges do not survive broker restarts.

**Performance concerns:**
- Is the prefetch count set appropriately for the consumer's processing rate? A prefetch count that is too high causes uneven distribution and memory pressure; too low causes consumer underutilization.
- Are large message payloads (above a few kilobytes) appropriate for the broker, or should the payload be stored externally with only a reference in the message?

**Security concerns:**
- Are virtual hosts used to isolate workloads with different trust levels?
- Are user permissions scoped to the minimum queues and exchanges each service requires?
- Is the management API port (15672) accessible only from trusted administrative networks?

**Observability concerns:**
- Are queue depth, consumer count, message rates, and dead-letter queue depth monitored with alerting thresholds?
- Are consumer processing times tracked to detect degradation before queue depth grows?

## Council Interactions

The RabbitMQ Specialist defers to the Security Architect on virtual host isolation and access control, to the SRE on broker clustering and high availability configuration, and to the Software Architect on message contract design and consumer decoupling strategy.
