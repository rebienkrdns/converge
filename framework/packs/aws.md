# AWS Pack

Activates the AWS Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that introduce, modify, or decommission AWS-managed infrastructure or services.

## Activate When

The decision involves selecting or changing an AWS service, modifying IAM policies, deploying infrastructure via CloudFormation or Terraform on AWS, configuring networking (VPC, security groups, load balancers), or setting cost governance policies.

## Added Concerns

**Architectural concerns:**
- Is the selected AWS service the minimum viable option for the problem? Managed services reduce operational burden but introduce vendor coupling. Is the coupling proportional to the benefit?
- Is the architecture designed for the failure model of the selected services? AWS services fail at the Availability Zone level. Is the design multi-AZ, or is single-AZ failure acceptable?
- Is the data residency and sovereignty model compatible with the applicable regulatory requirements?

**Security concerns:**
- Does every IAM role and policy follow the principle of least privilege? Wildcard actions (`*`) and wildcard resources (`*`) in production policies require explicit justification.
- Are S3 buckets blocked from public access unless explicitly required? Public access must be intentional, documented, and reviewed.
- Are CloudTrail and AWS Config enabled to provide an audit trail for all API calls and configuration changes?
- Are VPC endpoints used for traffic to AWS services that should not traverse the public internet?

**Cost concerns:**
- Are resource sizes chosen based on workload profiling, not on the largest available instance?
- Are reserved instances or savings plans evaluated for workloads with predictable, sustained usage?
- Are unused resources (idle EBS volumes, unattached Elastic IPs, stopped EC2 instances) identified and decommissioned?

**Observability concerns:**
- Are CloudWatch alarms configured for every service metric that affects user-facing reliability?
- Are cost anomaly detection alerts enabled to surface unexpected spending changes before they compound?

## Council Interactions

The AWS Specialist defers to the Security Architect on IAM design and network security, to the SRE on multi-AZ design and failure mode analysis, and to the Performance Engineer on instance sizing and service selection for latency-sensitive workloads.
