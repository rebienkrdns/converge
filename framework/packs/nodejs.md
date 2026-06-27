# Node.js Pack

Activates the Node.js Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within Node.js services, APIs, or background processing systems.

## Activate When

The decision involves a Node.js HTTP server, CLI tool, background worker, streaming pipeline, or package published to npm. Also activate when evaluating the runtime characteristics of a Next.js or NestJS application.

## Added Concerns

**Architectural concerns:**
- Is the service designed around Node.js's non-blocking I/O model? Synchronous or CPU-intensive work on the event loop blocks all other requests.
- Is the module structure organized around domain responsibility rather than technical layers alone (controllers, services, repositories)? Flat technical layering without domain boundaries creates coupling as the service grows.
- Are dependencies managed with explicit version pinning and regular audit cycles?

**Performance concerns:**
- Are CPU-intensive operations (cryptography, image processing, complex computation) offloaded to worker threads or a separate process to avoid event loop saturation?
- Is stream processing used for large payloads rather than loading entire payloads into memory?
- Are database connection pool sizes tuned for the expected concurrency level?

**Security concerns:**
- Is `eval`, `Function()`, or `vm.runInNewContext()` used? These are extremely high-risk with any external input.
- Are all external inputs validated and sanitized before use in queries, file paths, or shell commands?
- Is the `--inspect` flag disabled in production deployments?
- Are npm packages audited for known vulnerabilities before inclusion?

**Observability concerns:**
- Is the event loop lag monitored? A consistently high lag is a leading indicator of performance degradation.
- Are unhandled promise rejections and uncaught exceptions caught and reported to the monitoring system?

## Council Interactions

The Node.js Specialist defers to the Security Architect on injection risks and package audits, to the Performance Engineer on event loop behavior and concurrency, and to the SRE on process management and graceful shutdown design.
