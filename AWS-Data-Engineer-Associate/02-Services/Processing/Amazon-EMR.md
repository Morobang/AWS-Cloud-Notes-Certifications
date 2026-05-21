# Amazon EMR (Elastic MapReduce)

**Category**: Managed Big Data Processing  
**Exam weight**: High — the choice when Glue isn't enough or you need the full Hadoop ecosystem

---

## What Is It?

Amazon EMR is a managed cluster platform for running big data frameworks — Apache Spark, Hadoop, Hive, Presto/Trino, HBase, Flink, and others. You get the full power of open-source big data tools without managing cluster infrastructure yourself.

EMR provisions EC2 instances, installs frameworks, configures the cluster, and handles scaling and monitoring.

---

## EMR Architecture

### Node Types

| Node type | Role | Data stored? | Can use Spot? |
|---|---|---|---|
| **Master node** | Coordinates the cluster (YARN ResourceManager, HDFS NameNode) | Yes (HDFS metadata) | No — losing master = cluster failure |
| **Core node** | Runs tasks and stores HDFS data | Yes | Risky — losing core node loses HDFS data |
| **Task node** | Runs tasks only — no data storage | No | Yes — safe to lose |

**Best practice**: Use Spot Instances for task nodes only. Master and core nodes should be On-Demand or Reserved.

### HDFS vs EMRFS

| Storage | What it is | Behavior on cluster termination |
|---|---|---|
| **HDFS** | Local disk storage on cluster nodes | Data LOST when cluster terminates |
| **EMRFS** | S3 access from EMR (uses s3:// paths) | Data PERSISTS — S3 is durable |

**Use EMRFS (S3) for all production workloads.** Store data in S3, process with EMR, terminate the cluster when done. Restart fresh for the next run.

HDFS is only for temporary intermediate data within a single job (Spark shuffle data, temporary tables).

---

## EMR Deployment Options

### EMR on EC2 (Classic)

Provisions an EC2 cluster. You manage instance types, counts, and Auto Scaling.

### EMR on EKS

Run EMR Spark jobs on an existing Kubernetes (EKS) cluster. Use when you have existing EKS infrastructure and want to share it between EMR and other workloads.

### EMR Serverless

No cluster management — submit Spark or Hive jobs; EMR automatically provisions workers and scales to zero when jobs complete.

| | EMR on EC2 | EMR Serverless |
|---|---|---|
| Cluster management | You configure | None |
| Start time | 5-15 minutes | Fast (workers pre-initialized) |
| Cost | EC2 instance hours | Per vCPU-hour while running |
| Control | High | Lower |
| Use case | Long-running clusters, complex tuning | Variable/intermittent jobs |

---

## Key EMR Frameworks

### Apache Spark on EMR

The most common EMR use case. Spark runs distributed Python (PySpark), Scala, Java, or R workloads.

```python
# PySpark job reading from S3 and writing back
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("EMRJob").getOrCreate()

# Read Parquet from S3 (EMRFS)
df = spark.read.parquet("s3://my-lake/bronze/events/")

# Transform
result = df.filter(df.amount > 0) \
           .groupBy("region", "event_type") \
           .agg({"amount": "sum", "event_id": "count"})

# Write back to S3
result.write.mode("overwrite") \
    .partitionBy("region") \
    .parquet("s3://my-lake/silver/events-summary/")
```

### Apache Hive on EMR

HiveQL (SQL-like syntax) for large-scale batch queries over HDFS/S3. Less performant than Spark SQL for complex operations, but familiar to SQL users.

### Presto / Trino on EMR

Interactive query engine — runs sub-minute SQL over S3 data without pre-loading. EMR's Presto is an alternative to Athena for ad-hoc queries when you need custom connectors or configuration.

### Apache HBase on EMR

Wide-column NoSQL database running on HDFS. Use for high-volume random read/write of semi-structured data at scale. Less common — Amazon DynamoDB is usually preferred on AWS.

---

## Bootstrapping

EMR bootstrap actions run custom scripts before the framework starts:

```bash
#!/bin/bash
# Install custom Python packages
pip3 install pandas scikit-learn boto3
```

Bootstrapping is how you install custom dependencies that aren't part of the default EMR distribution.

---

## EMR Auto Scaling

**Managed Scaling** (recommended): EMR automatically adjusts the number of core and task nodes based on YARN memory usage. Set min/max nodes and let EMR scale.

**Custom Auto Scaling**: Define scaling policies triggered by EMR CloudWatch metrics.

---

## EMR Security

**Kerberos**: Authentication for Hadoop services — required for clusters that access sensitive data.

**IAM roles**:
- EC2 instance profile on the cluster nodes (what the cluster can access in AWS)
- EMR service role (what EMR service can do on your behalf to manage EC2)

**Encryption:**
- At rest: S3 encryption (SSE-KMS via EMRFS), local disk encryption with EBS
- In transit: TLS for data moving between cluster nodes and between cluster and S3

**EMR security configurations**: Reusable security templates — define encryption settings, Kerberos config, and IAM roles once; apply to multiple clusters.

---

## EMR vs Glue

| | AWS Glue | Amazon EMR |
|---|---|---|
| Management | Fully serverless | Manage cluster (or use EMR Serverless) |
| Framework | Managed Spark only | Spark + Hadoop + Hive + Presto + HBase + Flink |
| Custom libraries | Limited (wheel files via S3) | Install anything via bootstrap |
| Performance tuning | Limited | Full Spark/Hadoop configuration |
| Cold start | 2-10 minutes | 5-15 minutes (cluster boot) |
| Glue Data Catalog | Built-in | Optional (EMRFS + Glue catalog integration) |
| Use case | ETL pipelines, Glue Data Catalog integration | Large-scale processing, custom frameworks |

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Run Apache Hive queries on petabytes of S3 data | Amazon EMR with Hive |
| Need to install custom C++ extension libraries on Spark workers | Amazon EMR (Glue can't install system packages) |
| Process streaming data with Apache Flink at scale | Amazon EMR with Flink (or Managed Apache Flink for serverless) |
| Serverless Spark jobs, variable workload, minimal management | EMR Serverless |
| Data scientists need interactive Jupyter notebooks connected to Spark | EMR Studio |
| Reduce EMR cost for batch jobs that can tolerate interruptions | Spot Instances for task nodes |
