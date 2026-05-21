# Amazon Kinesis

## What you'll learn
- What Kinesis is and why real-time streaming matters
- The three Kinesis services and when to use each
- How Kinesis differs from SQS and batch analytics tools
- Key Kinesis facts for the exam

---

## What is Amazon Kinesis?

Amazon Kinesis is a suite of services for **real-time data streaming** — ingesting, processing, and analyzing data that arrives continuously and must be acted on immediately (within milliseconds to seconds).

Use cases requiring real-time processing include: fraud detection, clickstream analysis, IoT sensor monitoring, live dashboards, and application log monitoring.

---

## The three Kinesis services

### Kinesis Data Streams

The foundational streaming service. Producers write records to a stream; multiple consumers can read from the same stream simultaneously. You build the processing logic yourself (using Lambda, EC2, or the KCL library).

- Stores data for 24 hours by default (up to 365 days)
- Supports multiple consumers reading the same data simultaneously
- Requires you to manage shards (capacity units)
- Best when: you need custom processing logic or multiple independent consumers

### Kinesis Data Firehose

The easiest way to load streaming data into a destination. Firehose buffers incoming records and automatically delivers them to S3, Redshift, OpenSearch, or Splunk. No processing code required — just configure the destination.

- Fully managed — no shards, no consumers to build
- Optionally transforms data using Lambda before delivery
- Small latency buffer (60 seconds minimum, or based on data size)
- Best when: you need streaming data delivered to S3/Redshift/OpenSearch with minimal code

### Kinesis Data Analytics

Run SQL or Apache Flink queries against a live data stream. Kinesis Data Analytics reads from a Kinesis Data Stream or Firehose, applies your query in real time, and outputs results to another stream or destination.

- Use SQL or Apache Flink (Java/Python)
- Real-time aggregations, filtering, anomaly detection
- Best when: you need continuous queries on streaming data

---

## When to use Kinesis

**Fraud detection** — Financial transactions arrive as a stream; Kinesis Data Analytics detects suspicious patterns within milliseconds.

**Real-time dashboards** — Website clickstream data flows through Firehose into OpenSearch, powering a live user behavior dashboard.

**IoT sensor data** — Thousands of sensors send readings every second; Kinesis Data Streams ingests all of them and fans out to multiple downstream processors.

**Log delivery** — Application servers stream logs through Kinesis Firehose to S3 and OpenSearch for archiving and analysis.

---

## Kinesis vs SQS vs Batch analytics

| | Kinesis Data Streams | Amazon SQS | Batch analytics (Athena/Glue) |
|-|---------------------|------------|-------------------------------|
| Processing model | Real-time streaming | Async message queue | Batch (process stored data) |
| Latency | Milliseconds | Seconds | Minutes |
| Multiple consumers | Yes (fan-out) | No (each message consumed once) | N/A |
| Use case | Live stream processing | Decoupling services | Historical analysis |

---

## Exam focus

- Kinesis = **real-time** data streaming; keywords are "milliseconds" or "within seconds of arrival"
- **Firehose** = simplest delivery to S3/Redshift — no consumer code required
- **Data Streams** = custom processing, multiple consumers
- **Data Analytics** = real-time SQL/Flink queries on the stream
- Key differentiator from SQS: Kinesis is for **streaming analytics**; SQS is for **message queuing between services**
- Key differentiator from Athena/Glue: those process data at rest; Kinesis processes **data in motion**

---

## Practice questions

**Q1.** A fintech company must detect fraudulent transactions within 200 milliseconds of each transaction occurring. Which service enables this real-time processing?

A) Amazon Athena  
B) Amazon Kinesis  
C) AWS Glue  
D) Amazon QuickSight

**Answer: B** — Kinesis processes streaming data with millisecond-to-second latency. Kinesis Data Analytics can run continuous queries against the live stream to detect patterns in real time. Athena and Glue process data at rest in batches. QuickSight is a visualization tool.

---

**Q2.** A company wants to stream application logs from EC2 instances to S3 for archiving, with no custom processing code. Which Kinesis service is most appropriate?

A) Kinesis Data Streams  
B) Kinesis Data Firehose  
C) Kinesis Data Analytics  
D) Amazon SQS

**Answer: B** — Kinesis Data Firehose automatically buffers and delivers streaming data to S3 (and other destinations) with no consumer code required. Data Streams requires building a consumer. Data Analytics is for running queries on streams. SQS is a message queue, not a streaming delivery service.
