# Task 1.1: Perform Data Ingestion

## Learning Objectives
- Select the appropriate ingestion service for a given data source and latency requirement
- Configure Kinesis Data Streams and Firehose for streaming ingestion
- Use AWS DMS for database migration and ongoing CDC
- Understand batch vs streaming trade-offs

---

## Ingestion Patterns

### Pattern 1: Streaming Ingestion (Real-Time)

Use when: data arrives continuously, you need near-real-time processing (seconds to minutes latency).

#### Amazon Kinesis Data Streams

**What it is**: A real-time data streaming service. Think of it as a massive, durable pipe that data flows through.

**Key concepts**:
- **Shard**: The unit of capacity. 1 shard = 1 MB/s write, 2 MB/s read, 1000 records/s write
- **Partition key**: Determines which shard a record goes to. Same partition key = same shard = ordered delivery
- **Sequence number**: AWS assigns this — each record in a shard is numbered in order
- **Retention**: 24 hours default (up to 365 days)
- **Consumers**: Applications that read from the stream (Lambda, Kinesis Analytics, custom EC2 app)

**When to scale**: Add shards (reshard) when your throughput exceeds current shard capacity.

**Architecture pattern**:
```
IoT devices → Kinesis Data Streams → Lambda function → DynamoDB
                                   → Kinesis Analytics → Real-time alerts
                                   → Kinesis Firehose → S3 (long-term storage)
```

#### Amazon Kinesis Data Firehose

**What it is**: Managed delivery service. It captures, transforms, and delivers streaming data to destinations. No consumer application needed — it handles delivery automatically.

**Destinations**: S3, Amazon Redshift, Amazon OpenSearch, Splunk, HTTP endpoints

**Key differences from Kinesis Streams**:
| Feature | Kinesis Data Streams | Kinesis Firehose |
|---|---|---|
| Consumer app needed? | Yes — you write consumer code | No — automatic delivery |
| Transformation | Must do it yourself | Built-in Lambda transform |
| Latency to destination | Real-time (ms) | Near-real-time (buffer: 60-900s or 1-128 MB) |
| Management | Manual shard management | Fully managed, auto-scales |
| Use case | Custom real-time processing | Deliver to storage/analytics |

**Buffering**: Firehose buffers data before delivering. You set the buffer size (MB) or interval (seconds) — whichever triggers first, it delivers. This is why it's "near-real-time" not real-time.

#### Amazon MSK (Managed Streaming for Apache Kafka)

**What it is**: Fully managed Apache Kafka service. If your team already uses Kafka or you have Kafka producers/consumers, use MSK instead of rewriting everything for Kinesis.

**When to choose MSK over Kinesis**:
- You're already using Apache Kafka on-premises
- You need Kafka-specific features (consumer groups, exactly-once semantics, topic compaction)
- You want open-source compatibility — avoids vendor lock-in

**When to choose Kinesis over MSK**:
- Greenfield AWS project with no existing Kafka
- Want fully serverless (MSK Serverless exists but has limits)
- Simpler operational model

---

### Pattern 2: Batch Ingestion

Use when: data can be collected and processed periodically (hours or days latency acceptable).

#### AWS Database Migration Service (DMS)

**What it does**: Migrates databases to AWS and can continuously replicate changes.

**Two modes**:
1. **Full load**: One-time migration of all existing data
2. **CDC (Change Data Capture)**: Ongoing replication of insert/update/delete changes after the initial load

**Source → Target compatibility**:
- Source: Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, S3, and more
- Target: RDS, Aurora, Redshift, DynamoDB, S3, OpenSearch, and more

**Components**:
- **Replication Instance**: EC2 instance that runs the migration
- **Endpoints**: Source and target database connection configurations
- **Task**: The migration job (full load, CDC, or both)

**Exam tip**: For "migrate Oracle to Aurora with minimal downtime" → DMS full load + CDC, cut over during a maintenance window.

#### AWS DataSync

**What it does**: Transfers data from on-premises storage (NAS, SMB shares, HDFS) or other cloud providers to AWS storage (S3, EFS, FSx).

**Key features**:
- Automatically handles encryption, data validation, scheduling
- Can transfer at up to 10 Gbps
- Runs over AWS Direct Connect or internet

**DMS vs DataSync**:
- **DMS** = migrate/replicate **databases** (structured, SQL queries needed)
- **DataSync** = transfer **files/objects** (unstructured, just moving data)

#### AWS Snow Family

For when the internet is too slow — physical devices shipped to you:

| Device | Capacity | Use case |
|---|---|---|
| **Snowcone** | 8 TB HDD / 14 TB SSD | Edge computing, small migrations |
| **Snowball Edge** | 80 TB (Storage Optimized) | Large migrations, edge compute |
| **Snowmobile** | Up to 100 PB | Exabyte-scale data center migration |

**When to use Snow vs Direct Connect vs internet transfer?**
- Rule of thumb: if it would take more than a week to transfer over your internet connection, use Snow

---

### Pattern 3: Database Replication and CDC

**CDC (Change Data Capture)** captures row-level changes (INSERT, UPDATE, DELETE) from a database transaction log and streams them to a target. Critical for keeping data warehouses in sync with operational databases without full reloads.

AWS services that support CDC:
- **AWS DMS** — CDC from most relational databases
- **Amazon RDS** / **Aurora** → can publish bin log changes
- **DynamoDB Streams** — CDC natively for DynamoDB tables
- **Amazon MSK Connect** → Kafka Connect for CDC with Debezium

---

## Choosing the Right Ingestion Service

| Scenario | Service |
|---|---|
| Real-time IoT sensor data at high throughput | Kinesis Data Streams |
| Deliver logs to S3/Redshift without writing consumer code | Kinesis Data Firehose |
| Already have Kafka producers in your team | Amazon MSK |
| Migrate on-prem Oracle DB to Aurora | AWS DMS |
| Transfer 100TB of files from NAS to S3 | AWS DataSync |
| 50PB data center migration | AWS Snowmobile |
| 80TB migration, internet connection would take weeks | Snowball Edge |

---

## Key Numbers to Remember

- Kinesis Shard: **1 MB/s write, 2 MB/s read, 1,000 records/s**
- Kinesis default retention: **24 hours** (extendable to 365 days)
- Kinesis Firehose buffer: **60–900 seconds** or **1–128 MB** (whichever triggers first)
- DMS replication instance: runs on EC2 (you choose size based on workload)
- Snowball Edge: **80 TB** usable storage
