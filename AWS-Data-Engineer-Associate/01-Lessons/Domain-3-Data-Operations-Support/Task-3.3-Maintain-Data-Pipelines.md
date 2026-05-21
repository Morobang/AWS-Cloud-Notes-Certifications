# Task 3.3: Maintain and Monitor Data Pipelines

## Learning Objectives
- Monitor data pipelines with CloudWatch metrics, alarms, and dashboards
- Diagnose and fix common pipeline failures
- Optimize pipeline cost and performance
- Implement observability patterns for data quality and data freshness

---

## CloudWatch Monitoring for Data Services

### Glue Job Monitoring

| Metric | What it means | Alert threshold |
|---|---|---|
| `glue.driver.aggregate.numFailedTasks` | Count of failed Spark tasks | Alert if > 0 |
| `glue.driver.aggregate.numCompletedStages` | Pipeline progress | Use for job tracking |
| `glue.ALL.jvm.heap.used` | JVM heap memory usage | Alert if > 80% |
| `glue.driver.aggregate.bytesRead` | Total data read | Use for cost estimation |
| `glue.driver.ExecutorRunTime` | Executor CPU time | Performance baseline |

**Glue Job Bookmark**: Prevents reprocessing. Glue tracks which S3 objects have already been processed. On the next run, it only processes new files. Enable with `--job-bookmark-option job-bookmark-enable`.

**When a Glue job fails with OOM (out of memory):**
1. Look at `glue.ALL.jvm.heap.used` — confirms memory pressure
2. Increase `WorkerType` from `G.1X` to `G.2X` (doubles memory per worker)
3. Or add more workers (more parallelism, same memory per worker)
4. Use pushdown predicates to read fewer records

### Kinesis Monitoring

| Metric | What it means |
|---|---|
| `IncomingRecords` | Records written per second per shard |
| `GetRecords.IteratorAgeMilliseconds` | Consumer lag — how far behind the consumer is |
| `WriteProvisionedThroughputExceeded` | Shard is at capacity — producers are throttled |
| `ReadProvisionedThroughputExceeded` | Consumer reading too fast from a shard |

**IteratorAgeMilliseconds is the most important Kinesis metric.** If it's growing, your consumer isn't keeping up with the stream. Fix: add more consumer instances, or increase the shard count.

**Resharding**: Scale Kinesis by splitting or merging shards.
- `SplitShard`: splits one shard into two — doubles throughput capacity for that range
- `MergeShards`: combines two adjacent shards — reduces cost for low-traffic streams

### EMR Monitoring

| Metric | What it means |
|---|---|
| `HDFSUtilization` | HDFS disk usage percentage |
| `YARNMemoryAvailableMB` | Available cluster memory |
| `CoreNodesPending` | Nodes waiting to start (cluster may need scaling) |
| `RunningMapTasks` | Current Hadoop MapReduce activity |

For EMRFS (S3-backed) workloads, HDFS metrics are less relevant — focus on YARN memory and task completion rates.

**EMR Auto Scaling**: Set min/max instance counts and scale policies based on YARN metrics.

### Lambda Monitoring

| Metric | What it means |
|---|---|
| `Duration` | Execution time (p50, p99 are most useful) |
| `Errors` | Function errors (exceptions) |
| `Throttles` | Function is at concurrency limit |
| `ConcurrentExecutions` | Active function instances right now |
| `IteratorAge` | Lag for Kinesis/DynamoDB Streams triggers |

**Lambda concurrency limits**: Default account limit is 1,000 concurrent executions. If hit, new invocations are throttled. Request a limit increase or set reserved concurrency.

### DMS Monitoring

| Metric | What it means |
|---|---|
| `CDCLatencySource` | Lag between source database write and DMS capturing it |
| `CDCLatencyTarget` | Lag between DMS capturing and writing to target |
| `FreeableMemory` | Replication instance memory |
| `ReplicationSlotDiskUsage` | For PostgreSQL sources — WAL disk usage |

High `CDCLatencyTarget` → target is the bottleneck (slow writes to destination). Scale the replication instance or destination service.

---

## CloudWatch Alarms and Dashboards

### Setting Up Pipeline Alarms

Every data pipeline should have alarms for:

1. **Job failure**: SNS notification when Glue/EMR/Batch job fails
2. **Data freshness**: Alarm if the latest partition in S3 is more than X hours old (use a Lambda to check and emit a custom metric)
3. **Consumer lag**: Kinesis IteratorAgeMilliseconds above threshold
4. **Abnormal record counts**: Too few or too many records processed (indicates upstream issue)

```python
# Lambda function: emit custom data freshness metric
import boto3
from datetime import datetime, timezone

cloudwatch = boto3.client('cloudwatch')
s3 = boto3.client('s3')

def handler(event, context):
    # Check the latest partition in S3
    today = datetime.now(timezone.utc).strftime('%Y/%m/%d')
    response = s3.list_objects_v2(
        Bucket='my-data-lake',
        Prefix=f'events/date={today}/',
        MaxKeys=1
    )
    
    files_today = response.get('KeyCount', 0)
    
    cloudwatch.put_metric_data(
        Namespace='DataPipeline',
        MetricData=[{
            'MetricName': 'TodayFilesLoaded',
            'Value': files_today,
            'Unit': 'Count'
        }]
    )
```

