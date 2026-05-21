# AWS Global Accelerator

## What you'll learn
- What Global Accelerator does
- Static IP addresses and anycast routing
- Global Accelerator vs CloudFront
- Key exam facts

---

## What is AWS Global Accelerator?

AWS Global Accelerator is a networking service that **improves the availability and performance of global applications by routing user traffic through the AWS global network** rather than over the public internet.

It provides two static IP addresses that serve as fixed entry points to your application — regardless of which AWS Region the application is deployed in.

---

## How it works

1. Global Accelerator assigns you **two static anycast IP addresses**
2. Users' DNS resolves your domain to these IPs
3. User traffic enters the AWS network at the nearest AWS edge location (rather than traveling over unpredictable internet routes)
4. Traffic travels to your application over the AWS backbone network — which has fewer hops and more consistent performance than the public internet
5. Global Accelerator health-checks your endpoints and routes traffic to the healthiest, best-performing endpoint

---

## Anycast IPs

The same two IP addresses work for all Regions. Anycast routing automatically directs each user to the nearest edge location. You don't manage DNS failover — Global Accelerator handles it.

---

## Global Accelerator vs CloudFront

| | AWS Global Accelerator | Amazon CloudFront |
|-|-----------------------|-------------------|
| What it accelerates | Any TCP/UDP application | HTTP/HTTPS content |
| Caching | No | Yes — caches at edge |
| Static IPs | Yes (two anycast IPs) | No |
| Protocols | TCP, UDP (HTTP, HTTPS, gaming, VoIP) | HTTP/HTTPS only |
| Best for | Non-HTTP apps, gaming, IoT, static IPs | Web content delivery, CDN |

**CloudFront** accelerates content by caching at the edge. **Global Accelerator** accelerates any application traffic by routing over the AWS backbone — no caching.

---

## When to use it

**Non-HTTP applications** — Gaming servers, VoIP, IoT telemetry, and other UDP applications that CloudFront can't serve.

**Static IP requirement** — Applications where clients need fixed, unchanging IP addresses (e.g., whitelisted by firewalls).

**Multi-Region failover** — Route traffic to the nearest healthy endpoint; automatically failover to another Region if the primary is unhealthy.

**Consistent global performance** — Applications where users worldwide need low, predictable latency.

---

## Exam focus

- Global Accelerator = **route traffic over AWS backbone** for consistent global performance
- Provides **two static anycast IP addresses** — same IPs globally
- Works with any TCP/UDP traffic — not just HTTP
- Key differentiator from CloudFront: Global Accelerator = routing + performance (no cache); CloudFront = CDN + caching
- Use when exam describes: "static IPs for global app," "non-HTTP performance," "route over AWS network backbone"

---

## Practice questions

**Q1.** A gaming company has servers in multiple AWS Regions. They need a solution that gives users globally consistent low-latency connections to the nearest healthy server. The solution must also provide fixed IP addresses for firewall whitelisting. Which AWS service meets these requirements?

A) Amazon CloudFront  
B) Amazon Route 53  
C) AWS Global Accelerator  
D) Amazon API Gateway

**Answer: C** — Global Accelerator provides static anycast IPs, routes traffic over the AWS backbone to the nearest healthy endpoint, and supports non-HTTP protocols (TCP/UDP) required for gaming. CloudFront is a CDN for HTTP content — it doesn't provide static IPs or support game protocols. Route 53 provides DNS-based routing but not fixed IPs or backbone routing. API Gateway is for managed HTTP APIs.
