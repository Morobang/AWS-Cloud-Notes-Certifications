# Amazon Athena

**Category**: Serverless SQL Analytics  
**Exam weight**: High — the go-to tool for querying S3 data lakes

---

## What Is It?

Amazon Athena is a serverless, interactive SQL query service for data stored in Amazon S3. No cluster to provision — you write SQL, Athena runs it, you pay per TB scanned.

Under the hood: managed Presto/Trino cluster.

---

## How It Works

```
1. Your data is in S3 (CSV, JSON, Parquet, ORC, Avro, etc.)
2. Schema is defined in Glue Data Catalog (or you create tables in Athena directly)
3. You write a SQL query in Athena console or via API
4. Athena spins up distributed compute, reads from S3, returns results
5. Results are stored in an S3 output location
6. You pay $5 per TB of data scanned
```

---

## Pricing

**$5 per TB scanned**

This makes format and partitioning optimization directly translate to cost savings:

| Data format | Compression | Cost for same query |
|---|---|---|
| CSV (uncompressed) | None | $5.00 baseline |
| CSV (GZIP) | ~5:1 | ~$1.00 |
| Parquet (Snappy) | ~10:1 columnar | ~$0.10–0.30 |
| ORC (ZLIB) | ~12:1 columnar | ~$0.08–0.25 |

**Partitioning example savings**: A 1TB table partitioned by date. Querying one day = scan 1/365 of the data ≈ 2.7 GB → $0.01 instead of $5.00.

---

## Creating Tables

### Option 1: Use Glue Data Catalog

If Glue has already crawled your data, the tables are automatically available in Athena. Just select the database.

### Option 2: Create Table Manually

```sql
CREATE EXTERNAL TABLE events (
  event_id STRING,
  user_id STRING,
  event_type STRING,
  amount DOUBLE,
  created_at TIMESTAMP
)
PARTITIONED BY (year INT, month INT, day INT)
STORED AS PARQUET
LOCATION 's3://my-lake/events/'
TBLPROPERTIES ('parquet.compress'='SNAPPY');

-- Load partition metadata
MSCK REPAIR TABLE events;
-- OR add partitions manually:
ALTER TABLE events ADD PARTITION (year=2024, month=1, day=15)
LOCATION 's3://my-lake/events/year=2024/month=1/day=15/';
```

---

## Performance Optimization

### 1. Use Columnar Formats
Convert CSV/JSON to Parquet or ORC. Athena reads only the columns you query.

### 2. Partition Your Data
Structure your S3 data in Hive-style partitions:
```
s3://my-lake/events/year=2024/month=01/day=15/data.parquet
```
Queries with `WHERE year=2024 AND month=01` only scan that partition.

### 3. Partition Projection
Pre-compute partition values in the table definition — Athena doesn't need to list S3 objects:

```sql
CREATE EXTERNAL TABLE events (...)
PARTITIONED BY (year INT, month INT, day INT)
STORED AS PARQUET
LOCATION 's3://my-lake/events/'
TBLPROPERTIES (
  "projection.enabled" = "true",
  "projection.year.type" = "integer",
  "projection.year.range" = "2020,2030",
  "projection.month.type" = "integer",
  "projection.month.range" = "1,12",
  "projection.day.type" = "integer",
  "projection.day.range" = "1,31",
  "storage.location.template" = "s3://my-lake/events/year=${year}/month=${month}/day=${day}/"
);
```
Massive performance improvement for tables with many partitions.

### 4. Compress Data
Snappy (splittable with Parquet), ZSTD, or GZIP (non-splittable, avoid for large files).

### 5. File Size Optimization
- Too many small files = high overhead (Athena makes many S3 GET requests)
- Target: files between **128 MB and 1 GB**
- Use Glue or Spark to compact small files before querying

---

## Athena Workgroups

Workgroups separate query execution and billing:
- Different teams get different workgroups
- Per-workgroup cost controls (max data scanned per query, per workgroup)
- Result location per workgroup
- Query history per workgroup

```sql
-- Enforce max 10 GB scan per query in a workgroup
-- (set in console, not SQL — just shown as concept)
```

---

## Athena Federated Queries

Athena can query data sources beyond S3 using Lambda data source connectors:
- Amazon DynamoDB
- Amazon RDS / Aurora
- Amazon CloudWatch Logs
- On-premises databases via custom Lambda connector
- Third-party sources

```sql
-- Query DynamoDB from Athena using federated connector
SELECT *
FROM "lambda:dynamodb".mydb.orders
WHERE order_date > '2024-01-01';
```

---

## Athena for Apache Spark (Athena Spark)

Interactive Spark notebooks within Athena — no EMR cluster needed:
- Write PySpark code in a notebook interface
- Athena provisions serverless Spark under the hood
- Good for: data exploration, prototyping Spark transformations

---

## Result Caching

Athena caches query results for 7 days. If you run the exact same query again, Athena returns the cached result without scanning S3 again — zero cost.

Enable "Reuse previous query results" in workgroup settings.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Cheapest way to query S3 data with SQL | Amazon Athena |
| Athena costs too much — query is scanning entire table | Add partitioning + convert to Parquet |
| Schema of S3 CSV files needs to be defined for Athena | Glue Crawler → creates table in Glue Data Catalog |
| Multiple teams, need to track query costs per team | Use Athena Workgroups |
| Need to join S3 events with DynamoDB user profiles in one query | Athena Federated Query with DynamoDB connector |
| Table has 1000 partitions and queries list them slowly | Enable Partition Projection |
| Heavy computation, need full Spark API | Athena for Apache Spark (or EMR) |
