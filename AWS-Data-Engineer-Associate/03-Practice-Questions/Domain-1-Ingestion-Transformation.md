# Domain 1: Data Ingestion and Transformation — Practice Questions

> 25 questions | ~34% of the DEA-C01 exam

---

**Q1.** A company needs to ingest 500,000 clickstream events per second from their website and make them available for real-time fraud detection within 200 milliseconds. Which service should they use?

A) Amazon Kinesis Data Firehose  
B) Amazon Kinesis Data Streams  
C) Amazon SQS  
D) AWS Glue Streaming

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Kinesis Data Streams is designed for real-time streaming with millisecond latency. A custom consumer application can process records with sub-second latency.

- A (Firehose) buffers data before delivery (60-900 seconds) — too slow for 200ms fraud detection.
- C (SQS) is a message queue, not a streaming service optimized for this throughput.
- D (Glue Streaming) processes continuous streams but adds latency — not suitable for 200ms requirements.
</details>

---

**Q2.** A data engineer is running daily Glue ETL jobs that process all S3 data every time, even though only new files are added each day. What should be done to fix this?

A) Increase the number of DPUs  
B) Enable Glue Job Bookmarks  
C) Convert data to Parquet format  
D) Add pushdown predicates

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Glue Job Bookmarks track which data (files, database rows) have already been processed. When a job runs again, it only processes new/changed data since the last successful run — skipping already-processed data.

- A increases compute power but still processes all data.
- C improves query performance but doesn't solve the reprocessing problem.
- D reduces data read from S3 but requires explicit filters — Bookmarks are the purpose-built solution.
</details>

---

**Q3.** An organization receives JSON files from multiple partners in different schemas daily. They want to automatically discover the schema of each new file and make it queryable in Athena without manual intervention. What is the BEST approach?

A) Write a Lambda function to parse each file and create Athena tables  
B) Configure AWS Glue Crawlers to run on the S3 bucket and update the Glue Data Catalog  
C) Use Amazon Macie to scan and catalog the files  
D) Load all files into Redshift and query from there

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Glue Crawlers automatically scan data in S3, infer schemas, and create/update tables in the Glue Data Catalog. Athena uses the Data Catalog — once the Crawler runs, the tables are immediately queryable in Athena.

- A works but requires custom maintenance code.
- C (Macie) is for PII detection, not schema discovery.
- D adds complexity and cost — Athena queries S3 directly without loading.
</details>

---

**Q4.** A streaming data pipeline using Kinesis Data Streams shows that consumer processing is falling behind. Which CloudWatch metric indicates this problem?

A) `WriteProvisionedThroughputExceeded`  
B) `GetRecords.IteratorAgeMilliseconds`  
C) `IncomingRecords`  
D) `PutRecord.Success`

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

`GetRecords.IteratorAgeMilliseconds` measures the age of the last record retrieved — how far behind the consumer is from the tip of the stream. If this metric grows, the consumer is not keeping up with the data being ingested.

- A (`WriteProvisionedThroughputExceeded`) indicates the producer is exceeding shard write capacity — a producer problem, not consumer lag.
- C measures incoming volume, not processing lag.
- D measures put success rate, not consumer lag.
</details>

---

**Q5.** A company wants to continuously deliver IoT sensor data to Amazon S3 in Parquet format for later analysis with Athena. They don't want to write consumer code. Which service should they use?

A) Amazon Kinesis Data Streams with a custom Lambda consumer  
B) Amazon Kinesis Data Firehose with format conversion enabled  
C) Amazon MSK with a custom Kafka consumer  
D) AWS Glue Streaming job

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Kinesis Data Firehose is a fully managed delivery service that requires no consumer code. It can automatically convert incoming records to Parquet or ORC using a Glue Data Catalog schema before writing to S3.

Key combination: Firehose + "no consumer code" + Parquet conversion = the answer.
</details>

---

**Q6.** A data engineer needs to migrate a 50 TB Oracle database to Amazon Aurora PostgreSQL with less than 2 hours of downtime during the final cutover. Which approach should they use?

A) Export from Oracle to CSV, upload to S3, load into Aurora using COPY  
B) Use AWS DMS to perform a full load, then enable CDC replication, and cut over during maintenance  
C) Use AWS DataSync to transfer the database files directly  
D) Use Snowball Edge to physically transfer the data

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS DMS with full load + CDC (Change Data Capture) is the standard approach for database migrations with minimal downtime:
1. Full load: migrate all existing data (takes hours/days, application keeps running)
2. CDC: continuously replicate changes from Oracle to Aurora as they happen
3. Cutover: pause the application briefly, let CDC catch up (seconds/minutes), switch to Aurora

- A doesn't handle ongoing changes during migration.
- C (DataSync) moves files, not relational database data.
- D (Snowball) can't replicate ongoing database changes.
</details>

