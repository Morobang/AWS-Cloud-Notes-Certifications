# Task Statement 2.4: Identify Components and Resources for Security

## What you'll learn
- The AWS security services and what each one does
- How to protect against DDoS attacks
- How to manage encryption keys and secrets
- Where to find AWS compliance information

---

## Why AWS provides security services

Running security on your own means buying, configuring, and maintaining security tools for every threat category. AWS builds security services that handle specific threat categories at cloud scale. You enable and configure them — they do the detection, analysis, and reporting.

The key mental model: AWS security services watch what's happening in your environment so you don't have to watch everything manually.

---

## Threat detection and monitoring

### Amazon GuardDuty

GuardDuty is a threat detection service that continuously analyzes your AWS account for malicious or unauthorized behavior.

It works by analyzing three data sources automatically:
- **CloudTrail logs** — API calls and management activity
- **VPC Flow Logs** — network traffic patterns in your VPC
- **DNS logs** — DNS query patterns (can reveal communication with malicious domains)

You don't send GuardDuty data manually. You enable it, and it begins analyzing these sources automatically.

What it detects: compromised EC2 instances communicating with known malware command-and-control servers, unusual API calls from unfamiliar locations, port scanning, cryptocurrency mining activity, credential theft attempts.

GuardDuty produces **findings** — descriptions of detected threats, severity-rated from low to high. You can forward findings to Security Hub for centralized management, or to EventBridge to trigger automated remediation.

One important point: GuardDuty monitors for threats. It doesn't block anything on its own. Responding to findings requires action from you or automation you set up.

### Amazon Inspector

Inspector runs automated security assessments against your workloads to find vulnerabilities and unintended network exposure.

What it scans:
- **EC2 instances** — checks for software vulnerabilities (CVEs), unintended network accessibility
- **Lambda functions** — checks for vulnerable package dependencies
- **Container images** — scans images in ECR for known vulnerabilities

Inspector produces findings that tell you exactly what's vulnerable, how severe it is (using CVSS scores), and remediation guidance. Unlike GuardDuty (which watches for active threats), Inspector proactively finds weaknesses before attackers do.

Inspector integrates with AWS Systems Manager to scan EC2 instances without requiring an agent installation.

### Amazon Macie

Macie discovers and protects sensitive data stored in S3. It uses machine learning to automatically identify data that looks like personally identifiable information (PII): names, email addresses, credit card numbers, social security numbers, passport numbers, and similar.

Why it matters: organizations often don't know exactly what sensitive data is in their S3 buckets — especially after years of accumulation. Macie scans buckets and alerts you when it finds sensitive data that might be publicly accessible or inadequately protected.

Macie findings tell you which bucket and object contains sensitive data, what type of sensitive data was found, and what the exposure risk is.

---

## DDoS protection

### AWS Shield

A Distributed Denial of Service (DDoS) attack floods your application with traffic to make it unavailable. AWS Shield defends against these attacks.

**AWS Shield Standard** — automatically enabled for all AWS customers at no additional cost. Protects against the most common network and transport layer DDoS attacks (volumetric attacks, protocol attacks). Most applications receive adequate protection from Shield Standard without any configuration.

**AWS Shield Advanced** — paid service providing additional protection for your most important applications. What it adds over Standard:
- Protection for Elastic IP, CloudFront, Route 53, Global Accelerator, and Application Load Balancers
- Near real-time visibility into attacks
- 24/7 access to the AWS DDoS Response Team (DRT)
- Cost protection — AWS absorbs the cost of scaling resources during an attack, so you don't pay for the extra capacity an attack forces you to use
- Protection against more sophisticated application-layer attacks

Shield Advanced is for organizations running high-value internet-facing applications where availability is critical and downtime is costly.

### AWS WAF (Web Application Firewall)

WAF operates at the application layer (HTTP) to filter malicious web requests before they reach your application. While Shield handles volumetric DDoS attacks, WAF handles application-layer attacks.

