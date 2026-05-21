# Amazon Elastic Container Registry (Amazon ECR)

## What you'll learn
- What ECR is and why you need a container registry
- How ECR works with ECS and EKS
- Key ECR features
- Exam facts

---

## What is Amazon ECR?

Amazon Elastic Container Registry (ECR) is a **fully managed Docker container image registry**. It stores, manages, and deploys Docker container images privately within your AWS account.

Think of ECR as the AWS equivalent of Docker Hub — but private, integrated with your AWS IAM permissions, and hosted within your AWS account.

---

## Why you need a container registry

Before running a container, you need to store the container image somewhere that your container service (ECS or EKS) can pull it from. ECR provides that storage:

1. Developer builds a Docker image locally
2. Developer pushes the image to an ECR repository
3. ECS or EKS pulls the image from ECR when launching containers
4. ECR handles versioning via image tags

---

## Key features

- **Private repositories**: Images are accessible only within your AWS account and to IAM-authorized principals
- **IAM integration**: Use IAM policies to control who can push and pull images
- **Image scanning**: ECR can automatically scan images for known OS and software vulnerabilities (using Amazon Inspector)
- **Image replication**: Replicate images to other regions for multi-region deployments
- **Lifecycle policies**: Automatically delete old or untagged images to reduce storage costs
- **OCI compliance**: Supports Docker Image Manifest V2 and OCI image formats

---

## When to use it

**Storing application container images** — Every time a CI/CD pipeline builds a new version of a containerized application, it pushes the image to ECR. ECS or EKS pulls it during deployment.

**Security-conscious environments** — Organizations that cannot store images in public registries (Docker Hub) due to data or security policies use ECR for private storage.

**Multi-region deployments** — Use ECR replication to ensure container images are available in all regions where your application runs.

---

## Exam focus

- ECR = **private Docker container image registry** within your AWS account
- Works with ECS, EKS, and Fargate — they pull images from ECR
- Integrated with **IAM** for access control
- Supports **image vulnerability scanning** (via Amazon Inspector)
- **Lifecycle policies** automatically clean up old images
- Use when exam describes: "store container images," "private registry," "Docker image repository"

---

## Practice questions

**Q1.** A development team builds Docker container images in their CI/CD pipeline and needs to store them securely within AWS before deploying to Amazon ECS. Which service should they use to store the images?

A) Amazon S3  
B) Amazon ECR  
C) AWS CodeArtifact  
D) Amazon EFS

**Answer: B** — Amazon ECR is the managed Docker container image registry integrated with AWS. Container images are pushed to ECR and pulled by ECS during deployment. S3 stores objects but is not a container registry. CodeArtifact is for software packages (npm, Maven, PyPI), not container images. EFS is a shared file system.
