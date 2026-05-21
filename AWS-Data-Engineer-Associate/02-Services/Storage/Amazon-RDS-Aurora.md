# Amazon RDS and Aurora

**Category**: Managed Relational Database  
**Exam weight**: Medium — know when to choose RDS/Aurora as a data source or target, and how to integrate with analytics pipelines

---

## Amazon RDS

### What Is It?

Amazon RDS is a managed relational database service. AWS handles patching, backups, Multi-AZ failover, and hardware provisioning. You choose the engine: MySQL, PostgreSQL, MariaDB, Oracle, or SQL Server.

### Key Features for Data Engineers

**Automated backups**: Retained for 1–35 days. Point-in-time recovery (PITR) to any second within the retention window.

**Read replicas**: Create up to 5 read replicas for read scaling or analytics workloads. Replicas can be in a different Region.

**Multi-AZ**: Synchronous standby in a second AZ. Automatic failover in under 2 minutes. Not the same as a read replica — Multi-AZ standby is not queryable.

**Enhanced Monitoring**: OS-level metrics (CPU, memory, I/O) with 1-second granularity.

**DMS integration**: RDS is a supported source and target for AWS DMS migrations and CDC replication.

---

## Amazon Aurora

### What Is It?

Aurora is AWS's cloud-native relational database — MySQL and PostgreSQL compatible but built on a different storage architecture.

### Aurora Architecture

**Shared distributed storage**: A 6-way replicated storage volume (2 copies in each of 3 AZs). All Aurora instances (writer + replicas) share this storage.

```
Writer instance          Replica 1         Replica 2
     ↓                       ↓                 ↓
  ┌──────────────────────────────────────────────────┐
  │         Aurora Storage (6 replicas, 3 AZs)        │
  └──────────────────────────────────────────────────┘
```

**Key consequences:**
- Replicas read from the same data as the writer — no replication lag between them
- Auto-grows in 10 GB increments up to 128 TB — no storage management
- Failover to a replica completes in ~30 seconds (no data copy needed)

### Aurora vs RDS

| Feature | Aurora | RDS MySQL/PostgreSQL |
|---|---|---|
| Storage | Shared, auto-grows | Fixed, manual scaling |
| Replicas | Up to 15 (all share storage) | Up to 5 (each has its own copy) |
| Failover time | ~30 seconds | ~2 minutes |
| Performance | Up to 5x MySQL, 3x PostgreSQL | Standard |
| Storage replication | 6 copies across 3 AZs | 2 copies with Multi-AZ |
| Cost | Higher | Lower |

**For new applications**: Aurora is almost always better than RDS MySQL/PostgreSQL.

### Aurora Serverless v2

Aurora Serverless v2 auto-scales compute (ACUs — Aurora Capacity Units) in fractions of a second:
- Scales down to 0.5 ACU when idle, up to 128 ACU under load
- Pay only for compute consumed
- Useful for: development environments, unpredictable traffic, infrequently used applications

### Aurora Global Database

One primary Region, up to 5 secondary Regions:
- Secondary Regions have read replicas with <1-second replication lag
- In a disaster, a secondary can be promoted to a new primary in under 1 minute
- Use for: globally distributed applications, cross-region DR

### Aurora Multi-Master (MySQL)

Multiple write nodes in different AZs — all can accept writes simultaneously. Use for applications needing continuous write availability across AZ failures.

---

## RDS/Aurora in Data Engineering Pipelines

### RDS as a CDC Source

Use DMS to capture changes from RDS and stream them to analytics:

```
RDS PostgreSQL (production OLTP)
    ↓ AWS DMS (CDC via logical replication)
Amazon Kinesis Data Streams
    ↓ Lambda / Managed Flink
Amazon S3 / Redshift (analytics)
```

### Aurora read replica for Analytics

Create a read replica dedicated to analytics queries — isolates production OLTP from heavy analytical loads:

```
Aurora Writer (production writes)
    └── Aurora Reader (analytics) → Athena Federated Query / direct JDBC
```

### RDS Proxy

Connection pooling for RDS/Aurora — prevents connection storms from Lambda:

```
100 Lambda functions (each opens a connection) → RDS Proxy (pools) → RDS (5 connections)
```

Without RDS Proxy, Lambda functions that burst to thousands of concurrent executions would overwhelm RDS's connection limit.

---

## Aurora Machine Learning Integration

Aurora can call Amazon SageMaker and Amazon Comprehend directly from SQL:

```sql
-- Classify sentiment of customer reviews using Comprehend
SELECT review_id,
       aws_comprehend_detect_sentiment(review_text, 'en') AS sentiment
FROM customer_reviews;
```

Use for enriching data at query time without a separate ETL step.

---

## Backup and Restore

### Automated Backups

- Daily backup window + continuous transaction log backup
- PITR: restore to any second in the backup retention period (1–35 days)
- Backups stored in S3 (managed by AWS — not visible in your S3 console)

### Manual Snapshots

- Taken at any time; retained until you delete them (no automatic expiration)
- Can be copied cross-Region for DR
- Can be shared with other AWS accounts

### Aurora Backtrack

Rewind an Aurora cluster to a point in the past **without restoring from a backup**:
- Works in seconds (not minutes like a restore)
- Effective window: up to 72 hours back
- Use for: "I accidentally deleted a table — take me back 10 minutes"

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| MySQL application needs high availability with automatic failover | Amazon Aurora MySQL with Multi-AZ (or Aurora Global for cross-region) |
| Stream production PostgreSQL changes to a Redshift data warehouse | DMS CDC from RDS PostgreSQL to Redshift |
| Lambda functions spamming RDS with too many connections | RDS Proxy for connection pooling |
| Accidentally deleted rows in Aurora — need them back immediately | Aurora Backtrack |
| Analytics team queries are slowing down production RDS | Create a read replica for analytics, or migrate analytics to Redshift |
| Global application needs low-latency reads in multiple regions | Aurora Global Database |
