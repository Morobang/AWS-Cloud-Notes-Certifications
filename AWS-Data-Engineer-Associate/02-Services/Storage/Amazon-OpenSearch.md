# Amazon OpenSearch Service

**Category**: Search and Log Analytics  
**Exam weight**: Medium — know when to choose OpenSearch vs Athena/Redshift, and common ingestion patterns

---

## What Is It?

Amazon OpenSearch Service is a managed service running OpenSearch (the open-source fork of Elasticsearch). It provides full-text search, log analytics, and real-time operational dashboards.

In data engineering, OpenSearch is most commonly a destination for log and event data, and a real-time analytics layer for operational metrics.

---

## OpenSearch Core Concepts

| Concept | Description | Relational analogy |
|---|---|---|
| **Index** | A collection of documents | Table |
| **Document** | A JSON record | Row |
| **Field** | A key-value pair in a document | Column |
| **Shard** | A partition of an index; enables parallel search | Partition |
| **Replica** | A copy of a shard for redundancy | Read replica |
| **Cluster** | One or more nodes running OpenSearch | Database cluster |

### Inverted Index

OpenSearch builds an inverted index — for each unique word, a list of all documents containing that word:

```
"error" → [doc1, doc7, doc15, doc23]
"timeout" → [doc1, doc3, doc23]
```

This is why full-text search is fast — instead of scanning all documents for a word, OpenSearch looks it up in the index directly.

---

## OpenSearch vs Other AWS Analytics Services

| | OpenSearch | Athena | Redshift |
|---|---|---|---|
| Data model | JSON documents | Files in S3 | Columnar warehouse |
| Query language | OpenSearch Query DSL (JSON) | SQL | SQL |
| Full-text search | Native | No | Limited |
| Real-time ingest | Yes (near-real-time) | No (batch queries) | Batch load |
| Cost model | Per cluster node-hour | Per TB scanned | Per cluster hour |
| Use case | Log analytics, search | Ad-hoc S3 queries | Complex BI analytics |

**Choose OpenSearch when**: Full-text search, keyword search, log analytics, real-time operational dashboards.  
**Do not use OpenSearch for**: Complex aggregations over structured data (use Athena/Redshift), long-term data lake storage (use S3).

---

## Ingestion Patterns

### Kinesis Firehose → OpenSearch

The most common real-time log pipeline:

```
Application logs → Kinesis Firehose → Amazon OpenSearch
                                    → Amazon S3 (backup)
```

Firehose delivers batches to OpenSearch automatically. S3 backup preserves raw logs.

### Lambda → OpenSearch

For custom processing before indexing:

```
DynamoDB Streams → Lambda → transform → OpenSearch
```

Use the OpenSearch Python client:
```python
from opensearchpy import OpenSearch, RequestsHttpConnection
from requests_aws4auth import AWS4Auth
import boto3

# Get AWS credentials for signing
credentials = boto3.Session().get_credentials()
awsauth = AWS4Auth(credentials.access_key, credentials.secret_key,
                  'us-east-1', 'es', session_token=credentials.token)

client = OpenSearch(
    hosts=[{'host': 'search-cluster.us-east-1.es.amazonaws.com', 'port': 443}],
    http_auth=awsauth,
    use_ssl=True,
    connection_class=RequestsHttpConnection
)

# Index a document
client.index(
    index='app-logs',
    body={
        'timestamp': '2024-01-15T10:30:00Z',
        'level': 'ERROR',
        'message': 'Connection timeout to database',
        'service': 'payment-service'
    }
)
```

### AWS DMS → OpenSearch

Replicate relational database data to OpenSearch for full-text search capabilities:

```
RDS MySQL → DMS CDC → Amazon OpenSearch
```

Users can then search the full text of product descriptions or customer notes.

---

## OpenSearch Dashboards

OpenSearch includes a built-in visualization layer (OpenSearch Dashboards, formerly Kibana):
- Time-series charts of log error rates
- Geographic maps of user traffic
- Real-time table views of latest events

Access via the OpenSearch console — no separate setup required.

---

## Index Management

### Index Templates

Pre-define mappings (field types) for all indices matching a pattern:

```json
{
  "index_patterns": ["logs-*"],
  "template": {
    "mappings": {
      "properties": {
        "timestamp": {"type": "date"},
        "level": {"type": "keyword"},
        "message": {"type": "text"},
        "duration_ms": {"type": "integer"}
      }
    }
  }
}
```

Without explicit mappings, OpenSearch infers types — `keyword` for exact matches, `text` for full-text, `date` for dates.

### Index State Management (ISM)

Automate index lifecycle: roll over when an index reaches 50 GB, move to warm storage after 30 days, delete after 90 days.

```
Hot tier  (SSD)  → Warm tier (HDD) → Cold tier (S3) → Delete
   0–7 days           7–30 days         30–90 days      90+ days
```

**OpenSearch cold storage**: Uses S3 for cost-effective storage of older indexes — much cheaper than keeping them on cluster nodes.

### Index Rollover

Create a new index when the current one reaches a size or age threshold:

```
logs-2024-01-01 (full at 50 GB) → logs-2024-01-02 (new current)
```

An alias always points to the current index — applications don't need to know the actual index name.

---

## OpenSearch Security

**Authentication**: AWS IAM authentication (sign requests with AWS credentials) or internal user database.

**Encryption**:
- At rest: KMS encryption on all data
- In transit: TLS 1.2+ for all API calls

**Fine-grained access control**: Role-based access at the index, document type, and field level.

**VPC access**: Deploy OpenSearch within a VPC — accessible only from within the VPC (no public endpoint).

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Application sends logs to Kinesis. Need real-time dashboards showing error rates. | Kinesis Firehose → Amazon OpenSearch + OpenSearch Dashboards |
| Users need to search product catalog by keyword across name, description, and tags. | Amazon OpenSearch (full-text search on JSON documents) |
| Log data in OpenSearch is growing — older logs are rarely accessed but must be retained. | OpenSearch Index State Management — move to warm/cold tier, delete after retention period |
| A DynamoDB table needs to be searchable by any field, not just the partition/sort key. | DynamoDB Streams → Lambda → OpenSearch (maintain a search index) |
| Complex SQL aggregations joining 5 tables with GROUP BY. | Amazon Redshift or Athena — not OpenSearch |
