# Task Statement 4.2: Understand Resources for Billing, Budget, and Cost Management

## What you'll learn
- How AWS Budgets works and when to use each budget type
- What AWS Cost Explorer provides and how it differs from Budgets
- How to use the AWS Pricing Calculator before deploying
- How AWS Organizations consolidated billing works
- What cost allocation tags are and how they appear in billing reports
- What the Cost and Usage Report (CUR) contains

---

## AWS Budgets

AWS Budgets lets you set spending or usage limits and receive alerts when you approach or exceed them. Budgets are proactive — they tell you what is happening or is forecast to happen before the month ends.

### Budget types

| Type | What it monitors | Example use |
|------|-----------------|-------------|
| **Cost budget** | Total dollar spending | "Alert me when monthly spend exceeds $500" |
| **Usage budget** | Usage of a specific metric | "Alert me when EC2 hours exceed 800 hours/month" |
| **Reservation coverage budget** | % of usage covered by Reserved Instances or Savings Plans | "Alert me if RI coverage drops below 80%" |
| **Reservation utilization budget** | % of purchased RIs or Savings Plans being used | "Alert me if RI utilization drops below 70%" |

### Alerts

Budgets can send alerts at two trigger types:
- **Actual threshold**: triggered when real spending reaches a percentage of budget (e.g., 80%, 100%)
- **Forecasted threshold**: triggered when AWS predicts you will exceed the budget by end of month

Alerts can be sent to email addresses or to an SNS topic, which can trigger automated responses.

*Use when*: You want to be notified — or trigger automation — when costs or usage approach a defined limit.

---

## AWS Cost Explorer

Cost Explorer is a visualization and analysis tool. It shows historical spending data and provides forecasts based on usage trends.

**What Cost Explorer shows**:
- Up to 12 months of historical cost and usage data
- Costs broken down by service, region, account, usage type, or cost allocation tag
- Daily, monthly, or hourly granularity
- Forecasted costs for the next 12 months
- Reserved Instance and Savings Plans utilization and coverage reports

**Right-sizing recommendations**: Cost Explorer identifies EC2 instances that are consistently underutilized and recommends downsizing to a smaller instance type. This is the "right-sizing" feature.

**Cost anomaly detection**: Cost Explorer can identify unusual spending spikes — for example, a Lambda function triggered in a loop causing unexpected costs — and send alerts.

*Use when*: You want to understand where your money went, identify trends, investigate cost spikes, or evaluate whether Reserved Instances are being used efficiently.

### Budgets vs Cost Explorer

| | AWS Budgets | AWS Cost Explorer |
|-|-------------|-------------------|
| Purpose | Set limits and get alerts | Analyze and visualize historical costs |
| Direction | Forward-looking (alert before overspend) | Backward-looking (analyze what happened) |
| Action | Sends notifications | Provides reports and recommendations |

---

## AWS Pricing Calculator

The AWS Pricing Calculator is a free web-based tool for estimating the cost of a proposed AWS architecture before you build it. You configure services (EC2 instance type, hours per month, storage, data transfer, etc.) and the calculator produces a monthly cost estimate.

**When to use it**:
- Estimating costs before approving a project budget
- Comparing costs of different architectural options
- Calculating the cost difference between on-demand and reserved pricing
- Planning migration: comparing current on-premises costs to equivalent AWS costs

The Pricing Calculator is accessed at calculator.aws — it is separate from the AWS Console and does not require an AWS account to use.

*Use when*: You need a cost estimate before deploying — for budgeting, business cases, or architecture comparisons.

---

## AWS Organizations — Consolidated Billing

When multiple AWS accounts are grouped under AWS Organizations, consolidated billing combines all accounts into a single monthly invoice paid by the management (payer) account.

**Benefits**:
- One bill, one payment method for all accounts
- Usage from all accounts is pooled for volume pricing — reaching higher usage tiers faster
- Reserved Instances and Savings Plans purchased in one account can benefit resources in other accounts in the same organization
- Each member account's costs remain visible individually — you can still see per-account breakdowns

*Use when*: You need centralized payment and want volume discounts to apply across all accounts in your organization.

---

## Cost Allocation Tags

Cost allocation tags are key-value labels you attach to AWS resources. Once activated in the Billing console, they appear as columns in billing reports, letting you filter and group costs.

