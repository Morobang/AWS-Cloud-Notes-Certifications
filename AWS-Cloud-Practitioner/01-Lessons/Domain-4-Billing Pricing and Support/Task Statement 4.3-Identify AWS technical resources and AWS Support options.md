# Task Statement 4.3: Identify AWS Technical Resources and AWS Support Options

## What you'll learn
- Where to find official AWS documentation, whitepapers, and technical guidance
- The five AWS Support plans and their key differences
- How Trusted Advisor works and which checks require a paid plan
- The difference between the Service Health Dashboard and the Personal Health Dashboard
- The role of the AWS Trust and Safety team
- What the AWS Partner Network and AWS Marketplace provide

---

## Official AWS documentation and resources

### AWS Documentation

The primary reference for every AWS service. Organized by service, each section includes a user guide, API reference, CLI reference, and getting-started tutorials. When you need to know what a service does, how to configure it, or what an error means, the documentation is the authoritative source.

### AWS Whitepapers

In-depth technical papers covering architecture, security, compliance, and migration. Key whitepapers tested on the exam:
- **AWS Well-Architected Framework**: describes the six pillars (Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability)
- **AWS Cloud Adoption Framework (CAF)**: structured approach to cloud migration across business and technical perspectives
- **Overview of Amazon Web Services**: high-level introduction to the AWS platform

### AWS Prescriptive Guidance

Step-by-step implementation guides and migration patterns written by AWS experts. More specific than whitepapers — for example, a guide on migrating from Oracle to Aurora PostgreSQL or implementing a hub-and-spoke network with Transit Gateway.

*Use when*: You need a vetted, proven approach for a specific implementation task.

### AWS Knowledge Center

A searchable collection of answers to common questions and troubleshooting scenarios. Covers specific error messages, configuration examples, and step-by-step solutions for common issues across all AWS services.

*Use when*: You encounter a specific error or issue and want a known solution.

### AWS re:Post

A community-driven Q&A platform where AWS customers ask and answer questions. AWS employees provide official answers. Successor to the original AWS Forums.

*Use when*: You have a specific question and want community input, or you want to find how others have solved similar problems.

---

## AWS Support Plans

AWS offers five support tiers. The key differentiators are: response time for critical issues, access to technical support engineers, and access to a Technical Account Manager (TAM).

| Feature | Basic | Developer | Business | Enterprise On-Ramp | Enterprise |
|---------|-------|-----------|----------|--------------------|------------|
| **Cost** | Free | $29/month | $100/month or 10% of monthly bill | $5,500/month | $15,000/month |
| **Technical support access** | None | Business hours, email only | 24/7 phone, email, and chat | 24/7 phone, email, and chat | 24/7 phone, email, and chat |
| **Response — general guidance** | — | 24 business hours | 24 hours | 24 hours | 24 hours |
| **Response — system impaired** | — | 12 business hours | 12 hours | 12 hours | 12 hours |
| **Response — production down** | — | — | 4 hours | 4 hours | 1 hour |
| **Response — business-critical down** | — | — | 1 hour | 30 minutes | 15 minutes |
| **Trusted Advisor** | 7 core checks | 7 core checks | Full access | Full access | Full access |
| **Technical Account Manager** | No | No | No | Pool of TAMs | Dedicated TAM |

### When to choose each plan

**Basic** — Free. Customer service for billing and account questions. Access to AWS Health Dashboard and Knowledge Center. No technical support for service issues. For personal learning or exploration only.

**Developer** — $29/month. Adds email access to Cloud Support Associates during business hours. One primary contact can open unlimited cases. For individuals experimenting with AWS or running non-production workloads.

**Business** — $100/month minimum (or 10% of monthly bill if higher). 24/7 phone/chat/email access to Cloud Support Engineers. Full Trusted Advisor access. Infrastructure Event Management available for an additional fee. For production workloads where downtime has business impact.

**Enterprise On-Ramp** — $5,500/month. Adds access to a pool of Technical Account Managers (shared, not dedicated) and a 30-minute response time for business-critical outages. For companies scaling workloads and needing strategic guidance without a dedicated TAM.

**Enterprise** — $15,000/month. A dedicated Technical Account Manager, 15-minute response for critical outages, Infrastructure Event Management included, and Well-Architected reviews. For large enterprises running mission-critical workloads where AWS downtime has severe consequences.

---

## AWS Trusted Advisor

Trusted Advisor is an automated service that inspects your AWS account and provides recommendations across five categories.

### The five categories

| Category | What it checks |
|----------|---------------|
| **Cost Optimization** | Idle EC2 instances, unattached EBS volumes, underutilized Reserved Instances |
| **Security** | Security groups with unrestricted access, root account MFA, S3 bucket permissions, IAM password policy |
| **Fault Tolerance** | EBS snapshots age, Multi-AZ RDS, Auto Scaling configuration |
| **Performance** | High-utilization EC2 instances, CloudFront configuration |
| **Service Limits** | AWS service quotas you are approaching |

### Access by support plan

- **Basic and Developer plans**: 7 core security and limits checks only (MFA on root, unrestricted security groups, S3 public buckets, EBS/RDS public snapshots, IAM use, and service limits)
- **Business, Enterprise On-Ramp, and Enterprise plans**: Full access to all checks across all five categories, plus programmatic access via the Trusted Advisor API and CloudWatch integration

*Exam tip*: If a question asks which support plan is needed to access "all Trusted Advisor checks," the answer is Business or higher.

