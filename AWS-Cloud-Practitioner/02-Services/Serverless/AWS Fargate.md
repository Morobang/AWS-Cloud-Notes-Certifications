# AWS Fargate

## What you'll learn
- What Fargate is and how it fits with ECS and EKS
- Fargate vs EC2 launch type
- Key exam facts

---

## What is AWS Fargate?

AWS Fargate is a **serverless compute engine for containers** that works with both Amazon ECS and Amazon EKS. With Fargate, you run containers without managing the underlying EC2 instances — AWS provisions and manages the server infrastructure for you.

Fargate sits between Lambda (code) and EC2 (full servers) — it's serverless for containerized workloads.

---

## Fargate in the container stack

**Without Fargate (EC2 launch type)**:
- You create an ECS cluster
- You provision EC2 instances to run in the cluster
- You manage those EC2 instances: choose instance type, patch the OS, scale the fleet, pay for idle capacity

**With Fargate**:
- You create an ECS cluster
- You define your task (container specs: image, CPU, memory, networking)
- Fargate provisions and manages the compute automatically
- You only manage the container — not the host

---

## Fargate pricing

With Fargate, you pay for the **vCPU and memory resources your containers use while they run** — not for idle EC2 capacity. No over-provisioning.

---

## Fargate vs EC2 launch type

| | Fargate | EC2 Launch Type |
|-|---------|----------------|
| Server management | None — Fargate manages | You manage EC2 instances |
| Scaling | Automatic (task-level) | EC2 cluster + task scaling |
| Pricing | Per task vCPU + memory | Per EC2 instance hour |
| Control | Less control over host | Full control of EC2 |
| Best for | Most container workloads | Windows containers, GPU, max cost control |

---

## Fargate vs Lambda

| | AWS Fargate | AWS Lambda |
|-|------------|-----------|
| Packaging | Docker containers | Function code (.zip or container) |
| Runtime limit | No time limit (runs as long as needed) | 15-minute maximum |
| State | Can use persistent storage (EFS) | Stateless |
| Best for | Long-running services, microservices | Short, event-driven tasks |

---

## When to use it

**Containerized microservices** — Run ECS services without an EC2 fleet. Fargate manages the hosts; you manage the containers.

**Variable load workloads** — Pay only for what runs. No idle EC2 instances during off-hours.

**Security isolation** — Each Fargate task runs in its own kernel — stronger isolation than shared EC2 instances.

---

## Exam focus

- Fargate = **serverless compute engine for containers** — runs ECS/EKS tasks without managing EC2
- No EC2 instances to provision, patch, or scale
- Pay for **vCPU + memory per task**, not per instance
- Works with both **ECS** (Elastic Container Service) and **EKS** (Elastic Kubernetes Service)
- Use when exam describes: "run containers without managing servers," "serverless containers"

---

## Practice questions

**Q1.** A company runs containerized microservices on Amazon ECS. Their operations team spends significant time managing and patching the EC2 instances in the ECS cluster. Which change eliminates this operational overhead?

A) Migrate to Amazon EC2 Auto Scaling  
B) Use AWS Elastic Beanstalk  
C) Switch to AWS Fargate as the ECS launch type  
D) Migrate to Amazon EKS

**Answer: C** — Fargate removes the need to manage EC2 instances for ECS. The operations team no longer needs to provision, patch, or manage cluster hosts — Fargate handles this automatically. Auto Scaling still requires EC2 management. Elastic Beanstalk is for web applications, not containerized microservices. EKS still has EC2 nodes to manage (unless also using Fargate with EKS).
