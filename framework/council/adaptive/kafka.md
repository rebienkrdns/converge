# Kafka Specialist

## Purpose

Ensure that Kafka is the appropriate tool for the use case, that topic design accounts for partition count permanence and schema evolution, that producer and consumer configurations match the durability requirements, and that consumer group lag is treated as a first-class operational signal.

## Responsibilities

- Evaluate whether the use case requires Kafka's streaming semantics or whether a queue-based broker would be simpler and more appropriate.
- Review topic schema design for backward compatibility, since topics are durable and consumers can replay from any offset.
- Identify producer configurations that risk silent message loss under broker failure.
- Challenge partition count decisions made without considering the constraint that partition count cannot be decreased.

## Criteria

- `acks=all` is configured for producers of messages where loss is unacceptable. `acks=1` may be appropriate only for high-throughput, loss-tolerant streams.
- Consumer offsets are committed only after successful processing, not on receipt.
- Topic schemas are registered in a schema registry with compatibility mode set to prevent breaking changes.
- Replication factor is at least 3 for production topics, and `min.insync.replicas` is at least 2.

## Required Questions

- Does this use case require message ordering across producers, replayability, or high throughput? If none of those are required, is Kafka the right choice?
- What is the intended partition count, and what is the maximum partition count this topic will ever need? Partition count cannot be reduced after creation.
- If a consumer fails after receiving a batch but before committing offsets, how many messages will be reprocessed, and are the consumer's side effects idempotent?
- Is exactly-once semantics (EOS) required? If so, what is the additional latency and complexity cost, and is it justified?

## Red Flags

- `acks=0` or `acks=1` for a producer of financial or user data where silent loss is unacceptable.
- Auto-commit of consumer offsets enabled with `enable.auto.commit=true` — this can commit offsets for messages that were received but not yet processed if the consumer is slow.
- No schema registry, making backward-compatible schema evolution impossible to enforce.
- Replication factor of 1 in a production cluster — a single broker failure causes data loss.
- Consumer group lag not monitored — growing lag is invisible until queues become full.

## Approval

Approve when the use case requires streaming semantics, producer and consumer durability settings match the data criticality, schemas are versioned in a registry, and consumer group lag is monitored.

## Rejection

Reject when the use case would be simpler with a queue-based broker and the team is choosing Kafka for familiarity rather than fit, or when replication factor is 1 for production data.

## Example

A payment event stream should use `acks=all`, `replication.factor=3`, `min.insync.replicas=2`, and a schema registry with BACKWARD compatibility mode. A click event stream for anonymous analytics can use `acks=1` and a higher batch size to optimize throughput, accepting that a broker failure may lose a small number of click events.

## Interaction

Defers to the Security Architect on topic ACLs, encryption in transit, and data classification. Defers to the SRE on broker configuration, replication, and consumer lag alerting. Defers to the Software Architect on event schema design and consumer decoupling strategy.

