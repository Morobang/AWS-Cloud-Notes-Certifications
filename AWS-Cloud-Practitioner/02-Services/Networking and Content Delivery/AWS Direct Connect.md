# AWS Direct Connect

## What you'll learn
- What Direct Connect is and the problem it solves
- Direct Connect vs VPN
- Key use cases
- Key exam facts

---

## What is AWS Direct Connect?

AWS Direct Connect is a **dedicated private network connection between your on-premises data center and AWS**. Instead of routing traffic over the public internet, Direct Connect uses a physical dedicated fiber connection through an AWS Direct Connect location (colocation facility).

---

## The problem with internet-based connectivity

For most workloads, connecting to AWS over the internet works fine. But some scenarios need more:

- **Consistent bandwidth**: Internet is shared — bandwidth varies unpredictably
- **Consistent latency**: Internet routing can be unpredictable, causing latency spikes
- **Security**: Some organizations are prohibited from sending certain data over the public internet
- **Large data transfers**: Moving petabytes of data repeatedly over the internet is slow and expensive

Direct Connect addresses all of these by bypassing the public internet entirely.

---

## How Direct Connect works

1. You order a Direct Connect connection at an AWS Direct Connect location (a data center with physical connectivity to AWS)
2. Your network team connects your on-premises data center to the Direct Connect location (either directly or through a network provider)
3. Traffic between your data center and AWS travels over this private fiber — never touching the public internet

Direct Connect supports two connection speeds:
- **Hosted connections**: 50 Mbps – 10 Gbps (through Direct Connect partners)
- **Dedicated connections**: 1 Gbps or 10 Gbps (directly with AWS)

---

## Direct Connect vs Site-to-Site VPN

| | AWS Direct Connect | AWS Site-to-Site VPN |
|-|-------------------|--------------------|
| Connection type | Dedicated private fiber | Encrypted tunnel over the internet |
| Bandwidth | Consistent, guaranteed | Variable (shared internet) |
| Latency | Consistent, low | Variable |
| Setup time | Weeks to months (physical provisioning) | Minutes to hours |
| Cost | Higher (dedicated circuit) | Lower |
| Best for | Large data transfers, consistent performance | Quick hybrid connectivity, backup |

Many organizations use Direct Connect as the primary connection and Site-to-Site VPN as a backup.

---

## When to use it

**Hybrid cloud with large data volumes** — Regularly transferring large datasets between on-premises and AWS where internet bandwidth and costs are prohibitive.

**Compliance requirements** — Industries (finance, healthcare) that prohibit sensitive data over the public internet.

**Consistent performance** — Real-time applications that need predictable latency between on-premises and AWS.

---

## Exam focus

- Direct Connect = **dedicated private network connection** to AWS — not over the internet
- Consistent bandwidth and latency — bypasses the public internet
- Takes weeks to provision (physical connection required)
- More expensive than VPN; higher performance and consistency
- Use when exam describes: "private dedicated connection," "consistent bandwidth," "compliance requires no public internet," "large data transfer to AWS regularly"

---

## Practice questions

**Q1.** A financial services company needs a consistent, high-bandwidth private connection between their on-premises data center and AWS. They have compliance requirements that prohibit sending data over the public internet. Which AWS service meets this requirement?

A) AWS Site-to-Site VPN  
B) AWS Direct Connect  
C) AWS PrivateLink  
D) Amazon CloudFront

**Answer: B** — Direct Connect provides a dedicated private fiber connection to AWS that bypasses the public internet entirely — meeting both the consistency and compliance requirements. Site-to-Site VPN is encrypted but still travels over the public internet. PrivateLink provides private connectivity between VPCs and AWS services (within AWS), not from on-premises. CloudFront is a CDN.
