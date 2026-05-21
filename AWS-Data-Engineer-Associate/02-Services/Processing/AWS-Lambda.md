# AWS Lambda (for Data Engineering)

**Category**: Serverless Compute / Event-Driven Processing  
**Exam weight**: High — Lambda appears in virtually every data pipeline architecture

---

## What Is It?

Lambda is a serverless, event-driven compute service. You write a function; Lambda runs it in response to an event and scales automatically. For data engineering, Lambda handles lightweight transformations, event routing, and pipeline coordination.

---

## Lambda in Data Pipelines

Lambda is not a replacement for Glue or EMR — it's a complement. Its role is:

**Event-driven coordination:**
```
S3 upload → Lambda → start Glue job
DynamoDB write → Lambda → push to Kinesis
EventBridge → Lambda → run validation check → notify on failure
```

**Lightweight transformation:**
```
Kinesis stream → Lambda → parse/enrich record → write to DynamoDB
SQS queue → Lambda → clean record → write to S3
```

**Orchestration glue:**
```
Step Functions → Lambda → check file count → return result → Step Functions decides next step
```

---

## Key Lambda Concepts

### Triggers (Event Sources)

| Trigger | Invocation model | Notes |
|---|---|---|
| S3 (ObjectCreated) | Async | Lambda invoked when file lands in bucket |
| Kinesis Data Streams | Sync (polling) | Lambda polls shards; batch of records per invocation |
| DynamoDB Streams | Sync (polling) | Lambda polls stream; batch of change records |
| SQS | Sync (polling) | Lambda polls queue; batch up to 10,000 messages |
| SNS | Async | Lambda invoked per SNS message |
| EventBridge | Async | Lambda invoked on matched event rule |
| API Gateway | Sync | HTTP request → Lambda response |
| Kinesis Firehose | Sync | Called for transformation; must return records to Firehose |

### Invocation Models

**Synchronous**: Caller waits for Lambda to return a response. Example: API Gateway, Kinesis (event source mapping), Firehose.

**Asynchronous**: Lambda is called and the caller continues without waiting. Example: S3 events, SNS. Lambda retries up to 2 times on failure. Dead-Letter Queue (DLQ) captures failed invocations.

### Lambda Limits Relevant to Data Engineering

| Limit | Value |
|---|---|
| Max execution duration | 15 minutes |
| Max memory | 10,240 MB (10 GB) |
| Max deployment package | 250 MB (unzipped) |
| `/tmp` storage | 512 MB – 10 GB |
| Max concurrency (default) | 1,000 (soft limit, can increase) |
| Max payload (sync) | 6 MB |
| Max payload (async) | 256 KB |
| Batch size (Kinesis) | 1 – 10,000 records |
| Batch size (SQS) | 1 – 10,000 messages |

**15-minute limit is the key constraint.** If your transformation takes longer, use Glue, EMR, or AWS Batch.

---

## Lambda for Kinesis and SQS

### Kinesis Event Source Mapping

Lambda polls Kinesis shards and invokes your function with a batch:

```python
def handler(event, context):
    for record in event['Records']:
        # Decode the Kinesis record
        import base64
        data = base64.b64decode(record['kinesis']['data']).decode('utf-8')
        
        # Process
        import json
        payload = json.loads(data)
        
        # Write to target
        process_record(payload)
```

**Parallelization factor**: By default, one Lambda invocation per shard at a time. You can increase parallelization to up to 10 concurrent invocations per shard.

**On-failure destinations**: If a batch consistently fails (after all retries), route it to an SQS DLQ or S3 for inspection.

### SQS Event Source Mapping

Lambda processes SQS messages in batches:

```python
def handler(event, context):
    for record in event['Records']:
        body = json.loads(record['body'])
        process_message(body)
        # Success = Lambda auto-deletes the message from SQS
        # Failure = message returns to queue (or goes to DLQ)
```

**Bisect on error**: If a batch fails, Lambda splits the batch in half and retries each half — isolates the bad record.

---

## Lambda Layers

Layers are ZIP archives containing dependencies, shared code, or custom runtimes. They're mounted at `/opt` in the Lambda execution environment.

**Use layers when:**
- Multiple Lambda functions share the same library (e.g., `pandas`, `boto3` customization)
- Deployment package would exceed size limits without extracting common dependencies

```
Lambda function (your code)     ~10 MB
Lambda Layer (pandas + numpy)   ~50 MB
Total: 60 MB — within limits
```

---

## Lambda and AWS Glue Integration

Lambda can start and monitor Glue jobs:

```python
import boto3
import json

def handler(event, context):
    glue = boto3.client('glue')
    
    # Get the S3 path from the trigger event
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # Start the Glue job
    response = glue.start_job_run(
        JobName='transform-incoming-data',
        Arguments={
            '--input_path': f's3://{bucket}/{key}',
            '--output_path': 's3://processed-data/silver/',
            '--processing_date': key.split('/')[2]  # Extract date from path
        }
    )
    
    return {
        'statusCode': 200,
        'jobRunId': response['JobRunId']
    }
```

---

## Lambda Concurrency

**Reserved concurrency**: Guarantee a specific concurrency limit for a function. Other functions cannot use these reserved slots — prevents a high-traffic function from starving others.

**Provisioned concurrency**: Pre-warm Lambda containers to eliminate cold starts. Use for latency-sensitive functions where the first invocation cannot be slow.

**Cold starts**: Lambda must initialize a new container for the first invocation (or after inactivity). With Python, typical cold start is 100-500ms. Provisioned concurrency eliminates this.

---

## Lambda Destinations

For async invocations, route results to:
- **On success**: SNS, SQS, EventBridge, or another Lambda
- **On failure**: SNS, SQS, EventBridge, or another Lambda (more flexible than DLQ)

```
Async invocation (S3 event → Lambda)
    ├── Success → SNS (notify pipeline complete)
    └── Failure (after 2 retries) → SQS (dead letter)
```

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Trigger a Glue job when a new file lands in S3 | S3 Event Notification → Lambda → `glue.start_job_run()` |
| Process Kinesis records, transform, and write to DynamoDB | Lambda with Kinesis event source mapping |
| Process SQS messages, some fail intermittently — don't lose them | SQS DLQ — failed messages route there after max retries |
| Lambda needs pandas library, deployment package too large | Lambda Layer containing pandas/numpy |
| A data transformation runs for 20 minutes | Lambda is not suitable (15-min limit) — use Glue, EMR, or AWS Batch |
| Firehose records need JSON enrichment before landing in S3 | Lambda transformation function attached to Firehose delivery stream |
