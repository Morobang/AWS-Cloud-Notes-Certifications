# Amazon S3 (for Data Engineering)

**Category**: Object Storage / Data Lake Foundation  
**Exam weight**: Very high — every data engineering architecture touches S3

---

## What Is It?

Amazon S3 is the backbone of the AWS data lake. It stores virtually any data in any format at any scale, and almost every other data service reads from or writes to it.

For data engineering, S3 is not just "file storage" — it's a distributed storage platform with powerful features for organizing, protecting, and querying data at petabyte scale.

---

## S3 Data Lake Design

### Medallion Architecture on S3

```
s3://my-lake/
├── bronze/     ← Raw, unmodified data as ingested
│   └── events/date=2024-01-15/raw-events.json
├── silver/     ← Cleaned, validated, Parquet-formatted
│   └── events/year=2024/month=01/day=15/events.parquet
└── gold/       ← Business-aggregated, ready for analytics
    └── daily-summary/year=2024/month=01/summary.parquet
```

Different IAM roles/Lake Formation policies per layer — data scientists can read silver/gold but only ETL processes write to bronze.

### Partitioning

Partition data with Hive-style prefixes for Athena partition pruning:

```
s3://my-lake/events/year=2024/month=01/day=15/
```

Athena's `WHERE year=2024 AND month=01` only reads that month's data — massive cost savings.

**Partition granularity**: Choose based on query patterns. Daily partitions are most common. Hourly partitions only if queries routinely filter by hour (and each partition is large enough, >128 MB).

---

## S3 Storage Classes

| Class | Use in data lake | Min duration | Retrieval |
|---|---|---|---|
| Standard | Bronze (active ingestion), hot silver | None | Instant |
| Intelligent-Tiering | Unknown/variable access patterns | None | Instant |
| Standard-IA | Older silver layer (30-90 days ago) | 30 days | Instant |
| One Zone-IA | Reproducible data (can regenerate) | 30 days | Instant |
| Glacier Instant | Archive with occasional access | 90 days | Instant |
| Glacier Flexible | Compliance archive | 90 days | Minutes–hours |
| Glacier Deep Archive | Long-term legal retention (7+ years) | 180 days | 12–48 hours |

---

## Lifecycle Policies

Automatically transition or expire objects based on age:

```json
{
  "Rules": [{
    "Filter": {"Prefix": "bronze/"},
    "Status": "Enabled",
    "Transitions": [
      {"Days": 30, "StorageClass": "STANDARD_IA"},
      {"Days": 90, "StorageClass": "GLACIER_IR"},
      {"Days": 365, "StorageClass": "DEEP_ARCHIVE"}
    ],
    "Expiration": {"Days": 2555},
    "NoncurrentVersionExpiration": {"NoncurrentDays": 30}
  }]
}
```

---

## S3 Select and Glacier Select

Query individual S3 objects with SQL without loading the entire file:

```python
import boto3
s3 = boto3.client('s3')

response = s3.select_object_content(
    Bucket='my-lake',
    Key='silver/events/year=2024/month=01/events.parquet',
    ExpressionType='SQL',
    Expression="SELECT s.user_id, s.amount FROM S3Object s WHERE s.event_type = 'purchase'",
    InputSerialization={'Parquet': {}},
    OutputSerialization={'JSON': {}}
)
```

Returns only the matching rows, not the entire file. Useful for serverless record-level retrieval.

---

## S3 Versioning

When enabled, every write creates a new version. Deletes add a delete marker.

**Use in data engineering:**
- Protection against accidental ETL script overwrites
- Required for Cross-Region Replication
- Rollback to a previous version of a Glue script

**Cost**: Each version counts toward storage. Use lifecycle rules to expire old versions.

---

## S3 Multipart Upload

For files > 100 MB, use multipart upload:
- Split file into parts (5 MB – 5 GB each)
- Upload parts in parallel
- AWS assembles the complete object

```python
import boto3
from boto3.s3.transfer import TransferConfig

s3 = boto3.client('s3')
config = TransferConfig(
    multipart_threshold=100 * 1024 * 1024,  # 100 MB
    multipart_chunksize=100 * 1024 * 1024,   # 100 MB parts
    max_concurrency=10
)

s3.upload_file('large-file.parquet', 'my-bucket', 'silver/large-file.parquet', Config=config)
```

---

## S3 Event Notifications

Trigger downstream processing when objects are created:

**Targets:**
- SNS topic → fan out to multiple subscribers
- SQS queue → decouple from downstream consumers
- Lambda function → immediate processing
- EventBridge → route to any AWS service

```json
{
  "LambdaFunctionConfigurations": [{
    "LambdaFunctionArn": "arn:aws:lambda:...:ProcessFile",
    "Events": ["s3:ObjectCreated:*"],
    "Filter": {
      "Key": {
        "FilterRules": [
          {"Name": "prefix", "Value": "bronze/"},
          {"Name": "suffix", "Value": ".json"}
        ]
      }
    }
  }]
}
```

---

## S3 Access Patterns

### Pre-signed URLs

Generate a time-limited URL allowing anyone to upload or download a specific object — without AWS credentials:

```python
url = s3.generate_presigned_url(
    'get_object',
    Params={'Bucket': 'my-bucket', 'Key': 'report.csv'},
    ExpiresIn=3600  # 1 hour
)
```

Use for: allowing external partners to download reports, or web apps that upload directly to S3 without routing through your backend.

### Access Points

Named endpoints with their own access policies per team or use case. Instead of one complex bucket policy, each team has a simple access point with its own policy.

### S3 Object Lambda

Transform objects on retrieval — without creating modified copies:
- Read a CSV from S3, return JSON to the caller
- Strip PII fields from objects before serving to certain roles
- Enrich objects with database lookups on the fly

---

## S3 Tables

Amazon S3 Tables is purpose-built Iceberg table storage optimized for data lake workloads. It provides:
- Automatic file compaction (reduces small file problem)
- Snapshot management (automatic cleanup of old snapshots)
- Integrated with Athena, EMR, Glue, and Redshift Spectrum

Use S3 Tables when you want managed Iceberg storage without manually running compaction jobs.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Athena queries are expensive — scanning entire year of data for each query | Add date partitioning to S3 path, convert files to Parquet |
| Compliance requires data immutability for 7 years | S3 Object Lock in Compliance mode |
| Need to trigger a Glue job when a new file arrives in S3 | S3 Event Notification → Lambda → start Glue job |
| Bronze layer data should automatically move to Glacier after 1 year | S3 Lifecycle rule with transition after 365 days |
| Multiple teams need different access policies to the same bucket | S3 Access Points — one per team |
| Data lake needs ACID transactions, row-level updates, time travel | Apache Iceberg on S3 (or Amazon S3 Tables) |
