# Domain 3: Data Operations and Support — Practice Questions

> 18 questions | ~22% of the DEA-C01 exam

---

**Q1.** A Kinesis Data Streams consumer's `GetRecords.IteratorAgeMilliseconds` metric has been growing steadily for 3 hours. What does this indicate, and what is the BEST corrective action?

A) The producer is throttled — increase shard count  
B) The consumer is falling behind — add more consumer instances or split shards  
C) The data in the stream has expired — increase retention period  
D) The consumer is reading too fast — throttle it

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

`IteratorAgeMilliseconds` measures the age of the last record a consumer fetched from the stream. If it's growing, the consumer is processing records more slowly than they're being produced — it's falling behind. Solutions: add more consumer instances (scale out the consumer application) or split shards to increase parallelism.

- A (increase shard count because producer is throttled) — watch `WriteProvisionedThroughputExceeded` for producer throttling.
- C (retention period) — IteratorAge doesn't indicate expired data; it indicates consumer lag.
- D (throttle consumer) — would make the lag worse, not better.
</details>

---

**Q2.** A Glue ETL job fails with "Java heap space" errors intermittently on large input files. CloudWatch shows `glue.ALL.jvm.heap.used` peaking at 95% before failure. What is the BEST fix?

A) Increase the number of workers (DPUs)  
B) Upgrade the worker type from G.1X to G.2X  
C) Convert input files from CSV to Parquet  
D) Add pushdown predicates to the Glue job

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

G.2X workers have 8 vCPUs and 32 GB RAM (vs G.1X's 4 vCPUs and 16 GB RAM). When JVM heap is exhausted, increasing memory per worker (upgrading worker type) directly addresses OOM failures.

- A (more workers) adds more parallelism but the same memory per worker — won't fix OOM if the issue is data per-partition size.
- C (Parquet conversion) improves read performance but doesn't change the memory available to the JVM.
- D (pushdown predicates) reduces data read from S3 and is a good practice, but if the OOM is happening during processing (not during read), it won't fix the root cause.
</details>

---

**Q3.** A data pipeline runs nightly: it reads from S3, runs a Glue transformation, and loads to Redshift. The pipeline succeeds but the data loaded to Redshift is smaller than expected. What observability measure would have caught this earlier?

A) CloudWatch alarm on Glue `numFailedTasks`  
B) Custom CloudWatch metric emitting record counts at each pipeline stage  
C) CloudTrail logging for all S3 GetObject API calls  
D) Redshift STL query logs for slow queries

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Record count tracking at each stage (S3 input → Glue output → Redshift load) creates a data volume baseline. A CloudWatch alarm on "Redshift loaded records < 80% of Glue output records" would catch the discrepancy before business users notice.

- A (numFailedTasks) detects Spark task failures — useful but wouldn't catch a silent data drop.
- C (CloudTrail GetObject) tracks who accessed files — doesn't track record counts.
- D (Redshift slow queries) is performance monitoring, not data volume monitoring.
</details>

---

**Q4.** A Lambda function processes SQS messages to enrich records and write to DynamoDB. Occasionally, a DynamoDB `ProvisionedThroughputExceededException` causes Lambda to fail. Failed messages are being lost. What is the CORRECT configuration to prevent message loss?

A) Increase Lambda memory to reduce processing time  
B) Configure a Dead Letter Queue (DLQ) on the SQS queue  
C) Increase DynamoDB write capacity units  
D) Set Lambda reserved concurrency to 1

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

A DLQ on the SQS queue receives messages that fail processing after the configured maximum receive count. Failed messages aren't lost — they go to the DLQ for investigation and reprocessing. Both B and C could be valid, but B prevents message loss regardless of why Lambda fails.

- A (more Lambda memory) speeds up Lambda but doesn't prevent DynamoDB throttling or message loss.
- C (increase DynamoDB WCU) solves the throttling root cause but doesn't protect against other failure modes — DLQ provides a safety net.
- D (reserved concurrency = 1) would serialize processing and reduce DynamoDB write pressure, but dramatically reduces throughput and still doesn't protect against other errors.
</details>

---

**Q5.** An Athena workgroup has a per-query data scan limit of 10 GB. A data analyst runs a query that would scan 15 GB and receives an error. What is the purpose of this configuration?