---

**Q7.** An Athena query scanning 5 TB of CSV data costs $25. A data engineer proposes converting the data to Parquet with Snappy compression. If the conversion achieves 10:1 compression and Parquet reduces scanned data by 90% for typical queries, what would the new approximate cost per query?

A) $2.50  
B) $0.25  
C) $1.25  
D) $5.00

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Original cost: $25 (scanning 5 TB × $5/TB)

With Parquet + Snappy:
- Columnar format: query reads ~10% of columns = 0.5 TB
- Snappy compression: further reduces to ~0.05 TB
- Cost: 0.05 TB × $5/TB = **$0.25**

This demonstrates the dramatic savings from format optimization. In practice, results vary but 90-99% cost reductions are common.
</details>

---

**Q8.** A company processes data from multiple source systems and wants to ensure each ETL step can be safely re-run without duplicating records in the output. What design principle should they apply?

A) Parallelism  
B) Idempotency  
C) Immutability  
D) Normalization

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Idempotency means a pipeline step can be run multiple times with the same result — running it once is the same as running it ten times. Essential for safe retries and error recovery.

Practical implementations: UPSERT instead of INSERT, overwrite S3 partitions instead of append, deduplicate on a key.

- A (Parallelism) is about throughput.
- C (Immutability) means data doesn't change once written.
- D (Normalization) is database design.
</details>

---

**Q9.** A Glue ETL job reads a full year of data from S3 but only processes 1 day at a time. The job is slow and scans too much data. The S3 data is partitioned by `year/month/day`. How should you optimize it?

A) Enable Job Bookmarks  
B) Use pushdown predicates to filter at the S3 layer before reading  
C) Convert data to CSV format  
D) Increase DPU count

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Pushdown predicates tell Glue to read ONLY the partitions that match the filter — instead of reading all data into Spark and then filtering. For a day-level job processing `year=2024/month=01/day=15` data, add: `push_down_predicate="year=2024 AND month=1 AND day=15"`.

- A (Job Bookmarks) track incremental processing but don't optimize read volume per run.
- C makes things worse.
- D adds compute but still reads all the data.
</details>

---

**Q10.** Which of the following is a correct description of Apache Kafka's relationship with Amazon MSK?

A) MSK is a proprietary alternative to Kafka with an incompatible API  
B) MSK is fully managed Apache Kafka — your Kafka producers and consumers work without modification  
C) MSK only supports Kafka producers; consumers must use Kinesis  
D) MSK automatically converts Kafka messages to Kinesis format

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Amazon MSK (Managed Streaming for Apache Kafka) runs real Apache Kafka. It's 100% API-compatible — if you have existing Kafka producers and consumers, they work with MSK without code changes. AWS manages the cluster (brokers, ZooKeeper, patching, scaling).

Choose MSK when: existing Kafka ecosystem, open-source portability, Kafka-specific features needed.
Choose Kinesis when: new AWS-native project, want fully serverless, simpler operations.
</details>

---

**Q11.** A data engineer needs to configure S3 storage to minimize Athena query costs for a data set where queries almost always filter by `country` and `year`. How should the data be partitioned?

A) By `event_id` (unique per record)  
B) By `country` and `year`  
C) By file upload timestamp  
D) No partitioning is needed for Parquet files

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Partition by the columns you filter on most frequently. If queries almost always include `WHERE country='ZA' AND year=2024`, partitioning by `country` and `year` means Athena only reads the relevant subdirectory.

- A: partitioning by a high-cardinality unique key creates millions of tiny files — terrible.
- C: file upload time isn't used in queries.
- D: Parquet reduces column scans but without partitioning you still scan all rows.
</details>

---

**Q12.** What is the primary advantage of using the Kinesis Enhanced Fan-Out feature?

A) Increases the maximum record size from 1 MB to 10 MB  
B) Provides each registered consumer with a dedicated 2 MB/s read throughput per shard  
C) Automatically scales shards based on incoming traffic  
D) Reduces data retention costs

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Standard consumers SHARE 2 MB/s read capacity per shard across all consumers. Enhanced Fan-Out gives EACH registered consumer its own dedicated 2 MB/s per shard — no competition. Critical when multiple teams' applications all need to process the same stream independently at full speed.

Enhanced Fan-Out also uses HTTP/2 push (vs. polling) for lower latency (~70ms vs ~200ms).
</details>

---

**Q13.** An organization has 3 PB of on-premises NAS data to migrate to Amazon S3. Their internet connection is 1 Gbps shared. Approximately how many days would a full internet transfer take, and what is the better alternative?

A) ~3 days; use Direct Connect  
B) ~27 days; use AWS Snowball Edge  
C) ~7 days; use AWS DataSync  
D) ~270 days; use multiple Snowball Edge devices