### Two categories of tags

**AWS-generated tags** — Automatically added by AWS. Not customizable. Examples:
- `aws:createdBy` — which IAM user or role created the resource
- `aws:cloudformation:stack-name` — the CloudFormation stack that launched the resource

**User-defined tags** — Created and applied by your team. Examples:
- `Environment: prod` / `dev` / `test`
- `Department: finance` / `engineering` / `marketing`
- `Project: web-app-v2`
- `CostCenter: CC-12345`

Tags must be activated in the Billing and Cost Management console before they appear in billing reports. Tags applied before activation are not retroactively added to historical reports.

*Use when*: You need to track costs by team, project, or environment — for charge-backs, budget reporting, or identifying which workloads cost the most.

---

## AWS Cost and Usage Report (CUR)

The Cost and Usage Report is the most detailed billing data AWS provides. It is delivered as a CSV or Parquet file to an S3 bucket on a daily or monthly schedule.

**What CUR contains**:
- Line-item costs for every individual resource, by hour or day
- Reserved Instance amortization (spreading RI costs over usage periods)
- Savings Plans utilization
- Cost allocation tag values for each resource
- Usage type details (which API calls, GB of transfer, etc.)

Organizations with complex reporting requirements feed CUR into analytics tools — Amazon Athena to query it with SQL, Amazon QuickSight to build dashboards, or third-party BI tools.

*Use when*: You need the most granular billing data for custom analysis, charge-back reports to internal teams, or feeding an existing BI platform.

---

## Service selection quick reference

| Need | Service |
|------|---------|
| Get alerted before exceeding a spending limit | AWS Budgets |
| Analyze which services cost the most last quarter | AWS Cost Explorer |
| Estimate cost of a new architecture before building it | AWS Pricing Calculator |
| Combine billing across all accounts in one invoice | AWS Organizations consolidated billing |
| Track costs by department or project | Cost allocation tags + CUR or Cost Explorer |
| Export detailed per-resource billing data to S3 | Cost and Usage Report (CUR) |

---

## Practice questions

**Q1.** A company wants to receive an alert if their monthly AWS bill is forecast to exceed $10,000 before the end of the month. Which service and feature handles this?

A) AWS Cost Explorer with anomaly detection  
B) AWS Budgets with a forecasted alert  
C) AWS Pricing Calculator  
D) AWS Cost and Usage Report

**Answer: B** — AWS Budgets supports forecasted alerts that trigger when AWS predicts spending will exceed the configured threshold before the billing period ends. Cost Explorer analyzes historical data and can detect anomalies, but does not set spend limits with proactive alerts. Pricing Calculator estimates future costs for planning; it does not monitor actual spend. CUR exports billing data to S3 — it does not send alerts.

---

**Q2.** A DevOps team needs to identify which AWS services have contributed most to costs over the past 6 months and whether any EC2 instances are underutilized. Which tool should they use?

A) AWS Budgets  
B) AWS Pricing Calculator  
C) AWS Cost Explorer  
D) AWS Cost and Usage Report

**Answer: C** — Cost Explorer provides historical cost breakdowns by service, region, and account, with up to 12 months of data. It also provides right-sizing recommendations that identify underutilized EC2 instances. Budgets sets limits and sends alerts — it does not provide historical analysis or right-sizing. Pricing Calculator is a pre-deployment estimation tool. CUR provides raw line-item data but requires additional tools to analyze.

---

**Q3.** An organization with 15 AWS accounts under AWS Organizations purchases 100 Reserved Instances in the management account. How does this affect member accounts?

A) Only the management account can use the Reserved Instances  
B) Reserved Instance benefits can be shared across all accounts in the organization  
C) Member accounts must purchase their own Reserved Instances separately  
D) This is not possible — RIs must be purchased in each account individually

**Answer: B** — AWS Organizations enables RI sharing across accounts by default. When a Reserved Instance is not fully utilized in the account that purchased it, the discount automatically applies to matching usage in other accounts in the organization. This makes it more cost-effective to purchase RIs centrally.

---

**Next:** [Task 4.3 — Technical Resources and Support](./Task%20Statement%204.3-Identify%20AWS%20technical%20resources%20and%20AWS%20Support%20options.md)
