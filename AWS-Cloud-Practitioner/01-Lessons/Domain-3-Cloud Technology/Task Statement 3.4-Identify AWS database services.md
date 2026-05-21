# Task Statement 3.4: Identify AWS Database Services

## What you'll learn
- Relational vs. NoSQL databases and when to use each
- The key AWS database services and what each is optimized for
- How managed database services reduce operational overhead

---

## Relational vs. NoSQL

Before choosing a database service, understand the two fundamental types.

**Relational databases** store data in tables with rows and columns. They use SQL for queries. Data has a fixed schema — every row in a table has the same columns. Relationships between tables are enforced. Best when data is structured and the relationships between data matter: customer orders, financial transactions, user accounts.

**NoSQL databases** store data in flexible formats — key-value pairs, documents (JSON), wide columns, or graphs. No fixed schema — different items can have different fields. Scales horizontally to massive sizes. Best when data is unstructured, schemas vary, read/write speed at scale matters more than rigid relationships: user sessions, product catalogs, gaming leaderboards, IoT sensor data.

The wrong choice affects performance at scale. Forcing NoSQL-type data into a relational database (or vice versa) creates complexity and performance problems.

---

## Amazon RDS — Relational Database Service

RDS is AWS's managed relational database service. It runs standard database engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Amazon Aurora.

**What AWS manages**: OS patching, database engine patching, automated daily backups, Multi-AZ replication, storage scaling, failover.

**What you manage**: Database schema, queries, user accounts, encryption settings, which data you store.

**Multi-AZ deployment**: RDS automatically replicates your database to a standby instance in another Availability Zone. If the primary instance fails, RDS automatically promotes the standby — typically within 1–2 minutes. No manual intervention required.

**Read Replicas**: For read-heavy workloads, you can create read replicas — additional copies of the database that can serve read queries. This offloads traffic from the primary database.

*Use when*: Your application needs a traditional SQL database with managed operations. Most web applications, e-commerce platforms, CRMs.

---

## Amazon Aurora

Aurora is AWS's own cloud-native relational database engine. It's compatible with MySQL and PostgreSQL — so applications built for those databases work without code changes — but it's architected differently for better performance and availability.

**Key advantages over standard RDS MySQL/PostgreSQL:**
- Up to 5x faster than MySQL, 3x faster than PostgreSQL (AWS benchmarks)
- Storage automatically grows in 10 GB increments, up to 128 TB
- Replicates data across 3 Availability Zones automatically (6 copies of your data)
- Faster failover than standard RDS Multi-AZ — typically under 30 seconds

**Aurora Serverless**: A version of Aurora that automatically scales database capacity up and down based on application demand. Scales to zero when not in use — pay only when your database is actually processing queries. Good for variable or infrequent workloads.

*Use when*: You need a relational database with higher performance and availability than standard RDS, or when the workload justifies the higher cost of Aurora.

---

## Amazon DynamoDB

DynamoDB is AWS's fully managed NoSQL database. It stores data as key-value and document items. It's serverless — no instances to provision or scale. You specify the read/write capacity you need; DynamoDB handles everything else.

**Performance**: DynamoDB delivers single-digit millisecond response times at any scale. It can handle millions of requests per second.

**Scale**: DynamoDB tables can grow to any size automatically. There's no storage limit.

**Key features**:
- **DynamoDB Streams**: Capture a time-ordered log of all changes to items in a table. Useful for triggering Lambda functions on database changes.
- **Global Tables**: Replicate your DynamoDB table across multiple AWS Regions automatically. Applications in any Region read and write to their local copy; DynamoDB handles multi-Region replication.

*Use when*: You need high throughput at scale, flexible data schemas, global distribution, or a serverless database. Gaming leaderboards, user profiles, session data, shopping cart, IoT sensor readings.

*Avoid when*: You need complex SQL queries, joins across tables, or strict relational integrity.

---

## Amazon ElastiCache

ElastiCache is a managed in-memory caching service. It runs Redis or Memcached — two popular open-source in-memory data stores.

**Why caching matters**: Databases are fast, but in-memory is faster by orders of magnitude. If your application reads the same data repeatedly (popular product listings, session tokens, frequently-accessed user data), caching that data in memory means fewer database queries and much lower response times.

