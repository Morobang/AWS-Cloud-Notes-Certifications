# Task Statement 3.3: Identify AWS Compute Services

## What you'll learn
- EC2 — virtual servers and when to use them
- EC2 pricing models: On-Demand, Reserved, Spot, and Savings Plans
- Lambda — serverless functions and when they make sense
- Container services: ECS, EKS, and Fargate
- Elastic Beanstalk and other managed compute options

---

## The compute decision

"Compute" means anything that runs code. Every application needs compute. The question is how much control and responsibility you want for the underlying infrastructure.

At one end: EC2, where you manage the operating system and everything on top of it. At the other end: Lambda, where you only write the function code and AWS manages everything else. Most services fall somewhere in between.

---

## Amazon EC2 — Elastic Compute Cloud

EC2 gives you a virtual server in the cloud. You choose the operating system, instance type, storage, and network configuration. You can SSH into it, install software, configure the OS, and do everything you'd do with a physical server — but without buying hardware.

**When to use EC2**: Long-running applications, applications that need specific OS configurations, workloads requiring persistent server processes, applications you're lifting and shifting from on-premises.

### EC2 instance types

Instance types determine the CPU, memory, storage, and networking available to your instance. They're organized into families based on their purpose:

| Family | Optimized for | Example use case |
|--------|--------------|-----------------|
| General Purpose (t3, m6) | Balanced CPU/memory | Web servers, development environments |
| Compute Optimized (c6) | High CPU performance | Video encoding, HPC, batch processing |
| Memory Optimized (r6, x2) | Large RAM | In-memory databases, real-time analytics |
| Storage Optimized (i3, d3) | High disk I/O | Data warehouses, distributed file systems |
| Accelerated Computing (p4, g5) | GPUs | Machine learning training, graphics |

For the exam, you don't need to memorize instance type codes — but know the families and what they're optimized for.

### EC2 pricing models

This is heavily tested. Choosing the right pricing model can save 60–90% compared to On-Demand.

**On-Demand**
You pay by the hour (or second) with no commitment. Start and stop anytime. Most expensive per hour, but no upfront cost or minimum.
*Use when*: Unpredictable workloads, short-term projects, development/testing, applications you're still sizing.

**Reserved Instances (RIs)**
You commit to using a specific instance type in a Region for 1 or 3 years. In exchange, you get up to 72% discount over On-Demand.
- *Standard RI*: Highest discount, but you're locked into the exact instance type and Region.
- *Convertible RI*: Slightly lower discount, but you can change instance type, OS, or scope during the term.
*Use when*: Steady-state workloads you'll run continuously — production databases, stable web apps.

**Savings Plans**
A flexible alternative to Reserved Instances. You commit to a spending amount ($/hour) for 1 or 3 years, not a specific instance type. The discount applies automatically to any instance that matches your usage.
- *Compute Savings Plans*: Apply to EC2, Lambda, and Fargate.
- *EC2 Instance Savings Plans*: Apply to a specific instance family in a Region. Higher discount.
*Use when*: You want RI-level discounts but with more flexibility to change instance types.

**Spot Instances**
You bid on unused EC2 capacity. Prices fluctuate but can be up to 90% off On-Demand. However, AWS can terminate your Spot Instance with 2-minute notice when the capacity is needed elsewhere.
*Use when*: Workloads that can handle interruption — batch processing, data analysis, video rendering, CI/CD pipelines.

**Dedicated Hosts**
A physical server fully dedicated to your use. No other AWS customers share the hardware. Required for certain software licenses (Oracle, SQL Server) that are tied to physical cores.
*Use when*: Compliance requirements that prohibit shared hardware, or BYOL licensing that requires physical core counts.

---

## AWS Lambda

Lambda is a serverless compute service. You write a function — a block of code that performs a task — and AWS runs it when triggered. You never see or manage a server.

**How it works**:
1. An event triggers your function (an HTTP request, a file uploaded to S3, a message in a queue, a scheduled timer)
2. Lambda runs your code in a managed environment
3. Lambda scales automatically — one invocation or one million, the same code runs
4. You pay only for the time your code runs, measured in milliseconds

