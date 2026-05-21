# Amazon OpenSearch Service

## What you'll learn
- What OpenSearch Service is and what it's used for
- How OpenSearch differs from relational databases
- Common use cases and integration patterns
- Key exam facts

---

## What is Amazon OpenSearch Service?

Amazon OpenSearch Service (formerly Amazon Elasticsearch Service) is a managed search and log analytics engine built on **OpenSearch** (an open-source fork of Elasticsearch). It provides near-real-time search, log analysis, and data visualization through **OpenSearch Dashboards** (formerly Kibana).

AWS manages the provisioning, patching, backups, and scaling of the underlying clusters.

---

## What it's designed for

OpenSearch is optimized for two main workloads:

**Full-text search** — Unlike a relational database that matches exact values, OpenSearch indexes the content of text fields and can find documents that *contain* a word or phrase. Good for: product search, document search, e-commerce search bars.

**Log analytics** — Ingest large volumes of log data (application logs, security logs, access logs), index it, and search or visualize it in near real time. A common pattern is to stream logs via **Kinesis Data Firehose** into OpenSearch, then visualize in OpenSearch Dashboards.

---

## Key features

- **Near-real-time indexing**: Documents are searchable within seconds of being ingested
- **Full-text search**: Relevance scoring, fuzzy matching, phrase matching
- **OpenSearch Dashboards**: Built-in web UI for creating visualizations, dashboards, and log explorers
- **Scalable**: Clusters scale from a single node to hundreds of nodes
- **Kinesis Firehose integration**: Stream data directly into OpenSearch for real-time log analysis
- **CloudWatch Logs integration**: Send log groups to OpenSearch for centralized log search
- **Security analytics**: Used as a backend for SIEM (security information and event management) tools

---

## When to use it

**Application log monitoring** — Aggregate logs from microservices, index them in OpenSearch, and use Dashboards to search for errors and build alerting.

**Security event analysis** — Ingest CloudTrail or VPC Flow Logs into OpenSearch for security analytics and visualization.

**E-commerce product search** — Build a search bar that returns relevant products based on text queries, supporting typo tolerance and faceted filtering.

**Time-series data dashboards** — Visualize metrics and events over time.

---

## OpenSearch vs other services

| | OpenSearch Service | Amazon RDS | Amazon Athena |
|-|-------------------|------------|---------------|
| Data model | Document index | Relational tables | Files in S3 |
| Query type | Full-text search, aggregations | SQL (exact matches) | SQL (batch) |
| Latency | Near-real-time | Milliseconds | Seconds/minutes |
| Best for | Log search, text search | Transactional data | Ad-hoc S3 queries |

---

## Exam focus

- OpenSearch = **search and log analytics** — look for keywords like "full-text search," "log analysis," "Kibana-style dashboards"
- Formerly called **Amazon Elasticsearch Service**
- Often paired with **Kinesis Data Firehose** for streaming log delivery
- Includes **OpenSearch Dashboards** for visualization
- Not the right answer for transactional queries (use RDS) or columnar analytics (use Redshift/Athena)

---

## Practice questions

**Q1.** A company wants to aggregate application logs from 50 microservices, search through them in near real time, and create dashboards to monitor error rates. Which service is most appropriate?

A) Amazon RDS  
B) Amazon Athena  
C) Amazon OpenSearch Service  
D) Amazon Redshift

**Answer: C** — OpenSearch Service is designed for log ingestion, near-real-time indexing, full-text search, and visualization with OpenSearch Dashboards. RDS is for relational transactional data. Athena queries batch data in S3. Redshift is a data warehouse for analytical queries on structured data.
