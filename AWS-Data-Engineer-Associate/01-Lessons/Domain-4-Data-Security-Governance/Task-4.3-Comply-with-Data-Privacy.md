# Task 4.3: Comply with Data Privacy Regulations

## Learning Objectives
- Understand data privacy frameworks relevant to the DEA-C01 exam (GDPR, POPIA)
- Discover and classify PII in your data lake using Amazon Macie
- Implement data governance using AWS Lake Formation and AWS Glue
- Audit data access using CloudTrail and create compliance evidence

---

## Data Privacy Frameworks

### GDPR (General Data Protection Regulation)

European Union regulation governing how organizations handle personal data of EU residents.

**Key principles relevant to data engineers:**

| Principle | What it means for data engineering |
|---|---|
| **Lawful basis** | Personal data may only be processed for defined purposes |
| **Data minimization** | Collect only what you need — don't store PII you don't use |
| **Right to erasure (Art. 17)** | Users can request deletion of their data |
| **Right to access (Art. 15)** | Users can request a copy of their data |
| **Data portability** | Provide user data in a machine-readable format |
| **Privacy by design** | Build data protection into systems from the start |
| **Breach notification** | Report breaches to authorities within 72 hours |

### POPIA (Protection of Personal Information Act)

South Africa's data protection law — similar to GDPR in structure.

**Key requirements:**
- Appoint an Information Officer responsible for data compliance
- Define a lawful basis for processing personal information
- Data subjects can request access to, correction of, or deletion of their personal information
- Security measures must be in place for all personal information
- Cross-border data transfers require adequate protection at the destination

**For the exam**: POPIA is mentioned in the DEA-C01 guide alongside GDPR. Treat them similarly — both require PII discovery, access controls, data minimization, and the ability to fulfill erasure requests.

---

## PII Discovery with Amazon Macie

### What Macie Does

Macie uses ML and pattern matching to automatically discover PII and sensitive data in S3 buckets:
- Detects: names, email addresses, phone numbers, credit card numbers, SSNs, passport numbers, addresses
- Creates findings of two types:
  - **Policy findings**: bucket misconfigurations (public access, no encryption)
  - **Sensitive data findings**: PII detected in object content

### Running a Macie Discovery Job

1. Enable Macie in the account
2. Create a classification job — specify which S3 buckets to scan
3. Macie samples or fully scans objects
4. Findings appear in the Macie console and can be sent to Security Hub or EventBridge

**EventBridge integration**: Route Macie findings to Lambda for automated response:

```
Macie finding: S3 bucket contains credit card numbers
    ↓ EventBridge
    ↓ Lambda
    → Block public access on the bucket
    → Notify security team via SNS
    → Tag the bucket for review
```

### Macie Sensitive Data Discovery Results

For ongoing compliance, run scheduled discovery jobs:
- Monthly scans of the entire data lake
- Results stored in S3 as detailed findings
- Integrate with AWS Security Hub for a central view

---

## Data Cataloging for Compliance

### Glue Data Catalog Metadata

Add compliance metadata to your Glue Data Catalog tables:

```python
import boto3
glue = boto3.client('glue')

# Add PII classification to a table
glue.update_table(
    DatabaseName='customer_db',
    TableInput={
        'Name': 'customers',
        'Parameters': {
            'pii_classification': 'HIGH',
            'data_owner': 'privacy-team@company.com',
            'retention_days': '2555',  # 7 years
            'gdpr_subject': 'true',
            'last_pii_review': '2024-01-15'
        }
    }
)
```

This metadata is visible in the catalog and can be queried to find all tables with PII.

### Lake Formation Data Classification Tags

Use LF-TBAC tags to classify data sensitivity:

```
Tag key: data_classification
Tag values: public, internal, confidential, restricted

Tag key: pii_type
Tag values: none, pseudonymized, direct_pii
```

Apply tags at the database, table, or column level. Grant access based on tag values — users approved for `confidential` data automatically get access to all confidential-tagged tables.

---

## Auditing Data Access

### CloudTrail for Data Services

**Management events** (always logged):
- Who created/deleted a Glue job, table, database
- Who granted/revoked Lake Formation permissions
- Who created/modified S3 buckets

**Data events** (opt-in — disabled by default, must enable):
- S3 object-level: `GetObject`, `PutObject`, `DeleteObject` — who accessed which file
- DynamoDB item-level: `GetItem`, `PutItem`, `DeleteItem`
- Lambda invocations

