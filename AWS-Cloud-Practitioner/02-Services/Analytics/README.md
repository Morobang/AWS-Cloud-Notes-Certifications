# Analytics Services

AWS analytics services let you collect, process, query, and visualize data at any scale — from ad-hoc SQL queries to real-time streaming pipelines to petabyte-scale data warehouses.

## Services in this category

| Service | What it does | Key use case |
|---------|-------------|--------------|
| [Amazon Athena](./Amazon%20Athena.md) | SQL queries directly against S3 | Ad-hoc analysis without a database |
| [Amazon EMR](./Amazon%20EMR.md) | Managed Hadoop/Spark clusters | Large-scale big data processing |
| [AWS Glue](./AWS%20Glue.md) | Serverless ETL and data catalog | Transform and move data between stores |
| [Amazon Kinesis](./Amazon%20Kinesis.md) | Real-time data streaming | Process data within seconds of arrival |
| [Amazon OpenSearch Service](./Amazon%20OpenSearch%20Service.md) | Managed search and log analytics | Full-text search, log dashboards |
| [Amazon QuickSight](./Amazon%20QuickSight.md) | Business intelligence dashboards | Visualize data for stakeholders |
| [Amazon Redshift](./Amazon%20Redshift.md) | Petabyte-scale data warehouse | OLAP queries across historical data |

## How to choose

```
Data is already in S3 and you need occasional SQL queries → Athena
Data needs to be moved/transformed between stores → AWS Glue
Data arrives continuously and must be processed in real time → Kinesis
You need a data warehouse for large-scale analytics → Redshift
You need log search and visualization → OpenSearch Service
You need dashboards for business users → QuickSight
You need large-scale Spark/Hadoop processing → EMR
```
