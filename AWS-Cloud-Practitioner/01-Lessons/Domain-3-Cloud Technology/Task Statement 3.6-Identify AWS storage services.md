# Task Statement 3.6: Identify AWS Storage Services

## What you'll learn
- S3 — object storage and its storage class tiers
- EBS — block storage attached to EC2 instances
- EFS — shared file storage for multiple instances
- Archival storage with S3 Glacier
- Moving large amounts of data physically with the Snow Family
- Hybrid storage with Storage Gateway

---

## The three storage types

AWS storage falls into three fundamental categories:

**Object storage** (S3) — files stored as objects with unique keys. No file system. Accessed via HTTP. Scales to unlimited size. Best for files that are uploaded and retrieved as whole units: images, videos, documents, backups, website assets.

**Block storage** (EBS) — fixed-size blocks of data, like a hard drive attached to a server. The EC2 instance reads and writes to it like a local disk. High performance, low latency. Best for databases, operating system volumes, and applications that need a traditional disk.

**File storage** (EFS, FSx) — a shared file system that multiple servers can mount simultaneously, like a network drive. Best when multiple instances need to read and write the same files.

---

## Amazon S3 — Simple Storage Service

S3 is one of the most fundamental AWS services. It stores objects (files of any type, up to 5 TB each) in containers called buckets. Buckets have globally unique names across all of AWS.

**Key S3 properties**:
- **Durability**: 99.999999999% (11 nines). S3 stores three copies of every object across multiple Availability Zones.
- **Availability**: 99.99% for Standard storage class.
- **Scale**: Unlimited capacity. Buckets can hold any number of objects.
- **Access**: HTTP-based API. Not a file system — no folders (only key prefixes that look like paths).
- **Static website hosting**: S3 can serve static HTML, CSS, and JavaScript directly as a website.

### S3 storage classes

S3 automatically stores objects in Standard, but you can move objects to cheaper tiers based on how frequently they're accessed. This is storage lifecycle management.

| Storage class | Access pattern | Retrieval speed | Relative cost |
|--------------|---------------|----------------|---------------|
| **S3 Standard** | Frequently accessed | Milliseconds | High |
| **S3 Intelligent-Tiering** | Unknown or changing | Milliseconds | Medium (auto-optimizes) |
| **S3 Standard-IA** | Infrequently accessed | Milliseconds | Lower (+ retrieval fee) |
| **S3 One Zone-IA** | Infrequent, non-critical | Milliseconds | Lowest IA (single AZ only) |
| **S3 Glacier Instant Retrieval** | Rarely accessed, fast retrieval needed | Milliseconds | Very low |
| **S3 Glacier Flexible Retrieval** | Archive, hours acceptable | Minutes to hours | Very low |
| **S3 Glacier Deep Archive** | Long-term archive, rarely retrieved | 12 hours | Lowest |

**S3 Intelligent-Tiering** monitors access patterns and automatically moves objects between frequent and infrequent tiers. Use it when you're not sure how objects will be accessed — you pay a small monitoring fee but avoid retrieval fees.

**S3 Lifecycle policies** automatically transition objects between storage classes or delete them after a defined period. Example: move objects to Standard-IA after 30 days, Glacier after 90 days, delete after 7 years.

---

## Amazon EBS — Elastic Block Store

EBS provides persistent block storage volumes that attach to EC2 instances — like a hard drive for your virtual server. The data on an EBS volume persists even when the EC2 instance is stopped or terminated (depending on how you configure it).

**Key properties**:
- One EBS volume attaches to one EC2 instance at a time (with some exceptions for multi-attach)
- Volumes exist independently of instances — detach from one, reattach to another
- **Snapshots**: You can take point-in-time snapshots of EBS volumes to S3. Snapshots are used for backups and creating new volumes.
- Availability Zone–specific: an EBS volume lives in one AZ. To move it to another AZ, you snapshot it and create a new volume from the snapshot.

**EBS volume types**:
- **gp3 / gp2** (General Purpose SSD): Balanced performance. Default for most EC2 instances, operating system volumes.
- **io2 / io1** (Provisioned IOPS SSD): High-performance, low-latency. For I/O-intensive databases.
- **st1** (Throughput Optimized HDD): High-throughput sequential reads/writes. For big data, log processing.
- **sc1** (Cold HDD): Lowest cost. Infrequently accessed sequential data.

---

## Amazon EFS — Elastic File System

EFS is a managed NFS (Network File System) that multiple EC2 instances can mount simultaneously. Unlike EBS (one volume, one instance), EFS can be shared across hundreds or thousands of instances.

**Key properties**:
- **Elastic**: Grows and shrinks automatically as you add and remove files — no need to provision capacity.
- **Shared access**: Multiple instances in multiple AZs can read and write the same EFS filesystem concurrently.
- **Linux-only**: EFS uses NFS protocol, which is native to Linux. For Windows shared storage, use FSx for Windows File Server.

*Use when*: Content management systems that need shared storage across instances, web servers that share a common file system, machine learning training data that multiple instances access simultaneously.

---

## Amazon FSx

FSx provides managed file systems optimized for specific use cases:

**FSx for Windows File Server**: Fully managed Windows file server using the SMB protocol. For Windows applications that need a shared network drive — domain-joined, Active Directory integrated.

