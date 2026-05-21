# AWS Marketplace

## What you'll learn
- What AWS Marketplace is and what types of software it offers
- How Marketplace billing works
- Private Marketplace for enterprises
- Key exam facts

---

## What is AWS Marketplace?

AWS Marketplace is a **digital catalog of third-party software** that you can find, evaluate, purchase, and deploy directly to your AWS environment. It offers thousands of listings from independent software vendors (ISVs) across security, networking, machine learning, storage, business intelligence, and more.

---

## What Marketplace offers

| Category | Examples |
|----------|---------|
| Security | Firewalls, endpoint protection, vulnerability scanners |
| Machine learning | Pre-trained models, data labeling tools |
| Infrastructure | Operating systems, monitoring agents |
| Database | Database engines, replication tools |
| Business applications | CRM integrations, ERP connectors |
| Data products | Market data feeds, reference datasets |

---

## Deployment types

**Amazon Machine Images (AMIs)** — Pre-configured virtual machine images ready to launch on EC2. Common for security appliances and server software.

**SaaS subscriptions** — Software that runs in the vendor's environment; you access it via API or web interface. Billed through your AWS account.

**Container images** — Docker container images deployable on ECS or EKS.

**CloudFormation templates** — Pre-built infrastructure templates that deploy complex multi-tier architectures.

---

## Billing and procurement

All Marketplace purchases are consolidated into your **AWS bill** — no separate invoices or payment methods for individual vendors. This simplifies procurement and enables:

- **Volume discounts**: Marketplace usage counts toward your AWS spending for volume tier benefits
- **Consolidated billing**: Marketplace charges appear in AWS Cost Explorer with the same tags
- **Pay-as-you-go**: Many products offer hourly or monthly pricing with no upfront commitment
- **Annual subscriptions**: Discounted pricing with 1-year commitments
- **Bring your own license (BYOL)**: Use existing software licenses you already own

---

## Private Marketplace

Enterprises can create a **Private Marketplace** — a curated subset of the public Marketplace approved for use within their organization. Employees can only purchase products that appear in the Private Marketplace, enforcing procurement policies and security standards.

---

## When to use it

**Rapid procurement** — A security team needs a WAF appliance immediately. Instead of going through a lengthy vendor sales process, they find, subscribe, and deploy from Marketplace in minutes.

**Try before you buy** — Many products offer free trials through Marketplace, allowing evaluation before committing to a subscription.

**Standardized software stack** — An organization uses Private Marketplace to ensure all teams use approved software versions with vetted security configurations.

---

## Exam focus

- Marketplace = **digital catalog of third-party software** deployable on AWS
- All purchases **consolidated into your AWS bill**
- Offers AMIs, SaaS, containers, CloudFormation templates, and data products
- **Private Marketplace**: curated approved software catalog for enterprise procurement control
- Key differentiator: Marketplace provides software *from third-party vendors*; AWS provides its own services separately
- Use when exam mentions: "third-party software," "ISV," "software procurement," "pre-configured AMI"

---

## Practice questions

**Q1.** A company wants to deploy a pre-configured intrusion detection system (IDS) on AWS. They want to use an existing third-party IDS product they know well, bill it through their AWS account, and deploy it as quickly as possible. Which AWS service facilitates this?

A) AWS Security Hub  
B) AWS WAF  
C) AWS Marketplace  
D) Amazon GuardDuty

**Answer: C** — AWS Marketplace offers pre-configured AMIs and deployable software from third-party security vendors, billed through the AWS account. The company can find their preferred IDS vendor, launch it directly on EC2, and have it running in minutes. Security Hub, WAF, and GuardDuty are AWS-native security services, not third-party software catalogs.
