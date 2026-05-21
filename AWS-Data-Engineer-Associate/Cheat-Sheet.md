# AWS Data Engineer Associate — Cheat Sheet

> Quick reference for the DEA-C01 exam. Domain weights: D1=34%, D2=26%, D3=22%, D4=18%.

---

## DOMAIN 1 — Data Ingestion and Transformation (34%)

### Ingestion Service Decision Tree

```
Real-time streaming data?
├── Need custom consumer logic → Kinesis Data Streams
├── Just deliver to S3/Redshift/OpenSearch → Kinesis Data Firehose
└── Already using Apache Kafka → Amazon MSK

Batch/file transfer?
├── Database migration → AWS DMS
├── On-prem files/NFS/HDFS to S3 → AWS DataSync
└── Too much data for internet transfer → AWS Snow Family
    ├── < 80 TB → Snowball Edge
    └── > Petabyte → Snowmobile
```

### Kinesis Data Streams — Key Numbers
| Item | Value |
|---|---|
| Shard write capacity | 1 MB/s, 1,000 records/s |
| Shard read capacity | 2 MB/s (shared) or 2 MB/s per consumer (Enhanced Fan-Out) |
| Max record size | 1 MB |
| Default retention | 24 hours |
| Max retention | 365 days |
| Error when exceeded | `ProvisionedThroughputExceededException` |

**Kinesis Streams vs Firehose:**
| | Streams | Firehose |
|---|---|---|
| Consumer code needed? | Yes | No |
| Latency | Real-time (ms) | Near-real-time (60-900s buffer) |
| Auto-scales? | Manual reshard / On-demand mode | Yes (fully managed) |
| Destinations | Any (you code it) | S3, Redshift, OpenSearch, Splunk |

### AWS Glue — Core Concepts
| Component | Purpose |
|---|---|
| Data Catalog | Central metadata store (schema, location, partitions) |
| Crawler | Auto-discovers schema, creates/updates Catalog tables |
| ETL Job (Spark) | Distributed transformation at scale |
| ETL Job (Python Shell) | Single-node, small data |
| Streaming Job | Continuous Kinesis/Kafka → S3/Redshift |
| DataBrew | Visual, no-code data preparation |
| Data Quality | Rule-based data validation |
| Job Bookmark | Tracks processed data — reprocess only new data |
| Pushdown Predicate | Filter at S3 before reading into Spark → cost saving |
| DPU | Unit of compute (4 vCPU + 16 GB per DPU) |

### Data Formats
| Format | Type | When to use | Athena cost |
|---|---|---|---|
| CSV | Row-based | Simple, human-readable, small data | Most expensive |
| JSON | Row-based | APIs, semi-structured | Expensive |
| Parquet | Columnar | Analytics queries — **preferred** | Cheapest |
| ORC | Columnar | Hive/EMR — similar to Parquet | Cheapest |
| Avro | Row-based | Streaming, schema evolution | Medium |

**Rule**: Always convert raw CSV/JSON to Parquet before analytics. Saves 70-90% on Athena costs.

### S3 Partitioning
```
# Hive-style partitioning (most compatible)
s3://bucket/events/year=2024/month=01/day=15/data.parquet

# Benefits: Athena/Glue/Spectrum only read matching partitions
# Rule: Partition by columns you filter on most often
# Watch out: Don't over-partition → too many small files
```

### Orchestration Services
| Service | Use when |
|---|---|
| AWS Step Functions | AWS-native, visual workflow, multi-service orchestration |
| Amazon MWAA (Airflow) | Already using Airflow, complex DAG dependencies, open-source |
| AWS Glue Workflows | Glue-only pipelines (crawlers + jobs) |
| EventBridge | Event-triggered pipelines (new S3 file, schedule, pipeline completion) |

---

## DOMAIN 2 — Store and Manage Data (26%)

### Storage Service Decision Table
| Use case | Service |
|---|---|
| All data formats, any scale, data lake | Amazon S3 |
| Complex SQL analytics, BI, petabyte-scale | Amazon Redshift |
| Key-value / document, < 10ms latency, auto-scale | Amazon DynamoDB |
| Relational SQL, transactional apps | Amazon RDS / Aurora |
| Log analytics, full-text search | Amazon OpenSearch |
| Data lake governance, fine-grained permissions | AWS Lake Formation |
| Schema/metadata for data lake | AWS Glue Data Catalog |
| Time-series data (IoT, metrics) | Amazon Timestream |
| Graph data (networks, relationships) | Amazon Neptune |

