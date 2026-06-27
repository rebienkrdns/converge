# Docker Pack

Activates the Docker Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions involving container image design, multi-stage builds, or Compose-based local and production environments.

## Activate When

The decision involves a Dockerfile, a multi-stage build pipeline, a Docker Compose configuration, a container image published to a registry, or the transition of a service from bare-metal or VM deployment to a containerized runtime.

## Added Concerns

**Architectural concerns:**
- Is the container built around a single process or a minimal, well-defined responsibility? Containers that run multiple processes require a process supervisor and lose the operational simplicity that containers are designed to provide.
- Is the image built from a minimal, official base image? Heavier base images increase attack surface, image size, and pull time without benefit.
- Are multi-stage builds used to separate build dependencies from the runtime image? Build tools, source code, and intermediate artifacts must not appear in the production image.

**Security concerns:**
- Does the container run as a non-root user? Running as root inside a container provides unnecessary privilege escalation vectors if the container is compromised.
- Are secrets passed via environment variables at runtime (from a secrets manager), and never baked into the image layer?
- Are image layers scanned for known CVEs before promotion to a production registry?
- Is the base image pinned to a specific digest rather than a mutable tag like `latest`?

**Performance concerns:**
- Are layer cache semantics understood? Instructions that frequently change (copying source code) must appear after instructions that rarely change (installing system dependencies) to maximize cache reuse.
- Is the final image size within acceptable bounds for the deployment target? Large images slow cold-start times in autoscaling environments.

**Observability concerns:**
- Does the container emit logs to stdout and stderr rather than writing to log files inside the container? Container orchestrators and log aggregators consume stdout/stderr directly.
- Is the container configured with a health check that reflects actual readiness to serve traffic, not merely that the process started?

## Council Interactions

The Docker Specialist defers to the Security Architect on image security and secret injection, to the SRE on health check and graceful shutdown design, and to the Kubernetes Specialist when the container is deployed into a Kubernetes cluster.
