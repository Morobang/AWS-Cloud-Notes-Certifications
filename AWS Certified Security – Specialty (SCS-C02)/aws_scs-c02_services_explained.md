
# AWS Security Specialty (SCS-C02) In-Scope Services Explained

This document explains the key AWS services and features that are in scope for the AWS Security Specialty exam (SCS-C02). Each service is described with its purpose, security role, and a practical example.

---

## Management and Governance Services

### AWS CloudTrail
- **Purpose:** Records all API calls made in your AWS account.
- **Security Role:** Enables auditing and investigation by tracking who did what and when.
- **Example:** Detects unauthorized deletion of security groups by logging API calls.

### Amazon CloudWatch
- **Purpose:** Collects and monitors metrics, logs, and events.
- **Security Role:** Provides real-time alerting and visualization of security-related metrics.
- **Example:** Alert triggered when an EC2 instance’s CPU usage spikes unexpectedly, indicating possible compromise.

### AWS Config
- **Purpose:** Tracks resource configurations and compliance with policies.
- **Security Role:** Detects configuration drift and enforces compliance rules.
- **Example:** Alerts if an S3 bucket is publicly accessible contrary to security policies.

### AWS Organizations
- **Purpose:** Centralized management of multiple AWS accounts.
- **Security Role:** Applies governance and guardrails across accounts using Service Control Policies (SCPs).
- **Example:** Enforces policies preventing launching EC2 instances in unauthorized regions.

### AWS Systems Manager
- **Purpose:** Provides tools for resource management and automation.
- **Security Role:** Enables secure and automated operational tasks, such as patching and remote access.
- **Example:** Uses Session Manager to connect to instances without opening SSH ports.

### AWS Trusted Advisor
- **Purpose:** Provides real-time security and cost optimization recommendations.
- **Security Role:** Identifies security gaps and misconfigurations.
- **Example:** Warns if an S3 bucket is publicly accessible.

---

## Networking and Content Delivery Services

### Amazon VPC (Virtual Private Cloud)
- **Purpose:** Provides isolated virtual networks within AWS.
- **Security Role:** Controls network traffic with security groups and network ACLs.
- **Example:** Security groups allow only HTTPS traffic (port 443) to web servers.

#### VPC Features
- **Network Access Analyzer:** Detects unintended network exposure.
  - *Example:* Finds if an EC2 instance is reachable from the internet unexpectedly.
- **Network ACLs:** Stateless subnet-level packet filtering.
  - *Example:* Blocks inbound traffic from suspicious IP addresses.
- **Security Groups:** Stateful firewalls applied to instances.
  - *Example:* Allows inbound SSH access only from trusted IP ranges.
- **VPC Endpoints:** Private connections to AWS services without internet exposure.
  - *Example:* EC2 instances access S3 privately through a VPC endpoint.

---

## Security, Identity, and Compliance Services

### AWS Audit Manager
- **Purpose:** Automates audit evidence collection and compliance reporting.
- **Security Role:** Simplifies preparation for regulatory audits.
- **Example:** Collects logs and resource configuration data for HIPAA compliance audits.

### AWS Certificate Manager (ACM)
- **Purpose:** Manages SSL/TLS certificates.
- **Security Role:** Enables secure encrypted connections for applications.
- **Example:** Provides HTTPS certificates for CloudFront distributions.

### AWS CloudHSM
- **Purpose:** Provides hardware-based key storage for cryptographic operations.
- **Security Role:** Allows customers to manage encryption keys securely outside AWS control.
- **Example:** Manages encryption keys used for database encryption.

### Amazon Detective
- **Purpose:** Analyzes and investigates security incidents.
- **Security Role:** Helps identify root causes and lateral movement.
- **Example:** Investigates GuardDuty alerts by visualizing communication patterns.

### AWS Directory Service
- **Purpose:** Integrates AWS with Microsoft Active Directory.
- **Security Role:** Enables centralized authentication and authorization.
- **Example:** Allows users to log in to AWS resources using corporate credentials.

### AWS Firewall Manager
- **Purpose:** Central management of firewall rules across accounts.
- **Security Role:** Ensures consistent application of security policies.
- **Example:** Applies AWS WAF rules organization-wide to block common attacks.

### Amazon GuardDuty
- **Purpose:** Threat detection service using ML and anomaly detection.
- **Security Role:** Continuously monitors for malicious behavior.
- **Example:** Detects compromised IAM credentials by unusual API usage patterns.

