# Networking and Content Delivery — Category Overview

Networking services provide the connectivity fabric for AWS — from isolating resources in private networks to delivering content globally and connecting on-premises offices to AWS.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| Amazon VPC | Private virtual network in AWS | Isolation, subnets, routing, security groups |
| Amazon CloudFront | Global CDN | Cache and deliver content from edge locations |
| Amazon Route 53 | DNS service | Domain registration, DNS routing, health checks |
| Amazon API Gateway | Managed API service | Create REST, HTTP, and WebSocket APIs |
| AWS Direct Connect | Dedicated network link from on-premises to AWS | Private, consistent bandwidth connection |
| AWS Site-to-Site VPN | Encrypted tunnel from on-premises to VPC | Internet-based VPN for hybrid connectivity |
| AWS Client VPN | VPN for individual users to access AWS | Remote worker access to VPC resources |
| AWS VPN | Umbrella for Site-to-Site and Client VPN | — |
| AWS Transit Gateway | Hub connecting multiple VPCs and on-premises | Centralized network hub |
| AWS PrivateLink | Private connectivity between VPCs and services | No public internet traversal |
| AWS Global Accelerator | Route users to optimal AWS endpoints globally | Anycast IPs, AWS backbone routing |

---

## Exam selection guide

| Scenario | Service |
|---------|---------|
| "Cache content globally, lower latency for static files" | CloudFront |
| "DNS, domain registration, health checks" | Route 53 |
| "Create and manage REST APIs" | API Gateway |
| "Connect on-premises office to VPC privately, dedicated bandwidth" | Direct Connect |
| "Connect on-premises to VPC over encrypted internet tunnel" | Site-to-Site VPN |
| "Remote employees need access to VPC resources" | Client VPN |
| "Connect many VPCs together centrally" | Transit Gateway |
| "Access AWS services privately without internet" | PrivateLink |
| "Improve global application performance with static IPs" | Global Accelerator |
