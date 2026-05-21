# Task 3.2: Analyze and Visualize Data

## Learning Objectives
- Analyze data using Athena, Redshift, and EMR
- Build dashboards and visualizations in Amazon QuickSight
- Apply Amazon Managed Service for Apache Flink for real-time streaming analytics
- Choose the right analytics tool for a given scenario

---

## Analytics Tool Selection

Different analytics tools serve different needs:

| Scenario | Tool |
|---|---|
| Ad-hoc SQL on files in S3, pay per query | Amazon Athena |
| BI dashboards and self-service analytics | Amazon QuickSight |
| Complex multi-table analytics, petabyte-scale | Amazon Redshift |
| Real-time stream aggregations (e.g., per-minute metrics) | Amazon Managed Service for Apache Flink |
| Custom ML analytics, distributed processing | Amazon EMR |
| Full-text search, log dashboards | Amazon OpenSearch |

---

## Amazon Athena — Advanced Patterns

### CTAS (Create Table As Select)

Create a new, optimized table from query results:

```sql
CREATE TABLE silver_events
WITH (
  format = 'PARQUET',
  parquet_compression = 'SNAPPY',
  partitioned_by = ARRAY['year', 'month'],
  external_location = 's3://my-lake/silver/events/'
)
AS SELECT
  event_id,
  user_id,
  event_type,
  amount,
  year(event_date) AS year,
  month(event_date) AS month
FROM raw_events
WHERE event_type != 'TEST';
```

Use CTAS to:
- Convert CSV to Parquet in one SQL statement
- Add partitions to an unpartitioned table
- Filter out bad records as part of the transformation

### INSERT INTO (Incremental Loads)

```sql
INSERT INTO silver_events
SELECT ... FROM raw_events
WHERE event_date = '2024-01-15';
```

Appends new data to an existing Athena/Iceberg table.

### Views in Athena

Athena supports views as saved SQL queries — useful for abstracting complex joins from downstream consumers.

```sql
CREATE VIEW v_customer_orders AS
SELECT c.customer_name, o.order_date, o.amount
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```

### Athena Named Queries

Save frequently-used queries as named queries via API or CloudFormation — useful for automating recurring reports.

### Athena Workgroups for Cost Control

Workgroups enforce per-team cost controls:
- Maximum bytes scanned per query (prevents accidental full-table scans)
- Maximum bytes scanned per workgroup per day
- Separate result buckets per team

---

## Amazon QuickSight

### What It Is

QuickSight is AWS's fully managed BI service — dashboards, charts, ad-hoc analysis. Users connect to data sources, build visualizations, and share interactive dashboards.

### SPICE (Super-fast Parallel In-memory Calculation Engine)

SPICE is QuickSight's in-memory data store. When you import data into SPICE:
- Queries execute in milliseconds (no hitting the database)
- All users query SPICE, not the underlying source (reduces database load)
- Data is refreshed on a schedule (e.g., nightly) or triggered via API

**Direct query mode**: QuickSight queries Athena/Redshift directly without SPICE — results are live but slower.

**When to use SPICE**: High-concurrency dashboards (100+ users), expensive queries, controlled refresh cadence.  
**When to use direct query**: Real-time data where staleness is unacceptable.

### Row-Level Security (RLS)

RLS restricts which rows each user sees in the same dataset:

```
Dataset: sales_data (all regions)

User: alice@company.com → sees only rows where region = 'EMEA'
User: bob@company.com  → sees only rows where region = 'APAC'
```

Configure RLS by creating a permissions table mapping users to allowed values, then attaching it to the dataset.

**Column-level security**: Restrict which columns are visible to specific groups (hide PII columns from junior analysts).

### Embedded Dashboards

QuickSight dashboards can be embedded into your own web application using the Embedding SDK. Use cases:
- SaaS products with per-customer analytics
- Internal portals showing data to business users who don't log into QuickSight directly

### QuickSight Q (NLQ)

Natural language querying — users type plain English: "Show me sales by region last quarter" and QuickSight generates the chart automatically.

### ML Insights

Built-in ML features available without data science expertise:
- **Anomaly detection**: automatically flags unusual data points (e.g., spike in error rates)
- **Forecasting**: predict future values of a time-series metric
- **Auto-narratives**: generate text descriptions of data trends

### Data Sources QuickSight Connects To

- Amazon Athena (most common for data lake analytics)
- Amazon Redshift
- Amazon RDS / Aurora
- Amazon S3 (CSV/JSON files directly)
- Amazon OpenSearch
- Third-party sources (Salesforce, ServiceNow, Jira)

---

## Amazon Managed Service for Apache Flink

### What It Is

Formerly known as Amazon Kinesis Data Analytics for Apache Flink. It's a managed Apache Flink service for real-time streaming analytics — no cluster management needed.

