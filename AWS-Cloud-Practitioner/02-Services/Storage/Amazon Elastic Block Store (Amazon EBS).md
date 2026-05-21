# Amazon Elastic Block Store (Amazon EBS)

## What you'll learn
- What EBS is and how it differs from S3 and EFS
- EBS volume types
- Snapshots and how they work
- Key exam facts

---

## What is Amazon EBS?

Amazon Elastic Block Store (EBS) is a **block storage service that provides persistent storage volumes for EC2 instances**. Think of an EBS volume as a network-attached hard drive that you attach to an EC2 instance — it persists independently of the instance's lifecycle.

When you stop or terminate an EC2 instance, the data on an EBS volume can persist (if configured to do so).

---

## Key characteristics

- **Attached to one EC2 instance at a time**: A standard EBS volume can only be attached to one EC2 instance at a time (in the same AZ). Multi-Attach is available for certain volume types.
- **Availability Zone-specific**: An EBS volume exists in one AZ. To use it with an EC2 instance, both must be in the same AZ.
- **Persistent**: Data persists when the instance is stopped or rebooted
- **Network-attached**: Data is transmitted over the network, not via a physical cable

---

## EBS volume types

| Type | Technology | Use case | Key metric |
|------|-----------|---------|-----------|
| gp3 / gp2 | SSD | General purpose — boot volumes, dev environments | Balanced IOPS/throughput |
| io2 / io1 | SSD | High-performance databases (Oracle, SQL Server) | Highest IOPS, consistent performance |
| st1 | HDD | Big data, log processing, sequential throughput | High throughput |
| sc1 | HDD | Infrequently accessed, lowest cost block storage | Lowest cost |

**gp3** is the default and most common choice. **io2** is for databases requiring consistent, high IOPS.

---

## EBS Snapshots

A **snapshot** is a point-in-time backup of an EBS volume stored in Amazon S3 (managed by AWS):
- Snapshots are incremental: after the first full snapshot, subsequent snapshots only store changed blocks
- Snapshots can be used to create new EBS volumes (restore from backup)
- Snapshots can be copied to other Regions (for disaster recovery)
- Used to create an AMI (Amazon Machine Image) that captures the entire EC2 instance state

---

## EBS vs S3 vs EFS

| | Amazon EBS | Amazon S3 | Amazon EFS |
|-|-----------|----------|-----------|
| Type | Block | Object | File |
| Access | One EC2 instance at a time | Via HTTP API, any client | Multiple EC2 instances simultaneously |
| Protocol | Block device (ext4, NTFS) | REST API | NFS |
| Use case | OS disk, database storage | Files, backups, web assets | Shared application data |
| Persistence | Persists after instance stop | Always persistent | Always persistent |

---

## When to use it

**EC2 boot volumes** — Every EC2 instance has an EBS volume as its root (boot) device.

**Database storage** — Databases (MySQL, PostgreSQL, SQL Server, Oracle) store their data files on EBS volumes.

**Application data** — Application servers that need a local filesystem with high IOPS.

---

## Exam focus

- EBS = **persistent block storage** for EC2 instances — like a hard drive attached over the network
- Attached to **one EC2 instance** at a time; must be in the **same AZ**
- **Snapshots** = point-in-time backups stored in S3, used for backups and copying across Regions
- **gp3** = general purpose SSD (most common); **io2** = high-performance databases
- Data persists after instance is stopped (unlike instance store, which is ephemeral)

---

## Practice questions

**Q1.** A company runs an RDS-like database on an EC2 instance. They need high-performance storage with consistent IOPS for the database files, and automatic daily backups with the ability to restore to a point in time. Which EBS features support this?

A) gp2 volume with S3 replication  
B) io2 volume with EBS snapshots  
C) st1 volume with AWS Backup  
D) sc1 volume with CloudTrail

**Answer: B** — io2 EBS volumes provide the highest consistent IOPS for demanding databases. EBS snapshots create point-in-time backups that can be used to restore the volume to any snapshot point. gp2 is general purpose — not optimized for high consistent IOPS. st1 is for sequential throughput (big data), not database IOPS. sc1 is the cheapest, lowest-performance HDD option.
