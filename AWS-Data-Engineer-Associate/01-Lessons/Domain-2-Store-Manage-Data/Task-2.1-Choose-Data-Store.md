# Task 2.1: Choose the Appropriate Data Store

## Learning Objectives
- Apply a decision framework to select the right AWS data store for any workload
- Distinguish OLTP from OLAP requirements
- Know when to use NoSQL vs relational vs object storage
- Understand caching, graph, search, and time-series data patterns

---

## The Core Decision Framework

The exam will describe a workload and ask which data store fits. Work through these questions in order:

**1. What is the access pattern?**
- Random row lookups, writes, updates → OLTP (RDS, DynamoDB)
- Complex aggregations, full table scans, JOINs → OLAP (Redshift)
- Unstructured or semi-structured, query ad-hoc → S3 + Athena
- Full-text search, log analytics → OpenSearch

**2. What is the data structure?**
- Rows and columns, relationships → relational (RDS/Aurora)
- Key-value, documents, wide columns → NoSQL (DynamoDB, DocumentDB, Keyspaces)
- Any format at scale → object storage (S3)

**3. What are the performance requirements?**
- Sub-millisecond latency → caching layer (ElastiCache/MemoryDB) or DynamoDB
- Seconds acceptable → RDS, Redshift, Athena
- Minutes acceptable → EMR, batch Glue jobs

**4. Is the data relational (many tables, JOINs) or document/graph?**
- Relational → RDS/Aurora
- Document (JSON hierarchies) → DocumentDB
- Graph (relationships between entities) → Neptune

---

## Relational Databases — RDS and Aurora

### Amazon RDS

Managed relational database service. Supports MySQL, PostgreSQL, MariaDB, Oracle, SQL Server.

**Use when:**
- Application requires ACID transactions
- Schema is fixed and well-defined
- Queries use JOINs across normalized tables
- Standard OLTP workload (user accounts, orders, inventory)

**Not suited for:** analytical queries spanning billions of rows, or schema-less data.

### Amazon Aurora

AWS's cloud-native relational database — MySQL and PostgreSQL compatible, but faster and more resilient.

| Feature | Aurora | RDS MySQL/PostgreSQL |
|---|---|---|
| Storage | Auto-grows up to 128 TB, replicates to 6 copies | Fixed size, manual scaling |
| Replicas | Up to 15 Aurora Replicas | Up to 5 read replicas |
| Failover | Automatic, ~30 seconds | Manual or automated with Multi-AZ |
| Cost | Higher | Lower |
| Performance | Up to 5x MySQL, 3x PostgreSQL | Standard |

**Aurora Serverless v2**: Auto-scales compute capacity instantly — ideal for unpredictable or spiky workloads.

**Aurora Global Database**: Spans multiple AWS Regions — low-latency reads for global applications, disaster recovery.

**When to choose Aurora over RDS**: Production applications needing high availability, read replicas, or very large databases.

---

## Amazon DynamoDB — NoSQL Key-Value and Document Store

### When to Use DynamoDB

- Access pattern is known and consistent (get by partition key, query by sort key)
- Sub-10ms latency at any scale required
- Massive throughput: millions of reads/writes per second
- Serverless, no capacity planning
- Session data, user profiles, IoT device state, real-time leaderboards

### When NOT to Use DynamoDB

- You need complex JOINs across multiple tables
- Ad-hoc queries with unpredictable filter patterns
- Analytical workloads with aggregations

### DynamoDB Key Concepts for the Exam

**Primary key types:**
- Partition key only: `user_id` → each item accessed by a single key
- Partition key + sort key: `user_id` + `order_date` → query range of sort keys within a partition

**Read/Write capacity:**
- Provisioned: set RCU/WCU in advance — predictable traffic
- On-Demand: pay per request — unpredictable or spiky traffic

**Global Secondary Index (GSI)**: Query on any non-key attribute. DynamoDB maintains a separate copy of the table indexed by the GSI key.

**DynamoDB Streams**: Ordered change log (creates, updates, deletes). Feed into Lambda for event-driven processing.

**DynamoDB Accelerator (DAX)**: In-memory cache in front of DynamoDB. Reduces read latency from milliseconds to microseconds. Fully managed, API-compatible.

---

## Amazon Redshift — Data Warehouse

