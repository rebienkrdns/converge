# Kubernetes Pack

Activates the Kubernetes Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that affect workloads, operations, or infrastructure managed by Kubernetes.

## Activate When

The decision involves deploying, scaling, or modifying a workload on Kubernetes; changes to resource requests and limits; rollout strategies; cluster networking or ingress; RBAC policies; storage classes; or Helm chart design.

## Added Concerns

**Architectural concerns:**
- Are resource requests and limits set based on observed profiling data, or are they defaults that risk OOM kills or CPU throttling under real load?
- Is the deployment strategy (RollingUpdate, Recreate, Blue/Green via traffic splitting) chosen based on the service's tolerance for version skew and the cost of a failed rollout?
- Are workloads stateless, or is shared state managed via volumes, ConfigMaps, or Secrets with appropriate persistence and access controls?

**Security concerns:**
- Are Pods running with the minimum required privileges (non-root, read-only root filesystem, dropped capabilities)?
- Are Secrets stored in an external secret management system (Vault, AWS Secrets Manager, Sealed Secrets) rather than as plaintext in the cluster or in version control?
- Are RBAC roles scoped to the minimum namespace and resource set required for each service account?
- Are network policies in place to restrict inter-pod communication to declared paths?

**Performance and reliability concerns:**
- Are Pod Disruption Budgets defined to prevent simultaneous eviction of too many replicas during node drain or cluster upgrades?
- Are readiness and liveness probes configured correctly so that the load balancer does not route to Pods that are not ready to serve?
- Is horizontal autoscaling configured based on meaningful metrics (CPU utilization, request queue depth) rather than time-based schedules?

**Observability concerns:**
- Are resource consumption metrics (CPU, memory, throttling events) surfaced at the Pod and namespace level?
- Are cluster events and control plane logs retained and searchable?

## Council Interactions

The Kubernetes Specialist defers to the Security Architect on RBAC and Secret management, to the SRE on rollout strategy and reliability margins, and to the Performance Engineer on resource sizing. Activate the Docker Pack concurrently when the decision involves container image design or multi-stage builds.

