# AWS Trusted Advisor

## What you'll learn
- What Trusted Advisor checks and the five categories
- Access by Support plan tier
- Trusted Advisor vs Compute Optimizer vs Config
- Key exam facts

---

## What is AWS Trusted Advisor?

AWS Trusted Advisor is an **automated best practice checking service** that inspects your AWS account and recommends improvements across five categories: cost optimization, performance, security, fault tolerance, and service limits.

Think of it as an automated consultant that compares your account configuration against AWS best practices and flags gaps.

---

## The five check categories

**Cost optimization** — Identify underutilized or idle resources:
- EC2 instances with low CPU utilization (potential to downsize or terminate)
- Unattached EBS volumes (paying for storage not in use)
- Idle load balancers and unused Elastic IP addresses
- Reserved Instance recommendations

**Performance** — Identify configuration improvements for better performance:
- EC2 instances with high CPU utilization (potential to upsize)
- CloudFront configurations that could be optimized
- EBS throughput optimization

**Security** — Identify security gaps:
- S3 buckets with public access enabled
- Security groups allowing unrestricted inbound access (0.0.0.0/0 on sensitive ports)
- Root account not using MFA
- IAM access keys that haven't been rotated

**Fault tolerance** — Identify single points of failure:
- EC2 instances not in multiple Availability Zones
- RDS instances without Multi-AZ enabled
- S3 buckets without versioning

**Service limits** (Service Quotas) — Warn when you're approaching AWS service limits:
- EC2 instance count approaching regional limit
- VPC count approaching limit

---

## Access by Support plan

| Check category | Basic / Developer | Business | Enterprise |
|---------------|------------------|----------|------------|
| Security checks (core) | 7 checks | All checks | All checks |
| Cost optimization checks | Limited | All checks | All checks |
| Performance, fault tolerance | Limited | All checks | All checks |
| Service limits | Limited | All checks | All checks |
| Programmatic access (API) | No | Yes | Yes |

Business and Enterprise Support plans get access to all Trusted Advisor checks and the API (to integrate alerts into operational workflows).

---

## Trusted Advisor vs Compute Optimizer vs Config

| | Trusted Advisor | Compute Optimizer | AWS Config |
|-|----------------|------------------|-----------|
| Scope | Broad — 5 categories, many services | Deep — compute right-sizing | Configuration compliance |
| Cost advice | High-level (idle resources) | Specific instance type recommendations | No |
| Security | Yes | No | Yes (Config rules) |
| Automation | No — recommendations only | No — recommendations only | Yes — can auto-remediate |

---

## Exam focus

- Trusted Advisor = **automated best practice checks** — 5 categories: Cost, Performance, Security, Fault Tolerance, Service Limits
- **Business and Enterprise** plans unlock all checks
- Key checks to remember: public S3 buckets (security), idle EC2 (cost), no MFA on root (security), no Multi-AZ RDS (fault tolerance)
- Use when exam describes: "check if resources follow AWS best practices," "identify cost savings," "security posture review"

---

## Practice questions

**Q1.** A company wants to check whether any of their S3 buckets are publicly accessible and whether their root account has MFA enabled, using an automated AWS service. Which service provides these checks?

A) AWS Config  
B) Amazon GuardDuty  
C) AWS Trusted Advisor  
D) AWS CloudTrail

**Answer: C** — Trusted Advisor has built-in security checks that flag S3 buckets with public access and root accounts without MFA. Config can also detect public S3 buckets with a Config rule, but Trusted Advisor provides both checks in one place across multiple categories. GuardDuty detects active threats. CloudTrail records API calls but doesn't evaluate configuration against best practices.