### What Flink Does

Apache Flink is a stateful stream processing framework. Unlike batch jobs that process files, Flink processes records continuously as they arrive.

**Flink streaming concepts:**

| Concept | Description |
|---|---|
| **Data stream** | Continuous, unbounded sequence of records |
| **Window** | Grouping of records by time or count for aggregation |
| **Tumbling window** | Fixed, non-overlapping time segments (e.g., each 5-minute block) |
| **Sliding window** | Overlapping windows (e.g., last 5 minutes, updated every 1 minute) |
| **Session window** | Dynamic windows defined by activity gaps |
| **Watermark** | Mechanism for handling late-arriving data |
| **Stateful processing** | Flink maintains state between records (e.g., running totals) |

### Common Patterns

**Pattern 1: Kinesis → Flink → S3 (micro-batch aggregations)**
```
Kinesis Data Streams (raw clickstream)
    ↓
Managed Flink Application
  - Tumbling window: count events per user per 5 minutes
  - Filter out bot traffic
    ↓
S3 (aggregated, enriched data as Parquet)
    ↓
Athena / Redshift (analytics)
```

**Pattern 2: Kinesis → Flink → Kinesis (stream enrichment)**
```
Kinesis input stream (raw events)
    ↓
Flink (enrich: JOIN with reference data in DynamoDB)
    ↓
Kinesis output stream (enriched events)
    ↓
Multiple consumers (alerting, analytics, storage)
```

### Flink vs Lambda for Streaming

| | Managed Flink | Lambda (Kinesis trigger) |
|---|---|---|
| State management | Built-in (checkpointing, fault-tolerant) | You manage state externally |
| Complex windows | Yes (tumbling, sliding, session) | Limited |
| Throughput | Very high | Limited by concurrency |
| SQL support | Yes (Flink SQL) | No |
| Use case | Complex stateful streaming analytics | Simple per-record transforms |

---

## Amazon EMR for Analytics

EMR provides access to the full Hadoop ecosystem for large-scale analysis:

### Interactive Analysis with EMR Notebooks

EMR Studio provides Jupyter-like notebooks connected to running EMR clusters. Data scientists can:
- Run PySpark interactively against data in S3
- Prototype transformations before putting them in a scheduled job
- Visualize results with Matplotlib, Pandas, or bokeh

### Presto/Trino on EMR

EMR can run Presto or Trino — distributed SQL query engines that can query S3, Hive tables, Cassandra, and more in one query. Use when Athena's capabilities aren't enough or you need custom connectors.

### Hive on EMR

Apache Hive provides HiveQL (SQL-like) over HDFS/S3 data. Glue Data Catalog is compatible with Hive Metastore, so Hive on EMR can use the same table definitions as Athena and Glue.

---

## Redshift for Analytics

### Redshift Spectrum for Data Lake Queries

Run Redshift SQL queries that span warehouse (Redshift) and lake (S3) data:

```sql
-- Join warehouse customer table with S3 log table
SELECT c.customer_name, COUNT(*) AS event_count
FROM redshift_table.customers c
JOIN spectrum_schema.s3_events e ON c.customer_id = e.customer_id
WHERE e.event_date >= '2024-01-01'
GROUP BY c.customer_name;
```

The Redshift cluster handles query planning; Spectrum workers in S3 process the lake data.

### Redshift Materialized Views

Pre-compute and cache the results of expensive queries:

```sql
CREATE MATERIALIZED VIEW daily_sales_summary AS
SELECT DATE_TRUNC('day', order_date) AS day,
       region,
       SUM(amount) AS total_sales
FROM orders
GROUP BY 1, 2;

-- Refresh daily
REFRESH MATERIALIZED VIEW daily_sales_summary;
```

Dashboards query the materialized view (fast) instead of the raw table (slow).

---

## Exam Scenarios

**"A company wants to create self-service dashboards for 500 business users without exposing them to SQL or Athena."**
→ Import data into QuickSight SPICE → build dashboards → publish to users

**"A streaming pipeline processes Kinesis events. You need to count unique users per 5-minute window and detect anomalies in real time."**
→ Amazon Managed Service for Apache Flink (stateful windowed aggregations)

**"Analysts need to query S3 data lake tables AND Redshift warehouse tables in a single SQL query."**
→ Redshift Spectrum — mount S3 tables as external schemas in Redshift

**"A QuickSight dashboard shows data that is 24 hours old. Business wants near-real-time data."**
→ Switch from SPICE with daily refresh to Direct Query mode (or increase refresh frequency)

**"Regional managers should see only their region's data in the same QuickSight dashboard."**
→ Enable Row-Level Security with a user-region permissions table