Create a CloudWatch Alarm on `TodayFilesLoaded < 1` → sends SNS alert if no data loaded today.

### CloudWatch Logs Insights for Glue

Query Glue job logs directly in CloudWatch Logs Insights:

```
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

Glue job logs appear in `/aws-glue/jobs/logs-v2` log group.

---

## Dead Letter Queues (DLQ)

For event-driven pipelines (Lambda, SQS, Kinesis, SNS), configure a DLQ to capture failed messages instead of losing them.

**Lambda DLQ pattern:**
```
Kinesis → Lambda (fails after 3 retries) → SQS DLQ
```

The failed record lands in SQS where you can:
- Inspect it (why did it fail?)
- Re-process it after fixing the bug
- Archive it for manual review

**SQS redrive policy**: After a message fails N times in the main queue, automatically move it to the DLQ.

**Kinesis on-failure destination**: Configure Lambda's event source mapping to send failed Kinesis batches to SQS or S3.

---

## Cost Optimization

### Athena

- Use Parquet/ORC → reduces bytes scanned → reduces cost
- Partition data → filter by partition → less data scanned
- Use result caching (free for duplicate queries within 7 days)
- Set workgroup scan limits to prevent runaway queries

### Glue ETL

- Use **Glue job bookmarks** — skip already-processed files
- Use **pushdown predicates** — filter at the source before loading into memory
- Right-size DPUs — profile with CloudWatch to find the right worker count
- Use **Glue triggers** instead of always-on polling

### EMR

- Use **Spot Instances for Task nodes** — task nodes don't hold HDFS data, so losing them doesn't affect durability; Spot gives 60-90% discount
- Keep Core nodes On-Demand (they hold HDFS data)
- Use **EMRFS (S3)** instead of HDFS — terminate cluster between jobs → no idle cost
- Auto-scale based on YARN metrics

### Kinesis Data Streams

- Pay per shard-hour — don't over-provision shards
- Monitor `WriteProvisionedThroughputExceeded` → resize when needed (not upfront)
- Use **Enhanced Fan-Out** only for consumers that need dedicated throughput; regular GetRecords is cheaper

### Redshift

- Pause the cluster during off-hours (Redshift Serverless does this automatically)
- Use compression encodings — run `ANALYZE COMPRESSION` to get recommendations
- Use **Concurrency Scaling** instead of over-provisioning — Redshift adds capacity during peaks, you only pay for the extra seconds

---

## Data Quality Monitoring

### AWS Glue Data Quality

Define rules in Glue Data Quality (DQDL — Data Quality Definition Language):

```
Rules = [
  RowCount > 1000,
  IsComplete "user_id",
  Uniqueness "event_id" > 0.99,
  ColumnValues "amount" between 0 and 100000,
  ColumnDataType "created_at" = "TIMESTAMP"
]
```

Run these as part of a Glue ETL job:
- Pass: continue pipeline
- Fail: stop pipeline, send alert, quarantine bad records

### Record Count Checks

A simple but effective quality check: compare record counts at each stage.

```
Source records: 1,000,000
After ingestion to S3: 1,000,000 ✓
After Glue transform: 998,500 (0.15% dropped for bad records — expected)
After Redshift load: 998,500 ✓
```

If any count drops unexpectedly, trigger an alert. Track these as CloudWatch custom metrics.

---

## Observability Best Practices

### The Three Observability Pillars

1. **Metrics**: Numerical measurements (records processed, job duration, error count)
2. **Logs**: Text records of what happened (Glue job logs, CloudTrail API calls)
3. **Traces**: End-to-end request tracking (AWS X-Ray for distributed pipelines)

### Pipeline Observability Checklist

For every production pipeline:
- CloudWatch alarm on job failure → SNS notification
- Custom metric on records processed per run (emit from Lambda/Glue)
- CloudWatch alarm on data freshness (alert if today's partition missing by 8 AM)
- DLQ for event-driven pipelines
- Glue Data Quality rules on critical tables
- Monthly cost review per pipeline (tag all resources with pipeline name)

---

## Exam Scenarios

**"A Kinesis Data Streams consumer is processing events but IteratorAgeMilliseconds is growing steadily over 24 hours."**
→ Consumer is lagging — add more consumer instances or split shards to increase throughput

**"A Glue ETL job fails with Java heap space error."**
→ OOM — increase worker type from G.1X to G.2X, or add pushdown predicates to read less data

**"Lambda functions processing SQS messages occasionally fail with transient errors. Failed messages are being lost."**
→ Configure an SQS DLQ on the Lambda's event source — failed messages are routed to DLQ after max retries

**"A data engineer wants to ensure that no more than 5% of records in a Glue job output have null values in the customer_id column."**
→ Define a Glue Data Quality rule: `IsComplete "customer_id" > 0.95`

**"An Athena workgroup is generating unexpectedly high bills because analysts are running full-table scans."**
→ Set a per-query data scan limit in the workgroup configuration
