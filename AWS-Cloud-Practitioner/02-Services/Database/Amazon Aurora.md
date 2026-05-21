# Amazon Aurora

## What you'll learn
- What Aurora is and how it relates to RDS
- Aurora's storage architecture and why it's faster
- Aurora Serverless
- When to use Aurora vs standard RDS
- Key exam facts

---

## What is Amazon Aurora?

Amazon Aurora is a **cloud-native relational database** built by AWS that is **compatible with MySQL and PostgreSQL**. It is part of the RDS family but uses a different storage architecture that delivers significantly higher performance and availability.

Aurora is designed to provide the performance of commercial databases (Oracle, SQL Server) at the cost of open-source databases.

---

## Key performance characteristics

- **5x faster than MySQL** on RDS (based on AWS benchmarks)
- **3x faster than PostgreSQL** on RDS
- Distributed, fault-tolerant storage that automatically scales up to 128 TB
- Up to **15 low-latency Read Replicas** (vs 5 for standard RDS)
- Sub-10ms failover time (faster than standard RDS Multi-AZ)

---

## Aurora storage architecture

Aurora uses a **shared distributed storage volume** instead of each instance having its own storage:

- Data is automatically replicated **6 times across 3 Availability Zones** (2 copies per AZ)
- The cluster can survive losing 2 copies without losing write availability
- The cluster can survive losing 3 copies without losing read availability
- Storage automatically grows in 10 GB increments up to 128 TB

This architecture makes Aurora significantly more resilient than standard RDS.

---

## Aurora Serverless

Aurora Serverless automatically scales compute capacity up and down based on your application's needs. You define minimum and maximum Aurora Capacity Units (ACUs); Aurora starts, stops, and scales compute automatically.

- Automatically pauses when idle (you're not charged for compute when idle)
- Scales up within seconds when traffic arrives
- **Use when**: Infrequent or unpredictable workloads (development databases, reporting workloads that run periodically)

---

## Aurora vs Standard RDS

| | Amazon Aurora | Standard RDS |
|-|---------------|-------------|
| Compatibility | MySQL, PostgreSQL | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server |
| Performance | 5x MySQL, 3x PostgreSQL | Standard |
| Read Replicas | Up to 15 | Up to 5 |
| Storage replication | 6 copies across 3 AZs | 2 copies (Multi-AZ) |
| Failover time | < 30 seconds | 1–2 minutes |
| Serverless option | Yes (Aurora Serverless) | Limited |
| Cost | Higher than RDS MySQL/PostgreSQL | Lower |

---

## When to use Aurora

**High-traffic applications** — E-commerce platforms, SaaS applications, or any MySQL/PostgreSQL workload that needs more performance than standard RDS provides.

**High-availability requirements** — The 6-copy storage replication makes Aurora inherently more resilient than standard RDS.

**MySQL/PostgreSQL migration with less re-work** — Aurora is API-compatible with MySQL 8.0 and PostgreSQL 15, so most applications work without code changes.

**Variable/unpredictable workloads** — Aurora Serverless for dev/test environments or reporting databases that don't need to run 24/7.

---

## Exam focus

- Aurora = **MySQL and PostgreSQL compatible**, built by AWS for higher performance
- **5x faster** than MySQL on RDS, **3x faster** than PostgreSQL on RDS
- Storage automatically replicates **6 copies across 3 AZs**
- Up to **15 Read Replicas** (vs 5 for standard RDS)
- **Aurora Serverless** = auto-scales compute, pauses when idle
- More expensive than standard RDS MySQL/PostgreSQL but more performant
- Use when the exam describes needing "high-performance MySQL or PostgreSQL" or "higher availability"

---

## Practice questions

**Q1.** A company is running a high-traffic e-commerce application using RDS MySQL. They need higher read throughput, faster failover, and better storage resilience. Which AWS service provides MySQL compatibility with these improved capabilities?

A) Amazon DynamoDB  
B) Amazon Aurora  
C) Amazon ElastiCache  
D) Amazon Redshift

**Answer: B** — Aurora is MySQL-compatible but delivers 5x the performance of RDS MySQL. It supports up to 15 Read Replicas (vs 5 for RDS), replicates storage 6 times across 3 AZs, and fails over in under 30 seconds. DynamoDB is NoSQL. ElastiCache is an in-memory cache. Redshift is a data warehouse.
