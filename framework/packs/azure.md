# Azure Pack

Activates the Azure Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that introduce, modify, or decommission Azure-managed infrastructure or services.

## Activate When

The decision involves selecting or changing an Azure service, modifying Azure RBAC or Entra ID configurations, deploying infrastructure via Bicep or Terraform on Azure, configuring networking (VNet, NSGs, Azure Load Balancer), or setting cost governance policies.

## Added Concerns

**Architectural concerns:**
- Is the selected Azure service appropriate for the workload pattern? Azure offers multiple overlapping options (Azure Functions vs. Container Apps vs. AKS) with different operational models. The choice must be justified by the operational capability the team has, not by familiarity alone.
- Is the architecture designed for regional and availability zone failure? Azure services fail at the availability zone level. Multi-region design requires explicit Active/Active or Active/Passive decisions with defined RTO and RPO targets.
- Are resource groups organized by lifecycle and ownership, not just by project? Resources that are deployed and decommissioned together belong in the same resource group.

**Security concerns:**
- Are Managed Identities used for service-to-service authentication rather than service principal secrets stored in configuration?
- Are Azure Key Vault references used for secrets in App Service, Container Apps, and Function configurations?
- Are Azure Policy and Defender for Cloud enabled to enforce organizational standards and detect misconfigurations?
- Is Entra ID Conditional Access enforced for all human access to Azure management planes?

**Cost concerns:**
- Are Azure Reservations or savings plans evaluated for predictable long-running workloads?
- Are Azure Cost Management budgets and alerts configured to surface spending anomalies before they exceed acceptable thresholds?
- Are Dev/Test subscriptions or pricing used for non-production workloads?

**Observability concerns:**
- Are Azure Monitor and Application Insights configured for all application and infrastructure metrics?
- Are Log Analytics workspaces collecting logs from all critical services with appropriate retention policies?

## Council Interactions

The Azure Specialist defers to the Security Architect on Entra ID configuration and RBAC design, to the SRE on availability zone and regional failure design, and to the Performance Engineer on service selection for latency and throughput requirements.
