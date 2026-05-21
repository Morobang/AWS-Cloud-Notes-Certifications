# Amazon RDS

## What you'll learn
- What RDS is and which database engines it supports
- What RDS manages for you
- RDS Multi-AZ and Read Replicas
- When to use RDS vs Aurora vs DynamoDB
- Key exam facts

---

## What is Amazon RDS?

Amazon Relational Database Service (RDS) is a **managed relational database service**. AWS handles the database infrastructure — provisioning, patching, backups, and failover — so you can focus on your schema and queries.

RDS runs relational databases where data is organized into tables with rows and columns, and queries are written in SQL.

---

## Supported database engines

| Engine | Description |
|--------|-------------|
| Amazon Aurora | AWS-built, MySQL and PostgreSQL compatible |
| MySQL | Most popular open-source relational DB |
| PostgreSQL | Advanced open-source relational DB |
| MariaDB | MySQL fork with additional features |
| Oracle | Enterprise relational DB (BYOL or license-included) |
| Microsoft SQL Server | Microsoft's relational DB (BYOL or license-included) |

---

## What RDS manages for you

| Task | AWS handles |
|------|------------|
| Hardware provisioning | AWS provisions and replaces hardware |
| OS patching | AWS patches the underlying OS |
| Database software updates | AWS applies database version patches |
| Automated backups | Daily snapshots to S3, configurable retention |
| Database monitoring | CloudWatch metrics |
| Multi-AZ failover | Automatic failover to standby replica |

You are still responsible for: database schema, performance tuning, query optimization, and user/permission management.

---

## Multi-AZ deployments

When you enable **Multi-AZ**, RDS creates a **synchronous standby replica** in a different Availability Zone. If the primary database fails:
- RDS automatically fails over to the standby
- The DNS endpoint stays the same — your application reconnects automatically
- Typical failover time: 1–2 minutes

Multi-AZ is for **high availability and disaster recovery** — the standby is not accessible for reads, only for failover.

---

## Read Replicas

A **Read Replica** is an **asynchronous copy** of the database. Applications can read from replicas, offloading read traffic from the primary database.

- Up to 5 Read Replicas per RDS instance (15 for Aurora)
- Can be in the same region or a different region (cross-region read replicas)
- Replicas can be promoted to become a standalone database

Read Replicas improve **performance** (read scaling). Multi-AZ improves **availability** (failover).

---

## Multi-AZ vs Read Replicas

| | Multi-AZ | Read Replica |
|-|----------|-------------|
| Purpose | High availability (failover) | Read performance (scaling) |
| Replication | Synchronous | Asynchronous |
| Accessible for reads | No | Yes |
| Automatic failover | Yes | No (manual promotion) |
| Data lag | None (synchronous) | Small (asynchronous) |

---

## Exam focus

- RDS = **managed relational database** — AWS handles infrastructure, you handle schema and queries
- Supports 6 engines: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora
- **Multi-AZ** = high availability (automatic failover, standby not readable)
- **Read Replicas** = read scaling (asynchronous copies that serve reads)
- RDS is **not serverless** by default (use Aurora Serverless for serverless relational DB)
- Key differentiator from DynamoDB: RDS is relational/SQL; DynamoDB is NoSQL key-value

---

## Practice questions

**Q1.** A company runs a MySQL database on-premises and wants to migrate to AWS. They need automatic failover to a secondary database in a different Availability Zone if the primary fails. Which RDS feature provides this?

A) Read Replicas  
B) RDS Multi-AZ  
C) RDS Proxy  
D) Amazon ElastiCache

**Answer: B** — RDS Multi-AZ creates a synchronous standby replica in a different AZ and fails over automatically if the primary fails, with the same DNS endpoint. Read Replicas are for read scaling, not failover — they don't provide automatic promotion. RDS Proxy is a connection pooler. ElastiCache is an in-memory cache.

---

**Q2.** A read-heavy web application uses Amazon RDS MySQL. Database CPU and I/O are consistently high due to read queries. What is the most appropriate solution to reduce load on the primary database?

A) Enable Multi-AZ  
B) Increase the instance type  
C) Create Read Replicas  
D) Enable automated backups

**Answer: C** — Read Replicas create asynchronous copies of the database that can serve read traffic, offloading the primary instance. Multi-AZ creates a standby that doesn't serve reads. Increasing the instance type is vertical scaling (more expensive, has limits). Automated backups don't affect read performance.
