# AWS Audit Manager

## What you'll learn
- What Audit Manager does and the problem it solves
- Frameworks and evidence collection
- Audit Manager vs Artifact
- Key exam facts

---

## What is AWS Audit Manager?

AWS Audit Manager is a service that **continuously collects evidence of your AWS usage to help you prepare for compliance audits**. Instead of manually gathering screenshots, logs, and configuration exports when an audit is approaching, Audit Manager automatically collects and organizes evidence throughout the year.

---

## The problem it solves

Audit preparation is time-consuming: compliance auditors require evidence that your AWS environment meets specific controls (e.g., "Prove that all S3 buckets have encryption enabled" or "Show access logs for the past 12 months"). Manually gathering this evidence takes weeks.

Audit Manager automates evidence collection continuously — when an audit happens, the evidence is already organized.

---

## How it works

1. Choose a **framework**: Pre-built frameworks for standards like SOC 2, PCI DSS, HIPAA, NIST, CIS. Custom frameworks can also be built.

2. Audit Manager maps the framework's controls to AWS services and automatically collects evidence:
   - CloudTrail logs
   - Config configuration snapshots
   - Security Hub findings
   - Resource inventories

3. Evidence is organized by control — auditors can see, for each compliance requirement, the evidence that it's being met.

4. You can **add manual evidence** (screenshots, documents) for controls that aren't automated.

5. Generate **assessment reports** for auditors.

---

## Audit Manager vs Artifact

| | AWS Audit Manager | AWS Artifact |
|-|------------------|--------------|
| What it provides | Evidence of YOUR compliance posture | AWS's own compliance certifications |
| Usage | Ongoing evidence collection for your audit | Download AWS compliance documents |
| Example | "Prove our S3 buckets are encrypted" | "Download AWS SOC 2 report" |
| Automated? | Yes — continuous evidence collection | No — manual download |

---

## When to use it

**Regulatory compliance preparation** — SOC 2, PCI DSS, HIPAA, ISO 27001 audits where you need to prove your environment's compliance.

**Continuous compliance monitoring** — Stay audit-ready year-round rather than scrambling before each audit.

---

## Exam focus

- Audit Manager = **continuous evidence collection** for your compliance audits
- Uses pre-built frameworks (SOC 2, PCI DSS, HIPAA, NIST)
- Automates evidence gathering from CloudTrail, Config, Security Hub
- Key differentiator from Artifact: Audit Manager proves YOUR compliance; Artifact provides AWS's compliance certs
- Use when exam describes: "automate audit evidence collection," "prepare for compliance audit," "continuous compliance monitoring"

---

## Practice questions

**Q1.** A company undergoes an annual SOC 2 audit and needs to provide evidence that their AWS environment meets the required controls. They want to automate the evidence collection process throughout the year rather than manually gathering it before each audit. Which AWS service supports this?

A) AWS Artifact  
B) AWS Config  
C) AWS Audit Manager  
D) AWS Security Hub

**Answer: C** — Audit Manager continuously collects evidence from AWS services mapped to SOC 2 controls, organizing it for auditors. Artifact provides AWS's own compliance reports (like AWS's SOC 2) but not evidence of the company's own compliance. Config checks resource compliance but doesn't organize evidence by audit framework controls. Security Hub aggregates security findings but doesn't provide the audit evidence structure.
