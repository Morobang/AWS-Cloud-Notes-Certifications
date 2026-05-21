# Amazon Athena

## What you'll learn
- What Athena is and how it differs from a traditional database
- When to use Athena vs Redshift vs Glue
- How Athena integrates with S3 and the Glue Data Catalog
- How Athena is priced

---

## What is Amazon Athena?

Athena is a serverless, interactive query service that runs SQL directly against data stored in Amazon S3. There is no database to set up, no cluster to manage, and no data to load. You point Athena at your S3 data, define a schema (or let Glue discover it), and run standard SQL queries.

Athena uses **Presto** under the hood and supports standard ANSI SQL. It can query data in CSV, JSON, Parquet, ORC, Avro, and other formats.

---

## Key features

- **Serverless**: No infrastructure to provision, patch, or manage
- **Query in place**: Data stays in S3 — no ETL or loading required
- **Standard SQL**: Uses familiar SQL syntax (SELECT, JOIN, GROUP BY, etc.)
- **Pay per query**: Billed based on the amount of data scanned, not per hour
- **Glue integration**: Works with the AWS Glue Data Catalog for table definitions and schema discovery
- **Multiple formats**: Supports CSV, JSON, Parquet, ORC, Avro, and columnar formats
- **Federated queries**: Can query data in other sources (RDS, DynamoDB, on-premises) through connectors

---

## When to use it

**Ad-hoc analysis** — A data analyst wants to explore log files stored in S3 without setting up a database. Athena lets them run SQL immediately.

**One-time queries** — You need to investigate a specific incident in application logs for the past 30 days. Running one Athena query is faster than building an ETL pipeline.

**Cost-sensitive querying** — You only need to run queries occasionally. Athena charges per TB scanned, so infrequent queries are far cheaper than running a database 24/7.

**Pre-pipeline exploration** — Before building a full data pipeline, analysts use Athena to understand the shape and quality of raw data in S3.

---

## How pricing works

You are charged **per TB of data scanned** by each query. There is no charge when no queries are running. Pricing is approximately $5 per TB scanned.

To reduce costs and improve performance:
- Use **columnar formats** (Parquet, ORC) — Athena only reads the columns a query needs
- Use **partitioning** — structure your S3 data by date/region so Athena skips irrelevant partitions
- Compress data — compressed files are smaller, so less data is scanned

---

## Comparison: Athena vs Redshift vs Glue

| | Athena | Redshift | AWS Glue |
|-|--------|----------|----------|
| Purpose | Query data in S3 | Data warehouse (OLAP) | ETL — move and transform data |
| Data location | S3 (query in place) | Redshift cluster (loaded) | S3, RDS, Redshift, etc. |
| Setup | None | Cluster provisioning | Serverless |
| Pricing | Per TB scanned | Per hour (cluster) | Per DPU-hour |
| Best for | Ad-hoc, occasional queries | Complex analytics at scale | Building data pipelines |

---

## Exam focus

- Athena = serverless SQL queries on S3 data, **no ETL required**
- Pricing is **per TB scanned** — not per query, not per hour
- Use Parquet/ORC format to reduce scan costs and improve performance
- Integrates with **AWS Glue Data Catalog** to discover and manage table schemas
- Key differentiator from Redshift: Athena queries data **where it lives in S3**; Redshift requires loading data first
- Key differentiator from Glue: Athena **queries** data; Glue **transforms and moves** data

---

## Practice questions

**Q1.** A data analyst stores 3 years of application logs in S3 as compressed JSON files. They need to run occasional SQL queries to investigate anomalies. They want to minimize setup time and cost. Which service is most appropriate?

A) Amazon Redshift  
B) AWS Glue  
C) Amazon Athena  
D) Amazon EMR

**Answer: C** — Athena queries S3 data directly with no setup, no cluster, and no loading required. It charges only for data scanned, making it cost-effective for occasional queries. Redshift requires loading data into a cluster. Glue is an ETL service, not a query tool. EMR requires provisioning a cluster.

---

**Q2.** A company uses Athena to query large CSV files in S3. Query costs are high because Athena is scanning the entire dataset. What is the most effective way to reduce scan costs?

A) Move the data to Amazon Redshift  
B) Convert the CSV files to Parquet format  
C) Use Amazon EMR instead  
D) Increase the size of the query

**Answer: B** — Converting to Parquet (a columnar format) allows Athena to read only the columns referenced in the query instead of the entire row. This significantly reduces the amount of data scanned and therefore the cost. Moving to Redshift changes the architecture entirely. EMR doesn't reduce Athena scan costs.
