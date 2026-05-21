# Amazon Redshift

**Category**: Data Warehouse  
**Exam weight**: High — core service in Domain 2 and analytics scenarios

---

## What Is It?

Amazon Redshift is a fully managed, petabyte-scale data warehouse. It's designed for OLAP (Online Analytical Processing) — complex SQL queries over large historical datasets for BI and reporting.

Not a transactional database. Not for OLTP (frequent row-level inserts/updates/deletes).

---

## Architecture

```
                    ┌──────────────────┐
Client SQL query →  │   Leader Node    │  ← Query planning, compilation
                    └────────┬─────────┘
                             │ Distributes work
              ┌──────────────┼──────────────┐
        ┌─────┴─────┐  ┌─────┴─────┐  ┌────┴──────┐
        │ Compute   │  │ Compute   │  │ Compute   │
        │ Node 1    │  │ Node 2    │  │ Node N    │
        └─────┬─────┘  └─────┬─────┘  └─────┬─────┘
              │              │               │
        ┌─────┴─────┐  ┌─────┴─────┐  ┌─────┴─────┐
        │  Storage  │  │  Storage  │  │  Storage  │
        │ (local or │  │ (RA3: S3) │  │ (RA3: S3) │
        │   disk)   │  └───────────┘  └───────────┘
        └───────────┘
```

**Leader node**: Receives client queries, parses SQL, builds execution plan, distributes work to compute nodes.

**Compute nodes**: Execute the parallel query segments. Store data in columnar format.

**Node types:**
- **RA3**: Decoupled storage (data in S3) and compute (EC2) — recommended for most workloads
- **DC2**: Dense compute — local SSD storage, best for high-performance fixed datasets

---

## Key Design Concepts

### Distribution Styles

How rows are distributed across compute nodes:

| Style | Algorithm | When to use |
|---|---|---|
| `AUTO` | Redshift chooses automatically | Default — start here |
| `EVEN` | Round-robin across nodes | Large tables with no common join key |
| `KEY` | Hash of a column value | Tables frequently joined on this column — collocates matching rows |
| `ALL` | Full copy on every node | Small dimension tables joined to many fact tables |

**Goal**: Minimize data movement during joins. If two large tables are `KEY` distributed on the same join column, joins happen locally on each node without network transfer.

### Sort Keys

Like an index — data is stored physically sorted by the sort key. Dramatically speeds up range queries.

| Type | Description |
|---|---|
| `Compound` | Sort by column1, then column2, etc. — efficient for queries that filter on prefix columns |
| `Interleaved` | Equal weight to all key columns — better when queries filter on different columns |

**Rule of thumb**: Sort key on `date` column if you frequently query by date range.

### Columnar Storage + Compression

Redshift stores data in columns, not rows. Compression is applied per column based on the data type.

Run `ANALYZE COMPRESSION` to get column-level compression recommendations.

---

## Loading Data into Redshift

### COPY Command (Preferred)

```sql
COPY orders
FROM 's3://my-bucket/orders/'
IAM_ROLE 'arn:aws:iam::123456789:role/RedshiftS3Role'
FORMAT PARQUET;
```

The COPY command is massively parallel — each compute node reads files in parallel from S3. Far faster than single-row INSERTs.

**Supports**: S3 (CSV, JSON, Parquet, ORC), DynamoDB, EMR HDFS, remote hosts

### AWS Glue → Redshift

Glue jobs can write directly to Redshift using the `jdbc` connection type with the Glue Redshift connector. Under the hood, it uses S3 staging + COPY.

### Kinesis Firehose → Redshift

Firehose can deliver streaming data to Redshift: it stages data in S3 first, then issues a COPY command automatically.

---

## Querying External Data — Redshift Spectrum

Query data in S3 directly from Redshift without loading it:

```sql
-- Create external schema pointing to Glue Data Catalog
CREATE EXTERNAL SCHEMA my_lake
FROM DATA CATALOG
DATABASE 'my_glue_db'
IAM_ROLE 'arn:aws:iam::123456789:role/RedshiftSpectrumRole';

-- Join Redshift table with S3 data
SELECT r.customer_id, r.amount, l.region
FROM orders r                        -- in Redshift
JOIN my_lake.customers l             -- in S3 via Spectrum
ON r.customer_id = l.customer_id
WHERE r.date >= '2024-01-01';
```

**Use cases:**
- Query historical archive data without loading it into the warehouse
- Join warehouse data with data lake data
- Reduce Redshift storage costs by keeping old data in S3

---

## Maintenance Commands

| Command | What it does | When to run |
|---|---|---|
| `VACUUM` | Reclaims space after DELETEs, re-sorts rows | After bulk deletes or unsorted loads |
| `ANALYZE` | Updates column statistics used by the query planner | After large loads |
| `VACUUM REINDEX` | Re-indexes interleaved sort keys | Periodically on tables with many updates |

Redshift now runs `VACUUM` automatically — but manual runs may be needed for large delete operations.

---

## Redshift Serverless

No cluster to manage — Redshift scales automatically based on query demand:

- Pay per RPU-second (Redshift Processing Unit)
- Auto-pause when idle
- Good for: variable/unpredictable workloads, dev/test environments, BI dashboards with intermittent usage
- Limitation: less control over performance tuning vs provisioned cluster

---

## Monitoring Redshift Performance

Key views and tables:

```sql
-- Active queries
SELECT * FROM SVL_QUERY_SUMMARY WHERE userid > 1;

-- Query execution plan
EXPLAIN SELECT ...;

-- Disk space usage
SELECT * FROM SVV_DISKUSAGE;

-- Table design
SELECT * FROM SVV_TABLE_INFO WHERE "table" = 'my_table';
```

**CloudWatch metrics:**
- `CPUUtilization`: node CPU
- `DatabaseConnections`: active connections
- `ReadIOPS` / `WriteIOPS`: I/O performance
- `QueriesCompletedPerSecond`: throughput

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Load 1 TB of S3 CSV data into Redshift fast | COPY command with PARALLEL ON |
| Query S3 archive data from Redshift without loading | Redshift Spectrum |
| Tables joined frequently on `customer_id` | KEY distribution on `customer_id` |
| Small lookup table joined to all fact tables | ALL distribution |
| Queries on date ranges run slowly | Add `date` as sort key |
| Variable BI workload, don't want to manage cluster | Redshift Serverless |
| Real-time streaming data → Redshift | Kinesis Firehose → S3 → COPY (Firehose handles it) |
