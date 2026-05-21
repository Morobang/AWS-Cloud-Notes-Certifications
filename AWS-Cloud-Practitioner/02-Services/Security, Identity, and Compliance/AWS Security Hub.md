# AWS Security Hub

## What you'll learn
- What Security Hub is and what it aggregates
- Security standards and compliance checks
- Security Hub vs individual security services
- Key exam facts

---

## What is AWS Security Hub?

AWS Security Hub is a **cloud security posture management service that provides a comprehensive view of your security state across AWS accounts**. It aggregates, organizes, and prioritizes security findings from multiple AWS security services (and third-party tools) into a single dashboard.

---

## The problem Security Hub solves

Security findings come from many sources:
- GuardDuty: threat detection findings
- Inspector: vulnerability findings
- Macie: sensitive data findings
- Config: compliance findings
- Firewall Manager: policy findings
- IAM Access Analyzer: access policy findings

Without Security Hub, you'd need to check each service separately. Security Hub aggregates them all into one place.

---

## What Security Hub provides

**Centralized findings** — All findings from integrated services appear in Security Hub, normalized to a standard format (AWS Security Finding Format — ASFF). Filter, sort, and investigate across all findings from one dashboard.

**Security standards** — Pre-built compliance benchmarks that automatically check your account against hundreds of controls:
- **AWS Foundational Security Best Practices (FSBP)**: AWS-developed best practices
- **CIS AWS Foundations Benchmark**: Center for Internet Security recommendations
- **PCI DSS**: Payment card industry compliance
- **NIST SP 800-53**: Federal security controls

Security Hub automatically runs checks against these standards and shows your compliance score.

**Custom insights** — Create saved queries and visualizations of findings based on your criteria.

**Multi-account support** — Aggregate findings across all accounts in your AWS Organization into a single Security Hub account.

---

## Security Hub as the aggregation layer

Security Hub is not a detection service itself — it aggregates from detection services:

```
GuardDuty → 
Inspector  →  AWS Security Hub → Centralized view + compliance scores
Macie      →
Config     →
```

---

## Exam focus

- Security Hub = **centralized security findings dashboard** — aggregates GuardDuty, Inspector, Macie, Config, and more
- Provides **security standards compliance scores** (CIS, PCI DSS, AWS FSBP)
- Multi-account aggregation across an AWS Organization
- Security Hub doesn't detect threats — it aggregates findings from services that do
- Use when exam describes: "centralized view of security posture," "compliance dashboard," "aggregate security findings from multiple services"

---

## Practice questions

**Q1.** A security team wants a single dashboard that shows all security findings across their AWS Organization — including GuardDuty threat findings, Inspector vulnerability findings, and compliance check results against the CIS AWS Foundations Benchmark. Which service provides this?

A) Amazon GuardDuty  
B) AWS Config  
C) AWS Security Hub  
D) Amazon Inspector

**Answer: C** — Security Hub aggregates findings from GuardDuty, Inspector, and other services into one dashboard and provides compliance checks against standards like CIS AWS Foundations. GuardDuty only shows its own threat findings. Config shows its own compliance findings. Inspector shows its own vulnerability findings. Security Hub is the aggregation layer across all of them.
