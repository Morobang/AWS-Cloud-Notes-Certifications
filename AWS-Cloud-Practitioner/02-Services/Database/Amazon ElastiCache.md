# Amazon ElastiCache

## What you'll learn
- What ElastiCache is and why caching matters
- Redis vs Memcached: the two engines
- Common caching patterns
- Key exam facts

---

## What is Amazon ElastiCache?

Amazon ElastiCache is a **fully managed in-memory data store and cache** service. It provides sub-millisecond latency by storing frequently accessed data in memory (RAM) rather than reading it from a database on disk.

ElastiCache supports two open-source engines: **Redis** and **Memcached**.

---

## Why caching matters

Reading data from a relational database (RDS, Aurora) involves disk I/O and query processing — typically 1–10 milliseconds. Reading from an in-memory cache takes microseconds to low milliseconds.

For high-traffic applications where the same data is read repeatedly (product catalog, user profiles, session data), caching eliminates redundant database queries and dramatically reduces latency and database load.

---

## Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data structures | Strings, hashes, lists, sets, sorted sets, bitmaps | Strings only |
| Persistence | Optional (snapshot or AOF) | None (in-memory only) |
| Replication | Yes (Multi-AZ, Read Replicas) | No |
| Clustering | Yes (Redis Cluster) | Yes (horizontal sharding) |
| Pub/Sub | Yes | No |
| Leaderboards | Yes (sorted sets) | No |
| Use case | Complex data, persistence, HA needed | Simple caching, maximum simplicity |

**Choose Redis when**: You need data persistence, replication, complex data structures, or pub/sub messaging.

**Choose Memcached when**: You want the simplest possible caching with no additional features.

---

## Common caching patterns

**Cache-aside (lazy loading)**: Application checks the cache first. If data is present (cache hit), return it. If not (cache miss), query the database, store the result in cache, then return it.

**Write-through**: Every database write also updates the cache, keeping cache and database in sync at the cost of slightly higher write latency.

**Session store**: Store user session data in ElastiCache Redis for fast access and automatic expiration with TTL.

---

## When to use it

**Database read offloading** — Cache the results of expensive database queries so the same query doesn't hit the database repeatedly.

**Session management** — Store authenticated user sessions in Redis with automatic TTL expiration.

**Real-time leaderboards** — Use Redis sorted sets to maintain a live ranking of players, scores, or metrics.

**Pub/sub messaging** — Use Redis pub/sub for lightweight message broadcasting between application components.

---

## ElastiCache vs DAX

| | ElastiCache | DynamoDB Accelerator (DAX) |
|-|------------|---------------------------|
| Use with | RDS, Aurora, any application | DynamoDB only |
| Engines | Redis or Memcached | Proprietary (DynamoDB-native) |
| Best for | General-purpose caching | Accelerating DynamoDB specifically |

---

## Exam focus

- ElastiCache = **managed in-memory cache** — Redis or Memcached
- **Sub-millisecond** read latency
- **Redis**: more features (persistence, replication, complex data types)
- **Memcached**: simpler, no persistence, for basic caching
- Common exam scenario: application reads same data from database repeatedly → add ElastiCache to cache responses
- Key differentiator from DAX: ElastiCache is for any database; DAX is DynamoDB-specific

---

## Practice questions

**Q1.** A high-traffic e-commerce website reads the same product catalog data from RDS thousands of times per second. Database CPU is consistently high from repeated identical queries. Which solution reduces database load with the lowest latency?

A) Add more RDS Read Replicas  
B) Migrate to DynamoDB  
C) Add Amazon ElastiCache in front of RDS  
D) Increase the RDS instance size

**Answer: C** — ElastiCache caches the results of repeated database queries in memory. After the first read, the same query returns from cache in microseconds without touching the database. Read Replicas help scale reads but still hit the database. DynamoDB is a different data model entirely. Increasing the instance size is vertical scaling with diminishing returns.
