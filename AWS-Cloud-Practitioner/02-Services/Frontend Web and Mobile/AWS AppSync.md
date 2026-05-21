# AWS AppSync

## What you'll learn
- What GraphQL is and what AppSync provides
- Real-time subscriptions
- AppSync vs REST APIs (API Gateway)
- Key exam facts

---

## What is AWS AppSync?

AWS AppSync is a **fully managed GraphQL API service**. It lets you build flexible, real-time APIs that connect to multiple data sources — DynamoDB, Lambda, RDS, Elasticsearch, and HTTP endpoints — behind a single GraphQL endpoint.

---

## What is GraphQL?

GraphQL is an API query language that gives clients control over what data they receive:

- **REST**: Multiple endpoints, each returning a fixed data structure (`GET /users`, `GET /users/123/orders`)
- **GraphQL**: One endpoint; the client specifies exactly which fields it needs in the query

This is useful for mobile clients that need to minimize data transfer — fetch only the fields you need, in one request.

---

## Key AppSync features

**Single endpoint** — All data queries, mutations, and subscriptions go through one GraphQL endpoint.

**Multiple data sources** — One API can query DynamoDB for user data, call a Lambda function for business logic, and query RDS for relational data — in a single GraphQL query.

**Real-time subscriptions** — Clients can subscribe to data changes. When data is updated in DynamoDB, all subscribed clients receive the update in real time via WebSocket. Useful for live chat, collaborative editing, and dashboards.

**Offline support** — AppSync's client libraries (used with Amplify) can cache data locally and sync when connectivity is restored.

**Managed conflict resolution** — When multiple clients write the same data (common in offline sync scenarios), AppSync handles conflict resolution.

---

## AppSync vs API Gateway

| | AWS AppSync | Amazon API Gateway |
|-|-------------|-------------------|
| API style | GraphQL | REST or HTTP |
| Data fetching | Client specifies fields | Fixed response structure |
| Real-time | Built-in subscriptions | Requires WebSocket API setup |
| Data sources | Multiple in one query | One per endpoint |
| Best for | Flexible frontend queries, real-time data | REST APIs, microservice HTTP APIs |

---

## When to use it

**Mobile and web apps with complex data** — Clients query exactly what they need; the API handles fetching from multiple sources.

**Real-time features** — Live chat, collaborative document editing, live sports scores, stock tickers — subscribe to data changes.

**Offline-capable apps** — Mobile apps that need to work without connectivity and sync when reconnected.

---

## Exam focus

- AppSync = **managed GraphQL API** — single endpoint, multiple data sources, real-time subscriptions
- Built-in **real-time subscriptions** via WebSocket — clients receive updates when data changes
- Connects to DynamoDB, Lambda, RDS, and other AWS services
- Key differentiator from API Gateway: AppSync uses GraphQL; API Gateway uses REST/HTTP

---

## Practice questions

**Q1.** A developer is building a real-time collaborative app where multiple users see updates to shared data instantly. The app needs a flexible API that can fetch data from DynamoDB and Lambda in a single request. Which AWS service is most appropriate?

A) Amazon API Gateway  
B) AWS AppSync  
C) Amazon SQS  
D) AWS Step Functions

**Answer: B** — AppSync provides a GraphQL API with built-in real-time subscriptions (clients receive updates instantly when data changes) and can aggregate data from DynamoDB and Lambda in a single query. API Gateway uses REST and does not have built-in subscription support. SQS is a message queue, not an API. Step Functions orchestrates workflows, not APIs.
