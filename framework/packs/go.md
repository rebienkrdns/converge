# Go Pack

Activates the Go Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within Go services, libraries, or CLI tools.

## Activate When

The decision involves a Go service, API, background worker, CLI tool, or library. Also activate when evaluating Go usage within a polyglot system where the Go component handles a performance-critical or infrastructure-critical path.

## Added Concerns

**Architectural concerns:**
- Are interfaces defined at the consumer, not the producer? An interface defined where it is implemented couples callers to the implementation package.
- Are goroutines supervised? A goroutine that panics and is not recovered from can bring down the process. Goroutines that are started but never stopped leak resources.
- Is context propagation consistent? Context cancellation must flow through every I/O call for timeouts and cancellation to work correctly.

**Performance concerns:**
- Are allocations on the hot path minimized? Excessive heap allocations increase GC pressure and tail latency.
- Are sync.Pool instances used for expensive-to-allocate objects that are frequently reused?
- Are channel operations in the critical path benchmarked? Channel contention can become a bottleneck under high concurrency.

**Security concerns:**
- Are all external inputs validated before use in SQL queries, file paths, or os/exec calls?
- Is `math/rand` used for any security-sensitive operation? Only `crypto/rand` is appropriate for secrets, tokens, and nonces.
- Are TLS configurations using the standard library defaults or explicitly setting minimum TLS version and cipher suites?

**Observability concerns:**
- Are goroutine counts and heap allocations exposed via `runtime/pprof` or an equivalent profiling endpoint?
- Are structured logs (using `slog` or an equivalent) used rather than `fmt.Println`, to allow log parsing and filtering in production?

## Council Interactions

The Go Specialist defers to the Security Architect on cryptography and input validation, to the Performance Engineer on allocation patterns and concurrency design, and to the SRE on process lifecycle and graceful shutdown behavior.
