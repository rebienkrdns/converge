# Security Gate

## Purpose

Block implementation when the security risk of the decision has not been sufficiently modeled, when unmitigated critical threats exist, or when the access model violates least privilege.

## Applies When

The decision changes authentication or authorization behavior, introduces a new external input path, changes how secrets or credentials are handled, exposes a new network endpoint, processes personal or financial data, or modifies inter-service trust boundaries.

## Approval Conditions

All of the following must be true:

- A threat model exists that covers every trust boundary crossed by the decision. The threat model must address at minimum the STRIDE categories (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege).
- All identified threats rated Medium or higher have a documented control or an accepted residual risk with a named owner.
- Every IAM role, service account, or permission set introduced by the decision operates on the principle of least privilege. Wildcards on actions or resources require explicit documented justification.
- Secrets and credentials are managed through a secrets management system (environment variables from a vault, managed identity, or equivalent) and are not present in source code, configuration files, Docker image layers, or log output.
- Personal data, financial data, or other classified data handled by the decision is identified, classified, and its handling (collection, storage, transmission, retention, deletion) is documented.

## Blocking Conditions

The gate blocks if any of the following is true:

- No threat model has been produced for a decision that introduces a new external input path or modifies a trust boundary.
- A critical-rated threat has no control and no accepted residual risk.
- Any credential, API key, or secret is present in source code, a committed configuration file, or a container image layer.
- An IAM role or permission set grants `Action: *` or `Resource: *` in a production environment without documented justification reviewed by a Security Architect.
- Personal data is processed without a documented legal basis or retention policy where regulations require one.

## Evidence Required

- Completed security review using the Security Review Template.
- Threat model covering all trust boundaries introduced or modified by the decision.
- Access model: the complete list of IAM roles, permissions, and service accounts with their scope.
- Data handling description: classification, storage location, transmission method, retention period, and deletion procedure for all sensitive data in scope.
- Confirmation that no secrets appear in any artifact that will be stored in version control or a container registry.

