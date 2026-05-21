# Task Statement 2.2: Understand AWS Cloud Security, Governance, and Compliance Concepts

## What you'll learn
- How to protect data with encryption at rest and in transit
- How network security works in AWS (VPC, Security Groups, NACLs, WAF)
- How to track what's happening in your account (CloudTrail, CloudWatch, Config)
- How AWS Organizations and Service Control Policies provide governance
- What the major compliance standards are and how AWS helps you meet them

---

## Security concepts

### Encryption at rest vs. encryption in transit

**Encryption at rest** means your data is encrypted when stored — in S3, RDS, EBS volumes, and other storage services. If someone physically stole a hard drive, they couldn't read the data without the encryption key.

**Encryption in transit** means your data is encrypted while traveling over a network. This is what HTTPS does for web traffic. AWS services communicate over encrypted connections using TLS.

Neither is automatic for everything. You have to choose to enable encryption. For most AWS services, encryption is one setting away — but it's your responsibility to turn it on.

### Key management

AWS Key Management Service (KMS) handles encryption keys. There are two approaches:

**AWS-managed keys** — AWS creates and rotates the keys on your behalf. You control who can use them, but AWS handles the key lifecycle. Simpler to manage; meets most compliance needs.

**Customer-managed keys** — You create the key in KMS and control everything: rotation schedule, which services can use it, which users can access it. More control, more responsibility. Required when regulations demand that the customer hold the keys.

The rule: use customer-managed keys when compliance requires it or when you need to audit key usage independently. Use AWS-managed keys for everything else.

---

## Network security

### VPC — Virtual Private Cloud

A VPC is your private, isolated section of the AWS network. Every resource you create lives inside a VPC. You control the IP address ranges, how traffic flows, and what connects to the internet.

Inside a VPC, you divide the address space into **subnets**:

- **Public subnets** — have a route to the internet via an Internet Gateway. Web servers and load balancers typically go here.
- **Private subnets** — no direct internet route. Databases and backend services go here. They can initiate outbound internet connections through a NAT Gateway if needed, but cannot receive inbound connections from the internet.

This architecture means your database is never directly reachable from the public internet, even if your web servers are.

### Security Groups

Security Groups are virtual firewalls attached to individual resources (EC2 instances, RDS databases, Lambda functions, etc.). They control inbound and outbound traffic with allow rules.

Key behaviors:
- **Stateful**: If you allow inbound traffic, the response traffic is automatically allowed out. You only write rules for one direction.
- **Default deny**: Any traffic not explicitly allowed is blocked.
- **Instance-level**: Different resources can have different Security Groups.

A web server might have a Security Group that allows HTTP on port 80 from anywhere, HTTPS on port 443 from anywhere, and SSH on port 22 from your corporate IP only.

### Network Access Control Lists (NACLs)

NACLs are an additional layer of network control at the subnet level — they apply to all traffic entering or leaving a subnet, regardless of which resources are inside.

Key difference from Security Groups:
- **Stateless**: You must explicitly allow both inbound AND outbound traffic. If you allow inbound HTTP, you also need a rule allowing outbound responses.
- **Ordered rules**: Rules are processed in number order. First match wins.
- **Default allow**: A default NACL allows all traffic. Custom NACLs start with an explicit deny-all.

Think of Security Groups as the lock on your apartment door; NACLs are the checkpoint at the building entrance. Both layers add defense in depth.

### WAF — Web Application Firewall

WAF protects web applications from common web exploits: SQL injection (malicious database queries embedded in form inputs), cross-site scripting (malicious scripts injected into pages), bot traffic, and known attack patterns.

WAF operates at the application layer. It inspects HTTP/HTTPS requests before they reach your application and blocks requests that match rules you define or that match AWS Managed Rules (pre-built rule groups maintained by AWS).

You attach WAF to CloudFront, an Application Load Balancer, or API Gateway — wherever HTTP traffic enters your application.

---

## Logging and monitoring

### AWS CloudTrail

CloudTrail records every API call made in your AWS account: who took an action, what they did, when, from where, and whether it succeeded.

This includes actions taken through the Console, CLI, SDKs, and other AWS services — everything. CloudTrail is the audit log for your entire account.

**Default behavior**: CloudTrail event history is enabled automatically and stores 90 days of management events. To store logs longer-term, you create a Trail that ships logs to S3.

**What it captures**: Creating or deleting resources, changing permissions, launching instances, making configuration changes — any AWS API call.

CloudTrail is what you check after a security incident to answer "who did this and when." It's also what compliance auditors want to see.

### Amazon CloudWatch

