# Domain 3: Data Operations and Support

**Exam Weight: 22%** — ~14 questions

---

## What This Domain Covers

Running and maintaining data pipelines in production — monitoring, troubleshooting, automating deployments, and analyzing data.

---

## Key Topics

### 1. Monitoring Data Pipelines

**CloudWatch for data pipelines:**
- Glue job metrics: DPUs used, bytes read/written, job duration, error counts
- EMR cluster metrics: HDFS utilization, YARN memory, node health
- Kinesis metrics: IncomingRecords, GetRecords.IteratorAgeMilliseconds (lag indicator), WriteProvisionedThroughputExceeded
- Lambda metrics: Duration, Errors, ConcurrentExecutions, Throttles

**Key Kinesis monitoring metric:**
`GetRecords.IteratorAgeMilliseconds` — measures how far behind the consumer is from the latest record in the stream. If this grows, your consumer isn't keeping up — add more consumers or scale shards.

### 2. Troubleshooting Common Pipeline Issues

| Issue | Diagnosis | Fix |
|---|---|---|
| Glue job OOM (out of memory) | CloudWatch: `glue.driver.aggregate.numFailedTasks`, executor memory errors in logs | Increase `--worker-type` or `--num-workers`, use pushdown predicates to read less data |
| Kinesis throttling | `WriteProvisionedThroughputExceeded` metric | Add more shards (reshard) |
| Slow Athena queries | Query scans too much data | Add S3 partitioning, convert to Parquet, use partition projection |
| Redshift slow queries | Query plan analysis via `EXPLAIN` | Optimize distribution keys, add sort keys, run VACUUM/ANALYZE |
| DMS replication lag | Check replication instance CPU/memory | Scale replication instance, reduce LOB column sizes |

### 3. Data Quality Checks

**AWS Glue Data Quality** (native):
- Define rules in plain language: `"ColumnValues 'age' between 0 and 120"`
- Run as part of a Glue job or independently
- Results stored in Glue Data Catalog, viewable in console

**Other options:**
- **Great Expectations**: open-source, runs on EMR or Glue
- **Deequ**: Amazon's open-source library for large-scale data quality checks (runs on Spark)
- **Pandera**: Python DataFrame validation, good for smaller datasets in Lambda/Glue Python Shell

### 4. Cost Optimization

| Service | Cost optimization strategies |
|---|---|
| Glue ETL | Use pushdown predicates, bookmark jobs to skip processed data, right-size DPUs |
| Athena | Use Parquet/ORC, partition data, use compression, select only needed columns |
| EMR | Use Spot Instances for task nodes, use EMRFS (S3) not HDFS, resize clusters |
| Redshift | Use Redshift Serverless for variable workloads, pause cluster when not in use, use compression |
| Kinesis | Only pay for active shards — don't over-provision |

---

## Task Statements

| Task | Focus |
|---|---|
| [Task 3.1](./Task-3.1-Automate-Data-Processing.md) | Automate data processing and CI/CD for pipelines |
| [Task 3.2](./Task-3.2-Analyze-Data.md) | Analyze and visualize data with Athena, QuickSight |
| [Task 3.3](./Task-3.3-Maintain-Data-Pipelines.md) | Monitor, troubleshoot, and optimize pipelines |

---

## Amazon Athena — Deep Dive

### What It Is
Serverless interactive query service — SQL directly on S3. Uses Presto under the hood.

### Pricing
**$5 per TB of data scanned.** This is why format and partitioning matter so much.

### Performance Optimization
1. **Columnar format** (Parquet/ORC) — reads only needed columns
2. **Partition pruning** — query filters match partition structure → less data scanned
3. **Compression** — Snappy or ZSTD reduces bytes transferred
4. **Partition projection** — pre-compute partition ranges in table config instead of listing S3 objects (dramatically speeds up queries on many partitions)
5. **Result caching** — Athena caches query results for 7 days; identical queries reuse cached results

### Athena Federated Query
Athena can query data sources beyond S3 — RDS, DynamoDB, CloudWatch, even on-premises via Lambda data source connectors. You write the connector as a Lambda function.

---

## Amazon QuickSight

AWS's managed BI (Business Intelligence) and dashboard service.

| Feature | Description |
|---|---|
| **SPICE** | In-memory query engine — imported data is lightning fast |
| **Direct query** | Query Athena/Redshift directly without importing |
| **ML insights** | Anomaly detection, forecasting built-in |
| **Row-level security** | Different users see different data in the same dashboard |
| **Embedded dashboards** | Embed QuickSight dashboards in your own application |

**Connecting to data**: QuickSight can connect to Athena, Redshift, S3, RDS, and many others.
