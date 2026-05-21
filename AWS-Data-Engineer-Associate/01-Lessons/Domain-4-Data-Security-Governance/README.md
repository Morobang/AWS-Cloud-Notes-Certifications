# Domain 4: Data Security and Governance

**Exam Weight: 18%** — ~12 questions

---

## What This Domain Covers

Securing data at rest and in transit, controlling who can access what, auditing access, and complying with data privacy regulations.

---

## The Three Pillars of Data Security

```
1. Authentication & Authorization  →  WHO can access WHAT data
2. Encryption                      →  Data is unreadable without the key
3. Auditing & Governance           →  TRACK who accessed what, when
```

---

## Authentication and Authorization

### IAM for Data Services

Every data service integrates with IAM. Key patterns:

**Glue service role**: The IAM role Glue assumes to read S3, write to S3, and write to CloudWatch. Must have permissions for:
- `s3:GetObject`, `s3:PutObject` on data buckets
- `glue:*` for Data Catalog access
- `cloudwatch:PutMetricData`, `logs:CreateLogGroup`

**Lambda execution role**: Lambda's IAM role that allows it to access other services (DynamoDB, S3, Kinesis, etc.)

**Redshift IAM role**: Allows Redshift to read from S3 for COPY commands.

### Lake Formation Permissions

Lake Formation adds a permissions layer ON TOP of IAM for data lake resources:

| IAM | Lake Formation |
|---|---|
| Coarse-grained (bucket/table level) | Fine-grained (column/row level) |
| S3 bucket policies | Table permissions in Data Catalog |
| Must manage per user/role | Centralized permission management |

**Column-level security**: Grant access to specific columns only.  
Example: analysts can query customer tables but not the `ssn` or `credit_card` columns.

**Row-level filtering**: Different users see different rows.  
Example: regional manager sees only rows where `region = 'ZA'`.

---

## Encryption

### Encryption at Rest

| Service | Encryption options |
|---|---|
| **S3** | SSE-S3 (AWS-managed), SSE-KMS (customer-managed key), SSE-C (customer-provided key), client-side |
| **Redshift** | Enabled at cluster creation — AES-256 with KMS |
| **DynamoDB** | Default encryption with AWS-owned key; optionally KMS customer-managed key |
| **RDS/Aurora** | Enable at creation — AES-256 with KMS |
| **Glue Data Catalog** | Encrypt at rest — metadata encrypted with KMS |

**SSE-KMS vs SSE-S3:**
- SSE-S3: AWS manages the key entirely — simpler, no audit trail
- SSE-KMS: You control the key in KMS — audit trail (CloudTrail logs key usage), can disable/rotate key

**For compliance**: Use SSE-KMS — you can prove who used the key and when, and you can revoke access by disabling the key.

### Encryption in Transit

- All AWS services use HTTPS/TLS by default
- Enforce HTTPS on S3 buckets with a bucket policy:
```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "Bool": {"aws:SecureTransport": "false"}
  }
}
```

---

## Data Masking and Tokenization

For PII (Personally Identifiable Information) protection:

| Technique | Description | Reversible? |
|---|---|---|
| **Masking** | Replace with fake data (`John Doe` → `REDACTED`) | No |
| **Tokenization** | Replace with a token (`SSN-12345`) stored in a secure vault | Yes (with vault access) |
| **Encryption** | Encrypt the value with a key | Yes (with key) |
| **Hashing** | One-way hash (SHA-256) | No |
| **Pseudonymization** | Replace identifier with a pseudonym | Yes (with mapping table) |

AWS Glue DataBrew has built-in PII detection and masking features.

---

## Auditing and Compliance

### CloudTrail for Data Access Auditing

**Data events** in CloudTrail (disabled by default, must enable):
- S3 object-level events (`GetObject`, `PutObject`, `DeleteObject`)
- DynamoDB item-level events
- Lambda function executions

Enable data events for compliance-sensitive buckets.

### AWS Macie

Automatically discovers PII in S3:
- Scans S3 buckets for names, SSNs, credit card numbers, passport numbers, etc.
- Creates findings you can review and alert on
- Integrates with Security Hub

### AWS Config Rules for Data Governance

Example compliance rules:
- `s3-bucket-server-side-encryption-enabled` — alert if any bucket lacks encryption
- `s3-bucket-public-read-prohibited` — alert if any bucket allows public read
- `dynamodb-pitr-enabled` — ensure point-in-time recovery is on

---

## Task Statements

| Task | Focus |
|---|---|
| [Task 4.1](./Task-4.1-Apply-Authentication-Authorization.md) | IAM, Lake Formation, resource policies |
| [Task 4.2](./Task-4.2-Ensure-Data-Encryption.md) | KMS, SSE options, encryption in transit |
| [Task 4.3](./Task-4.3-Comply-with-Data-Privacy.md) | GDPR, POPIA, PII discovery, data retention |

---

## Key Exam Scenarios

**"How do you ensure only certain columns of a table in the Data Catalog are accessible to analysts?"**  
→ AWS Lake Formation column-level permissions

**"A data engineer needs to query encrypted S3 data with Athena. How do you grant access without sharing the encryption key?"**  
→ Grant the IAM role attached to the Athena workgroup permission to use the KMS key (`kms:Decrypt`, `kms:GenerateDataKey`)

**"How do you detect if someone accidentally uploaded a file containing credit card numbers to S3?"**  
→ Enable Amazon Macie on the bucket

**"How do you ensure all S3 data writes use server-side encryption?"**  
→ S3 Bucket Policy with `Deny` on `s3:PutObject` if `s3:x-amz-server-side-encryption` header is missing
