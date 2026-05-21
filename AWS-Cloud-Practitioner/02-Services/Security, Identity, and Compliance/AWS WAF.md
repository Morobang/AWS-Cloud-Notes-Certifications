# AWS WAF (Web Application Firewall)

## What you'll learn
- What a WAF is and what attacks it blocks
- Web ACLs and rules
- WAF deployment points
- Key exam facts

---

## What is AWS WAF?

AWS WAF is a **web application firewall** that protects web applications and APIs from common web exploits that could affect availability, compromise security, or consume excessive resources.

WAF inspects HTTP/HTTPS requests and allows or blocks them based on rules you define.

---

## What WAF protects against

Common threats WAF rules address:

| Threat | Description |
|--------|-------------|
| SQL injection | Malicious SQL code in form inputs to manipulate databases |
| Cross-site scripting (XSS) | Injecting scripts into pages viewed by other users |
| IP-based blocking | Block traffic from known malicious IP addresses or ranges |
| Rate limiting | Block IPs that send too many requests in a short period |
| Geographic blocking | Block requests from specific countries |
| Bad bots | Block known malicious bots and scrapers |

---

## Key concepts

**Web ACL** (Web Access Control List) — The main WAF resource. A Web ACL contains a collection of rules that are evaluated in order for each incoming request. WAF either allows or blocks the request based on the rules.

**Rules** — Define what to inspect and what action to take:
- **AWS Managed Rules**: Pre-built rule groups maintained by AWS covering common threats (OWASP Top 10, known bad IPs, etc.)
- **Custom rules**: Write your own rules based on IP addresses, request headers, body content, etc.

**Rate-based rules** — Block an IP if it makes more than N requests in 5 minutes.

---

## Where WAF is deployed

WAF attaches to:
- **Amazon CloudFront** — Inspect requests at the edge before they reach your origin
- **Application Load Balancer (ALB)** — Inspect requests before they reach your EC2 instances
- **Amazon API Gateway** — Protect your REST APIs
- **AWS AppSync** — Protect GraphQL APIs

---

## WAF vs Shield

| | AWS WAF | AWS Shield |
|-|---------|-----------|
| Protects against | Application layer attacks (SQL injection, XSS, bad IPs) | DDoS attacks (volumetric, state-exhaustion) |
| Layer | Layer 7 (application) | Layer 3/4 (network) + Layer 7 (with Shield Advanced) |
| Rules | You define rules | Automatic + DRT support (Advanced) |
| Best for | Web app security | DDoS mitigation |

WAF and Shield are complementary — use both for comprehensive web application protection.

---

## Exam focus

- WAF = **web application firewall** — blocks SQL injection, XSS, bad IPs, rate limits
- Attaches to **CloudFront, ALB, API Gateway**
- **Web ACL** = container for rules; rules evaluated in order per request
- **AWS Managed Rules** = pre-built rule sets for common threats (no custom rule writing needed)
- Use when exam describes: "protect against SQL injection," "block malicious IPs," "rate limit web requests"

---

## Practice questions

**Q1.** A company's web application hosted behind an Application Load Balancer has been experiencing SQL injection attacks. They want to automatically detect and block these requests. Which AWS service provides this protection?

A) AWS Shield Standard  
B) Amazon GuardDuty  
C) AWS WAF  
D) Amazon Inspector

**Answer: C** — WAF provides application-layer (Layer 7) protection including SQL injection detection and blocking. Deploy WAF on the ALB and enable the AWS Managed Rules for SQL injection. Shield protects against DDoS volumetric attacks, not application-layer SQL injection. GuardDuty detects threats through behavioral analysis of logs and API calls — not inline request blocking. Inspector scans for vulnerabilities but doesn't inspect web traffic.
