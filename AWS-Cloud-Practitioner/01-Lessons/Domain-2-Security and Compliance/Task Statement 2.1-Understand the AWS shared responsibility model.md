# Task Statement 2.1: Understand the AWS Shared Responsibility Model

## What you'll learn
- Who is responsible for which security tasks — AWS or you
- How that responsibility changes based on which AWS services you use
- The common mistakes people make when they first work with AWS security

---

## The core concept

The **AWS Shared Responsibility Model** answers one question: if something goes wrong, whose job was it to prevent it?

The answer is always one of two things:

**AWS is responsible for "security OF the cloud"** — the physical infrastructure, the hardware, the data centers, the networking, and the underlying software that runs AWS services. You never touch this layer. You can't configure it, and you can't break it.

**You are responsible for "security IN the cloud"** — everything you put into AWS: your data, your applications, your access controls, your encryption settings, your network configuration.

The apartment building analogy that actually works: the building owner secures the entrance, maintains the elevators, handles the fire suppression system, and vets maintenance staff. You lock your apartment door, secure your valuables, and control who you give your keys to. The building owner being responsible for the hallways doesn't mean you can leave your apartment unlocked.

---

## What AWS always handles

No matter which AWS services you use, AWS is always responsible for:

- **Physical security of data centers** — guards, cameras, biometric access, unmarked buildings. AWS data center locations are not publicly disclosed.
- **Hardware** — servers, networking gear, storage hardware. When hardware fails, AWS replaces it. You never see this happen.
- **Hypervisor and virtualization** — the software layer that creates virtual machines and ensures your EC2 instances are isolated from other customers' instances.
- **Global network infrastructure** — the fiber, routers, and switches connecting AWS regions and availability zones.
- **Host operating system patching** — for managed services (RDS, Lambda, ElastiCache, etc.), AWS patches and maintains the underlying operating systems.

---

## What you always handle

No matter which AWS services you use, you are always responsible for:

- **Identity and Access Management (IAM)** — creating users, assigning permissions, enabling MFA, rotating credentials. AWS gives you the tools; you do the configuration.
- **Data protection** — choosing whether to encrypt your data at rest and in transit. Encryption options exist, but you must turn them on.
- **Network configuration** — setting security group rules, configuring VPC subnets, controlling what traffic can reach your resources.
- **Application security** — if you write code that runs on AWS, that code's security is your responsibility. AWS doesn't audit your application for SQL injection vulnerabilities.
- **Compliance implementation** — AWS infrastructure can be HIPAA-eligible, but you must configure your workload to meet HIPAA requirements. AWS being compliant doesn't make your application compliant.

---

## How responsibility shifts by service type

This is where it gets nuanced — and where the exam tests you.

The more AWS manages a service, the more AWS takes on. The more control you have, the more you're responsible for.

### IaaS — Infrastructure as a Service (Example: Amazon EC2)

EC2 gives you a virtual server. You control everything above the hardware — the operating system, the applications, the data.

| Layer | Responsible party |
|-------|------------------|
| Physical hardware and data center | AWS |
| Hypervisor (virtualization) | AWS |
| Guest operating system | **You** |
| Application installation and patching | **You** |
| Security software (antivirus, etc.) | **You** |
| Network firewall rules (security groups) | **You** |
| Data encryption | **You** |
| IAM and access control | **You** |

When your EC2 instance gets compromised because the OS wasn't patched — that's on you, not AWS.

### PaaS — Platform as a Service (Example: Amazon RDS)

RDS manages the database engine for you. You interact with the database; AWS handles the underlying OS and database software.

| Layer | Responsible party |
|-------|------------------|
| Physical hardware and data center | AWS |
| Hypervisor | AWS |
| Host operating system | AWS |
| Database engine patching | AWS |
| Automated backups | AWS |
| Database access controls | **You** |
| Data encryption settings | **You** |
| Database user accounts and permissions | **You** |
| What data you store | **You** |

You don't have SSH access to the underlying OS. You just connect to the database. AWS handles the rest.

### SaaS-like — Fully Managed Services (Example: Amazon S3, AWS Lambda)

For S3, AWS manages the storage infrastructure entirely. You put objects in, you get objects out. For Lambda, you provide function code — AWS provides and manages all the execution infrastructure.

| Layer | Responsible party |
|-------|------------------|
| Physical hardware and infrastructure | AWS |
| Storage/compute software and runtime | AWS |
| Service availability and durability | AWS |
| Bucket/function access permissions | **You** |
| Data encryption (encryption is optional but available) | **You** |
| What data you store and what code you run | **You** |

