# AWS Site-to-Site VPN

## What you'll learn
- What Site-to-Site VPN does
- How it connects on-premises to AWS
- VPN vs Direct Connect
- Key exam facts

---

## What is AWS Site-to-Site VPN?

AWS Site-to-Site VPN creates an **encrypted tunnel between your on-premises network and your Amazon VPC** over the public internet. It connects your corporate network to AWS as if they were the same private network — at a fraction of the cost and setup time of Direct Connect.

---

## How it works

**Your side**: A **Customer Gateway** — your on-premises VPN device (router or firewall) with a public IP address.

**AWS side**: A **Virtual Private Gateway** (VGW) — attached to your VPC, it terminates the VPN connection on the AWS side.

The two gateways establish an IPSec encrypted tunnel. Traffic between your on-premises network and your VPC travels through this tunnel, encrypted end-to-end.

**Two tunnels**: AWS always creates two VPN tunnels for redundancy — if one tunnel fails, traffic failovers to the second.

---

## Site-to-Site VPN vs Direct Connect

| | Site-to-Site VPN | Direct Connect |
|-|-----------------|----------------|
| Connection type | Encrypted internet tunnel | Dedicated private fiber |
| Setup time | Minutes to hours | Weeks to months |
| Cost | Lower | Higher |
| Bandwidth | Variable (shared internet) | Consistent, guaranteed |
| Latency | Variable | Consistent, low |
| Best for | Quick setup, lower cost, backup | Large data, consistent performance |

**Common pattern**: Use Direct Connect as the primary connection; Site-to-Site VPN as the backup/failover.

---

## AWS VPN

"AWS VPN" is an umbrella term that covers:
- **Site-to-Site VPN**: Network-to-network (data center to VPC)
- **Client VPN**: User-to-network (remote employee to VPC)

---

## When to use it

**Hybrid connectivity, quick setup** — Connect an on-premises office to AWS in hours when you don't need the bandwidth or cost of Direct Connect.

**Backup connection** — Use as a failover for Direct Connect.

**Development/test environments** — Temporary or lower-traffic hybrid connectivity without long-term Direct Connect commitments.

---

## Exam focus

- Site-to-Site VPN = **encrypted tunnel over the internet** from on-premises network to AWS VPC
- Uses a **Customer Gateway** (your side) and **Virtual Private Gateway** (AWS side)
- **Two tunnels** per VPN connection for redundancy
- Faster and cheaper to set up than Direct Connect, but uses the internet (variable performance)
- Use when exam describes: "connect office to AWS," "hybrid connectivity," "encrypted connection to VPC"

---

## Practice questions

**Q1.** A company wants to connect their on-premises office to their Amazon VPC so that on-premises servers can access EC2 instances using private IP addresses. They need the connection set up within 24 hours at low cost. Which AWS service provides this connectivity?

A) AWS Direct Connect  
B) AWS Site-to-Site VPN  
C) Amazon VPC Peering  
D) AWS PrivateLink

**Answer: B** — Site-to-Site VPN creates an encrypted tunnel between the on-premises network and the VPC, allowing private IP communication in hours. Direct Connect provides a dedicated private connection but takes weeks to provision. VPC Peering connects two VPCs — not on-premises networks. PrivateLink provides private connectivity between VPCs and services within AWS.