<details>
<summary>Answer & Explanation</summary>

**Answer: D**

Calculation: 3 PB = 3,000 TB. At 1 Gbps = 125 MB/s = ~10 TB/day.  
3,000 TB / 10 TB/day = **300 days** (ballpark ~270 days at sustained transfer).

For petabyte-scale migrations, AWS recommends multiple Snowball Edge devices (80 TB usable each) — you'd need ~38 devices. Ship them in batches.

For true exabyte migrations, Snowmobile (truck-mounted container, up to 100 PB) would be used.
</details>

---

**Q14.** A company stores event data as CSV files in S3. They want to reduce Athena query costs. A data engineer proposes converting to Parquet. Which AWS service is BEST suited to perform this format conversion at scale?

A) AWS Lambda  
B) Amazon EC2 with a custom script  
C) AWS Glue ETL job  
D) Amazon Kinesis Data Firehose

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Glue ETL is the managed, serverless Spark service purpose-built for this. A Glue job can read CSV from S3, convert to Parquet (with partitioning), and write back to S3 — no cluster management needed.

- A (Lambda) has a 15-minute timeout and memory limits — can't handle large files.
- B (EC2) works but requires cluster management.
- D (Firehose) converts incoming streams — not for converting existing historical S3 data.
</details>

---

**Q15.** In AWS Step Functions, what state type would you use to execute both a Glue ETL job AND a DMS task simultaneously to reduce total pipeline runtime?

A) Choice  
B) Task  
C) Parallel  
D) Map

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The **Parallel** state runs multiple branches of work simultaneously. If the Glue job and DMS task are independent, running them in parallel reduces total pipeline duration from (Glue time + DMS time) to max(Glue time, DMS time).

- A (Choice) is branching logic (if/else).
- B (Task) is a single unit of work.
- D (Map) processes each item in an array — not for running different tasks in parallel.
</details>

---

**Q16.** A data engineer needs to continuously load streaming data from Kinesis Firehose to Amazon Redshift. The data arrives as JSON and needs to be in a Redshift-compatible format. What is the recommended approach?

A) Use Lambda to transform JSON to CSV before Firehose delivers to Redshift  
B) Configure Firehose to convert JSON to Parquet using a Glue Data Catalog schema, stage in S3, then use the Firehose Redshift destination  
C) Write a custom consumer that reads from Kinesis Streams and does INSERT INTO Redshift  
D) Use Glue Streaming to transform and load every 5 minutes

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Kinesis Firehose has a built-in Redshift destination: it stages data in S3, then automatically issues a Redshift COPY command. You can also configure Firehose to convert JSON to Parquet/ORC using the Glue Data Catalog schema. This is the fully managed, no-code solution.

- C (custom INSERT) is extremely slow for streaming volumes.
- D (Glue Streaming) works but adds complexity over the built-in Firehose feature.
</details>

---

**Q17.** What is the key difference between AWS Glue Python Shell jobs and Glue Spark jobs?

A) Python Shell jobs use Spark; Spark jobs use Pandas  
B) Python Shell jobs run on a single node and are suitable for small data; Spark jobs run on a distributed cluster and are suitable for large data  
C) Python Shell jobs are free; Spark jobs are charged per DPU  
D) Python Shell jobs can read from S3; Spark jobs can only read from JDBC sources

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

- **Python Shell**: single-node Python environment. Good for small data processing, API calls, simple transformations. Limited to 1 or 16 DPU.
- **Spark**: distributed cluster (driver + multiple executors). Handles TB of data. More powerful, higher cost.

Choose Python Shell when your data fits comfortably in memory on one machine. Choose Spark for everything else.
</details>

---

**Q18.** A company needs to build a data pipeline that handles occasional schema changes in source data gracefully — for example, when the source adds new columns. Which Glue feature handles this?

A) Glue Job Bookmarks  
B) DynamicFrame's `resolveChoice()` and handling of schema flexibility  
C) Glue Crawler version management  
D) Glue Data Quality rules

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

AWS Glue DynamicFrames are designed for schema flexibility. Unlike Spark DataFrames which enforce a strict schema, DynamicFrames can handle:
- Missing fields
- Type conflicts (same column name with different types in different records)
- New unexpected columns

The `resolveChoice()` method handles ambiguous types; DynamicFrames don't fail when encountering new/unexpected fields.
</details>

---

**Q19.** A data pipeline using Amazon MWAA (Apache Airflow) fails with a task error halfway through. What does Airflow do by default?

A) Restarts the entire DAG from the beginning  
B) Marks the failed task as failed and stops the DAG run, unless a retry is configured  
C) Automatically skips the failed task and continues downstream  
D) Sends a CloudWatch alarm and waits for manual intervention

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

