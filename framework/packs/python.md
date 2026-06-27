# Python Pack

Activates the Python Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions made within Python services, data pipelines, ML systems, or scripts promoted to production.

## Activate When

The decision involves a Python web service (Django, FastAPI, Flask), a data pipeline, a machine learning system, a scheduled job, or any Python script that is being promoted from experimental use to a production dependency.

## Added Concerns

**Architectural concerns:**
- Is the application structured to avoid the Global Interpreter Lock (GIL) as a scalability constraint? CPU-bound workloads must use multiprocessing or a compiled extension; the GIL prevents true parallel execution in threads.
- Are async frameworks (FastAPI, asyncio) used only where the I/O-bound concurrency model provides measurable benefit? Mixing sync and async code incorrectly introduces subtle blocking.
- Is dependency management reproducible? A `requirements.txt` without pinned versions or a project without a lockfile is not reproducible across environments.

**Performance concerns:**
- Are NumPy or similar vectorized operations used for bulk numerical work rather than Python-level loops?
- Is the database access layer using connection pooling appropriate for the deployment concurrency model?
- Are long-running CPU-bound tasks moved to a task queue (Celery, RQ, Dramatiq) rather than blocking the web process?

**Security concerns:**
- Is `eval()`, `exec()`, or `pickle.loads()` used with external data? These are arbitrary code execution vectors.
- Are all SQL queries parameterized? Python ORMs and raw `cursor.execute()` both support parameterization; string interpolation into SQL is never acceptable.
- Are environment variables used for secrets, or are secrets embedded in source files or config files committed to version control?

**Observability concerns:**
- Are structured logs emitted (using `structlog` or the standard `logging` module with a JSON formatter) rather than `print()` statements?
- Are exceptions captured and routed to an error tracking system (Sentry, Rollbar) rather than silently swallowed?

## Council Interactions

The Python Specialist defers to the Security Architect on injection risks and secret management, to the Performance Engineer on concurrency model selection, and to the Data or AI Specialist when the decision involves ML model serving or pipeline design.
