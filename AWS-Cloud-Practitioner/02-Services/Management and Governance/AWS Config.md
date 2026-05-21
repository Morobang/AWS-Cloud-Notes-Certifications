# AWS Config

## What you'll learn
- What AWS Config does and what problem it solves
- Configuration history and Config rules
- Config vs CloudTrail
- Key exam facts

---

## What is AWS Config?

AWS Config is a service that **continuously monitors and records the configuration of your AWS resources** and lets you evaluate those configurations against desired settings (compliance rules). It answers the question: "What did my resource configuration look like at any point in time, and does it comply with our policies?"

---

## Two core functions

**1. Configuration history** — Config continuously records the configuration of every AWS resource (EC2 instances, security groups, S3 buckets, IAM roles, etc.). You can look back at any point in time and see exactly how a resource was configured — and what changed.

Example: "Show me the security group configuration 30 days ago, and show me every change made to it since then."

**2. Config rules** — Rules that evaluate whether resources meet your compliance requirements. Config checks resources against these rules and marks them as **Compliant** or **Non-compliant**.

Example rules:
- "No S3 buckets should be publicly accessible"
- "All EC2 instances must have encryption enabled on their EBS volumes"
- "Security groups must not allow inbound access from 0.0.0.0/0 on port 22"

Config can use AWS managed rules (100+ pre-built) or custom rules (Lambda functions).

---

## AWS Config vs AWS CloudTrail

| | AWS Config | AWS CloudTrail |
|-|-----------|----------------|
| What it tracks | Resource configuration state over time | API calls (who did what) |
| Question answered | "What does/did this resource look like?" | "Who made this API call?" |
| Compliance | Yes — Config rules evaluate compliance | No |
| History | Configuration timeline per resource | Event history of API calls |

CloudTrail tells you someone changed a security group. Config shows you what the security group looked like before and after, and whether it now violates a compliance rule.

---

## Remediation

Config can automatically remediate non-compliant resources using **AWS Systems Manager Automation documents** — for example, automatically disabling public access on an S3 bucket that violates the "no public buckets" rule.

---

## When to use it

**Compliance auditing** — Continuously check that resources meet security policies (encryption enabled, no public access, approved instance types).

**Change management** — After an incident, trace exactly what changed in a resource's configuration and when.

**Security posture** — Identify resources that are out of compliance and fix them before they become vulnerabilities.

---

## Exam focus

- Config = **resource configuration history and compliance rules**
- Tracks configuration state of resources over time (not API calls — that's CloudTrail)
- **Config rules** evaluate compliance: Compliant vs Non-compliant
- Can automatically **remediate** non-compliant resources
- Use when exam describes: "track configuration changes," "ensure compliance," "audit resource settings," "detect non-compliant resources"

---

## Practice questions

**Q1.** A company needs to ensure that no S3 buckets in their account are publicly accessible, and they want to be automatically alerted when any bucket is made public. Which AWS service enforces this compliance requirement?

A) AWS CloudTrail  
B) Amazon GuardDuty  
C) AWS Config  
D) AWS Trusted Advisor

**Answer: C** — Config rules continuously evaluate S3 bucket configurations against the "no public access" rule. When a bucket becomes non-compliant, Config flags it and can trigger an SNS notification or automated remediation. CloudTrail records who made the API call to change the bucket but doesn't evaluate ongoing compliance. GuardDuty detects threats through behavior analysis. Trusted Advisor does check for public S3 buckets but doesn't provide continuous real-time enforcement.
