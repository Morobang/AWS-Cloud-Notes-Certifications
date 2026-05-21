# Task 2.2: Understand Data Store Schemas, Design, and Optimization

## Learning Objectives
- Design schemas for Redshift (star/snowflake, distribution keys, sort keys)
- Apply DynamoDB single-table design principles
- Understand schema evolution with Glue, Parquet, and Apache Iceberg
- Choose between normalization and denormalization for analytics workloads

---

## Redshift Schema Design

### Star Schema vs Snowflake Schema

Analytical queries in Redshift almost always use dimensional modeling.

**Star schema**: One large fact table at the center, surrounded by denormalized dimension tables.

```
          dim_customer
               |
dim_product — fact_sales — dim_date
               |
          dim_store
```

- `fact_sales`: contains measurable events (sale_amount, quantity)
- `dim_customer`: customer attributes (name, region, segment)
- Queries join fact + dimensions: `SELECT region, SUM(amount) FROM fact_sales JOIN dim_customer ...`

**Snowflake schema**: Dimension tables are further normalized into sub-dimensions.

```
dim_customer → dim_region → dim_country
```

For Redshift: prefer star schema (fewer JOINs = faster queries). The storage cost of denormalization is worth the query speed.

---

### Redshift Distribution Styles

Distribution style controls how rows are spread across compute nodes. The goal: put related data on the same node to minimize data movement during JOINs.

| Style | How it works | When to use |
|---|---|---|
| `KEY` | Rows with the same key value go to the same node | Large fact tables — distribute by the JOIN key |
| `ALL` | Full copy of the table on every node | Small dimension tables — no data movement needed |
| `EVEN` | Rows distributed round-robin | Large tables with no obvious JOIN key |
| `AUTO` | Redshift decides — starts with ALL for small, EVEN for large | Default — let Redshift optimize |

**Best practice**: Join `fact_sales` (KEY on `customer_id`) with `dim_customer` (KEY on `customer_id`) → both tables collocated on same node → JOIN needs zero data movement.

### Redshift Sort Keys

Sort keys define the physical sort order on disk, like an index in traditional databases.

**Compound sort key**: `SORTKEY(year, month, day)` — optimal for queries that filter in prefix order (`WHERE year = 2024 AND month = 1`).

**Interleaved sort key**: Equal weight to all columns — better for ad-hoc queries filtering on any single column. Higher maintenance cost (VACUUM takes longer).

**Choosing a sort key**: Use the column that appears most in WHERE clauses. For time-series data, `created_at` or `event_date` is almost always the right choice.

### VACUUM and ANALYZE

Redshift doesn't immediately free space from DELETEs/UPDATEs — it marks rows as deleted.

- `VACUUM FULL table_name` — reclaims space, re-sorts rows, rebuilds zone maps
- `VACUUM DELETE ONLY` — reclaims space without re-sorting (faster)
- `ANALYZE table_name` — updates query planner statistics (run after large data loads)

**Redshift Serverless and RA3** run automatic vacuuming — less maintenance overhead.

### WLM (Workload Management)

Redshift queues manage concurrent queries. Configure WLM to:
- Separate BI dashboard queries (low concurrency, fast) from ETL loads (high concurrency, slow)
- Assign memory percentages per queue
- Set query timeouts and concurrency limits

---

## DynamoDB Data Modeling

### The Single-Table Design Pattern

DynamoDB's best practice is to store multiple entity types in ONE table. This contrasts sharply with relational design.

**Why single-table?**: Each DynamoDB API call is one network round trip. In a relational app, fetching a user + their orders + their profile requires 3 JOINs (or 3 queries). In DynamoDB, with proper key design, you fetch all three with one query.

**Generic key pattern:**

| PK | SK | Attributes |
|---|---|---|
| `USER#u001` | `#METADATA#u001` | name, email, created_at |
| `USER#u001` | `ORDER#2024-01-15#o001` | amount, status |
| `USER#u001` | `ORDER#2024-01-16#o002` | amount, status |

Query: `PK = USER#u001 AND SK begins_with ORDER#2024-01` → returns all January orders for user u001.

### Access Pattern First Design

**Step 1**: List ALL access patterns your application needs before designing the schema.  
**Step 2**: Design keys and indexes to serve every access pattern.

If an access pattern can't be served by the primary key, create a Global Secondary Index (GSI).

**GSI projection types:**
- `KEYS_ONLY` — cheapest, only key attributes projected into the index
- `INCLUDE` — specific additional attributes
- `ALL` — all attributes (most expensive, most flexible)

### DynamoDB Streams + Lambda Pattern

```
DynamoDB write → Streams → Lambda → process → write to S3 / OpenSearch / another DynamoDB table
```

Common use: maintain a derived view (e.g., write a flattened summary to S3 for analytics every time the source table changes).

