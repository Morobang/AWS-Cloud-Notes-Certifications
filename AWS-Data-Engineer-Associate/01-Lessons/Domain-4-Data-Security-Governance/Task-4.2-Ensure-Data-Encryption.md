# Task 4.2: Ensure Data Encryption and Masking

## Learning Objectives
- Apply encryption at rest for S3, Redshift, DynamoDB, RDS, and Glue
- Enforce encryption in transit
- Understand KMS key types, key policies, and envelope encryption
- Implement data masking for PII protection

---

## Encryption at Rest — S3

S3 offers four server-side encryption (SSE) options:

| Option | Key management | Audit trail | Use when |
|---|---|---|---|
| **SSE-S3** | AWS manages key entirely | No | Basic encryption, simplest setup |
| **SSE-KMS** | KMS CMK (you control) | Yes (CloudTrail) | Compliance, need key audit or rotation |
| **DSSE-KMS** | Dual-layer KMS | Yes | Highest compliance (two independent encryption layers) |
| **SSE-C** | You provide the key per request | No | You want full key custody (unusual) |
| **Client-side** | You encrypt before upload | Depends on your logging | Application-managed encryption |

### SSE-KMS Deep Dive

When you `PUT` an object with `x-amz-server-side-encryption: aws:kms`:
1. S3 calls KMS to generate a **data encryption key (DEK)**
2. S3 uses the DEK to encrypt the object
3. S3 stores the encrypted DEK alongside the object
4. The plaintext DEK is discarded

When you `GET` the object:
1. S3 sees the encrypted DEK
2. S3 calls KMS to decrypt the DEK (KMS checks your permissions)
3. S3 uses the decrypted DEK to decrypt the object

**Every S3 object operation with SSE-KMS calls KMS.** At scale (millions of objects), watch for KMS API rate limits.

### Enforce Encryption on Upload

Bucket policy denying unencrypted uploads:
```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "StringNotEquals": {
      "s3:x-amz-server-side-encryption": "aws:kms"
    }
  }
}
```

Any `PutObject` request without the KMS encryption header is denied.

### S3 Default Encryption

S3 buckets can have a default encryption setting — all new objects are encrypted even if the requester doesn't specify it. Enforce this AND use a bucket policy to be doubly safe.

---

## AWS KMS — Key Management Service

### Key Types

| Key type | Who manages material | Cost | Use case |
|---|---|---|---|
| AWS-managed key (`aws/s3`, `aws/rds`) | AWS | Free | Default encryption — quick setup, limited control |
| Customer-managed key (CMK) | You (via KMS) | $1/month + per-API | Full control — custom policies, rotation, audit |
| Customer-owned key (external) | You import key material | $1/month + per-API | Must use existing hardware-generated key |
| AWS CloudHSM | Dedicated HSM hardware | High | FIPS 140-2 Level 3 compliance |

### KMS Key Policies

Key policies control who can use or manage a KMS key. Every KMS key has a key policy. IAM policies alone are not enough — the key policy must also grant access.

```json
{
  "Sid": "Allow data engineering role to use key",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::123456789:role/DataEngineerRole"
  },
  "Action": [
    "kms:Decrypt",
    "kms:GenerateDataKey",
    "kms:DescribeKey"
  ],
  "Resource": "*"
}
```

**To access an SSE-KMS-encrypted S3 object**, a principal needs BOTH:
- IAM policy: `s3:GetObject` on the bucket
- KMS key policy: `kms:Decrypt` on the key

### Key Rotation

Enable automatic annual key rotation for CMKs:
- KMS generates new key material each year
- Old material retained to decrypt data encrypted with it
- New material used for all new encryptions
- No data re-encryption required (AWS handles this transparently)

### Envelope Encryption

The pattern KMS uses:

```
KMS Master Key (CMK)
    ↓ encrypt
Data Encryption Key (DEK) — unique per object
    ↓ encrypt
Your actual data
```

The CMK never leaves KMS. Only the encrypted DEK is stored with the data. To decrypt, KMS decrypts the DEK, the DEK decrypts the data. This is why you can have billions of objects — only one CMK is needed, each object has its own DEK.

---

## Encryption at Rest — Other Services

### Amazon Redshift

Enabled at cluster creation — cannot add encryption to an existing unencrypted cluster without restoring a snapshot.

- Encryption uses AES-256
- Master key in KMS — Redshift generates a cluster encryption key, encrypts with KMS CMK
- **HSM integration**: Redshift can use CloudHSM for key storage (FIPS compliance)

### Amazon RDS / Aurora

Enable at creation — check "Enable encryption" and select a KMS key. Cannot encrypt an existing unencrypted instance — must:
1. Take a snapshot
2. Copy the snapshot with encryption enabled
3. Restore from the encrypted snapshot

Encrypted read replicas inherit the encryption setting from the primary.

### Amazon DynamoDB

All DynamoDB tables are encrypted at rest — you choose the key type:
- **AWS-owned key**: default, no charge, no control
- **AWS-managed key** (`aws/dynamodb`): free, basic audit
- **Customer-managed key (CMK)**: full control, $1/month