---

## AWS Health Dashboard

The Health Dashboard provides two distinct views:

**Service Health Dashboard** (public) — Shows real-time status of AWS services across all regions. Publicly available. When AWS experiences a service outage or disruption, this is where it is reported. Accessible at status.aws.amazon.com.

**Personal Health Dashboard** (AWS Health) — Shows events and notifications that specifically affect your AWS account and resources. Includes scheduled maintenance that will impact your instances, service degradations affecting your workloads, and required actions (such as certificate renewals). Accessed through the AWS Management Console.

The distinction: the Service Health Dashboard shows what is happening to AWS globally; the Personal Health Dashboard shows what is happening to your resources specifically.

---

## AWS Trust and Safety team

The Trust and Safety team handles abuse reports involving AWS infrastructure. If you believe an AWS resource is being used for spam, phishing, malware distribution, DDoS attacks, or other policy violations, you report it to this team.

This team also investigates customers whose AWS resources are being used abusively (whether intentionally or because the account was compromised). They can suspend or terminate accounts that violate AWS acceptable use policies.

*Exam note*: If a question asks where to report suspected abuse of AWS resources, the answer is the AWS Trust and Safety team.

---

## AWS Partner Network (APN)

The AWS Partner Network is the global program for companies that build on or sell with AWS.

**Technology Partners** — Companies that build software products that run on or integrate with AWS. Examples: database vendors, security tools, monitoring platforms. Their products are often available through AWS Marketplace.

**Consulting Partners** — Companies (system integrators, managed service providers, consulting firms) that help customers design, migrate, and manage workloads on AWS. Consulting partners are tiered by demonstrated expertise: Select, Advanced, and Premier.

*Use when*: You need outside help migrating to AWS or implementing a complex architecture, or when you want to purchase validated software through AWS Marketplace.

---

## AWS Marketplace

AWS Marketplace is a digital catalog of software from third-party vendors. You can find, purchase, and deploy software directly to your AWS environment, with billing consolidated into your AWS bill.

**What Marketplace offers**:
- Pre-configured AMIs (virtual machine images ready to launch on EC2)
- SaaS subscriptions
- Professional services listings
- Data products (datasets and data feeds)

**Pricing models available**: pay-as-you-go (hourly or monthly), annual subscriptions, bring your own license (BYOL), and free trials.

Organizations can create a **Private Marketplace** — a curated subset of Marketplace products that employees are allowed to purchase, enforcing procurement policies across the organization.

---

## Technical resources quick reference

| Resource | What it provides |
|----------|-----------------|
| AWS Documentation | Official reference for every service |
| AWS Whitepapers | In-depth architecture, security, and migration guidance |
| AWS Prescriptive Guidance | Step-by-step implementation guides |
| AWS Knowledge Center | Answers to common questions and error troubleshooting |
| AWS re:Post | Community Q&A platform |
| AWS Support Plans | Tiered technical support with defined response times |
| Trusted Advisor | Automated account-level recommendations |
| Health Dashboard | Service health (public) and account-specific health events |
| Trust and Safety team | Abuse reporting and policy enforcement |
| AWS Partner Network | Partners who build on or consult around AWS |
| AWS Marketplace | Third-party software, billed through AWS |

---

## Practice questions

**Q1.** A company runs a mission-critical e-commerce platform. They require a dedicated Technical Account Manager and need AWS to respond to critical production outages within 15 minutes. Which support plan meets these requirements?

A) Business Support  
B) Enterprise On-Ramp  
C) Enterprise Support  
D) Developer Support

**Answer: C** — Enterprise Support is the only plan that includes a dedicated TAM (not a shared pool) and a 15-minute response time for business-critical outages. Enterprise On-Ramp provides a pool of TAMs (shared) and a 30-minute critical response time. Business Support has a 1-hour critical response and no TAM. Developer Support has no production support at all.

---

**Q2.** A security engineer notices unusual traffic originating from EC2 instances in their account and suspects an external party is using compromised AWS resources to conduct a DDoS attack. Which AWS team should they contact?

A) AWS Professional Services  
B) AWS Trust and Safety team  
C) AWS Partner Network  
D) AWS Technical Account Manager

**Answer: B** — The AWS Trust and Safety team investigates abuse reports involving AWS resources, including DDoS attacks, spam, phishing, and malware. If resources in an AWS account are being used abusively (whether by the customer or due to compromise), Trust and Safety is the correct contact. Professional Services is for implementation engagements; the Partner Network is for software/consulting partners; a TAM provides strategic technical guidance.

---

**Q3.** A developer on a Basic Support plan wants to use Trusted Advisor to identify all underutilized EC2 instances and unattached EBS volumes to reduce costs. Can they do this?

A) Yes — all Trusted Advisor checks are available on all plans  
B) No — cost optimization checks require Business Support or higher  
C) Yes — cost checks are included in the 7 core checks for Basic  
D) No — Trusted Advisor is only available with Enterprise Support

**Answer: B** — Basic and Developer plans provide access to only 7 core Trusted Advisor checks, which focus on security (MFA on root, unrestricted security groups, public S3 buckets) and service limits. Cost optimization checks — including idle EC2 instances and unattached EBS volumes — require Business Support or higher. The full set of 100+ checks across all five categories requires Business, Enterprise On-Ramp, or Enterprise.

---

**Domain 4 complete.** You now understand AWS pricing models, cost management tools, and support resources.
