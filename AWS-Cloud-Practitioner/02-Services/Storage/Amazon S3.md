# Amazon S3 (Simple Storage Service)

## What you'll learn
- What object storage is and how S3 works
- S3 storage classes and when to use each
- S3 security, versioning, and lifecycle policies
- Key exam facts

---

## What is Amazon S3?

Amazon Simple Storage Service (S3) is a **fully managed object storage service** that stores data as objects within containers called buckets. It provides virtually unlimited storage with 99.999999999% (11 nines) durability — the highest durability of any AWS storage service.

---

## Key concepts

**Bucket** — A container for objects. Bucket names are globally unique across all AWS accounts. A bucket lives in one AWS Region.

**Object** — Any file stored in S3 (image, video, document, backup, log file, etc.), plus metadata. Objects can be 0 bytes to 5 TB.

**Key** — The unique identifier for an object within a bucket (essentially the file path: `photos/2024/vacation.jpg`).

---

## S3 storage classes

S3 offers multiple storage classes for different access frequencies and cost profiles:

| Storage class | Use case | Cost | Retrieval |
|--------------|---------|------|----------|
| S3 Standard | Frequently accessed data | Higher | Immediate |
| S3 Standard-IA | Infrequently accessed, needs fast retrieval | Lower storage, retrieval fee | Immediate |
| S3 One Zone-IA | Infrequent access, non-critical (one AZ only) | Lower | Immediate |
| S3 Glacier Instant Retrieval | Archive with millisecond access | Very low | Immediate |
| S3 Glacier Flexible Retrieval | Archive, rare access | Ultra-low | 1–5 min to 12 hr |
| S3 Glacier Deep Archive | Long-term compliance archive, rarely accessed | Lowest | 12–48 hours |
| S3 Intelligent-Tiering | Unknown or changing access pattern | Monitoring fee | Immediate |

**S3 Intelligent-Tiering** automatically moves objects between tiers based on actual access patterns — no manual tiering needed.

---

## Lifecycle policies

S3 lifecycle policies automatically transition objects between storage classes (or delete them) based on age:

Example: "Move objects to Standard-IA after 30 days, move to Glacier after 90 days, delete after 7 years."

This reduces cost without manual management.

---

## Key features

**Versioning** — Keep multiple versions of an object. Protects against accidental deletion or overwrite — you can restore previous versions.

**Replication** — Cross-Region Replication (CRR) automatically copies objects to a bucket in another Region. Same-Region Replication (SRR) copies within the same Region.

**Static website hosting** — Serve a static website (HTML, CSS, JS) directly from an S3 bucket — no web server needed.

**Event notifications** — Trigger Lambda, SQS, or SNS when objects are created or deleted.

**S3 Block Public Access** — Account-level setting that prevents any bucket from becoming publicly accessible (even if a bucket policy tries to allow it).

---

## S3 security

- **Bucket policies**: JSON policies attached to the bucket
- **IAM policies**: Grant IAM users/roles access to S3 operations
- **Encryption at rest**: SSE-S3 (AWS managed keys), SSE-KMS (your KMS keys), SSE-C (your keys)
- **Encryption in transit**: HTTPS (TLS)

---

## When to use it

**Data lake** — Store raw data (logs, sensor data, files) for analytics with Athena, EMR, or Redshift Spectrum.

**Static website** — Host HTML/JS/CSS files with global delivery via CloudFront.

**Application storage** — Store user-uploaded content (photos, documents, videos).

**Backup and archive** — Store database backups, system backups, and compliance archives.

---

## Exam focus

- S3 = **object storage** — unlimited, durable, highly available
- **11 nines durability** (99.999999999%)
- Storage classes: Standard → Standard-IA → Glacier Instant → Glacier Flexible → Glacier Deep Archive (cost decreases, retrieval time increases)
- **Lifecycle policies** = automatically transition objects between storage classes
- **Versioning** = keep multiple versions of objects
- Use when exam describes: "store files," "data lake," "static website hosting," "object storage," "S3 bucket"

---

## Practice questions

**Q1.** A company stores application logs in Amazon S3. Logs are accessed frequently in the first 30 days, rarely between 30–90 days, and must be retained for 7 years for compliance. Which S3 feature automates cost-efficient storage management for this pattern?

A) S3 Versioning  
B) S3 Cross-Region Replication  
C) S3 Lifecycle Policy  
D) S3 Block Public Access

**Answer: C** — A lifecycle policy can automatically transition objects: Standard (0–30 days) → Standard-IA (30–90 days) → Glacier Deep Archive (90 days–7 years) → delete (after 7 years). This automates cost optimization based on the access pattern. Versioning keeps multiple versions but doesn't move data between storage classes. CRR copies to another Region. Block Public Access controls public access permissions.

**Q2.** A media company needs to archive completed video projects. The files will rarely be accessed but must be retrievable within 12 hours when needed. Which S3 storage class provides the lowest cost for this use case?

A) S3 Standard  
B) S3 Standard-IA  
C) S3 Glacier Flexible Retrieval  
D) S3 Glacier Deep Archive

**Answer: D** — S3 Glacier Deep Archive offers the lowest storage cost for long-term archival. Retrieval takes 12–48 hours, which meets the "within 12 hours" requirement (using Bulk retrieval). Standard and Standard-IA cost more and are designed for more frequent access. Glacier Flexible Retrieval is cheaper than Standard-IA but more expensive than Deep Archive.
