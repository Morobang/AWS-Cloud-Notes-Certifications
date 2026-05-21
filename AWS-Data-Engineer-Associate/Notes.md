# AWS Data Engineer Associate — Personal Notes

> The stuff that finally clicked. Built on top of the Cloud Practitioner foundation.

---

## The mental shift from CCP to DEA

The Cloud Practitioner asks "do you know what S3 is?"  
The Data Engineer asks "given 500 TB of JSON data in S3 being queried daily by 50 analysts, how do you cut Athena costs by 80% while ensuring analysts can't see PII columns?"

The DEA is about **implementation decisions and trade-offs**, not just recognition.

---

## The thing that unlocks Domain 1: think in terms of latency

Every ingestion question comes down to one question: **how fresh does the data need to be?**

- **Seconds / milliseconds** → Kinesis Data Streams → real-time consumers
- **Minutes** → Kinesis Firehose (buffer time) → near-real-time delivery to S3
- **Hours** → Scheduled Glue job / batch
- **Days / offline** → Snow Family

If you internalize that hierarchy, you can answer most Domain 1 service selection questions.

---

## Domain 1 mental models

### Kinesis Streams vs Firehose — the analogy that stuck

**Kinesis Streams** = a river. Water flows in continuously. You can have multiple boats (consumers) on the river reading from different points. You're responsible for each boat and what you do with the water.

**Kinesis Firehose** = a municipal water treatment plant. Water comes in, gets filtered (optional Lambda transform), and automatically delivered to your house (S3/Redshift/OpenSearch). You don't drive the boat — the plumbing does it for you.

The exam loves this question: "need to deliver clickstream data to S3 without writing consumer code" → Firehose.

### The shard limit I always forget

1 shard = 1 MB/s write. If you have 100 MB/s of incoming data, you need at LEAST 100 shards.  
`ProvisionedThroughputExceededException` = you ran out of shard capacity. Add shards.

### Glue Job Bookmark

This is the one Glue feature that's on almost every practice exam. Job Bookmark = "process new data only, skip what I've already done."

Without it: your daily Glue job reprocesses all historical data every day — costs multiply.  
With it: Glue tracks the last processed checkpoint and only touches new files/rows.

Turn it OFF when you deliberately want to reprocess historical data (like when fixing a bug).

### Why Parquet matters — the math

Say you have 100 TB of CSV data and query 5 out of 100 columns:
- CSV: Athena reads 100 TB → $500/query
- Parquet (columnar): Athena reads ~5 TB → $25/query

That's a 20x cost reduction from just changing the file format. Add partitioning (only scan relevant dates) and you get another 10-100x. This is the highest-ROI optimization in data engineering.

---

## Domain 2 mental models

### Redshift distribution keys — the one thing to get right

Wrong distribution = massive performance problems. The goal is to **collocate data that gets joined together** on the same node, so the join happens locally without network transfers.

```
big fact table   → KEY distribution on customer_id
big fact table   → KEY distribution on customer_id
→ Both distributed on customer_id = rows land on same node = fast join, no network

dimension table  → ALL distribution (copy to every node)
→ Every node has the full dimension table = any join is local
```

I failed a practice question on this until I drew it out. The key insight: when two huge tables join on the same column and both are KEY-distributed on that column, the matching rows are guaranteed to be on the same node.

### The data lake vs warehouse confusion

I kept confusing when to use what. Here's how I now think about it:

**Data lake (S3 + Athena)**: "I want to keep EVERYTHING cheaply and query it occasionally." Raw logs, historical data, ML training data, unstructured documents. You pay per query.

**Data warehouse (Redshift)**: "I want to run FAST, complex SQL queries repeatedly." BI dashboards, monthly reports, queries that run hundreds of times a day. You pay for the cluster running 24/7.

**Lakehouse**: Keep cold/large data in S3, hot/frequently-queried data in Redshift. Use Redshift Spectrum to join across both. Best of both worlds.

