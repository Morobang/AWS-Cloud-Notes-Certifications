# Task Statement 4.1: Compare AWS Pricing Models

## What you'll learn
- How the AWS Free Tier works and its three categories
- The core AWS pricing principles (pay-as-you-go, reserve, volume)
- How Reserved Instances and Savings Plans reduce costs
- How pricing varies by region and service

---

## AWS Free Tier

The AWS Free Tier gives new and existing customers access to AWS services at no charge, within defined limits. It has three distinct categories:

**12-month Free Tier** — Available for 12 months following your initial AWS account sign-up date. Common examples:
- 750 hours/month of EC2 t2.micro or t3.micro (Linux)
- 5 GB of S3 Standard storage
- 750 hours/month of RDS db.t2.micro (MySQL, PostgreSQL, etc.)
- 30 GB of EBS storage

**Always Free** — These offers do not expire and are available to all AWS customers indefinitely. Examples:
- AWS Lambda: 1 million free requests per month + 400,000 GB-seconds of compute time
- Amazon DynamoDB: 25 GB of storage + 25 read/write capacity units
- Amazon CloudWatch: 10 custom metrics and dashboards

**Free Trials** — Short-term trials that start from the first use of a specific service. Examples:
- Amazon SageMaker: 2 months free for new customers
- Amazon Redshift: 2 months free

*Exam note*: The exam tests whether you can distinguish between these three categories. If a question asks what happens after 12 months, "Always Free" services continue; "12-month" services do not.

---

## Core pricing principles

AWS pricing is built around three principles that reduce costs as you commit more or use more.

### Pay-as-you-go

You pay only for what you use, with no upfront fees, no contracts, and no termination charges. Pricing is typically measured in:
- **Hours or seconds** for compute (EC2, Lambda)
- **GB stored** for storage (S3, EBS, RDS)
- **GB transferred** for data transfer
- **Requests** for API calls (S3, Lambda, API Gateway)

When you stop using a resource, costs stop. There are no idle fees unless the resource itself is running (a stopped EC2 instance still accrues EBS storage charges, but no compute charges).

### Pay less when you reserve

Committing to use a resource for 1 or 3 years provides significant discounts over on-demand pricing.

**Reserved Instances (RIs)** — Capacity reservations for EC2, RDS, ElastiCache, Redshift, and other services.

| Commitment type | Discount vs On-Demand | Payment options |
|----------------|----------------------|-----------------|
| 1-year, no upfront | ~30–40% | Monthly |
| 1-year, partial upfront | ~40–50% | Upfront + monthly |
| 1-year, all upfront | ~40–50% | All upfront |
| 3-year, all upfront | ~60–70% | All upfront |

**Savings Plans** — A more flexible alternative to RIs. You commit to a minimum dollar amount of compute spend per hour (e.g., $10/hour), and AWS applies discounted rates to matching usage across EC2, Lambda, and Fargate — regardless of instance family, region, or OS.

- **Compute Savings Plans**: Most flexible. Applies to any EC2 instance regardless of region, instance family, or size, plus Lambda and Fargate.
- **EC2 Instance Savings Plans**: Applies to a specific instance family in a specific region. Higher discount than Compute Savings Plans.

*Use RIs or Savings Plans when*: You have predictable, steady-state workloads that you know will run for 1–3 years.

### Pay less with volume

Many AWS services charge less per unit as usage increases. Pricing tiers automatically apply as you use more — no action required.

**S3 example** (simplified):
- First 50 TB/month: $0.023 per GB
- Next 450 TB/month: $0.022 per GB
- Over 500 TB/month: $0.021 per GB

**Data transfer**: Inbound data transfer (from internet to AWS) is free. Outbound data transfer starts at a higher per-GB rate and decreases at higher volumes.

**AWS Organizations consolidated billing** aggregates usage across all accounts in an organization, so the combined usage reaches volume discount tiers faster than individual accounts would on their own.

---

## How pricing varies

### By service category

Different services use different pricing dimensions:

| Service | Primary pricing dimension |
|---------|--------------------------|
| EC2 | Instance hours, instance type, region |
| S3 | GB stored, requests, data transfer out |
| RDS | Instance hours, storage, I/O, data transfer |
| Lambda | Number of requests + GB-seconds of execution |
| CloudFront | GB transferred + number of requests |
| Data Transfer | GB transferred out of AWS to the internet |

**Data transfer pricing** — key rules for the exam:
- Data transfer **into** AWS (inbound): free
- Data transfer **between AWS services in the same region**: generally free or very low cost
- Data transfer **between regions**: charged per GB
- Data transfer **out to the internet**: charged per GB (volume discounts apply)

### By region

The same EC2 instance type costs different amounts in us-east-1 (N. Virginia), eu-west-1 (Ireland), and ap-southeast-1 (Singapore). Generally, US East (N. Virginia) has the lowest pricing for most services.

---

## Pricing model comparison

| Model | Best for | Risk |
|-------|----------|------|
| On-Demand | Variable workloads, new projects, short-term usage | Highest cost per hour |
| Reserved Instances | Steady-state workloads you'll run 1–3 years | Cost if usage drops |
| Savings Plans | Flexible compute commitments across EC2/Lambda/Fargate | Commitment still applies if usage drops |
| Spot Instances | Fault-tolerant, flexible workloads | Instance can be interrupted with 2-minute notice |
| Dedicated Hosts | Compliance requirements, BYOL licensing | Highest cost per host |

---

## Practice questions

**Q1.** A company is building a new proof-of-concept application on AWS and is unsure if it will be used long-term. Which pricing model minimizes financial risk?

A) 3-year Reserved Instances  
B) On-Demand pricing  
C) Savings Plans  
D) Dedicated Hosts

**Answer: B** — On-Demand pricing has no commitment and no upfront cost. The company pays only for what it uses and can stop at any time. Reserved Instances and Savings Plans require 1–3 year commitments, creating cost exposure if the project is cancelled. Dedicated Hosts are appropriate for compliance or licensing needs, not cost minimization for uncertain workloads.

---

**Q2.** An organization runs 20 AWS accounts under AWS Organizations. Their combined S3 usage totals 800 TB per month across all accounts. How does consolidated billing affect their S3 costs?

A) Each account is billed separately, so there is no pricing benefit  
B) The organization's total usage is combined, allowing it to reach higher volume discount tiers  
C) Consolidated billing only affects EC2 pricing  
D) Only the management account receives discounts

**Answer: B** — Consolidated billing aggregates usage across all member accounts. The combined 800 TB reaches higher S3 volume tiers that individual accounts with smaller usage would not reach on their own, reducing the effective per-GB price. All accounts benefit.

---

**Q3.** A company's Lambda function processes images and runs approximately 5 million invocations per month. What portion falls under the AWS Always Free Tier?

A) None — the Free Tier only applies to new accounts  
B) The first 1 million requests per month  
C) The first 750 hours per month  
D) All Lambda usage is always free

**Answer: B** — Lambda's Always Free Tier includes 1 million free requests per month for all AWS customers indefinitely. The remaining 4 million invocations are billed at standard rates. The 750-hour free tier applies to EC2, not Lambda. Always Free benefits do not expire and are not limited to new accounts.

---

**Next:** [Task 4.2 — Billing and Cost Management](./Task%20Statement%204.2-Understand%20resources%20for%20billing%2C%20budget%2C%20and%20cost%20management.md)
