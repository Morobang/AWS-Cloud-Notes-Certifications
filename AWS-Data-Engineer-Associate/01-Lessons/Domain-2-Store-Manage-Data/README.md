# Domain 2: Store and Manage Data

**Exam Weight: 26%** — ~17 questions

---

## What This Domain Covers

Choosing the right data store for the right workload, managing data lifecycles, and understanding the differences between data lake, data warehouse, and operational database patterns.

---

## The Storage Decision Framework

The most important skill in this domain is choosing the right storage service. Ask these questions:

```
Is it structured data needing SQL queries?
  ├── Transactional (OLTP, frequent writes) → RDS or Aurora or DynamoDB
  └── Analytical (OLAP, complex aggregations) → Redshift

Is it unstructured or semi-structured data?
  └── S3 (data lake) + Athena for ad-hoc queries

Is it key-value or document data needing < 10ms latency?
  └── DynamoDB

Is it log/search data needing full-text search?
  └── Amazon OpenSearch Service

Is it time-series data (IoT, metrics)?
  └── Amazon Timestream

Is it graph data (relationships, networks)?
  └── Amazon Neptune
```

---

## Data Lake vs Data Warehouse

| | Data Lake | Data Warehouse |
|---|---|---|
| **AWS service** | S3 + AWS Glue + Athena | Amazon Redshift |
| **Data format** | Any — raw, structured, unstructured | Structured, processed |
| **Schema** | Schema-on-read (define when querying) | Schema-on-write (define before loading) |
| **Use case** | All data, exploratory analytics, ML | Business reporting, structured analytics |
| **Cost** | Low storage cost, pay per query | Higher — dedicated compute cluster |
| **Flexibility** | High | Lower |

**Modern pattern**: Data Lakehouse — combine both. Store raw data in S3 (lake), load curated data into Redshift (warehouse). Use Redshift Spectrum to query S3 directly from Redshift.

---

## Key Services in This Domain

| Service | Type | When to use |
|---|---|---|
| **Amazon S3** | Object storage / Data lake | Store any data, any scale, any format |
| **Amazon Redshift** | Data warehouse | Complex analytics, BI, petabyte-scale SQL |
| **Amazon DynamoDB** | NoSQL database | High-throughput, low-latency key-value |
| **Amazon RDS / Aurora** | Relational DB | Transactional apps, SQL workloads |
| **Amazon OpenSearch** | Search + analytics | Log analytics, full-text search |
| **AWS Lake Formation** | Data lake governance | Centralize permissions, manage a data lake |
| **AWS Glue Data Catalog** | Metadata store | Central schema registry for the data lake |

---

## Task Statements in This Domain

| Task | Focus |
|---|---|
| [Task 2.1](./Task-2.1-Choose-Data-Store.md) | Choose the appropriate data store for a given use case |
| [Task 2.2](./Task-2.2-Transform-Data-Stores.md) | Understand data store schemas, design, and optimization |
| [Task 2.3](./Task-2.3-Manage-Data-Lifecycle.md) | Manage data lifecycle — retention, archival, deletion |

---

## Amazon Redshift — Deep Dive for the Exam

### Architecture
- **Leader node**: receives queries, parses SQL, creates execution plan
- **Compute nodes**: execute the plan, process data in parallel
- **RA3 node type**: separates storage (S3) from compute — can scale compute independently

### Key Design Concepts

**Distribution styles** (how data is spread across nodes):
| Style | When to use |
|---|---|
| `AUTO` | Let Redshift choose (default, usually good) |
| `EVEN` | Large tables with no obvious join key |
| `KEY` | Distribute by column used in frequent joins |
| `ALL` | Small dimension tables — replicate to every node |

**Sort keys** (like an index for Redshift):
- Compound sort key: sort by multiple columns in order (column1, column2)
- Interleaved sort key: equal weight to all columns — better for ad-hoc queries

**VACUUM + ANALYZE**: Regular maintenance commands
- `VACUUM`: Reclaims space and re-sorts rows after deletes/updates
- `ANALYZE`: Updates statistics used by the query planner

### Redshift Spectrum
Query data directly in S3 without loading it into Redshift. Uses Glue Data Catalog for schema. Powerful for:
- Querying archived data that's too large/old for the main cluster
- Joining warehouse data with lake data in one SQL query

### Redshift Serverless
No cluster management — Redshift scales automatically and you pay only for compute used. Good for:
- Variable/unpredictable workloads
- Teams that don't want to manage cluster sizing

---

## AWS Lake Formation

### What It Is
A service that makes it easier to set up, secure, and manage a data lake. It provides:
- Centralized access control — grant column-level permissions, row-level filters
- Data catalog integration with Glue
- Tag-based access control (LF-RBAC)
- Cross-account data sharing

### Without vs With Lake Formation

**Without Lake Formation**: You manage S3 bucket policies + IAM policies per user per table — gets messy with many users.

**With Lake Formation**: Define permissions in one place ("analyst role can read columns A, B, C from table X, but not column D which contains PII"). All services that query the catalog (Athena, EMR, Glue) respect these permissions automatically.
