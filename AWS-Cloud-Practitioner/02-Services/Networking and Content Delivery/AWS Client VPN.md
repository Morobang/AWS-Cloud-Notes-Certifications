# AWS Client VPN

## What you'll learn
- What Client VPN does and who uses it
- Client VPN vs Site-to-Site VPN
- Key exam facts

---

## What is AWS Client VPN?

AWS Client VPN is a **managed VPN service that lets individual users (remote employees) securely connect to AWS resources or on-premises networks** from any location using a VPN client application on their device.

---

## Client VPN vs Site-to-Site VPN

Both are VPN solutions, but for different scenarios:

| | Client VPN | Site-to-Site VPN |
|-|-----------|-----------------|
| What connects | Individual users (laptops, phones) | Entire networks (offices, data centers) |
| Use case | Remote employees accessing VPC resources | Connecting offices/data centers to AWS |
| Setup | VPN client software on each device | VPN hardware/router at network level |
| Scale | Per-user connections | Network-to-network tunnel |

---

## How Client VPN works

1. Create a Client VPN endpoint attached to a VPC
2. Configure authentication (Active Directory, certificate-based, SAML/SSO)
3. Users download the OpenVPN client and a configuration file
4. Users connect to the Client VPN endpoint from anywhere — it appears as if they're on the corporate network
5. Traffic is encrypted end-to-end

Users can access:
- EC2 instances and other resources in the VPC
- On-premises resources (via VPC to corporate network connectivity)
- Other AWS services with private endpoints

---

## Use cases

**Remote workforce** — Employees working from home need access to internal AWS-hosted applications.

**Contractor access** — Give third parties access to specific VPC resources without exposing them to the public internet.

**Developer access** — Developers need to connect to databases and internal services running in private subnets.

---

## Exam focus

- Client VPN = **individual user access** to VPC resources via VPN client software
- Users connect from any device, any location
- Managed service — no VPN server infrastructure to maintain
- Key differentiator from Site-to-Site VPN: Client VPN = individual users; Site-to-Site = networks

---

## Practice questions

**Q1.** A company needs remote employees to securely access EC2 instances and RDS databases in a private VPC subnet from their home laptops. Which AWS service enables this?

A) AWS Site-to-Site VPN  
B) AWS Direct Connect  
C) AWS Client VPN  
D) AWS PrivateLink

**Answer: C** — Client VPN allows individual remote users to connect to VPC resources from any location using a VPN client on their devices. Site-to-Site VPN connects entire networks (office-to-VPC), not individual users. Direct Connect is a dedicated circuit for network-to-network connectivity. PrivateLink provides private connectivity between services within AWS.
