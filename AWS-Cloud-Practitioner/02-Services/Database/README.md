# Database Services

AWS offers managed database services for every data model — relational, NoSQL, in-memory, graph, and document.

## Services in this category

| Service | Type | Key use case |
|---------|------|--------------|
| [Amazon RDS](./Amazon%20RDS.md) | Relational (SQL) | Traditional SQL databases — managed |
| [Amazon Aurora](./Amazon%20Aurora.md) | Relational (SQL) | High-performance MySQL/PostgreSQL compatible |
| [Amazon DynamoDB](./Amazon%20DynamoDB.md) | NoSQL (key-value/document) | Serverless, high-scale NoSQL |
| [Amazon ElastiCache](./Amazon%20ElastiCache.md) | In-memory cache | Sub-millisecond latency data layer |
| [Amazon Redshift](../Analytics/Amazon%20Redshift.md) | Data warehouse (OLAP) | Large-scale analytics |
| [Amazon DocumentDB](./Amazon%20DocumentDB.md) | Document (MongoDB-compatible) | JSON document data |
| [Amazon Neptune](./Amazon%20Neptune.md) | Graph | Highly connected relationships |

## How to choose

```
Need a managed MySQL/PostgreSQL/SQL Server/Oracle → RDS
Need MySQL/PostgreSQL but faster and more scalable → Aurora
Need serverless NoSQL at any scale → DynamoDB
Need to cache frequently accessed data → ElastiCache
Need to analyze petabytes of historical data → Redshift
Need MongoDB-compatible document storage → DocumentDB
Need to traverse connected relationships (social graph, fraud) → Neptune
```
