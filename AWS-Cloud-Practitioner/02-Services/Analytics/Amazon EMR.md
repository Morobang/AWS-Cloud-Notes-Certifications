# Amazon EMR

## What you'll learn
- What EMR is and what open-source frameworks it runs
- When to use EMR vs other analytics services
- How EMR clusters work
- Key EMR concepts for the exam

---

## What is Amazon EMR?

Amazon EMR (Elastic MapReduce) is a managed big data platform that runs open-source distributed processing frameworks — primarily **Apache Spark** and **Apache Hadoop** — on a cluster of EC2 instances. AWS handles the cluster provisioning, configuration, and scaling; you focus on writing your data processing code.

EMR is designed for processing very large datasets that are too big to handle on a single machine, using distributed computing across a cluster of nodes.

---

## Frameworks EMR supports

| Framework | What it's used for |
|-----------|-------------------|
| Apache Spark | In-memory distributed data processing, ML, streaming |
| Apache Hadoop (MapReduce) | Batch processing of large datasets |
| Apache Hive | SQL-like queries on Hadoop data |
| Apache HBase | NoSQL database on Hadoop |
| Presto | Interactive SQL queries |
| Apache Flink | Real-time stream processing |

---

## How an EMR cluster works

An EMR cluster consists of:
- **Primary node**: Manages the cluster, coordinates task distribution
- **Core nodes**: Store data (HDFS) and run processing tasks
- **Task nodes**: Run processing tasks only (no data storage) — can use Spot Instances

Clusters can be:
- **Long-running**: Persistent cluster for ongoing workloads (data warehouse, interactive queries)
- **Transient**: Spin up, run a job, terminate — cost-effective for batch processing

EMR can store data in HDFS (on the cluster) or in **Amazon S3** (using EMRFS). Using S3 as persistent storage is a common pattern: the cluster can be terminated after each job, and the data remains in S3.

---

## When to use it

**Large-scale batch ETL** — Processing terabytes of raw log data daily using Spark jobs before loading results into Redshift.

**Existing Hadoop/Spark workloads** — You have Spark code running on-premises and want to migrate to AWS without rewriting it.

**Machine learning on big data** — Training ML models on datasets too large for a single machine using Spark MLlib.

**Custom analytics pipelines** — You need fine-grained control over frameworks and cluster configuration that managed services like Glue don't provide.

---

## EMR vs other analytics services

| | EMR | AWS Glue | Amazon Athena |
|-|-----|----------|---------------|
| Framework | Open-source (Spark, Hadoop) | Serverless Spark | Presto SQL |
| Setup | Provision a cluster | No cluster (serverless) | No cluster (serverless) |
| Control | Full cluster control | Managed | Managed |
| Use case | Custom big data jobs | ETL pipelines | SQL queries on S3 |
| Best for | Existing Spark/Hadoop code | Simple ETL without cluster management | Ad-hoc querying |

---

## Exam focus

- EMR = managed cluster for **Spark, Hadoop, and other open-source big data frameworks**
- You manage the cluster (instance types, size, framework versions)
- More control and flexibility than Glue, but requires more configuration
- Can use **Spot Instances** for task nodes to reduce cost
- Works with **S3 as the data lake** — common pattern: read from S3, process, write results back to S3 or Redshift
- Use when the exam mentions: "large-scale data processing," "Spark workloads," "Hadoop migration," or "custom big data processing"

---

## Practice questions

**Q1.** A company has existing Apache Spark jobs that process 10 TB of data daily and want to run them on AWS with minimal code changes. Which service is most appropriate?

A) Amazon Athena  
B) AWS Glue  
C) Amazon EMR  
D) Amazon Kinesis

**Answer: C** — EMR runs Apache Spark natively on managed clusters. Existing Spark code runs without modification. Athena is SQL-only. Glue also runs Spark but with less control and is better for simpler ETL. Kinesis handles streaming data, not batch Spark jobs.