Use Redshift when you need complex analytical SQL queries over large datasets (gigabytes to petabytes).

| Signal | Service |
|---|---|
| "Analyze sales by region over the last 5 years" | Redshift |
| "Get user profile by user_id" | DynamoDB or RDS |
| "Query raw log files in S3" | Athena |

Key differentiator: Redshift uses columnar storage and MPP (massively parallel processing). It's optimized for `SELECT ... GROUP BY ... ORDER BY ...` over large data, not for `INSERT`/`UPDATE` per row.

---

## Amazon S3 — Data Lake

S3 is not a database, but it's the foundation of every data lake.

**Use S3 when:**
- Data has no fixed schema (CSV, JSON, Parquet, images, logs)
- You want the cheapest storage at any scale
- Data will be processed by Glue, EMR, or queried by Athena
- Long-term retention with lifecycle tiering to Glacier

**Schema-on-read**: No schema is enforced when writing. Schema is applied at query time by Athena or Glue.

---

## Amazon OpenSearch Service — Search and Log Analytics

OpenSearch (formerly Elasticsearch) is purpose-built for:
- Full-text search: "find all documents containing 'payment failure'"
- Log analytics: ingest application logs, run aggregations, build dashboards (OpenSearch Dashboards)
- Real-time operational analytics on JSON documents

**Common pipeline**: Kinesis Firehose → OpenSearch (for real-time log ingestion)

**Not suited for**: structured relational queries, long-term data lake storage.

---

## Specialized Data Stores

### Amazon Neptune — Graph Database

Use for data with complex many-to-many relationships:
- Social networks (friends-of-friends)
- Fraud detection (transaction networks)
- Knowledge graphs
- Recommendation engines ("users who bought X also bought Y")

Supports two graph models: **Property Graph** (Gremlin/openCypher) and **RDF** (SPARQL).

### Amazon Keyspaces — Managed Apache Cassandra

Wide-column NoSQL database. Use when:
- Migrating an existing Cassandra application to AWS
- Time-series data with high write throughput (IoT sensors, metrics)
- Cassandra Query Language (CQL) compatibility required

### Amazon MemoryDB for Redis

Redis-compatible, durable in-memory database:
- Not just a cache — data is persisted to a multi-AZ transaction log
- Ultra-low latency (microseconds), durable reads
- Use for: session management, real-time leaderboards, or primary database for latency-sensitive apps

vs. ElastiCache for Redis: ElastiCache is a cache (data loss on failure acceptable); MemoryDB is a primary database (zero data loss).

### Amazon DocumentDB — Managed MongoDB

MongoDB-compatible document database:
- Migrate existing MongoDB applications without code changes
- Store and query JSON documents with flexible schemas
- Not the same code as MongoDB — DocumentDB emulates MongoDB APIs

---

## Data Store Decision Table

| Workload | Best service |
|---|---|
| OLTP transactional app, relational schema | Amazon RDS or Aurora |
| OLTP at massive scale, known access patterns, NoSQL | Amazon DynamoDB |
| OLAP complex analytics over large datasets | Amazon Redshift |
| Ad-hoc SQL on files in S3 | Amazon Athena |
| Full-text search, log analytics | Amazon OpenSearch |
| Graph traversals, relationship queries | Amazon Neptune |
| Session store, sub-millisecond latency, durable | Amazon MemoryDB |
| Cassandra migration, high-write time-series | Amazon Keyspaces |
| MongoDB documents, flexible JSON | Amazon DocumentDB |
| Raw data lake, any format, any scale | Amazon S3 |

---

## Exam Scenarios

**"A company needs to store user clickstream data. Queries will always look up a user's events by user_id with a date range. Needs to handle millions of events per second."**
→ DynamoDB with partition key = `user_id`, sort key = `event_timestamp`

**"Analysts need to run ad-hoc SQL queries joining 10 tables with billions of rows each."**
→ Amazon Redshift

**"A fraud detection system needs to traverse networks of related transactions to find suspicious patterns."**
→ Amazon Neptune

**"An application uploads raw JSON logs to S3 hourly. Data scientists want to run SQL queries against the last 90 days."**
→ Amazon Athena (query S3 directly), with Glue Data Catalog for the schema

**"A social media app needs user profile lookups at sub-5ms latency, 500,000 requests per second."**
→ DynamoDB with DAX (in-memory cache layer)
