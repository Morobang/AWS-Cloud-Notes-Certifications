# Amazon MSK (Managed Streaming for Apache Kafka)

**Category**: Managed Event Streaming  
**Exam weight**: Medium-High — know when to choose Kafka vs Kinesis

---

## What Is It?

Amazon MSK is a fully managed Apache Kafka service. Kafka is the industry-standard open-source distributed event streaming platform — MSK lets you run it on AWS without managing brokers, ZooKeeper, storage, or patching.

---

## Apache Kafka Core Concepts

Understanding Kafka concepts is required because MSK exposes them directly — unlike Kinesis, which abstracts them.

| Concept | Description |
|---|---|
| **Broker** | A Kafka server that stores and serves messages |
| **Topic** | A named category for messages (like a channel) |
| **Partition** | A topic is split into partitions — the unit of parallelism |
| **Offset** | A sequential ID for each message within a partition |
| **Producer** | Application that writes messages to a topic |
| **Consumer** | Application that reads messages from a topic |
| **Consumer group** | Group of consumers that share reading of a topic (each partition consumed by one member) |
| **Replication factor** | How many copies of each partition exist across brokers (durability) |
| **Retention** | How long messages are kept (time-based or size-based; Kafka can retain for weeks) |

### Kafka vs Kinesis Concepts

| Kafka | Kinesis equivalent |
|---|---|
| Topic | Stream |
| Partition | Shard |
| Consumer group offset | Shard iterator position |
| Broker | AWS-managed (not exposed) |
| Retention (days to weeks) | Retention (24h – 365 days) |

---

## MSK Cluster Types

| Type | Description |
|---|---|
| **Provisioned** | You specify broker instance type and count — predictable capacity and cost |
| **Serverless** | MSK manages capacity automatically — you pay per partition-hour |

**MSK Serverless**: Suitable for variable workloads, development environments, or when you don't want to manage broker sizing. Fewer configuration options than Provisioned.

---

## MSK Connect

Managed Kafka Connect — run Kafka connector plugins without managing Connect workers:

- **Source connectors**: Pull data from external systems INTO Kafka topics (databases, S3, HTTP)
- **Sink connectors**: Push data FROM Kafka topics TO destinations (S3, OpenSearch, Redshift, DynamoDB)

**Example with Debezium source connector**: Capture database change events (CDC) from MySQL/PostgreSQL and publish them to Kafka topics.

```
MySQL binlog → MSK Connect (Debezium) → Kafka topic (CDC events) → consumers
```

---

## MSK vs Kinesis Data Streams

| | Amazon MSK | Kinesis Data Streams |
|---|---|---|
| Underlying tech | Apache Kafka | AWS-proprietary |
| Migration from Kafka | Easy — same APIs and client libs | Requires code changes |
| Partition/shard count | You configure per topic | Managed, can merge/split |
| Retention | Up to weeks or longer | 1 day – 365 days |
| Consumer offset mgmt | Kafka consumer groups | Iterator checkpoint (Lambda) or sequence number |
| Ecosystem | Vast (Kafka Connect, Streams, Flink, Spark) | AWS-native |
| Pricing | Broker instance hours | Per shard-hour + data transfer |
| Expertise needed | Kafka knowledge required | Less — more abstracted |

**Choose MSK when:**
- You have existing Kafka applications to migrate to AWS
- Your team knows Kafka and wants to use the Kafka ecosystem (Kafka Streams, KSQL, Debezium)
- You need Kafka-specific features (compacted topics, exactly-once semantics with Kafka transactions)
- You need message retention longer than Kinesis supports

**Choose Kinesis when:**
- AWS-native project with no Kafka background
- Tighter AWS service integration (Kinesis Firehose, Lambda triggers) without custom connectors
- Simpler management with less Kafka expertise

---

## MSK and Data Engineering Patterns

### CDC (Change Data Capture) Pipeline

```
Database (PostgreSQL)
    ↓ Debezium CDC connector (via MSK Connect)
MSK Topic: customer-updates
    ├── Consumer 1: Managed Apache Flink → streaming aggregations → S3
    └── Consumer 2: Lambda (sink) → DynamoDB (real-time view)
```

### Lambda as MSK Consumer

Lambda can consume from MSK topics directly (Event Source Mapping):

```python
def handler(event, context):
    for record in event['records']['my-topic-0']:
        import base64
        value = base64.b64decode(record['value']).decode('utf-8')
        # Process the message
        print(f"Offset {record['offset']}: {value}")
```

Lambda's Event Source Mapping manages offset commits automatically.

### Flink on MSK

Amazon Managed Service for Apache Flink natively supports MSK as both a source and sink:

```java
// Flink Kafka source (simplified)
KafkaSource<String> source = KafkaSource.<String>builder()
    .setBootstrapServers("broker-1:9092,broker-2:9092")
    .setTopics("user-events")
    .setGroupId("flink-consumer-group")
    .setValueOnlyDeserializer(new SimpleStringSchema())
    .build();
```

---

## MSK Security

**Encryption in transit**: TLS 1.2+ between clients and brokers, and between brokers.

**Authentication methods:**
| Method | Description |
|---|---|
| IAM | AWS IAM authentication — clients sign requests with AWS credentials |
| SASL/SCRAM | Username/password stored in AWS Secrets Manager |
| mTLS | Mutual TLS — client certificate authentication |

**Authorization**: Apache Kafka ACLs (Access Control Lists) define which principals can produce/consume from which topics.

---

## Monitoring MSK

Key CloudWatch metrics:

| Metric | What to watch |
|---|---|
| `UnderReplicatedPartitions` | Partitions with fewer replicas than configured — broker health issue |
| `ActiveControllerCount` | Should always be 1; 0 = cluster problem |
| `OfflinePartitionsCount` | Partitions with no leader — data unavailable |
| `BytesInPerSec` | Producer throughput per broker |
| `ConsumerLag` | How far behind consumers are (via Consumer Group offset tools) |

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Migrate existing on-premises Kafka application to AWS | Amazon MSK — Kafka-compatible, same client libraries |
| Capture PostgreSQL database changes and stream to multiple consumers | MSK Connect with Debezium CDC connector |
| Need message retention for 14 days for replay/reprocessing | Amazon MSK (Kinesis max with extended retention) |
| Streaming pipeline with AWS-native services, no Kafka experience | Amazon Kinesis Data Streams |
| Process Kafka messages in real-time with SQL-like windowed aggregations | Amazon Managed Service for Apache Flink with MSK source |