**Redis vs Memcached**:
- **Redis** supports more data structures (lists, sets, sorted sets), persistence, replication, and pub/sub. Most common choice.
- **Memcached** is simpler — just a key-value cache. Better for pure caching scenarios without the extra features.

*Use when*: You have a high-read workload hitting the same data repeatedly. Reducing database load, improving API response times.

---

## Amazon Redshift

Redshift is AWS's managed data warehouse. Unlike transactional databases (RDS, DynamoDB) that handle many small read/write operations, Redshift is optimized for analytical queries across massive datasets.

**OLTP vs OLAP**:
- **OLTP** (Online Transaction Processing) — many small, fast transactions. Order placed, record inserted. Use RDS or DynamoDB.
- **OLAP** (Online Analytical Processing) — complex queries aggregating large amounts of historical data. "What were total sales by region last quarter?" Use Redshift.

**What makes Redshift fast for analytics**: It stores data in columns rather than rows (columnar storage). When a query only needs a few columns from a billion-row table, Redshift reads only those columns — much more efficient than reading entire rows.

**Redshift Serverless**: No cluster to manage — Redshift automatically provisions capacity for your queries.

*Use when*: Business intelligence and analytics, running SQL queries against historical data, building data warehouses.

---

## Amazon DocumentDB

DocumentDB is a managed document database compatible with MongoDB. If you're using MongoDB and want to migrate to AWS without rewriting your application, DocumentDB is the path — it understands MongoDB's query language and drivers.

*Use when*: You're migrating a MongoDB workload to AWS, or you need a scalable document database with AWS management.

---

## Amazon Neptune

Neptune is a managed graph database. Graph databases are optimized for highly connected data — data where the relationships between items are as important as the items themselves.

*Use when*: Social networks (who follows whom, who knows whom), fraud detection (identify suspicious transaction patterns), recommendation engines (users who bought X also bought Y), knowledge graphs.

---

## Choosing the right database

| Scenario | Service |
|----------|---------|
| Traditional relational database, standard SQL | RDS |
| Relational database needing maximum performance | Aurora |
| Serverless, high-scale NoSQL | DynamoDB |
| Speed up repeated database reads with caching | ElastiCache |
| Business intelligence, analytical queries on large data | Redshift |
| MongoDB-compatible document database | DocumentDB |
| Highly connected data, social networks, fraud detection | Neptune |

---

## Practice questions

**Q1.** A retail company runs a product catalog that receives 10 million reads per day but updates only when stock changes (a few thousand times daily). Response time is critical — customers leave if the page loads slowly. What database combination would best address this?

A) DynamoDB only  
B) RDS with read replicas  
C) RDS as the primary database with ElastiCache caching frequent reads  
D) Redshift for analytical queries

**Answer: C** — ElastiCache caches the frequently-read product catalog data in memory, so most of the 10M daily reads never hit the database at all. When stock updates occur, the cache is refreshed. This dramatically reduces database load and response time. RDS read replicas help but don't reduce queries as dramatically as memory caching. Redshift is for analytics, not transactional applications.

---

**Q2.** A company wants to analyze five years of sales transactions to identify trends, seasonal patterns, and regional performance. The dataset is 10 TB. Which service is most appropriate?

A) Amazon RDS  
B) Amazon DynamoDB  
C) Amazon ElastiCache  
D) Amazon Redshift

**Answer: D** — Redshift is purpose-built for analytical queries over large historical datasets (OLAP). It's optimized for the kind of complex aggregation queries used in business intelligence. RDS and DynamoDB are for transactional workloads. ElastiCache is an in-memory cache, not a database for analytical queries.

---

**Q3.** An application tracks which users are friends with which other users, and needs to answer queries like "find all friends of friends within 3 degrees of separation." Which database type is best suited?

A) Amazon RDS (relational)  
B) Amazon Neptune (graph)  
C) Amazon DynamoDB (key-value/document)  
D) Amazon Redshift (data warehouse)

**Answer: B** — Neptune is a graph database optimized for traversing relationships between entities. Queries about degrees of connection, paths between nodes, and relationship patterns are what graph databases are built for. Relational databases can technically do this with complex JOIN chains, but performance degrades badly at scale. DynamoDB and Redshift aren't designed for graph traversal.

---

**Next:** [Task 3.5 — Network Services](./Task%20Statement%203.5-Identify%20AWS%20network%20services.md)
