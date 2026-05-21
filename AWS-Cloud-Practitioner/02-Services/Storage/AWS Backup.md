# AWS Backup

## What you'll learn
- What AWS Backup is and what it centralizes
- Backup plans, vaults, and recovery points
- Key exam facts

---

## What is AWS Backup?

AWS Backup is a **fully managed, centralized backup service** that automates and manages backups across multiple AWS services from a single place. Instead of configuring backup settings separately for each service (RDS snapshots, EBS snapshots, DynamoDB backups, EFS backups), you define backup policies in AWS Backup and it handles everything.

---

## The problem without AWS Backup

Different AWS services have different backup mechanisms:
- RDS: automated snapshots (up to 35 days)
- EBS: manual or automated snapshots
- DynamoDB: on-demand backups and PITR
- EFS: AWS Backup integration
- S3: versioning and replication
- EC2: AMIs and EBS snapshots

Without AWS Backup, configuring, monitoring, and auditing backups across all these services is complex and inconsistent.

---

## Key concepts

**Backup plan** — A policy that defines:
- **Backup frequency**: How often to back up (hourly, daily, weekly)
- **Backup window**: When the backup should run
- **Retention period**: How long to keep the backup
- **Lifecycle rule**: When to transition backups to cold storage

**Backup vault** — A container that stores backup recovery points. You can have multiple vaults (e.g., separate vaults for production and development). Vaults can be encrypted with KMS keys.

**Recovery point** — A snapshot/backup of a resource at a specific point in time. Used to restore the resource.

**Vault Lock** — Write-once, read-many (WORM) compliance for backup vaults. Once locked, backups in the vault cannot be deleted before the retention period expires — even by an administrator.

---

## Supported services

AWS Backup supports:
- Amazon EBS (volumes)
- Amazon RDS (databases and Aurora clusters)
- Amazon DynamoDB (tables)
- Amazon EFS (file systems)
- Amazon FSx (file systems)
- Amazon S3 (buckets)
- AWS Storage Gateway (volumes)
- Amazon EC2 (instances via AMIs)
- VMware on-premises

---

## AWS Backup vs native backups

| | AWS Backup | Native service backups |
|-|-----------|----------------------|
| Coverage | Multiple services in one place | Per service only |
| Policy management | Centralized backup plans | Per-service configuration |
| Compliance | Audit and report across all resources | Per service |
| Cross-account | Yes | Limited |
| WORM compliance | Vault Lock | Varies per service |

---

## When to use it

**Compliance requirements** — Create a centralized backup policy that meets regulatory requirements across all AWS services.

**Cross-account backup** — Back up resources from multiple accounts into a central backup account for protection from account-level incidents.

**Audit and reporting** — Get a single view of backup activity and compliance across the organization.

---

## Exam focus

- AWS Backup = **centralized backup service** across EBS, RDS, DynamoDB, EFS, EC2, FSx, S3
- **Backup plan** = policy defining frequency, retention, and lifecycle
- **Backup vault** = storage container for recovery points; can be protected with Vault Lock (WORM)
- Use when exam describes: "centralized backup," "backup policy across services," "compliance backup retention"

---

## Practice questions

**Q1.** A company wants to enforce a policy that all RDS databases, EBS volumes, and DynamoDB tables are backed up daily and retained for 30 days, across all accounts in their organization. Which AWS service provides centralized management of this backup policy?

A) Amazon RDS automated backups  
B) AWS Backup  
C) AWS Storage Gateway  
D) Amazon S3 Lifecycle Policies

**Answer: B** — AWS Backup allows you to define a backup plan once and apply it to RDS, EBS, and DynamoDB across all organizational accounts. Native RDS backups only cover RDS. Storage Gateway is for hybrid storage. S3 lifecycle policies manage object storage transitions, not cross-service backup policies.
