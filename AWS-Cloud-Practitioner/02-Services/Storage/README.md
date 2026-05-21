# Storage — Category Overview

AWS provides multiple storage types — each designed for different access patterns, performance needs, and use cases. Choosing the right storage service is a common exam scenario.

---

## Services in this category

| Service | Type | Purpose |
|---------|------|---------|
| Amazon S3 | Object | Store and retrieve any amount of data via web API |
| Amazon EBS | Block | Persistent block storage volumes for EC2 instances |
| Amazon EFS | File | Managed NFS file system shared across multiple EC2 instances |
| Amazon FSx | File | Managed file systems: Windows, Lustre, NetApp, OpenZFS |
| Amazon S3 Glacier | Object (archive) | Long-term archival at very low cost |
| AWS Storage Gateway | Hybrid | Connect on-premises to S3, EBS, and tape-compatible storage |
| AWS Backup | Management | Centralized backup across AWS services |
| AWS Elastic Disaster Recovery | DR | Rapid recovery of on-premises or cloud workloads |

---

## Storage type quick reference

| Type | What it is | AWS service |
|------|-----------|-------------|
| Object | Files stored as objects with metadata, accessed via API | S3, S3 Glacier |
| Block | Raw storage volumes, like a hard drive, attached to one server | EBS |
| File | Shared file system, mounted by multiple servers at once | EFS, FSx |

---

## Exam selection guide

| Scenario | Service |
|---------|---------|
| "Store images, videos, files via web API" | Amazon S3 |
| "Boot volume for EC2, database storage" | Amazon EBS |
| "Share files across multiple EC2 instances simultaneously" | Amazon EFS |
| "Long-term archival at lowest cost" | S3 Glacier |
| "Windows file server in AWS" | Amazon FSx for Windows |
| "High-performance storage for HPC/ML" | Amazon FSx for Lustre |
| "Connect on-premises to cloud storage" | AWS Storage Gateway |
| "Centralized backup for RDS, EBS, DynamoDB, EFS" | AWS Backup |
| "Disaster recovery — fast failover for on-premises servers" | AWS Elastic Disaster Recovery |
