# AWS Budgets

## What you'll learn
- What AWS Budgets is and the four budget types
- How alerts work (actual vs forecasted)
- How Budgets differs from Cost Explorer
- Key exam facts

---

## What is AWS Budgets?

AWS Budgets is a cost management service that lets you set custom spending or usage thresholds and receive alerts when you approach or exceed them. It is a **proactive** tool — it tells you what is happening now and what is forecast to happen before the billing period ends.

---

## The four budget types

| Budget type | What it monitors | Example |
|-------------|-----------------|---------|
| **Cost budget** | Total dollar spending | "Alert me when monthly spend exceeds $500" |
| **Usage budget** | Usage of a specific metric | "Alert me when EC2 hours exceed 1,000/month" |
| **RI coverage budget** | % of usage covered by Reserved Instances | "Alert me if RI coverage drops below 80%" |
| **RI utilization budget** | % of purchased RIs being used | "Alert me if RI utilization drops below 70%" |

---

## How alerts work

Budgets supports two types of alert triggers:

**Actual threshold** — Triggered when real spending has already reached a percentage of the budget (e.g., 80%, 100%). You've already spent this much.

**Forecasted threshold** — Triggered when AWS predicts you *will* exceed the budget before the month ends, based on current spending trends. This is the proactive warning — you haven't overspent yet.

Alerts are delivered to:
- Email addresses
- Amazon SNS topics (which can trigger Lambda for automated responses)

---

## AWS Budgets vs AWS Cost Explorer

| | AWS Budgets | AWS Cost Explorer |
|-|-------------|-------------------|
| Direction | Forward-looking (alert before overspend) | Backward-looking (analyze what happened) |
| Action | Send notifications, trigger automation | Provide reports and recommendations |
| Best for | Setting limits and getting alerts | Understanding historical spending |

---

## When to use it

**Cost control** — A startup sets a $500/month budget with alerts at 75% and 100% so the finance team is never surprised.

**Reserved Instance monitoring** — Set a utilization budget to alert if purchased RIs are underutilized (money wasted on unused reservations).

**Per-project tracking** — Create separate budgets for different projects by filtering by cost allocation tags.

---

## Exam focus

- Budgets = **proactive spending limits and alerts**
- Supports **forecasted alerts** (before you exceed) and **actual alerts** (after you've spent it)
- Four types: **Cost, Usage, RI Coverage, RI Utilization**
- Alerts go to email or **SNS** (for automation)
- Key differentiator from Cost Explorer: Budgets is for *setting limits and alerting*; Cost Explorer is for *analyzing past spending*

---

## Practice questions

**Q1.** A company wants to be notified by email if their monthly AWS bill is predicted to exceed $8,000 before the end of the month. Which service and feature addresses this requirement?

A) AWS Cost Explorer with anomaly detection  
B) AWS Budgets with a forecasted alert  
C) AWS Pricing Calculator  
D) AWS Cost and Usage Report

**Answer: B** — AWS Budgets supports forecasted alerts that trigger when AWS predicts spending will exceed the defined threshold before the billing period ends. This is the "predicted to exceed" requirement. Cost Explorer analyzes historical data. Pricing Calculator estimates future architecture costs. CUR exports billing data to S3.
