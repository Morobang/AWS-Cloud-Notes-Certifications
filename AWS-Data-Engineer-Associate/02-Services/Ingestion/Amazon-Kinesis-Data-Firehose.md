# Amazon Kinesis Data Firehose

**Category**: Managed Streaming Delivery  
**Exam weight**: High — the default way to load streaming data into S3, Redshift, or OpenSearch

---

## What Is It?

Amazon Kinesis Data Firehose is a fully managed service that captures, transforms, and loads streaming data into data stores. You send records to Firehose; Firehose buffers them, optionally transforms them, and delivers them to a destination — no consumer code to write.

**Key distinction from Kinesis Data Streams**: Streams requires you to write consumer code. Firehose is a managed delivery pipeline — you point it at a destination and it handles everything.

---

## How It Works

```
Producers (apps, IoT, Kinesis Data Streams)
    ↓
Kinesis Data Firehose Delivery Stream
  ├── Buffer records (by size or time)
  ├── Optional transformation (Lambda)
  ├── Optional format conversion (JSON → Parquet/ORC)
  └── Deliver to destination
    ↓
Destination (S3 / Redshift / OpenSearch / HTTP endpoint / Splunk)
```

---

## Buffering

Firehose does not deliver records one at a time. It buffers and delivers in batches:

| Buffer hint | Range | What it means |
|---|---|---|
| Buffer size | 1 MB – 128 MB | Deliver when accumulated data reaches this size |
| Buffer interval | 60 – 900 seconds | Deliver when this time passes, even if size not reached |

Whichever threshold is met first triggers delivery. Lower interval = more frequent files = higher S3 PUT costs but lower latency.

**For near-real-time use cases**: Set interval to 60 seconds (minimum). Records land in S3 within ~60 seconds of being sent to Firehose.

---

## Destinations

| Destination | Use case |
|---|---|
| **Amazon S3** | Most common — build a data lake from streaming data |
| **Amazon Redshift** | Stream data directly to a Redshift table (via S3 COPY) |
| **Amazon OpenSearch** | Real-time log/event analytics |
| **HTTP endpoint** | Custom destination (your API, Datadog, New Relic, MongoDB) |
| **Splunk** | SIEM log analytics |

### S3 Delivery

Firehose automatically adds a UTC timestamp prefix to S3:
```
s3://my-bucket/prefix/2024/01/15/13/
  ├── delivery-stream-1-2024-01-15-13-00-00-uuid
  └── delivery-stream-1-2024-01-15-13-01-00-uuid
```

You can customize the prefix using dynamic partition keys — e.g., route records to different S3 prefixes based on a field value.

### Redshift Delivery

Firehose first writes to S3, then runs a Redshift `COPY` command to load the data. Intermediate S3 bucket is required.

---

## Lambda Transformation

Before delivery, Firehose can invoke a Lambda function to transform each record:

**Common transforms:**
- Add fields (e.g., add `ingestion_timestamp`)
- Filter records (drop test events)
- Convert format (parse a JSON string field, flatten nested JSON)
- Enrich records (lookup user region from DynamoDB)

**Lambda must return records in this format:**

```python
def handler(event, context):
    output = []
    for record in event['records']:
        # Decode
        import base64
        payload = base64.b64decode(record['data']).decode('utf-8')
        
        # Transform
        import json
        data = json.loads(payload)
        data['processed'] = True
        
        # Encode and return
        output.append({
            'recordId': record['recordId'],
            'result': 'Ok',
            'data': base64.b64encode(json.dumps(data).encode('utf-8')).decode('utf-8')
        })
    
    return {'records': output}
```

**Result values**: `Ok`, `Dropped` (skip the record), `ProcessingFailed` (send to error bucket).

---

## Format Conversion

Firehose can automatically convert JSON records to columnar formats:

**JSON → Parquet or ORC (no Lambda needed)**:
- Requires a Glue Data Catalog table as the schema source
- Firehose reads the schema from the catalog and converts records
- Columnar output → cheaper Athena queries

This is the recommended way to land streaming data in the data lake in an Athena-optimized format.

---

## Dynamic Partitioning

Route records to different S3 prefixes based on field values — without a Lambda:

```
{ "tenant_id": "acme", "event_type": "purchase", ... }

→ s3://my-lake/tenant=acme/event_type=purchase/2024/01/15/file.parquet
```

Configure jq-like expressions in Firehose to extract partition values from the record. Requires enabling dynamic partitioning and setting a buffer size small enough for the partitioning to be effective.

---

## Error Handling

Records that fail transformation or delivery are written to a separate S3 error bucket. You can set up an S3 Event Notification on the error bucket → Lambda → alert/reprocess.

---

## Firehose vs Kinesis Data Streams

| | Kinesis Data Streams | Kinesis Data Firehose |
|---|---|---|
| Consumer | You write consumer code | Fully managed — no code |
| Retention | 24h – 365 days | No storage — delivers immediately |
| Processing | Custom (Lambda, KDA, app) | Built-in Lambda + format conversion |
| Destinations | Any (your code decides) | S3, Redshift, OpenSearch, HTTP |
| Latency | Sub-second | 60–900 seconds (buffering) |
| Scaling | Manage shards manually | Automatic |
| Use case | Complex streaming, multiple consumers | Simple delivery to a known destination |

**Common pattern**: Kinesis Data Streams (buffer, fan-out to multiple consumers) → one consumer writes to Firehose → Firehose delivers to S3 as Parquet.

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Ingest IoT sensor data to S3 in Parquet format, no code needed | Kinesis Data Firehose with format conversion (JSON→Parquet) |
| Stream application logs to OpenSearch for real-time dashboards | Kinesis Data Firehose → Amazon OpenSearch |
| Filter test events from a stream before storing in S3 | Firehose with Lambda transformation (return `Dropped` for test events) |
| Route records to different S3 prefixes by customer ID | Firehose with dynamic partitioning |
| Need to replay records consumed 2 days ago | Kinesis Data Streams (not Firehose — Firehose has no replay) |