CloudWatch is the monitoring service. It collects metrics (numerical measurements) and logs (text records) from your AWS resources and applications.

**Metrics**: CPU utilization, network traffic, disk I/O, request counts, error rates — any numerical measurement over time. CloudWatch stores these and lets you graph them.

**Alarms**: You set thresholds on metrics. When CPU exceeds 80% for 5 minutes, send an email. When error rate exceeds 5%, trigger an Auto Scaling event. Alarms drive automated response.

**Logs**: Application log files, VPC Flow Logs, CloudTrail logs, Lambda execution logs — CloudWatch Logs centralizes them for searching and retention.

CloudWatch Dashboards give you a live view of your system's health across multiple metrics on one screen.

### AWS Config

Config tracks the configuration state of your AWS resources over time. It answers the question: "What did this resource look like at any point in the past?"

If a Security Group was changed last Tuesday and an incident happened Thursday, Config can show you exactly what the Security Group looked like before and after the change.

Config also supports **Config Rules** — automated checks that evaluate whether your resources comply with specific requirements. Examples:

- "All S3 buckets must have encryption enabled" — Config flags any bucket that doesn't.
- "All EC2 instances must belong to a VPC" — Config flags any instance outside a VPC.
- "CloudTrail must be enabled in all regions" — Config alerts if it's not.

Config Rules can be AWS-managed (pre-built) or custom. They run continuously and report compliance status — which is exactly what security auditors ask for.

---

## Governance

Governance is the set of controls that ensure your cloud environment is used as intended — proper spending, appropriate security configurations, and compliance with internal policies.

### AWS Organizations

AWS Organizations lets you manage multiple AWS accounts centrally from a single management account. Large companies use multiple accounts to separate environments (production, development, testing), business units, or teams.

Benefits:
- **Consolidated billing**: One invoice for all accounts. Volume discounts apply across the organization.
- **Central policy enforcement**: Apply controls to all accounts from one place.
- **Account isolation**: A mistake in one account doesn't affect others.

Accounts are organized into **Organizational Units (OUs)** — groups of accounts that share policies. You might have a "Production" OU and a "Development" OU with different rules applied to each.

### Service Control Policies (SCPs)

SCPs are guardrails that set the maximum permissions available in accounts within your organization. They don't grant permissions — they define what permissions are available to be granted.

Important distinction: An SCP that allows an action doesn't mean a user in that account can do it. The user still needs their own IAM policy allowing it. But an SCP that denies an action blocks it for everyone in that account, even account administrators — with the exception of the management account.

Common SCP use cases:
- "No account in this organization may create resources outside us-east-1 and eu-west-1" — enforces data residency.
- "No account may disable CloudTrail" — protects your audit trail.
- "Development accounts may not access production data" — enforces environment separation.

SCPs are the policy mechanism that makes multi-account governance actually enforceable.

### Resource tagging

Tags are key-value pairs you attach to AWS resources. Example: `Environment: Production`, `Department: Engineering`, `Project: RevenuePortal`.

Tags enable:
- **Cost allocation**: See how much each team, project, or environment is spending.
- **Automation**: Scripts that target "all Development servers" without hardcoding resource IDs.
- **Access control**: IAM policies that restrict access based on tags (only resources tagged `Owner: YourTeam`).
- **Compliance**: AWS Config rules that flag untagged resources.

Consistent tagging is the prerequisite for cost visibility and resource management at scale.

---

## Compliance

Compliance means meeting specific regulatory or industry requirements that apply to your industry and customers.

### Common compliance standards

**GDPR (General Data Protection Regulation)** — EU law governing personal data of EU residents. Key requirements: explicit consent before collecting data, right to access their own data, right to request deletion, 72-hour breach notification, appropriate security measures. Applies to any company handling EU resident data, regardless of where the company is located.

**HIPAA (Health Insurance Portability and Accountability Act)** — US law protecting healthcare information. Requires encryption, access controls, audit logging, and a Business Associate Agreement with any service provider (including AWS) that handles protected health information.

**PCI DSS (Payment Card Industry Data Security Standard)** — Industry standard for any company processing payment card data. Requires network segmentation, encryption, access controls, regular vulnerability scanning, and log monitoring.

**SOC 1, SOC 2, SOC 3** — Service Organization Control reports, independent audits of internal controls. SOC 2 covers security, availability, processing integrity, confidentiality, and privacy. Commonly required in B2B contracts. SOC 3 is the public summary version.

**ISO 27001** — International standard for information security management systems. Covers policies, controls, and ongoing improvement processes.

### AWS's role in compliance

AWS itself holds many compliance certifications. AWS data centers and infrastructure are HIPAA-eligible, SOC 2 certified, PCI DSS certified, ISO 27001 certified, and more.

