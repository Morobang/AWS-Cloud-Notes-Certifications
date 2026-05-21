# Task 1.2: Transform and Process Data

## Learning Objectives
- Configure and run AWS Glue ETL jobs
- Understand Glue Crawlers and the Data Catalog
- Choose between Glue, EMR, Lambda, and Glue DataBrew for transformations
- Apply data format conversions and partitioning strategies

---

## AWS Glue — The Core ETL Service

### What Is Glue?

AWS Glue is a fully managed serverless ETL (Extract, Transform, Load) service. You define the transformation logic, Glue handles the server provisioning, scaling, and execution.

### Glue Components

#### 1. Glue Data Catalog
A central metadata repository — it stores table definitions, schemas, and connection info for all your data sources.

Think of it as the "card catalog" of your data library:
- Every table, column, data type, and location is recorded
- Other services (Athena, EMR, Redshift Spectrum) query it to understand where data lives
- Compatible with Apache Hive Metastore

#### 2. Glue Crawlers
Automated schema discovery. You point a crawler at S3, RDS, DynamoDB, etc., and it:
- Reads sample data
- Infers the schema
- Creates/updates table definitions in the Data Catalog

**Crawler workflow:**
```
Crawler runs → reads data source → infers schema → creates table in Data Catalog
```

Set crawlers on a schedule (e.g., daily) or trigger them when new data arrives.

#### 3. Glue ETL Jobs
Python or Scala scripts that run on Apache Spark under the hood. Glue generates a starter script from the catalog — you can customize it.

**Job types:**
| Type | What it is | When to use |
|---|---|---|
| Spark | Full distributed Spark job | Large datasets, complex transformations |
| Python Shell | Simple Python script on a single node | Small datasets, quick transformations |
| Streaming | Continuous processing of Kinesis or Kafka | Near-real-time ETL |
| Ray | Distributed Python with Ray | Python-native distributed workloads |

**DPU (Data Processing Unit)**: The unit of compute in Glue. 1 DPU = 4 vCPU + 16 GB RAM. More DPUs = faster jobs = higher cost.

#### 4. Glue DataBrew
Visual data preparation tool — no code required. Business analysts can clean, normalize, and transform data through a UI. Useful for:
- One-time data quality fixes
- Teams without Python/Spark skills
- Quick profiling of a new dataset

#### 5. Glue Workflows
Visual orchestration within Glue — chain crawlers, jobs, and triggers into a pipeline. For more complex multi-service orchestration, use Step Functions instead.

### Dynamic Frames vs DataFrames

Glue uses its own abstraction called a **DynamicFrame** (on top of Spark DataFrames):
- Can handle data with inconsistent schemas (schema evolution)
- Built-in functions like `resolveChoice()`, `dropNullFields()`, `unbox()`
- Can be converted to a Spark DataFrame when you need standard Spark APIs

---

## Amazon EMR — Big Data at Scale

### What Is EMR?

Amazon EMR (Elastic MapReduce) is a managed cluster of EC2 instances running open-source big data frameworks — Spark, Hadoop, Hive, Presto, HBase.

### EMR vs Glue

| Feature | AWS Glue | Amazon EMR |
|---|---|---|
| Management | Fully serverless | You manage cluster config |
| Framework | Managed Spark | Spark + Hadoop + Hive + Presto + more |
| Starting speed | Slower (cold start: minutes) | Faster once cluster is running |
| Cost model | Per DPU-hour, billed per second | EC2 instance hours |
| Custom libraries | Limited | Install anything on the cluster |
| Control | Less — Glue abstracts Spark | More — full Spark/Hadoop control |

**Choose Glue when**: You want serverless, you're comfortable with managed Spark, and you don't need custom frameworks.  
**Choose EMR when**: You need Hadoop ecosystem tools, custom libraries, maximum performance control, or very large-scale processing.

### EMR Storage Options

| Storage | Use case |
|---|---|
| **HDFS** (local) | Temporary high-performance storage on the cluster — data lost when cluster terminates |
| **EMRFS (S3)** | Persistent storage — data survives cluster termination. Preferred for most workloads |
| **Amazon EFS** | Shared file system across cluster nodes |