WAF inspects HTTP headers, body, and query strings against rules you define. You can block:
- **SQL injection** — attempts to manipulate database queries through form inputs
- **Cross-site scripting (XSS)** — malicious scripts injected into web content
- **Bad bots** — scrapers, scanners, and automated attack tools
- **Geographic restrictions** — block traffic from specific countries
- **Rate limiting** — block IPs that send too many requests per minute

AWS provides **Managed Rules** — pre-built rule groups maintained by AWS and AWS Marketplace security partners that cover known threats without requiring you to write rules from scratch.

WAF attaches to CloudFront, Application Load Balancers, and API Gateway.

---

## Centralized security management

### AWS Security Hub

Security Hub aggregates security findings from multiple AWS security services — GuardDuty, Inspector, Macie, IAM Access Analyzer, Firewall Manager, and others — into a single dashboard.

Without Security Hub, you'd need to check each security service separately. Security Hub normalizes findings into a standard format (ASFF — AWS Security Finding Format) and lets you see all of them in one place.

Security Hub also runs **security standards checks**: automated checks against frameworks like the AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, and PCI DSS. These checks continuously evaluate your configuration against known best practices and report compliance scores.

---

## Secrets and key management

### AWS Secrets Manager

Secrets Manager stores and manages sensitive configuration values that your applications need: database passwords, API keys, OAuth tokens, private certificates.

The core problem it solves: applications need credentials to connect to databases, but storing those credentials in code or config files is a security risk. Secrets Manager stores them securely and provides them to your application at runtime via an API call.

Key feature: **automatic rotation**. Secrets Manager can automatically rotate database passwords on a schedule (e.g., every 30 days), update the secret, and update the database — all without your application going down. If a password is ever compromised, the damage window is limited to how recently it was rotated.

Applications retrieve secrets programmatically. A Lambda function calls Secrets Manager to get the database password when it needs to connect — the password is never hardcoded anywhere.

### AWS KMS (Key Management Service)

KMS creates and manages the cryptographic keys used to encrypt your data across AWS services.

When you enable encryption for an S3 bucket, EBS volume, or RDS database, that encryption uses a KMS key. KMS handles key storage and access control. The actual encryption and decryption happens in the service (S3, EBS, RDS), but KMS controls who can use which keys.

**Key types:**
- **AWS-managed keys** — created and managed by AWS on your behalf, one per service. Simple to use; no charge for the key itself.
- **Customer-managed keys (CMKs)** — you create, configure, and manage these. You control the rotation schedule, who can use them, and which AWS services can access them. Required when compliance mandates customer key ownership.

Every use of a KMS key is logged in CloudTrail — giving you a full audit trail of who encrypted or decrypted what and when.

---

## AWS compliance and security resources

### AWS Artifact

Artifact is the self-service portal for downloading AWS's compliance reports and agreements. When a customer, partner, or auditor asks for evidence that AWS meets a specific compliance standard, you go to Artifact.

Available documents:
- SOC 1, SOC 2, and SOC 3 reports
- PCI DSS Attestation of Compliance
- ISO 27001, 27017, and 27018 certificates
- HIPAA Business Associate Agreement (BAA)
- GDPR data processing addendums

You access Artifact through the AWS Console. Reports require you to accept a non-disclosure agreement before downloading.

### AWS Trusted Advisor

Trusted Advisor analyzes your AWS account against five categories of best practices and makes recommendations:

1. **Cost Optimization** — identifies idle resources, underutilized instances, savings opportunities
2. **Performance** — identifies performance improvements
3. **Security** — identifies security gaps
4. **Fault Tolerance** — identifies single points of failure
5. **Service Limits** — warns when you're approaching service quotas