### Data Lake vs Data Warehouse
| | Data Lake | Data Warehouse |
|---|---|---|
| Storage | S3 | Redshift |
| Schema | Schema-on-read | Schema-on-write |
| Data types | Raw, any format | Processed, structured |
| Query tool | Athena | Redshift SQL |
| Cost | Low storage, pay per query | Higher (dedicated cluster) |

### Amazon Redshift — Key Concepts
| Concept | Exam point |
|---|---|
| Distribution: EVEN | Large tables, no common join key |
| Distribution: KEY | Tables frequently joined on this column → reduces data movement |
| Distribution: ALL | Small dimension tables → replicate to every node |
| Sort key | Like an index — add to column used in WHERE range filters |
| COPY command | Fastest way to load data from S3 — parallel across all nodes |
| VACUUM | Reclaim space + re-sort after deletes/updates |
| ANALYZE | Update statistics for query planner |
| Redshift Spectrum | Query S3 data directly from Redshift without loading |
| RA3 | Decoupled compute + S3 storage |
| Serverless | Auto-scales, no cluster management, pay per use |

### S3 Storage Classes
| Class | Retrieval | Use case |
|---|---|---|
| Standard | Instant | Active data lake |
| Intelligent-Tiering | Instant | Unknown access patterns |
| Standard-IA | Instant (fee) | Data accessed monthly |
| One Zone-IA | Instant (fee) | Reproducible data |
| Glacier Instant | Instant | Archive, rarely accessed |
| Glacier Flexible | Minutes–hours | Compliance archive |
| Glacier Deep Archive | 12–48 hours | Cheapest, 7+ year retention |

### S3 Data Management
| Feature | What it does |
|---|---|
| Lifecycle rules | Auto-transition or expire objects by age |
| Versioning | Keep all object versions — protect against overwrites/deletes |
| Object Lock (WORM) | Prevent modification/deletion for compliance |
| CRR | Replicate to another Region for DR |
| SRR | Replicate within same Region |
| Event Notifications | Trigger Lambda/SQS/SNS on object events |

### AWS Lake Formation — Permissions
| Permission type | What it controls |
|---|---|
| Table permissions | SELECT, INSERT, DELETE, ALTER on entire tables |
| Column-level | Access to specific columns only |
| Row-level filters | Users see only rows matching a condition |
| LF-tags (TBAC) | Tag-based permissions at scale |
| Cross-account sharing | Share catalog resources to other AWS accounts |

---

## DOMAIN 3 — Data Operations and Support (22%)

### Amazon Athena
| Item | Value/Detail |
|---|---|
| Pricing | $5 per TB scanned |
| Formats | CSV, JSON, Parquet, ORC, Avro, and more |
| Engine | Presto/Trino (managed) |
| Schema source | Glue Data Catalog |
| Result cache | 7 days |
| Partition projection | Pre-compute partitions — faster queries, no S3 listing |
| Federated queries | Query DynamoDB, RDS, CloudWatch, custom via Lambda connector |
| Workgroups | Separate billing, quotas, and results per team |

**Athena cost optimization**: Parquet + partitioning + compression = 90% cost reduction vs CSV.

### Amazon QuickSight
| Feature | Detail |
|---|---|
| SPICE | In-memory engine — import data for fast dashboards |
| Direct query | Query Athena/Redshift live (no SPICE import) |
| Row-level security | Different users see different data |
| Embedded | Embed dashboards in your application |
| ML insights | Anomaly detection + forecasting built-in |

### Monitoring Key Metrics
| Service | Key Metric | What it means |
|---|---|---|
| Kinesis Streams | `IteratorAgeMilliseconds` | Consumer lag — if growing, consumer is behind |
| Kinesis Streams | `WriteProvisionedThroughputExceeded` | Need more shards |
| Glue | `glue.driver.aggregate.numFailedTasks` | Job failure rate |
| Redshift | `CPUUtilization` | Node CPU usage |
| Lambda | `Duration` + `Throttles` | Execution time and rate limiting |
| EMR | `YARNMemoryAvailablePercentage` | Cluster memory pressure |

