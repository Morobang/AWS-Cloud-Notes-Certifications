# Frontend Web and Mobile — Category Overview

AWS Frontend Web and Mobile services help developers build, deploy, and connect web and mobile applications to backend AWS resources quickly — without managing infrastructure.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| AWS Amplify | Full-stack web and mobile app development and hosting | CI/CD + hosting + backend services for frontend apps |
| AWS AppSync | Managed GraphQL API service | Real-time GraphQL APIs connected to AWS data sources |

---

## How they relate

**Amplify** is the broader platform — it handles hosting, CI/CD deployment, authentication, and backend provisioning for a frontend app. Amplify can use AppSync as its API layer when the app needs GraphQL.

**AppSync** is the API layer — it provides a managed GraphQL endpoint that can connect to DynamoDB, Lambda, RDS, HTTP endpoints, and more.

---

## Exam selection guide

- "Host and deploy a web or mobile app with CI/CD" → **AWS Amplify**
- "Build a GraphQL API with real-time subscriptions" → **AWS AppSync**
- "Mobile app needs authentication, storage, and API in one framework" → **AWS Amplify**
- "Multiple frontend clients need to subscribe to live data updates" → **AWS AppSync**
