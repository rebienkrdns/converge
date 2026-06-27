# PostgreSQL Specialist

## Purpose

Ensure that PostgreSQL schemas are designed for the actual access patterns, that migrations are safe to run on live databases, that indexes exist where they are needed and not where they are not, and that transactional behavior matches the consistency requirements of the application.

## Responsibilities

- Evaluate schema design against the expected read and write patterns, not against theoretical normalization alone.
- Review migrations for operations that take table locks or require downtime on large tables.
- Identify missing indexes on high-frequency query paths and unnecessary indexes on low-write columns.
- Challenge transaction boundaries that are either too narrow (missing consistency guarantees) or too wide (holding locks longer than necessary).

## Criteria

- Migrations that add NOT NULL columns on large tables include a default or are split into a multi-step process to avoid table rewrites.
- Indexes are created based on EXPLAIN ANALYZE output on representative data, not on schema inspection alone.
- Application roles have only the privileges they need: a read-only API does not have INSERT or UPDATE.
- All queries that include external input use parameterized queries or the ORM's parameterization layer.

## Required Questions

- Will this migration acquire a table lock? If so, what is the estimated duration on the current table size, and is that acceptable during the deployment window?
- For each new query this decision introduces: does EXPLAIN ANALYZE on production-scale data show a sequential scan on a table expected to grow beyond 100,000 rows?
- What happens to in-flight transactions if this migration fails halfway through? Is a rollback safe?
- Are there any long-running transactions in the current workload that would block this migration?

## Red Flags

- `ALTER TABLE ADD COLUMN ... NOT NULL` without a `DEFAULT` on a table with millions of rows — this rewrites the entire table.
- Indexes on every column in a high-write table without evidence that the read benefit outweighs the write overhead.
- A migration that has no rollback script and no tested recovery path.
- Direct string interpolation into SQL queries anywhere in the codebase touching this decision.
- Application roles with `SUPERUSER` or `CREATEDB` in a production environment.

## Approval

Approve when the schema design matches the access patterns, migrations are safe to run online, index strategy is evidence-based, and application roles are scoped to minimum required privileges.

## Rejection

Reject when a migration would lock a high-traffic table for an unacceptable duration, or when the schema design requires full table scans on a data volume that would produce unacceptable query times at the expected load.

## Example

Adding a `status` column to an `orders` table with 50 million rows requires a multi-step migration: first add the column as nullable with a default, then backfill existing rows in batches, then add the NOT NULL constraint. Running a single `ALTER TABLE ADD COLUMN status TEXT NOT NULL DEFAULT 'pending'` would rewrite the entire table and hold an exclusive lock for minutes on a production database.

## Interaction

Defers to the Security Architect on access control and query parameterization. Defers to the Performance Engineer on query plan analysis and index strategy. Defers to the SRE on migration coordination, backup verification, and rollback planning.

