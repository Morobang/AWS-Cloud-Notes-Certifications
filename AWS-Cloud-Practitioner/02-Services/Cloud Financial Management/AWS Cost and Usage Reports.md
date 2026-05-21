# AWS Cost and Usage Reports (CUR)

## What you'll learn
- What the Cost and Usage Report is and what it contains
- How CUR differs from Cost Explorer
- How organizations use CUR
- Key exam facts

---

## What is the AWS Cost and Usage Report?

The AWS Cost and Usage Report (CUR) is the **most granular billing data** AWS provides. It generates detailed CSV or Parquet files containing line-item cost and usage data for every AWS resource in your account, delivered to an S3 bucket on a daily or monthly schedule.

CUR is not a dashboard or visualization tool — it is a raw data export intended to be loaded into analytics tools for custom reporting.

---

## What CUR contains

| Data type | Detail |
|-----------|--------|
| Line-item costs | Individual resource cost per hour or day |
| Usage type | Specific usage dimension (e.g., "BoxUsage:t3.medium") |
| Cost allocation tags | User-defined tag values per resource |
| Reserved Instance amortization | RI costs spread over usage periods |
| Savings Plans utilization | Savings Plans coverage per line item |
| Resource IDs | Specific EC2 instance IDs, S3 bucket names, etc. |

---

## How it's used

CUR delivers files to S3, where they can be queried and analyzed:

- **Amazon Athena**: Run SQL queries against CUR files in S3 for flexible ad-hoc analysis
- **Amazon QuickSight**: Build cost dashboards from Athena queries on CUR data
- **Amazon Redshift**: Load CUR data into Redshift for complex organizational cost analytics
- **Third-party BI tools**: Tableau, Power BI, Grafana, and others can connect to CUR data via S3

---

## CUR vs Cost Explorer vs Budgets

| | CUR | Cost Explorer | AWS Budgets |
|-|-----|---------------|-------------|
| Type | Raw data export | Visual analysis tool | Alert service |
| Granularity | Most detailed (per resource, per hour) | Summary and trends | Threshold-based |
| Access | Files in S3 | Console/API | Console/API |
| Best for | Custom reports, BI tools, charge-backs | Quick analysis and recommendations | Proactive alerts |

---

## When to use it

**Charge-backs** — A company with multiple teams allocates costs by department using cost allocation tags in CUR, then reports back to each department their AWS spending.

**Custom BI dashboards** — Finance builds a cost dashboard in QuickSight by querying CUR data in Athena.

**Long-term storage** — CUR files in S3 provide a permanent audit trail of all AWS costs.

**Third-party cost tools** — Many third-party cost management tools (CloudHealth, Apptio) ingest CUR as their primary data source.

---

## Exam focus

- CUR = **most detailed billing data** — line-item per resource, exported to S3
- Delivered as **CSV or Parquet** files to an S3 bucket
- Use with **Athena** (query), **QuickSight** (visualize), or **Redshift** (warehouse)
- Includes **cost allocation tag** values for each resource
- Key differentiator from Cost Explorer: CUR is raw data for building custom reports; Cost Explorer is a built-in visualization tool
- Use when the exam mentions: "detailed billing data," "line-item costs," "charge-backs," "custom cost analytics"

---

## Practice questions

**Q1.** A company's finance team needs to build custom monthly cost reports broken down by team and project, using cost allocation tags. They want to feed this data into their existing Tableau BI platform. Which AWS service provides the data?

A) AWS Cost Explorer  
B) AWS Budgets  
C) AWS Cost and Usage Reports  
D) Amazon QuickSight

**Answer: C** — CUR exports detailed line-item billing data (including cost allocation tag values) to S3, from which it can be consumed by Tableau or other BI tools. Cost Explorer is a built-in AWS tool that doesn't export data to Tableau. Budgets sets alerts. QuickSight is an AWS-native BI tool that would require CUR as its data source anyway.
