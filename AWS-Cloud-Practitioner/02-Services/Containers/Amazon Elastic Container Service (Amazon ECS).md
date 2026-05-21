# Amazon Elastic Container Service (Amazon ECS)

## What you'll learn
- What ECS is and how it orchestrates containers
- ECS launch types: EC2 and Fargate
- How ECS compares to EKS
- Key exam facts

---

## What is Amazon ECS?

Amazon Elastic Container Service (ECS) is a **fully managed container orchestration service**. It runs Docker containers at scale — scheduling, deploying, scaling, and managing containerized applications across a cluster of compute resources.

ECS is AWS's own container orchestration system, designed to be simpler than Kubernetes while deeply integrated with AWS services.

---

## Key concepts

**Task definition** — A blueprint describing your container: the Docker image (from ECR), required CPU and memory, port mappings, environment variables, and logging configuration.

**Task** — A running instance of a task definition. A single running container or group of containers.

**Service** — Manages long-running tasks — ensures a specified number of tasks are always running, replaces failed tasks, and integrates with load balancers.

**Cluster** — A logical grouping of compute resources where tasks run.

---

## ECS launch types

### EC2 launch type
You provision and manage a cluster of EC2 instances. ECS schedules tasks onto these instances. You control the EC2 instances (instance type, patching, scaling).

*Use when*: You need control over the underlying infrastructure (specific instance types, GPU instances, spot savings).

### Fargate launch type
AWS manages the underlying compute infrastructure. You specify the CPU and memory for each task; Fargate provisions the right compute automatically. No EC2 instances to manage.

*Use when*: You want to run containers without managing servers (serverless containers).

---

## ECS vs EKS

| | Amazon ECS | Amazon EKS |
|-|-----------|-----------|
| Orchestration | AWS-native (proprietary) | Kubernetes (open standard) |
| Complexity | Simpler | More complex |
| Portability | AWS-specific | Kubernetes workloads portable to other clouds/on-prem |
| Learning curve | Easier if new to containers | Requires Kubernetes knowledge |
| Best for | Teams starting with containers on AWS | Teams with existing Kubernetes expertise |

---

## When to use it

**Microservices deployment** — Run a set of microservices as separate ECS services, each automatically scaled based on CPU/memory usage.

**Batch container workloads** — Run tasks on a schedule (like cron jobs) using ECS scheduled tasks.

**API backends** — Deploy containerized REST APIs behind an Application Load Balancer with ECS managing the container lifecycle.

---

## Exam focus

- ECS = **AWS-native container orchestration** — simpler than Kubernetes
- Two launch types: **EC2** (you manage servers) or **Fargate** (serverless — AWS manages servers)
- Uses **task definitions** to define containers; **services** to keep them running at scale
- Pulls images from **ECR** (Amazon's container registry)
- Key differentiator from EKS: ECS is AWS-specific; EKS uses standard Kubernetes
- Use when exam describes: "run containers on AWS," "container orchestration," "deploy Docker applications"

---

## Practice questions

**Q1.** A company wants to run containerized microservices on AWS without managing any EC2 instances. They want AWS to handle all server provisioning and scaling. Which combination of services achieves this?

A) Amazon ECS with EC2 launch type  
B) Amazon EKS with self-managed nodes  
C) Amazon ECS with Fargate launch type  
D) Amazon ECR with EC2

**Answer: C** — ECS with the Fargate launch type is serverless container execution — you define the container task (CPU, memory, image) and AWS provisions and manages all the underlying compute. ECS with EC2 requires managing EC2 instances. EKS with self-managed nodes also requires managing EC2. ECR is just a container registry, not an orchestration service.