---

## Schema Evolution

### The Problem

Real-world data changes over time. A new field gets added to a JSON payload. A column gets renamed. Old and new records have different shapes. Schema evolution is how you handle this without breaking existing pipelines.

### Parquet Schema Evolution

Parquet files are self-describing — each file stores its own schema. If you add a new column:

- Old files: column is missing → readers return `null` for that column
- New files: column exists → readers return the value

This backward compatibility is why Parquet is the preferred format for data lakes. New columns can be added without reprocessing old files.

**Schema merging in Athena/Glue**: Enable `schema_merging=true` so that when files with different schemas coexist in a partition, Athena merges them into a superset schema.

### Apache Iceberg — ACID Transactions on S3

Apache Iceberg is an open table format for data lakes. It solves problems that raw S3 + Parquet cannot:

| Problem | Iceberg solution |
|---|---|
| Update/delete individual rows in S3 | Copy-on-write or merge-on-read updates |
| Schema evolution without rewriting files | Metadata tracks schema changes per snapshot |
| Time travel (query data as of yesterday) | Snapshots stored in Iceberg metadata |
| ACID transactions | Optimistic concurrency with Iceberg catalog |
| Reading consistent data while writing | Snapshot isolation |

**How it works**: Iceberg maintains a metadata layer (manifest files + snapshot metadata) on top of Parquet files. Writers commit a new snapshot atomically.

```
s3://my-lake/
├── metadata/
│   ├── v1.metadata.json         ← snapshot 1 schema + file list
│   └── v2.metadata.json         ← snapshot 2 (after an update)
└── data/
    ├── partition=2024-01/file1.parquet
    └── partition=2024-01/file2.parquet
```

**AWS support for Iceberg:**
- Glue Data Catalog: native Iceberg table support
- Athena: read/write Iceberg tables with `INSERT INTO`, `UPDATE`, `DELETE`, `MERGE INTO`
- EMR: Spark with Iceberg connector
- Amazon S3 Tables: purpose-built Iceberg table storage (optimized for Iceberg workloads)

**When to use Iceberg**: Any data lake workload needing row-level updates, deletes, time travel, or concurrent read/write consistency.

### Avro Schema Evolution

Avro schemas are JSON-defined and stored separately from the data. Avro supports:
- Adding new optional fields (with defaults) — backward compatible
- Removing optional fields — forward compatible

Avro is commonly used with Kafka/MSK, where the schema is registered in a **Schema Registry**. Every message references a schema ID → the consumer can always decode old and new messages correctly.

---

## Glue Data Catalog Schema Management

When a Glue Crawler re-runs on S3 data that has changed:

- New columns detected → added to the table definition (additive change, safe)
- Column removed → Glue marks it, existing files still have the data
- Column type changed → potential incompatibility — Glue flags this

**Schema version history**: Glue stores every schema version for a table. You can see the history in the console or via API.

**Updating schemas manually:**
```python
import boto3
glue = boto3.client('glue')

glue.update_table(
    DatabaseName='my_db',
    TableInput={
        'Name': 'events',
        'StorageDescriptor': {
            'Columns': [
                {'Name': 'event_id', 'Type': 'string'},
                {'Name': 'user_id', 'Type': 'string'},
                {'Name': 'new_column', 'Type': 'int'}  # Added column
            ],
            ...
        }
    }
)
```

---

## Normalization vs Denormalization

| Approach | Relational OLTP | Analytical OLAP |
|---|---|---|
| Goal | Eliminate redundancy, safe writes | Maximize read speed, minimize JOINs |
| Approach | Normalize (3NF, BCNF) | Denormalize (star schema, wide flat tables) |
| JOINs | Many, handled efficiently by row-store | Minimize — columnar stores optimize for few JOINs |
| Updates | Fast — update one place | Slow — must update every copy |

**For data engineering**: Data arrives normalized (from OLTP databases). The ETL process denormalizes it for the analytical layer (Redshift star schema or Parquet flat files for Athena).

---

## Exam Scenarios

**"A Redshift query joining the fact table (2B rows) to a dimension table (10K rows) is slow."**
→ Change dimension table distribution to `ALL` — replicate it to every node, eliminating data movement.

**"You need to store product catalog data in DynamoDB. Products can be looked up by product_id or by category+brand combination."**
→ Primary key: `product_id`. GSI: partition key = `category`, sort key = `brand`.

**"Old data files are CSV with 10 columns. New files are Parquet with 12 columns. Athena queries return null for the new columns on old rows."**
→ Expected Parquet schema evolution behavior — this is correct. Enable schema merging if needed.

**"A data lake on S3 needs to support row-level deletes (GDPR erasure) and time travel to query data as of last week."**
→ Migrate tables to Apache Iceberg format using Athena or Glue.
