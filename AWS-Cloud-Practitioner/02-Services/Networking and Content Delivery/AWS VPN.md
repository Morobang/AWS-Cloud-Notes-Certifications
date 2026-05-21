# AWS VPN

## What you'll learn
- What AWS VPN covers as an umbrella term
- The two products under AWS VPN
- Key exam facts

---

## What is AWS VPN?

**AWS VPN** is the umbrella term for two separate VPN products:

1. **AWS Site-to-Site VPN** — Connects an entire on-premises network (office, data center) to an AWS VPC via an encrypted IPSec tunnel over the internet.

2. **AWS Client VPN** — Connects individual remote users (employees on laptops) to AWS VPC resources via a VPN client application.

---

## Quick comparison

| | AWS Site-to-Site VPN | AWS Client VPN |
|-|---------------------|----------------|
| Connects | On-premises network ↔ VPC | Individual user devices ↔ VPC |
| Setup | Customer Gateway + Virtual Private Gateway | VPN endpoint + client software |
| Use case | Hybrid office-to-cloud connectivity | Remote employee access |
| Authentication | Pre-shared keys or certificates | Active Directory, certificates, SAML/SSO |

---

## When the exam says "AWS VPN"

If an exam question mentions "AWS VPN" without specifying Site-to-Site or Client, use context to determine which:

- "Connect on-premises network to VPC" → Site-to-Site VPN
- "Remote employees access VPC resources" → Client VPN

---

## Exam focus

- AWS VPN = umbrella term for **Site-to-Site VPN** and **Client VPN**
- Both travel over the internet (encrypted); Direct Connect uses a dedicated private link
- VPN = quick, internet-based, encrypted; Direct Connect = dedicated, private, consistent

See individual service files for detailed coverage:
- [AWS Site-to-Site VPN](AWS%20Site-to-Site%20VPN.md)
- [AWS Client VPN](AWS%20Client%20VPN.md)
