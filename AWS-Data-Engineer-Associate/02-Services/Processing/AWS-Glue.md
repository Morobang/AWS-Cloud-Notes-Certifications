# AWS Glue

**Category**: Serverless ETL / Data Integration  
**Exam weight**: Very High — core service across Domain 1 and Domain 2

---

## What Is It?

AWS Glue is a fully serverless data integration service for ETL (Extract, Transform, Load). It discovers, catalogs, prepares, moves, and transforms data between data stores.

---

## Glue Components Map

```
AWS Glue
├── Data Catalog          → Metadata repository (schemas, tables, partitions)
│   ├── Databases         → Logical grouping of tables
│   └── Tables            → Schema definitions + S3 location
├── Crawlers              → Auto-discover schema, update Data Catalog
├── Connections           → Database connection configs (JDBC, S3, Kafka)
├── ETL Jobs              → Spark/Python scripts that transform data
│   ├── Spark jobs        → Distributed processing
│   ├── Python Shell      → Single-node Python
│   ├── Streaming         → Kinesis/Kafka → S3/Redshift continuous
│   └── Ray               → Distributed Python with Ray
├── Workflows             → Orchestrate crawlers + jobs
├── DataBrew              → Visual no-code data preparation
├── Data Quality          → Rule-based data validation
└── Triggers              → Start jobs on schedule or event
```

---

## Glue Data Catalog

The central metadata store for your data lake:

- **Databases**: Logical namespace (like a database name)
- **Tables**: Schema definition — column names, types, S3 path, partition structure
- **Partitions**: Partition values and their S3 locations

**Hive Metastore compatibility**: Glue Data Catalog is compatible with Apache Hive Metastore. EMR, Athena, and Redshift Spectrum all read from it.

**Cross-account sharing**: A central account can own the catalog; other accounts query it via Lake Formation.

---

## Glue Crawlers

### How a Crawler Works
1. You define the data source (S3 path, database, etc.)
2. Crawler reads sample data (first 10+ files)
3. Classifiers determine the format (CSV, JSON, Parquet, etc.)
4. Crawler infers schema
5. Creates/updates tables in the Data Catalog

### Crawler Scheduling
- On demand (manual trigger)
- Scheduled (cron expression)
- Event-triggered (via Step Functions or Lambda)

### Handling Schema Changes
- If new columns appear: Glue can add them to the existing table
- If columns are removed: Configurable — update or ignore
- If incompatible changes: Glue creates a new table

---

## Glue ETL Jobs

### Job Types

| Type | Runtime | Use case |
|---|---|---|
| Spark (G.1X, G.2X, G.4X) | Apache Spark 3.x | Large datasets, complex transforms |
| Python Shell | Python 3 | Small data, boto3 calls, simple transforms |
| Streaming | Spark Streaming | Continuous processing from Kinesis/Kafka |
| Ray | Ray distributed | Python ML workloads at scale |

### DPU Sizing

| Worker Type | vCPU | Memory | Use for |
|---|---|---|---|
| G.025X | 2 | 4 GB | Development, small jobs |
| G.1X | 4 | 16 GB | Standard production jobs |
| G.2X | 8 | 32 GB | Memory-intensive joins |
| G.4X | 16 | 64 GB | Very large datasets |
| G.8X | 32 | 128 GB | Largest workloads |

**Cost**: You pay per DPU-hour, billed per second (after 10 min minimum).

### Job Bookmarks

Glue tracks which data has already been processed. If a job is re-run, it only processes NEW files/records, not already-processed ones.

Enable when:
- Processing incremental S3 files (new files added daily)
- Don't want to reprocess old data

Disable when:
- Reprocessing historical data intentionally
- Using your own tracking mechanism

### Pushdown Predicates

Filter data at the source before pulling it into Spark. Dramatically reduces data read for partitioned data.

```python
# Without pushdown — reads ALL data, then filters in Spark
df = spark.read.parquet("s3://my-lake/events/")
df_filtered = df.filter(df.date == "2024-01-15")

# With pushdown — reads ONLY the 2024-01-15 partition from S3
datasource = glueContext.create_dynamic_frame.from_catalog(
    database="my_db",
    table_name="events",
    push_down_predicate="date = '2024-01-15'"  # Only reads this partition
)
```

---

## Glue Dynamic Frames

Glue's own data structure (built on Spark DataFrames) with extra features for messy data:

| Operation | What it does |
|---|---|
| `resolveChoice()` | Handle ambiguous column types |
| `dropNullFields()` | Remove fields with all null values |
| `unbox()` | Unpack a nested field |
| `flatten()` | Flatten nested JSON structure |
| `toDF()` | Convert to Spark DataFrame |
| `fromDF()` | Convert back from Spark DataFrame |

---

## Common Glue Patterns

### Pattern 1: Bronze → Silver ETL
```python
# Read raw CSV from Bronze layer
raw = glueContext.create_dynamic_frame.from_catalog(
    database="bronze", table_name="orders_raw"
)

# Clean and transform
from awsglue.transforms import DropNullFields, Filter

cleaned = DropNullFields.apply(raw)
valid_orders = Filter.apply(cleaned, f=lambda x: x["amount"] > 0)

# Write Parquet to Silver layer (partitioned by date)
glueContext.write_dynamic_frame.from_options(
    valid_orders,
    connection_type="s3",
    connection_options={
        "path": "s3://my-lake/silver/orders/",
        "partitionKeys": ["year", "month", "day"]
    },
    format="parquet"
)
```

### Pattern 2: Read from RDS, Write to S3
```python
# Requires a Glue Connection to RDS
orders = glueContext.create_dynamic_frame.from_catalog(
    database="my_db",
    table_name="rds_orders"
)

# Write to S3
glueContext.write_dynamic_frame.from_options(
    orders,
    connection_type="s3",
    connection_options={"path": "s3://my-lake/orders/"},
    format="parquet"
)
```

---

## AWS Glue DataBrew

Visual data preparation — no code required:
- **Profiling**: Automatically analyze column statistics (nulls, duplicates, distributions)
- **Recipes**: List of transformation steps applied to data
- **Projects**: A workspace combining a dataset and a recipe
- **PII handling**: Built-in actions to detect and mask/redact PII

Use DataBrew for:
- Quick data quality assessment on a new dataset
- One-time cleaning tasks
- Non-technical team members doing data prep

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Auto-discover schema of new S3 CSV files | Glue Crawler |
| Central schema store for Athena + Redshift + EMR | Glue Data Catalog |
| Process 10 TB of data daily with Spark | Glue Spark job (G.1X or G.2X workers) |
| Process new files only, skip already-processed | Enable Job Bookmarks |
| Reduce cost of Glue job reading partitioned S3 | Use pushdown predicates |
| Non-technical analyst needs to clean data | AWS Glue DataBrew |
| Continuous S3 → Redshift loading | Glue Streaming job |