**When to use Lambda**:
- Event-driven processing (process an image whenever it's uploaded to S3)
- Backend APIs (handle HTTP requests via API Gateway)
- Scheduled tasks (run a cleanup job every night at midnight)
- Data transformation (process records from a database stream)
- Small, short-lived tasks

**When not to use Lambda**:
- Tasks that run longer than 15 minutes (Lambda's maximum timeout)
- Applications that need persistent connections or server state
- Workloads with very consistent, high-volume traffic (Reserved EC2 may be cheaper)

**Key Lambda concept**: You don't manage servers, patching, or scaling. If 10,000 requests come in simultaneously, Lambda spins up 10,000 concurrent executions automatically — no configuration needed.

---

## Containers

Containers package an application and its dependencies together in a portable unit. A container runs the same way regardless of where it's deployed — on a developer's laptop, a test server, or production. This solves the "it works on my machine" problem.

**Docker** is the standard container format. An image is the blueprint; a container is a running instance of that image.

### Amazon ECS — Elastic Container Service

ECS is AWS's managed container orchestration service. You define container tasks (which image to run, how much CPU/memory, networking), and ECS runs and manages them across a cluster of servers.

ECS works with both EC2 instances (you manage the underlying servers) and Fargate (AWS manages the servers).

### Amazon EKS — Elastic Kubernetes Service

EKS is managed Kubernetes. Kubernetes is the dominant open-source container orchestration system. If your team already uses Kubernetes and you're moving to AWS, EKS lets you run standard Kubernetes without managing the Kubernetes control plane.

### AWS Fargate

Fargate is the serverless compute option for containers. Works with both ECS and EKS. Instead of provisioning EC2 instances to run your containers, Fargate manages the underlying infrastructure automatically — you just define the container and its requirements.

**EC2 vs Fargate for containers**: With EC2-backed ECS, you manage the underlying instances — patching, scaling, right-sizing. With Fargate, you specify the container specs and AWS handles everything else. Fargate is simpler; EC2 gives more control and can be more cost-effective at scale.

---

## Other compute services

### AWS Elastic Beanstalk

Already covered in Task 3.1. Upload your application code, choose a platform (Python, Node.js, Java, etc.), and Beanstalk provisions and manages the EC2 instances, load balancer, and auto-scaling. The fastest way to deploy a traditional web application without managing infrastructure.

### AWS Batch

Batch manages and runs hundreds of thousands of batch computing jobs. You define jobs (Docker containers or scripts), and Batch handles provisioning the right amount of compute capacity, queuing, and execution — scaling from a handful to millions of jobs.

*Use when*: Running scientific simulations, processing large datasets, financial risk calculations, anything with a large number of independent parallel tasks.

### Amazon Lightsail

Lightsail is the simplest entry point to AWS. It bundles compute, storage, and networking into one fixed-price product. Lower flexibility than EC2, but much easier to get started.

*Use when*: Simple websites, WordPress hosting, small applications, developers who don't need EC2's flexibility and want a predictable monthly cost.

---

## Choosing the right compute service

| Scenario | Service |
|----------|---------|
| Traditional app, need OS control | EC2 |
| Event-driven functions, short tasks | Lambda |
| Run Docker containers, manage servers | ECS on EC2 |
| Run Docker containers, no server management | Fargate |
| Already using Kubernetes | EKS |
| Deploy a web app without managing servers | Elastic Beanstalk |
| Large-scale batch/parallel jobs | AWS Batch |
| Simple website with fixed monthly cost | Lightsail |

---

## Practice questions

**Q1.** A company runs an e-commerce website with stable, predictable traffic throughout the year. They've been using On-Demand EC2 instances for 18 months. What change would most reduce their compute costs?

A) Switch to Spot Instances  
B) Purchase 1-year Reserved Instances  
C) Move to Elastic Beanstalk  
D) Enable Auto Scaling

**Answer: B** — Stable, predictable traffic is the ideal use case for Reserved Instances. They provide up to 72% discount for a 1-year commitment. Spot Instances can be terminated with 2 minutes notice — not suitable for customer-facing e-commerce. Beanstalk and Auto Scaling are deployment/scaling features that don't change the pricing model.

---

**Q2.** A developer needs to process uploaded images by creating thumbnail versions. When a user uploads a photo to S3, the thumbnail should be created automatically. Which compute service is best suited for this?

A) EC2  
B) Elastic Beanstalk  
C) AWS Lambda  
D) Amazon ECS

**Answer: C** — Lambda is ideal for event-driven tasks. S3 can trigger a Lambda function automatically when a new object is uploaded. The function runs, creates the thumbnail, saves it, and exits. You pay only for the milliseconds it runs. EC2 would be constantly running and idle between uploads. Beanstalk manages web servers, not event processing.

---

**Q3.** A company runs batch genomics analyses that process thousands of DNA samples in parallel. Each job runs for 30 minutes. The jobs can be queued and run overnight. Which compute option provides the most cost-effective solution?

A) On-Demand EC2  
B) Reserved EC2 instances  
C) Spot Instances via AWS Batch  
D) AWS Lambda

**Answer: C** — Batch jobs that can tolerate interruption and restart are perfect for Spot Instances, which can be 90% cheaper than On-Demand. AWS Batch manages the queue, provisioning, and scheduling automatically. Lambda has a 15-minute maximum timeout, so 30-minute jobs can't run there. On-Demand is most expensive. Reserved would require maintaining capacity continuously.

---

**Next:** [Task 3.4 — Database Services](./Task%20Statement%203.4-Identify%20AWS%20database%20services.md)
