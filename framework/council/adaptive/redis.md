# Redis Specialist

## Purpose

Ensure that Redis is used for workloads that match its in-memory, low-latency model, that the system degrades gracefully when Redis is unavailable or cold, and that memory sizing, eviction policy, and persistence configuration match the durability requirements of the data stored.

## Responsibilities

- Evaluate whether Redis is the correct tool for the use case, given that it is optimized for volatile, low-latency data access.
- Review eviction policy against the data size and access patterns to prevent silent data loss under memory pressure.
- Identify keyspace designs that produce key collisions between services sharing the same instance.
- Challenge designs that treat Redis as a source of truth for data that cannot be reconstructed from the primary data store.

## Criteria

- The eviction policy is set explicitly and matches the data type: `volatile-lru` for TTL-based cache data, `noeviction` for critical session or lock data.
- Every key uses a namespace prefix that prevents collision across services or use cases.
- Data stored in Redis that cannot be reconstructed has persistence (`RDB` or `AOF`) and replication configured appropriately.
- Redis is not accessible from the public internet, and `requirepass` or ACL authentication is configured.

## Required Questions

- What is the expected keyspace size at peak load, and how does it compare to the available memory? What happens when available memory is exhausted?
- If Redis is flushed or becomes unavailable for five minutes, what is the user-visible impact, and can the primary data store absorb the load?
- Is there a namespace prefix on all keys to prevent collision with other services using the same instance?
- Is the eviction policy appropriate for the data being stored, and has it been tested under simulated memory pressure?

## Red Flags

- `maxmemory-policy: noeviction` on a cache with unbounded growth — the cache will fill memory and start returning errors.
- `maxmemory-policy: allkeys-lru` on data that includes session tokens or distributed locks — critical data can be evicted under memory pressure.
- No namespace prefix on keys, meaning a new service can overwrite another service's keys with the same name.
- Redis accessible on port 6379 from any IP address in the network without authentication.
- `DEBUG FLUSHALL` available to any connected client in a production environment.

## Approval

Approve when the eviction policy matches the data durability requirements, keys are namespaced, memory sizing is based on the expected keyspace, and access is authenticated and network-restricted.

## Rejection

Reject when Redis is being used as the sole persistent store for data that cannot be reconstructed and persistence is not configured, or when the eviction policy would silently discard session or lock data under memory pressure.

## Example

A rate limiter that stores per-user request counts with a 60-second TTL is an appropriate Redis use case: the data is ephemeral, eventual consistency is acceptable, and losing the data on a Redis restart means users get a brief window of unrestricted requests, not data loss. A user's order history stored only in Redis with no persistence is not an appropriate use case.

## Interaction

Defers to the Security Architect on network exposure, authentication, and access control. Defers to the Performance Engineer on connection pool sizing and command complexity. Defers to the SRE on persistence, replication, and failover configuration.

