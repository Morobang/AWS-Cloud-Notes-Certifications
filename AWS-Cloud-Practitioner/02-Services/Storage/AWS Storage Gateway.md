# AWS Storage Gateway

## What you'll learn
- What Storage Gateway is and the hybrid storage problem it solves
- The three gateway types
- Key exam facts

---

## What is AWS Storage Gateway?

AWS Storage Gateway is a **hybrid cloud storage service that provides on-premises applications with access to AWS cloud storage**. It connects your on-premises environment to S3, EBS, and tape-compatible storage — bridging the gap between your data center and the AWS cloud.

---

## The problem it solves

Organizations with on-premises infrastructure often want the benefits of cloud storage (scalability, durability, cost) but can't immediately move everything to the cloud. Applications on-premises may be designed to write to local storage (NFS, SMB, iSCSI, or tape). Storage Gateway lets those applications write to familiar interfaces while actually storing data in AWS.

---

## Three gateway types

**S3 File Gateway**
- Presents an NFS or SMB interface to on-premises clients
- Files written to the gateway are stored as objects in Amazon S3
- Recent files are cached locally at the gateway for low-latency access
- Use when: On-premises applications need to store files in S3 using existing NFS/SMB protocols; backup to S3

**Volume Gateway**
- Presents iSCSI block storage volumes to on-premises applications
- Two modes:
  - **Cached volumes**: Primary data stored in S3; recently accessed data cached on-premises
  - **Stored volumes**: All data stored on-premises; asynchronous backup to S3 (EBS snapshots)
- Use when: On-premises applications need block storage with cloud backup

**Tape Gateway (Virtual Tape Library)**
- Presents a virtual tape library (VTL) to on-premises backup applications
- Backup software writes to "virtual tapes" which are stored in S3 and can be archived to S3 Glacier
- Compatible with common backup software (Veeam, Backup Exec, NetBackup, etc.)
- Use when: Replacing physical tape backup infrastructure with cloud-backed virtual tapes

---

## Storage Gateway gateway types summary

| Gateway type | Interface | Where data lives | Use case |
|-------------|----------|-----------------|---------|
| S3 File Gateway | NFS / SMB | Amazon S3 | File backup to S3, NFS/SMB to S3 |
| Volume Gateway | iSCSI | S3 (EBS snapshots) | Block storage with cloud backup |
| Tape Gateway | Virtual Tape Library | S3 / S3 Glacier | Replace physical tape backup |

---

## When to use it

**Backup to S3** — On-premises servers send backups via NFS/SMB to the File Gateway, which stores them in S3 — eliminating physical backup infrastructure.

**Replace tape backups** — Use Tape Gateway with existing backup software to store backups on virtual tapes in S3 and Glacier instead of physical tape.

**Hybrid applications** — Applications that can't move to the cloud use Storage Gateway to store data in AWS while appearing to use local storage.

---

## Exam focus

- Storage Gateway = **hybrid storage bridge** between on-premises and AWS
- **S3 File Gateway** = NFS/SMB → S3 objects
- **Volume Gateway** = iSCSI block storage → S3/EBS snapshots
- **Tape Gateway** = virtual tape library → S3/S3 Glacier
- Use when exam describes: "connect on-premises to AWS storage," "backup to S3 via NFS," "replace tape backup with cloud," "hybrid storage"

---

## Practice questions

**Q1.** A company's on-premises backup software writes backups to a physical tape library. They want to replace the tapes with cloud storage, while keeping their existing backup software and workflows unchanged. Which AWS Storage Gateway type enables this?

A) S3 File Gateway  
B) Volume Gateway  
C) Tape Gateway  
D) AWS DataSync

**Answer: C** — Tape Gateway presents a virtual tape library to the backup software using the same iSCSI-based VTL interface as physical tape libraries. The backup software operates unchanged while virtual tapes are stored in S3 and archived to S3 Glacier. S3 File Gateway presents NFS/SMB (file protocol, not tape). Volume Gateway presents iSCSI block volumes, not a tape library interface. DataSync transfers files but doesn't emulate a tape library.
