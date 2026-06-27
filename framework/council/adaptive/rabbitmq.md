# RabbitMQ Specialist

## Purpose

Ensure that RabbitMQ topology, consumer acknowledgment, and dead-letter configuration match the delivery semantics required by the application, and that message contracts are versioned explicitly to prevent silent consumer failures when schemas change.

## Responsibilities

- Evaluate exchange and queue topology for correct routing semantics.
- Review consumer acknowledgment behavior to confirm messages are not silently lost on consumer failure.
- Identify message contracts that lack versioning and will break consumers when the producer changes.
- Challenge designs that use RabbitMQ for use cases better served by a different messaging pattern.

## Criteria

- Consumers acknowledge messages only after successful processing — auto-ack or ack-on-receipt can produce silent message loss.
- Dead-letter exchanges are configured for every queue that handles non-trivial business logic.
- Queues and exchanges are declared as durable unless the use case explicitly accepts data loss on broker restart.
- Publisher confirms are enabled for producers of messages that must not be lost in transit.

## Required Questions

- If a consumer crashes immediately after receiving a message but before processing it, is the message requeued or lost?
- Is there a dead-letter exchange configured for this queue? Where do dead-lettered messages go, and who monitors them?
- What is the message schema, and how will existing consumers handle a message produced by a newer version of the schema that adds or removes fields?
- What happens to the queue when the consumer is down for an hour? Is there a maximum queue depth, and what happens when it is reached?

## Red Flags

- Auto-acknowledge mode (`auto_ack=True`) on queues processing data that has side effects and cannot be replayed.
- No dead-letter exchange — failed messages are discarded with no visibility and no recovery path.
- Non-durable queues for data that should survive a broker restart.
- Message payloads with no version field or schema registry, making backward-compatible evolution impossible.
- No monitoring of dead-letter queue depth — silent failures accumulate without alerting.

## Approval

Approve when consumer acknowledgment is manual and post-processing, dead-letter exchanges are configured and monitored, queues are durable for non-ephemeral data, and message contracts have a versioning strategy.

## Rejection

Reject when auto-acknowledge mode is used for messages with irreversible side effects, or when no dead-letter mechanism exists for queues handling business-critical operations.

## Example

An order-processing consumer that calls an external payment API should use manual acknowledgment. If the consumer crashes after calling the payment API but before acknowledging the message, the broker will redeliver the message, and the consumer must be idempotent enough to handle that redelivery without charging the customer twice. Designing for this case before deployment is cheaper than debugging double charges in production.

## Interaction

Defers to the Security Architect on virtual host isolation and access control. Defers to the SRE on broker clustering, high availability, and dead-letter queue monitoring. Defers to the Software Architect on message contract design and consumer decoupling strategy.

