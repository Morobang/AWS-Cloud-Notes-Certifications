# Amazon API Gateway

## What you'll learn
- What API Gateway is and what it manages
- REST vs HTTP vs WebSocket APIs
- API Gateway + Lambda: the serverless API pattern
- Key exam facts

---

## What is Amazon API Gateway?

Amazon API Gateway is a **fully managed service for creating, deploying, and managing APIs** at any scale. It acts as the "front door" for applications to access backend services — Lambda functions, EC2 instances, other AWS services, or any HTTP endpoint.

---

## What API Gateway does

API Gateway handles the undifferentiated heavy lifting of API management:

- **Request routing**: Route incoming API requests to the correct backend (Lambda, EC2, Step Functions, etc.)
- **Authentication and authorization**: Validate API keys, verify JWT tokens, integrate with Cognito, or use Lambda authorizers
- **Throttling**: Rate-limit requests to protect backend services from being overwhelmed
- **Caching**: Cache API responses to reduce latency and backend load
- **Monitoring**: Automatically logs metrics (request count, latency, error rates) to CloudWatch
- **SDK generation**: Generate client SDKs for iOS, Android, JavaScript

---

## Three API types

**REST API** — The original API Gateway offering. Full feature set: request transformation, usage plans, API keys, caching.

**HTTP API** — Simpler, lower-latency, lower-cost alternative to REST API. Lacks some features (no API keys, no caching) but is faster and 70% cheaper. Best for most Lambda-backed APIs.

**WebSocket API** — Maintains persistent two-way connections between clients and the backend. Used for chat applications, live dashboards, and real-time notifications.

---

## API Gateway + Lambda: the serverless API pattern

The most common pattern on AWS:

- Client sends HTTP request to API Gateway endpoint
- API Gateway routes to a Lambda function
- Lambda executes business logic (database query, computation, etc.)
- Lambda returns a response; API Gateway formats and returns it to the client

This pattern requires no servers — API Gateway and Lambda are both fully managed.

---

## Stages

APIs in API Gateway are deployed to **stages** (e.g., dev, staging, prod). Each stage has its own URL and can have different settings.

---

## Exam focus

- API Gateway = **managed API front door** — create, manage, and secure APIs
- **REST API** = full-featured; **HTTP API** = simpler, cheaper; **WebSocket API** = real-time bidirectional
- Standard serverless pattern: **API Gateway + Lambda** (no servers, pay per request)
- Features: throttling, caching, auth, monitoring
- Use when exam describes: "create a REST API," "expose Lambda function via HTTP," "manage API traffic"

---

## Practice questions

**Q1.** A developer is building a serverless backend for a mobile app. They need an HTTPS endpoint that routes requests to Lambda functions, handles authentication, and throttles requests to protect the functions from traffic spikes. Which AWS service provides this API layer?

A) Elastic Load Balancing  
B) Amazon API Gateway  
C) AWS AppSync  
D) Amazon CloudFront

**Answer: B** — API Gateway provides a managed HTTPS endpoint that routes requests to Lambda functions, with built-in support for authentication (Cognito, API keys, Lambda authorizers) and throttling. ELB distributes traffic to EC2 or container fleets — not designed for Lambda API patterns. AppSync provides GraphQL APIs specifically. CloudFront is a CDN that can route to API Gateway but isn't the API layer itself.
