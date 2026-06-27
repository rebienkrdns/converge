# PostgreSQL Pack

Activates the PostgreSQL Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that affect schema design, query patterns, index strategy, or operational configuration of a PostgreSQL database.

## Activate When

The decision involves a new PostgreSQL schema, a schema migration on a live database, a query pattern that touches more than one table, index creation or removal, replication configuration, partitioning strategy, or connection pool tuning.

## Added Concerns

**Architectural concerns:**
- Is the data model normalized to the appropriate level for the query patterns it must support? Over-normalization forces expensive joins on hot paths. Under-normalization duplicates data and creates consistency risks.
- Are migrations designed to be zero-downtime? Adding a NOT NULL column without a default, or removing a column still referenced by application code, locks the table or breaks the application on deploy.
- Is the schema designed for the expected write pattern? Wide tables with many nullable columns, frequent updates to indexed columns, and high-frequency appends each have different optimal designs.

**Performance concerns:**
- Are indexes created only where the query planner cannot satisfy the query efficiently without them? Unused indexes consume write capacity and storage without benefit.
- Are EXPLAIN ANALYZE outputs reviewed for sequential scans on large tables, nested loop joins on large datasets, and high actual-vs-estimated row count discrepancies that indicate stale statistics?
- Are long-running transactions identified and bounded? Idle-in-transaction connections hold locks and prevent VACUUM from reclaiming dead tuples.

**Security concerns:**
- Does every application role operate with the minimum required privileges? An application that only reads data should not have write access to any table.
- Are prepared statements or parameterized queries used for all queries that include external input?
- Are audit logs enabled for sensitive tables that store personal or financial data?

**Observability concerns:**
- Are `pg_stat_statements` and slow query logs enabled to surface expensive queries in production?
- Are replication lag, connection pool saturation, table bloat, and lock wait events monitored with alerting thresholds?

## Council Interactions

The PostgreSQL Specialist defers to the Security Architect on access control and data classification, to the Performance Engineer on query plan analysis and index strategy, and to the SRE on replication, backup, and recovery design.