### Lake Formation — finally understood it

For months I couldn't explain Lake Formation clearly. Then it clicked:

Lake Formation is not a new storage system. It's a **permission management layer** on top of existing Glue Data Catalog. Instead of writing S3 bucket policies (coarse-grained, bucket-level), you write Lake Formation permissions (fine-grained, column-level).

The value: one permission store → controls what Athena, Glue, EMR, and Redshift Spectrum can all see. Change it once, it affects all query engines.

---

## Domain 3 mental models

### Athena optimization — the order that matters

When optimizing slow/expensive Athena queries:
1. **First**: Is the data in columnar format (Parquet/ORC)? If not, convert first — biggest win.
2. **Second**: Is the data partitioned by the columns in your WHERE clause? If not, add partitions.
3. **Third**: Is your compression efficient? Add Snappy or ZSTD.
4. **Fourth**: Are files too small (< 128 MB)? Compact with Glue.
5. **Fifth**: Too many partitions to list? Enable Partition Projection.

### The CloudWatch metric I always need for Kinesis troubleshooting

`GetRecords.IteratorAgeMilliseconds` — measures how old the oldest unprocessed record is.

If this is 0 → consumer is keeping up perfectly.  
If this is growing → consumer is falling behind. Either optimize the consumer or add more shards.

This is the first metric I check when a Kinesis pipeline is in trouble.

---

## Domain 4 mental models

### SSE-KMS vs SSE-S3 — why it matters for compliance

SSE-S3: AWS manages the key. Simple, no cost for KMS calls, but you can't audit key usage.

SSE-KMS: You manage the key in KMS. Every use of the key appears in CloudTrail. You can disable the key (data becomes inaccessible). You can restrict which services/roles can use the key.

For compliance: **Always SSE-KMS**. When an auditor asks "who accessed the encryption key for this data?" — CloudTrail has the answer. With SSE-S3, you can't answer that question.

### The Lake Formation permission hierarchy

Both IAM AND Lake Formation must allow access. This tripped me up in practice questions.

If a user's IAM policy allows `s3:GetObject` on a bucket, but Lake Formation denies SELECT on that table → **access is denied**. Both gates must pass.

The practical implication: if a user can't access a table even though their IAM looks right, check Lake Formation permissions too.

---

## Things I got wrong in DEA practice tests

**Q: What's the fastest way to load 5 TB into Redshift?**  
A: COPY command from S3. NOT individual INSERT statements. COPY is parallel and uses all compute nodes simultaneously. Single INSERTs are 1,000x slower.

**Q: A Glue job is reprocessing all historical data every time it runs. How do you fix it?**  
A: Enable Job Bookmarks. Without bookmarks, Glue doesn't know what it's already processed.

**Q: What format should you use to store streaming IoT data for Athena queries?**  
A: Parquet (via Firehose → S3 with format conversion enabled). Raw JSON is fine for ingestion but expensive to query.

**Q: A company needs to give analysts access to customer data but hide SSN and DOB columns. What service?**  
A: AWS Lake Formation with column-level security. Not just IAM — IAM can't restrict at the column level.

**Q: EMR cluster terminates and all data is lost. How do you prevent this?**  
A: Store data in S3 (EMRFS), not HDFS. HDFS is local to the cluster — when the cluster terminates, it's gone. EMRFS persists in S3 independently.

---

## The DEA in 5 bullet points

1. **Kinesis** = streaming. Streams for custom consumers. Firehose for managed delivery. MSK if you're already Kafka.
2. **Glue** = serverless ETL. Data Catalog = schema store. Crawlers = auto-schema. Job Bookmark = incremental.
3. **Parquet + partitioning** = Athena cost optimization. This will be on the exam multiple times.
4. **Redshift** = analytics/BI. COPY = load data. Spectrum = query S3. Distribution key = performance.
5. **Lake Formation** = fine-grained permissions (column/row) for the data lake. Both IAM AND LF must allow access.
