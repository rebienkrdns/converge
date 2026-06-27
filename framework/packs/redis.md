# Redis Pack

Activates the Redis Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that introduce or modify Redis as a cache, session store, message broker, rate limiter, or distributed lock provider.

## Activate When

The decision involves introducing Redis into the stack, changing the eviction policy, adding a new data structure or Lua script, modifying keyspace design, or changing the persistence, replication, or clustering configuration.

## Added Concerns

**Architectural concerns:**
- Is Redis being used for the right class of problem? Redis is optimized for in-memory, low-latency access to volatile or semi-volatile data. Using it as a primary durable store for data that requires strong consistency guarantees is a misuse of the tool.
- Is the keyspace designed with namespacing that prevents collisions between different services or use cases sharing the same instance?
- Is data stored in Redis reconstructible from the primary data store if the instance is flushed? If not, Redis is acting as a source of truth, which requires a persistence and replication strategy appropriate for that role.

**Performance concerns:**
- Are operations on large sorted sets or hash maps profiled under expected cardinality? O(log N) and O(N) commands on large key spaces can block the single-threaded command loop.
- Is connection pooling used on the application side? Opening a new connection per request defeats the latency advantage of Redis.
- Is the memory footprint of the keyspace understood and bounded? An unbounded keyspace will exhaust available memory and trigger eviction or OOM behavior.

**Security concerns:**
- Is Redis exposed only within a private network, behind authentication? Redis was designed for trusted networks and has historically had no authentication by default.
- Is `CONFIG SET` and `DEBUG` command access restricted in production?
- Are sensitive data values encrypted before storage, since Redis data-at-rest protection depends on the underlying infrastructure?

**Observability concerns:**
- Are cache hit and miss rates monitored? A degrading hit rate is an early signal of keyspace exhaustion, eviction, or TTL misconfiguration.
- Are eviction events, connection count, and memory usage monitored with alerting thresholds?

## Council Interactions

The Redis Specialist defers to the Security Architect on network exposure and access control, to the Performance Engineer on connection pool sizing and command complexity, and to the SRE on persistence, replication, and failover configuration.
