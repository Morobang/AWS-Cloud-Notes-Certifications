# AWS X-Ray

## What you'll learn
- What distributed tracing is and why it's needed
- What X-Ray does and how it works
- Key X-Ray concepts (traces, segments, service map)
- Key exam facts

---

## What is AWS X-Ray?

AWS X-Ray is a **distributed tracing service** that helps developers analyze and debug production applications — particularly microservices and serverless architectures where a single user request flows through multiple services.

X-Ray collects data about requests, identifies which service is slow or failing, and visualizes the entire request path as a trace.

---

## Why distributed tracing is needed

In a monolithic application, a single process handles a request — errors and latency are easy to locate. In a microservices architecture, a single user request might pass through:

- API Gateway → Lambda → DynamoDB
- → SQS → another Lambda → RDS
- → SNS → a third service

If the request is slow or fails, which service is responsible? X-Ray answers this by tracing the full path.

---

## Key concepts

**Trace** — The full end-to-end record of a single request as it passes through all services. Each trace has a unique trace ID.

**Segment** — The portion of a trace contributed by a single service. Each service that processes the request creates a segment with timing and metadata.

**Subsegment** — A more granular unit within a segment — for example, a specific database call or external HTTP request made by a service.

**Service map** — A visual diagram of all services involved in your application and how they connect. Shows latency and error rates on each connection — makes it easy to spot bottlenecks.

**Sampling** — X-Ray does not trace every request (that would be too expensive). It samples a percentage of requests to keep costs manageable while still providing representative data.

---

## How X-Ray works

1. You instrument your application by adding the X-Ray SDK to your code (or enabling X-Ray on Lambda/API Gateway without code changes)
2. The SDK adds a trace ID to each incoming request
3. As the request passes through each service, each service adds its segment data
4. X-Ray assembles all segments into a complete trace
5. The X-Ray console shows the service map, traces, and performance analytics

---

## X-Ray vs CloudWatch

| | AWS X-Ray | Amazon CloudWatch |
|-|-----------|-----------------|
| Purpose | Distributed request tracing | Metrics, logs, alarms |
| Granularity | Individual request traces | Aggregate metrics |
| Best for | Latency debugging in microservices | Infrastructure monitoring |
| Visualizes | Request path across services | CPU, memory, error rates over time |

CloudWatch tells you that something is wrong (e.g., error rate spiked). X-Ray tells you where and why (e.g., the DynamoDB call in service B is taking 800ms).

---

## When to use it

**Microservices debugging** — A request fails but you don't know which service is responsible. X-Ray's service map and traces pinpoint the exact failure point.

**Latency analysis** — Identify which downstream service call is adding the most latency to user requests.

**Serverless applications** — Trace requests through Lambda, API Gateway, DynamoDB, and S3 with minimal instrumentation.

---

## Exam focus

- X-Ray = **distributed tracing** — traces requests across microservices and serverless applications
- Produces a **service map** showing all services and their connections
- **Trace** = full request path; **segment** = one service's contribution
- Use when exam describes: "identify which service is causing latency," "trace requests across Lambda functions," "debug microservices performance"
- Complements CloudWatch: CloudWatch for metrics/logs, X-Ray for request-level tracing

---

## Practice questions

**Q1.** A company runs a serverless application using Lambda, API Gateway, and DynamoDB. Users report intermittent slow responses but the team cannot identify which component is responsible. Which AWS service can help trace the full request path and identify the bottleneck?

A) Amazon CloudWatch  
B) AWS X-Ray  
C) AWS CloudTrail  
D) AWS Config

**Answer: B** — X-Ray provides distributed tracing that shows the full request path through API Gateway, Lambda, and DynamoDB, with timing for each step. This pinpoints exactly where latency is being added. CloudWatch shows aggregate metrics but not per-request traces. CloudTrail records API calls for auditing, not performance tracing. Config tracks resource configuration changes.