### AWS IAM Identity Center (formerly AWS SSO)
- **Purpose:** Centralizes identity and access management.
- **Security Role:** Simplifies user access management with single sign-on.
- **Example:** Allows developers to access multiple AWS accounts with one login.

### AWS Identity and Access Management (IAM)
- **Purpose:** Manages user identities, roles, and permissions.
- **Security Role:** Implements least privilege access control.
- **Example:** Grants a Lambda function only the permissions it needs to access DynamoDB.

### Amazon Inspector
- **Purpose:** Automates security assessments of EC2 and container images.
- **Security Role:** Detects vulnerabilities and security misconfigurations.
- **Example:** Identifies outdated software versions on EC2 instances.

### AWS Key Management Service (KMS)
- **Purpose:** Manages encryption keys and cryptographic operations.
- **Security Role:** Centralizes key management with audit and access controls.
- **Example:** Encrypts EBS volumes and S3 objects using customer-managed keys.

### Amazon Macie
- **Purpose:** Uses ML to detect sensitive data in S3.
- **Security Role:** Prevents data leaks by alerting on exposed PII.
- **Example:** Flags S3 buckets containing unencrypted credit card numbers.

### AWS Network Firewall
- **Purpose:** Stateful network firewall for VPCs.
- **Security Role:** Provides advanced traffic filtering.
- **Example:** Blocks outbound traffic to malicious IP addresses.

### AWS Security Hub
- **Purpose:** Aggregates security alerts and compliance checks.
- **Security Role:** Provides a centralized dashboard for security posture.
- **Example:** Consolidates findings from GuardDuty, Inspector, and Macie.

### AWS Shield
- **Purpose:** DDoS protection for AWS resources.
- **Security Role:** Mitigates volumetric and protocol attacks.
- **Example:** Automatically blocks DDoS attacks targeting your web application.

### AWS WAF (Web Application Firewall)
- **Purpose:** Protects web applications by filtering HTTP/S traffic.
- **Security Role:** Blocks attacks like SQL injection and cross-site scripting.
- **Example:** Blocks traffic containing SQL injection attempts before reaching your app.

---

# Summary Table

| Service                   | Purpose                                      | Security Role/Example                                |
|---------------------------|----------------------------------------------|-----------------------------------------------------|
| AWS CloudTrail            | API call logging                             | Detect unauthorized API calls                        |
| Amazon CloudWatch         | Monitoring and alerting                      | Trigger alerts on suspicious CPU spikes             |
| AWS Config                | Resource compliance tracking                 | Flag publicly accessible S3 buckets                  |
| AWS Organizations         | Multi-account governance                     | Enforce policies across accounts                     |
| AWS Systems Manager       | Automation and secure resource management   | Connect to EC2 without SSH                            |
| AWS Trusted Advisor       | Recommendations on security gaps             | Alert on open S3 buckets                              |
| Amazon VPC                | Virtual network and firewall                 | Control inbound/outbound network traffic             |
| Network Access Analyzer   | Network exposure detection                    | Find unintended internet access                      |
| AWS Audit Manager         | Audit evidence automation                     | Prepare for HIPAA audit                               |
| AWS Certificate Manager   | Manage SSL/TLS certificates                   | Provide HTTPS certificates for CloudFront            |
| AWS CloudHSM              | Hardware security modules                      | Manage encryption keys securely                       |
| Amazon Detective          | Security incident investigation               | Analyze GuardDuty alerts                              |
| AWS Directory Service     | Active Directory integration                   | Corporate login to AWS                               |
| AWS Firewall Manager      | Firewall policy management                     | Apply WAF rules globally                              |
| Amazon GuardDuty          | Threat detection                              | Detect compromised credentials                        |
| AWS IAM Identity Center   | Single sign-on and centralized identity       | One login for multiple AWS accounts                   |
| AWS IAM                   | Identity and permission management             | Least privilege access                                |
| Amazon Inspector          | Vulnerability scanning                        | Detect outdated software                              |
| AWS KMS                   | Encryption key management                      | Encrypt EBS and S3 data                               |
| Amazon Macie              | Sensitive data detection                       | Detect PII in S3                                     |
| AWS Network Firewall      | Network traffic filtering                      | Block malicious IP addresses                          |
| AWS Security Hub          | Centralized security findings                  | Consolidate alerts from multiple services            |
| AWS Shield                | DDoS protection                               | Automatically mitigate DDoS attacks                   |
| AWS WAF                   | Web application firewall                      | Block SQL injection attacks                           |

---

*This file can be used as a study guide or reference for the AWS Security Specialty exam.*

---
