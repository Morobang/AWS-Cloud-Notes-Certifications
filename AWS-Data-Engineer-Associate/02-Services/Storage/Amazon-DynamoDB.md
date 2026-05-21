# Amazon DynamoDB (for Data Engineering)

**Category**: NoSQL Database  
**Exam weight**: High — DynamoDB appears as a source, target, and state store in data pipelines

---

## What Is It?

DynamoDB is a fully managed, serverless NoSQL key-value and document database delivering single-digit millisecond performance at any scale. In data engineering, it serves as:
- A high-throughput write target for streaming pipelines
- A low-latency lookup store for pipeline enrichment
- A state tracking table for pipeline metadata
- A source for DMS CDC migration

---

## Data Model

### Primary Keys

Every DynamoDB table requires a primary key. Two options:

**Simple primary key (partition key only):**
```
Table: user_profiles
Partition key: user_id (e.g., "usr-001")
```
Every item accessed by its unique user_id. No two items can have the same user_id.

**Composite primary key (partition key + sort key):**
```
Table: order_history
Partition key: user_id  (e.g., "usr-001")
Sort key:      order_date (e.g., "2024-01-15T10:30:00Z")
```
Multiple items can share the same partition key — differentiated by sort key. Query: all orders for a user in January.

### Partition Key Design Rules

- **Spread load**: High-cardinality partition keys distribute traffic across DynamoDB's internal partitions. Avoid low-cardinality keys (e.g., `status = active/inactive` on a million-item table → "hot partition")
- **Access pattern driven**: The partition key must match your primary access pattern

### Write Sharding (Hot Partition Solution)

If a partition key is naturally low-cardinality (e.g., sensor_type), add a random suffix:

```
Original: sensor_type = "temperature"
Sharded:  sensor_type = "temperature#3"  (where #3 is random 0-9)
```

Writers distribute across 10 partitions. Readers query all 10 and combine results.

---

## Indexes

### Global Secondary Index (GSI)

Query on attributes that aren't the primary key:

```
Table: orders
Primary key: order_id
GSI: customer_id (partition) + order_date (sort)

Query: "Get all orders for customer C-100 in the last 30 days"
→ GSI query: customer_id = "C-100" AND order_date >= "2024-01-01"
```

GSIs maintain their own throughput capacity — provision separately.

### Local Secondary Index (LSI)

Alternative sort keys on the same partition key:

```
Table: events
Primary key: user_id + timestamp
LSI: user_id + event_type
→ Query: all events of type "purchase" for user X (without scanning all events)
```

LSIs must be created at table creation time. GSIs can be added later.

---

## Read/Write Capacity

**Provisioned mode**: Set explicit Read Capacity Units (RCU) and Write Capacity Units (WCU). Auto Scaling adjusts these based on traffic.

**On-Demand mode**: Pay per request. DynamoDB handles scaling automatically.

| Mode | Use when |
|---|---|
| Provisioned | Predictable, steady-state workloads |
| On-Demand | Spiky, unpredictable, or new tables |

**Capacity units:**
- 1 WCU = 1 KB write per second
- 1 RCU = 1 strongly consistent 4 KB read per second (or 2 eventually consistent reads)

---

## DynamoDB Streams

Streams capture ordered change events for each item in a table:

```
INSERT item → stream record (NewImage = new item)
UPDATE item → stream record (OldImage = before, NewImage = after)
DELETE item → stream record (OldImage = deleted item)
```

**Retention**: 24 hours.

**Common uses:**
- Lambda trigger: process changes in real time
- Replicate to OpenSearch: full-text search index
- Audit log: write all changes to S3
- Cross-region replication: DynamoDB Global Tables uses Streams internally

```python
def handler(event, context):
    for record in event['Records']:
        if record['eventName'] == 'INSERT':
            new_item = record['dynamodb']['NewImage']
            # Deserialize and process new item
```

---

## DAX (DynamoDB Accelerator)

In-memory cache in front of DynamoDB. Fully managed, API-compatible.

- Read latency: milliseconds → **microseconds**
- Write latency: unchanged (writes go through DAX to DynamoDB)
- Cache: item-level and query-level caching
- Use for: read-heavy workloads with repetitive access patterns

**When NOT to use DAX**: Strongly consistent reads (DAX serves eventually consistent reads), write-heavy workloads.

---

## DynamoDB TTL (Time To Live)

Automatically delete items after an expiration timestamp:

```python
import time
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('pipeline_state')

# Store pipeline run state, auto-delete after 7 days
table.put_item(Item={
    'pipeline_id': 'daily-etl-2024-01-15',
    'status': 'completed',
    'records_processed': 1500000,
    'expires_at': int(time.time()) + (7 * 24 * 60 * 60)
})
```

TTL deletes occur within 48 hours of expiration. Deleted items appear in Streams as DELETE events (useful for archival).

---

## DynamoDB in Data Engineering Patterns

### Enrichment Lookup (Low-Latency Reference Data)

In a streaming pipeline, Lambda needs to enrich records with user profile data:

```
Kinesis record → Lambda → DynamoDB GetItem (user profile) → enriched record → S3
```

DynamoDB at microsecond latency allows enrichment without adding significant processing delay.

### Pipeline State Tracking

Track which files have been processed to implement idempotency:

```
Table: processing_state
PK: file_key (S3 path)
Attributes: status, processed_at, record_count
```

Before processing a file, check if it's already in the table. After processing, write the status. Prevents duplicate processing.

### DynamoDB as a Real-Time Target

Kinesis Data Streams → Lambda → DynamoDB: build a real-time materialized view:

```
Order events (Kinesis) → Lambda → DynamoDB "current_cart" table
                                   (always contains each user's current cart state)
```

---

## DynamoDB Exports

Export entire tables to S3 for analytics without impacting live traffic:

```python
table.export_to_s3(
    S3Bucket='my-lake',
    S3BucketOwner='123456789',
    ExportFormat='DYNAMODB_JSON'  # or AMAZON_ION
)
```

Export writes DynamoDB JSON format to S3. Run a Glue job to convert to Parquet for Athena.

**Point-in-time export**: Export the table as of any point in the last 35 days (requires PITR enabled).

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Real-time leaderboard: get top 100 scores in under 5ms | DynamoDB with proper partition key design (or + DAX) |
| Enrichment: Lambda needs to look up user city from user_id in a streaming pipeline | DynamoDB GetItem (single lookup per record at sub-ms latency) |
| Detect that a Lambda function wrote the same record twice | DynamoDB conditional write: `ConditionExpression="attribute_not_exists(pk)"` |
| Export DynamoDB table to S3 for analytics | DynamoDB export to S3, then Glue/Athena |
| Automatically expire session tokens after 24 hours | DynamoDB TTL attribute set to `now() + 86400 seconds` |
