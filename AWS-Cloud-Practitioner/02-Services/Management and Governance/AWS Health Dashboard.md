# AWS Health Dashboard

## What you'll learn
- The two views of the Health Dashboard
- Service Health vs Personal Health
- Key use cases
- Key exam facts

---

## What is the AWS Health Dashboard?

The AWS Health Dashboard provides **information about the health of AWS services and how events affecting those services impact your specific AWS resources**.

It has two views:

**Service Health** (formerly called AWS Service Health Dashboard) — Shows the current and historical status of all AWS services across all Regions. Publicly accessible at status.aws.amazon.com. Anyone can see if AWS services are experiencing issues.

**Your Account Health** (formerly called AWS Personal Health Dashboard) — Shows events that specifically affect your account and your resources. Personalized alerts and notifications about maintenance, scheduled changes, and service disruptions that impact services you are using.

---

## Service Health view

- Shows whether each AWS service in each Region is operating normally, experiencing degraded performance, or experiencing an outage
- Historical data available — see past incidents
- No authentication required — public page

Use this when: "Is AWS S3 in us-east-1 having issues right now?"

---

## Your Account Health view

- Shows only events relevant to your account — not general global status
- Alerts for:
  - **Scheduled maintenance**: "Your EC2 instance will undergo host maintenance on [date]. Action may be required."
  - **Operational issues**: AWS service problems affecting your specific resources
  - **Account notifications**: Billing alerts, security notifications, compliance updates
- Events categorized by **service**, **Region**, and **affected resources**
- Can configure **EventBridge rules** to trigger Lambda or SNS notifications on health events

---

## AWS Health Aware (AHA)

An open-source solution that proactively alerts teams via Slack, Teams, or email when AWS health events affect their accounts. Uses the Health API to automate notifications from the Personal Health Dashboard.

---

## Health Dashboard vs CloudWatch

| | AWS Health Dashboard | Amazon CloudWatch |
|-|---------------------|------------------|
| Source of truth | AWS service health (AWS-side events) | Your application/resource metrics |
| What it shows | AWS-caused issues affecting your resources | Performance metrics you collect |
| Use case | "Is AWS having an outage that affects me?" | "Is my application performing well?" |

---

## When to use it

**Proactive maintenance planning** — Check for upcoming scheduled maintenance on EC2 instances and plan accordingly.

**Incident response** — During an outage, check whether the issue is on AWS's side (Service Health) or in your application (CloudWatch).

**Automated alerting** — Set up EventBridge rules to notify your team automatically when AWS health events affect your services.

---

## Exam focus

- Health Dashboard = **AWS service health status** — both global and account-specific
- **Service Health** = public status of all AWS services (status.aws.amazon.com)
- **Your Account Health** = personalized view of events affecting your resources
- Use when exam describes: "AWS outage notification," "scheduled maintenance alerts," "know when AWS services are degraded"

---

## Practice questions

**Q1.** A company wants to receive automatic notifications when AWS announces scheduled maintenance for their EC2 instances. Which AWS service provides personalized health events that are specific to their account's resources?

A) Amazon CloudWatch  
B) AWS Trusted Advisor  
C) AWS Health Dashboard (Your Account Health)  
D) AWS Config

**Answer: C** — The Your Account Health view of the AWS Health Dashboard shows events that specifically affect your resources — including EC2 maintenance notifications. You can configure EventBridge rules to send these notifications via SNS to email or Slack. CloudWatch monitors your own metrics. Trusted Advisor checks best practices. Config tracks configuration compliance.
