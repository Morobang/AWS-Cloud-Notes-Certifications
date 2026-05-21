# AWS CloudTrail

## What you'll learn
- What CloudTrail records and why it matters
- CloudTrail trails and event types
- CloudTrail vs CloudWatch
- Key exam facts

---

## What is AWS CloudTrail?

AWS CloudTrail is a **service that records all API calls made in your AWS account** — creating a complete audit trail of who did what, when, from where, and on which resource.

Every time someone (a user, a role, or an AWS service) makes a call to an AWS API — whether through the console, CLI, SDK, or another service — CloudTrail logs that event.

---

## What CloudTrail records

Each log entry (event) includes:

- **Who** made the call (IAM user, role, or AWS service)
- **What** was called (e.g., `ec2:TerminateInstances`, `s3:DeleteObject`)
- **When** (timestamp)
- **Where from** (source IP address)
- **Which resource** was affected
- **What was returned** (response, including whether it succeeded)

---

## Event types

**Management events** — Operations on AWS resources (creating/deleting/modifying resources). Examples: `CreateBucket`, `RunInstances`, `DeleteUser`. Enabled by default in all accounts.

**Data events** — Operations on data within resources. Examples: S3 `GetObject`, `PutObject`; Lambda `Invoke`. Must be explicitly enabled — these are higher volume.

**Insights events** — Detect unusual API activity (e.g., a sudden spike in `TerminateInstances` calls that deviates from normal patterns).

---

## Trails

By default, CloudTrail stores the last 90 days of management events in the **Event history** view in the console — no configuration needed.

To keep logs longer or enable advanced features, you create a **trail**:
- Delivers log files to an **S3 bucket** for long-term retention
- Optionally sends events to **CloudWatch Logs** for searching and alerting
- Can cover a single Region or all Regions (a best practice)

---

## CloudTrail vs CloudWatch

| | AWS CloudTrail | Amazon CloudWatch |
|-|---------------|-----------------|
| Purpose | API call audit log | Performance monitoring |
| What it records | API calls (who, what, when, where) | Metrics, logs, events |
| Use case | Security investigation, compliance | Operational alerts, dashboards |
| Question answered | "Who deleted this S3 bucket?" | "Is my application responding slowly?" |

---

## When to use it

**Security investigation** — "Who terminated these EC2 instances at 3 AM?" or "Which IAM user changed this security group?"

**Compliance** — Regulators require a complete audit trail of who accessed or modified systems.

**Change tracking** — Understand what changed in your environment and when.

**Threat detection** — CloudTrail Insights detects unusual API activity that may indicate a compromised account.

---

## Exam focus

- CloudTrail = **API call logging** — records every AWS API call for auditing and security
- Tracks **who, what, when, and from where** for all account activity
- Default 90-day history; **trails** extend retention to S3
- Key differentiator from CloudWatch: CloudTrail = audit trail; CloudWatch = performance monitoring
- Use when exam describes: "who made this change," "audit trail," "compliance logging," "detect unauthorized activity"

---

## Practice questions

**Q1.** A security team needs to investigate whether an unauthorized user accessed and deleted objects from an S3 bucket last week. Which AWS service provides the logs showing who made the S3 API calls?

A) Amazon CloudWatch  
B) AWS Config  
C) AWS CloudTrail  
D) Amazon GuardDuty

**Answer: C** — CloudTrail logs every API call including S3 operations — who made the call, what resource was accessed, and when. The team can search CloudTrail event history or logs to identify the API calls and the identity that made them. CloudWatch monitors metrics and logs application output but doesn't record API-level who/what/when. Config tracks configuration state changes. GuardDuty detects threats based on patterns but CloudTrail is the audit log that shows specific API calls.
