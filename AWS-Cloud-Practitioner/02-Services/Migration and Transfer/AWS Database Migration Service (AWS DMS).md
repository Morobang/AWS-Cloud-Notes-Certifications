# AWS Database Migration Service (AWS DMS)

## What you'll learn
- What DMS does and the types of migrations it supports
- Homogeneous vs heterogeneous migration
- DMS with SCT
- Key exam facts

---

## What is AWS Database Migration Service?

AWS Database Migration Service (DMS) is a **managed service that migrates databases to AWS with minimal downtime**. The source database remains fully operational during migration — DMS replicates data continuously until you're ready to cut over.

---

## How DMS works

1. Create a replication instance (a managed EC2-like instance that runs the migration)
2. Define source and target endpoints (e.g., source: on-premises Oracle; target: Amazon Aurora PostgreSQL)
3. Configure a migration task specifying which tables/schemas to migrate
4. DMS reads from the source, converts data format if needed, and writes to the target
5. After initial load, DMS uses Change Data Capture (CDC) to replicate ongoing changes from source to target
6. When ready, perform the cutover — point your application at the new database

Because CDC keeps the target in sync with the source in near real-time, the cutover window is minimal.

---

## Two types of migration

**Homogeneous migration** — Same database engine to same engine: MySQL to MySQL, Oracle to Oracle, SQL Server to SQL Server. No schema conversion required.

**Heterogeneous migration** — Different database engine: Oracle to PostgreSQL, SQL Server to Aurora, MySQL to DynamoDB. Requires schema conversion (handled by **AWS Schema Conversion Tool** before DMS).

---

## Source and target databases

**Sources**: Oracle, SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP ASE, IBM Db2, and more. Including on-premises, EC2, and RDS instances.

**Targets**: RDS (MySQL, PostgreSQL, Oracle, SQL Server, MariaDB), Aurora, Redshift, DynamoDB, S3, OpenSearch, Kinesis Data Streams.

---

## Continuous data replication

DMS can also be used for **ongoing replication** (not just one-time migration):
- Replicate from on-premises to AWS for a hybrid setup
- Replicate for analytics (e.g., from RDS to Redshift)
- Replicate from one Region to another

---

## When to use it

**Database migration to RDS or Aurora** — Migrate an on-premises Oracle or MySQL database to Amazon Aurora with minimal downtime.

**Cross-engine migration** — Move from a commercial database (Oracle, SQL Server) to an open-source database (PostgreSQL, MySQL) to reduce licensing costs.

**Continuous replication** — Keep an AWS database in sync with an on-premises database during a phased migration.

---

## Exam focus

- DMS = **migrate databases to AWS with minimal downtime**
- Uses **Change Data Capture (CDC)** to keep target in sync with source during migration
- **Homogeneous**: same engine (MySQL → MySQL); **Heterogeneous**: different engine (Oracle → Aurora) — requires SCT first
- Use when exam describes: "migrate database to RDS," "minimize downtime during database migration," "replicate Oracle to Aurora"

---

## Practice questions

**Q1.** A company wants to migrate their on-premises Oracle database to Amazon Aurora PostgreSQL. The database must remain available to the production application throughout the migration with minimal downtime at cutover. Which AWS service handles this migration?

A) AWS Schema Conversion Tool  
B) AWS Application Migration Service  
C) AWS Database Migration Service  
D) AWS Snowball

**Answer: C** — DMS migrates databases with minimal downtime using continuous replication (CDC). Note: Since this is Oracle to PostgreSQL (heterogeneous), SCT would be used first to convert the schema — but DMS performs the actual data migration. Application Migration Service migrates servers, not databases. Snowball is for large offline data transfers, not database migrations.