**FSx for Lustre**: High-performance parallel file system optimized for compute-intensive workloads. Used in HPC, machine learning, and video processing. Integrates with S3 — can read from and write back to S3 automatically.

---

## Amazon S3 Glacier

S3 Glacier is low-cost archival storage for data you rarely access but must retain. It's part of the S3 storage class family but deserves separate mention because it's tested specifically:

**Glacier Instant Retrieval**: Millisecond access at the price of Glacier. For rarely accessed but important data where fast retrieval is still required.

**Glacier Flexible Retrieval**: Retrieval in minutes to hours depending on the retrieval tier. For backup archives, media libraries.

**Glacier Deep Archive**: Cheapest storage in AWS. 12-hour retrieval time. For regulatory archives, historical records, data you might access once or twice in a decade.

---

## AWS Snow Family

The Snow Family solves a specific problem: how do you move petabytes of data to AWS when internet transfer would take months?

**AWS Snowcone**: Smallest device — 8 TB to 14 TB. Ruggedized, fits in a backpack. For edge computing and data collection in remote locations.

**AWS Snowball Edge**: Mid-sized device — 80 TB to 210 TB per device. Has local compute capability (runs EC2 AMIs and Lambda). Used in groups for larger transfers.

**AWS Snowmobile**: A semi-truck containing a massive storage container — up to 100 PB per truck. For exabyte-scale migrations.

**How it works**: AWS ships you the device, you connect it to your data center network, copy your data, and ship it back. AWS imports the data into S3. The alternative (internet transfer) for 100 TB of data at 1 Gbps takes about 10 days — and that's assuming perfect conditions.

*Use when*: Transferring more than 10 TB of data — at that point, Snowball is usually faster and cheaper than internet transfer.

---

## AWS Storage Gateway

Storage Gateway connects on-premises applications to AWS cloud storage. It's a hybrid storage service — it looks like a local storage device to your on-premises systems, but behind the scenes it's storing data in AWS.

**Types**:
- **File Gateway**: On-premises applications see an NFS or SMB share; data is stored as S3 objects. Good for file archiving and backup to the cloud.
- **Volume Gateway**: On-premises servers see an iSCSI block device; data is backed by S3 with local cache for low-latency access.
- **Tape Gateway**: On-premises backup applications see virtual tape drives; data goes to S3/Glacier. Replaces physical tape libraries with cloud-based archival.

*Use when*: You want to extend on-premises storage to the cloud, move backups to S3 transparently, or archive tape library data to Glacier.

---

## AWS Backup

AWS Backup is a centralized service for managing backup policies across AWS services — EBS, RDS, DynamoDB, EFS, S3, and others. Instead of configuring backups separately for each service, you define backup policies in one place and apply them across your entire infrastructure.

---

## Storage service selection

| Scenario | Service |
|----------|---------|
| Store files/objects, website assets, backups | S3 |
| Rarely accessed data, long-term archive | S3 Glacier |
| Block storage for EC2 instance | EBS |
| Shared file system across multiple Linux instances | EFS |
| Shared file system for Windows instances | FSx for Windows |
| High-performance HPC file system | FSx for Lustre |
| Move petabytes of data physically to AWS | Snow Family |
| Extend on-premises backup to cloud | Storage Gateway |

---

## Practice questions

**Q1.** A company stores compliance documents that must be retained for 7 years. Documents are almost never accessed after the first 90 days, but must be retrievable within 24 hours if requested. Which S3 storage class is most cost-effective?

A) S3 Standard  
B) S3 Standard-IA  
C) S3 Glacier Flexible Retrieval  
D) S3 Glacier Deep Archive

**Answer: C** — Glacier Flexible Retrieval offers retrieval in minutes to hours, meeting the 24-hour requirement, at a much lower cost than Standard or Standard-IA. Glacier Deep Archive has a 12-hour standard retrieval time and could work, but Flexible Retrieval is the safer answer for a 24-hour requirement. Standard and Standard-IA are more expensive for data that's rarely accessed.

---

**Q2.** A company needs to migrate 500 TB of video archive data to S3. Their internet connection is 1 Gbps. Internet transfer of 500 TB would take approximately 46 days. What is the better approach?

A) Buy more internet bandwidth  
B) Use AWS Direct Connect  
C) Use AWS Snowball  
D) Use S3 multipart upload

**Answer: C** — Snowball devices are shipped to you, loaded with data, and shipped back — typically completing in under 2 weeks regardless of data size. Direct Connect takes weeks to provision and would still require 46 days of transfer. Multipart upload and additional bandwidth don't change the fundamental time-to-transfer problem.

---

**Q3.** A media company runs 20 video encoding servers that all need read/write access to the same working directory of raw video files simultaneously. Which storage solution is appropriate?

A) One large EBS volume  
B) S3 with each server managing its own objects  
C) Amazon EFS  
D) Instance store on each server

**Answer: C** — EFS is a shared file system that multiple EC2 instances can mount and access concurrently. EBS can only attach to one instance at a time (with limited multi-attach exceptions). S3 doesn't provide a file system — it's object storage accessed via HTTP. Instance store is local to each individual instance and not shared.

---

**Next:** [Task 3.7 — AI/ML and Analytics Services](./Task%20Statement%203.7-Identify%20AWS%20AI%20and%20ML%20services%20and%20analytics%20services.md)
