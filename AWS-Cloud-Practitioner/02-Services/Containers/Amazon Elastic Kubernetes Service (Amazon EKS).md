# Amazon Elastic Kubernetes Service (Amazon EKS)

## What you'll learn
- What Kubernetes is and what EKS manages
- EKS node types
- When to use EKS vs ECS
- Key exam facts

---

## What is Amazon EKS?

Amazon Elastic Kubernetes Service (EKS) is a **managed Kubernetes service**. Kubernetes is an open-source container orchestration platform that automates deploying, scaling, and managing containerized applications. EKS runs the Kubernetes **control plane** for you — the complex distributed system that manages the cluster state, schedules workloads, and handles failover.

You still provide the worker nodes (EC2 instances or Fargate) where your containers run, but the control plane management is handled by AWS.

---

## What Kubernetes / EKS does

- Schedules containers onto available nodes based on resource requirements
- Replaces failed containers automatically
- Scales deployments up and down based on load
- Manages networking between containers (pods)
- Handles rolling deployments and rollbacks

---

## EKS node options

**Self-managed nodes** — You provision and manage EC2 instances that join the EKS cluster. Full control over instance type and configuration.

**Managed node groups** — AWS provisions and manages EC2 instances for you, but they're still EC2 instances in your account. AWS handles updates and patches.

**AWS Fargate** — Serverless. EKS schedules pods on Fargate — no EC2 instances to manage.

---

## Why use EKS over ECS

| | Amazon EKS | Amazon ECS |
|-|-----------|-----------|
| Orchestration | Kubernetes (industry standard) | AWS-proprietary |
| Portability | Workloads portable to other Kubernetes environments | AWS-only |
| Ecosystem | Vast Kubernetes tool ecosystem (Helm, Istio, etc.) | Limited to AWS integrations |
| Complexity | Higher | Lower |
| Best for | Kubernetes expertise, multi-cloud, open standards | Simple container workloads on AWS only |

**Choose EKS when**: Your team knows Kubernetes, you want portability, or you rely on Kubernetes-native tools and the ecosystem (Helm charts, operators, Istio service mesh).

**Choose ECS when**: You're new to containers, you're AWS-only, and you want simplicity.

---

## Exam focus

- EKS = **managed Kubernetes on AWS** — AWS manages the control plane
- Worker nodes can be: EC2 (self-managed or managed groups) or **Fargate** (serverless)
- Kubernetes workloads are **portable** — the same configurations work on-premises or on other cloud providers
- Key differentiator from ECS: EKS uses **standard Kubernetes**; ECS is AWS-proprietary
- Use when exam describes: "Kubernetes," "existing Kubernetes workloads," "container portability across clouds," "Kubernetes-native tools"

---

## Practice questions

**Q1.** A company has a large portfolio of containerized microservices currently running on Kubernetes in their own data center. They want to migrate to AWS while keeping the same orchestration tools and configurations. Which service minimizes migration effort?

A) Amazon ECS  
B) Amazon EKS  
C) AWS Elastic Beanstalk  
D) Amazon EC2 with Docker

**Answer: B** — EKS runs standard Kubernetes, so existing Kubernetes manifests, Helm charts, and tooling work without modification. ECS uses a different (AWS-proprietary) orchestration model that would require rewriting container configurations. Beanstalk is PaaS, not container orchestration. Running Docker on EC2 requires manually managing Kubernetes, which is complex.
