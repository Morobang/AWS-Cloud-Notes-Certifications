# AWS Glue

## What you'll learn
- What Glue is and its three main components
- What ETL means and when you need it
- How Glue compares to EMR and Athena
- Key Glue facts for the exam

---

## What is AWS Glue?

AWS Glue is a serverless **ETL (Extract, Transform, Load)** service. ETL is the process of pulling data from source systems, reshaping or cleaning it, and loading it into a destination for analysis. Glue automates the infrastructure for this process — no cluster to provision or manage.

---

## The three main components

### Glue Crawlers

A **crawler** automatically scans a data source (S3, RDS, Redshift, DynamoDB), infers the schema and data types, and writes the metadata to the Glue Data Catalog. You point the crawler at your data and it figures out what's in it.

### Glue Data Catalog

The **Data Catalog** is a centralized metadata repository. It stores table definitions, schemas, and data location information. Other AWS services — including Athena, Redshift Spectrum, and EMR — use the Glue Data Catalog as their central metadata store.

Think of it as the "card catalog" for your data lake.

### Glue Jobs (ETL Jobs)

**Glue Jobs** are serverless Apache Spark or Python scripts that extract data from a source, transform it (join, filter, aggregate, convert formats), and load it into a destination. Glue can auto-generate transformation code based on the source and destination schemas.

---

## When to use it

**Building data pipelines** — Move data from operational databases (RDS, DynamoDB) into a data lake in S3 or into Redshift for analytics.

**Format conversion** — Convert CSV files to Parquet (columnar format) to reduce Athena query costs and improve performance.

**Schema discovery** — Automatically discover the schema of files in S3 so Athena can query them without manual table definitions.

**Data preparation** — Clean and normalize data from multiple sources before loading it into a data warehouse.

---

## Glue vs EMR vs Athena

| | AWS Glue | Amazon EMR | Amazon Athena |
|-|----------|------------|---------------|
| Purpose | Serverless ETL | Managed Spark/Hadoop clusters | SQL query on S3 |
| Infrastructure | Serverless | You provision cluster | Serverless |
| Use case | Move and transform data | Custom big data processing | Ad-hoc queries |
| Code | Auto-generated or custom PySpark | Your Spark/Hadoop code | SQL |

---

## Exam focus

- Glue = **serverless ETL** — no cluster management
- The **Glue Data Catalog** is the central metadata store used by Athena, EMR, and Redshift
- **Crawlers** automatically discover schemas from data in S3 or databases
- Glue is the right answer when the exam describes: "move data between stores," "transform data before loading into Redshift," "build a data pipeline"
- Key difference from Athena: Glue **moves and transforms** data; Athena **queries** data in place

---

## Practice questions

**Q1.** A company wants to automatically build a central catalog of all datasets stored across multiple S3 buckets, and then make that data queryable by data analysts using SQL. Which AWS service automatically discovers and catalogs the data?

A) Amazon Athena  
B) AWS Glue Crawlers  
C) Amazon Redshift  
D) Amazon Kinesis

**Answer: B** — Glue Crawlers scan S3 (and other sources), infer schemas, and populate the Glue Data Catalog. Once cataloged, the data can be queried by Athena using the catalog metadata. Athena queries the data but does not catalog it. Redshift and Kinesis serve different purposes.
