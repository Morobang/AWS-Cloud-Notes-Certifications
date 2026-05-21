# AWS Schema Conversion Tool (AWS SCT)

## What you'll learn
- What SCT does and when it's needed
- How SCT and DMS work together
- Key exam facts

---

## What is AWS Schema Conversion Tool?

AWS Schema Conversion Tool (SCT) is a **tool that automatically converts a database schema from one engine to another** — for example, converting an Oracle database schema to PostgreSQL. It handles the DDL (tables, views, stored procedures, functions, indexes) that is specific to the source database's SQL dialect.

SCT is used in **heterogeneous database migrations** — when the source and target database engines are different.

---

## Why schema conversion is needed

Different database engines have different SQL dialects, data types, and features:
- Oracle uses `VARCHAR2` and `NUMBER`; PostgreSQL uses `VARCHAR` and `NUMERIC`
- Oracle has `ROWNUM`; PostgreSQL uses `LIMIT`
- Stored procedures written in PL/SQL (Oracle) need to be rewritten in PL/pgSQL (PostgreSQL)

Manually converting an Oracle schema with hundreds of tables, stored procedures, and functions to PostgreSQL would take weeks. SCT automates most of this conversion.

---

## How SCT works

1. Connect SCT to the source database
2. SCT analyzes the schema and produces an **assessment report** — showing which objects can be automatically converted, which need manual review, and which require significant changes
3. SCT automatically converts as much as possible
4. A developer reviews and fixes the items that couldn't be automatically converted (usually complex stored procedures)
5. Apply the converted schema to the target database
6. Use DMS to migrate the actual data

---

## SCT + DMS workflow

For a heterogeneous migration:

1. **SCT**: Convert the schema (tables, views, procedures) from source to target engine
2. Apply converted schema to the target database
3. **DMS**: Migrate the data from source to target (with ongoing CDC replication)
4. Test and cut over

SCT handles the schema; DMS handles the data.

---

## Supported conversions

| Source | Target examples |
|--------|----------------|
| Oracle | PostgreSQL, Aurora PostgreSQL, MySQL, Aurora MySQL |
| SQL Server | PostgreSQL, Aurora PostgreSQL, MySQL |
| Teradata | Redshift |
| Netezza | Redshift |
| Microsoft Access | MySQL, Aurora MySQL |

---

## Exam focus

- SCT = **convert database schema** from one engine to another
- Used for **heterogeneous migrations** (different source and target engines)
- SCT converts schema; **DMS** migrates data
- Always used together for heterogeneous migrations: SCT first, then DMS
- Use when exam describes: "convert Oracle schema to PostgreSQL," "heterogeneous database migration," "schema conversion before migrating"

---

## Practice questions

**Q1.** A company is migrating their on-premises SQL Server database to Amazon Aurora MySQL. Their database has hundreds of stored procedures written in T-SQL. Which tool converts the database objects (tables, stored procedures, views) to MySQL syntax before the data is migrated?

A) AWS DMS  
B) AWS Schema Conversion Tool (SCT)  
C) AWS Application Migration Service  
D) AWS Glue

**Answer: B** — SCT converts database schema, including stored procedures, from one engine (SQL Server/T-SQL) to another (MySQL). After SCT converts the schema, DMS migrates the actual data. DMS handles data migration but does not convert schema between different engines. Application Migration Service migrates whole servers. Glue is an ETL service for data processing, not database schema conversion.
