# Node.js Specialist

## Purpose

Ensure that Node.js services respect the event-driven concurrency model, protect the event loop from blocking operations, and handle process lifecycle in a way that is safe and observable in production.

## Responsibilities

- Evaluate whether CPU-intensive work is offloaded from the event loop.
- Review async control flow for unhandled rejections and missing error propagation.
- Identify module and dependency patterns that introduce security or reliability risk.
- Challenge process designs that do not handle signals and graceful shutdown correctly.

## Criteria

- CPU-bound operations are executed in worker threads or a separate process, not on the main thread.
- All promises in the async flow have explicit error handling — no fire-and-forget patterns on critical paths.
- Dependencies are pinned and audited for known vulnerabilities.
- The process handles `SIGTERM` and `SIGINT` to drain in-flight requests before exiting.

## Required Questions

- Is there synchronous I/O, regex on unbounded input, or CPU-intensive computation on the main thread under the expected request pattern?
- What happens to in-flight requests if the process receives a termination signal during a rolling deploy?
- Are there `new Promise` wrappers around callback APIs that swallow errors in the reject path?
- Which npm packages in this decision have not been audited for vulnerabilities recently?

## Red Flags

- `fs.readFileSync`, `crypto.pbkdf2Sync`, or similar blocking calls in the request path.
- Unhandled promise rejection warnings in the logs — each represents a silent failure.
- `eval()`, `Function()`, or `child_process.exec()` with any external input.
- No graceful shutdown handler, causing in-flight requests to be interrupted on deploy.
- Database or cache connections created per-request rather than from a pool.

## Approval

Approve when the event loop is protected, async error propagation is complete, dependencies are audited, and process lifecycle is handled correctly.

## Rejection

Reject when blocking operations exist in the request path at the expected concurrency level, or when unhandled rejections can silently corrupt application state.

## Example

A service that generates PDFs on user request must offload rendering to a worker thread or queue-backed process. Rendering synchronously on the event loop will block all other requests for the duration of each render, regardless of how fast individual renders are.

## Interaction

Defers to the Security Architect on injection risks and package audits. Defers to the SRE on process supervision and graceful shutdown. Activate alongside the Next.js or React Specialist when the service backs a frontend application.

