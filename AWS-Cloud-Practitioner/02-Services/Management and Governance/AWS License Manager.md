# AWS License Manager

## What you'll learn
- What License Manager does
- BYOL (Bring Your Own License) on AWS
- Key use cases
- Key exam facts

---

## What is AWS License Manager?

AWS License Manager is a service that **helps you manage software licenses from vendors like Microsoft, Oracle, SAP, and IBM across your AWS and on-premises environments**. It tracks license usage, enforces license rules, and prevents license violations.

---

## The problem it solves

Many enterprise software vendors (Microsoft SQL Server, Oracle Database, Windows Server, IBM products) have complex licensing models — licenses tied to the number of CPU cores, sockets, virtual machines, or users. Running this software on EC2 without tracking can lead to:

- **License overuse** (non-compliance = large fines from audits)
- **Paying for unused licenses** (wasted cost)

License Manager provides visibility and control over license consumption across your AWS fleet.

---

## BYOL (Bring Your Own License)

BYOL allows customers to use their existing on-premises software licenses on AWS EC2 Dedicated Hosts or Dedicated Instances, rather than paying AWS for a new license through the instance pricing.

For example: If you already own Windows Server licenses, you can bring those licenses to AWS (using Dedicated Hosts) instead of paying for the Windows license included in regular On-Demand pricing.

License Manager tracks your BYOL license usage to ensure you stay compliant with vendor agreements.

---

## Key features

- **License configurations**: Define rules based on your license terms (e.g., "This Oracle license covers 8 cores maximum")
- **License tracking**: Monitor how many licenses are in use across EC2 instances, RDS, and even on-premises servers (via Systems Manager)
- **Hard and soft limits**: Stop new instance launches if a license limit would be exceeded (hard limit) or just alert when approaching a limit (soft limit)
- **License reports**: Generate reports for vendor audits

---

## When to use it

**BYOL compliance** — Track and enforce usage of Windows Server, SQL Server, or Oracle licenses brought to AWS, ensuring you don't exceed the number of licenses you own.

**License audit preparation** — Generate reports showing license usage at any point in time to support vendor audit requests.

**Cost optimization** — Identify unused or underused license allocations and reclaim them.

---

## Exam focus

- License Manager = **software license tracking and compliance** across AWS and on-premises
- Key use case: **BYOL** — bringing existing licenses to AWS Dedicated Hosts
- Prevents license overuse that could result in compliance violations and audit fines
- Use when exam describes: "manage software licenses," "BYOL compliance," "track Microsoft or Oracle license usage"

---

## Practice questions

**Q1.** A company has existing Microsoft SQL Server licenses and wants to use them on AWS EC2 instances rather than paying for new licenses through AWS pricing. They need to track license usage to ensure compliance with their license agreement. Which AWS service helps manage this?

A) AWS Service Catalog  
B) AWS Systems Manager  
C) AWS License Manager  
D) AWS Cost Explorer

**Answer: C** — License Manager tracks BYOL license usage across EC2 instances and helps ensure you don't exceed your license entitlements. It can enforce hard limits to prevent launching instances that would violate the license count. Service Catalog manages approved product provisioning. Systems Manager handles operational management. Cost Explorer analyzes spending.