**Best practice**: Store data in S3 (EMRFS), not HDFS. That way you can terminate the cluster when processing is done and restart it fresh for the next job — saving cost.

---

## AWS Lambda for Data Transformation

Lambda is ideal for lightweight, event-driven transformations:

**Use Lambda for data transformation when:**
- Data arrives in S3 (S3 Event → Lambda trigger)
- Records arrive in Kinesis/SQS that need light processing
- Simple data enrichment (lookup, format conversion) under 15 minutes
- Transforming individual records, not massive datasets

**Don't use Lambda for:**
- Processing terabytes of data (no distributed computing)
- Jobs that run longer than 15 minutes
- Complex multi-step Spark transformations

**Common Lambda data pattern:**
```
S3 upload → S3 Event Notification → Lambda → transforms record → writes to DynamoDB/S3
```

---

## Data Formats Deep Dive

### Why Format Choice Matters

Converting data to the right format can reduce query costs by 70-90% and cut query time from minutes to seconds.

### Columnar vs Row-Based

**Row-based** (CSV, JSON, Avro):
- Stores all columns of a row together
- Good for: writing new records, reading full rows
- Bad for: analytical queries that only need a few columns

**Columnar** (Parquet, ORC):
- Stores all values of a column together
- Good for: analytical queries, compression (similar values compress well)
- Bad for: frequent row-level writes

**Example**: A table with 100 columns, 1 billion rows, you query 5 columns.
- CSV: Athena must read ALL 100 columns × 1 billion rows
- Parquet: Athena reads ONLY 5 columns × 1 billion rows → ~20x less data scanned → ~20x cheaper

### Compression Algorithms

| Algorithm | Compression ratio | Speed | Splittable? |
|---|---|---|---|
| GZIP | High | Slow | No (bad for Spark) |
| Snappy | Medium | Fast | No |
| LZO | Medium | Fast | Yes |
| BZIP2 | Very high | Slowest | Yes |

**Splittable** matters for Spark/EMR: if a file isn't splittable, one worker processes the entire file — no parallelism. Parquet and ORC are inherently splittable by row group.

### Format Conversion Best Practice

Always convert raw CSV/JSON (Bronze layer) to Parquet or ORC before storing in the Silver/Gold layers. This is called **data compaction** or **format optimization**.

---

## S3 Partitioning Strategies

Partitioning divides data into folders based on column values, so queries only read relevant partitions.

**Without partitioning:**
```
s3://my-bucket/events/
  └── events.parquet  (12 months of data in one file)
```
→ Every query scans all 12 months, even if you only want January.

**With partitioning by date:**
```
s3://my-bucket/events/
  ├── year=2024/month=01/day=01/events.parquet
  ├── year=2024/month=01/day=02/events.parquet
  └── year=2024/month=02/day=01/events.parquet
```
→ A query for January data only scans `year=2024/month=01/` — massive cost and speed improvement.

**Partition key choices:**
- Date/time: almost always include this
- Region/country: if queries are frequently regional
- Event type: if queries filter by type often
- **Don't over-partition**: too many small files = overhead. Aim for files >128MB per partition

---

## Common Transformation Operations in Glue

```python
# Common Glue DynamicFrame operations

# Read from S3 (Glue Data Catalog table)
datasource = glueContext.create_dynamic_frame.from_catalog(
    database="my_db",
    table_name="raw_events"
)

# Drop null fields
cleaned = datasource.drop_nulls()

# Rename a field
renamed = datasource.rename_field("old_name", "new_name")

# Filter records
from awsglue.transforms import Filter
filtered = Filter.apply(frame=datasource, f=lambda x: x["status"] == "active")

# Convert to Spark DataFrame for complex transformations
df = datasource.toDF()
df_transformed = df.withColumn("new_col", df["col1"] + df["col2"])

# Convert back to DynamicFrame and write to S3
output = DynamicFrame.fromDF(df_transformed, glueContext, "output")
glueContext.write_dynamic_frame.from_options(
    frame=output,
    connection_type="s3",
    connection_options={"path": "s3://my-bucket/silver/events/"},
    format="parquet"
)
```