DynamoDB Streams also uses the same key as the table.

### AWS Glue Data Catalog

Encrypt the metadata catalog itself:
- Enable catalog encryption with a KMS key in Glue settings
- Encrypt connection passwords (JDBC credentials) separately

### AWS Glue ETL — Data in Transit

Glue ETL jobs transfer data between S3, databases, and JDBC sources. Enable encryption settings:
- **S3 encryption**: uses SSE-KMS for data written to S3
- **JDBC encryption**: encrypts JDBC connections
- **CloudWatch encryption**: encrypts job logs

---

## Encryption in Transit

All AWS service APIs use HTTPS/TLS 1.2+ by default. You cannot disable TLS on AWS service endpoints.

**What you need to enforce:**

1. **S3 — deny non-HTTPS requests** (bucket policy with `aws:SecureTransport: false` Deny)
2. **Redshift — require SSL for connections** (set `require_ssl=true` in parameter group)
3. **RDS — enforce SSL connections** (set `rds.force_ssl=1` for PostgreSQL, use SSL option group for MySQL)
4. **DynamoDB** — TLS enforced by default, no action needed

**In-transit encryption for EMR:**
Enable TLS for data in transit between cluster nodes (Spark shuffle data, HDFS data) — configure in EMR security configuration.

---

## Data Masking

### Masking Strategies

| Technique | Method | Reversible | When to use |
|---|---|---|---|
| **Static masking** | Replace PII in a copy of the data | No | Analytics environments — create a de-identified copy |
| **Dynamic masking** | Show masked data on query, keep original | No (original is intact) | Production — analysts see `***-****` for SSN |
| **Tokenization** | Replace with opaque token, map stored in vault | Yes (with vault) | Payment data (PCI-DSS), need to rejoin later |
| **Hashing** | SHA-256 hash of the value | No | Pseudonymization (same input → same hash, usable as JOIN key) |
| **Encryption** | Encrypt the field with a key | Yes (with key) | GDPR erasure — delete the key to "erase" data |
| **Nullification** | Set field to NULL | No | Hard delete in analytics environments |
| **Data generalization** | Replace exact value with a range (age 28 → 25-34) | No | Statistics without individual exposure |

### Glue DataBrew PII Detection

DataBrew can automatically detect and mask PII:
1. Run a **Profile job** to discover PII in datasets (credit cards, emails, SSNs, phone numbers)
2. Define a **Recipe** with masking steps (replace, hash, encrypt)
3. Run the Recipe to produce a de-identified version

### Redshift Dynamic Data Masking

Redshift supports column-level masking policies:
```sql
-- Create a masking policy for credit card numbers
CREATE MASKING POLICY mask_credit_card
WITH (credit_card_num varchar(20))
USING (
  CASE
    WHEN IS_MEMBER_OF('analysts') THEN 'XXXX-XXXX-XXXX-' || RIGHT(credit_card_num, 4)
    ELSE credit_card_num
  END
);

-- Attach to a column
ATTACH MASKING POLICY mask_credit_card
ON orders(credit_card_number)
TO ROLE analysts;
```

Analysts querying `orders` see `XXXX-XXXX-XXXX-1234`. Full numbers are only visible to roles not subject to the masking policy.

---

## GDPR and Right to Erasure

**The challenge**: GDPR requires you to delete a user's personal data on request. But S3 and data warehouses are designed for immutability — you can't easily delete one person's records from a 1 TB Parquet file.

**Cryptographic erasure approach**: Encrypt each user's PII with a user-specific KMS key. When the user requests erasure, delete the KMS key. All of their encrypted data is now unreadable — effectively erased without physically modifying files.

**Apache Iceberg approach**: Use Iceberg's row-level delete capability to physically remove rows matching the user, then compact files.

**Tokenization approach**: Store PII in a separate, dedicated system (DynamoDB). Replace PII in the data lake with tokens. Erasure = delete the token from DynamoDB. The lake retains the token but it's now meaningless.

---

## Exam Scenarios

**"A company must ensure that all S3 uploads use server-side encryption with a specific KMS key. Uploads without the KMS header must be rejected."**
→ S3 bucket policy: Deny `s3:PutObject` if `s3:x-amz-server-side-encryption` is not `aws:kms`, OR if `s3:x-amz-server-side-encryption-aws-kms-key-id` is not the specific key ARN

**"An Athena query against an SSE-KMS-encrypted S3 bucket returns AccessDenied."**
→ The IAM role used by Athena has `s3:GetObject` but is missing `kms:Decrypt` and `kms:GenerateDataKey` on the KMS key — update the key policy and IAM policy

**"A company needs to ensure analysts can query the orders table but see only the last 4 digits of credit card numbers."**
→ Redshift Dynamic Data Masking policy on the credit_card column

**"A GDPR erasure request requires deleting a specific user's data from a 10 TB Parquet data lake on S3."**
→ Cryptographic erasure (encrypt user data with user-specific KMS key, delete key) or Iceberg row-level deletes + file compaction
