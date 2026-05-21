# AWS Key Management Service (AWS KMS)

## What you'll learn
- What KMS does and why encryption keys need management
- Customer Managed Keys vs AWS Managed Keys
- How KMS integrates with AWS services
- Key exam facts

---

## What is AWS KMS?

AWS Key Management Service (KMS) is a **managed service for creating and controlling encryption keys**. It provides centralized key management for encrypting data across AWS services — S3, RDS, EBS, Lambda, and more.

KMS handles the operational complexity of key management: storage, rotation, access control, and auditing.

---

## Why key management matters

Encryption is only as secure as the key management. Questions to answer:
- Where is the encryption key stored?
- Who can use the key to decrypt data?
- When was the key last rotated?
- Who has been using the key?

KMS answers all of these.

---

## KMS Key types

**AWS Managed Keys** — Created and managed by AWS on behalf of AWS services. You don't manage them directly — they're used automatically when you enable encryption in services like S3 (SSE-S3) or RDS. Free.

**Customer Managed Keys (CMK)** — Keys you create, manage, and control:
- You define the key policy (who can use the key)
- You can enable automatic annual rotation
- You can disable or delete the key
- More control; small monthly cost per key

**AWS Owned Keys** — Used internally by AWS services; you can't see or manage them.

---

## Key policies

Every CMK has a **key policy** that defines who can use and manage the key. Combined with IAM policies, this gives granular control. By default, only the root account has access to a CMK.

---

## KMS integration with AWS services

KMS integrates with nearly every AWS service that supports encryption:

| Service | KMS use |
|---------|---------|
| S3 | Server-side encryption (SSE-KMS) |
| EBS | Encrypt EBS volumes |
| RDS | Encrypt database storage |
| Lambda | Encrypt environment variables |
| Secrets Manager | Encrypts secrets using KMS |
| CloudTrail | Encrypts log files |

When you enable encryption in these services, they call KMS to encrypt/decrypt data — you don't need to write encryption code.

---

## KMS vs CloudHSM

| | AWS KMS | AWS CloudHSM |
|-|---------|-------------|
| Key storage | AWS-managed HSMs | Dedicated HSM in your VPC |
| Control | Shared infrastructure | Single-tenant dedicated hardware |
| Compliance | FIPS 140-2 Level 2 | FIPS 140-2 Level 3 |
| Management | AWS manages HSM hardware | You manage the HSM |
| Best for | Most encryption use cases | Strict regulatory requirements, customer-owned keys |

---

## Exam focus

- KMS = **managed encryption key service** — create, control, and audit encryption keys
- **Customer Managed Keys (CMK)** = you create and manage; **AWS Managed Keys** = AWS manages for you
- Integrates with S3, RDS, EBS, Lambda, and most AWS services for at-rest encryption
- **CloudTrail logs all KMS key usage** — full audit trail of who used which key when
- Use when exam describes: "encrypt data at rest," "manage encryption keys," "KMS key for S3 encryption"

---

## Practice questions

**Q1.** A company wants to encrypt all data stored in their Amazon S3 buckets using keys they control. They need to be able to audit all key usage and restrict which IAM roles can decrypt the data. Which AWS service provides this capability?

A) AWS Secrets Manager  
B) AWS Certificate Manager  
C) AWS Key Management Service (KMS)  
D) AWS CloudHSM

**Answer: C** — KMS Customer Managed Keys allow the company to create keys with specific key policies (controlling who can decrypt), enable key usage logging via CloudTrail, and use the keys for S3 SSE-KMS encryption. Secrets Manager stores secrets (credentials, API keys) — not a general key management service. ACM manages SSL/TLS certificates. CloudHSM provides dedicated hardware HSMs — more appropriate when FIPS Level 3 is required.
