# Domain 1: Data Ingestion and Transformation

**Exam Weight: 34%** — This is the biggest domain. Expect ~22 questions.

---

## What This Domain Covers

Data ingestion is getting data INTO your data platform. Transformation is cleaning, converting, and reshaping it. Together they form the "E" and "T" in ETL (Extract, Transform, Load).

As a data engineer, you'll spend most of your time designing and maintaining the pipelines that:
1. **Ingest** raw data from various sources (APIs, databases, IoT devices, clickstreams)
2. **Transform** it into usable formats for analysts and ML models

---

## The Big Picture — Data Pipeline Flow

```
Data Sources          Ingestion              Transform            Store
─────────────         ─────────────          ──────────           ──────────
• Databases     →     • DMS                  • Glue ETL     →     • S3 (Data Lake)
• APIs          →     • Kinesis Streams      • Lambda             • Redshift
• IoT devices   →     • Kinesis Firehose     • EMR Spark          • DynamoDB
• Logs          →     • MSK (Kafka)          • Step Functions     • OpenSearch
• Files         →     • DataSync / Snow                           • RDS/Aurora
```

---

## Batch vs Streaming — The Most Important Distinction

### Batch Processing
- Data is collected over a period and processed all at once
- Examples: process yesterday's sales data every night at midnight
- Tools: AWS Glue, Amazon EMR, AWS Batch
- Latency: minutes to hours (acceptable for historical analysis)
- Lower cost per record processed

### Streaming Processing
- Data is processed continuously as it arrives, in real time or near-real time
- Examples: fraud detection on each credit card transaction, real-time dashboards
- Tools: Kinesis Data Streams, Kinesis Data Analytics, MSK (Kafka)
- Latency: milliseconds to seconds
- Higher cost, more complex

**The exam loves asking you to choose between batch and streaming.** If the scenario involves:
- "real-time", "near-real-time", "as events occur" → Streaming
- "daily reports", "historical analysis", "nightly jobs" → Batch

---

## Task Statements in This Domain

| Task | Focus |
|---|---|
| [Task 1.1](./Task-1.1-Perform-Data-Ingestion.md) | Perform data ingestion — choose the right service for the source and pattern |
| [Task 1.2](./Task-1.2-Transform-Data.md) | Transform and process data — Glue, Lambda, EMR, data formats |
| [Task 1.3](./Task-1.3-Orchestrate-Data-Pipelines.md) | Orchestrate data pipelines — Step Functions, Glue Workflows, MWAA |

---

## Key Services for Domain 1

| Service | Purpose | Batch or Stream? |
|---|---|---|
| Amazon Kinesis Data Streams | Real-time data streaming ingestion | Stream |
| Amazon Kinesis Data Firehose | Managed delivery of streams to S3/Redshift/OpenSearch | Stream |
| Amazon MSK | Managed Apache Kafka | Stream |
| AWS Glue | Serverless ETL — discover, catalog, transform data | Batch (+ Streaming) |
| Amazon EMR | Big data processing with Spark/Hadoop at scale | Batch (+ Streaming) |
| AWS Lambda | Lightweight transformation in response to events | Both |
| AWS DMS | Migrate databases to AWS, replicate ongoing changes | Batch + CDC |
| AWS DataSync | Transfer large datasets from on-premises to S3/EFS | Batch |
| AWS Step Functions | Orchestrate multi-step data workflows | Orchestration |
| Amazon MWAA | Managed Apache Airflow for complex DAG pipelines | Orchestration |
| AWS Snow Family | Physical data transfer for massive offline datasets | Batch |

---

## Data Formats — Know These Cold

| Format | Description | When to Use |
|---|---|---|
| **CSV** | Plain text, comma-separated, no schema | Simple data, human-readable, imports |
| **JSON** | Nested key-value, flexible schema | APIs, semi-structured data |
| **Parquet** | Columnar, compressed, schema embedded | Analytics queries (Athena, Redshift Spectrum) — preferred |
| **ORC** | Columnar, highly compressed | Hive/EMR workloads — similar to Parquet |
| **Avro** | Row-based, schema evolution support | Kafka/streaming, schema registry |

**Key exam point**: Parquet and ORC are columnar formats — they dramatically reduce query costs in Athena because you only read the columns you query. If you SELECT 3 columns from a 100-column CSV, Athena reads the entire file. With Parquet, it reads only those 3 columns.

---

## The Medallion Architecture (Bronze/Silver/Gold)

This is the standard data lake pattern tested on the exam:

| Layer | Name | What it contains | Quality |
|---|---|---|---|
| Raw | **Bronze** | Exact copy of source data, unmodified | Low — could be corrupt, duplicates, nulls |
| Validated | **Silver** | Cleaned, deduplicated, schema-enforced | Medium — validated, enriched |
| Curated | **Gold** | Business-ready aggregates and features | High — optimized for analytics/ML |

Benefits:
- Raw data is never lost (you can always reprocess from Bronze)
- Progressively higher quality as you move through layers
- Each layer has different access controls (analysts only touch Gold)

---

## Common Exam Scenarios

**"A company receives 10,000 IoT sensor readings per second and needs to store them in S3 with no data loss"**  
→ Kinesis Data Firehose (buffers and delivers to S3 automatically)

**"A data engineer needs to move a 500 TB on-premises Oracle database to AWS Aurora with minimal downtime"**  
→ AWS DMS (Database Migration Service) with CDC (Change Data Capture)

**"A company wants to automatically discover the schema of new CSV files dropped in S3"**  
→ AWS Glue Crawler

**"An analytics team queries S3 data and costs are too high because they scan entire files"**  
→ Convert to Parquet/ORC format + add S3 partitioning by date

---

## Start Here

Work through the task statements in order:
1. [Task 1.1: Perform Data Ingestion](./Task-1.1-Perform-Data-Ingestion.md)
2. [Task 1.2: Transform and Process Data](./Task-1.2-Transform-Data.md)
3. [Task 1.3: Orchestrate Data Pipelines](./Task-1.3-Orchestrate-Data-Pipelines.md)