### Data Quality Tools
| Tool | Type | Best for |
|---|---|---|
| AWS Glue Data Quality | Managed | Glue-native rule validation |
| AWS Deequ | Open-source (Spark) | Large-scale Spark data quality |
| Great Expectations | Open-source | Python-based, flexible |
| Pandera | Open-source | Pandas/Python DataFrame validation |
| AWS Glue DataBrew | Visual UI | No-code data profiling |

---

## DOMAIN 4 — Data Security and Governance (18%)

### Encryption at Rest
| Service | Options |
|---|---|
| S3 | SSE-S3 (AWS-managed), SSE-KMS (CMK), SSE-C (customer key), Client-side |
| Redshift | AES-256 + KMS (enable at cluster creation) |
| DynamoDB | AWS-owned key (default) or KMS CMK |
| RDS/Aurora | Enable at creation — KMS |
| Glue Data Catalog | KMS encryption for metadata |

**SSE-KMS advantages over SSE-S3:**
- You control the key — can disable/rotate/audit
- CloudTrail logs every key usage
- Can revoke access by disabling the key

### Enforce HTTPS on S3
```json
{"Effect": "Deny", "Action": "s3:*",
 "Condition": {"Bool": {"aws:SecureTransport": "false"}}}
```

### Data Privacy and PII
| Technique | Reversible? | Use case |
|---|---|---|
| Masking | No | Display data without exposing real values |
| Tokenization | Yes (with vault) | Payment processing, PII in analytics |
| Encryption | Yes (with key) | Store securely, decrypt when needed |
| Hashing | No | Consistent pseudonymization, joins |
| Pseudonymization | Yes (with mapping) | GDPR compliance |

### Compliance Services
| Service | Purpose |
|---|---|
| AWS Macie | Discover PII in S3 (names, SSNs, credit cards) |
| AWS CloudTrail | API audit trail — who did what, when |
| AWS Config | Configuration compliance rules |
| AWS Artifact | Access AWS compliance reports (SOC, PCI, ISO) |
| AWS Security Hub | Centralized security findings |

### Lake Formation Security
| Feature | Exam scenario |
|---|---|
| Column-level | "Analysts can't see SSN column" |
| Row filters | "Regional staff see only their region" |
| Cross-account | "Share data catalog with partner account" |
| Audit via CloudTrail | "Prove who queried which data" |

---

## SERVICE QUICK-FIRE COMPARISON

### Glue vs EMR
| | Glue | EMR |
|---|---|---|
| Management | Serverless | You manage cluster |
| Framework | Managed Spark | Spark + Hadoop + Hive + more |
| Custom libs | Limited | Install anything |
| Control | Less | More |
| Cold start | ~2 min | Instant (if cluster running) |

### DMS vs DataSync
- **DMS** = migrate/replicate DATABASES (structured, uses CDC)
- **DataSync** = transfer FILES (NFS, SMB, HDFS to S3/EFS)

### Kinesis Streams vs Kinesis Firehose vs MSK
- **Streams** = real-time, custom consumer, manual scaling
- **Firehose** = managed delivery to storage, near-real-time, no consumer code
- **MSK** = Kafka-compatible, open-source, existing Kafka ecosystem

### Step Functions vs MWAA
- **Step Functions** = AWS-native, visual, any AWS service, pay per transition
- **MWAA** = Apache Airflow, Python DAGs, open-source, always-on (more expensive)

---

## EXAM TRAPS

| Trap | Right answer |
|---|---|
| "Cheapest Athena" | Parquet + partitioning + compression |
| "Real-time data pipeline" | Kinesis Data Streams (not SQS) |
| "Deliver stream to S3 without custom code" | Kinesis Firehose (not Streams) |
| "Already use Kafka on-prem" | Amazon MSK |
| "Auto-discover S3 schema" | Glue Crawler |
| "Central schema for Athena + Redshift + EMR" | Glue Data Catalog |
| "Skip already-processed Glue data" | Job Bookmark |
| "Load data to Redshift fastest" | COPY command (not INSERT) |
| "Query S3 from Redshift without loading" | Redshift Spectrum |
| "Column-level security on data lake" | AWS Lake Formation |
| "PII in S3 detected automatically" | Amazon Macie |
| "Migrate Oracle to Aurora minimal downtime" | DMS + CDC |
| "Move 500 TB offline to AWS" | Snowball Edge |

---

*Exam: DEA-C01 | 65 questions | 130 minutes | Score 720/1000 to pass*
