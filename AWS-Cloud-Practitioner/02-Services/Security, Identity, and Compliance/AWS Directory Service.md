# AWS Directory Service

## What you'll learn
- What Directory Service provides
- The two main offerings: Managed Microsoft AD and AD Connector
- Key use cases
- Key exam facts

---

## What is AWS Directory Service?

AWS Directory Service provides **managed directory services in AWS**, including a fully managed Microsoft Active Directory and a proxy connector to existing on-premises Active Directory. It allows you to use Active Directory in the AWS cloud without managing directory servers yourself.

---

## Why Active Directory matters

Active Directory (AD) is the dominant enterprise identity system — storing user accounts, groups, computers, and security policies. Many Windows applications and services require AD for authentication. AWS services like WorkSpaces, RDS for SQL Server, and EC2 Windows instances can integrate with AD.

---

## Three offerings

**AWS Managed Microsoft AD** — A fully managed, actual Microsoft Active Directory running on AWS. Two editions:
- **Standard Edition**: Up to 30,000 directory objects; small/medium organizations
- **Enterprise Edition**: Up to 500,000 objects; large organizations

Use this when: You want native Microsoft AD in AWS — for WorkSpaces, EC2 Windows domain joins, SSO with AD-aware applications.

**AD Connector** — A proxy that redirects AD authentication requests to your existing on-premises Active Directory. It doesn't store any directory information in AWS itself.

Use this when: You have an on-premises AD and want AWS services to authenticate against it without replicating or migrating the directory to AWS.

**Simple AD** — A managed directory powered by Samba, compatible with Microsoft AD APIs. Simpler and less expensive but doesn't support all Microsoft AD features (no trust relationships, no Group Policy Objects).

Use this when: You need basic AD-compatible directory for Linux EC2 instances and simple use cases.

---

## AWS Managed Microsoft AD vs AD Connector

| | Managed Microsoft AD | AD Connector |
|-|---------------------|-------------|
| Directory location | Runs in AWS | Stays on-premises |
| Standalone? | Yes — no on-premises dependency | No — requires on-premises AD |
| Best for | New AD in AWS or AD migration | Extend on-premises AD to AWS |
| Features | Full Microsoft AD features | Proxy — inherits on-premises AD features |

---

## Integration with AWS services

- **WorkSpaces**: Users sign in with AD credentials
- **RDS SQL Server**: Join the domain for Windows Authentication
- **EC2 Windows**: Domain join instances
- **IAM Identity Center**: Use AD as the identity source for SSO

---

## Exam focus

- Directory Service = **managed Active Directory in AWS**
- **Managed Microsoft AD** = real AD in AWS, standalone
- **AD Connector** = proxy to existing on-premises AD (no migration needed)
- **Simple AD** = basic Samba-based directory, lightweight
- Use when exam describes: "Active Directory in AWS," "domain join EC2 instances," "use existing on-premises AD with AWS services"

---

## Practice questions

**Q1.** A company already has an on-premises Microsoft Active Directory and wants their Amazon WorkSpaces users to log in with their existing AD credentials. They do not want to replicate or migrate their directory to AWS. Which AWS Directory Service option should they use?

A) AWS Managed Microsoft AD  
B) AD Connector  
C) Simple AD  
D) AWS IAM Identity Center

**Answer: B** — AD Connector proxies authentication to the existing on-premises AD without storing any directory data in AWS. Users can sign in to WorkSpaces using their on-premises credentials. Managed Microsoft AD creates a new AD in AWS — overkill if the company just wants to use their existing AD. Simple AD is a lightweight Samba-based directory — not a proxy to an existing AD. IAM Identity Center provides SSO but would also need a directory (could use AD Connector as the source).