A) Prevent queries that are expected to fail  
B) Control and limit query costs per team  
C) Enforce query timeout policies  
D) Protect S3 buckets from unauthorized access

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Athena charges $5 per TB scanned. Per-query and per-workgroup scan limits prevent accidental full-table scans from generating large unexpected bills. This is a cost control mechanism.

- A (prevent expected failures) — scan limits reject based on data volume, not expected success.
- C (query timeout) — Athena has separate query timeout settings.
- D (S3 access control) — IAM and bucket policies handle S3 access, not workgroup scan limits.
</details>

---

**Q6.** A data quality rule in AWS Glue Data Quality is defined as: `RowCount > 100000`. A Glue job runs and produces only 50,000 records, causing the rule to fail. What happens next in the pipeline by default?

A) The Glue job is retried automatically  
B) The pipeline continues with a warning in CloudWatch  
C) The Glue job status is set to FAILED and downstream steps are not executed  
D) The failed records are written to a DLQ

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

When Glue Data Quality rules fail, the Glue job can be configured to fail (the default for `FAIL_JOB` action). The job run status becomes FAILED, which prevents downstream Step Functions or EventBridge triggers from firing.

- A (retry) — retries wouldn't produce more records; the root cause needs investigation.
- B (warning only) — the behavior depends on the configured action (`FAIL_JOB` vs `NONE`).
- D (DLQ for failed records) — DLQ is for event-driven processing, not data quality rule failures.
</details>

---

**Q7.** A company runs EMR on EC2 with core nodes on On-Demand instances and task nodes on Spot Instances. A Spot Interruption terminates several task nodes. What is the impact?

A) The entire EMR cluster terminates  
B) HDFS data on the terminated nodes is lost permanently  
C) In-progress Spark tasks on terminated nodes are retried on remaining nodes  
D) The cluster pauses until Spot capacity is restored

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Task nodes do not store HDFS data. When Spark tasks running on task nodes are interrupted, Spark's fault tolerance mechanisms automatically retry the failed tasks on other available nodes (core or remaining task nodes).

- A (cluster terminates) — only task nodes are affected; the master and core nodes continue running.
- B (HDFS data lost) — task nodes don't store HDFS data. Only core nodes hold HDFS data.
- D (cluster pauses) — Spark continues with available resources; failed tasks are rescheduled.
</details>

---

**Q8.** A data engineer needs to automatically start a Step Functions pipeline whenever a new Parquet file lands in an S3 prefix. What is the MOST event-driven approach with the least operational overhead?

A) Poll S3 with a Lambda function on a 1-minute schedule  
B) Configure S3 Event Notification to publish to EventBridge, then route to Step Functions  
C) Use a Glue Crawler to detect new files and trigger Step Functions via Lambda  
D) Configure DMS to monitor S3 for new files

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

S3 Event Notifications publish events to EventBridge (or SNS/SQS/Lambda) immediately when an object is created. EventBridge rules can directly trigger Step Functions executions — no polling, no delay, no additional Lambda needed.

- A (scheduled poll) introduces up to 1-minute latency and is not truly event-driven.
- C (Glue Crawler) is designed for schema discovery, not pipeline triggering — adds unnecessary complexity.
- D (DMS) is for database migration, not S3 file monitoring.
</details>

---

**Q9.** A company uses QuickSight with SPICE for executive dashboards. The SPICE dataset is refreshed daily at midnight. Executives complain that dashboards show yesterday's data in the morning. What should the data engineer do?

A) Migrate dashboards to Direct Query mode  
B) Increase the SPICE refresh frequency to hourly  
C) Increase SPICE storage capacity  
D) Migrate data from Athena to Redshift

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Increasing the SPICE refresh schedule from daily to hourly ensures dashboards show data no more than 1 hour old. This balances freshness with cost (each refresh queries Athena/Redshift).

- A (Direct Query) solves freshness but every dashboard load runs a live query — higher latency and more load on the underlying source.
- C (more SPICE capacity) doesn't affect refresh frequency.
- D (Redshift) doesn't solve the freshness problem — the SPICE refresh schedule is the issue.
</details>

---

**Q10.** A Kinesis Data Streams producer receives `ProvisionedThroughputExceededException` errors during peak hours. The stream has 4 shards. What is the BEST resolution?

