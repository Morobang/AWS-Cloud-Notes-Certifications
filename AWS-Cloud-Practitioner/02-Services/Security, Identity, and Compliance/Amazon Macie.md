# Amazon Macie

## What you'll learn
- What Macie does and what sensitive data it finds
- How Macie works with S3
- Macie vs Rekognition vs Comprehend
- Key exam facts

---

## What is Amazon Macie?

Amazon Macie is a **data security service that uses machine learning to automatically discover, classify, and protect sensitive data in Amazon S3**. It detects personally identifiable information (PII), financial data, healthcare data, and other sensitive content — alerting you when sensitive data is stored in unexpected locations or is publicly accessible.

---

## What Macie detects

Macie can identify:

| Category | Examples |
|----------|---------|
| PII | Names, addresses, dates of birth, SSNs, passport numbers |
| Financial data | Credit card numbers, bank account numbers |
| Credentials | API keys, private keys, passwords in files |
| Healthcare data | Patient IDs, medical record numbers |
| Custom data | Define your own patterns for business-specific sensitive data |

---

## How Macie works

1. Enable Macie and connect it to your S3 buckets
2. Macie continuously evaluates S3 bucket security posture (public access, encryption settings, sharing policies)
3. You run **sensitive data discovery jobs** that scan S3 object contents using ML classifiers
4. Macie generates **findings** for:
   - Sensitive data discovered in S3 objects
   - Policy findings: buckets that are publicly accessible, unencrypted, or shared with external accounts

---

## Policy findings

In addition to content scanning, Macie generates policy findings for S3 bucket configuration issues:
- Bucket made public (high severity)
- Encryption disabled
- Bucket shared with external AWS account
- Replication disabled

---

## Macie vs other services

| | Amazon Macie | Amazon Comprehend | Amazon Rekognition |
|-|-------------|------------------|-------------------|
| Data type | S3 file contents (text, documents) | Unstructured text you provide | Images and video |
| Sensitive data | PII, financial, credentials in S3 | PII detection in text (via API call) | Faces in images |
| Scope | S3 bucket scanning | On-demand text analysis | On-demand image analysis |

---

## When to use it

**Data governance** — Ensure no sensitive customer data is accidentally uploaded to publicly accessible S3 buckets.

**Compliance** — Prove to auditors that you monitor for PII in storage (GDPR, HIPAA, PCI DSS).

**Data discovery** — Find where sensitive data lives across hundreds of S3 buckets.

---

## Exam focus

- Macie = **sensitive data discovery in S3** — detects PII, financial data, credentials
- Uses ML to scan S3 object contents
- Also generates policy findings for S3 bucket misconfigurations (public access, no encryption)
- Use when exam describes: "find PII in S3," "detect sensitive data in S3 buckets," "data classification in S3"

---

## Practice questions

**Q1.** A company stores customer data across hundreds of Amazon S3 buckets. They need to automatically identify which buckets contain personally identifiable information (PII) and whether any of those buckets are publicly accessible. Which AWS service provides this?

A) Amazon Rekognition  
B) AWS Config  
C) Amazon Macie  
D) Amazon Inspector

**Answer: C** — Macie automatically discovers and classifies sensitive data (including PII) across S3 buckets using ML, and also flags buckets with public access enabled. Rekognition analyzes images and video. Config checks resource configuration compliance but doesn't scan S3 object contents for sensitive data. Inspector scans for software vulnerabilities in EC2, containers, and Lambda — not S3 data classification.
