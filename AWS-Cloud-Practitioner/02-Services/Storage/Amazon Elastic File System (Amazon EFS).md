# Amazon Elastic File System (Amazon EFS)

## What you'll learn
- What EFS is and how it differs from EBS
- NFS shared file system concepts
- EFS storage classes
- Key exam facts

---

## What is Amazon EFS?

Amazon Elastic File System (EFS) is a **fully managed, serverless NFS file system** that can be mounted by multiple EC2 instances simultaneously. Unlike EBS (which attaches to one instance), EFS is a shared file system that many instances can read and write concurrently — like a network drive that all servers on a network can access.

---

## Key characteristics

- **Shared access**: Multiple EC2 instances — even in different AZs within a Region — can mount and access the same EFS file system at the same time
- **Fully managed**: AWS handles the infrastructure, replication, and patching
- **Automatic scaling**: Grows and shrinks automatically as you add or remove files — no capacity planning required
- **NFS protocol**: Instances mount EFS using the Network File System (NFS) protocol

---

## EFS storage classes

| Class | Use case | Cost |
|-------|---------|------|
| EFS Standard | Frequently accessed shared files | Higher |
| EFS Standard-IA | Infrequently accessed shared files | Lower (retrieval fee) |

EFS Intelligent-Tiering automatically moves files between Standard and Standard-IA based on access patterns.

---

## EFS vs EBS

| | Amazon EFS | Amazon EBS |
|-|-----------|-----------|
| Access | Multiple EC2 instances simultaneously | One EC2 instance at a time |
| Protocol | NFS | Block device |
| Scaling | Automatic — grows/shrinks | Fixed size (resize manually) |
| Availability | Multi-AZ within a Region | Single AZ |
| Use case | Shared application files, CMS, home directories | Single instance OS, databases |
| OS support | Linux only | Linux and Windows |

---

## EFS vs FSx

EFS is for Linux workloads using NFS. For Windows workloads (SMB protocol) or specialized high-performance file systems, use Amazon FSx.

---

## When to use it

**Content management systems** — Multiple web servers share a common file system for media uploads (all servers see the same files).

**Home directories** — Users' home directories on a Linux-based system accessible from any instance.

**Data science / analytics** — Shared training data accessible from multiple EC2 instances running ML workloads.

**Lift-and-shift applications** — Applications that already use NFS file shares can mount EFS the same way.

---

## Exam focus

- EFS = **shared NFS file system** — mounted by multiple EC2 instances simultaneously
- **Linux only** (NFS protocol)
- **Automatic scaling** — no capacity planning; grows/shrinks automatically
- Multi-AZ — data is replicated across AZs in a Region
- Key differentiator from EBS: EFS = shared, multi-instance; EBS = single instance
- Use when exam describes: "shared storage across multiple EC2 instances," "NFS," "Linux shared file system"

---

## Practice questions

**Q1.** A company has a fleet of 20 Linux web servers that need access to a shared pool of uploaded user files. All servers must be able to read and write files simultaneously. Which AWS storage service is most appropriate?

A) Amazon EBS  
B) Amazon S3  
C) Amazon EFS  
D) Amazon FSx for Windows

**Answer: C** — EFS provides a shared NFS file system that all 20 Linux EC2 instances can mount simultaneously. Each instance can read and write files, and all instances see the same data. EBS attaches to one instance at a time. S3 is object storage accessed via API — not a mountable file system for application use. FSx for Windows uses SMB — not appropriate for Linux servers.