A) Enable Enhanced Fan-Out for the consumer  
B) Split shards to increase the stream's write capacity  
C) Switch from Kinesis Data Streams to Kinesis Firehose  
D) Add exponential backoff to the producer

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

`ProvisionedThroughputExceededException` on writes means the stream is at write capacity (1 MB/s or 1,000 records/s per shard). Splitting shards increases total capacity. 4 shards → 8 shards doubles write capacity.

- A (Enhanced Fan-Out) is a read-side optimization — doesn't affect write throughput.
- C (switch to Firehose) solves delivery to S3 but Firehose has different ingestion limits; it doesn't increase streaming capacity.
- D (exponential backoff) reduces immediate pressure but doesn't increase capacity — records will continue failing at peak; it's a client-side mitigation, not a fix.
</details>

---

**Q11.** A data engineer wants to detect anomalies in daily record counts automatically. Every day the pipeline processes around 1 million records, but occasionally drops to 100,000 (indicating an upstream issue). What is the BEST AWS-native solution?

A) Schedule a Lambda to check record count daily and compare to a hard-coded threshold  
B) Use CloudWatch Anomaly Detection on a custom metric for daily record count  
C) Configure Glue Data Quality rule: `RowCount > 900000`  
D) Create a CloudWatch Alarm with threshold = 1,000,000

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

CloudWatch Anomaly Detection uses ML to learn the normal pattern of a metric (including seasonality and trends) and alerts when the metric deviates significantly from the expected band. Unlike a hard-coded threshold, it adapts as volumes naturally grow or follow weekly patterns.

- A (hard-coded threshold in Lambda) works but requires manual maintenance as volumes change.
- C (Glue Data Quality rule) is valid but only runs at job execution time, not as ongoing monitoring.
- D (CloudWatch Alarm with hard threshold) doesn't adapt to growing data volumes — will generate false positives or miss anomalies.
</details>

---

**Q12.** A data engineer is optimizing Glue ETL job costs. The job reads 500 GB of CSV files from S3, transforms them, and writes Parquet to S3. Only 3 of the 50 columns are needed in the output. What optimizations should be applied?

A) Use more DPUs for faster processing  
B) Apply pushdown predicates and column projection in the Glue data source  
C) Convert input CSV to Parquet before running the Glue job  
D) Use Glue DataBrew instead of Glue ETL

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Column projection (selecting only the 3 needed columns at the source) reduces data transferred from S3 to Glue workers. Pushdown predicates (WHERE clause at the source) further reduces rows read. Both reduce DPU-hours by reducing the data the job processes.

- A (more DPUs) processes faster but doesn't reduce cost per record.
- C (pre-convert to Parquet) would help if the CSVs were reused, but adding a separate conversion job adds its own cost.
- D (DataBrew) is a no-code tool for one-time transformations — not optimized for recurring large-scale ETL.
</details>

---

**Q13.** A Step Functions state machine orchestrates a 5-step data pipeline. Step 3 (a Glue job) fails intermittently due to a transient API error. How should the state machine be configured to handle this?

A) Add a `Wait` state of 60 seconds before Step 3  
B) Add a `Retry` configuration on the Glue job Task state with MaxAttempts=3 and exponential backoff  
C) Wrap Step 3 in a `Parallel` state with a duplicate branch  
D) Add a `Choice` state after Step 3 to check for failure

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Step Functions `Retry` configuration handles transient failures automatically. With `MaxAttempts=3` and `BackoffRate=2`, the state machine retries the Glue task 3 times with exponential backoff (e.g., 1s, 2s, 4s) before marking as failed.

- A (Wait before step) delays execution but doesn't handle failures that occur during execution.
- C (Parallel with duplicate) would run two Glue jobs simultaneously — doubles cost and doesn't handle failures.
- D (Choice after failure) can route to error handling but doesn't automatically retry the failing step.
</details>

---

**Q14.** An EMR cluster running Spark jobs uses HDFS as its storage layer. The team notices that data is lost when the cluster terminates at the end of a job. What should they change?

A) Use larger instance types to prevent OOM errors  
B) Store intermediate and final data in Amazon S3 using EMRFS, not HDFS  
C) Enable EMR auto-scaling to prevent cluster termination  
D) Use Amazon EFS instead of HDFS

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

HDFS is ephemeral — data is lost when the cluster terminates. EMRFS (EMR File System) uses S3 as the storage backend. Data written to S3 persists after cluster termination. This enables the "transient cluster" pattern: terminate after each job, restart fresh next time.

