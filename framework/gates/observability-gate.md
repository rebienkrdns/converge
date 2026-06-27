# Observability Gate

## Purpose

Block production deployment of any decision that introduces new failure modes that are invisible to the monitoring system, or that would require the operations team to guess at the state of the system rather than observe it directly.

## Applies When

The decision introduces a new service, a new background job, a new inter-service communication path, a new data processing pipeline, or any new component that can fail in ways that affect users or system integrity.

## Approval Conditions

All of the following must be true:

- Every failure mode identified in the decision's risk assessment is either detectable through an existing monitoring signal or has a new monitoring signal defined as part of the delivery.
- Structured logs are emitted for all critical operations in the decision's scope. Structured means machine-parseable: JSON or equivalent format with consistent field names, not free-text strings mixed with request data.
- The success criteria from the Understand phase are measurable in production: the metrics or signals that confirm the criteria are met exist and are collected.
- Alerting rules are configured for failure conditions that require human response. An alert that requires action must have a named on-call owner.
- Operational ownership is defined: a named team or individual is responsible for responding to alerts from the components introduced by this decision.

## Blocking Conditions

The gate blocks if any of the following is true:

- A failure mode exists that would cause user-visible data loss or service unavailability with no detection signal. A failure that is invisible is a failure that will not be discovered until a user reports it.
- Logs for the new components are unstructured or missing entirely, preventing automated log aggregation, search, and alerting.
- No alerting is configured for failure conditions that require response, meaning incidents will be discovered reactively rather than detected proactively.
- The team responsible for the decision cannot name the person or rotation that will respond to an alert from the new components at 2 AM.
- The decision introduces distributed operations across multiple services or processes with no distributed trace that connects a user request to its chain of operations.

## Evidence Required

- Monitoring plan: the metrics, logs, and traces that will be collected, with the tool or platform that collects them.
- Alert definitions: the specific conditions that trigger an alert, the severity, and the owner of each alert.
- Dashboard or runbook reference: the operational resource a responder would use during an incident in the new system.
- Observability gap analysis: for each failure mode in the risk assessment, the signal that would detect it.