In Airflow, a failed task marks the DAG run as failed by default. Downstream tasks that depend on the failed task won't run. You can configure:
- `retries`: automatically retry N times
- `retry_delay`: wait between retries
- `on_failure_callback`: custom Python function on failure (e.g., send Slack alert)

The key point: Airflow respects task dependencies — only the failed task and its downstream tasks are affected.
</details>

---

**Q20.** What is the purpose of S3 partitioning in a data lake architecture, specifically for query performance?

A) It compresses the data to reduce storage costs  
B) It converts the data to columnar format  
C) It allows query engines to skip scanning irrelevant files by filtering on partition columns  
D) It replicates data across multiple Availability Zones

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

Partitioning organizes data into S3 prefixes by column values (`/year=2024/month=01/`). When a query has `WHERE year=2024 AND month=01`, Athena/Glue/Spectrum reads ONLY that S3 prefix — not the entire dataset. This is called **partition pruning**.

- A is compression (separate feature).
- B is columnar format (Parquet/ORC — separate from partitioning).
- D is S3 replication (CRR/SRR — for DR, not query performance).
</details>

---

**Q21.** A company ingests data from 1,000 IoT devices. Each device sends events identified by its device ID. They need to ensure that events from the same device are always processed in the correct order. How should the Kinesis partition key be configured?

A) Use a random UUID as the partition key  
B) Use the device ID as the partition key  
C) Use the event timestamp as the partition key  
D) Use the event type as the partition key

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Kinesis guarantees ordering WITHIN a shard. Records with the same partition key always go to the same shard, ensuring they're processed in order.

Using `device_id` as the partition key ensures all events from device `ABC123` go to the same shard → processed in sequence.

Using random UUIDs would distribute records randomly — no ordering guarantee for a specific device.
</details>

---

**Q22.** A data engineer is configuring Kinesis Data Firehose to deliver data to S3. They want to avoid tiny files that hurt Athena performance. What Firehose settings control file size?

A) Shard count and partition key  
B) Buffer size (MB) and buffer interval (seconds)  
C) Compression algorithm and encryption type  
D) IAM role permissions and S3 bucket policy

<details>
<summary>Answer & Explanation</summary>

**Answer: B**

Firehose buffers records before writing to S3. You configure:
- **Buffer size**: delivers when accumulated data reaches X MB (1–128 MB)
- **Buffer interval**: delivers after X seconds even if buffer isn't full (60–900 seconds)

Whichever threshold is hit first triggers delivery. Larger buffers = larger files = better for Athena. Balance with acceptable latency.
</details>

---

**Q23.** Which data transformation tool is best suited for a data analyst (non-engineer) who needs to clean and normalize a CSV dataset but has no Python or Spark knowledge?

A) AWS Glue ETL Spark job  
B) Amazon EMR with Hadoop  
C) AWS Glue DataBrew  
D) AWS Lambda

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

AWS Glue DataBrew is a visual, no-code data preparation tool. Users can:
- Profile data (see column statistics, null counts, distributions)
- Apply 250+ built-in transformations through a UI
- Build reusable "recipes" without writing code
- Automatically detect and handle PII

Designed exactly for non-technical users who need to clean data.
</details>

---

**Q24.** A company needs to process 10 billion records daily with complex multi-table joins and custom Python ML transformations. The team has deep Apache Spark expertise. Which service should they use?

A) AWS Lambda  
B) Amazon Kinesis Data Analytics  
C) Amazon EMR with Apache Spark  
D) AWS Glue Python Shell

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

For complex Spark workloads requiring maximum control, custom Python/Scala libraries, and performance tuning, Amazon EMR is the answer. The team can configure the cluster, install custom dependencies, tune Spark settings, and use advanced Spark features not available in managed Glue.

- A (Lambda) has 15min limit and can't handle 10B records.
- B (Kinesis Analytics) is for real-time stream SQL, not batch ML.
- D (Glue Python Shell) is single-node — can't handle 10B records at scale.
</details>

---

**Q25.** In the Medallion architecture (Bronze/Silver/Gold), what is stored in the Bronze layer?

A) Cleaned, validated data in Parquet format ready for analytics  
B) Business-ready aggregated data optimized for BI tools  
C) Raw, unmodified copies of source data exactly as ingested  
D) Normalized relational data with foreign key constraints

<details>
<summary>Answer & Explanation</summary>

**Answer: C**

The Bronze layer is the raw data landing zone — exact copies of source data, nothing changed. This ensures you always have the original data available for reprocessing if downstream transformations had bugs.

- Silver = cleaned, validated, schema-enforced data
- Gold = business-ready aggregates and curated datasets for analytics/ML

The Bronze layer is immutable — you never modify data there, only append.
</details>
