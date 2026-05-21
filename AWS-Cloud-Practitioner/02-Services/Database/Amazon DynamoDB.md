# Amazon DynamoDB

## What you'll learn
- What DynamoDB is and how it differs from relational databases
- Key DynamoDB concepts (tables, items, attributes, keys)
- DynamoDB Global Tables
- When to use DynamoDB
- Key exam facts

---

## What is Amazon DynamoDB?

Amazon DynamoDB is a **fully managed, serverless NoSQL key-value and document database**. It delivers single-digit millisecond performance at any scale — from thousands to billions of requests per day.

Unlike relational databases that require a predefined schema, DynamoDB is **schema-flexible**: each item (row) can have different attributes. You only define the primary key; all other attributes can vary per item.

---

## How DynamoDB works

**Table** — The top-level container for data (analogous to a table in SQL).

**Item** — A single record in the table (analogous to a row). Each item is a collection of attributes.

**Attribute** — A named data value within an item (analogous to a column, but flexible — not all items need the same attributes).

**Primary key** — Required for every item. Two options:
- **Partition key only**: A single attribute that uniquely identifies each item
- **Partition key + Sort key (composite key)**: Allows multiple items with the same partition key, distinguished by sort key

---

## Key features

- **Serverless**: No instances to provision or manage — AWS handles all infrastructure
- **Automatic scaling**: Capacity scales automatically as traffic increases or decreases
- **Single-digit millisecond latency**: Consistent performance at any scale
- **Multi-AZ by default**: Data is automatically replicated across 3 Availability Zones
- **DynamoDB Streams**: Capture a time-ordered sequence of changes to items — used to trigger Lambda functions on data changes
- **DynamoDB Accelerator (DAX)**: In-memory cache for DynamoDB — reduces read latency from milliseconds to **microseconds**
- **Point-in-time recovery (PITR)**: Restore the table to any point in the last 35 days
- **Time to Live (TTL)**: Automatically delete items after a specified time

---

## DynamoDB Global Tables

**Global Tables** provide a fully managed **multi-Region, multi-active database**. DynamoDB replicates data across multiple AWS Regions automatically. Any Region can accept writes, and changes are propagated to all other Regions.

*Use when*: You need low-latency access for globally distributed users, or high availability that survives an entire Region failing.

---

## DynamoDB vs RDS

| | Amazon DynamoDB | Amazon RDS |
|-|----------------|-----------|
| Data model | Key-value / Document (NoSQL) | Relational (SQL) |
| Schema | Flexible (no predefined schema) | Fixed schema required |
| Scaling | Serverless, automatic | Vertical + Read Replicas |
| Queries | Key-based or index-based | Full SQL (JOINs, aggregations) |
| Transactions | DynamoDB Transactions (limited) | Full ACID transactions |
| Best for | High-scale, flexible data models | Complex queries, relational data |

---

## When to use it

**High-traffic web applications** — Shopping carts, user session data, leaderboards — workloads that need consistent millisecond latency under any load.

**IoT data** — Sensor readings that arrive at high volume with a simple key-based access pattern.

**Gaming leaderboards** — Store and retrieve scores at massive scale with consistent performance.

**Mobile backends** — Store user preferences, state, and profiles that don't fit a rigid schema.

---

## Exam focus

- DynamoDB = **serverless NoSQL** — no instances, no schema (except primary key)
- **Single-digit millisecond** latency at any scale
- **Global Tables** = multi-Region, multi-active replication
- **DAX** = DynamoDB Accelerator — in-memory cache for microsecond reads
- **DynamoDB Streams** = event stream of item changes — triggers Lambda
- **TTL** = auto-delete items after a defined time
- Key differentiator: DynamoDB is NoSQL/key-value; RDS is relational/SQL

---

## Practice questions

**Q1.** A mobile gaming company needs to store player session data for millions of active users globally. The data model is simple (user ID → session attributes) and they require consistent single-digit millisecond response times at scale. Which database service is most appropriate?

A) Amazon RDS MySQL  
B) Amazon Aurora  
C) Amazon DynamoDB  
D) Amazon Redshift

**Answer: C** — DynamoDB is designed for exactly this use case: simple key-value access patterns, serverless scaling to any volume, and consistent millisecond latency. RDS and Aurora are relational databases that don't scale as elastically. Redshift is a data warehouse for analytical queries, not transactional session data.

---

**Q2.** A company uses DynamoDB for their session store. They notice read latencies are higher than desired. Which service can reduce DynamoDB read latency to microseconds?

A) Amazon ElastiCache  
B) DynamoDB Streams  
C) DynamoDB Accelerator (DAX)  
D) DynamoDB Global Tables

**Answer: C** — DynamoDB Accelerator (DAX) is a fully managed in-memory cache specifically for DynamoDB that delivers microsecond response times. ElastiCache is a general-purpose in-memory cache (Redis/Memcached) that could also work but is not DynamoDB-native. Streams capture change events. Global Tables add multi-Region replication, not caching.