- A (larger instances) doesn't prevent data loss on termination.
- C (auto-scaling) keeps the cluster running longer but data is still lost when it eventually terminates.
- D (EFS) is a persistent network file system but is much slower than EMRFS for Spark-on-S3 access patterns.
</details>

---

**Q15.** A company wants to reduce Athena query costs by 70% without changing query logic. Queries currently scan 10 TB/day. What combination of changes achieves the GREATEST cost reduction?

A) Switch from S3 Standard to S3 Intelligent-Tiering  
B) Convert CSV to Parquet with Snappy compression AND partition data by date  
C) Enable Athena result caching  
D) Upgrade to Athena v3 engine

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Parquet columnar format + Snappy compression typically reduces data scanned by 80-95% for analytical queries (10x compression + only reading needed columns). Partitioning by date further limits scans to relevant time ranges. Combined, this can reduce a 10 TB scan to under 1 TB — well over 70% reduction.

- A (S3 storage class) affects storage cost, not Athena scan cost.
- C (result caching) only helps for exact duplicate queries — doesn't help for slightly different queries.
- D (Athena engine version) improves performance but doesn't directly reduce data scanned.
</details>

---

**Q16.** A data engineer builds a pipeline to detect fraud patterns in real-time. Events arrive in Kinesis Data Streams. The pipeline needs to: count distinct users per 5-minute window, alert if any user exceeds 50 transactions in 5 minutes, and maintain this count across record boundaries. Which service is BEST suited?

A) AWS Lambda with Kinesis event source mapping  
B) Amazon Managed Service for Apache Flink  
C) Amazon Kinesis Data Firehose with Lambda transformation  
D) Amazon EMR with Spark Streaming

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Managed Apache Flink handles stateful windowed aggregations natively. A 5-minute tumbling window that maintains per-user counts across records requires state management — Flink's core capability. It processes records in real-time, maintains state with fault-tolerant checkpointing.

- A (Lambda with Kinesis) can do per-record processing but cannot natively maintain windowed aggregations across invocations without external state storage (DynamoDB) — complex and limited.
- C (Firehose + Lambda) buffers records (60-900s) — too slow for real-time fraud detection with 5-minute windows.
- D (EMR Spark Streaming) is valid but requires managing an EMR cluster; Managed Flink is serverless.
</details>

---

**Q17.** A data engineer notices that CloudTrail shows hundreds of `GetObject` calls on an S3 bucket containing PII by an IAM role that shouldn't have access. What service would detect this pattern AUTOMATICALLY and alert the security team?

A) Amazon Macie  
B) AWS Config  
C) Amazon GuardDuty  
D) AWS Security Hub

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

GuardDuty analyzes CloudTrail, VPC Flow Logs, and DNS logs using ML to detect anomalous and suspicious activity. Unusual S3 access patterns (e.g., a role accessing PII data it doesn't normally access) trigger GuardDuty findings like `Policy:S3/BucketBlockPublicAccessDisabled` or behavioral anomaly findings.

- A (Macie) scans S3 objects for PII content — it finds where PII is stored, not anomalous access patterns.
- B (Config) evaluates resource configuration compliance — not behavioral access patterns.
- D (Security Hub) aggregates findings from GuardDuty, Macie, and others but doesn't itself detect the access pattern.
</details>

---

**Q18.** A company runs a batch data pipeline once a day at midnight. The pipeline reads from S3, runs through Glue, and loads to Redshift. Between runs, the Redshift cluster is idle but they pay for it 24/7. What is the BEST cost optimization?

A) Use Reserved Instances for the Redshift cluster  
B) Pause the Redshift cluster when not in use, or migrate to Redshift Serverless  
C) Reduce the cluster to the smallest available node type  
D) Move Redshift to a smaller AWS Region

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Pausing a Redshift cluster stops billing for compute (you still pay for storage). Redshift Serverless scales to zero when idle and only charges for compute seconds during active queries. Both options eliminate idle cluster costs.

- A (Reserved Instances) reduces per-hour cost but you still pay for all 24 hours even when idle.
- C (smallest node type) reduces cost but cluster must still run 24/7 for the midnight pipeline.
- D (different region) doesn't address idle cost and may increase data transfer costs.
</details>
