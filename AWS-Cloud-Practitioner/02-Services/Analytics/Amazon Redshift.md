# Amazon Redshift

## What you'll learn
- What Redshift is and how it differs from RDS
- The OLAP vs OLTP distinction
- When to use Redshift vs Athena
- Key Redshift concepts for the exam

---

## What is Amazon Redshift?

Amazon Redshift is a **fully managed, petabyte-scale data warehouse** built for analytical workloads. Unlike RDS (which is optimized for transactional operations), Redshift is optimized for complex analytical queries that aggregate and scan large amounts of historical data.

Redshift stores data in **columnar format** — instead of storing all columns for a row together, it stores each column separately. This means a query like `SELECT SUM(revenue) FROM orders` only reads the `revenue` column, skipping all other data. This makes analytical queries dramatically faster.

---

## OLAP vs OLTP

| | OLTP (Online Transaction Processing) | OLAP (Online Analytical Processing) |
|-|--------------------------------------|--------------------------------------|
| Example services | RDS, Aurora | Redshift |
| Operations | INSERT, UPDATE, DELETE | SELECT with complex aggregations |
| Query pattern | Simple queries on individual rows | Complex queries scanning millions of rows |
| Optimization | Fast writes, row storage | Fast reads, columnar storage |
| Use case | E-commerce orders, banking transactions | Business intelligence, historical reports |

Redshift is an **OLAP** database. RDS is an **OLTP** database.

---

## Key features

- **Columnar storage**: Reads only the columns needed by a query
- **Massively parallel processing (MPP)**: Queries are distributed across all cluster nodes
- **Petabyte scale**: Handles datasets too large for RDS
- **Redshift Spectrum**: Query data in S3 directly without loading it into Redshift (similar to Athena)
- **Automated backups**: Continuous snapshots to S3
- **Integration**: Works with QuickSight (BI), Glue (ETL), and S3 (data lake)

---

## When to use it

**Data warehouse** — Load nightly transaction data from RDS into Redshift for business intelligence reporting.

**Large-scale analytics** — Running queries that join billions of rows across multiple tables.

**Historical data analysis** — Analyzing 5 years of sales data to identify seasonal trends.

**Centralized reporting** — Multiple business teams (finance, marketing, operations) all query the same Redshift warehouse.

---

## Redshift vs Athena

| | Amazon Redshift | Amazon Athena |
|-|----------------|---------------|
| Data location | Loaded into Redshift cluster | Stays in S3 |
| Setup | Cluster provisioning | None (serverless) |
| Query speed | Very fast (indexed, compressed) | Depends on data scanned |
| Cost model | Per cluster-hour | Per TB scanned |
| Best for | Ongoing complex analytics | Occasional ad-hoc queries |

Use Redshift when you need **consistently fast** analytical queries. Use Athena when you need **occasional** queries on data already in S3.

---

## Exam focus

- Redshift = **data warehouse** for OLAP — not for transactional workloads
- Uses **columnar storage** for fast analytical queries
- **Petabyte-scale** — the largest analytical datasets in AWS go into Redshift
- **Redshift Spectrum** extends queries to S3 data without loading it
- Key differentiators: Redshift vs RDS (OLAP vs OLTP), Redshift vs Athena (ongoing vs ad-hoc queries)
- Integrates with **QuickSight** for dashboards and **Glue** for data pipelines

---

## Practice questions

**Q1.** A retail company runs complex analytical queries joining 5 years of transaction history across multiple tables. The 50 TB dataset performs too slowly on their RDS database. Which service is designed for this workload?

A) Amazon DynamoDB  
B) Amazon Redshift  
C) Amazon Athena  
D) Amazon ElastiCache

**Answer: B** — Redshift is a petabyte-scale data warehouse for complex multi-table analytical queries on large historical datasets. DynamoDB is a NoSQL database for transactional workloads. Athena is better for ad-hoc queries, not sustained complex analytics. ElastiCache is an in-memory cache.

---

**Q2.** Which characteristic best describes Amazon Redshift compared to Amazon RDS?

A) Redshift is designed for high-volume write operations  
B) Redshift uses row-based storage to optimize individual record lookups  
C) Redshift uses columnar storage optimized for analytical queries across large datasets  
D) Redshift is a NoSQL database

**Answer: C** — Redshift stores data in columnar format, allowing it to read only the columns needed by a query. This makes it highly efficient for analytical queries that aggregate data across millions of rows. RDS is row-based and optimized for transactional workloads.
