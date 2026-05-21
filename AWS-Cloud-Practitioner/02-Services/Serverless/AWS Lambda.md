# AWS Lambda

## What you'll learn
- What Lambda is and the event-driven model
- How Lambda executes and how it's billed
- Common Lambda event sources
- Lambda vs EC2
- Key exam facts

---

## What is AWS Lambda?

AWS Lambda is a **serverless compute service that runs your code in response to events**. You upload your code (a function); Lambda runs it when triggered — without you provisioning or managing any servers.

Lambda is the foundation of serverless architecture on AWS.

---

## How Lambda works

1. You write a **Lambda function** in a supported language (Python, Node.js, Java, Go, Ruby, .NET, or a custom runtime)
2. You configure a **trigger** — the event that invokes the function
3. When the trigger fires, Lambda runs your function in a managed execution environment
4. Lambda scales automatically: if 1,000 events arrive simultaneously, it runs 1,000 concurrent function instances
5. When the function completes, the execution environment is released

---

## Common event triggers

| Trigger | Example |
|---------|---------|
| API Gateway | HTTP request hits your REST API |
| S3 | File uploaded to an S3 bucket |
| DynamoDB Streams | Item changed in a DynamoDB table |
| SNS | Message published to an SNS topic |
| SQS | Message in an SQS queue |
| EventBridge | Scheduled event (cron) or AWS event |
| CloudWatch Alarms | Alarm state changes |
| Cognito | User authentication event |

---

## Lambda pricing

Lambda charges based on:
- **Number of requests**: First 1 million requests per month free; then per million requests
- **Duration**: The time your function runs, measured in milliseconds, multiplied by the amount of memory configured

You pay nothing when your function isn't running. This makes Lambda very cost-effective for infrequent or spiky workloads.

---

## Execution limits

- **Maximum execution time**: 15 minutes per invocation
- **Memory**: 128 MB to 10 GB
- **Deployment package**: Up to 250 MB (or larger via container images)

Lambda is not suitable for long-running processes (beyond 15 minutes) — those belong on EC2, ECS, or Batch.

---

## Lambda vs EC2

| | AWS Lambda | Amazon EC2 |
|-|-----------|-----------|
| Provisioning | None — serverless | Choose instance type and size |
| Scaling | Automatic, instantaneous | Auto Scaling (minutes) |
| Pricing | Per request + duration | Per hour (instance runs even idle) |
| State | Stateless (no persistent storage) | Stateful (persistent disk) |
| Max runtime | 15 minutes | Unlimited |
| Best for | Short, event-driven tasks | Long-running applications, servers |

---

## When to use it

**API backend** — Pair with API Gateway to build a fully serverless REST API — no web servers to manage.

**Data processing** — Process files as they're uploaded to S3 (resize images, parse CSVs, scan for viruses).

**Automation** — Run maintenance scripts on a schedule (EventBridge cron) without a running EC2 instance.

**Event-driven workflows** — React to changes in DynamoDB, messages in SQS, or events in EventBridge.

---

## Exam focus

- Lambda = **serverless functions** — event-driven, no servers to manage
- Triggers: API Gateway, S3, DynamoDB, SQS, SNS, EventBridge, and more
- **Pay per request + duration** — no charge when not running
- **15-minute maximum** execution time
- Scales automatically — handles concurrent invocations
- Use when exam describes: "event-driven," "run code without servers," "trigger function on S3 upload," "serverless API"

---

## Practice questions

**Q1.** A company wants to automatically generate thumbnail images every time a photo is uploaded to an Amazon S3 bucket. The image processing takes about 30 seconds. No servers should be managed. Which AWS service handles this?

A) Amazon EC2  
B) AWS Elastic Beanstalk  
C) AWS Lambda  
D) AWS Batch

**Answer: C** — Lambda can be triggered by S3 upload events. The function runs the image processing (well within the 15-minute limit) and stores the thumbnail. No servers to provision or manage — Lambda scales automatically as uploads arrive. EC2 and Elastic Beanstalk require managing server infrastructure. Batch is for longer-running batch jobs that don't need event-driven triggering.

**Q2.** Which statement best describes the AWS Lambda pricing model?

A) Pay per hour the function is deployed  
B) Pay a flat monthly fee based on configured memory  
C) Pay per request and per millisecond of execution time  
D) Pay per EC2 instance that Lambda uses

**Answer: C** — Lambda charges for the number of requests (invocations) and the duration of execution (in milliseconds × configured memory). There is no charge when the function is not running. Lambda runs on managed infrastructure — you never interact with EC2 instances.
