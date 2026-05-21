# AWS Certified Data Engineer – Associate (DEA-C01)

> Study guide for the DEA-C01 exam. Assumes you already hold the AWS Cloud Practitioner and have basic familiarity with AWS.

---

## Exam Overview

| Item | Details |
|---|---|
| Exam code | DEA-C01 |
| Level | Associate |
| Duration | 130 minutes |
| Questions | 65 (plus ~15 unscored) |
| Passing score | 720 / 1000 |
| Format | Multiple choice + Multiple response |
| Prerequisite | None (but CCP + cloud experience strongly recommended) |

## Domains & Weights

| Domain | Title | Weight |
|---|---|---|
| Domain 1 | Data Ingestion and Transformation | 34% |
| Domain 2 | Store and Manage Data | 26% |
| Domain 3 | Data Operations and Support | 22% |
| Domain 4 | Data Security and Governance | 18% |

**Domain 1 is the biggest** — nearly a third of the exam. Kinesis, Glue, Lambda, and data pipeline design will show up constantly.

---

## Folder Structure

```
AWS-Data-Engineer-Associate/
├── README.md                          ← You are here
├── Cheat-Sheet.md                     ← Quick reference (all services + concepts)
├── Notes.md                           ← Personal takeaways and memory tricks
├── 01-Lessons/
│   ├── Domain-1-Data-Ingestion-Transformation/
│   │   ├── README.md
│   │   ├── Task-1.1-Perform-Data-Ingestion.md
│   │   ├── Task-1.2-Transform-Data.md
│   │   └── Task-1.3-Orchestrate-Data-Pipelines.md
│   ├── Domain-2-Store-Manage-Data/
│   │   ├── README.md
│   │   ├── Task-2.1-Choose-Data-Store.md
│   │   ├── Task-2.2-Transform-Data-Stores.md
│   │   └── Task-2.3-Manage-Data-Lifecycle.md
│   ├── Domain-3-Data-Operations-Support/
│   │   ├── README.md
│   │   ├── Task-3.1-Automate-Data-Processing.md
│   │   ├── Task-3.2-Analyze-Data.md
│   │   └── Task-3.3-Maintain-Data-Pipelines.md
│   └── Domain-4-Data-Security-Governance/
│       ├── README.md
│       ├── Task-4.1-Apply-Authentication-Authorization.md
│       ├── Task-4.2-Ensure-Data-Encryption.md
│       └── Task-4.3-Comply-with-Data-Privacy.md
├── 02-Services/
│   ├── Ingestion/
│   │   ├── Amazon-Kinesis-Data-Streams.md
│   │   ├── Amazon-Kinesis-Data-Firehose.md
│   │   ├── Amazon-MSK.md
│   │   ├── AWS-Database-Migration-Service.md
│   │   └── AWS-DataSync.md
│   ├── Processing/
│   │   ├── AWS-Glue.md
│   │   ├── Amazon-EMR.md
│   │   ├── AWS-Lambda.md
│   │   └── AWS-Step-Functions.md
│   ├── Storage/
│   │   ├── Amazon-S3.md
│   │   ├── Amazon-Redshift.md
│   │   ├── Amazon-DynamoDB.md
│   │   ├── Amazon-RDS-Aurora.md
│   │   └── Amazon-OpenSearch.md
│   ├── Analytics/
│   │   ├── Amazon-Athena.md
│   │   ├── Amazon-QuickSight.md
│   │   └── AWS-Lake-Formation.md
│   └── Orchestration/
│       ├── Amazon-MWAA.md
│       └── AWS-Step-Functions.md
└── 03-Practice-Questions/
    ├── README.md
    ├── Domain-1-Ingestion-Transformation.md
    ├── Domain-2-Store-Manage.md
    ├── Domain-3-Operations-Support.md
    └── Domain-4-Security-Governance.md
```

---

## How Data Engineer Differs from Cloud Practitioner

| CCP | DEA |
|---|---|
| Knows S3 exists | Knows S3 storage classes, partitioning strategies, lifecycle policies, and formats (Parquet vs JSON vs ORC) |
| Knows Kinesis exists | Builds streaming pipelines with Kinesis, handles shards, partition keys, retention |
| Knows Glue exists | Writes Glue ETL jobs, configures crawlers, uses Glue Data Catalog, optimizes DPUs |
| Knows Lambda exists | Uses Lambda as a transformation step in data pipelines |
| Knows Redshift exists | Designs Redshift schemas, optimizes distribution keys, uses Spectrum |

The DEA exam tests IMPLEMENTATION, not just recognition.

---

## Key Data Engineering Concepts to Master

1. **Data pipeline architecture** — batch vs streaming, Lambda architecture, Kappa architecture
2. **Data formats** — CSV, JSON, Parquet, ORC, Avro — when to use each
3. **Data partitioning** — how to partition S3 data for query performance
4. **ETL vs ELT** — when to transform before or after loading
5. **Data lake vs data warehouse** — S3 + Athena vs Redshift
6. **Data catalog** — Glue Data Catalog, Lake Formation permissions
7. **Streaming** — Kinesis Data Streams, Firehose, MSK, their differences
8. **Orchestration** — Step Functions, MWAA (Managed Airflow), Glue Workflows

---

## Study Path (Recommended Order)

1. Domain 1 → Ingestion & Transformation (heaviest domain, start here)
2. Domain 2 → Storage & Data Management  
3. Domain 4 → Security & Governance (overlaps with CCP knowledge)
4. Domain 3 → Operations & Support (monitoring, troubleshooting)
5. Review Cheat-Sheet + Notes
6. Practice Questions per domain
7. Full mock exam
