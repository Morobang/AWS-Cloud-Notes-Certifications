# Amazon Inspector

## What you'll learn
- What Inspector scans for and what it covers
- Automated vs manual scanning
- Inspector vs GuardDuty
- Key exam facts

---

## What is Amazon Inspector?

Amazon Inspector is an **automated vulnerability management service** that continuously scans AWS workloads for software vulnerabilities and unintended network exposure. It helps you identify security weaknesses in EC2 instances, container images, and Lambda functions before attackers exploit them.

---

## What Inspector scans

**EC2 instances** — Scans for:
- Known software vulnerabilities (CVEs — Common Vulnerabilities and Exposures) in installed packages
- Unintended network exposure (network reachability analysis)

**Amazon ECR container images** — Scans Docker images pushed to ECR for known CVEs in the base OS packages and application libraries.

**AWS Lambda functions** — Scans Lambda function code and the layers it uses for known vulnerabilities in software packages.

---

## How Inspector works

- Inspector is always-on — it doesn't require you to schedule scans manually
- When a new CVE is published, Inspector immediately re-evaluates all affected resources against the new vulnerability — you don't need to trigger a new scan
- Uses the AWS Systems Manager agent for EC2 scanning (the same SSM agent used for Systems Manager features)

---

## Inspector findings

Each finding includes:
- **Severity**: Critical, High, Medium, Low, Informational
- **CVE ID** (if applicable)
- **Affected resource**
- **Remediation guidance**

Findings are consolidated in Inspector and can also be sent to **AWS Security Hub** for centralized security management.

---

## Inspector vs GuardDuty

| | Amazon Inspector | Amazon GuardDuty |
|-|-----------------|-----------------|
| What it does | Scans for software vulnerabilities and network exposure | Detects active threats and suspicious behavior |
| Nature | Preventive/proactive (find weaknesses before attack) | Detective/reactive (detect attacks in progress) |
| Data | Software package versions, network config | CloudTrail, VPC Flow Logs, DNS |
| Output | CVE findings, network exposure findings | Threat findings (malicious activity detected) |

---

## When to use it

**Vulnerability management** — Keep track of which EC2 instances and containers have unpatched CVEs.

**Container security** — Scan images before deploying to ensure no known vulnerabilities in the base image.

**Compliance** — Many compliance frameworks require regular vulnerability scanning; Inspector provides continuous automated scanning.

---

## Exam focus

- Inspector = **automated vulnerability scanning** for EC2, ECR containers, and Lambda
- Scans for **CVEs (software vulnerabilities)** and unintended network exposure
- Continuous, always-on — automatically re-evaluates when new CVEs are published
- Use when exam describes: "scan for vulnerabilities," "find unpatched software," "container image security scanning"
- Contrast with GuardDuty: Inspector = vulnerability scanning; GuardDuty = threat detection

---

## Practice questions

**Q1.** A company wants to continuously monitor their EC2 instances and Lambda functions for known software vulnerabilities and receive alerts when high-severity CVEs are detected. Which AWS service provides this?

A) Amazon GuardDuty  
B) AWS Config  
C) Amazon Inspector  
D) AWS Security Hub

**Answer: C** — Inspector continuously scans EC2 instances, ECR images, and Lambda functions for CVEs. When a new high-severity CVE is published, Inspector immediately flags affected resources. GuardDuty detects active threats, not software vulnerabilities. Config checks resource configuration compliance. Security Hub aggregates findings from Inspector (and other services) but doesn't perform the scanning itself.
