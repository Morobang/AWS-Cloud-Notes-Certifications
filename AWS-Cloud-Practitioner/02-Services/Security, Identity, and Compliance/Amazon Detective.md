# Amazon Detective

## What you'll learn
- What Amazon Detective does
- How it differs from GuardDuty
- Key use cases
- Key exam facts

---

## What is Amazon Detective?

Amazon Detective is a **security investigation service that makes it easy to analyze, investigate, and quickly identify the root cause of security findings or suspicious activities**. It automatically collects log data from AWS services and uses machine learning, statistical analysis, and graph theory to build a linked set of data that enables you to investigate security issues.

---

## The problem it solves

When GuardDuty (or Security Hub) generates a finding — "EC2 instance is communicating with a malicious IP" — the security team needs to investigate:
- Which EC2 instance?
- When did this start?
- What other activity was associated with this instance?
- Is this part of a larger pattern?
- How did the attacker get in?

Without Detective, this investigation requires manually correlating CloudTrail logs, VPC Flow Logs, GuardDuty findings, and other data — time-consuming and error-prone.

Detective builds an interactive graph of all this data, making investigation fast and visual.

---

## How Detective works

Detective automatically:
- Ingests data from CloudTrail (API calls), VPC Flow Logs (network traffic), and GuardDuty findings
- Builds a behavioral graph linking entities (IAM users, roles, EC2 instances, IP addresses) and their relationships over time
- Establishes baselines of normal behavior
- When you investigate a finding, it shows the full context: timeline, relationships, anomalies

**Investigation flow**:
1. GuardDuty generates a finding
2. You click "Investigate in Detective" from GuardDuty
3. Detective shows a visual graph: which entity was involved, what it did before/during/after the finding, whether this is unusual
4. You quickly determine scope of compromise and root cause

---

## Detective vs GuardDuty

| | Amazon Detective | Amazon GuardDuty |
|-|-----------------|-----------------|
| Role | Investigate and understand security events | Detect suspicious activity |
| Phase | After detection — root cause analysis | Ongoing monitoring — detection |
| Output | Investigative graph and timeline | Findings (alerts) |
| Analogy | Forensic investigator | Alarm system |

GuardDuty tells you something is wrong. Detective helps you understand what happened and why.

---

## When to use it

**Security incident investigation** — After a GuardDuty finding, use Detective to quickly determine: scope of the compromise, affected resources, attacker's entry point, timeline.

**Threat hunting** — Proactively search for suspicious behavior patterns across your environment.

---

## Exam focus

- Detective = **security investigation** — root cause analysis for security findings
- Analyzes: **CloudTrail, VPC Flow Logs, GuardDuty findings**
- Builds behavioral graphs linking entities and events over time
- Key differentiator from GuardDuty: GuardDuty detects; Detective investigates
- Use when exam describes: "investigate a GuardDuty finding," "root cause analysis," "understand scope of security incident"

---

## Practice questions

**Q1.** After Amazon GuardDuty generates a finding about suspicious activity involving an IAM role, a security analyst needs to quickly determine what other AWS resources were accessed by that role, when the suspicious activity began, and whether it is part of a larger pattern. Which AWS service assists with this investigation?

A) AWS CloudTrail  
B) AWS Config  
C) Amazon Detective  
D) Amazon Inspector

**Answer: C** — Detective aggregates CloudTrail, VPC Flow Logs, and GuardDuty data into an interactive graph, making it easy to investigate findings — showing the entity's behavior over time, related resources, and anomalies. CloudTrail contains the raw API call logs but requires manual querying and correlation. Config tracks configuration state, not behavioral investigation. Inspector scans for vulnerabilities, not behavioral analysis.
