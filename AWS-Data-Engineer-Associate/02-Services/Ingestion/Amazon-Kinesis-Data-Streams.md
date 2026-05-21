# Amazon Kinesis Data Streams

**Category**: Real-time data streaming  
**Exam weight**: High — appears in many Domain 1 scenarios

---

## What Is It?

A massively scalable, durable real-time data streaming service. Producers write records to the stream; multiple consumers can read from it in parallel.

Think of it as a massive, ordered, durable log of events that multiple applications can read simultaneously.

---

## Core Concepts

| Concept | Definition |
|---|---|
| **Stream** | The collection of data — made up of one or more shards |
| **Shard** | Basic unit of capacity. Each shard handles specific throughput |
| **Record** | A unit of data: partition key + sequence number + data blob |
| **Partition key** | Determines which shard a record goes to. Same key → same shard → ordered |
| **Sequence number** | Auto-assigned by AWS, unique per shard, strictly ordered |
| **Retention period** | How long records stay in the stream (24h default, up to 365 days) |
| **Producer** | Writes records to the stream |
| **Consumer** | Reads records from the stream |

---

## Shard Capacity

**Per shard limits:**
- Write: **1 MB/s** or **1,000 records/s** (whichever hits first)
- Read: **2 MB/s** (shared across all consumers) OR 2 MB/s **per consumer** with Enhanced Fan-Out
- Max record size: **1 MB**

**If you exceed shard capacity:**
- `ProvisionedThroughputExceededException` error
- Fix: add more shards (reshard) or reduce record rate/size

---

## Consumers

### Standard Consumer (Polling)
- Consumers poll `GetRecords` API
- 2 MB/s read throughput SHARED across all consumers on a shard
- 5 `GetRecords` API calls per shard per second

### Enhanced Fan-Out Consumer
- Uses `SubscribeToShard` API — push model, no polling
- **2 MB/s per consumer per shard** — dedicated bandwidth
- Lower latency (~70ms vs ~200ms)
- Additional cost
- Use when: multiple consumer applications OR need lowest latency

### Consumer services:
- AWS Lambda
- Amazon Kinesis Data Analytics (real-time SQL/Flink)
- Amazon Kinesis Data Firehose
- AWS Glue Streaming
- Custom applications (Kinesis Client Library / KCL)

---

## Ordering and Partition Keys

Records within a shard are **strictly ordered** (sequence number). Across shards, there's no ordering guarantee.

**Design consideration**: If you need all events for a customer to be processed in order, use `customer_id` as the partition key — all events for that customer go to the same shard.

---

## Retention Period

Default: **24 hours**  
Extended: up to **365 days** (additional cost)

Data in the stream is available to consumers during the retention window. After it expires, it's gone.

Use extended retention when:
- Consumers might fall behind
- You need to reprocess historical stream data
- Disaster recovery / replay scenarios

---

## Scaling (Resharding)

**Split shard**: Split one shard into two → double capacity  
**Merge shards**: Combine two adjacent shards into one → halve capacity

Resharding takes time and temporarily affects throughput — plan ahead.

**On-demand mode**: Automatically scales capacity up and down — no manual shard management. Up to 200 MB/s write. Higher cost than provisioned for predictable workloads.

---

## Kinesis Producer Library (KPL) and Consumer Library (KCL)

**KPL** (producer side):
- Automatic batching, compression, retry logic
- Aggregates multiple small records into one Kinesis record
- Use for high-throughput producers

**KCL** (consumer side):
- Manages shard assignment across multiple consumer instances
- Handles checkpointing (tracks which records were processed)
- Automatically rebalances when consumers are added/removed

---

## Common Architecture Patterns

```
Mobile/Web events → KPL → Kinesis Streams → Lambda (real-time processing)
                                           → Kinesis Analytics (real-time aggregations)
                                           → Kinesis Firehose → S3 (archive)
```

```
IoT sensors → Kinesis Streams → Lambda → DynamoDB (latest state)
                                       → Firehose → S3 + Redshift (historical analysis)
```

---

## Exam Scenarios

**"Need real-time ingestion of millions of events/second"** → Kinesis Data Streams  
**"Consumer is falling behind"** → Check `IteratorAgeMilliseconds` metric, add shards or optimize consumer  
**"Multiple teams need to independently process the same stream"** → Enhanced Fan-Out (dedicated 2MB/s per consumer)  
**"Need to replay 3 days of stream data"** → Extended retention period  
**"Events for the same user must be processed in order"** → Use `user_id` as partition key
