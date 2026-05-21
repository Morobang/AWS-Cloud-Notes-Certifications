# AWS Outposts

## What you'll learn
- What AWS Outposts is and why it exists
- What services run on Outposts
- When to use Outposts vs a normal AWS Region
- Key exam facts

---

## What is AWS Outposts?

AWS Outposts is **AWS infrastructure installed in your own data center or on-premises facility**. It delivers the same AWS hardware, software, APIs, and tools that run in AWS Regions — but physically located in your building.

From your applications' perspective, Outposts looks like just another AWS availability zone. You use the same AWS Management Console, APIs, and SDKs to interact with resources on Outposts as you would with any other AWS resource.

---

## Why Outposts exists

Some organizations have workloads that cannot run in a public cloud Region for specific reasons:

- **Data residency requirements**: Laws or regulations requiring data to stay within a specific physical building or jurisdiction
- **Ultra-low latency**: Applications that require sub-millisecond latency to on-premises systems (manufacturing automation, real-time trading)
- **Existing on-premises dependencies**: Applications tightly coupled to other systems that cannot be moved to the cloud
- **Disconnected or intermittent connectivity**: Environments where consistent internet connectivity to an AWS Region cannot be guaranteed

---

## Services available on Outposts

Outposts supports a subset of AWS services running locally:

- Amazon EC2 (compute)
- Amazon EBS (block storage)
- Amazon S3 (object storage — Outposts S3)
- Amazon RDS (relational databases)
- Amazon EKS (Kubernetes)
- Amazon ECS (containers)
- AWS App Mesh

---

## Outposts vs AWS Local Zones vs AWS Wavelength

| | AWS Outposts | AWS Local Zones | AWS Wavelength |
|-|--------------|----------------|----------------|
| Location | Customer's facility | AWS-operated facility in a city | Telecom carrier's 5G network |
| Who manages hardware | AWS (ships rack to you) | AWS | AWS |
| Use case | Strict data residency, on-prem dependencies | Low-latency to specific metro areas | Ultra-low latency to 5G devices |

---

## When to use it

**Healthcare or financial services data residency** — Regulations require patient records or transaction data to physically remain within a specific facility.

**Factory automation** — A manufacturing line needs millisecond-level latency to communicate with robots and sensors located on the factory floor.

**Migration bridge** — During a long-term migration, some workloads stay on-premises while leveraging AWS services and management tooling.

---

## Exam focus

- Outposts = **AWS infrastructure in your data center** — same APIs and console as cloud AWS
- AWS **owns and manages** the hardware (racks shipped to you, managed by AWS)
- Use when: data residency requirements, ultra-low latency to on-premises systems, or regulatory constraints prevent using public cloud
- Not for cost savings — Outposts are expensive; use them only when required
- Key differentiator: Outposts brings AWS to *your* location; AWS Regions and Local Zones are *AWS's* locations

---

## Practice questions

**Q1.** A hospital system requires that patient data and processing remain within their own data center due to regulatory requirements, but they want to use AWS services and management tools. Which service allows this?

A) AWS Local Zones  
B) AWS Direct Connect  
C) AWS Outposts  
D) AWS Wavelength

**Answer: C** — Outposts installs AWS infrastructure physically within the customer's data center, satisfying strict data residency requirements while providing the same AWS APIs, services, and management tools. Local Zones are AWS-operated facilities (outside the customer's control). Direct Connect is a network connection to AWS. Wavelength is for 5G edge computing.
