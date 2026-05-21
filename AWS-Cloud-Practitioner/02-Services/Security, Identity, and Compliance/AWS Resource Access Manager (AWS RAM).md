# AWS Resource Access Manager (AWS RAM)

## What you'll learn
- What RAM does and what resources it shares
- RAM vs VPC Peering
- Key use cases
- Key exam facts

---

## What is AWS Resource Access Manager?

AWS Resource Access Manager (RAM) is a service that **allows you to share AWS resources across AWS accounts within your organization**. Instead of duplicating resources in every account, you create them once and share them.

---

## Why resource sharing matters

In a multi-account organization:
- Each team has their own AWS account (for isolation)
- But some resources make sense to share — a central VPC subnet, a Transit Gateway, a License Manager configuration

Without RAM, you'd need to create and manage these resources separately in each account. With RAM, create once, share to many.

---

## What can be shared with RAM

| Resource type | Use case |
|---------------|---------|
| VPC subnets | Share a subnet with another account so their EC2 instances run in it |
| Transit Gateway | Share a central Transit Gateway across accounts |
| AWS License Manager configurations | Share license rules across accounts |
| Amazon Route 53 Resolver rules | Share DNS rules across accounts |
| AWS Glue Data Catalog | Share metadata for cross-account queries |
| Amazon Aurora DB clusters | Share with other accounts |
| AWS CodeBuild projects | Share build environments |

---

## How RAM works

1. Resource owner creates a resource (e.g., a subnet or Transit Gateway)
2. Owner creates a resource share in RAM, specifying:
   - Which resource(s) to share
   - Which accounts (or OUs in the organization) to share with
3. Recipients accept the share invitation (or auto-accept within the same organization)
4. The resource appears in the recipient's account and they can use it

The resource is still owned and managed by the original account — recipients use it but don't manage it.

---

## RAM vs VPC Peering

| | AWS RAM | VPC Peering |
|-|---------|------------|
| What it shares | Subnets, Transit Gateways, etc. | Network connectivity between two VPCs |
| Use case | Use resources from another account | Route traffic between VPC IP ranges |
| Shared ownership | No — owner manages; recipients use | Both accounts manage their VPC |

---

## When to use it

**Centralized networking** — Create subnets in a central networking account and share them with application accounts — all workloads run in centrally managed subnets.

**Shared Transit Gateway** — One Transit Gateway serves all accounts in the organization — share it via RAM.

**License sharing** — Share License Manager configurations so all accounts use the same license rules.

---

## Exam focus

- RAM = **share AWS resources across accounts** — without duplicating them
- Common resources to share: **VPC subnets, Transit Gateways, Route 53 Resolver rules**
- Works within AWS Organizations — resources can be shared to specific accounts or OUs
- Use when exam describes: "share a subnet across accounts," "share a Transit Gateway," "share resources without duplicating them"

---

## Practice questions

**Q1.** A company has a central networking account that owns VPC subnets in a shared services VPC. They want application teams in other AWS accounts to launch EC2 instances into these centrally managed subnets. Which AWS service enables this?

A) VPC Peering  
B) AWS Transit Gateway  
C) AWS Resource Access Manager  
D) AWS PrivateLink

**Answer: C** — RAM allows the networking account to share VPC subnets with other accounts. EC2 instances in those accounts can be launched into the shared subnets. VPC Peering connects two VPCs for network routing — it doesn't share subnets. Transit Gateway routes traffic between VPCs — it doesn't share subnets (though it can itself be shared via RAM). PrivateLink provides private service endpoints, not subnet sharing.
