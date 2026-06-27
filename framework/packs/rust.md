# Rust Pack

Activates the Rust Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within Rust systems, libraries, or WebAssembly targets.

## Activate When

The decision involves a Rust service, library, CLI tool, WebAssembly module, embedded system component, or FFI boundary used from another language.

## Added Concerns

**Architectural concerns:**
- Is ownership and borrowing modeled correctly at the API boundary? Types that require runtime borrow checking (RefCell, Mutex) indicate a design that the compiler cannot verify statically. Is this intentional?
- Are `unsafe` blocks used? Every `unsafe` block is a manual contract that the programmer upholds. It must be documented, minimal in scope, and reviewed by someone with Rust expertise.
- Is the crate structure organized around stable public APIs, or are implementation details exposed through `pub` visibility that should be `pub(crate)` or private?

**Performance concerns:**
- Are zero-cost abstractions used throughout the hot path, or are there boxing, dynamic dispatch, or allocation patterns that introduce overhead that the type system could have avoided?
- Is the async runtime chosen (Tokio, async-std) appropriate for the I/O model and the target environment?
- Are binary sizes acceptable for the deployment target? Rust binaries can grow large with many dependencies.

**Security concerns:**
- Are all FFI boundaries treated as trust boundaries? Data received across an FFI call must be validated before use, as the compiler's safety guarantees do not extend across language boundaries.
- Are external inputs that cross into `unsafe` code validated before the crossing?
- Are dependencies audited with `cargo audit` for known vulnerabilities?

**Observability concerns:**
- Are panics caught at the top of each thread or task and reported to the monitoring system? A panic in an async task is silent unless explicitly caught.
- Are structured tracing spans (`tracing` crate) used on the critical path?

## Council Interactions

The Rust Specialist defers to the Security Architect on `unsafe` code review and FFI boundary design, to the Performance Engineer on allocation and async model evaluation, and to the Software Architect on crate boundary and API surface design.
