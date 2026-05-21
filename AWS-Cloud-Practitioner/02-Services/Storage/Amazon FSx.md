# Amazon FSx

## What you'll learn
- What FSx is and the four file system types
- FSx for Windows vs FSx for Lustre
- Key exam facts

---

## What is Amazon FSx?

Amazon FSx is a **fully managed file storage service** that provides high-performance, feature-rich file systems built on popular open-source and commercial file system technologies. FSx handles the infrastructure — you get the file system you need without managing the underlying servers.

---

## Four FSx file systems

**Amazon FSx for Windows File Server**
- A fully managed Windows native file system using the SMB (Server Message Block) protocol
- Active Directory integrated — users authenticate with their AD credentials
- Supports Windows NTFS permissions, DFS (Distributed File System)
- Use when: Windows EC2 instances or on-premises Windows servers need a shared file system

**Amazon FSx for Lustre**
- A high-performance parallel file system for compute-intensive workloads
- Delivers hundreds of gigabytes per second of throughput with sub-millisecond latency
- Integrates with S3 — can use S3 as a data repository; files are transparently loaded from S3 on access
- Use when: HPC (high-performance computing), ML training, video processing, financial simulations

**Amazon FSx for NetApp ONTAP**
- Managed NetApp ONTAP file system — supports NFS, SMB, and iSCSI
- Ideal for organizations already using NetApp on-premises who want to migrate to AWS
- Supports NetApp data management features (snapshots, cloning, compression)
- Use when: Lift-and-shift of NetApp workloads to AWS

**Amazon FSx for OpenZFS**
- Managed OpenZFS file system — supports NFS
- Known for data management features, compression, and low latency
- Use when: Linux workloads that benefit from ZFS-specific features

---

## FSx for Windows vs Amazon EFS

| | FSx for Windows | Amazon EFS |
|-|----------------|-----------|
| Protocol | SMB | NFS |
| OS | Windows (also Linux clients) | Linux only |
| AD integration | Yes — native | No |
| Best for | Windows workloads | Linux workloads |

---

## FSx for Lustre vs EFS

| | FSx for Lustre | Amazon EFS |
|-|---------------|-----------|
| Performance | Extremely high (sub-ms, GB/s) | Standard NFS performance |
| Use case | HPC, ML, video processing | Shared web content, home directories |
| S3 integration | Yes — native | No |
| Cost | Higher | Lower |

---

## When to use it

**Windows shared drives** — Replace on-premises Windows file servers with FSx for Windows — AD-integrated SMB shares.

**HPC and ML training** — FSx for Lustre provides the throughput required for model training on large datasets and scientific simulations.

**NetApp migration** — Organizations using NetApp on-premises can migrate to FSx for NetApp ONTAP with minimal change.

---

## Exam focus

- FSx = **managed specialized file systems**
- **FSx for Windows** = SMB, Active Directory, Windows-native
- **FSx for Lustre** = high-performance, HPC, ML, integrates with S3
- Key differentiator from EFS: FSx for Windows = SMB/Windows; EFS = NFS/Linux; FSx for Lustre = ultra-high performance
- Use when exam describes: "Windows file server," "SMB shares," "HPC file system," "high-performance ML storage"

---

## Practice questions

**Q1.** A company uses Windows EC2 instances that need access to a shared SMB file system integrated with their Active Directory. Which AWS storage service meets this requirement?

A) Amazon EFS  
B) Amazon S3  
C) Amazon FSx for Windows File Server  
D) Amazon FSx for Lustre

**Answer: C** — FSx for Windows File Server is a managed Windows SMB file system with native Active Directory integration — exactly what Windows EC2 instances need for a shared file system. EFS uses NFS (Linux). S3 is object storage. FSx for Lustre is a high-performance Linux parallel file system — not SMB, not AD-integrated.
