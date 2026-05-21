# AWS Artifact

## What you'll learn
- What AWS Artifact provides
- Compliance reports vs agreements
- Key exam facts

---

## What is AWS Artifact?

AWS Artifact is a **self-service portal for accessing AWS compliance documentation and agreements**. It provides on-demand access to AWS security and compliance reports, and allows you to review and accept agreements with AWS.

---

## Two sections

**Artifact Reports** — Download AWS compliance reports and certifications:
- SOC 1, SOC 2, and SOC 3 reports
- PCI DSS Attestation of Compliance
- ISO 27001, ISO 9001, ISO 27017, ISO 27018 certifications
- FedRAMP authorizations
- HIPAA-eligible service documentation
- And many others

These reports demonstrate AWS's own compliance with various standards and regulations — useful when you need to show auditors that your cloud infrastructure provider meets compliance requirements.

**Artifact Agreements** — Review and accept legal agreements with AWS:
- Business Associate Agreement (BAA) — required under HIPAA before using AWS for healthcare data
- Non-Disclosure Agreements (NDAs)
- Agreements can be accepted for individual accounts or for all accounts in your AWS Organization

---

## Who uses it

**Compliance teams** — Need to show auditors that AWS has relevant certifications.

**Legal teams** — Need to sign a BAA before using AWS for HIPAA-covered workloads.

**Security teams** — Need documentation of AWS's security controls for risk assessments.

---

## Exam focus

- Artifact = **download AWS compliance reports and sign agreements** — self-service portal
- Reports: SOC, PCI DSS, ISO certifications, FedRAMP, HIPAA
- Agreements: BAA (for HIPAA), NDAs
- Use when exam describes: "download AWS compliance documentation," "get SOC 2 report," "sign a HIPAA Business Associate Agreement"

---

## Practice questions

**Q1.** A healthcare company needs to process patient data on AWS. Their compliance team requires documentation confirming that AWS is HIPAA-eligible, and their legal team needs to sign a Business Associate Agreement with AWS. Where do they access these?

A) AWS Trusted Advisor  
B) AWS Security Hub  
C) AWS Artifact  
D) AWS Audit Manager

**Answer: C** — AWS Artifact provides the HIPAA-eligible services documentation under Artifact Reports, and allows the legal team to review and accept the BAA under Artifact Agreements. Trusted Advisor checks AWS best practices. Security Hub aggregates security findings. Audit Manager collects evidence for compliance audits but doesn't provide AWS compliance certifications.
