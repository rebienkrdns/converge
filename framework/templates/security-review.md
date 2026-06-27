# Security Review Template

Use this template when introducing, modifying, or decommissioning any component that handles authentication, authorization, sensitive data, external input, secrets, or inter-service communication. This review must be completed before the Security Gate can pass.

---

## Scope

Describe what is being reviewed. Include the component name, its role in the system, the data it handles, and the trust boundaries it crosses or enforces.

_Guidance: Be specific about what the component does with sensitive data. "Handles user data" is not sufficient. "Receives PII from the registration form, stores it encrypted at rest, and transmits it to the billing service over mTLS" is._

## Assets Under Protection

List every asset that requires protection within the scope of this review.

| Asset | Classification | Owner | Location |
|---|---|---|---|
| | Confidential / Internal / Public | | |

_Asset classification must use the organization's data classification policy. If no policy exists, use: Public, Internal, Confidential, Restricted._

## Trust Boundary Map

Describe each trust boundary the component crosses or enforces. For each boundary, state what is on each side and what the crossing mechanism is.

| Boundary | From | To | Mechanism | Authentication | Authorization |
|---|---|---|---|---|---|
| | | | | | |

## Threat Model

For each credible threat, complete the following:

### Threat [N]: [Name]

**Category:** [STRIDE category: Spoofing / Tampering / Repudiation / Information Disclosure / Denial of Service / Elevation of Privilege]

**Description:** How this threat could materialize in the specific context of the scope.

**Likelihood:** Low / Medium / High — with the basis for the rating.

**Impact:** Low / Medium / High / Critical — with the basis for the rating.

**Current control:** What already exists to prevent or detect this threat.

**Control adequacy:** Whether the current control is sufficient.

## Controls

For each control in place, describe its implementation and its limitations.

| Control | Threat addressed | Implementation | Limitation |
|---|---|---|---|
| | | | |

## Residual Risk

After all controls are applied, describe what risk remains. For each residual risk, state whether it is accepted, mitigated further, or blocking.

| Residual Risk | Accepted / Further Mitigation / Blocking | Owner |
|---|---|---|
| | | |

_A residual risk marked "Blocking" prevents Security Gate approval until resolved._

## Security Gate Readiness

Answer each of the following before presenting to the Security Gate:

- [ ] All assets are identified and classified.
- [ ] All trust boundaries are documented and enforced.
- [ ] All STRIDE categories have been considered, even if the result is "not applicable."
- [ ] No critical threat is uncontrolled.
- [ ] Secrets are not hardcoded, logged, or transmitted in plaintext.
- [ ] All residual risks are explicitly accepted or assigned for further mitigation.