The fact that S3 offers 99.999999999% durability doesn't mean your bucket's permissions are correctly configured. Public S3 buckets that expose customer data are one of the most common AWS security incidents — and they're entirely the customer's fault, not AWS's.

---

## The responsibility matrix

| Security task | EC2 | RDS | S3 | Lambda |
|--------------|-----|-----|-----|--------|
| Physical data center security | AWS | AWS | AWS | AWS |
| Hypervisor / virtualization | AWS | AWS | AWS | AWS |
| Host OS patching | **You** | AWS | AWS | AWS |
| Database / runtime patching | **You** | AWS | N/A | AWS |
| Network firewall settings | **You** | **You** | **You** | AWS |
| Data encryption | **You** | **You** | **You** | **You** |
| IAM and access control | **You** | **You** | **You** | **You** |
| Application security | **You** | **You** | N/A | **You** |

The one constant: **data and access management are always your responsibility** regardless of service type.

---

## Common misconceptions

**"AWS handles all security."**
Wrong. AWS handles infrastructure security. You must configure your applications, encrypt your data, and set up proper access controls. A poorly configured AWS environment is insecure regardless of what AWS does.

**"Managed services mean I have no security responsibility."**
Wrong. RDS manages the database engine; you still control who can connect to the database and whether data is encrypted. "Managed" means AWS handles operations, not that security is automatic.

**"My application is compliant because AWS is compliant."**
Wrong. AWS holds many compliance certifications (HIPAA, SOC 2, PCI DSS, ISO 27001). But those certifications cover AWS's infrastructure. Your application running on that infrastructure still needs to be configured to meet compliance requirements — you must enable encryption, configure audit logging, implement access controls, and sign the appropriate agreements (like a HIPAA Business Associate Agreement with AWS).

---

## Decision rule for the exam

When a question asks who is responsible for a security task, ask:

1. **Can the customer configure or change this?** If yes → customer's responsibility.
2. **Is this about physical infrastructure or underlying AWS software?** If yes → AWS's responsibility.
3. **What service type?** More managed (RDS, Lambda, S3) → AWS handles more. Less managed (EC2) → customer handles more.

The rule of thumb: if you can SSH into it, patch it, or configure it — it's yours.

---

## Practice questions

**Q1.** A company running their website on Amazon EC2 gets hacked through an unpatched operating system vulnerability. Who is responsible?

A) AWS, because they manage EC2 infrastructure  
B) The customer, because OS patching on EC2 is the customer's responsibility  
C) Both equally, because EC2 is a shared service  
D) AWS, because they should automatically patch EC2 instances

**Answer: B** — EC2 is IaaS. You control and are responsible for the guest operating system, including applying security patches.

---

**Q2.** Which of the following is AWS always responsible for, regardless of which service a customer uses?

A) Encrypting customer data stored in S3  
B) Configuring security group rules for EC2 instances  
C) Patching the physical hardware and hypervisor  
D) Setting up multi-factor authentication for IAM users

**Answer: C** — Physical infrastructure and hypervisor security are always AWS's responsibility. The other options are always the customer's responsibility.

---

**Q3.** A customer uses Amazon RDS for their database. What security tasks are they still responsible for?

A) Patching the database engine and operating system  
B) Managing physical server hardware and data center security  
C) Configuring database access controls and enabling data encryption  
D) Maintaining the automatic backup infrastructure

**Answer: C** — With RDS, AWS handles OS and database engine patching, hardware, and backup infrastructure. The customer is still responsible for access controls and encryption settings.

---

**Q4.** A healthcare company wants to store patient data in AWS and claims it's automatically HIPAA-compliant because AWS holds a HIPAA compliance certification. Is this correct?

A) Yes — AWS's compliance certifications cover all customer workloads  
B) No — AWS provides HIPAA-eligible infrastructure, but the customer must configure their workload to meet HIPAA requirements  
C) Yes — as long as the data is stored in AWS, HIPAA requirements are automatically met  
D) No — AWS is not HIPAA-certified at all

**Answer: B** — AWS's compliance certifications cover the infrastructure layer. The customer must still configure encryption, access controls, and audit logging, and must sign a Business Associate Agreement with AWS.

---

**Next:** [Task 2.2 — Security, Governance, and Compliance Concepts](./Task%20Statement%202.2-Understand%20AWS%20Cloud%20security,%20governance,%20and%20compliance%20concepts.md)
