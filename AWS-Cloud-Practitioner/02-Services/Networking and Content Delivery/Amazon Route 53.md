# Amazon Route 53

## What you'll learn
- What Route 53 does: DNS, domain registration, health checks
- Routing policies
- Key exam facts

---

## What is Amazon Route 53?

Amazon Route 53 is a **highly available and scalable Domain Name System (DNS) web service**. It provides three main functions: DNS routing, domain registration, and health checking.

The name "Route 53" refers to TCP/UDP port 53, which DNS uses.

---

## What DNS does

DNS translates human-readable domain names (e.g., `www.example.com`) into IP addresses (e.g., `192.0.2.1`) that computers use to route traffic. Route 53 is AWS's DNS service — managing the DNS records for your domains.

---

## Three core functions

**DNS service** — Host your domain's DNS zone and manage records (A, CNAME, MX, TXT, Alias, etc.). Route 53 resolves domain names to AWS or non-AWS endpoints.

**Domain registration** — Register new domain names directly through Route 53 (e.g., purchase `mycompany.com`).

**Health checks** — Route 53 can monitor the health of your endpoints (EC2 instances, ELBs, etc.) and route traffic away from unhealthy ones. Health checks are the foundation for DNS failover.

---

## Routing policies

Route 53 supports multiple routing policies that control how DNS queries are resolved:

| Policy | How it works | Use case |
|--------|-------------|---------|
| Simple | Returns a single record value | One endpoint |
| Weighted | Distributes traffic by percentage | A/B testing, gradual deployments |
| Latency-based | Routes to the Region with lowest latency | Global app, serve from closest Region |
| Failover | Routes to primary; switches to secondary if health check fails | Active-passive disaster recovery |
| Geolocation | Routes based on user's geographic location | Serve different content to different countries |
| Geoproximity | Routes based on distance, with bias settings | Fine-grained geographic routing |
| Multivalue answer | Returns multiple healthy records | Simple load balancing with health checks |

---

## Route 53 and high availability

**DNS failover** — Configure a primary record pointing to your main endpoint and a secondary pointing to a backup (another Region, S3 static site, etc.). If Route 53 health checks detect the primary is unhealthy, DNS traffic automatically routes to the secondary.

This is a common pattern for multi-Region disaster recovery.

---

## Alias records

Alias records are Route 53-specific DNS records that map a domain name directly to an AWS resource (ELB, CloudFront distribution, S3 bucket, API Gateway) — including the zone apex (e.g., `example.com` — not just `www.example.com`). Alias records are free (no charge per query).

---

## Exam focus

- Route 53 = **DNS service** + domain registration + health checks
- Routing policies: **Simple, Weighted, Latency, Failover, Geolocation, Multivalue**
- **Failover routing** = active-passive DR with health checks
- **Latency-based routing** = route users to closest Region
- **Alias records** = map domain to AWS resources (ELB, CloudFront, S3)

---

## Practice questions

**Q1.** A global application is deployed in us-east-1 and eu-west-1. The company wants users to automatically be directed to the Region that provides the lowest network latency from their location. Which Route 53 routing policy achieves this?

A) Failover routing  
B) Weighted routing  
C) Latency-based routing  
D) Geolocation routing

**Answer: C** — Latency-based routing directs users to the AWS Region that has the lowest network latency from their location — not necessarily the geographically closest, but the fastest based on measured latency. Failover routing switches between primary and secondary on health check failure. Weighted routing distributes traffic by percentage. Geolocation routing routes based on country/continent — not latency.
