# Amazon Simple Notification Service (Amazon SNS)

## What you'll learn
- What SNS is and how the publish/subscribe model works
- What types of subscribers SNS supports
- The fan-out pattern and when to use it
- How SNS compares to SQS
- Key exam facts

---

## What is Amazon SNS?

Amazon SNS is a **fully managed pub/sub (publish/subscribe) messaging service**. Publishers send messages to a **topic**; SNS immediately delivers that message to **all subscribers** of that topic simultaneously.

The defining characteristic is **fan-out**: one message in, many deliveries out — all at the same time.

---

## How it works

1. You create an **SNS topic**
2. **Subscribers** register to receive messages from that topic
3. A **publisher** sends a message to the topic
4. SNS delivers the message to **every subscriber** simultaneously

---

## Subscriber types

SNS can deliver messages to:

| Subscriber type | Example use |
|----------------|-------------|
| Email/SMS | Alert a person directly |
| Amazon SQS | Fan-out to queue for async processing |
| AWS Lambda | Trigger serverless processing |
| HTTP/HTTPS endpoint | Webhook to an external system |
| Amazon Kinesis Data Firehose | Stream to S3/Redshift |
| Mobile push notifications | iOS, Android, FireOS |

---

## The fan-out pattern

Fan-out is the canonical SNS use case. A single event triggers multiple independent downstream systems simultaneously.

**Example**: An e-commerce order is placed. One SNS message triggers:
- Lambda function sends a confirmation email (via SES)
- SQS queue feeds an inventory update process
- SQS queue feeds a warehouse notification process
- Lambda function logs the event to analytics

All four happen simultaneously from one published message.

---

## When to use it

**Multi-consumer notifications** — One event (new user registered, order placed, file uploaded) needs to reach multiple different systems at once.

**Alerting** — CloudWatch alarms publish to an SNS topic; the topic delivers alerts via email and SMS simultaneously.

**Decoupled fan-out** — Instead of one service calling multiple downstream APIs directly, it publishes to SNS and each consumer subscribes independently.

---

## SNS vs SQS

| | Amazon SNS | Amazon SQS |
|-|-----------|-----------|
| Pattern | Publish/subscribe (push) | Message queue (pull) |
| Consumers | All subscribers get each message | One consumer gets each message |
| Delivery | Immediate, parallel to all subscribers | Consumers poll and receive one at a time |
| Persistence | Messages not stored (fire-and-forget) | Messages stored until consumed (up to 14 days) |
| Best for | Fan-out to multiple consumers | Decoupling producer/consumer processing speeds |

---

## Exam focus

- SNS = **pub/sub fan-out** — one message to **many** subscribers simultaneously
- Subscribers can be Lambda, SQS, email, SMS, HTTP endpoints, mobile push
- **Fan-out pattern**: SNS → multiple SQS queues → each queue feeds an independent consumer
- Messages are **not stored** in SNS — if a subscriber is unavailable, the message is lost (use SQS for durability)
- Key differentiator: SNS delivers to all subscribers; SQS delivers to one consumer

---

## Practice questions

**Q1.** When a new order is placed on an e-commerce platform, the system needs to simultaneously: send a confirmation email, update inventory, and notify the warehouse. All three must receive the same order data at the same time. Which service enables this?

A) Amazon SQS  
B) Amazon SNS  
C) Amazon EventBridge  
D) AWS Step Functions

**Answer: B** — SNS fan-out delivers the same message to all subscribers simultaneously. Each of the three downstream systems subscribes to the order topic and receives the same message at the same time. SQS delivers each message to only one consumer. EventBridge routes based on content filtering. Step Functions orchestrates sequential workflows.