**This does not mean your workload is automatically compliant.**

AWS's certifications cover the infrastructure layer. Your workload — your application configuration, your encryption settings, your access controls — must separately meet those requirements. AWS gives you the compliant foundation; you build a compliant application on top of it.

### AWS Artifact

AWS Artifact is a self-service portal where you can access AWS's compliance reports and certifications on demand. You can download:
- SOC 1, SOC 2, and SOC 3 reports
- PCI DSS attestations
- ISO certifications
- HIPAA eligibility documentation

When your customers or auditors ask "Can you prove your infrastructure provider is compliant?" — you pull the relevant report from Artifact and share it.

Artifact also hosts agreements you may need to sign — such as the Business Associate Agreement for HIPAA.

### AWS Compliance Center

The Compliance Center provides documentation and resources about how AWS meets specific regulatory requirements. If you need to understand how AWS handles data residency, privacy regulations in a specific country, or a particular compliance framework, this is where to look.

---

## Exam focus

| Topic | What to know |
|-------|-------------|
| Encryption at rest vs. in transit | At rest = stored data; in transit = moving data. Both require you to enable them. |
| Customer-managed vs. AWS-managed keys | Customer-managed = you control rotation and access; AWS-managed = simpler, AWS handles lifecycle. |
| Security Groups vs. NACLs | Security Groups = stateful, instance-level; NACLs = stateless, subnet-level. |
| CloudTrail | Records all API calls — the audit log for your account. |
| CloudWatch | Metrics, alarms, logs — monitors operational health. |
| AWS Config | Tracks resource configuration history; flags compliance violations. |
| AWS Organizations | Manages multiple accounts; enables consolidated billing and policy enforcement. |
| SCPs | Set maximum permission boundaries for accounts; prevent but don't grant access. |
| AWS Artifact | Self-service portal for AWS compliance reports and agreements. |
| HIPAA/GDPR/PCI DSS | Know what industry each covers; know that AWS being certified does not make your workload compliant. |

---

## Practice questions

**Q1.** A security team needs to investigate a suspected unauthorized access to their AWS account three weeks ago. Which service gives them a record of API actions taken at that time?

A) Amazon CloudWatch  
B) AWS Config  
C) AWS CloudTrail  
D) AWS Trusted Advisor

**Answer: C** — CloudTrail records all API calls with timestamps, identities, and outcomes. CloudWatch monitors performance metrics. Config tracks resource configurations. Trusted Advisor provides recommendations.

---

**Q2.** A company's database is behind a web application. The security team wants to ensure the database cannot be reached from the internet even if the web server is compromised. What architectural approach achieves this?

A) Apply a Security Group to the database that allows all traffic  
B) Place the database in a private subnet with no route to the internet  
C) Enable encryption on the database  
D) Install WAF on the database

**Answer: B** — Private subnets have no internet route. Even if the web server is compromised, the attacker cannot reach the database from the internet directly. Encryption protects data at rest but doesn't prevent network access. WAF operates at the application layer, not at the network architecture level.

---

**Q3.** A large company wants to ensure that no development account in their AWS organization can create resources in EU regions, to comply with a data residency policy. What is the correct mechanism?

A) Apply IAM deny policies to every user in every development account  
B) Use a Service Control Policy that denies resource creation in EU regions, applied to the Development OU  
C) Create a NACL that blocks EU region traffic  
D) Configure CloudTrail to block EU region API calls

**Answer: B** — SCPs applied to an Organizational Unit affect all accounts in that OU without requiring changes to individual users or accounts. IAM policies in each account would require constant maintenance. NACLs control network traffic within a VPC, not account-level permissions. CloudTrail records actions but doesn't block them.

---

**Q4.** A healthcare company is using AWS RDS to store patient records. An IT manager says "We're HIPAA compliant because AWS is HIPAA-eligible." What's wrong with this statement?

A) Nothing — AWS eligibility covers all customer workloads  
B) AWS HIPAA eligibility covers the infrastructure; the company must still configure encryption, access controls, and audit logging, and sign a Business Associate Agreement  
C) AWS does not support HIPAA compliance at all  
D) HIPAA compliance is only required for storing data, not for compute

**Answer: B** — AWS provides a HIPAA-eligible foundation, but the customer is responsible for configuring their workload to meet HIPAA requirements: encrypting PHI, controlling access, enabling audit logging, and executing a BAA with AWS.

---

**Next:** [Task 2.3 — Access Management Capabilities](./Task%20Statement%202.3-Identify%20AWS%20access%20management%20capabilities.md)
