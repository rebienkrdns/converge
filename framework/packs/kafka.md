# Kafka Pack

Activates the Kafka Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions involving event streaming, log-based messaging, or stream processing using Apache Kafka or a compatible system (Redpanda, Confluent Platform).

## Activate When

The decision involves introducing Kafka as an event streaming platform, designing a new topic schema, selecting a partition strategy, configuring consumer group behavior, evaluating exactly-once semantics requirements, or choosing between Kafka and a queue-based alternative.

## Added Concerns

**Architectural concerns:**
- Is Kafka the appropriate tool for this use case? Kafka is optimized for high-throughput, ordered, replayable event streams. For task queues with acknowledgment and retry semantics, a traditional message broker may be simpler and more appropriate.
- Is the topic schema designed as a public API? Topics in Kafka are durable and replayable. Every schema change must be backward-compatible, or it will break consumers that replay from an earlier offset.
- Is the partition count designed for the expected throughput and consumer parallelism? Partition count cannot be reduced after creation, and increasing it changes the routing of keyed messages.

**Reliability concerns:**
- Is `acks=all` configured for producers of messages that must not be lost? `acks=1` (the default in some clients) loses messages if the leader fails between write and replication.
- Are consumers committing offsets only after successful processing? Auto-commit at a regular interval can cause re-processing or loss depending on the timing of a consumer failure.
- Is exactly-once semantics (EOS) required? EOS requires idempotent producers and transactional consumers and carries a performance and complexity cost that must be justified.

**Performance concerns:**
- Is the consumer group lag monitored? Growing lag is the primary signal that consumer throughput is insufficient for the producer rate.
- Are producers batching messages appropriately? Very low `linger.ms` values reduce throughput; very high values increase latency.

**Security concerns:**
- Is ACL-based authorization configured to restrict which producers and consumers can read or write each topic?
- Is encryption in transit (TLS) and at rest enabled for topics that carry sensitive data?

**Observability concerns:**
- Are consumer group lag, producer throughput, partition leader distribution, and under-replicated partition counts monitored?
- Are schema registry alerts configured for schema compatibility violations before they reach production?

## Council Interactions

The Kafka Specialist defers to the Security Architect on topic ACLs and data classification, to the SRE on broker configuration, replication factor, and retention policies, and to the Software Architect on event schema design and consumer decoupling.
