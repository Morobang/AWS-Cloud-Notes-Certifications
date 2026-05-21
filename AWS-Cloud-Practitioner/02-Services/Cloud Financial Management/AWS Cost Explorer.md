# AWS Cost Explorer

## What you'll learn
- What Cost Explorer is and what it shows
- Key Cost Explorer features including right-sizing
- How Cost Explorer differs from AWS Budgets
- Key exam facts

---

## What is AWS Cost Explorer?

AWS Cost Explorer is a visualization and analysis tool that shows you **historical** AWS spending data and usage patterns. It provides pre-built and custom reports so you can understand where your money is being spent, identify trends, and make informed decisions about cost optimization.

---

## What it shows

- Up to **12 months of historical** cost and usage data
- Costs broken down by: **service** (EC2, S3, RDS), **region**, **account** (in an Organization), **usage type**, or **cost allocation tag**
- **Daily, monthly, or hourly** granularity
- **Forecasts** for the next 12 months based on historical trends
- **Reserved Instance and Savings Plans** utilization and coverage reports

---

## Key features

### Cost analysis reports
Pre-built reports include monthly costs by service, daily costs, and costs by linked account. Custom reports let you filter and group by any dimension.

### Right-sizing recommendations
Cost Explorer identifies EC2 instances that are consistently underutilized (low CPU, memory) and recommends downsizing to a cheaper instance type. Each recommendation shows the estimated monthly savings.

### Cost anomaly detection
Cost Explorer can detect unusual spending spikes and send alerts when spending deviates significantly from historical patterns. For example: a Lambda function in a loop running up unexpected charges.

### Savings Plans recommendations
Based on your On-Demand usage history, Cost Explorer recommends Compute or EC2 Instance Savings Plans and shows the estimated savings.

---

## Cost Explorer vs AWS Budgets

| | AWS Cost Explorer | AWS Budgets |
|-|-------------------|-------------|
| Direction | Backward-looking (analyze what happened) | Forward-looking (alert before overspend) |
| Action | Provides reports and recommendations | Sends alerts, triggers automation |
| Data | Historical (up to 12 months) | Current spend vs threshold |
| Best for | "Where did our money go this quarter?" | "Alert me before I exceed $X" |

Both are used together: Budgets prevents surprises; Cost Explorer explains them.

---

## When to use it

**Cost investigation** — After an unexpectedly high bill, use Cost Explorer to identify which service or account caused the spike.

**Trend analysis** — Review the past 6 months of spending by service to understand growth patterns.

**Right-sizing** — Use the recommendations report to identify oversized EC2 instances and calculate potential savings from downsizing.

**RI and Savings Plans management** — Monitor utilization to ensure purchased reservations are being used efficiently.

---

## Exam focus

- Cost Explorer = **analyze and visualize historical costs** — backward-looking
- Up to **12 months** of historical data + 12-month forecast
- **Right-sizing recommendations** for EC2 (identifies underutilized instances)
- **Cost anomaly detection** for unexpected spend spikes
- Key differentiator from Budgets: Cost Explorer *analyzes* the past; Budgets *limits* the future
- Key differentiator from CUR: Cost Explorer is a UI/reporting tool; CUR is a raw data export

---

## Practice questions

**Q1.** A DevOps team wants to identify which AWS services have been the most expensive over the past 6 months and find EC2 instances that could be downsized to reduce costs. Which tool should they use?

A) AWS Budgets  
B) AWS Pricing Calculator  
C) AWS Cost Explorer  
D) AWS Cost and Usage Report

**Answer: C** — Cost Explorer provides historical cost breakdowns by service with up to 12 months of data, and includes right-sizing recommendations that identify underutilized EC2 instances. Budgets sets limits and sends alerts. Pricing Calculator estimates future architecture costs. CUR exports raw line-item data that requires additional tools to analyze.
