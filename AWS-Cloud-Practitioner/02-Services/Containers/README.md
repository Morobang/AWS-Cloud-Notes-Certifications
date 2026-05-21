# Container Services

Containers package application code with all its dependencies into a portable unit that runs consistently across environments.

## Services in this category

| Service | What it does | Key use case |
|---------|-------------|--------------|
| [Amazon ECR](./Amazon%20Elastic%20Container%20Registry%20%28Amazon%20ECR%29.md) | Store and manage Docker container images | Private container image registry |
| [Amazon ECS](./Amazon%20Elastic%20Container%20Service%20%28Amazon%20ECS%29.md) | Run containers on AWS (AWS-native orchestration) | Deploy containerized applications |
| [Amazon EKS](./Amazon%20Elastic%20Kubernetes%20Service%20%28Amazon%20EKS%29.md) | Managed Kubernetes on AWS | Run Kubernetes without managing control plane |

## How to choose

```
Need to store Docker images securely → ECR
Want to run containers using AWS-native tools → ECS
Already using Kubernetes or need Kubernetes features → EKS
Want containers without managing servers → Use ECS or EKS with AWS Fargate
```

**Note:** AWS Fargate (serverless container compute) is in the [Serverless](../Serverless/) category.
