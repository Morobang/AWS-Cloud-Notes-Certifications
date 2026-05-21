# Amazon EC2

## What you'll learn
- What EC2 is and how virtual servers work
- EC2 instance families and when to use each
- All five EC2 pricing models
- Key EC2 concepts for the exam

---

## What is Amazon EC2?

Amazon EC2 (Elastic Compute Cloud) provides **resizable virtual servers** in the cloud. An EC2 instance is a virtual machine — it has a CPU, memory, storage, and networking, just like a physical server. You choose the operating system, install software, configure it, and connect to it remotely.

EC2 gives you full control over the server environment. Unlike managed services where AWS handles the server, with EC2 you are responsible for the operating system, security patches, installed software, and configuration.

---

## Instance families

EC2 instances are grouped into families based on the ratio of CPU to memory and the underlying hardware type.

| Family | Optimized for | Example use |
|--------|--------------|-------------|
| **General Purpose** (t, m) | Balanced CPU/memory | Web servers, development environments |
| **Compute Optimized** (c) | High CPU relative to memory | Batch processing, gaming servers, ML inference |
| **Memory Optimized** (r, x) | High memory relative to CPU | In-memory databases, real-time big data analytics |
| **Storage Optimized** (i, d) | High I/O, local NVMe storage | NoSQL databases, data warehousing, log processing |
| **Accelerated Computing** (p, g, inf) | GPUs / dedicated hardware | ML training, video rendering, HPC |

---

## Five pricing models

### On-Demand
Pay by the second/hour with no commitment. Stop the instance and billing stops. Most flexible, highest per-unit cost.

*Use when*: Variable or unpredictable workloads, short-term projects, testing.

### Reserved Instances (RI)
Commit to use a specific instance type in a specific region for 1 or 3 years. Discounts of 30–72% over On-Demand.

*Use when*: Steady-state workloads you know will run for 1+ years. Can be Standard (least flexible) or Convertible (can change instance family).

### Savings Plans
Commit to a minimum $/hour of compute spend for 1 or 3 years. Applies automatically across EC2, Lambda, and Fargate. More flexible than RIs.

*Use when*: You have predictable compute spend but want flexibility across instance types, regions, or compute services.

### Spot Instances
Use AWS spare EC2 capacity at discounts of up to 90% over On-Demand. AWS can reclaim Spot capacity with a 2-minute warning.

*Use when*: Fault-tolerant, flexible workloads that can be interrupted — batch processing, data analysis, stateless web servers.

### Dedicated Hosts
A physical server dedicated entirely to your use. You control instance placement on the hardware.

*Use when*: Compliance requirements that prohibit sharing physical servers with other customers; bring-your-own software licenses (BYOL) tied to physical cores or sockets.

---

## Key concepts

**AMI (Amazon Machine Image)** — A template that defines the OS, application server, and applications for an instance. You launch instances from AMIs.

**Instance store** — Temporary storage physically attached to the host. Data is lost if the instance is stopped or terminated.

**EBS volume** — Persistent block storage attached to an EC2 instance. Data survives instance stop/start. Snapshots create backups in S3.

**Security group** — A virtual firewall controlling inbound and outbound traffic to the instance at the instance level.

**Elastic IP** — A static public IP address that can be re-mapped to a different instance if needed.

**User data** — A script that runs automatically when an instance first starts — used to bootstrap software installation and configuration.

---

## Exam focus

- EC2 = **virtual servers** — you control the OS and everything on top of it
- Instance families: **t/m** (general), **c** (compute), **r** (memory), **i** (storage), **p/g** (GPU)
- Five pricing models: **On-Demand, Reserved, Savings Plans, Spot, Dedicated**
- **Spot** = cheapest but can be interrupted; use for fault-tolerant batch workloads
- **Dedicated Hosts** = compliance and BYOL — the only option that gives you physical server isolation
- **AMI** = template for launching instances; **EBS** = persistent storage; **Security group** = instance firewall

---

## Practice questions

**Q1.** A media company runs video transcoding jobs that can be interrupted without data loss. They want the lowest possible compute cost. Which EC2 pricing model should they use?

A) On-Demand  
B) Reserved Instances  
C) Spot Instances  
D) Dedicated Hosts

**Answer: C** — Spot Instances offer up to 90% discount over On-Demand by using spare EC2 capacity. Video transcoding is a classic Spot use case: it's batch work that can checkpoint and resume if interrupted. On-Demand is most expensive. Reserved requires a commitment. Dedicated Hosts are for compliance/BYOL.

---

**Q2.** A company has a compliance requirement that their EC2 instances cannot share physical hardware with other AWS customers. Which pricing option satisfies this?

A) Reserved Instances  
B) Spot Instances  
C) Savings Plans  
D) Dedicated Hosts

**Answer: D** — Dedicated Hosts are physical servers dedicated exclusively to one customer. Reserved Instances, Spot, and Savings Plans all run on shared physical hardware (though isolated at the hypervisor level). Dedicated Hosts are the only option that provides physical server isolation.
