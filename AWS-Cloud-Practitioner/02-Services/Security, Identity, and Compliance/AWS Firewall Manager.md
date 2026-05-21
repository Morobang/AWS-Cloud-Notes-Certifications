# AWS Firewall Manager

## What you'll learn
- What Firewall Manager does
- Centralized security policy management
- Firewall Manager vs WAF vs Shield
- Key exam facts

---

## What is AWS Firewall Manager?

AWS Firewall Manager is a **security management service that allows you to centrally configure and manage firewall rules across your AWS Organization accounts and resources**. Instead of configuring WAF rules, Shield Advanced protections, and security groups in each account separately, you define policies once and Firewall Manager applies them everywhere.

---

## The problem it solves

A large organization has 50 AWS accounts. Each account needs:
- WAF rules on their Application Load Balancers
- Shield Advanced protection on their CloudFront distributions
- Security group rules on EC2 instances

Without Firewall Manager: configure these in 50 accounts separately. When a new WAF rule is needed, update 50 accounts.

With Firewall Manager: define the policy centrally; Firewall Manager automatically applies it to all accounts (and any new accounts added to the organization).

---

## What Firewall Manager manages

| Policy type | What it covers |
|-------------|---------------|
| AWS WAF | Web ACLs on ALBs, CloudFront, API Gateway, AppSync |
| AWS Shield Advanced | Enable Shield Advanced on resources across accounts |
| Security Groups | Enforce security group rules on EC2, ELB |
| Network Firewall | Deploy AWS Network Firewall across VPCs |
| Route 53 Resolver DNS Firewall | DNS-level protection |

---

## Firewall Manager vs WAF vs Shield

| | Firewall Manager | AWS WAF | AWS Shield |
|-|-----------------|---------|-----------|
| Role | Management/enforcement layer | Web application protection | DDoS protection |
| Scope | Multi-account, multi-resource | Individual ALB, CloudFront, API GW | Individual resources |
| Purpose | Apply policies across accounts consistently | Block malicious web requests | Mitigate DDoS attacks |

Firewall Manager uses WAF and Shield under the hood — it's the management plane that ensures they're consistently deployed everywhere.

---

## When to use it

**Multi-account organization security** — Ensure every account in your organization has the same baseline WAF rules without manually configuring each account.

**New account compliance** — Automatically apply security policies to newly created accounts through AWS Organizations.

**Remediation** — Identify and fix non-compliant resources (those missing required WAF rules) automatically.

---

## Exam focus

- Firewall Manager = **centralized security policy management** across AWS Organization accounts
- Manages: WAF, Shield Advanced, security groups, Network Firewall
- Works with **AWS Organizations** — policies apply to all accounts/OUs
- Use when exam describes: "enforce WAF rules across all accounts," "centrally manage security policies," "ensure new accounts get security policies automatically"

---

## Practice questions

**Q1.** A company with 40 AWS accounts needs to ensure all Application Load Balancers across all accounts have the same WAF rules applied. New accounts added to the organization should automatically receive these rules. Which AWS service manages this?

A) AWS WAF  
B) AWS Security Hub  
C) AWS Firewall Manager  
D) AWS Config

**Answer: C** — Firewall Manager defines WAF policies once and automatically applies them to ALBs across all accounts in the organization, including new accounts. WAF manages rules on individual resources — not centrally across accounts. Security Hub aggregates findings but doesn't enforce security policies. Config checks configuration compliance but doesn't centrally deploy WAF rules.
