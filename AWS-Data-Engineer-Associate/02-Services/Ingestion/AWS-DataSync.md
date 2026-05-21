# AWS DataSync

**Category**: Data Transfer and Migration  
**Exam weight**: Medium — know when DataSync fits vs DMS vs Snow Family vs Storage Gateway

---

## What Is It?

AWS DataSync is a fully managed data transfer service that automates moving large amounts of data between on-premises storage and AWS, or between AWS storage services.

DataSync is optimized for **file and object data** — not database migration (that's DMS).

---

## What DataSync Transfers

**Source → Destination combinations:**

| Source | Destination |
|---|---|
| On-premises NFS | Amazon S3 |
| On-premises SMB | Amazon S3 |
| On-premises HDFS | Amazon S3 or EFS |
| Amazon S3 | Amazon S3 (cross-Region, cross-account) |
| Amazon EFS | Amazon S3 or Amazon EFS |
| Amazon FSx | Amazon S3 or Amazon FSx |
| Google Cloud Storage | Amazon S3 |
| Azure Blob Storage | Amazon S3 |

---

## How DataSync Works

```
On-premises NAS / HDFS / NFS share
    ↓
DataSync Agent (VM deployed on-premises)
    ↓ TLS-encrypted transfer
DataSync Service (AWS-managed)
    ↓
Amazon S3 / EFS / FSx
```

**DataSync Agent**: A VMware or Hyper-V virtual appliance you deploy on-premises. It reads from the source, optimizes transfer, and sends data over Direct Connect or internet.

**For AWS-to-AWS transfers**: No agent needed — DataSync connects directly between AWS services.

---

## Key Features

**Automated scheduling**: Configure recurring transfers (hourly, daily, weekly) to keep data synchronized.

**Incremental transfers**: After the initial full transfer, subsequent runs only transfer changed or new files — efficient for ongoing sync.

**Data integrity verification**: DataSync performs checksums end-to-end to verify data transferred correctly.

**Bandwidth throttling**: Control how much network bandwidth DataSync uses — important when sharing a Direct Connect or internet connection with production traffic.

**Preserve metadata**: DataSync preserves POSIX permissions, timestamps, and NFS/SMB metadata when transferring files.

---

## Transfer Performance

DataSync uses multiple parallel connections and optimized protocols to achieve up to **10 Gbps** of throughput — much faster than manual `aws s3 cp` or rsync.

For very large datasets where network bandwidth isn't feasible, use AWS Snow Family instead.

---

## DataSync vs Other Transfer Services

| | AWS DataSync | AWS Snow Family | AWS DMS | AWS Storage Gateway |
|---|---|---|---|---|
| Data type | Files, objects | Any data | Database rows | Files, block, tape |
| Transfer method | Network (internet or Direct Connect) | Physical device | Network | Network (continuous) |
| Best for | Large file datasets, ongoing sync | Petabyte-scale, no network | Database migration/replication | Hybrid access (not bulk migration) |
| Ongoing sync | Yes | No (one-time) | Yes (CDC) | Yes (hybrid mount) |
| Agent required | Yes (on-premises) | No | Yes | Yes |

---

## Common DataSync Patterns

### Initial Data Lake Seeding

Move terabytes of on-premises files to S3 before starting a cloud migration:

```
On-premises file server (NFS)
    ↓ DataSync
Amazon S3 (initial seeding)
```

Faster and more reliable than manual upload.

### Ongoing Data Synchronization

A hybrid scenario where data is generated on-premises but analyzed in the cloud:

```
On-premises HDFS (Hadoop)
    ↓ DataSync (daily sync at 2 AM)
Amazon S3 (data lake)
    ↓
Athena / Glue (analytics)
```

### Cross-Region or Cross-Account S3 Replication

When S3 native CRR isn't sufficient (e.g., cross-account with filters, or third-party storage):

```
S3 bucket (Region A, Account A)
    ↓ DataSync
S3 bucket (Region B, Account B)
```

---

## Exam Scenarios

| Scenario | Answer |
|---|---|
| Transfer 500 TB of on-premises NFS data to S3 in 2 weeks | AWS DataSync (over Direct Connect) — if network is available |
| Transfer 500 TB of on-premises data to S3 but no available network bandwidth | AWS Snowball Edge |
| Ongoing daily sync of on-premises files to S3 | AWS DataSync with scheduled task |
| Migrate a MySQL database from on-premises to RDS | AWS DMS |
| Hybrid app needs on-premises servers to access S3 via NFS/SMB | AWS Storage Gateway (File Gateway) — not DataSync |