Enable data events for compliance-sensitive buckets:
```python
import boto3
ct = boto3.client('cloudtrail')

ct.put_event_selectors(
    TrailName='my-trail',
    EventSelectors=[{
        'ReadWriteType': 'All',
        'IncludeManagementEvents': True,
        'DataResources': [
            {
                'Type': 'AWS::S3::Object',
                'Values': ['arn:aws:s3:::compliance-data-bucket/']
            },
            {
                'Type': 'AWS::DynamoDB::Table',
                'Values': ['arn:aws:dynamodb:us-east-1:123456789:table/customer_profiles']
            }
        ]
    }]
)
```

### Lake Formation Audit Logs

Lake Formation logs all permission grants and access events to CloudTrail. When a user queries an Athena table governed by Lake Formation, the event includes:
- Which principal made the request
- Which table/columns were accessed
- Whether the request was allowed or denied

### Athena Query History

Athena stores query execution history for 45 days. You can retrieve:
- Who ran which SQL query
- When it ran
- What data it scanned
- Whether it succeeded

Use this for compliance audits: "show me all queries run against the customer table in the last 30 days."

---

## AWS Audit Manager

Audit Manager automates evidence collection for compliance frameworks:

- **Built-in frameworks**: SOC2, PCI-DSS, HIPAA, GDPR, ISO 27001, NIST, FedRAMP
- Automatically collects evidence from CloudTrail, Config, Security Hub, IAM
- Organizes evidence by control (e.g., "encryption at rest" → collects evidence that S3 encryption is enabled)
- Produces assessment reports for auditors

For the exam: Audit Manager = automated compliance evidence collection. AWS Artifact = download AWS's own compliance reports (SOC, PCI, ISO). They're different.

---

## Data Retention and Deletion

### Retention Implementation

| Requirement | AWS implementation |
|---|---|
| Keep data for 7 years | S3 Lifecycle — no expiration for 7 years; Object Lock (WORM) for immutability |
| Delete data after 90 days | S3 Lifecycle expiration rule |
| DynamoDB session data expires in 24h | DynamoDB TTL attribute |
| GDPR right to erasure | Iceberg row-level deletes + compaction, OR cryptographic erasure |

### S3 Object Lock for Regulatory Compliance

Object Lock modes:
- **Governance mode**: Object cannot be deleted by most users; accounts with `s3:BypassGovernanceRetention` permission can override
- **Compliance mode**: Object CANNOT be deleted by ANYONE — not even root — until retention period expires

**Retention period**: Set at object creation. Cannot be shortened after enabling.

**Legal hold**: An indefinite hold with no expiration — used when litigation is pending.

```python
import boto3
s3 = boto3.client('s3')

# Store an object with compliance mode, 7-year retention
s3.put_object(
    Bucket='compliance-archive',
    Key='financial-records/2024/q1.parquet',
    Body=data,
    ObjectLockMode='COMPLIANCE',
    ObjectLockRetainUntilDate='2031-01-01T00:00:00Z'
)
```

---

## Data Lineage

Data lineage tracks where data came from, how it was transformed, and where it went. Required for:
- Debugging: why does this report show the wrong number?
- Compliance: prove that PII was properly masked before reaching the analytics layer
- Impact analysis: if the source schema changes, which downstream tables are affected?

**AWS Lake Formation** tracks lineage for Glue-based transformations.

**Amazon DataZone** (newer service, DEA-C01 v1.1): A data governance portal that provides data lineage, a data catalog with business context, and data access request workflows.

---

## Exam Scenarios

**"A company must comply with GDPR. They store EU customer data in S3. How do they discover which S3 objects contain PII?"**
→ Enable Amazon Macie and run classification jobs on the relevant S3 buckets

**"Auditors need proof that no one has accessed the customer PII table in the last 90 days."**
→ Enable CloudTrail data events on the S3 bucket and Glue Data Catalog, then query CloudTrail logs via Athena

**"Financial records must be retained for 7 years and must not be modifiable or deletable during that period."**
→ S3 Object Lock in Compliance mode with a 7-year retention period

**"A user exercised their GDPR right to erasure. The company stores data in S3 Parquet files with billions of rows."**
→ Two options: (1) Iceberg row-level deletes with file compaction; (2) Cryptographic erasure — encrypt user data fields with a user-specific KMS key and delete the key

**"A company processes personal information of South African citizens. Which AWS services help them comply with POPIA?"**
→ Amazon Macie (PII discovery), Lake Formation (access controls), CloudTrail (audit logs), AWS Audit Manager (evidence collection), S3 Object Lock (retention enforcement)
