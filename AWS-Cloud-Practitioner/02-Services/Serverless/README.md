# Serverless — Category Overview

Serverless compute services let you run code and containers without managing servers. AWS provisions, scales, and maintains the underlying infrastructure automatically — you pay only for what executes.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| AWS Lambda | Run code in response to events | Functions triggered by events, billed per millisecond |
| AWS Fargate | Run containers without managing servers | Serverless container execution for ECS and EKS |

---

## Serverless vs server-based

| | Serverless | Server-based (EC2) |
|-|-----------|-------------------|
| Provisioning | Automatic — no instances to manage | Manual — choose instance type and size |
| Scaling | Automatic — scales to zero | Manual or Auto Scaling |
| Pricing | Pay per execution/request | Pay per hour instance runs (even when idle) |
| Maintenance | None | OS patches, security updates |

---

## Exam selection guide

- "Run code in response to an event — S3 upload, API call, schedule" → **AWS Lambda**
- "Run Docker containers without managing EC2 instances" → **AWS Fargate**
- "Both ECS and EKS can use this serverless compute engine" → **AWS Fargate**
