# Amazon Simple Queue Service (Amazon SQS)

## What you'll learn
- What SQS is and how message queues work
- Standard queues vs FIFO queues
- When to use SQS vs SNS
- Key exam facts

---

## What is Amazon SQS?

Amazon SQS is a **fully managed message queue service**. Producers send messages to a queue; consumers poll the queue and process messages at their own pace. If the consumer is unavailable or slow, messages accumulate in the queue and are processed when the consumer recovers.

The defining purpose of SQS is **decoupling**: separating the producer (which creates work) from the consumer (which does the work) so neither is dependent on the other's availability or speed.

---

## How it works

1. A **producer** sends a message to an SQS queue
2. The message is **stored in the queue** (up to 14 days by default)
3. A **consumer** polls the queue, receives and processes the message
4. The consumer **deletes** the message from the queue after successful processing
5. If the consumer fails to delete within the **visibility timeout**, the message becomes visible again for another consumer to pick up

---

## Standard queues vs FIFO queues

| | Standard Queue | FIFO Queue |
|-|----------------|-----------|
| Ordering | Best-effort (approximately in order) | Strictly first-in, first-out |
| Delivery | At-least-once (same message may appear twice) | Exactly-once processing |
| Throughput | Nearly unlimited | 300–3,000 messages/second |
| Use case | Most workloads; order doesn't matter | Order matters; no duplicate processing allowed |
| Example | Processing image uploads | Financial transactions, inventory updates |

**Use FIFO when**: order of processing matters (e.g., bank transactions, order state machine steps) or when duplicate processing would cause problems.

---

## When to use it

**Traffic spike buffering** — A web application receives 10,000 orders per minute during a sale. Instead of overwhelming a processing service, orders go to an SQS queue and the processor consumes at its own pace.

**Decoupling microservices** — Service A sends work to an SQS queue; Service B polls and processes it. If Service B is restarted, messages accumulate and are processed when it comes back — nothing is lost.

**Dead-letter queue (DLQ)** — Messages that fail processing repeatedly can be moved to a dead-letter queue for investigation, preventing a bad message from blocking the entire queue.

---

## SQS vs SNS

| | Amazon SQS | Amazon SNS |
|-|-----------|-----------|
| Pattern | Queue (pull) | Pub/sub (push) |
| Consumers | One consumer per message | All subscribers get every message |
| Persistence | Messages stored up to 14 days | Fire-and-forget |
| Best for | Decoupling producer/consumer | Fan-out to multiple consumers |

**Combined pattern**: SNS + SQS fan-out. SNS delivers to multiple SQS queues (each for a different consumer), giving you both fan-out delivery and per-consumer message buffering with persistence.

---

## Exam focus

- SQS = **message queue** for **decoupling** services
- Messages persist in the queue until consumed (up to 14 days)
- **Standard**: at-least-once, best-effort ordering
- **FIFO**: exactly-once, strict ordering — use when order and no duplicates matter
- **Dead-letter queue (DLQ)**: captures failed messages for troubleshooting
- **Visibility timeout**: prevents two consumers from processing the same message simultaneously
- Key differentiator from SNS: SQS delivers to **one consumer**; SNS delivers to **all subscribers**

---

## Practice questions

**Q1.** A video processing application receives video upload notifications from S3. The processing takes 10 minutes per video. During peak hours, uploads arrive faster than they can be processed. Which service should be used to ensure no uploads are lost and the processor can work at its own pace?

A) Amazon SNS  
B) Amazon SQS  
C) Amazon EventBridge  
D) Amazon Kinesis

**Answer: B** — SQS buffers the upload notifications in a queue. The processing service consumes at its own pace, and no messages are lost even if processing lags behind. SNS is push-based and doesn't buffer; EventBridge routes events but doesn't buffer them; Kinesis is for real-time streaming, not workload buffering.

---

**Q2.** A banking system processes fund transfer requests. It is critical that transfers are processed in the exact order they were submitted and that each transfer is processed exactly once. Which SQS queue type should be used?

A) Standard queue  
B) FIFO queue  
C) Dead-letter queue  
D) Either — SQS always processes in order

**Answer: B** — FIFO queues guarantee strict first-in, first-out ordering and exactly-once processing, preventing duplicate processing of transfers. Standard queues have best-effort ordering and at-least-once delivery — allowing potential duplicates and out-of-order processing, which is unacceptable for financial transactions.