The Security category is what's relevant here. Trusted Advisor flags things like:
- S3 buckets with public access enabled
- Security Groups that allow unrestricted access on sensitive ports (SSH, RDP)
- IAM users with no MFA
- Root user access keys that exist (you shouldn't have them)
- CloudTrail not enabled

Free tier users get access to six core checks in Security and Service Limits. Business and Enterprise support plans get full access to all checks and recommendations.

---

## Service selection summary

| Security need | AWS service |
|--------------|-------------|
| Detect threats from compromised accounts or instances | GuardDuty |
| Find software vulnerabilities in EC2, Lambda, containers | Inspector |
| Discover sensitive data (PII) in S3 buckets | Macie |
| Protect against DDoS attacks — basic | Shield Standard (automatic, free) |
| Protect against DDoS attacks — advanced | Shield Advanced |
| Filter malicious web requests (SQL injection, XSS) | WAF |
| Centralize findings from multiple security services | Security Hub |
| Store and auto-rotate application secrets and passwords | Secrets Manager |
| Manage encryption keys for data at rest | KMS |
| Download AWS compliance reports and certifications | AWS Artifact |
| Get security best practice recommendations for your account | Trusted Advisor |

---

## Practice questions

**Q1.** An application running on EC2 is connecting to a known malware command-and-control server. Which service would detect and alert on this behavior?

A) Amazon Inspector  
B) Amazon Macie  
C) Amazon GuardDuty  
D) AWS Shield

**Answer: C** — GuardDuty analyzes VPC Flow Logs and DNS logs to detect anomalous network behavior including communication with known malicious domains. Inspector scans for vulnerabilities. Macie finds sensitive data in S3. Shield defends against DDoS attacks.

---

**Q2.** A developer hardcoded a database password in the application source code. After a security review, the team wants to move to a secure, automated approach where the password rotates every 30 days without application downtime. Which service handles this?

A) AWS KMS  
B) AWS Secrets Manager  
C) AWS Certificate Manager  
D) AWS IAM

**Answer: B** — Secrets Manager stores application secrets and supports automatic rotation. The application retrieves the password from Secrets Manager at runtime. KMS manages encryption keys, not application passwords. Certificate Manager handles TLS certificates. IAM manages access control, not application secrets.

---

**Q3.** A company wants to download a copy of AWS's SOC 2 report to share with a customer who requires proof of their cloud provider's security controls. Where do they find this?

A) AWS Trusted Advisor  
B) AWS Config  
C) AWS Artifact  
D) AWS Security Hub

**Answer: C** — AWS Artifact is the self-service portal for accessing AWS compliance reports including SOC 1/2/3, PCI DSS, ISO certifications, and more. Trusted Advisor gives recommendations. Config tracks resource configurations. Security Hub aggregates security findings.

---

**Q4.** An e-commerce website uses an Application Load Balancer and runs promotional sales events. During a recent sale, the site was hit with a volumetric flood attack that made it unreachable for 20 minutes. The company wants additional protection including AWS incident response support and cost protection during attacks. Which service addresses this?

A) AWS Shield Standard  
B) AWS Shield Advanced  
C) AWS WAF  
D) Amazon GuardDuty

**Answer: B** — Shield Advanced provides enhanced DDoS protection, 24/7 access to the DDoS Response Team, and cost protection against scaling charges incurred during attacks. Shield Standard is already active but doesn't include DRT access or cost protection. WAF filters application-layer requests but doesn't handle volumetric network-level attacks. GuardDuty detects threats but doesn't provide DDoS protection.

---

**Q5.** A company discovers that an S3 bucket contains thousands of files with customer credit card numbers that should not be there. Which service would have automatically detected this?

A) Amazon GuardDuty  
B) Amazon Inspector  
C) Amazon Macie  
D) AWS Config

**Answer: C** — Macie uses ML to scan S3 buckets and identify sensitive data including credit card numbers, social security numbers, and other PII. GuardDuty detects threats and unusual behavior. Inspector finds software vulnerabilities. Config tracks resource configuration changes.

---

**Domain 2 complete.** You now understand the shared responsibility model, how to secure cloud environments, how to control access with IAM, and what security services AWS provides.

**Next:** [Domain 3 — Cloud Technology](../../Domain-3-Cloud%20Technology/README.md)
