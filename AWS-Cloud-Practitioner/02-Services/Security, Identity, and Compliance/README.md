# Security, Identity, and Compliance — Category Overview

Security is Domain 2 of the Cloud Practitioner exam (30% of questions). This category is the most heavily tested. You need to know what each service does at a conceptual level — not deep technical implementation.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| AWS IAM | Identity and access management | Users, roles, policies, least privilege |
| AWS IAM Identity Center | Single sign-on for multiple accounts | Centralized access to all accounts/apps |
| Amazon Cognito | Identity for web/mobile app users | User pools and identity pools |
| AWS Directory Service | Managed Microsoft Active Directory | AD in AWS |
| AWS Artifact | Compliance documentation portal | Download AWS compliance reports |
| AWS Audit Manager | Continuous compliance evidence collection | Automate audit preparation |
| AWS Certificate Manager (ACM) | SSL/TLS certificates | Free managed certificates |
| AWS CloudHSM | Hardware security module | Customer-controlled key storage |
| AWS KMS | Key management for encryption | Managed encryption keys |
| AWS Secrets Manager | Store and rotate secrets | Automatic secret rotation |
| AWS Shield | DDoS protection | Standard (free) and Advanced |
| AWS WAF | Web application firewall | Block SQL injection, XSS, malicious IPs |
| AWS Firewall Manager | Centralized WAF/Shield policy management | Enforce rules across accounts |
| AWS Security Hub | Centralized security findings dashboard | Aggregates GuardDuty, Inspector, Macie |
| Amazon GuardDuty | Threat detection via ML | Detect suspicious activity |
| Amazon Inspector | Automated vulnerability scanning | EC2, Lambda, container CVEs |
| Amazon Macie | Sensitive data discovery in S3 | Detect PII in S3 buckets |
| Amazon Detective | Security investigation | Visualize root cause of findings |
| AWS Resource Access Manager (RAM) | Share AWS resources across accounts | Share subnets, Transit Gateways |

---

## Exam keyword map

| Keyword | Service |
|---------|---------|
| "MFA, least privilege, IAM policies" | IAM |
| "SSO across multiple accounts" | IAM Identity Center |
| "App user sign-up/sign-in" | Cognito |
| "Download SOC2, PCI compliance reports" | Artifact |
| "SSL certificate for CloudFront/ALB" | ACM |
| "Encrypt data at rest" | KMS |
| "Rotate database passwords automatically" | Secrets Manager |
| "DDoS protection" | Shield |
| "Block SQL injection, bad IPs" | WAF |
| "Detect threats, unusual API activity" | GuardDuty |
| "Scan EC2 for CVEs" | Inspector |
| "Find PII in S3" | Macie |
| "Aggregate security findings" | Security Hub |
| "Investigate security incidents" | Detective |
